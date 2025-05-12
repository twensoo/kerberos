### kerberos


---

# 📘 Глава 4. Разработка программного обеспечения

## 4.1. Требования к разрабатываемому ПО

### 4.1.1. Общие требования

Для достижения цели — **разработки программного обеспечения, выявляющего атаки на протокол Kerberos через анализ аномалий в сетевом трафике Active Directory**, — были сформулированы следующие общие требования:

- Программа должна быть **автономной**, не требующей установки дополнительных зависимостей.
- Должна поддерживать **множество источников данных**: сетевой трафик (PCAP), логи Windows Event Logs и потоковое наблюдение за сетью.
- Программа должна **обнаруживать известные атаки** на Kerberos: Golden Ticket, Silver Ticket, Kerberoasting, AS-REP Roasting, Pass-the-Ticket.
- Результаты анализа должны выводиться в удобочитаемом формате: CLI, JSON или CSV.
- Программа должна иметь возможность интеграции с системами мониторинга безопасности (SIEM) через Syslog, Slack или Email.

### 4.1.2. Функциональные требования

| № | Функция | Описание |
|----|--------|----------|
| 1 | Сбор данных из PCAP-файлов | Чтение пакетов и извлечение сообщений Kerberos |
| 2 | Захват трафика в реальном времени | Возможность live capture с помощью tcpdump/tshark |
| 3 | Парсинг Kerberos-сообщений | Извлечение параметров билетов (TGT/TGS), SPN, пользователей |
| 4 | Чтение логов Windows | Поддержка чтения EVT/ETW-файлов с событиями Kerberos (ID 4768, 4769, 4771) |
| 5 | Обнаружение атак | Анализ отклонений от нормального поведения и сигнатур |
| 6 | Вывод результатов | Цветовая индикация уровня риска, сохранение в файл |
| 7 | Интеграция с внешними системами | Отправка алертов через Syslog, Email или API |

### 4.1.3. Нефункциональные требования

| Категория | Требование |
|-----------|------------|
| Совместимость | Работа на Windows и Linux |
| Производительность | Обработка трафика в реальном времени или близком к нему |
| Безопасность | Никакая конфиденциальная информация не должна записываться без согласия пользователя |
| Удобство использования | CLI-интерфейс с понятным выводом |
| Расширяемость | Поддержка добавления новых правил анализа и типов атак |
| Документированность | Комментарии в коде, README, примеры использования |

---

## 4.2. Архитектура системы

Разработанное программное обеспечение состоит из нескольких взаимодействующих модулей, каждый из которых выполняет свою функцию в процессе сбора, анализа и обнаружения аномалий в Kerberos-трафике.

### 4.2.1. Модуль сбора данных

Этот модуль отвечает за получение исходных данных для анализа.

#### Возможности:
- Чтение PCAP-файлов.
- Захват трафика в реальном времени.
- Чтение Windows Event Logs (EVTX).
- Группировка событий по IP, имени пользователя и времени.

#### Техническая реализация:
- Использование `scapy` и `pyshark` для парсинга пакетов.
- Использование `Evtx` для чтения журналов Windows.
- Реализация фильтрации на основе порта 88 (Kerberos).

### 4.2.2. Модуль парсинга и фильтрации

Этот модуль отвечает за обработку сырых данных и выделение только тех событий, которые связаны с протоколом Kerberos.

#### Возможности:
- Извлечение ASN.1 структур из пакетов.
- Извлечение имени пользователя, SPN, типа запроса, TTL билета.
- Фильтрация нерелевантного трафика.

#### Пример парсинга Kerberos-запроса:
```python
from scapy.all import *
from scapy.layers.kerberos import Kerberos

def extract_kerberos_data(pkt):
    if pkt.haslayer(Kerberos):
        kerb_layer = pkt[Kerberos]
        return {
            'type': kerb_layer.type,
            'user': kerb_layer.cname,
            'spn': kerb_layer.sname,
            'till': kerb_layer.till,
            'timestamp': pkt.time
        }
```

### 4.2.3. Модуль анализа и обнаружения аномалий

Этот модуль реализует алгоритмы обнаружения атак.

#### Методы анализа:
- **Сигнатурный анализ:** сравнение с известными шаблонами атак.
- **Поведенческий анализ:** сравнение текущих значений с историческими данными.
- **Контекстный анализ:** проверка соответствия между этапами Kerberos-сессии.

#### Пример правила обнаружения Golden Ticket:
```python
def detect_golden_ticket(ticket):
    if ticket['lifetime'] > 86400 and ticket['user'] == 'krbtgt':
        return {
            'alert': 'Golden Ticket detected',
            'severity': 'high',
            'description': 'Ticket with long lifetime issued for krbtgt account'
        }
```

#### Пример правила обнаружения Kerberoasting:
```python
def detect_kerberoasting(tgs_requests):
    if len(tgs_requests) > 20 and tgs_requests[0]['time_diff'] < 5:
        return {
            'alert': 'Potential Kerberoasting detected',
            'severity': 'medium',
            'description': 'Multiple TGS requests to service accounts in a short time'
        }
```

### 4.2.4. Модуль вывода результатов

Этот модуль отвечает за отображение и сохранение результатов анализа.

#### Возможности:
- Цветовой вывод в CLI.
- Сохранение результатов в JSON/CSV.
- Отправка алертов через Syslog, Slack, Email.

#### Пример вывода в консоль:
```python
from termcolor import colored

def display_alert(alert):
    color_map = {
        'low': 'green',
        'medium': 'yellow',
        'high': 'red'
    }
    print(colored(f"[!] {alert['description']}", color_map.get(alert['severity'], 'white')))
```

#### Пример формата JSON:
```json
{
  "timestamp": "2025-04-05T10:34:22",
  "alert": "Golden Ticket detected",
  "severity": "high",
  "description": "Ticket with long lifetime issued for krbtgt account",
  "source_ip": "192.168.1.100"
}
```

---

## 4.3. Реализация модулей

### 4.3.1. Парсинг сетевого трафика

Для парсинга сетевого трафика использовались следующие библиотеки:

- **Scapy**: позволяет читать и анализировать сетевые пакеты, в том числе с Kerberos-заголовками.
- **PyShark**: предоставляет удобный API для работы с Wireshark-пакетами, особенно при работе с ASN.1-структурами.

#### Пример кода:
```python
from scapy.all import *
from scapy.layers.kerberos import Kerberos

def parse_kerberos_packets(pcap_file):
    packets = rdpcap(pcap_file)
    for pkt in packets:
        if Kerberos in pkt:
            kerb_pkt = pkt[Kerberos]
            print(f"[+] Kerberos packet found: {kerb_pkt.show()}")
```

### 4.3.2. Анализ журналов Windows

Для чтения событий безопасности Windows использовался Python-модуль `Evtx`, позволяющий работать с ETL/EVTX-файлами.

#### Пример кода:
```python
import xml.etree.ElementTree as ET
import Evtx.Evtx as evtx

def read_event_logs(log_file):
    with evtx.Evtx(log_file) as log:
        for record in log.records():
            xml = ET.fromstring(record.xml())
            event_id = xml.find('.//EventID').text
            if event_id in ['4768', '4769', '4771']:
                yield parse_kerberos_event(xml)

def parse_kerberos_event(xml_data):
    # Извлечение имени пользователя, SPN, времени и других полей
    return {
        'event_id': ...,
        'user': ...,
        'spn': ...,
        'timestamp': ...
    }
```

### 4.3.3. Алгоритмы обнаружения аномалий

Были реализованы следующие правила анализа:

#### Golden Ticket:
```python
def detect_golden_ticket(ticket):
    if ticket['lifetime'] > 86400 and ticket['user'] == 'krbtgt':
        return "Golden Ticket detected"
```

#### Kerberoasting:
```python
def detect_kerberoasting(tgs_requests):
    if len(tgs_requests) > 20 and tgs_requests[0]['time_diff'] < 5:
        return "Potential Kerberoasting detected"
```

#### Silver Ticket:
```python
def detect_silver_ticket(session):
    if 'TGT' not in session and 'TGS' in session:
        return "Possible Silver Ticket detected"
```

### 4.3.4. Интерфейс вывода результатов

Результаты выводились в консоль с цветовой индикацией:

```python
from termcolor import colored

def display_alert(alert):
    color_map = {
        'low': 'green',
        'medium': 'yellow',
        'high': 'red'
    }
    print(colored(f"[!] {alert['description']}", color_map.get(alert['severity'], 'white')))
```

Пример вывода:
```
[!] Potential Kerberoasting detected at 10:34:22 from 192.168.1.100
```

---

## 4.4. Используемые технологии

### 4.4.1. Язык программирования

- **Python 3.10+** — выбран как основной язык реализации благодаря широкой поддержке сетевых и аналитических библиотек.

### 4.4.2. Библиотеки и фреймворки

| Название | Назначение |
|---------|------------|
| Scapy | Парсинг сетевого трафика |
| PyShark | Декодирование Kerberos-сообщений |
| Pandas | Обработка и анализ логов |
| NumPy | Математические операции |
| Evtx | Чтение Windows Event Logs |
| Termcolor | Цветовое оформление вывода |
| Argparse | Обработка командной строки |
| Logging | Логгирование работы программы |

### 4.4.3. Инструменты анализа

| Инструмент | Назначение |
|-----------|------------|
| Wireshark | Анализ и экспорт PCAP-файлов |
| Tcpdump | Захват трафика в реальном времени |
| Event Viewer | Чтение и экспорт Windows Security Logs |
| Sigma | Создание сигнатур для сравнения |
| Jupyter Notebook | Предварительный анализ данных и отладка алгоритмов |

---


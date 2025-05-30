### kerberos


---

---

# 📘 Глава 1. Введение

## 1.1. Актуальность темы

Протокол Kerberos является основным механизмом аутентификации в доменных сетях Microsoft Active Directory и используется для обеспечения безопасного доступа к ресурсам корпоративной инфраструктуры. Несмотря на высокую степень криптографической защищённости, протокол подвержен различным типам атак, позволяющим злоумышленникам обходить систему безопасности без использования явно вредоносных действий.

Такие атаки, как **Golden Ticket**, **Silver Ticket**, **Kerberoasting**, **AS-REP Roasting** и **Pass-the-Ticket**, всё чаще используются при проведении пентестов и сложных APT-атак. Они не оставляют следов в традиционных средствах защиты, таких как антивирусы или фаерволы, поскольку маскируются под нормальную активность сети и используют легитимные протоколы доменной среды.

Это делает особенно важным разработку программных решений, способных анализировать сетевой трафик Active Directory и выявлять признаки атак на протокол Kerberos на основе аномального поведения. Такие системы позволят повысить уровень безопасности информационных систем и снизить риски компрометации доменной среды.

Существующие подходы к обнаружению атак на Kerberos включают сигнатурный анализ (основанный на известных шаблонах), использование правил SIEM-систем и ручное расследование инцидентов. Однако они требуют значительных ресурсов и часто неэффективны против новых или малоизвестных методов атак. Таким образом, актуальной задачей становится разработка программного обеспечения, реализующего поведенческий подход к выявлению угроз, основанному на анализе отклонений от нормального поведения Kerberos-сессий.

---

## 1.2. Цель работы

Целью данной выпускной квалификационной работы является **разработка программного обеспечения, выявляющего атаки на протокол Kerberos через анализ аномалий в сетевом трафике Active Directory**.

Достижение этой цели предполагает создание системы, способной:
- Собирать данные из сетевого трафика (PCAP) и журналов событий Windows.
- Парсить Kerberos-сообщения и извлекать ключевые параметры.
- Обнаруживать отклонения от нормального поведения пользователей и сервисов.
- Формировать уведомления о потенциальных атаках и выводить их в удобочитаемом виде.

---

## 1.3. Задачи исследования

Для достижения указанной цели были поставлены следующие задачи:

1. **Провести анализ особенностей функционирования протокола Kerberos в среде Active Directory.**
   - Изучить этапы аутентификации: AS-REQ / AS-REP, TGS-REQ / TGS-REP, AP-REQ.
   - Исследовать структуру Kerberos-билетов и формат сообщений.
   - Рассмотреть роль KDC, SPN и учетной записи krbtgt.

2. **Исследовать известные методики атак на протокол Kerberos.**
   - Изучить такие атаки, как Golden Ticket, Silver Ticket, Kerberoasting, AS-REP Roasting, Pass-the-Ticket.
   - Выявить характерные сетевые и логовые индикаторы компрометации.
   - Определить способы обнаружения каждой из них.

3. **Выявить характерные сетевые и логовые индикаторы компрометации.**
   - Проанализировать частоту запросов TGT/TGS.
   - Исследовать TTL билетов, SPN, ошибки аутентификации.
   - Создать модели нормального поведения Kerberos-сессий.

4. **Разработать модель поведенческого анализа.**
   - Построить профили поведения пользователей и сервисов.
   - Реализовать сравнение текущих событий с историческими данными.
   - Разработать правила обнаружения аномалий.

5. **Спроектировать и реализовать программный модуль.**
   - Реализовать сбор данных из PCAP-файлов и логов Windows.
   - Разработать парсер Kerberos-сообщений.
   - Создать алгоритмы обнаружения атак.
   - Организовать вывод результатов в CLI и JSON-формате.

6. **Протестировать разработанное программное обеспечение.**
   - Провести тестирование на эталонных сценариях атак.
   - Оценить эффективность обнаружения по метрикам точности и скорости.
   - Сравнить с существующими решениями.

---

## 1.4. Объект и предмет исследования

### Объект исследования:
Система аутентификации на основе протокола Kerberos в среде Microsoft Active Directory.

### Предмет исследования:
Процесс выявления атак на протокол Kerberos с помощью поведенческого анализа сетевого трафика и журналирования событий безопасности.

---

## 1.5. Методология и подходы

Работа выполнена с применением следующих методологий и подходов:

- **Анализ научной и технической литературы** по протоколу Kerberos и современным угрозам.
- **Экспериментальный подход** — проведение тестовых атак в контролируемой лабораторной среде.
- **Сигнатурный и поведенческий анализ** для обнаружения аномалий.
- **Разработка программного обеспечения** с использованием языка Python и библиотек для анализа сетевого трафика и логов Windows.


## 1.6. Структура работы

Работа состоит из **шести глав**, списка использованных источников и **приложений**.

| Глава | Тема |
|-------|------|
| **Глава 1** | Введение — содержит цель, задачи, объект и предмет исследования, а также обосновывается её актуальность. |
| **Глава 2** | Теоретические основы — описание протокола Kerberos, его роли в Active Directory и классификация типовых атак. |
| **Глава 3** | Анализ атак и выявление аномалий — моделирование угроз, выявление аномалий и методы их обнаружения. |
| **Глава 4** | Разработка программного обеспечения — описание архитектуры и реализации программы. |
| **Глава 5** | Тестирование и оценка эффективности — проведение тестов и анализ полученных результатов. |
| **Глава 6** | Заключение — выводы по результатам исследования и перспективы дальнейшей работы. |

**Приложения** содержат исходный код программы, примеры обнаружения атак, чек-лист тестирования и другие материалы.

---


# 📘 Глава 2. Теоретические основы

## 2.1. Протокол Kerberos: архитектура и этапы аутентификации

Протокол Kerberos — это механизм централизованной аутентификации, разработанный проектом Athena в Университете Беркли и стандартизированный в RFC 4120 как Kerberos версии 5. Его цель — обеспечить безопасное подтверждение подлинности пользователей и сервисов в небезопасной сети без передачи паролей по каналу связи.

### 2.1.1. Основные компоненты Kerberos

| Компонент | Описание |
|----------|----------|
| **KDC (Key Distribution Center)** | Центр распространения ключей, состоит из Authentication Server (AS) и Ticket Granting Server (TGS) |
| **TGT (Ticket Granting Ticket)** | Временный билет, выдаваемый при успешной аутентификации у AS |
| **TGS-билет (Service Ticket)** | Билет для доступа к конкретному сервису |
| **SPN (Service Principal Name)** | Уникальный идентификатор сервиса в домене |
| **Principal** | Имя пользователя или сервиса в формате user@REALM |

### 2.1.2. Этапы аутентификации в Kerberos

#### Этап 1. Запрос TGT (AS-REQ)
Клиент отправляет запрос на KDC с именем пользователя и другими данными. Он может содержать предварительную аутентификацию (pre-authentication).

#### Этап 2. Ответ от AS (AS-REP)
KDC проверяет личность пользователя. Если аутентификация успешна, он возвращает:
- **TGT**, зашифрованный ключом krbtgt,
- **Сессионный ключ**, зашифрованный хэшем пользователя.

#### Этап 3. Запрос TGS (TGS-REQ)
Для доступа к конкретному сервису клиент запрашивает у TGS билет на доступ к нему, используя TGT.

#### Этап 4. Ответ от TGS (TGS-REP)
KDC возвращает:
- **TGS-билет**, зашифрованный ключом сервиса,
- **Сессионный ключ клиента и сервиса**, зашифрованный сессионным ключом клиента и TGT.

#### Этап 5. Доступ к сервису (AP-REQ / AP-REP)
Клиент представляет TGS-билет и авторизуется у сервиса, который проверяет подлинность билета и позволяет получить доступ к ресурсу.

### 2.1.3. Безопасность Kerberos

- Все сообщения шифруются с использованием симметричных ключей.
- Пароль никогда не передается по сети.
- Билеты имеют ограниченное время жизни.
- Поддерживается pre-authentication для предотвращения offline-атак.

---

## 2.2. Роль Kerberos в Microsoft Active Directory

Microsoft Active Directory (AD) интегрирует протокол Kerberos как стандартный механизм аутентификации пользователей и компьютеров в доменной среде. Это делает Kerberos центральным элементом безопасности всей корпоративной инфраструктуры.

### 2.2.1. Интеграция Kerberos в AD

- **KDC** реализован в виде службы NetLogon на контроллерах домена.
- **krbtgt** — специальная учетная запись домена, используемая для шифрования TGT.
- Каждый компьютер в домене имеет свою учетную запись, которая используется для аутентификации в домене.

### 2.2.2. Политики Kerberos в AD

Политики определяют параметры безопасности Kerberos:

| Параметр | Описание |
|----------|----------|
| **max-ticket-life** | Максимальное время жизни TGT (по умолчанию 10 часов) |
| **max-renewable-ticket-life** | Максимальное время продления билета (до 7 дней) |
| **realm** | Имя домена (например, LAB.LOCAL) |
| **pre-authentication** | Обязательная предварительная аутентификация (по умолчанию включена) |

Эти политики можно настраивать через групповые политики (GPO), но их изменение влияет на уровень безопасности и производительность.

### 2.2.3. Журналирование событий Kerberos в Windows

Windows Event Logs содержит события, связанные с Kerberos:

| ID события | Описание |
|-----------|----------|
| **4768** | Запрос TGT |
| **4769** | Запрос TGS |
| **4771** | Ошибка аутентификации Kerberos |
| **4624** | Успешный вход в систему |
| **4625** | Неудачная попытка входа |

Эти события являются важным источником данных для обнаружения аномальной активности.

---

## 2.3. Типовые атаки на протокол Kerberos

Несмотря на надёжность самого протокола, злоумышленники находят способы его эксплуатировать, используя слабые места в конфигурации, управлении учётными записями и политиками безопасности.

### 2.3.1. Golden Ticket

- **Описание**: Создание поддельного TGT вне системы, используя хэш учетной записи krbtgt.
- **Цель**: Получение неограниченного доступа ко всем ресурсам домена.
- **Методы получения хэша krbtgt**:
  - Использование Mimikatz на контроллере домена.
  - Перехват трафика с помощью Pass-the-Ticket.
- **Обнаружение**:
  - Долгий TTL билета.
  - Использование имени пользователя, которого нет в системе.
  - Отсутствие события входа в систему.

### 2.3.2. Silver Ticket

- **Описание**: Подделка TGS-билета для конкретного сервиса с использованием хэша его учетной записи.
- **Цель**: Получение доступа к одному сервису без обращения к KDC.
- **Методы получения хэша сервиса**:
  - Извлечение из памяти сервера сервиса с помощью Mimikatz.
  - Использование ранее украденного хэша из дампа SAM/NTDS.
- **Обнаружение**:
  - Использование билета без предшествующего TGT.
  - Запросы к SPN, которые не соответствуют нормальному поведению.

### 2.3.3. Pass-the-Ticket (PtT)

- **Описание**: Использование украденного билета без расшифровки.
- **Инструменты**:
  - Mimikatz (`kerberos::list /export`)
  - Rubeus
- **Обнаружение**:
  - Повторное использование одного и того же билета.
  - Активность из разных хостов с одинаковым билетом.

### 2.3.4. Kerberoasting

- **Описание**: Запрос TGS для пользовательских SPN с последующим оффлайн-перебором хэшей.
- **Цель**: Получение паролей от учетных записей сервисов.
- **Методы выполнения**:
  - `GetUserSPNs.py` (Impacket)
  - Rubeus
- **Обнаружение**:
  - Множество запросов TGS к разным SPN.
  - Высокая частота таких запросов.

### 2.3.5. AS-REP Roasting

- **Описание**: Получение части зашифрованного ответа AS-REP без предварительной аутентификации.
- **Условие**: Выключение pre-authentication у определённых пользователей.
- **Методы выполнения**:
  - Rubeus (`asreproast`)
- **Обнаружение**:
  - Множество запросов AS-REQ без последующего ответа клиента.
  - Использование пользователей с выключенной pre-authentication.

### 2.3.6. Brute-force атаки через Kerberos

- **Описание**: Перебор паролей путём множественных попыток аутентификации.
- **Методы выполнения**:
  - Kerbrute
  - Rubeus
- **Обнаружение**:
  - Серия событий 4771 с ошибкой "bad password".
  - Повторные попытки входа от одного IP-адреса или пользователя.

---

## 2.4. MITRE ATT&CK и классификация атак на Kerberos

MITRE ATT&CK Framework предоставляет структурированное описание тактик и техник, применяемых злоумышленниками. Некоторые из них напрямую связаны с протоколом Kerberos:

| ID техники | Название | Описание |
|------------|----------|----------|
| T1097      | Pass the Ticket | Использование украденных билетов Kerberos |
| T1558      | Steal or Forge Kerberos Tickets | Создание поддельных билетов (Golden/Silver Ticket) |
| T1485      | Kerberoasting | Запрос и перебор TGS-билетов |
| T1533      | Account Manipulation | Изменение параметров учетных записей для облегчения атак |
| T1078      | Valid Accounts | Использование легитимных учетных данных для входа |

---

## 2.5. Заключение по главе

В данной главе были рассмотрены архитектура и принципы функционирования протокола Kerberos, его роль в среде Microsoft Active Directory, а также типовые атаки, направленные на обход легитимной аутентификации. Было показано, что большинство современных методов атак на доменные сети используют Kerberos как вектор проникновения, что подчеркивает необходимость внедрения систем обнаружения таких угроз.

Эти данные послужат основой для дальнейшего анализа поведенческих моделей и разработки программного обеспечения, способного выявлять атаки на основе аномального поведения Kerberos-сессий.

---
---

# 📘 Глава 3. Анализ атак и выявление аномалий

## 3.1. Характеристики нормального поведения в Kerberos-сессиях

Для эффективного обнаружения атак на протокол Kerberos необходимо понимать, как выглядит **нормальное поведение пользователей и сервисов** в доменной среде Active Directory. Это позволяет создать **базовые модели поведения (baseline)**, по которым можно сравнивать текущую активность и выявлять отклонения.

### 3.1.1. Обычные сценарии использования Kerberos:

#### a) Пользовательский вход:
- При входе пользователя в систему запрашивается **TGT (Ticket Granting Ticket)**.
- Затем запрашиваются **TGS (Ticket Granting Service tickets)** для доступа к ресурсам: файловым серверам, почтовым сервисам и т.д.
- Доступ к ресурсам осуществляется через **AP-REQ / AP-REP** сообщения.

#### b) Сервисное взаимодействие:
- Сервисы используют свои учетные записи и SPN (Service Principal Name).
- Они могут запрашивать билеты на другие сервисы (делегирование).

#### c) Обновление билетов:
- Билеты имеют ограниченное время жизни (обычно до 10 часов).
- Перед истечением срока действия они могут быть обновлены без повторной аутентификации.

### 3.1.2. Типичные показатели активности Kerberos:

| Метрика | Нормальный диапазон |
|--------|---------------------|
| Количество TGT за день | 1–3 на пользователя |
| Количество TGS за день | 5–20 на пользователя |
| Среднее TTL билета | 6–10 часов |
| Частота обращений к KDC | Равномерное распределение в течение рабочего дня |
| Ошибки аутентификации (ID 4771) | 0–2 попытки на пользователя |

### 3.1.3. Профили поведения:

#### a) Пользовательский профиль:
- Время входа (например, утро, будние дни).
- Используемые SPN: например, CIFS/fileserver, HTTP/webapp.
- Географическое расположение (IP-адрес, логическая зона сети).

#### b) Сервисный профиль:
- Регулярные запросы к другим сервисам.
- Фиксированное время активности (например, ночью — бэкапы, задания планировщика).
- Использование конкретных SPN.

#### c) Администраторский профиль:
- Доступ к критическим ресурсам (например, контроллер домена, SQL-сервер).
- Выполнение привилегированных операций (например, управление GPO, изменение политик Kerberos).

---

## 3.2. Признаки атак на протокол Kerberos

Некоторые из наиболее распространённых атак на протокол Kerberos оставляют характерные следы в сетевом трафике и логах событий безопасности Windows. Эти индикаторы могут быть использованы для автоматического обнаружения угроз.

### 3.2.1. Golden Ticket

- **Описание**: Создание поддельного TGT вне системы, используя хэш учетной записи krbtgt.
- **Цель**: Получение неограниченного доступа ко всем ресурсам домена.
- **Признаки:**
  - Длительное время жизни билета (до нескольких дней).
  - Отсутствие соответствующего события входа в систему.
  - Использование нестандартного имени пользователя или группы.
- **Логи Windows (Event ID):** 4768, 4769, 4624 (при входе).

### 3.2.2. Silver Ticket

- **Описание**: Подделка TGS-билета для конкретного сервиса с использованием хэша его учетной записи.
- **Цель**: Получение доступа к одному сервису без обращения к KDC.
- **Признаки:**
  - Отсутствие предшествующего TGT.
  - Запросы к необычным SPN.
  - Попытки доступа к сервисам без прохождения полной аутентификации.
- **Логи Windows (Event ID):** 4769, 4624, 4625.

### 3.2.3. Kerberoasting

- **Описание**: Запрос TGS для пользовательских SPN с последующим оффлайн-перебором хэшей.
- **Цель**: Получение паролей от учетных записей сервисов.
- **Признаки:**
  - Множество запросов TGS к разным SPN.
  - Запросы к SPN, связанным с учетными записями сервисов (например, MSSQLSvc, HTTP).
  - Высокая частота таких запросов в короткий промежуток времени.
- **Логи Windows (Event ID):** 4769.

### 3.2.4. AS-REP Roasting

- **Описание**: Получение части зашифрованного ответа AS-REP без предварительной аутентификации.
- **Условие**: Выключенная pre-authentication.
- **Признаки:**
  - Множество запросов AS-REQ без последующего ответа клиента.
  - Использование пользователей с выключенной pre-authentication.
- **Логи Windows (Event ID):** 4771 (ошибка "KDC_ERR_PREAUTH_REQUIRED" отсутствует).

### 3.2.5. Pass-the-Ticket

- **Описание**: Использование украденного билета без расшифровки.
- **Признаки:**
  - Повторное использование одного и того же билета.
  - Активность из разных хостов с одинаковым билетом.
  - Отсутствие событий аутентификации до использования билета.
- **Логи Windows (Event ID):** 4624, 4672.

### 3.2.6. Brute-force через Kerberos

- **Описание**: Перебор паролей путём множественных попыток аутентификации.
- **Признаки:**
  - Серия событий 4771 с ошибкой "bad password".
  - Повторные попытки входа от одного IP-адреса или пользователя.
  - Высокая частота запросов AS-REQ.
- **Логи Windows (Event ID):** 4771.

---

## 3.3. Методы обнаружения атак

Для анализа сетевого трафика и выявления атак на протокол Kerberos применяются различные методы, в том числе сигнатурный и поведенческий анализ. Комбинирование этих подходов позволяет повысить точность обнаружения и снизить количество ложных срабатываний.

### 3.3.1. Сигнатурный анализ

Сигнатурный метод заключается в сравнении текущих событий с заранее известными шаблонами атак.

#### Примеры сигнатур:

- **Golden Ticket**: TTL билета > 24 часа + использование krbtgt.
- **Kerberoasting**: более 20 TGS-запросов за минуту.
- **AS-REP Roasting**: AS-REQ без последующего AS-REP.

#### Инструменты:
- Sigma rules
- Snort / Suricata правила
- YARA правила для трафика

### 3.3.2. Поведенческий анализ

Поведенческий метод основан на построении моделей нормального поведения и выявлении отклонений от них.

#### Подходы:
- **Статистический анализ**: сравнение частоты событий с историческими данными.
- **Контекстный анализ**: учёт времени, географии, типа устройства, роли пользователя.
- **Межсетевой анализ**: выявление несоответствий между этапами Kerberos-сессии.

#### Примеры правил:
- Необычно высокое количество TGS-билетов для обычного пользователя.
- Использование TGS без предыдущего TGT.
- Активность Kerberos вне рабочего времени.

### 3.3.3. Возможности машинного обучения

Машинное обучение может применяться для построения сложных моделей поведения и автоматического обнаружения аномалий.

#### Перспективные алгоритмы:
- **Isolation Forest** — для обнаружения выбросов.
- **Autoencoder** — для выявления нехарактерных паттернов.
- **LSTM** — для анализа временных последовательностей Kerberos-событий.

#### Преимущества:
- Возможность обнаружения новых, ранее неизвестных атак.
- Автоматизация построения моделей поведения.
- Устойчивость к изменению поведения пользователей со временем.

#### Ограничения:
- Требуется большое количество качественных данных для обучения.
- Сложность интерпретации результатов.
- Высокие вычислительные требования.

---

## 3.4. Идентификаторы событий безопасности Windows

В дополнение к сетевому трафику, для обнаружения атак на Kerberos можно использовать журналы безопасности Windows, где фиксируются ключевые события аутентификации.

| Event ID | Описание |
|----------|-----------|
| **4768** | Запрос TGT (Authentication Service Request) |
| **4769** | Запрос TGS (Ticket Granting Service Request) |
| **4771** | Ошибка аутентификации Kerberos (например, неверный пароль) |
| **4672** | Назначение специальных привилегий при входе пользователя |
| **4624** | Успешный вход в систему |
| **4625** | Неудачная попытка входа |

Эти события позволяют строить модель поведения и выявлять аномальную активность даже при отсутствии возможности перехвата сетевого трафика.

---

## 3.5. Заключение по главе

В данной главе были рассмотрены модели нормального поведения Kerberos-сессий, а также признаки наиболее распространённых атак на протокол Kerberos. Было показано, что большинство современных атак оставляют характерные следы в сетевом трафике и логах безопасности, что позволяет реализовать системы обнаружения на основе анализа аномалий.

Были представлены три основных метода обнаружения: сигнатурный, поведенческий и на основе машинного обучения. На основе этого материала была спроектирована и реализована программная система, способная анализировать сетевой трафик Active Directory и выявлять признаки атак на протокол Kerberos.

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


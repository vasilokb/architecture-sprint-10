# MVP компании «Медикаменте»

## 1. Требования по FURPS+
### Функциональность (Functionality)
- **Запись пациентов**: Клиенты могут самостоятельно записываться к специалистам через клиентский портал. Сотрудники ресепшена просматривают и управляют записями через портал ресепшена, отправляя напоминания за день до приёма.
- **Управление данными пациентов**: Персональные данные (ФИО, дата рождения, телефон, email) и медицинские данные (хронические заболевания, анализы) собираются и хранятся с учётом конфиденциальности.
- **Ведение журнала записей**: Автоматизированный журнал доступен администраторам и врачам через соответствующие порталы.
- **Хранение медицинских карт**: Централизованное хранилище с доступом по ролям (RBAC) для врачей и пациентов.
- **Интеграция с лабораторией**: Получение результатов анализов через API с маскированием PII.
- **Обработка платежей**: Интеграция с контрольно-кассовыми машинами (ККМ) через адаптер платежей, подготовка к будущему платёжному шлюзу.
- **Уведомления**: Автоматические напоминания пациентам и сотрудникам через email с минимальным использованием PII.
- **Удаление данных**: Обработка запросов клиентов на удаление данных с уведомлением об успешном выполнении.

### Надёжность (Usability)
- Интуитивно понятные интерфейсы порталов (React.js) с валидацией данных на фронтенде и бэкенде для минимизации ошибок.
- Простая навигация для записи и просмотра данных.

### Производительность (Reliability)
- Система обрабатывает до 500 одновременных запросов на запись, поддерживая пятикратный рост клиентской базы.
- Время ответа API < 1 секунды для 95% запросов.

### Поддерживаемость (Performance)
- Микросервисная архитектура с оркестрацией в Kubernetes упрощает обновления и масштабирование.
- Логирование и мониторинг через Victoria Metrics для быстрого выявления проблем.

### Безопасность (Supportability)
- **Шифрование**: Данные в базах (pgcrypto) и файлах (S3 SSE-KMS) шифруются, передача защищена TLS 1.3.
- **Контроль доступа**: RBAC через сервис безопасности, пациенты видят только свои данные, врачи — данные своих пациентов.
- **Аудит**: Все операции с PII логируются через шину событий (Kafka).
- **Удаление данных**: Обработка запросов на удаление в течение 30 дней (ФЗ-152).
- **Мониторинг**: Алертинг при аномалиях через Victoria Metrics.

### Дополнительные требования (+)
- **Масштабируемость**: Архитектура поддерживает рост филиалов и пользователей.
- **Конфигурируемость**: Настройки RBAC и тегов PII изменяются через конфигурационные файлы.
- **Соответствие законодательству**: Полное соответствие ФЗ-152 (персональные данные) и ФЗ-323 (медицинская информация).

## 2. Описание MVP
MVP представляет собой автоматизированную систему для записи пациентов, управления данными и интеграции с лабораторией, с упором на защиту конфиденциальных данных. Основные функции:
- **Клиентский портал**: Позволяет пациентам записываться к специалистам, просматривать свои данные (анализы, записи) и запрашивать удаление данных.
- **Портал ресепшена**: Обеспечивает управление записями, контрактами и отправку напоминаний.
- **Портал врача**: Даёт доступ к записям и медицинским данным пациентов.
- **Интеграция с лабораторией**: Автоматический обмен данными с внешними лабораториями, маскирующий PII.
- **Уведомления**: Автоматические напоминания о приёмах с минимальным использованием PII.
- **Обработка платежей**: Интеграция с ККМ и 1С Бухгалтерия через адаптер платежей, анонимизирующий данные.
- **Удаление данных**: Сервис управления данными обрабатывает запросы на удаление с уведомлением клиентов.
- **Безопасность**: Шифрование, RBAC, аудит операций и мониторинг аномалий.

Архитектура построена на микросервисах, развёрнутых в Kubernetes, с использованием технологий из стека IT-отдела (Java, Spring Boot, Postgres, Kafka, Victoria Metrics). Существующие системы (1С Бухгалтерия, 1С Торговля и склад, ККМ, Exchange Mail Server) сохранены для минимизации изменений.

## 3. Диаграмма C4 и описание
### Диаграмма контейнеров C4
![](https://www.plantuml.com/plantuml/png/h5VVJnl75dxdhvZBomLamyrDsuSyDO4qIff8Rb3xKbNEi1xC5dlNsXr3K5KfCKaX1OMaQfGeJK99sggF1kA4t_mBi_zHppS7rjxLBWXK72dZyPdlVEVCET_CV1vg6UXclTRt7zTpQisA4bDQDy9B8oE1d1skkdggET4CLU3udbQU7dRy-aYZ9ZsiAOQwKdCdHaO_APOmzVMjcoDr6MeLq5Hvb5P8rrF1S0FuVVyLPjKScDresNoa_pTCEsgPZjarMtqb5OI-Dz2GsYKV1T5Fbkte9xD5l-z7hNxCl8GbVnTcZmRlpHPCQEA-sI70RK63hUY-HTsBble7Zt5bfUvwRgW3gVq0M4zeyHwMidrqpt8vYEvPJsiqehbedgp8mkm8CkWGUCSSHekWODfTy8hl7A7-Od4MZZ0EQGdejS4RNehC6e3TrOuCGrS5762Rp102qrWbE4kHrhK9ZC8-92pEsWUprGMPyFtfQQKQZFCqceyUcaquXYdGG0w-ewT0MKyRs0HkKGBfXmNOTA5dPQ2cV2g0CZl8pWy0G8qcsablCQGmRyqhyrUCNBvwLm_Kv0IiVwELwmGQRmFbOEK8Pvy8Bj8UhqGhWbna60gQtESqSs6H3nJR6sArZ-_ubSnwgyEq-wxuJQyYWxc1kgguZgnJuIcEhiTA4ETdvbVpWdRdYHs_fZYUzG-A7_e4_P6Rrt0HBPaE750ooHy0bkpliHDFpPQfYsPSHuNmahSgHiPVt443ik6MgrAhMJb7go-NhXSfARlHO_CHQHfl1AvN5QCrlraHrzWCyqIuJVkqYBsAMaWFkWevAuZRLowF5eITUKGpBOeLsoh6nlpn_i4C2auDlLbk-86MjLH3nWLa3h4hG3ExO71RIKSFVnzQDizfzj26kIom9sHjT_2fsIlm-820zhYw16h9x1FwKlGmar2W7DNGhk_rS5hjkizgsZIZTzPhcvA_2mk4Goc9QQv090J-GLyG9untW8zVCbcn7EHGCXii4HbKLjFXnpI8sJQHo10z6ZyyFK-oqV1TJwF0ZqkGwpmkkvGpyBaXPsH1S65H5VlwNyYN-BPq_RgmhRK9bUD3W66-owJE1q8-xSFUKm9mPwH-oEe7ibODbv56KEt9wlcOiwZKSfcVlfVveVn9C32BmyW1sx7TtyA6f2Nop051gijKHQeQIFHSEY1uPZYG0nFoYlaCoJwvnbwaWQnq2zAyJ31i3fGQOQSZ4HHaTZIUhzr9rx6XvEqHdMyRI2mXxiL5aq-SpfjMN6EKUYjnq4CK4CQy7coxntfSSzlvN2jIot9TUhAgydiOTDCL7gsaolW4xcQTrd9oMyoUsZhMP5GCGe0J8COjTAhwZ4qazASoOMyefWtunE8C3IhveQu6AamKFHrLlL5rKkkor8SBFbeacmUdbpeeGtTEePkjFVcKiwMcaupbB2HZYM6Ihz8QOEyyPfsvdyGjKp_EpsnWqgsfSgZzW0esuSfUX1yHMFI09hRfwr7MnY-cESKcqL96fiL0QUoBWhXHkZPu5cAXSfg1g-ToHNdTT22FSCXtHrm4JsYRepjK6rq1rZIcuviZO7MYyIObaf78Z90pgQaPb1izjC3fRzOBWMgXlbm21-w8B-NajEJh0dXZs_WwaRBjuPJZiEvxhlOHJJcS2xMgPrnzCsBXIlh6TR1MYbjA1wx35RN6ocohYMB5WXrq8yi53YA6o3devwm-o8fiQ1M_qQ10BInEF7tETNwOb_Hv1K8Np1g9S1lxopBSUp7kbU4V-uxl-6lHF0hN9cU3ne_f6vLPwi7PbunfeZdLpIsSfbzctNTEbeECDmjVPFWzbSu8AzzwfeV153CTo8fASNjhDtaJNKHhLu3MVTMTcoE_-TB-fwVXbdVahD0lOsq_O1Kr1qMYoKzdaCSwM4CNZbxggTANBj_BR9rTlUjCIQ-gn2sQ5kDs-goSSCLVO6d2KPvuqf7xuoe2zJtwi8wr-VPYdqnYJ6efIe4x8vqvQ-xvMed0hKvfuK-AxdaKzyxGKAoNMMVnqD2bsBGe81Uddpu3vW_gdZS5lXUYQHzWGK4ef6nG72ijae4t7pSM-DjNowDeENei6bKdc6jeVxZN_TkiUwR0jf56t0cL5xhQdL5tY0yNtqvYbrTI4fh0Vfv-mOv_TRDe3xAVoUiEsXfRid0ac1y5492vhb3ndRYRYSJb9rfAkFiTFcArG8DGCWWSFj8d4LlGy8DvRt4_OtMapD7wZjaOxjtETrZ1TKGBJjzKMf2wYw6I4jJdBS7uw6ptfY7dMYdeUBNIiH3ZNlmVMEmZHbiZpuiNFlt_O28hJwZomHjzr_gdVQjPRIeAmli-nLqcq5RJ849NWXGKMmozsRbj5uBu1eAq18HkHBfmKUZ61v3OQXQbFaDUmIVnV9dap9qvS-xSf3TGfJkRcfbpvdpppSzCSGlSSlQArqdEM3t4K-JqrDh2WV53vP-a5QGKJeMBWQs3p2iSXHmJDjwMHacJzcjj1iHAVQv9zLnMVI0UIw0k_qJAHNUZUr03fkXKWHtiJgG6mnMgzWKXPvvCFeNA37QBuNvPzBRxqRV6CteQ5UWYwIoV91s_FveHq8P1HPnrhaX55twosQcMgzEpJVmMZTGVO7jyiZukcb-FcUYM4WdoJRhV0n_T6puIfj0Wx3sEx4AFQHKwil9aYkllzBij4SlwYMD-FzrqYlr6t6lJAAIMOWuG1bYexEDVFbF8kdz5jZXVlj9lcHfG4LYfIWSK1MGZAC5ocsY2NP5nSG8KG8i-bRRzz-u3PznBpdWYASD6M8UY4s83Zt5ES2RKm6ouVuv0p55149EPbgv48JvnL1j8AzB6jzWzubPH8s80kvNbtjQhWt9RPIsmk4XJk7tE4jD4WlfULUfewelX3DBrUZl_jU-XiWvwHb0---YNuE9Rl1QddqKdnlTkAmx2BqykWYyxjQS3zyQXKdKc1Ue_Bpfv7F2u_Q4E9w-8I-ogo8D-wXXnAa3eH1GSCFyu_3WHkJZd4E4NVpwUYr_6hvZzy3hcc9wHC8xe4s5OldGiLz2NXRb93V8qO7tnV1XbytWPkOvy6aJnRXM-iUCgz6QQXqctD693DJVZKBiWoDAC1sAc8pDZkE7E5Lf7uZsxjTx2yKTqUujUCVlq2kTEepcwLZuUrR2l2iUB_AXDeayxXmHRFbM2L42TtcbwYg_WOWIgP99PiH7Ea6jYTGkS6i33cPcjnrG69_FgwIY7HmL380tNEXNL8REEZJdt1Qm6X7o6YTsyLe_mDLpxjzowMh3j9ypxW2BQaW5UpD7a3TcrcXkyFI9U09g2MjxZa8cjj9iOZquourNNHLDOyHVAW-40rvYpi6zbDNOGj6lF1toS0rFS-fUebrC7ZcyC5ucTnEY8oUWX6mK4DPQ3_k7pmQENWwtN-tiqiptSAuT0kRQCZ-qcsFqmsxNwak_ERHmUl-FDFhb5TBUbeF3r5yFXe2pUpA-5mGsqgNq_lIZKT9Cbgo3k_Ebb7t-Bf-4Db9da-XTinfLTo0d4m058o0jcMxGlcwtHic0XoPhYNf8RGHu2YSgAO7OEaL0JNQ9EarqDrIpL9JiJpLN1FzQlXXj4wrMJAGrI5FWx9OvIuu7ITHEPEJNj8q5pQCFa5moo9p0UyrBa2FgIINTewLkR-KBpfQHQVSbrv7PCNLNUo8O9ABd6Qxxjre9QlRUY5QTRAwQvLEviEbsIm_YvhlP0Ns_um8RUKc9jOCaPkaOP3OmX5XcP5iaxxxG7GEoDXWOg8QgZn7O3-GEdDNCn-BREu2pZIa-xLQ8TYadaSsfdwV8Jl3koct_VUB-GGlriGoUdVibJEuj7SME9YQHzmUpARGkMu6zTklROzWGhaBGds0xiEYJenkBotAprwD2mTZti1-k_aumxMfCVTspvVcfWpoFC0KGjPp2uxDnSnvzb09SArAqucuQpEuXUl1D_IyLSxuSfNaiaGSPVJgcrExqbnryL_n6dyBwqyIpKu-6FxFu2)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

' Пользователи
Person(patient, "Пациент", "Клиент медицинской системы")
Person(administrator, "Администратор", "Сотрудник ресепшена")
Person(doctor, "Врач", "Специалист-медик")
Person(cassier, "Кассир", "Принимает платежи")
Person(bookkeeper, "Бухгалтер", "Ведёт бухгалтерский учёт")
Person(warehouse_keeper, "Сотрудник склада", "Ведёт учёт ТМЦ")
Person_Ext(lab, "Лаборатория", "Внешняя лаборатория анализов")

' Основная система
Boundary(medicamente, "Система МЕДИКАМЕНТЕ") {
    ' Новые сервисы
    Boundary(new_services, "Новые сервисы") {
        Container(api_gateway, "API-шлюз", "Spring Cloud Gateway", "Маршрутизация, RBAC, защита от DDoS")
        Container(client_portal, "Клиентский портал", "React.js", "Запись на приём, просмотр данных")
        Container(reception_portal, "Портал ресепшена", "React.js", "Управление записями и контрактами")
        Container(doctor_portal, "Портал врача", "React.js", "Просмотр записей и медицинских данных")
        Container(appointment_service, "Сервис записей", "Java, Spring Boot", "Управление записями и контрактами [PII тегирование]")
        Container(patient_service, "Сервис пациентов", "Java, Spring Boot", "Управление персональными данными [PII тегирование]")
        Container(medical_service, "Сервис медицинских данных", "Java, Spring Boot", "Хранение медицинских карт и анализов [PII тегирование]")
        Container(lab_integration, "Сервис интеграции с лабораторией", "Java, Spring Boot", "Интеграция с API лаборатории [маскирование PII]")
        Container(notification_service, "Сервис уведомлений", "Java, Spring Boot", "Отправка напоминаний [минимальный PII]")
        Container(data_management_service, "Сервис управления данными", "Java, Spring Boot", "Обработка запросов на удаление данных")
        ContainerDb(appointment_db, "База данных записей", "Postgres", "Хранит данные записей и контрактов [шифрование]")
        ContainerDb(patient_db, "База данных пациентов", "Postgres", "Хранит персональные данные [шифрование]")
        ContainerDb(medical_db, "Медицинская база", "Postgres", "Хранит медицинские данные [шифрование]")
        Container(file_storage, "Хранилище файлов", "S3", "Хранит файлы пациентов (PDF, JPG) [шифрование]")
        Container(security_service, "Сервис безопасности", "Java, Spring Boot", "RBAC, аудит, мониторинг доступа")
        Container(event_bus, "Шина событий", "Kafka", "Передача событий аудита")
        Container(monitoring_system, "Система мониторинга", "Victoria Metrics", "Сбор метрик и алертинг")
        Container(payment_adapter, "Адаптер платежей", "Java, Spring Boot", "Интеграция с ККМ и 1С [анонимизация PII]")
    }

    ' Существующие системы
    Container(buh_system, "1С Бухгалтерия", "Существующая система", "Учёт платежей и бухгалтерия")
    Container(trade_system, "1С Торговля и склад", "Существующая система", "Учёт ТМЦ")
    Container(kkm, "ККМ", "Существующая система", "Контрольно-кассовая машина")
    Container(mail_server, "Exchange Mail Server", "Существующая система", "Почтовый сервис")

    ' Комментарии по Data Privacy
    note right of patient_db
        **Хранение данных**:
        - Персональные данные (ФИО, телефон, email) шифруются с помощью pgcrypto.
        - Тегирование как `sensitive` для контроля доступа.
        - Защита от SQL-инъекций через параметризованные запросы.
        - Ротация ключей шифрования каждые 6 месяцев.
        **Уничтожение данных**:
        - Удаление по запросу клиента через data_management_service в течение 30 дней (ФЗ-152).
        - Автоматическое удаление через 7 лет после последнего взаимодействия.
        - Уведомление клиента об удалении через notification_service.
        **Защита данных**:
        - Хранение: pgcrypto, RBAC (доступ только patient_service и data_management_service).
        - Передача: TLS 1.3 для всех API-запросов.
    end note

    note right of medical_db
        **Хранение данных**:
        - Медицинские данные (анализы, диагнозы) шифруются с помощью pgcrypto.
        - Тегирование как `medical` для ограничения доступа.
        - Защита от SQL-инъекций через параметризованные запросы.
        - Ротация ключей шифрования каждые 6 месяцев.
        **Уничтожение данных**:
        - Удаление по запросу клиента через data_management_service в течение 30 дней.
        - Хранение 10 лет для медицинских записей (ФЗ-323).
        - Уведомление клиента об удалении.
        **Защита данных**:
        - Хранение: pgcrypto, RBAC (доступ для medical_service, врачей, пациентов).
        - Передача: TLS 1.3 для API и S3 API.
    end note

    note right of appointment_db
        **Хранение данных**:
        - Данные записей и контрактов шифруются с помощью pgcrypto.
        - Тегирование PII как `sensitive` (например, идентификатор пациента).
        - Защита от SQL-инъекций через параметризованные запросы.
        - Ротация ключей шифрования каждые 6 месяцев.
        **Уничтожение данных**:
        - Удаление по запросу клиента через data_management_service в течение 30 дней.
        - Хранение 5 лет для контрактов (налоговые требования).
        - Уведомление клиента об удалении.
        **Защита данных**:
        - Хранение: pgcrypto, RBAC (доступ для appointment_service, администраторов).
        - Передача: TLS 1.3 для API.
    end note

    note right of file_storage
        **Хранение данных**:
        - Файлы (PDF, JPG) шифруются с помощью S3 SSE-KMS.
        - Метаданные тегируются как `medical` или `sensitive`.
        - Ротация ключей KMS каждые 12 месяцев.
        **Уничтожение данных**:
        - Удаление по запросу клиента через data_management_service в течение 30 дней.
        - Хранение синхронизировано с medical_db (10 лет).
        **Защита данных**:
        - Хранение: S3 SSE-KMS, RBAC для доступа.
        - Передача: TLS 1.3 для S3 API.
    end note

    note right of security_service
        **Аудит и контроль доступа**:
        - RBAC: пациенты видят только свои данные, врачи — данные своих пациентов, администраторы — записи.
        - Аудит всех операций (чтение, запись, передача PII) через event_bus.
        - Интеграция с monitoring_system для алертинга при аномалиях (например, массовый доступ).
        - Хранение логов аудита 1 год с шифрованием.
    end note

    note right of lab_integration
        **Маскирование PII**:
        - Передача только идентификаторов и результатов анализов.
        - PII (ФИО, телефон) скрываются в API-контрактах.
        - Аудит операций получения данных через event_bus.
    end note

    note right of notification_service
        **Минимизация PII**:
        - Использование шаблонов с минимальным PII (appointment_id, время приёма).
        - Аудит отправки уведомлений через event_bus.
        - Передача через SMTP с TLS 1.3.
    end note

    note right of payment_adapter
        **Анонимизация PII**:
        - Передача в buh_system только анонимизированных данных (appointment_id, сумма).
        - Замена TCP/IP-OLE на REST API с TLS 1.3.
        - Аудит операций через event_bus.
    end note
}

' Взаимодействия
Rel(patient, client_portal, "Запись на приём, просмотр данных", "HTTPS [TLS 1.3]")
Rel(administrator, reception_portal, "Управление записями", "HTTPS [TLS 1.3]")
Rel(doctor, doctor_portal, "Просмотр записей и данных", "HTTPS [TLS 1.3]")
Rel(client_portal, api_gateway, "Запросы к сервисам", "HTTPS [TLS 1.3]")
Rel(reception_portal, api_gateway, "Запросы к сервисам", "HTTPS [TLS 1.3]")
Rel(doctor_portal, api_gateway, "Запросы к сервисам", "HTTPS [TLS 1.3]")
Rel(api_gateway, security_service, "Проверка доступа и аудит", "REST API [TLS 1.3]")
Rel(api_gateway, appointment_service, "Маршрутизация запросов", "REST API [TLS 1.3]")
Rel(api_gateway, patient_service, "Маршрутизация запросов", "REST API [TLS 1.3]")
Rel(api_gateway, medical_service, "Маршрутизация запросов", "REST API [TLS 1.3]")
Rel(api_gateway, data_management_service, "Запросы на удаление данных", "REST API [TLS 1.3]")
Rel(appointment_service, appointment_db, "Хранение записей и контрактов", "SQL [шифрование]")
Rel(appointment_service, patient_service, "Получение данных пациента", "REST API [TLS 1.3]")
Rel(appointment_service, notification_service, "Отправка напоминаний", "REST API [TLS 1.3]")
Rel(patient_service, patient_db, "Хранение данных", "SQL [шифрование]")
Rel(medical_service, medical_db, "Хранение медицинских данных", "SQL [шифрование]")
Rel(medical_service, file_storage, "Хранение файлов", "S3 API [TLS 1.3]")
Rel(lab_integration, medical_service, "Передача результатов анализов", "REST API [TLS 1.3]")
Rel(lab_integration, lab, "Получение анализов", "REST API [TLS 1.3]")
Rel(notification_service, mail_server, "Отправка email", "SMTP [TLS 1.3]")
Rel(cassier, kkm, "Процессинг платежа", "TCP/IP-OLE")
Rel(kkm, payment_adapter, "Передача данных о платежах", "REST API [TLS 1.3]")
Rel(payment_adapter, buh_system, "Передача анонимизированных данных", "REST API [TLS 1.3]")
Rel(appointment_service, payment_adapter, "Передача платёжных данных", "REST API [TLS 1.3]")
Rel(appointment_service, event_bus, "Отправка событий аудита", "Kafka")
Rel(patient_service, event_bus, "Отправка событий аудита", "Kafka")
Rel(medical_service, event_bus, "Отправка событий аудита", "Kafka")
Rel(notification_service, event_bus, "Отправка событий аудита", "Kafka")
Rel(lab_integration, event_bus, "Отправка событий аудита", "Kafka")
Rel(payment_adapter, event_bus, "Отправка событий аудита", "Kafka")
Rel(data_management_service, patient_db, "Удаление данных", "SQL [шифрование]")
Rel(data_management_service, medical_db, "Удаление данных", "SQL [шифрование]")
Rel(data_management_service, appointment_db, "Удаление данных", "SQL [шифрование]")
Rel(data_management_service, file_storage, "Удаление файлов", "S3 API [TLS 1.3]")
Rel(data_management_service, notification_service, "Уведомление об удалении", "REST API [TLS 1.3]")
Rel(event_bus, security_service, "Получение событий аудита", "Kafka")
Rel(security_service, monitoring_system, "Передача метрик и алертов", "REST API [TLS 1.3]")
Rel(bookkeeper, buh_system, "Процессинг платежей", "")
Rel(warehouse_keeper, trade_system, "Учёт ТМЦ", "")
Rel(trade_system, buh_system, "Учёт ТМЦ", "")

@enduml
```


### Описание диаграммы C4
Диаграмма контейнеров C4 описывает архитектуру MVP, включая новые сервисы, существующие системы и меры защиты конфиденциальных данных.

**Компоненты**:
- **Новые сервисы** (граница `new_services`):
    - `api_gateway` (Spring Cloud Gateway): Единая точка входа для запросов, реализует RBAC и защиту от DDoS.
    - `client_portal`, `reception_portal`, `doctor_portal` (React.js): Интерфейсы для пациентов, администраторов и врачей.
    - `appointment_service` (Java, Spring Boot): Управляет записями и контрактами, использует `appointment_db`.
    - `patient_service` (Java, Spring Boot): Хранит персональные данные в `patient_db`.
    - `medical_service` (Java, Spring Boot): Управляет медицинскими данными в `medical_db` и файлами в `file_storage`.
    - `lab_integration` (Java, Spring Boot): Интегрируется с лабораторией, маскируя PII.
    - `notification_service` (Java, Spring Boot): Отправляет напоминания с минимальным PII через `mail_server`.
    - `data_management_service` (Java, Spring Boot): Обрабатывает запросы на удаление данных.
    - `security_service` (Java, Spring Boot): Управляет RBAC, аудитом и мониторингом.
    - `event_bus` (Kafka): Передаёт события аудита от всех сервисов к `security_service`.
    - `monitoring_system` (Victoria Metrics): Собирает метрики и алерты.
    - `payment_adapter` (Java, Spring Boot): Интегрирует ККМ и `buh_system`, анонимизируя PII.
    - Базы данных (`appointment_db`, `patient_db`, `medical_db`) (Postgres): Хранят данные с шифрованием (pgcrypto).
    - `file_storage` (S3): Хранит файлы с шифрованием (SSE-KMS).
- **Существующие системы**:
    - `buh_system` (1С Бухгалтерия): Учёт платежей.
    - `trade_system` (1С Торговля и склад): Учёт ТМЦ.
    - `kkm`: Контрольно-кассовая машина.
    - `mail_server` (Exchange): Отправка email.

**Взаимодействия**:
- Пользователи взаимодействуют с порталами через HTTPS (TLS 1.3).
- Запросы проходят через `api_gateway`, который проверяет доступ в `security_service`.
- Сервисы обмениваются данными через REST API (TLS 1.3) и хранят данные в базах (SQL, шифрование).
- `lab_integration` получает данные от лаборатории, отправляя события аудита в `event_bus`.
- `notification_service` отправляет email через `mail_server` и логирует операции.
- `payment_adapter` заменяет TCP/IP-OLE на REST API для ККМ и `buh_system`, анонимизируя данные.
- `data_management_service` удаляет данные из баз и уведомляет клиентов через `notification_service`.
- Все сервисы, работающие с PII, отправляют события аудита в `event_bus`.
- `security_service` передаёт метрики в `monitoring_system` для алертинга.

**Меры Data Privacy** (подробно в комментариях на диаграмме):
- **Хранение**: Шифрование (pgcrypto, S3 SSE-KMS), тегирование PII (`sensitive`, `medical`), защита от SQL-инъекций, ротация ключей (6–12 месяцев).
- **Уничтожение**: Удаление данных по запросу через `data_management_service` в течение 30 дней, хранение 5–10 лет в зависимости от типа данных, уведомление клиентов.
- **Защита**: TLS 1.3 для передачи, RBAC, аудит через `event_bus`, маскирование и анонимизация PII, защита от DDoS.

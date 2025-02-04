# Домашнее задание к занятию «Микросервисы: принципы» - Скрыпников Илья 
#
## Задача 1: API Gateway  
### Сравнительная таблица решений:

| Решение           | Маршрутизация | Аутентификация (встроенная) | HTTPS | Кластеризация | Управление          | Лицензия                     |
|--------------------|---------------|-----------------------------|-------|---------------|---------------------|------------------------------|
| **NGINX**          | Да            | Через модули (Lua)          | Да    | Только Plus   | Конфигурационные файлы | Open-Source / Платная (Plus) |
| **Kong**           | Да            | Да (JWT, OAuth, плагины)    | Да    | Да            | API/Admin-интерфейс | Open-Source                  |
| **Apache APISIX**  | Да            | Да (плагины)                | Да    | Да            | API/Dashboard       | Open-Source                  |
| **AWS API Gateway**| Да            | Да (IAM, Cognito)           | Да    | Управляется AWS | AWS Console         | Платная (SaaS)               |
| **Envoy**          | Да            | Через фильтры (OPA)         | Да    | Да            | xDS                | Open-Source                  |

### Выбор: **Apache APISIX**  
**Обоснование:**  
1. **Маршрутизация и HTTPS**:  
   - Динамическая маршрутизация и терминация HTTPS из коробки.  
   - Поддержка REST, gRPC, WebSocket.  

2. **Аутентификация**:  
   - Встроенные плагины для JWT, OAuth, Key Auth.  
   - Интеграция с LDAP и внешними системами.  

3. **Кластеризация**:  
   - Использует **etcd** для распределённого хранения конфигураций.  

4. **Гибкость**:  
   - Поддержка Lua и WebAssembly для кастомизации.  

5. **Простота эксплуатации**:  
   - Удобный веб-интерфейс (Dashboard) и API.  

**Альтернативы:**  
- **Kong**: Аналогичен APISIX, но менее гибок в расширении.  
- **AWS API Gateway**: Подходит для AWS-экосистемы, но привязывает к облаку.  

---

## Задача 2: Брокер сообщений  
### Сравнительная таблица решений:

| Решение           | Кластеризация | Сохранение на диск | Скорость | Форматы сообщений | Права доступа | Простота эксплуатации |
|--------------------|---------------|--------------------|----------|-------------------|---------------|------------------------|
| **RabbitMQ**       | Да            | Да (Durable queues)| Средняя  | Любые             | ACL, VHost    | Средняя               |
| **Apache Kafka**   | Да            | Да                 | Очень высокая | Любые             | SASL, ACL     | Сложная (ZooKeeper)   |
| **NATS JetStream** | Да            | Да                 | Высокая  | Любые             | JWT, ACL      | Средняя               |
| **Redis Streams**  | Да (Cluster)  | Да                 | Очень высокая | Любые             | RBAC          | Простая               |
| **AWS SQS**        | Да (managed)  | Да                 | Высокая  | Любые             | IAM           | Очень простая (SaaS)  |

### Выбор: **NATS JetStream**  
**Обоснование:**  
1. **Кластеризация**:  
   - Встроенная кластеризация без зависимостей (в отличие от Kafka с ZooKeeper).  

2. **Скорость**:  
   - Низкая задержка, оптимизирован для микросервисов.  

3. **Сохранение сообщений**:  
   - Гарантированная доставка и сохранение на диск через JetStream.  

4. **Разделение прав**:  
   - Гибкие настройки через JWT и ACL.  

5. **Простота**:  
   - Легковесный, интегрируется с Kubernetes.  

**Альтернативы:**  
- **Kafka**: Лучше для Big Data, но сложнее в управлении.  
- **Redis Streams**: Подходит для проектов с Redis, но слабее в правах доступа.  

---

## Итог:  
- **API Gateway**: **Apache APISIX** — современное, гибкое и легко управляемое решение.  
- **Брокер сообщений**: **NATS JetStream** — оптимален для микросервисов с упором на скорость и простоту.

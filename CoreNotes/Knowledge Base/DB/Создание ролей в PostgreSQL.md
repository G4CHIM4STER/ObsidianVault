___
Теги: #knowledge 
Дата создания: 2025-02-13 14:22 
Последнее изменение: четверг 13-го февраля 2025 14:22:37
<< [[2025-02-12]] | [[2025-02-14]] >> 
___
## Основное

```sql
-- 1. Создаем роль-группу chuvakadmin
CREATE ROLE chuvakadmin WITH NOLOGIN;

-- 2. Создаем пользователей и добавляем их в группу
CREATE ROLE nliskov WITH LOGIN PASSWORD 'secure_password1';
CREATE ROLE aborisov WITH LOGIN PASSWORD 'secure_password2';

GRANT chuvakadmin TO nliskov, aborisov;

-- 3. Даем права на подключение к базе данных
GRANT CONNECT ON DATABASE your_database_name TO chuvakadmin;

-- 4. Даем права на использование схемы
GRANT USAGE ON SCHEMA your_schema_name TO chuvakadmin;

-- 5. Даем полные права на все объекты в схеме
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA your_schema_name TO chuvakadmin;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA your_schema_name TO chuvakadmin;
GRANT ALL PRIVILEGES ON ALL FUNCTIONS IN SCHEMA your_schema_name TO chuvakadmin;
GRANT ALL PRIVILEGES ON ALL PROCEDURES IN SCHEMA your_schema_name TO chuvakadmin;

-- 6. Устанавливаем права по умолчанию для будущих объектов
ALTER DEFAULT PRIVILEGES IN SCHEMA your_schema_name 
GRANT ALL PRIVILEGES ON TABLES TO chuvakadmin;

ALTER DEFAULT PRIVILEGES IN SCHEMA your_schema_name 
GRANT ALL PRIVILEGES ON SEQUENCES TO chuvakadmin;

ALTER DEFAULT PRIVILEGES IN SCHEMA your_schema_name 
GRANT ALL PRIVILEGES ON FUNCTIONS TO chuvakadmin;

ALTER DEFAULT PRIVILEGES IN SCHEMA your_schema_name 
GRANT ALL PRIVILEGES ON PROCEDURES TO chuvakadmin;
```

___
### Zero-Links
- 

### Links
- 

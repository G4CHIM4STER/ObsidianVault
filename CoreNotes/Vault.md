## Развертывание в докере

### Cоздать docker-compose

```
version: "3.9"

services:

  vault:

    image: 10.100.22.3:10002/hashicorp/vault:1.15.2

    container_name: vault

    restart: always

    ports:

      - "8080:8080"

    cap_add:

      - IPC_LOCK

    environment:

      VAULT_ADDR: "http://0.0.0.0:8080"

      VAULT_API_ADDR: "http://vault:8080"

      VAULT_LOG_LEVEL: "info"

    volumes:

      - ./vault/config:/vault/config

      - ./vault/data:/vault/data

    command: ["vault", "server", "-config=/vault/config/vault.hcl"]

    depends_on:

      - postgres

  

  postgres:

    image: 10.100.22.3:10002/postgres:15.4

    container_name: vault-postgres

    restart: always

    environment:

      POSTGRES_USER: vault

      POSTGRES_PASSWORD: 123qwe!E

      POSTGRES_DB: vault

    ports:

      - "5432:5432"

    volumes:

      - postgres_data:/var/lib/postgresql/data

  

volumes:

  postgres_data:
```

**Запустить** 

```
docker-compose up -d
```

### Создать в ручную необходимую конфигурацию Postgres для Vault

**Зайти в контейнер с postgres**

```
docker exec -it vault-postgres psql -U vault
```

**Создать в ручную необходимую конфигурацию Postgres для Vault**

```
CREATE TABLE vault_kv_store (
  parent_path TEXT COLLATE "C" NOT NULL,
  path        TEXT COLLATE "C",
  key         TEXT COLLATE "C",
  value       BYTEA,
  CONSTRAINT pkey PRIMARY KEY (path, key)
);

CREATE INDEX parent_path_idx ON vault_kv_store (parent_path);

CREATE TABLE vault_ha_locks (
  ha_key                                      TEXT COLLATE "C" NOT NULL,
  ha_identity                                 TEXT COLLATE "C" NOT NULL,
  ha_value                                    TEXT COLLATE "C",
  valid_until                                 TIMESTAMP WITH TIME ZONE NOT NULL,
  CONSTRAINT ha_key PRIMARY KEY (ha_key)
);
```

**Выключить контейнеры** 

```
docker compose down
```

**Перейти в директорию с конфигурацией  и создать файл конфига на сервере** 

```
cd /vault/config
vim vault.hcl
```

**Вставить и изменить конфиг при необходимости**

```
storage "postgresql" {

  connection_url = "postgresql://vault:123qwe!E@vault-postgres:5432/vault?sslmode=disable"

}

  

listener "tcp" {

  address     = "0.0.0.0:8080"

  tls_disable = true

}

  

ui = true

disable_mlock = true

log_level = "info"
```

**запустить контейнеры** 

```
docker-compose up -d
```

## Инициализация/разблокировка/вход Vault 
### Инициализация делается один раз при первом запуске 

```
docker exec -it vault sh
vault operator init
```

**в выводе будет указаны 5 ключей для разблокировки хранилища vault и root-token для доступа к CLI/UI (СОХРАНИТЕ ВСЕ КЛЮЧИ!!!)**

```
Unseal Key 1: 9f3c8d04f3c6bc74d7a5d5d14e84d4cbec0a3b214bba7ec66cddcb3b7a30d82d
Unseal Key 2: 0d2b9861a8ed5fd122cdac2a32706a2bb449b4b09a878f6e183bc4fdf2d5415b
Unseal Key 3: 4034be6b87c0a12f43aa6db8a5d1fdd2ab712c44b91c21e32a8a420dbdf30743
Unseal Key 4: 67eb1c1e61fbb1c8da1b97cb059f6f4ab4d9e2ff1ec90f53ad18c4152c5c497f
Unseal Key 5: 524f14c372f94db69be527705cd5fc9f71df8d88cddfef2e1577c3a2dd78e0f2

Initial Root Token: s.xxxxxxxxxxxxxxxxxxxxxxxx
```

**Чтобы разблокировать хранилище нужно ввести 3 из 5 ключей (при стандартной настройке)**

```
vault operator unseal <Unseal Key 1>
vault operator unseal <Unseal Key 2>
vault operator unseal <Unseal Key 3>
```

**Чтобы использовать CLI нужно залогиниться** 

```
vault login <Initial Root Token>
```

## Создание и получение секретов

**создать движок для секрета значение=ключ**

```
vault secrets enable -path={путь} kv-v2
```

**добавить секрет**

```
vault kv put {путь} {ключ}={"значение"} {ключ}={"значение"}
```

**Получить секрет** 

```
vault kv get {путь}
```

**Удалить секрет**

```
vault kv delete {путь}
```

## Политики 
### описывают, кто и что может делать

**Создать политику**

```
vault policy write {название политики} -<<EOF
path "{Путь движка секретов}" {
  capabilities = ["create", "update", "read"]
}
EOF
```

**Проверить политику**

```
vault policy read {название политики}
```

**Удалить политику**

```
vault policy delete {название политики}
```

## Создание способов аутентификации

### Метод `userpass` (для работы человека в основном используется)

**Включение**

```
vault auth enable userpass
```

**Создание юзера**

```
vault write auth/userpass/users/{название юзера} \
  password={"Пароль"} \
  policies={"Название политики"}
```

**Вход под пользователем**

```
vault login -method=userpass username={юзер} password={пароль}
```

### Метод `approle` (часто используется для автоматизации)

**Включение**

```
vault auth enable approle
```

**Создание роли**

```
vault write auth/approle/role/{название роли} \
token_policies={"Название политики"} \
token_ttl=1h \
token_max_ttl=4h
```

**Получение  role-id и  secret-id**

```
vault read auth/approle/role/{название роли}/role-id
vault write -f auth/approle/role/{название роли}/secret-id
```

## Получение в ci/cd секретов и передача скриптам

### template ci/cd для получения

```
stages:

- fetch-secrets

- deploy

  

variables:

VAULT_ADDR: "http://test-vault.ysts.ru:8080" # URL Vault-сервера

  

fetch_secrets:

stage: fetch-secrets

script:

- |

set -e # Завершить выполнение при любой ошибке

  

LOGIN_RESPONSE=$(curl -s --request POST --data "{\"role_id\":\"$VAULT_ROLE_ID\", \"secret_id\":\"$VAULT_SECRET_ID\"}" \

"$VAULT_ADDR/v1/auth/approle/login") # Аутентификация по AppRole

  

HTTP_STATUS=$? # Сохраняем код завершения команды curl

  

VAULT_TOKEN=$(echo "$LOGIN_RESPONSE" | jq -r ".auth.client_token") # Извлечение токена из ответа

  

CREDS=$(curl -s -w "\nHTTP_STATUS: %{http_code}" --header "X-Vault-Token: $VAULT_TOKEN" \

"$VAULT_ADDR/v1/gitlab/data/pip_proxy_creds") # Получение секрета с кодом ответа

  

CREDS_BODY=$(echo "$CREDS" | head -n -1) # Извлечение тела ответа

CREDS_CODE=$(echo "$CREDS" | tail -n1 | awk -F': ' '{print $2}') # Извлечение HTTP-кода

  

USERNAME=$(echo "$CREDS_BODY" | jq -r ".data.data.username") # Извлечение поля username

PASSWORD=$(echo "$CREDS_BODY" | jq -r ".data.data.password") # Извлечение поля password
  

echo "USERNAME=$USERNAME" > vault.env # Сохраняем username в файл

echo "PASSWORD=$PASSWORD" >> vault.env # Сохраняем password в файл

  

artifacts:

reports:

dotenv: vault.env # Экспортируем переменные в следующие этапы
```

GitLab делает следующее: Выполняет твою джобу и создаёт файл `vault.env`.извлекает переменные из этого файла и автоматически добавляет их в окружение всех последующих джоб этого пайплайна. Этот файл становится артефактом текущей джобы и хранится временно (по умолчанию срок хранения 30 дней), но он не попадает в репозиторий.

### Пример на питоне

```
import os

username = os.getenv('USERNAME')
password = os.getenv('PASSWORD')

if not all([username, password]):
    raise ValueError("Одна или несколько переменных окружения не заданы!")

print(f"USERNAME: {username}")
print(f"PASSWORD: {password}")
```


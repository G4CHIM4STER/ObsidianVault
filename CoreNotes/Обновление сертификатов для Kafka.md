___
Теги: #knowledge #task 
Дата создания: 2025-10-16 10:19 
Последнее изменение: четверг 16-го октября 2025 10:19:06
<< [[2025-10-15]] | [[2025-10-17]] >> 
___
## Основное

1. Конвертируем `.jks` в `.p12`
```sh
keytool -importkeystore \
  -srckeystore mykeystore.jks -srcstoretype JKS \
  -destkeystore ntech.p12 -deststoretype PKCS12 \
  -srcalias ntech
```

2. Извлекаем ключ и сертификат
```sh
openssl pkcs12 -in ntech.p12 -nocerts -nodes -out ntech-key.pem
openssl pkcs12 -in ntech.p12 -clcerts -nokeys -out ntech-cert.pem
openssl pkcs12 -in ntech.p12 -cacerts -nokeys -out rootCA.pem
```

3. asd
```sh

```
___
### Zero-Links
- [[00 Работа]]
- [[00 NTechLab]]

### Links
- 

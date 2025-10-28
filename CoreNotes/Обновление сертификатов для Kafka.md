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
  -srckeystore ntech.jks -srcstoretype JKS \
  -destkeystore ntech.p12 -deststoretype PKCS12 \
  -srcalias ntech
```

2. Извлекаем ключ и сертификат
```sh
openssl pkcs12 -in ntech.p12 -nocerts -nodes -out ntech-key.pem
openssl pkcs12 -in ntech.p12 -clcerts -nokeys -out ntech-cert.pem
openssl pkcs12 -in ntech.p12 -cacerts -nokeys -out rootCA.pem
```

3. Проверка
```sh
openssl rsa -check -noout -in ntech-key.pem
openssl verify -verbose -CAfile rootCA.pem ntech-cert.pem
openssl rsa -modulus -noout -in ntech-key.pem | openssl md5
openssl x509 -modulus -noout -in ntech-cert.pem | openssl md5
```

4. Далее сертификаты нужны вставить в нужный `vault` нужного окружения, предварительно прочитав ключи и сертификаты в формате пригодном для `yml`
```sh
echo "  vault_kafka_cert_ca: |"
awk '{print "      " $0}' rootCA.pem

echo "  vault_kafka_cert_client: |"
awk '{print "      " $0}' ntech-cert.pem

echo "  vault_kafka_key_client: |"
awk '{print "      " $0}' ntech-key.pem
```

___
### Zero-Links
- [[00 Работа]]
- [[00 NTechLab]]

### Links
- 

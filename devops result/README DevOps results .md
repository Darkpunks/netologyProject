__________________________________________________________________________
Курсовая работа по итогам модуля "DevOps и системное администрирование"
__________________________________________________________________________
Результат
Результатом курсовой работы должны быть снимки экрана или текст:

1. Процесс установки и настройки ufw

ufw в ubuntu установлен по умолчанию
```
ivan@ubuntu-focal:~$ sudo ufw allow 22
Skipping adding existing rule
Skipping adding existing rule (v6)

ivan@ubuntu-focal:~$ sudo ufw allow 443
Rule added
Rule added (v6)


ivan@ubuntu-focal:~$ sudo ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
```
2. Процесс установки и выпуска сертификата с помощью hashicorp vault
```
ivan@ubuntu-focal:~$ sudo apt install gpg
Reading package lists... Done
Building dependency tree
Reading state information... Done
gpg is already the newest version (2.2.19-3ubuntu2.2).
gpg set to manually installed.
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
```
Устанавливаем дальше:
```
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg >/dev/null

gpg --no-default-keyring --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --fingerprint
```
<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/devops%20result/2.jpg">


3. Процесс установки и выпуска сертификата с помощью hashicorp vault

Создал СА и выпустил сертификат для своего домена docxz.cf
```
vault secrets enable pki
vault secrets tune -max-lease-ttl=87600h pki

vault write -field=certificate pki/root/generate/internal \
     common_name="docxz.cf" \
     issuer_name="docxz-2022" \
     ttl=87600h > docxz_root_2022_ca.crt

vault write pki/roles/2022-servers allow_any_name=true


vault write pki/config/urls \
     issuing_certificates="$VAULT_ADDR/v1/pki/ca" \
     crl_distribution_points="$VAULT_ADDR/v1/pki/crl"


export ISSUE_DOMAIN="docxz.cf"
export ISSUE_DOMAIN_DIR=$ISSUE_DOMAIN-$(date '+%Y-%m-%d_%H.%m.%S')

mkdir $ISSUE_DOMAIN_DIR

vault write -format=json pki/issue/2022-servers common_name="$ISSUE_DOMAIN" ttl="730h" > $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.json
cat $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.json | jq -r '.data.certificate' > $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.cert.pem
cat $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.json | jq -r '.data.ca_chain' > $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.ca_chain.pem
cat $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.json | jq -r '.data.private_key' > $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.cert.key
```



3. Процесс установки и настройки сервера nginx
```
sudo apt install nginx

sudo systemctl start nginx

sudo systemctl status nginx
```
<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/devops%20result/3.1.jpg">



создаю файлик docxz.cf в available-sites 

```
server {
        listen              443 ssl;
    server_name         docxz.cf;
    ssl_certificate     /etc/nginx/certs/docxz.cf/docxz.cf.cert.pem;
    ssl_certificate_key /etc/nginx/certs/docxz.cf/docxz.cf.cert.key;
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers         HIGH:!aNULL:!MD5;

        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        

        location / {
                try_files $uri $uri/ =404;
        }

}
```
<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/devops%20result/3.2.jpg">

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/devops%20result/3.3.jpg">

4. Страница сервера nginx в браузере хоста не содержит предупреждений


ОТВЕТ: Приложил на скрине.

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/devops%20result/4.1.jpg">




5. Скрипт генерации нового сертификата работает (сертификат сервера ngnix должен быть "зеленым")
```
#/bin/bash
ISSUE_DOMAIN="docxz.cf"
ISSUE_DOMAIN_DIR=/etc/nginx/certs/docxz.cf

vault write -format=json pki/issue/2022-servers common_name="$ISSUE_DOMAIN" ttl="730h" > $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.json
cat $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.json | jq -r '.data.certificate' > $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.cert.pem
cat $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.json | jq -r '.data.ca_chain' > $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.ca_chain.pem
cat $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.json | jq -r '.data.private_key' > $ISSUE_DOMAIN_DIR/$ISSUE_DOMAIN.cert.key

systemctl reload nginx


ivan@ubuntu-focal:/etc/nginx/certs$ sudo nano docxz.cf_renew.sh
ivan@ubuntu-focal:/etc/nginx/certs$ sudo chmod +x docxz.cf_renew.sh
```
<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/devops%20result/4.2.jpg">

6. Crontab работает (выберите число и время так, чтобы показать что crontab запускается и делает что надо)

Отредактировал через sudo crontab -e: 

```
# запуск скрипта 27 числа в 18:19  каждый месяц:
19 18 27 * * /etc/nginx/certs/docxz.cf_renew.sh
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/devops%20result/5.1.jpg">

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/devops%20result/5.2.jpg">

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/devops%20result/5.3.jpg">

# Установка Zabbix 6.0 LTS Ubuntu Server 22.04 на Платформе AWS EC2 и установка SSL
![thb](https://github.com/vprimin/pub/blob/main/Manuals/images/awszabbix.jpg)

*Всем привет. В данной статье рассмотрим как ставить заббикс в облаке на платформе AWS EC2 + Установим сертификаты SSL c помощью сервиса certbot*
 
Перед тем как начать нам понадобится
1. E-mail адрес для регистрации аккантуа на AWS
2. Кредитная карта (желательно виртуальная)
3. Домен второго или третьего уровная типа zabbix.domain.com (Доступ к ДНС серверу/регистратору домена)

Заходим на https://aws.amazon.com/ и регистрируем аккаунт, включаем двухфакторку, бюджет алерты.
Поднимаем виртуальную машину на Ubuntu 22.04 тариф t2.micro. Открываем порты, 22, 80, 443
Присваеваем IP аддрес (Elastic IPs)
Подключаемся по ssh (не забываем про chmod 400)

Обновляемся
```
sudo su
apt update
apt upgrade
reboot
```
Устанавливаем веб сервер Nginx
```
sudo su
apt install nginx
```
Далее сразу установим сертификаты с помощью сервиса certbot
https://certbot.eff.org/

```
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --nginx
```
Сохраняем пути к сертификатам, они нам еще понадобятся.
Далее ставим Субд
```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list

sudo apt update
sudo apt install postgresql-13
systemctl status postgresql 
```
Тут вводим пароль два раза и запоминаем, он нам еще пригодиться, его нужно будет ввести в настройках файла конфигурации заббикса. 
Далее:создаем базу данных и пользователя
```
sudo -u postgres createdb -O zabbix zabbix
sudo -u postgres createuser --pwprompt zabbix
```
Устанавливаем заббикс
```
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu22.04_all.deb
dpkg -i zabbix-release_6.0-4+ubuntu22.04_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-nginx-conf zabbix-sql-scripts zabbix-agent
```
И делаем экспорт из ранее скачанного архива в нашу БД
```
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```
Далее заходим текстовым редактором в файл конфигурации zabbix и находим там значение 'DBPassword=' - убираем # - знак комментария и вводим наш пароль от базы данных. Сохраняем. 
```
nano /etc/zabbix/zabbix_server.conf 
```
Далее настраиваем файл конфигурации серера nginx 
```
nano /etc/zabbix/nginx.conf 
```
В шапке файла конфигурации нам надо изменить значения на те, которые приведены ниже:
```
server {
listen 443 default_server ssl;
ssl_certificate /etc/letsencrypt/live/zabbix.skp.kz/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/zabbix.skp.kz/privkey.pem;
server_name     zabbix.skp.kz;
```
Поросто замените значение 'zabbix.skp.kz' на свой URL.

Далее перезагружаем службу заббикс сервера и добавляем в автозапуск 
```
systemctl restart zabbix-server zabbix-agent nginx php8.1-fpm
ssystemctl enable zabbix-server zabbix-agent nginx php8.1-fpm
```
Если все настроено верно по вашему адресу должен открыться веб-интерфейс Zabbix подписанным SSL сертификатом 
```
https://zabbix.dnsname.url
```

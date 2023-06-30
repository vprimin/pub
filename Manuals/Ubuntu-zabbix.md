# Установка Zabbix 6.0 LTS Ubuntu Server 20.04
![thb](https://github.com/vprimin/pub/blob/main/Manuals/images/thubnail.png)

*PostgeSQL nginx*
 
```
sudo su
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-1+ubuntu20.04_all.deb

dpkg -i zabbix-release_6.0-1+ubuntu20.04_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-nginx-conf zabbix-sql-scripts zabbix-agent
```
Далее установим сервер баз данных, создадим пользователя и таблицу. 

```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list

sudo apt update
sudo apt install postgresql-13
systemctl status postgresql
sudo -u postgres createuser --pwprompt zabbix
```
Тут вводим пароль два раза и запоминаем, он нам еще пригодиться, его нужно будет ввести в настройках файла конфигурации заббикса. Далее:создаем базу данных
```
sudo -u postgres createdb -O zabbix zabbix
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
В самом начале файла убераем знак комментария # и вводим наш адрес локальной сети.

```
listen 80;
server_name 192.168.151.70; 
```
в моем случае это подсеть 151 - вы  вводите ваш айпи адрес который вы настроили для вашего сервера.

Далее перезагружаем службу заббикс сервера и добавляем в автозапуск 

Но перед этим я удалю веб сервер апачи, так как не хочу чтобы он у меня стоял рядом с nginx и конфиликтовал по 80 порту

```
sudo apt remove apache2
```
Далее перезагружаем службу заббикс сервера и добавляем в автозапуск 
```
systemctl restart zabbix-server zabbix-agent nginx php7.4-fpm
systemctl enable zabbix-server zabbix-agent nginx php7.4-fpm
```
Если все настроено верно по адресу  должен открыться  веб-интерфейс Zabbix: 
```
http://server_ip_or_name
```

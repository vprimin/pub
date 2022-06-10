# Установка Zabbix agent на Windows
![thb](https://github.com/vprimin/pub/blob/main/Manuals/thubnail.png)

*Установка агента*
 
```
C:\zabbix\bin\zabbix_agentd.exe –-config  C:\zabbix\conf\zabbix_agentd.conf -–install
```
Далее запустим агент 

```
C:\zabbix\bin\zabbix_agentd.exe --start
```
и визуально убедимся что агент запущен и есть среди служб (services.msc) 

Далее добавим правила в сетевой экран
Из под PowerShell (с правами администратора) выполним команды

```
New-NetFirewallRule -DisplayName 'Zabbix agent_inb' -Profile 'Any' -Direction Inbound -Action Allow -Protocol TCP -LocalPort 10050
New-NetFirewallRule -DisplayName 'Zabbix agent_out' -Profile 'Any' -Direction Outbound -Action Allow -Protocol TCP -LocalPort 10050
```
И тоже самое сделаем на нашем Zabbix сервере под управлением Ubuntu
```
sudo ufw allow from any to any port 10050 proto tcp
```
Далее заходим в веб консоль заббикса и добавляем хост 
Ссылка на видео https://youtu.be/MLAnl_IShfs

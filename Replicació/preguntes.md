# ACTIVITAT 1. CONFIGURACIÓ D’UN SISTEMA DE RÈPLICA (5 punts)
## CONFIGURACIÓ MASTER
• Verifica que el paràmetre server-id té un valor numèric (per defecte és 1).
```
Per mirar quin parametre tenim fare nano /var/lib/mysql/auto.cnf
```
![image](https://user-images.githubusercontent.com/100956247/170578968-41ee3a24-c6e6-4ab3-8c5e-714e1c6285d9.png)

• Fes un FLUSH DELS LOGS utilitzant la comanda FLUSH LOGS dins del MySQL
o mysql> FLUSH LOGS;
![image](https://user-images.githubusercontent.com/100956247/170579132-51e30523-bd8f-44d8-9dbf-7819fee1b1bb.png)
• Realitza una comprovació dels logs com a master mitjançant SHOW MASTER LOGS
o mysql> SHOW MASTER LOGS
![image](https://user-images.githubusercontent.com/100956247/170579232-12f5cac4-b682-4faa-ba04-1497f99d1bba.png)

## CONFIGURACIÓ SLAVE i MASTER
• Realitza una còpia de la màquina virtual a on tinguis SGBD MySQL. Aquesta nova màquina serà
que farà d'eslau.
```
Despres de fer la copia tanco el servei mysql i esborro el auto.cnf perque quan engeguem fem la copia no tingui la mateixa server_id que el master.
```
![image](https://user-images.githubusercontent.com/100956247/170586003-5b564233-db74-48a3-bd45-957c04668655.png)

```
Comprovem que el serverid no sigui el mateix:
```
Slave
![image](https://user-images.githubusercontent.com/100956247/170585927-2afc5aa7-fb91-4958-a3e2-6590163b74d9.png)
Master
![image](https://user-images.githubusercontent.com/100956247/170586590-6cf5cb8d-1fdb-4e5b-ba0d-330c96335e9b.png)


• Esbrina quina IP tenen cadascuna de les màquines (master, slave).
```
Master IP: 192.168.56.103
Slave IP: 192.168.56.104
```
• Crea un backup de la BD a la màquina master utilitzant:
o $> mysqldump –-user=root –-password=vostrepwd -–master-data=2
sakila > /tmp/master_backup.sql
```

```
• Edita el fitxer master_backup.sql i busca la línia que comenci per --CHANGE MASTER TO.... i
busca els valors MASTER_LOG_FILE i MASTER_LOG_POS.
```

```
### SLAVE
o Para el servei de MySQL.
```

```
o Modifica el fitxer de configuració /etc/my.conf
▪ Comenta els paràmetres log-bin i binlog_format. D'aquesta manera
desactivarem el sistema de log-bin.
```

```
▪ Assigna un valor al paràmetre server-id (diferent que el del Master)
```

```
▪ Torna engegar el servei MySQL.
```

```
### MASTER
o Afegeix l'usuari slave amb la IP de la màquina slave
▪ mysql> CREATE USER 'slave'@'IP-SERVIDOR-SLAVE'
-> IDENTIFIED BY 'patata';
```

```
o Afegix el permís de REPLICATION SLAVE a l'usuari que acabes de crear.
▪ mysql> GRANT REPLICATION SLAVE ON *.*
-> TO 'slave'@'IP-SERVIDOR-SLAVE';
▪ mysql> FLUSH PRIVILEGES;
```

```
• A la màquina SLAVE executa la següent comanda ajudant-te de les dades del pas 3 i 4:
o mysql> CHANGE MASTER TO
-> MASTER_HOST = '<ip-servidor-master>',
-> MASTER_USER = 'slave',
-> MASTER_PASSWORD = 'patata',
-> MASTER_PORT = '3306',
-> MASTER_LOG_FILE = '<valor trobat en el pas 4>',
-> MASTER_LOG_POS = <valor trobat en el pas 4>,
-> MASTER_CONNECT_RETRY = 10;
 ```
 
 ```
# ACTIVITAT 2 – REPLICACIÓ SEMISÍNCRONA AMB MASTER PASSIU (3 punts)
 ```
 
 ```
# ACTIVITAT 3 – ENTORNS AMB MÚLTIPLES ORÍGENS (2 punts)
 ```
 
 ```
# ACTIVITAT 4 – TOPOLOGIA DE SLAVE RELAY VIA BLACKHOLE (2 punts)
```
 
````
# ACTIVITAT 5 – EINES PERCONA TOOLKIT
1. Instal·la i explica com funciona alguna d’aquestes eines (Percona Toolkit) ( 2 punts, 1 punt per eina).
Per exemple:
 ◦ pt-table-checksum: https://www.percona.com/doc/percona-toolkit/2.1/pt-table-checksum.html
```
 
```
 ◦ pt-table-sync: https://www.percona.com/doc/percona-toolkit/2.1/pt-table-sync.html
```
 
```
 # ACTIVITAT 6 – BACKUP (2 punts)
Mitjançant l’eina XtraBackup de Percona realitza una còpia completa de la BD i una restauració
```
 
```

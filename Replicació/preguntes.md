# ACTIVITAT 1. CONFIGURACIÓ D’UN SISTEMA DE RÈPLICA (5 punts)
## CONFIGURACIÓ MASTER
• Verifica que el paràmetre server-id té un valor numèric (per defecte és 1).
```
Anem al my.cnf i afegim una linia que posi server-uuid=1
```

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

Master IP: 192.168.56.103
![image](https://user-images.githubusercontent.com/100956247/170613231-2d7870e2-343a-440d-8814-f72063b5f2ba.png)

Slave IP: 192.168.56.104
![image](https://user-images.githubusercontent.com/100956247/170613193-95296a68-a9d4-436f-890d-2d6ae761808b.png)


• Crea un backup de la BD a la màquina master utilitzant:
o $> mysqldump –-user=root –-password=vostrepwd -–master-data=2
sakila > /tmp/master_backup.sql
![image](https://user-images.githubusercontent.com/100956247/170603584-41daf3de-6083-4419-9eae-bebce9c7fe14.png)

• Edita el fitxer master_backup.sql i busca la línia que comenci per --CHANGE MASTER TO.... i
busca els valors MASTER_LOG_FILE i MASTER_LOG_POS.

![image](https://user-images.githubusercontent.com/100956247/170616846-d80512de-3160-4868-a295-408d58520d59.png)

### SLAVE
o Para el servei de MySQL.
![image](https://user-images.githubusercontent.com/100956247/170605564-b2d58c18-5592-41c9-8556-255123b7e855.png)

o Modifica el fitxer de configuració /etc/my.conf
▪ Comenta els paràmetres log-bin i binlog_format. D'aquesta manera
desactivarem el sistema de log-bin.
```
Per desactivar-ho anem al my.cnf i descomentem la linia que posa #disable_log_bin
```
▪ Torna engegar el servei MySQL.
![image](https://user-images.githubusercontent.com/100956247/170606143-3b33b12d-ab02-4dbb-860c-8a43e5cd54aa.png)

### MASTER
o Afegeix l'usuari slave amb la IP de la màquina slave
▪ mysql> CREATE USER 'slave'@'IP-SERVIDOR-SLAVE'
-> IDENTIFIED BY 'patata';
![image](https://user-images.githubusercontent.com/100956247/170606594-232c4838-7d6a-4386-adb3-2eb89d35e0b8.png)

o Afegix el permís de REPLICATION SLAVE a l'usuari que acabes de crear.
▪ mysql> GRANT REPLICATION SLAVE ON *.*
-> TO 'slave'@'IP-SERVIDOR-SLAVE';
▪ mysql> FLUSH PRIVILEGES;
![image](https://user-images.githubusercontent.com/100956247/170606785-71e120d1-ecae-489f-bac2-ba865f12e003.png)

• A la màquina SLAVE executa la següent comanda ajudant-te de les dades del pas 3 i 4:
o mysql> CHANGE MASTER TO
-> MASTER_HOST = '<ip-servidor-master>',
-> MASTER_USER = 'slave',
-> MASTER_PASSWORD = 'patata',
-> MASTER_PORT = '3306',
-> MASTER_LOG_FILE = '<valor trobat en el pas 4>',
-> MASTER_LOG_POS = <valor trobat en el pas 4>,
-> MASTER_CONNECT_RETRY = 10;
![image](https://user-images.githubusercontent.com/100956247/170608360-ba5b9074-89dc-4a5e-ab34-0f5a6ff97bf0.png)

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
pt-table-checksum realitza una comprovació de coherència de la rèplica en línia executant consultes de checksum al master, que produeix resultats diferents a les rèpliques que no són coherents amb el master. El DSN opcional especifica l'amfitrió principal. L'estat de sortida de l'eina és diferent de zero si es troben diferències o si es produeixen advertències o errors.
```
 ![image](https://user-images.githubusercontent.com/100956247/170736371-49537094-9fe6-445f-a3b9-4d9a95c8409e.png)

 ◦ pt-table-sync: https://www.percona.com/doc/percona-toolkit/2.1/pt-table-sync.html
```
Aquesta eina canvia les dades, de manera que quan es sincronitza un servidor que és un slave de rèplica amb els mètodes –replicate o –sync-to-master, sempre fa els canvis al master de rèplica, mai directament a l'slave de rèplica. Aquesta és, en general, l'única manera segura de tornar a sincronitzar una rèplica amb el seu master; els canvis a la rèplica solen ser la font dels problemes en primer lloc. Tanmateix, els canvis que fa al master haurien de ser canvis sense operacions que estableixin les dades als seus valors actuals i, de fet, només afectin la rèplica.
```
 ![image](https://user-images.githubusercontent.com/100956247/170736525-a801fd53-ab9e-44cb-ba1e-11f0ee728a82.png)

 # ACTIVITAT 6 – BACKUP (2 punts)
Mitjançant l’eina XtraBackup de Percona realitza una còpia completa de la BD i una restauració
```
 
```

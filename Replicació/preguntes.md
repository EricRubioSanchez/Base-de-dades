# ACTIVITAT 1. CONFIGURACIÓ D’UN SISTEMA DE RÈPLICA (5 punts)
CONFIGURACIÓ MASTER
• Verifica que el paràmetre server-id té un valor numèric (per defecte és 1).
• Fes un FLUSH DELS LOGS utilitzant la comanda FLUSH LOGS dins del MySQL
o mysql> FLUSH LOGS;
• Realitza una comprovació dels logs com a master mitjançant SHOW MASTER LOGS
o mysql> SHOW MASTER LOGS
CONFIGURACIÓ SLAVE i MASTER
• Realitza una còpia de la màquina virtual a on tinguis SGBD MySQL. Aquesta nova màquina serà
que farà d'eslau.
• Esbrina quina IP tenen cadascuna de les màquines (master, slave).
• Crea un backup de la BD a la màquina master utilitzant:
o $> mysqldump –-user=root –-password=vostrepwd -–master-data=2
sakila > /tmp/master_backup.sql
• Edita el fitxer master_backup.sql i busca la línia que comenci per --CHANGE MASTER TO.... i
busca els valors MASTER_LOG_FILE i MASTER_LOG_POS.
• SLAVE
o Para el servei de MySQL.
o Modifica el fitxer de configuració /etc/my.conf
▪ Comenta els paràmetres log-bin i binlog_format. D'aquesta manera
desactivarem el sistema de log-bin.
▪ Assigna un valor al paràmetre server-id (diferent que el del Master)
▪ Torna engegar el servei MySQL.
• MASTER
o Afegeix l'usuari slave amb la IP de la màquina slave
▪ mysql> CREATE USER 'slave'@'IP-SERVIDOR-SLAVE'
-> IDENTIFIED BY 'patata';
o Afegix el permís de REPLICATION SLAVE a l'usuari que acabes de crear.
▪ mysql> GRANT REPLICATION SLAVE ON *.*
-> TO 'slave'@'IP-SERVIDOR-SLAVE';
▪ mysql> FLUSH PRIVILEGES;
• A la màquina SLAVE executa la següent comanda ajudant-te de les dades del pas 3 i 4:
o mysql> CHANGE MASTER TO
-> MASTER_HOST = '<ip-servidor-master>',
-> MASTER_USER = 'slave',
-> MASTER_PASSWORD = 'patata',
-> MASTER_PORT = '3306',
-> MASTER_LOG_FILE = '<valor trobat en el pas 4>',
-> MASTER_LOG_POS = <valor trobat en el pas 4>,
-> MASTER_CONNECT_RETRY = 10;
# ACTIVITAT 2 – REPLICACIÓ SEMISÍNCRONA AMB MASTER PASSIU (3 punts)
# ACTIVITAT 3 – ENTORNS AMB MÚLTIPLES ORÍGENS (2 punts)
# ACTIVITAT 4 – TOPOLOGIA DE SLAVE RELAY VIA BLACKHOLE (2 punts)
# ACTIVITAT 5 – EINES PERCONA TOOLKIT
1. Instal·la i explica com funciona alguna d’aquestes eines (Percona Toolkit) ( 2 punts, 1 punt per eina).
Per exemple:
 ◦ pt-table-checksum: https://www.percona.com/doc/percona-toolkit/2.1/pt-table-checksum.html
 ◦ pt-table-sync: https://www.percona.com/doc/percona-toolkit/2.1/pt-table-sync.html
# ACTIVITAT 6 – BACKUP (2 punts)
Mitjançant l’eina XtraBackup de Percona realitza una còpia completa de la BD i una restauració

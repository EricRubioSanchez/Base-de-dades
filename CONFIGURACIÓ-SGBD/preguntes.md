1. Quins són els logs activats per defecte? Crea un fitxer de configuració anomenat 
logs.cnf a on:
Activa els logs que no ho estiguin per defecte i indica les configuracions necessàries 
per activar-los. Indica les rutes dels fitxer de log de Binary, Slow Query i General. Quins 
paràmetres has creat/modificat?
```
Els logs activats per defecte es poden veure amb la comanda:
 nano /etc/my.cnf
En el meu cas esta activat per defecte el log bin i log error.

En el meu cas abans de crear el fitxer vull crear el directori percona amb la comanda:
        mkdir /etc/percona
I per crear el fitxer utilitzem aquesta comanda:
        nano /etc/percona/logs.cnf

Per activar els logs que estiguin desabilitats per defecte tindrem que utilitzar per començar aquetsa comanda:
      sudo chown mysql:mysql logs.cnf
Despres aquesta no posarem res dintre del fitxer:
       nano /etc/my.cnf
Entrem dintre de la ruta:
       cd /var/lib/mysql/mysql
I utilitzem aquesta comanda en la cual abaix del tot posarem aixo !include /etc/percona/logs.cnf :
        sudo nano general_log.log
Despres aquesta que no posem res dintre
        sudo nano show_query_log.log
Per confirmar aquestes dos:
        sudo chown mysql:mysql general_log.log
        sudo chown mysql:mysql show_query_log.log
Tornem al path /etc/percona
Editem el fitxer logs.cnf amb nano /etc/percona/logs.cnf y posem dintre:

       [mysqld]

       general_log = 1
       log_output = FILE
       general_log_file = /var/lib/mysql/mysql/general_log.log

       slow_query_log = 1
       slow_query_log_file = /var/lib/mysql/mysql/slow_query_log.log
       long_query_time = 2

Els parametres que he modificat son:
Activem el log general i li canviem el path.
Activem el slow query log, li canviem el path i posem que conti com slow query si triga 2 segonds.

Les rutes del fitxers:
Binary:
    /var/lib/mysql/binlog.index 
Slow Query:
    /var/lib/mysql/mysql/slow_query_log.log  
General:
    /var/lib/mysql/mysql/general_log.log 
```
2. Comprova l'estat de les opcions de log que has utilitzat mitjançant una sessió de mysql 
client. 
Exemple: (mysql> SHOW GLOBAL VARIABLES LIKE '%log')
```
 show variables like '%log';
+----------------------------------+---------------------+
| Variable_name                    | Value               |
+----------------------------------+---------------------+
| back_log                         | 151                 |
| general_log                      | ON                  |
| innodb_api_enable_binlog         | OFF                 |
| innodb_scrub_log                 | OFF                 |
| log_statements_unsafe_for_binlog | ON                  |
| relay_log                        | localhost-relay-bin |
| slow_query_log                   | ON                  |
| sync_binlog                      | 1                   |
| sync_relay_log                   | 10000               |
+----------------------------------+---------------------+
```
3. Modifica el fitxer de configuració i desactiva els logs de binary, slow query i general. Nota: 
Simplament desactiva'ls no borris altres paràmetres com la ruta dels fitxers, etc...
```
Per desabilitar el slow query i el general:
Tornem al path /etc/percona
Editem el fitxer logs.cnf amb nano /etc/percona/logs.cnf y posem dintre:

       [mysqld]

       general_log = 2
       log_output = FILE
       general_log_file = /var/lib/mysql/mysql/general_log.log

       slow_query_log = 2
       slow_query_log_file = /var/lib/mysql/mysql/slow_query_log.log
       long_query_time = 2
       
Per desabilitar el log binary:
utilitzem el nano /etc/my.cnf i ens surtira un output similar a aquest:
# Percona Server template configuration
#
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/8.0/en/server-configuration-defaults.html

[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M
#
# Remove the leading "# " to disable binary logging
# Binary logging captures changes between backups and is enabled by
# default. It's default setting is log_bin=binlog
# disable_log_bin
#
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.

llavors per desabilitar el log binary anem a la linia on posa # disable_log_bin i eliminen #, guardem el document i ja s'ha desabilitat. 

```
4. Activa els logs en temps d'execució mitjançant la sentència SET GLOBAL. També canvia 
el destí de log general a una taula (paràmetre log_output). Quines són les sentències que 
has utilitzat? A quina taula es guarden els dels del general log?
```
Per començar utilitzarem la base de dades mysql amb la comanda:
      use mysql;
Per activar els logs en temps d'execució amb la sentència SET GLOBAL farem:
      SET GLOBAL general_log = 'ON';
Per canviar el destí de log general a una taula utilitzarem la sentència:
      SET GLOBAL log_output = 'table';
Es guarda a la taula general_log.

```
5. Carrega la BD Sakila localitzada a la web de 
o Descarrega't el fitxer sakila-schema.sql del Moodle.
o carrega la BD dins del MySQL utilitzant la sentència:
mysql> SOURCE <ruta_fitxer>/sakila-schema.sql;
```
Anem a aquest enllaç per descarrgarla:
       https://dev.mysql.com/doc/index-other.html
       
La instalem i la pasem a la maquina virtual en el meu cas ho he fet amb la aplicacio WinSCP
       https://winscp.net/eng/download.php
       
I per transformarla a mysql utilitzem aquesta sentència:       
       mysql> SOURCE /var/lib/mysql/mysql/sakila-schema.sql
       
L'Output tindria que ser similar a aquest:
Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected, 1 warning (0,00 sec)

...

```
6. Compte el numero de sentències CREATE TABLE dins del general log mitjançant una sentència SQL.
o Mostra quina sentència has utilitzat i mostra'n el resultat.
```

```
7. Executa una query mitjançant la funció SLEEP(11) per tal de que es guardi en el log de 
Slow Query Log. Mostra el contingut del log demostrant-ho
```
Sentencia:
       Select(Sleep(11));
Output:
       +-------------+
       | (Sleep(11)) |
       +-------------+
       |           0 |
       +-------------+
       1 row in set (11,00 sec)

Reiniciem la maquina mysql amb la comanda service mysql restart
i despres llegim les logs del slow_query amb aquesta comanda:
            cat /var/lib/mysql/mysql/slow_query_log.log
I ens surtira un output similar a aquest:

/usr/sbin/mysqld, Version: 8.0.27-18 (Percona Server (GPL), Release 18, Revision 24801e21b45), Time: 2022-03-31T15:44:19.737747Z. started with:
Tcp port: 3306  Unix socket: /var/lib/mysql/mysql.sock
Time                 Id Command    Argument
# Time: 2022-03-31T15:45:29.799236Z
# User@Host: root[root] @ localhost []  Id:     8
# Schema:   Last_errno: 0  Killed: 0
# Query_time: 11.001218  Lock_time: 0.000000  Rows_sent: 1  Rows_examined: 1  Rows_affected: 0  Bytes_sent: 59
SET timestamp=1648741518;
Select(Sleep(11));
/usr/sbin/mysqld, Version: 8.0.27-18 (Percona Server (GPL), Release 18, Revision 24801e21b45), Time: 2022-03-31T15:45:49.681462Z. started with:
Tcp port: 3306  Unix socket: /var/lib/mysql/mysql.sock
Time                 Id Command    Argument

Dintre del output es pot apreciar la nostra sentencia sleep i el temps que ha trigat en ferla.
# Query_time: 11.001218  Lock_time: 0.000000  Rows_sent: 1  Rows_examined: 1  Rows_affected: 0  Bytes_sent: 59
SET timestamp=1648741518;
Select(Sleep(11));
```
8. Assegura't que el Binary Log estigui activat i borra tots els logs anteriors mitjançant la 
sentència RESET MASTER.
 Crea i esborra una base de dades anomenada foo. Utilitza la sentències:
mysql> CREATE DATABASE foo;
mysql> DROP DATABASE foo;
 Mitjançant la sentència SHOW BINLOG EVENTS llista els events i comprova les sentències anteriors en quin fitxer de log estan.
 Realitza un rotate log mitjançant la sentència FLUSH LOGS. Què realitza exactament 
aquesta sentència?
 Crea i esborra una altra base de dades com l'exemple anteior del foo. Però en aquest 
cas anomena la base de dades bar
 Llista tots els fitxers de log i els últims canvis mitjançant la sentència SHOW. Quina 
sentència has utilitzat? Mostra'n el resultat.
 Esborra el primer binary log. Quina sentència has utilitzat?
 Utilitza el programa mysqlbinlog per mostrar el fitxer mysql-bin.000002 
o Quin és el seu contingut?
o Quin número d'event ha estat el de la creació de la base de dades bar?
```
M'aseguro que el Binary log estigui activat amb la comanda:
SHOW VARIABLES LIKE 'log_bin';
Output:
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| log_bin       | ON    |
+---------------+-------+
1 row in set (0,01 sec)

Borrem els logs amb la comanda 
RESET MASTER;
Output:
Query OK, 0 rows affected (0,00 sec)

Creem la base de dades foo:
CREATE DATABASE foo;
Output:
Query OK, 1 row affected (0,00 sec)
L'eliminem:
DROP DATABASE foo;
Output:
Query OK, 1 row affected (0,00 sec)

Utilitzem la sentència SHOW BINLOG EVENTS:
Output:
+---------------+-----+----------------+-----------+-------------+--------------------------------------+
| Log_name      | Pos | Event_type     | Server_id | End_log_pos | Info |
+---------------+-----+----------------+-----------+-------------+--------------------------------------+
| binlog.000001 |   4 | Format_desc    |         1 |         125 | Server ver: 8.0.27-18, Binlog ver: 4 |
| binlog.000001 | 125 | Previous_gtids |         1 |         156 |                                      |
| binlog.000001 | 156 | Anonymous_Gtid |         1 |         233 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS' |
| binlog.000001 | 233 | Query          |         1 |         338 | CREATE DATABASE foo /* xid=5 */      |
| binlog.000001 | 338 | Anonymous_Gtid |         1 |         415 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS' |
| binlog.000001 | 415 | Query          |         1 |         516 | DROP DATABASE foo /* xid=7 */        |
| binlog.000001 | 516 | Stop           |         1 |         539 |                                      |
+---------------+-----+----------------+-----------+-------------+--------------------------------------+
7 rows in set (0,00 sec)

Per buscar en quin fitxer log estan ulitizo la sentència:
SHOW VARIABLES LIKE 'log_bin_index';
Output:
+------------------------------------------------+-----------------------------+
| Variable_name                                  | Value                       |
+------------------------------------------------+-----------------------------+
| log_bin_index                                  | /var/lib/mysql/binlog.index |
+------------------------------------------------+-----------------------------+
1 rows in set (0,00 sec)

Utilitzem la sentència:
FLUSH LOGS;
Output:
Query OK, 0 rows affected (0,01 sec)

La sentència dels FLUSH LOGS serveix per limpiar els logs de dintre dels arxius de Binary logs, logs error i general log.

Creem la base de dades bar:
CREATE DATABASE bar;
Output:
Query OK, 1 row affected (0,00 sec)

L'eliminem:
DROP DATABASE bar;
Output:
Query OK, 0 rows affected (0,01 sec)

He utilitzat la sentencia:
SHOW BINARY LOGS;
Output:
+---------------+-----------+-----------+
| Log_name      | File_size | Encrypted |
+---------------+-----------+-----------+
| binlog.000001 |       539 | No        |
| binlog.000002 |       200 | No        |
| binlog.000003 |       516 | No        |
+---------------+-----------+-----------+
3 rows in set (0,00 sec)

Per esborra la primera utilitzo:
PURGE BINARY LOGS TO 'binlog.000002';
Output:
Query OK, 0 rows affected (0,00 sec)
```
9. De quina manera podem desactivar el binary log només d’una sessió en concret. Imagina’t 
que ets un administrador de la BD i no vols que les instruccions que facis es gravin en el 
binary_log.
```

```

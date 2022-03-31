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

```
6. Compte el numero de sentències CREATE TABLE dins del general log mitjançant una sentència SQL.
o Mostra quina sentència has utilitzat i mostra'n el resultat.
```

```
7. Executa una query mitjançant la funció SLEEP(11) per tal de que es guardi en el log de 
Slow Query Log. Mostra el contingut del log demostrant-ho
```

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

```
9. De quina manera podem desactivar el binary log només d’una sessió en concret. Imagina’t 
que ets un administrador de la BD i no vols que les instruccions que facis es gravin en el 
binary_log.
```

```

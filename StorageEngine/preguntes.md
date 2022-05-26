# ACTIVITAT 1. REALITZA I/O RESPON ELS SEGÜENTS APARTATS (obligatòria) (1 punts)
1. Indica quins són els motors d’emmagatzematge que pots utilitzar (quins estan actius)? Mostra al comanda utilitzada i el resultat d’aquesta.
```
He utilitzat la comanda:
 SHOW ENGINES;
Output:
```
![image](https://user-images.githubusercontent.com/100956247/162487616-6ce10e24-6772-479a-8493-5c48622f01dc.png)

2. Com puc saber quin és el motor d’emmagatzematge per defecte. Mostra com canviar aquest 
paràmetre de tal manera que les noves taules que creem a la BD per defecte utilitzin el motor 
MyISAM?
```
Entrem en el my.cnf amb la comanda:
     nano /etc/my.cnf
I dintre escrivim aquesta linia.
     default-storage-engine = MyISAM
```
![image](https://user-images.githubusercontent.com/100956247/164481288-d0510f89-c7fc-41b0-8da6-c0ea9577b868.png)

3. Com podem saber quin és el motor d'emmagatzematge per defecte?
```
Dintre del mysql posem la sentencia:
SHOW ENGINES;
I ens retornara una taula on posara quina esta per Default.
```
4. Explica els passos per instal·lar i activar l'ENGINE MyRocks. MyRocks és un motor d'emmagatzematge per MySQL basat en RocksDB (SGBD incrustat de tipus clau-valor). Aquest tipus d’emmagatzematge està optimitzat per ser molt eficient en les escriptures amb lectures 
acceptables.
```
Instalem el MyRocks amb la comanda:
       yum install percona-server-rocksdb
Output:
Actualización de repositorios de Subscription Management.
Última comprobación de caducidad de metadatos hecha hace 0:07:17, el jue 21 abr 2022 16:56:43 CEST.
Dependencias resueltas.
=============================================================================================================================================================================================================================================
 Paquete                                                        Arquitectura                                   Versión                                                    Repositorio                                                   Tam.
=============================================================================================================================================================================================================================================
Instalando:
 percona-server-rocksdb                                         x86_64                                         8.0.27-18.1.el8                                            ps-80-release-x86_64                                          14 M

Resumen de la transacción
=============================================================================================================================================================================================================================================
Instalar  1 Paquete

Tamaño total de la descarga: 14 M
Tamaño instalado: 55 M
¿Está de acuerdo [s/N]?: s
Descargando paquetes:
percona-server-rocksdb-8.0.27-18.1.el8.x86_64.rpm 
...

Despres l'activem amb la comanda:
         ps-admin --enable-rocksdb -u root -p contraseña
Output:
Enter password:

Checking if RocksDB plugin is available for installation ...
INFO: ha_rocksdb.so library for RocksDB found at /usr/lib64/mysql/plugin/ha_rocksdb.so.

Checking RocksDB engine plugin status...
INFO: RocksDB engine plugin is not installed.

Installing RocksDB engine...
INFO: Successfully installed RocksDB engine plugin.
```
![image](https://user-images.githubusercontent.com/100956247/164489035-61ce124c-1d1f-44bd-9bc5-3f5d9a72ec91.png)


# ACTIVITAT 3 – STORAGE ENGINE MyRocks (1 punt)
```
DROP DATABASE IF EXISTS MyRocks;
CREATE DATABASE MyRocks;
USE MyRocks;
CREATE TABLE Llocs(
Lloc_id int unsigned not null AUTO_INCREMENT,
nom varchar(30) not null,
CONSTRAINT PK_lloc PRIMARY KEY (Lloc_id)
)ENGINE=RocksDB;
CREATE TABLE Rocks(
Rock_id int unsigned not null AUTO_INCREMENT,
nom varchar(30) not null,
DatadeTrobada DATE not null,
LlocdeTrobada varchar(30) not null,
Comentari varchar(100),
CONSTRAINT PK_rocks PRIMARY KEY(Rock_id))
ENGINE=RocksDB;
INSERT INTO Rocks(nom,DataDeTrobada,LlocdeTrobada,Comentari) VALUES ('Ametista','2022-04-21','Sevilla','En meitat del camp');
INSERT INTO Llocs(nom) Values ('Sevilla');
```
# ACTIVITAT 4. INNODB part I. REALITZA ELS SEGÜENTS APARTATS (obligatòria) (2 punts)

1. Desactiva l’opció que ve per defecte de innodb_file_per_table
```
SET GLOBAL innodb_file_per_table=OFF;
```
2. Quins són els permisos i l'usuari i grup de la carpeta que conté el directori de dades (datadir
```
Ho comprovem anan al /var/lib/mysql i utilitzant la comanda:
ls -alist
Ens retornara un output en el que dintre surtira aixó:
101966892 12288 -rw-r-----   1 mysql mysql 12582912 may 25 16:36  ibdata1
```
3. Mostra quina és la mida del tablespace de sistema (System Tablespace). Per què té aquesta
mida inicial?
```
Anem al MYSQL i dintre fem la sentencia:
SHOW VARIABLES LIKE '%data_file%';
Per defecte esta posat en 12MG i incrementa 8 MG cada vegada que es troba sense espai.
```
![image](https://user-images.githubusercontent.com/100956247/170293017-029977b7-be27-48be-99a2-4420744a323c.png)

4. Importa la BD Sakila com a taules InnoDB (https://dev.mysql.com/doc/index-other.html)
```
Com la taula Sakila te les taules amb InnoDB no fa falta canviar-hi les engines nomes instalar-ho.

Anem a aquest enllaç per descarrgarla:
       https://dev.mysql.com/doc/index-other.html
       
La instalem i la pasem a la maquina virtual en el meu cas ho he fet amb la aplicacio WinSCP
       https://winscp.net/eng/download.php
       
I per transformarla a mysql utilitzem aquesta sentència:       
       mysql> SOURCE /var/lib/mysql/mysql/sakila-schema.sql
```
5. Quin/quins són els fitxers de dades? A on es troben i quina és la seva mida?
```
Aquest/s fitxers són els fitxers ibdata, a on es guardaran totes les dades de les taules de la BD. Incloent els índexs.
El fitxer es troba dintre de la ruta /var/lib/mysql i per defecte la seva mida es de 12MG 
```
6. Canvia la configuració del MySQL per:
o Canviar la localització del directori de dades a /hd-mysql/
```
Per canviar al directori de dades anem al my.cnf i la linia que posa:
datadir=/var/lib/mysql
la canviem per la ruta var/hd-mysql/
````
o Tenir dos fitxers corresponents al tablespace de sistema complint:
        ▪ Tots dos han de tenir la mateixa mida inicial (50MB)
```
        Perquè dos fitxers ibdata tinguin la mateixa mida inicial de 50MB escrivim aixó dintre del fitxer my.cnf
                [mysqld]
        innodb_data_file_path=ibdata1:50M;ibdata2:50M:autoextend
```
▪ El tablespace ha de créixer de 5MB en 5MB.

![image](https://user-images.githubusercontent.com/100956247/170592034-13a6405d-1a9f-40e8-b210-aba004e0d9c9.png)

▪ Situa aquests fitxers en una nova localització simulant el següent:
                • /disk1/primer fitxer de dades → simularà un disc dur
                • /disk2/segon fitxer de dades → simularà un segon disc dur.
                ![image](https://user-images.githubusercontent.com/100956247/170591619-220579c3-4190-456a-be13-17b7710ac5b9.png)

```
sudo mount -a per guardar que cada vegada que iniciem la maquina seguexien els canvis
innodb_data_file_path =  /var/lib/mysql/ibdata:100G;/disk2/mysql/ibdata2:1000M:autoextend
```
# ACTIVITAT 5. INNODB part II. REALITZA ELS SEGÜENTS APARTATS (obligatòria) (1 punt)
1. Partint de l'esquema anterior configura el Percona Server perquè cada taula generi el seu 
propi tablespace en una carpeta anomenada tspaces (aquesta pot estar situada a on vULgueu). 
      • Indica quins són els canvis de configuració que has realitzat
```

```
# ACTIVITAT 6. INNODB part III. REALITZA ELS SEGÜENTS APARTATS (obligatòria) (1 punt)
1. Crea un tablespace anomenat 'ts1' situat a /discs-mysql/disc1/ i col·loca les taules 
actor, address i category de la BD Sakila.
```

```
2. Crea un altre tablespace anomenat 'ts2' situat a /discs-mysql/disc2/ i col·loca-hi la 
resta de taules.
```

```
3. Comprova que pots realitzar operacions DML a les taules dels dos tablespaces.
```

```
4. Quines comandes i configuracions has realitzat per fer els dos apartats anteriors?
```

```
# ACTIVITAT 7. REDOLOG. REALITZA ELS SEGÜENTS APARTATS (2 punts)
1. Com podem comprovar (Innodb Log Checkpointing):
• LSN (Log Sequence Number)
• L'últim LSN actualitzat a disc
• Quin és l'últim LSN que se li ha fet Checkpoint
```
Si fem la sentenceia aquesta sentencia dintre del MYSQL:
SHOW ENGINE INNODB STATUS\G;
Y anem a la part de LOG
```
![image](https://user-images.githubusercontent.com/100956247/170527453-048fa460-73c6-4f96-912e-85714fce822f.png)
2. Com podem mirar el número de pàgines modificades (dirty pages)? I el número total de 
pàgines?
```
Amb la mateixa sentencia si anem a altre seccio surt.
```
![image](https://user-images.githubusercontent.com/100956247/170527622-65c866c9-f944-4da3-a2fe-61e804897314.png)


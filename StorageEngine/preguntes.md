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


#ACTIVITAT 3 – STORAGE ENGINE MyRocks (1 punt)
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

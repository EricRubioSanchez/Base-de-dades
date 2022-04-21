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
percona-server-rocksdb-8.0.27-18.1.el8.x86_64.rpm                                                                                                                                                            1.4 MB/s |  14 MB     00:09
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                                                        1.4 MB/s |  14 MB     00:09
Ejecutando verificación de operación
Verificación de operación exitosa.
Ejecutando prueba de operaciones
Prueba de operación exitosa.
Ejecutando operación
  Preparando          :                                                                                                                                                                                                                  1/1
  Instalando          : percona-server-rocksdb-8.0.27-18.1.el8.x86_64                                                                                                                                                                    1/1
  Ejecutando scriptlet: percona-server-rocksdb-8.0.27-18.1.el8.x86_64                                                                                                                                                                    1/1


 * This release of Percona Server is distributed with RocksDB storage engine.
 * Run the following script to enable the RocksDB storage engine in Percona Server:

        ps-admin --enable-rocksdb -u <mysql_admin_user> -p[mysql_admin_pass] [-S <socket>] [-h <host> -P <port>]


  Verificando         : percona-server-rocksdb-8.0.27-18.1.el8.x86_64                                                                                                                                                                    1/1
Productos instalados actualizados.

Instalado:
  percona-server-rocksdb-8.0.27-18.1.el8.x86_64

¡Listo!
```

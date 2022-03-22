1. Un cop realitzada la instal·lació realitza una securització de la mateixa. Quin programa realitza
aquesta tasca? Realitza una securització de la instal·lació indicant que la contrasenya de root
sigui patata.
```
El programa que realitza la tasca de securització es mysql_secure_installation.
```
```
La primera comanda ens creara una contrasenya temporal
grep “temporary password” /var/log/mysqld.log

La segona comanda canviarem la contrasenya creada per la maquina amb una nostra que compleixi les politiques en el meu cas es P@ssw0rd.
Output:
Securing the MySQL server deployment.

Enter password for user root:

The existing password for the user account root has expired. Please set a new password.

New password:

Re-enter new password:
The 'validate_password' component is installed on the server.
The subsequent steps will run with the existing configuration
of the component.

Iniciem el servidor mysql
service mysql start
Output:
Redirecting to /bin/systemctl start mysql.service

Comprovem que estigui iniciat amb la comanda

service mysql status
Output:
Redirecting to /bin/systemctl status mysql.service
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor pres>
   Active: active (running) since Tue 2022-03-22 16:14:42 CET; 38min ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 888 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/>
 Main PID: 5529 (mysqld)
   Status: "Server is operational"
    Tasks: 39 (limit: 11400)
   Memory: 590.2M
   CGroup: /system.slice/mysqld.service
           └─5529 /usr/sbin/mysqld

mar 22 16:13:49 localhost.localdomain systemd[1]: Starting MySQL Server...
mar 22 16:14:42 localhost.localdomain systemd[1]: Started MySQL Server.

Utilitzem aquesta comanda:
firewall-cmd --zone=public --add-port=3306/tcp --permanent
Output:
success

Aquesta comanda també:
firewall-cmd --reload
Output:
success

Aquesta comanda es perqueè cada vegada que s'inicii la maquina també s'inicii el servidor mysql:
systemctl enable --now mysqld

Entrem dintre del mysql:
mysql -u root -p
Output:
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 16
Server version: 8.0.27-18 Percona Server (GPL), Release 18, Revision 24801e21b45

Copyright (c) 2009-2021 Percona LLC and/or its affiliates
Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Amb aquesta comanda farem acepti la contrasenya amb les politiques en LOW:
SET GLOBAL validate_password.policy=LOW;
Output:
Query OK, 0 rows affected (0,00 sec)

I amb aquesta comanda farem que amb una contrasenya de 6 digíts ya entri dintre de les polítiques:
SET GLOBAL validate_password.length = 6;
Output:
Query OK, 0 rows affected (0,00 sec)

I per últim posam la contrasenya patata:
set password = 'patata';
Output:
Query OK, 0 rows affected (0,01 sec)
```
2. Quines són les instruccions per arrancar / verificar status / apagar servei de la base de dades
de Percona Server en el sistema operatiu?
```

```
3. A on es troba i quin nom rep el fitxer de configuració del SGBD Percona Server?
```

```
4. A on es troben físicament els fitxers de dades (per defecte). Com ho has sabut?
```

```
5. Crea un usuari anomenat asix en el sistema operatiu i en SGBD de tal manera que aquest
usuari del sistema operatiu no hagi d'introduir l'usuari i password cada vegada que cridem al
client mysql?
http://dev.mysql.com/doc/refman/5.7/en/password-security-user.html
Usuari SO-→ asix / patata
Usuari MySQL → asix / patata
```

```
6. El servei de MySQL (mysqld) escolta al port 3306. Quina modificació/passos caldrien fer per
canviar aquest port a 33306 per exemple? Important: No realitzis els canvis. Només indica els
passos que faries.
```

```
7. Ensenya al professor que us podeu connectar al SGBD.
```

```

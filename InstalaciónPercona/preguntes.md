1. Un cop realitzada la instal·lació realitza una securització de la mateixa. Quin programa realitza
aquesta tasca? Realitza una securització de la instal·lació indicant que la contrasenya de root
sigui patata.
```
El programa que realitza la tasca de securització es mysql_secure_installation.
```
```
Securing the MySQL server deployment.

Enter password for user root:

The existing password for the user account root has expired. Please set a new password.

New password:

Re-enter new password:
The 'validate_password' component is installed on the server.
The subsequent steps will run with the existing configuration
of the component.
2. Quines són les instruccions per arrancar / verificar status / apagar servei de la base de dades
de Percona Server en el sistema operatiu?
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

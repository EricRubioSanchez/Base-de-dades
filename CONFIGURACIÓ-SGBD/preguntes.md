1. Quins són els logs activats per defecte? Crea un fitxer de configuració anomenat 
logs.cnf a on:
Activa els logs que no ho estiguin per defecte i indica les configuracions necessàries 
per activar-los. Indica les rutes dels fitxer de log de Binary, Slow Query i General. Quins 
paràmetres has creat/modificat?
2. Comprova l'estat de les opcions de log que has utilitzat mitjançant una sessió de mysql 
client. 
Exemple: (mysql> SHOW GLOBAL VARIABLES LIKE '%log')
3. Modifica el fitxer de configuració i desactiva els logs de binary, slow query i genral. Nota: 
Simplament desactiva'ls no borris altres paràmetres com la ruta dels fitxers, etc...
4. Activa els logs en temps d'execució mitjançant la sentència SET GLOBAL. També canvia 
el destí de log general a una taula (paràmetre log_output). Quines són les sentències que 
has utilitzat? A quina taula es guarden els dels del general log?
5. Carrega la BD Sakila localitzada a la web de 
o Descarrega't el fitxer sakila-schema.sql del Moodle.
o carrega la BD dins del MySQL utilitzant la sentència:
mysql> SOURCE <ruta_fitxer>/sakila-schema.sql;
6. Compte el numero de sentències CREATE TABLE dins del general log mitjançant una sentència SQL.
o Mostra quina sentència has utilitzat i mostra'n el resultat.
7. Executa una query mitjançant la funció SLEEP(11) per tal de que es guardi en el log de 
Slow Query Log. Mostra el contingut del log demostrant-ho

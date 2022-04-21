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

```

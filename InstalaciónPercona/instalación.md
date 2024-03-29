# Instalació de Percona Server en Red Hat Enterprise Linux

## Reactivació Red Hat Enterpirse Linux

### Esborrar el system profile localment
1. Primera comanda
 `subscription-manager remove --all`
 
 Output:
 ```
 El perfil del consumidor "1dd74b3d-3373-45e0-88d4-b39bbf849476" ha sido borrado del servidor. Use el comando 'clean' o cancele el registro para quitar el perfil         local.
 ```
2. Segona comanda
`subscription-manager unregister`
  
 Output:
 ```
 Anulando el registro de: subscription.rhsm.redhat.com:443/subscription
 Se ha cancelado el registro del sistema.
 ```
3. Tercera comanda
`subscription-manager clean`

 Output:
 ```
 Todos los datos eliminados
 ```
### Registrar-se i subscriure
1. Primera comanda
`subscription-manager register`

 Output:
 ```
 Registrándose a: subscription.rhsm.redhat.com:443/subscription
 Nombre de usuario:
 Contraseña:
 El sistema ha sido registrado con ID: ed6dfdd5-b8d7-4c24-bee8-5cd36cf8beb9
 El nombre del sistema registrado es: localhost.localdomain
 ```
2. Segona comanda
`subscription-manager refresh`

 Output:
 ```
 Todos los datos actualizados
 ```
3. Tercera comanda
`subscription-manager attach --auto`

 Output:
 ```
 Estatus de productos instalados:
 Nombre de producto: Red Hat Enterprise Linux for x86_64
 Estatus:            Suscrito
 ```

## Instalació
### Instalació del repositoris de Percona
1. Primera comanda
`yum -y update`
 Output:
 ```
 Actualización de repositorios de Subscription Management.
 Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)                   4.3 MB/s |  39 MB     00:09
 Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)                       18 kB/s | 4.1 kB     00:00
 Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)                      5.6 MB/s |  45 MB     00:08
 Última comprobación de caducidad de metadatos hecha hace 0:00:06, el vie 18 mar 2022 19:11:59 CET.
 Dependencias resueltas.
 ===========================================================================================================
  Paquete               Arq.   Versión                               Repositorio                       Tam.
 ===========================================================================================================
 Instalando:
 kernel                x86_64 4.18.0-348.20.1.el8_5                 rhel-8-for-x86_64-baseos-rpms    7.0 M
 ...
 ```
2. Segona comanda
`yum install https://repo.percona.com/yum/percona-release-latest.noarch.rpm`
 Output:
 ```
 Actualización de repositorios de Subscription Management.
 Última comprobación de caducidad de metadatos hecha hace 0:12:42, el vie 18 mar 2022 19:11:59 CET.
 percona-release-latest.noarch.rpm                                           71 kB/s |  20 kB     00:00
 Dependencias resueltas.
 ===========================================================================================================
  Paquete                       Arquitectura         Versión               Repositorio                 Tam.
 ===========================================================================================================
 Instalando:
  percona-release               noarch               1.0-27                @commandline                20 k

 Resumen de la transacción
 ===========================================================================================================
 Instalar  1 Paquete
 
 Tamaño total: 20 k
 Tamaño instalado: 32 k
 ¿Está de acuerdo [s/N]?:
 ...
 ```
3. Tercera comanda
 `percona-release setup ps80`
 Output:
 ```
  * Disabling all Percona Repositories
 On Red Hat 8 systems it is needed to disable the following DNF module(s): mysql  to install Percona-Server
 Do you want to disable it? [y/N] y
 Disabling dnf module...
 Actualización de repositorios de Subscription Management.
 Percona Release release/noarch YUM repository                              8.7 kB/s | 1.8 kB     00:00
 Dependencias resueltas.
 ...
 ```
4. Quarta comanda
 `yum install percona-server-server`
 Output:
 ```
 Actualización de repositorios de Subscription Management.
 Última comprobación de caducidad de metadatos hecha hace 0:12:42, el vie 18 mar 2022 19:11:59 CET.
 percona-release-latest.noarch.rpm                                           71 kB/s |  20 kB     00:00
 Dependencias resueltas.
 ===========================================================================================================
  Paquete                       Arquitectura         Versión               Repositorio                 Tam.
 ===========================================================================================================
 Instalando:
  percona-release               noarch               1.0-27                @commandline                20 k

 ...
 ```

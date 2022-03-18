# Instalació de Percona Server en Red Hat Enterprise Linux

## Reactivació Red Hat Enterpirse Linux
  1. Primera comanda

    subscription-manager remove --all
 
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
  

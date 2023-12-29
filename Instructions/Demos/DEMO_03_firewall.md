---
demo:
  title: 'Demostración: Creación y configuración de una instancia de Azure Firewall'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demostración: Creación y configuración de una instancia de Azure Firewall

**Nota:** Azure Firewall puede tardar unos minutos en implementarse.

En esta demostración, se explorará Azure Firewall.
Revise y cree una directiva de Azure Firewall y Firewall.
1.  [Diapositiva auxiliar] Antes de comenzar la demostración, vamos a revisar lo que es Azure Firewall.
2.  Acceda a Azure Portal.
3.  Cree una instancia de Azure Firewall.
4.  ⓘ En la pestaña Datos básicos se explican las opciones de configuración disponibles a medida que se rellenan. 
5.  Acepte los restantes valores predeterminados y, después, seleccione Revisar y crear.
6.  Una vez completada la implementación, vaya al recurso de firewall y revise la página de información general. 


### Configuración de una regla de aplicación 

1. [Diapositiva auxiliar] Reglas de directiva de Azure Firewall

Se trata de la regla de aplicación que permite el acceso saliente a www.google.com.
1.  Vaya a la directiva de firewall que creó.
2.  Seleccione Application rules (Reglas de aplicación).
3.  Seleccione Agregar una colección de reglas.
4.  En Nombre, escriba App-Coll01.
5.  En Prioridad, escriba 200.
6.  En Acción de recopilación de reglas, seleccione Denegar.
7.  En Reglas, para Nombre, escriba Allow-Google.
8.  En Tipo de origen, seleccione Dirección IP.
9.  En Origen, escriba 10.0.2.0/24.
10. En Protocolo:Puerto, escriba http, https.
11. En Tipo de destino, seleccione FQDN.
12. En Destino, escriba www.google.com
13. Seleccione Agregar.

Azure Firewall incluye una colección de reglas integradas para FQDN de infraestructura que están permitidos de forma predeterminada. Estos FQDN son específicos para la plataforma y no se pueden usar para otros fines. Para más información, consulte Nombres de dominio completos de infraestructura.

### Configurar una regla de red
Se trata de la regla de red que permite el acceso saliente a dos direcciones IP en el puerto 53 (DNS).
1.  Seleccione Reglas de red.
2.  Seleccione Agregar una colección de reglas.
3.  En Nombre, escriba Net-Coll01.
4.  En Prioridad, escriba 200.
5.  En Acción de recopilación de reglas, seleccione Denegar.
6.  En Rule collection group (Grupo de recopilación de reglas), seleccione DefaultNetworkRuleCollectionGroup.
7.  En Reglas, para Nombre, escriba Allow-DNS.
8.  En Tipo de origen, seleccione Dirección IP.
9.  En Origen, escriba 10.0.2.0/24.
10. En Protocolo, seleccione UDP.
11. En Puertos de destino, escriba 53.
12. En Tipo de destino, seleccione Dirección IP.
13. En Destino, escriba 209.244.0.3, 209.244.0.4.
Estos son servidores DNS públicos ofrecidos por CenturyLink.
14. Seleccione Agregar.


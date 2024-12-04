---
lab:
  title: 'Ejercicio 04: Configuración del enrutamiento de red'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Ejercicio 04: Configuración del enrutamiento de red

## Escenario

Para asegurarte de que se aplican las directivas de firewall, el tráfico de aplicación saliente debe enrutarse a través del firewall. Identificas estos requisitos. 
+ Se requiere una tabla de rutas. Esta tabla de rutas se asociará a las subredes front-end y back-end.  
+ Se requiere una ruta para filtrar todo el tráfico IP saliente de las subredes al firewall. Se usará la dirección IP privada del firewall. 

## Tareas de aptitudes

+ Creación y configuración de una tabla de rutas.
+ Vinculación de una tabla de rutas a una subred.
  
## Diagrama de arquitectura

![Diagrama en el que se muestra una red virtual con un firewall y una tabla de rutas.](../Media/task-3.png)


## Instrucciones del ejercicio

### Creación de una tabla de rutas

Azure crea automáticamente una [tabla de rutas](https://learn.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) para cada subred de una red virtual de Azure. La tabla de rutas incluye las siguientes [rutas del sistema](https://learn.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#system-routes) predeterminadas. Puedes crear tablas de rutas y rutas para invalidar las rutas predeterminadas del sistema de Azure.

**Registro de la dirección IP privada de app-vnet-firewall**

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escribe **Firewall**. Selecciona **Firewall** en los resultados de la búsqueda.

1. Selecciona **app-vnet-firewall**.

1. Selecciona **Información general** y registra la **dirección IP privada**.

**Adición de la tabla de rutas**

1. En el cuadro de búsqueda, escribe **Tabla de rutas**. Cuando tabla de rutas aparezca en los resultados de la búsqueda, selecciónalo.

1. En la página Tabla de rutas, selecciona **+ Crear** y crea la tabla de rutas. 

    | Propiedad       | Valor                        |
    | :------------- | :--------------------------- |
    | Suscripción   | **Selecciona la suscripción** |
    | Grupo de recursos | **RG1**                      |
    | Región         | **Este de EE. UU.**                  |
    | Nombre           | `app-vnet-firewall-rt`     |

1. Selecciona **Revisar y crear** y, luego, **Crear**.

1. Espera a que se complete la implementación de la tabla de rutas y selecciona **Ir al recurso**.  

### Asociación de la tabla de rutas a la subred

1. En el portal, continúa trabajando con la tabla de rutas y selecciona **app-vnet-firewall-rt**.

1. En la hoja **Configuración**, selecciona **Subredes** y, a después, ** + Asociar**.

1. Configura una asociación a la subred de front-end y selecciona **Aceptar**.  

    | Propiedad        | Valor              |
    | :-------------- | :----------------- |
    | Red virtual | **app-vnet** (RG1) |
    | Subred          | **Front-end**       |

1. Configura una asociación a la subred de back-end y selecciona **Aceptar**.  

    | Propiedad        | Valor              |
    | :-------------- | :----------------- |
    | Red virtual | **app-vnet** (RG1) |
    | Subred          | **backend**       |

### Creación de una ruta en la tabla de rutas

1. En el portal, continúa trabajando con la tabla de rutas y selecciona **app-vnet-firewall-rt**.

1. En la hoja **Configuración**, selecciona **Rutas** y, después, **+ Agregar**.

1. Configura la ruta y selecciona **Agregar**. 

    | Propiedad                            | Valor                                                   |
    | :---------------------------------- | :------------------------------------------------------ |
    | Nombre de ruta                          | **outbound-firewall**                                   |
    | Tipo de destino                    | **Direcciones IP**                                        |
    | Intervalos de direcciones IP de destino y CIDR | **0.0.0.0/0**                                           |
    | Tipo de próximo salto                       | **Aplicación virtual**                                   |
    | Dirección de próximo salto                    | **dirección IP privada del firewall** |


### Aprende más con el curso en línea

+ [Administra y controla el flujo de tráfico en la implementación de Azure con rutas](https://learn.microsoft.com/training/modules/control-network-traffic-flow-with-routes/). En este módulo, aprenderás a controlar el tráfico de red virtual de Azure implementando rutas personalizadas. Este módulo tiene dos espacios aislados. 

### Puntos clave

Enhorabuena por completar este ejercicio. Estos son los puntos clave:

+ El tráfico de red en Azure se enruta automáticamente entre las redes locales, las redes virtuales y las subredes de Azure. Las rutas del sistema controlan este enrutamiento.
+ Las rutas definidas por el usuario invalidan las rutas predeterminadas del sistema para que el tráfico pueda enrutarse a través de dispositivos virtuales de red (NVA). 
+ Los dispositivos virtuales de red (NVA) controlan el flujo del tráfico de red. Algunos ejemplos de NVA son firewalls, equilibradores de carga y enrutadores.
+ Las tablas de rutas contienen información de enrutamiento y están asociadas a una subred. 

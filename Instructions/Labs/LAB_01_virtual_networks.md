---
lab:
  title: 'Ejercicio 01: Creación y configuración de redes virtuales'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Ejercicio 01: Creación y configuración de redes virtuales

## Escenario

La organización está migrando una aplicación basada en web a Azure. Tu primera tarea consiste en colocar las redes y subredes virtuales. También debes emparejar de forma segura las redes virtuales. Identificas estos requisitos. 
+ Se requieren dos redes virtuales, **app-vnet** y **hub-vnet**. Esto simula una arquitectura de red radial. 
+ app-vnet hospedará la aplicación. Esta red virtual requiere de dos subredes. La **subred de front-end** hospedará los servidores web. La **subred back-end** hospedará los servidores de base de datos.
+ hub-vnet solo requiere una subred para el firewall. 
+ Las dos redes virtuales deben ser capaces de comunicarse entre sí de forma segura y privada a través de un **emparejamiento de red virtual**. 
+ Ambas redes virtuales deben estar en la misma región. 

## Tareas de aptitudes

+ Creación de una red virtual.
+ Creación de una subred.
+ Configuración del emparejamiento de VNET.

## Diagrama de arquitectura

![Diagrama que muestra dos redres virtuales que están emparejadas.](../Media/task-1.png)

## Instrucciones del ejercicio

**Nota**: para completar este laboratorio, necesitarás una [suscripción de Azure](https://azure.microsoft.com/free/) con el rol RBAC **Colaborador** asignado. En este laboratorio, cuando se te pida que crees un recurso, para las propiedades que no se especifican, usa el valor predeterminado.

### Creación de redes y subredes virtuales radiales

Una [red virtual de Azure](https://learn.microsoft.com/azure/virtual-network/virtual-networks-overview) permite que muchos tipos de recursos de Azure se comuniquen de forma segura entre sí, con Internet y con las redes locales. Todos los recursos de Azure en una red virtual se implementan en [subredes](https://learn.microsoft.com/azure/virtual-network/virtual-network-manage-subnet?tabs=azure-portal) de la red virtual. 

1. Inicia sesión en **Azure Portal** - `https://portal.azure.com`.
   
1. Busca y selecciona `Virtual Networks`.
   
1. Selecciona **+ Crear** y completa la configuración de la **app-vnet**. Esta red virtual requiere dos subredes, **front-end** y **back-end**. 

    | Propiedad             | Valor           |
    | :------------------- | :-------------- |
    | Grupo de recursos       | **RG1**         |
    | Nombre de la red virtual | `app-vnet`    |
    | Región               | **Este de EE. UU.**     |
    | Espacio de direcciones IPv4   | **10.1.0.0/16** |
    | Nombre de subred          | `frontend`    |
    | Intervalo de direcciones de subred | **10.1.0.0/24** |
    | Nombre de subred          | `backend`     |
    | Intervalo de direcciones de subred | **10.1.1.0/24** |

    **Nota**: deja los demás valores de configuración en sus valores predeterminados. Cuando hayas finalizado, selecciona **"Revisar y crear"** y, a continuación, **Crear**.
   
1. Crea la configuración de red virtual **Hub-vnet**. Esta red virtual tiene la subred de firewall. 

    | Propiedad             | Valor                    |
    | :------------------- | :----------------------- |
    | Grupo de recursos       | **RG1**                  |
    | Nombre                 | `hub-vnet` |
    | Región               | **Este de EE. UU.**              |
    | Espacio de direcciones IPv4   | **10.0.0.0/16**          |
    | Nombre de subred          | **AzureFirewallSubnet**  |
    | Intervalo de direcciones de subred | **10.0.0.0/24**          |

1. Una vez completadas las implementaciones, busca y selecciona tus "redes virtuales".

1. Comprueba que se implementaron las redes y las subredes virtuales. 

### Configuración de una relación del mismo nivel entre las redes virtuales

El [emparejamiento de red virtual](https://learn.microsoft.com/azure/virtual-network/virtual-network-peering-overview) permite conectar sin problemas dos o más redes virtuales en Azure. 

1. Busca y selecciona la máquina virtual `app-vnet`.
   
1. En la hoja **Configuración**, selecciona **Emparejamientos**.
   
1. **+ Agregar** un emparejamiento entre la dos redes virtuales. 

    | Propiedad                                 | Valor                          |
    | :--------------------------------------- | :----------------------------- |
    | Nombre del vínculo de emparejamiento remoto              | `app-vnet-to-hub` |
    | Red virtual    | `hub-vnet` |
    | Nombre del vínculo de emparejamiento de red virtual local | `hub-to-app-vnet` |

    **Nota**: deja los demás valores de configuración en sus valores predeterminados. Selecciona **Agregar** para crear el emparejamiento de grupo de red virtual.

1. Una vez completada la implementación, comprueba que el **Estado de emparejamiento** es **Conectado**.

## Aprende más con el curso en línea

+ [Introducción a las redes virtuales de Azure](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/). En este módulo, descubrirás cómo diseñar e implementar servicios de red de Azure. Obtendrás información sobre redes virtuales, direcciones IP públicas y privadas, DNS, emparejamiento de redes virtuales, enrutamiento y Azure Virtual NAT.

## Puntos clave

Enhorabuena por completar este ejercicio. Estos son los puntos clave:

+ Las redes virtuales (VNet) de Azure proporcionan un entorno de red seguro y aislado para los recursos en la nube. Puedes crear varias redes virtuales por región y suscripción.
+ Al diseñar redes virtuales asegúrate de que el espacio de direcciones de VNet (bloque CIDR) no se solape con otros intervalos de red de tu organización.
+ Una subred es un intervalo de direcciones IP en la red virtual. Puedes segmentar VNet en subredes de diferentes tamaños y crear tantas subredes como necesites para la organización y la seguridad dentro del límite de la suscripción. Cada subred debe tener su propio intervalo de direcciones únicas.
+ Algunos servicios de Azure, como Azure Firewall, requieren su propia subred.
+ El emparejamiento de redes virtuales permite conectar sin problemas dos redes virtuales de Azure. A efectos de conectividad las redes virtuales aparecen como una sola.

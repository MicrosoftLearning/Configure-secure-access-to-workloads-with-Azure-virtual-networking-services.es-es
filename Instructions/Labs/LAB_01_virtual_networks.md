---
lab:
  title: 'Ejercicio: Aislamiento y segmentación de red para la aplicación web'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Laboratorio: Proporcionar una red virtual de centro de servicios compartidos con aislamiento y segmentación

## Escenario

Se le ha encargado aplicar [principios de confianza cero](https://learn.microsoft.com/security/zero-trust/azure-infrastructure-networking) a la red virtual de un centro en Azure. El departamento de TI necesita aislamiento y segmentación de redes para la aplicación web en una red de tipo spoke. Para proporcionar aislamiento y segmentación de red para la aplicación web, debe crear una red virtual de Azure con subredes con el espacio de direcciones que proporcionó el equipo de TI. Una vez creada la red virtual, el siguiente paso es configurar el emparejamiento de red virtual. Esto permite que las redes virtuales se comuniquen entre sí de forma segura y privada.

### Diagrama de la arquitectura

![Diagrama que muestra dos redres virtuales que están emparejadas.](../Media/task-1.png)

### Tareas de aptitudes

- Creación de una red virtual
- Creación de una subred
- Configuración del emparejamiento de red virtual

## Instrucciones del ejercicio

>**Nota**: Para completar este laboratorio, necesitará una [suscripción de Azure](https://azure.microsoft.com/free/) con el rol RBAC **Colaborador** asignado.

> En este laboratorio, cuando se le pida que cree un recurso, para las propiedades que no se especifican, use el valor predeterminado.

### Creación de redes y subredes virtuales de tipo hub-and-spoke

Comience creando las redes virtuales que se muestran en el diagrama anterior.

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.
   
1. Busque y seleccione `Virtual Networks`.
   
1. Selecciona **+ Crear** y completa la configuración de la **app-vnet**. Esta red virtual requiere dos subredes, **front-end** y **back-end**. 

    | Propiedad             | Valor           |
    | :------------------- | :-------------- |
    | Grupo de recursos       | **RG1**         |
    | Nombre de la red virtual | `app-vnet`    |
    | Region               | **Este de EE. UU.**     |
    | Espacio de direcciones IPv4   | **10.1.0.0/16** |
    | Nombre de subred          | `frontend`    |
    | Intervalo de direcciones de subred | **10.1.0.0/24** |
    | Nombre de subred          | `backend`     |
    | Intervalo de direcciones de subred | **10.1.1.0/24** |

    **Nota**: Deje los demás valores de configuración en sus valores predeterminados. Cuando hayas finalizado, selecciona **"Revisar y crear"** y, a continuación, **Crear**.
   
1. Crea la configuración de red virtual **Hub-vnet**. Esta red virtual tiene la subred de firewall. 

    | Propiedad             | Valor                    |
    | :------------------- | :----------------------- |
    | Grupo de recursos       | **RG1**                  |
    | Nombre                 | `hub-vnet` |
    | Region               | **Este de EE. UU.**              |
    | Espacio de direcciones IPv4   | **10.0.0.0/16**          |
    | Nombre de subred          | **AzureFirewallSubnet**  |
    | Intervalo de direcciones de subred | **10.0.0.0/24**          |

1. Una vez completadas las implementaciones, busca y selecciona tu **grupo de recursos**. Confirma que las nuevas redes virtuales forman parte del grupo de recursos. 

### Configurar una relación del mismo nivel entre las redes virtuales

1. Busca y selecciona la máquina virtual `app-vnet`.
   
1. En la hoja **Configuración**, selecciona **Emparejamientos**.
   
1. **+ Agregar** un emparejamiento entre la dos redes virtuales. 

    | Propiedad                                 | Valor                          |
    | :--------------------------------------- | :----------------------------- |
    | Nombre del vínculo de emparejamiento              | `app-vnet-to-hub` |
    | Red virtual    | `hub-vnet` |
    | Nombre del vínculo de emparejamiento de red virtual local | `hub-to-app-vnet` |

    **Nota**: Deje los demás valores de configuración en sus valores predeterminados. Seleccione **Agregar** para crear el emparejamiento de grupo de red virtual.

    [Más información sobre el emparejamiento de red virtual](https://learn.microsoft.com/azure/virtual-network/virtual-network-manage-peering?tabs=peering-portal).

1. Una vez completada la implementación, comprueba que el **Estado de emparejamiento** es **Conectado**. 

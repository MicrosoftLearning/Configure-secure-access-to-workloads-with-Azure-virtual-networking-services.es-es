---
lab:
  title: 'Ejercicio 02: Creación y configuración de grupos de seguridad de red'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Ejercicio 02: Creación y configuración de grupos de seguridad de red

## Escenario

La organización necesita que el control del tráfico en app-vnet se controle estrechamente. Identificas estos requisitos.
+ La subred de front-end tiene servidores web a los que se puede acceder desde Internet. Se requiere un **grupo de seguridad de aplicaciones** (ASG) para esos servidores. El ASG debe estar asociado a cualquier interfaz de máquina virtual que forme parte del grupo. Esto permitirá que los servidores web se administren fácilmente. 
+ Se requiere una **regla de NSG** para permitir el tráfico HTTPS entrante al ASG. Esta regla usa el protocolo TCP en el puerto 443. 
+ La subred back-end tiene servidores de base de datos usados por los servidores web front-end. Se requiere un **grupo de seguridad de red** (NSG) para controlar este tráfico. El grupo de seguridad de red debe estar asociado a cualquier interfaz de máquina virtual a la que accedan los servidores web. 
+ Se requiere una **regla de NSG** para permitir el tráfico de red entrante desde el ASG a los servidores back-end.  Esta regla usa el servicio MS SQL y el puerto 1443. 
+ Para las pruebas, se debe instalar una máquina virtual en la subred de front-end (VM1) y en la subred de back-end (VM2).  El grupo de TI ha proporcionado una plantilla de Azure Resource Manager para implementar estos **servidores Ubuntu**. 

## Tareas de aptitudes

+ Creación de un grupo de seguridad de red.
+ Creación de reglas de grupo de seguridad de red.
+ Asociación del grupo de seguridad de red con una subred.
+ Creación y uso de grupos de seguridad de aplicaciones en las reglas del grupo de seguridad de red.

## Diagrama de arquitectura

![Diagrama en el que se muestra un grupo de seguridad de red y un grupo de seguridad de aplicaciones en una red virtual.](../Media/task-2.png)




## Instrucciones del ejercicio

### Creación de la infraestructura de red para el ejercicio

**Nota:** este ejercicio requiere que se instalen las redes y subredes virtuales del laboratorio 01. Si necesitas implementar esos recursos, se proporciona una [plantilla](https://github.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/blob/main/Allfiles/Labs/All-Labs/create-vnet-subnets-template.json).

1. Usa el icono (superior derecho) para iniciar una sesión de **Cloud Shell**. Como alternativa, ve directamente a `https://shell.azure.com`.

1. Si se te pide que selecciones **Bash** o **PowerShell**, selecciona **PowerShell**.

1. El almacenamiento no es necesario para esta tarea Selecciona la suscripción. 

1. Implementa las máquinas virtuales necesarias para este ejercicio. 

   ```powershell
   $RGName = "RG1"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateUri https://raw.githubusercontent.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/main/Instructions/Labs/azuredeploy.json
   ```
  
1. En el portal, busca y selecciona `virtual machines`. Comprueba que vm1 y vm2 están en **ejecución**.

### Creación de grupos de seguridad de aplicaciones

[Los grupos de seguridad de aplicaciones (ASG)](https://learn.microsoft.com/azure/virtual-network/application-security-groups) permiten agrupar servidores con funciones similares. Por ejemplo, todos los servidores web que hospedan la aplicación. 

1. En el portal, busca y selecciona `Application security groups`.
   
1. Selecciona **+ Crear** y configura el grupo de seguridad de aplicaciones. 

    | Propiedad       | Valor                        |
    | :------------- | :--------------------------- |
    | Suscripción   | **Selecciona la suscripción** |
    | Grupo de recursos | **RG1**                      |
    | Nombre           | `app-backend-asg`          |
    | Región         | **Este de EE. UU.**                  |

1. Selecciona **Revisar y crear** y luego **Crear**.

**Nota**: crearás el grupo de seguridad de aplicaciones en la misma región de la red virtual existente.

**Asociación del grupo de seguridad de aplicaciones a la interfaz de red de la máquina virtual**

1. En Azure Portal, busca y selecciona `VM2`.

1. En la hoja **Redes**, selecciona la **Grupos de seguridad de aplicaciones** y, después, selecciona **Agregar grupos de seguridad de aplicaciones**.

1. Selecciona **app-backend-asg** y, después, selecciona **Agregar**.
   
### Creación de la subred y asociación con el grupo de seguridad de red

Los [grupos de seguridad de red (NSG)](https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview) protegen el tráfico de red de una red virtual. 

1. En el portal, busca y selecciona `Network security group`.

1. Selecciona **+ Crear** y configura el grupo de seguridad de red. 

    | Propiedad       | Valor                        |
    | :------------- | :--------------------------- |
    | Suscripción   | **Seleccione la suscripción** |
    | Grupo de recursos | **RG1**                      |
    | Nombre           | `app-vnet-nsg`            |
    | Región         | **Este de EE. UU.**                  |

1. Selecciona **Revisar y crear** y luego **Crear**.

**Asocia el grupo de seguridad de red con la subred de back-end de app-vnet.**

Los grupos de seguridad de red se pueden asociar a subredes o interfaces de red individuales conectadas a Azure Virtual Machines. 

1. Selecciona **Ir al recurso** o navega al recurso **app-vnet-nsg**.

1. En la hoja **Configuración**, selecciona **Subredes**.

1. Selecciona **+ Asociar**

1. Selecciona **app-vnet (RG1)** y, a continuación, la subred **back-end**. Selecciona **Aceptar**.

### Creación de reglas de grupo de seguridad de red

Un grupo de seguridad de red usa las [reglas de seguridad](https://learn.microsoft.com/azure/virtual-network/network-security-group-how-it-works) para filtrar el tráfico entrante y saliente. 

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escribe **grupos de seguridad de red**. En los resultados de la búsqueda, selecciona Grupos de seguridad de red.

1. En la lista de grupos de seguridad de red, selecciona **app-vnet-nsg**.

1. En la hoja **Configuración**, selecciona **Reglas de seguridad de entrada**.

1. Selecciona **+ Agregar** y configura una regla de seguridad de entrada. 

    | Propiedad                               | Valor                          |
    | :------------------------------------- | :----------------------------- |
    | Origen                                 | **Cualquiera**                        |
    | Intervalos de puertos de origen                     | **\***                         |
    | Destino                            | **Grupo de seguridad de aplicaciones** |
    | Grupo de seguridad de aplicación de destino | **app-backend-asg**            |
    | Servicio                                | **SSH**                        |
    | Acción                                 | **Permitir**                      |
    | Prioridad                               | **100**                        |
    | Nombre                                   | **AllowSSH**                   |


### Aprende más con el curso en línea

+ [Filtra el tráfico de red con un grupo de seguridad de red mediante Azure Portal](https://learn.microsoft.com/training/modules/filter-network-traffic-network-security-group-using-azure-portal/). En este módulo, te centrarás en filtrar el tráfico de red mediante grupos de seguridad de red (NSG) de Azure Portal. Aprenderás a crear, configurar y aplicar grupos de seguridad de red para mejorar la seguridad de red.
+ [Protegerás y aislarás el acceso a recursos de Azure mediante grupos de seguridad de red y puntos de conexión de servicio](https://learn.microsoft.com/training/modules/secure-and-isolate-with-nsg-and-service-endpoints/). En este módulo, aprenderás sobre los grupos de seguridad de red y cómo restringir la conectividad de red. 

### Puntos clave

Enhorabuena por completar este ejercicio. Estos son los puntos clave:

+ Los grupos de seguridad de aplicaciones permiten organizar máquinas virtuales y definir directivas de seguridad de red basadas en las aplicaciones de la organización.
+ El grupo de seguridad de red de Azure se utiliza para filtrar el tráfico de red entre recursos de Azure en una red virtual de Azure.
+ Puedes asociar cero o un grupo de seguridad de red a cada subred e interfaz de red de la red virtual en una máquina virtual. 
+ Un grupo de seguridad de red contiene reglas de seguridad que permiten o deniegan el tráfico de red entrante o saliente de varios recursos de Azure.
+ Une las máquinas virtuales a un grupo de seguridad de aplicaciones. A continuación, usa el grupo de seguridad de aplicaciones como origen o destino en las reglas del grupo de seguridad de red.




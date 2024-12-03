---
lab:
  title: 'Ejercicio 03: Creación y configuración de una instancia de Azure Firewall'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Ejercicio 03: Creación y configuración de una instancia de Azure Firewall

## Escenario

La organización necesita una seguridad de red centralizada para la red virtual de la aplicación. A medida que aumenta el uso de la aplicación, se necesitarán filtros de nivel de aplicación más granulares y protección contra amenazas avanzada. Además, se espera que la aplicación necesite actualizaciones continuas de canalizaciones de Azure DevOps. Identificas estos requisitos.
+ Se necesita Azure Firewall para mayor seguridad en app-vnet. 
+ Se debe configurar una **directiva de firewall** para ayudar a administrar el acceso a la aplicación. 
+ Se requiere una **regla de aplicación** de directiva de firewall. Esta regla permitirá que la aplicación acceda a Azure DevOps para que se pueda actualizar el código de aplicación. 
+ Se requiere una **regla de red** de directiva de firewall. Esta regla permitirá la resolución DNS. 

### Tareas de aptitudes

+ Creación de una instancia de Azure Firewall.
+ Creación y configuración de una directiva de firewall.
+ Creación de una colección de reglas de aplicación.
+ Creación de una colección de reglas de red.

## Diagrama de arquitectura

![Diagrama en el que se muestra una red virtual con un firewall y una tabla de rutas.](../Media/task-3.png)


  
## Instrucciones del ejercicio

### Creación de una subred de Azure Firewall en nuestra red virtual existente

1. En el cuadro de búsqueda de la parte superior del portal, escribe **Redes virtuales**. En los resultados de la búsqueda, selecciona **Redes virtuales**.

1. Selecciona **app-vnet**.

1. Selecciona **Subredes**.

1. Selecciona **+Subred**.

1. Escribe la información siguiente y selecciona **Guardar**.

    | Propiedad      | Valor                   |
    | :------------ | :---------------------- |
    | Nombre          | **AzureFirewallSubnet** |
    | Intervalo de direcciones | **10.1.63.0/24**        |

**Nota**: deja los demás valores de configuración en sus valores predeterminados.

### Creación de una instancia de Azure Firewall

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escribe **Firewall**. Selecciona **Firewall** en los resultados de la búsqueda.

1. Selecciona **+ Crear**.

1. Crea un firewall con los valores de la tabla siguiente. Para cualquier propiedad que no se especifique, usa el valor predeterminado.
    >**Nota:** Azure Firewall puede tardar unos minutos en implementarse.

    | Propiedad                 | Valor                                             |
    | :----------------------- | :------------------------------------------------ |
    | Grupo de recursos           | **RG1**                                           |
    | Nombre                     | **app-vnet-firewall**                             |
    | SKU del firewall             | **Estándar**                                      |
    | Administración del firewall      | **Uso de una directiva de firewall para administrar este firewall** |
    | Directiva de firewall          | Selecciona **Agregar nuevo**.                                |
    | Nombre de la directiva              | **fw-policy**                                     |
    | Region                   | **Este de EE. UU.**                                       |
    | Nivel de directiva              | **Estándar**                                      |
    | Elegir una red virtual | **Utilizar existente**                                  |
    | Red virtual          | **app-vnet** (RG1)                                |
    | Dirección IP pública        | Agregar nuevo: **fwpip**                                |

    [Obtén más información sobre cómo crear un firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal).

1. Selecciona **Revisar y crear** y luego **Crear**.

### Actualización de la directiva de firewall

1. En el portal, busca y selecciona `Firewall Policies`. 

1. Selecciona **fw-policy**.

### Adición de una regla de aplicación

1. En la hoja **Configuración**, selecciona **Reglas de aplicación** y, luego, **Agregar una colección de reglas**.

1. Configura la colección de reglas de aplicación y selecciona **Agregar**. 

    | Propiedad               | Valor                                     |
    | :--------------------- | :---------------------------------------- |
    | Nombre                   | `app-vnet-fw-rule-collection`         |
    | Tipo de colección de reglas   | **Aplicación**                           |
    | Priority               | `200`                                   |
    | Acción de colección de reglas | **Permitir**                                 |
    | Grupo de colección de reglas  | **DefaultApplicationRuleCollectionGroup** |
    | Nombre             | `AllowAzurePipelines`                |
    | Tipo de origen      | **Dirección IP**                         |
    | Origen           | `10.1.0.0/23`                       |
    | Protocolo         | `https`                             |
    | Tipo de destino | **FQDN**                                  |
    | Destino      | `dev.azure.com, azure.microsoft.com` |

**Nota**: la regla **AllowAzurePipelines** permite que la aplicación web acceda a Azure Pipelines. La regla permite que la aplicación web acceda al servicio Azure DevOps y al sitio web de Azure.

### Adición de una regla de red

1. En la hoja **Configuración**, selecciona **Reglas de red** y, después, **Agregar una colección de red**.

1. Configura la regla de red y selecciona **Agregar**.  

    | Propiedad               | Valor                                 |
    | :--------------------- | :------------------------------------ |
    | Nombre                   | `app-vnet-fw-nrc-dns`               |
    | Tipo de colección de reglas   | **Network**                           |
    | Prioridad               | `200`                        |
    | Acción de colección de reglas | **Permitir**                             |
    | Grupo de colección de reglas  | **DefaultNetworkRuleCollectionGroup** |
    | Regla                  | **AllowDns**         |
    | Origen                | `10.1.0.0/23`      |
    | Protocolo              | **UDP**              |
    | Puertos de destino     | `53`               |
    | Direcciones de destino | **1.1.1.1, 1.0.0.1** |

### Comprobación del estado de la directiva de firewall y del firewall

1. En el portal de búsqueda, busca y selecciona **Firewall**. 

1. Mira **app-vnet-firewall** y asegúrate de que el **estado de aprovisionamiento** es **Correcto**. Esta operación puede tardar unos minutos. 

1. En el portal busca y selecciona **Directivas de firewall**.

1. Mira **fw-policy** y asegúrate de que el **estado de aprovisionamiento** es **Correcto**. Esta operación puede tardar unos minutos.

### Aprende más con el curso en línea

+ [Introducción a Azure Firewall](https://learn.microsoft.com/training/modules/introduction-azure-firewall/). En este módulo, conocerás las características, las reglas, las opciones de implementación y la administración de Azure Firewall.
+ [Introducción a Azure Firewall Manager](https://learn.microsoft.com/training/modules/intro-to-azure-firewall-manager/). En este módulo, aprenderás cómo Azure Firewall Manager proporciona una directiva de seguridad central y una administración de rutas para perímetros de seguridad basados en la nube

### Puntos clave

Enhorabuena por completar este ejercicio. Estos son los puntos clave:

+ Azure Firewall es un servicio de seguridad basado en la nube que protege los recursos de red virtual de Azure de amenazas entrantes y salientes.
+ Una directiva de firewall es un recurso de Azure que contiene una o más colecciones de reglas de NAT, red y aplicación.
+ Las reglas de red permiten o deniegan el tráfico en función de las direcciones IP, los puertos y los protocolos.
+ Las reglas de aplicación permiten o deniegan el tráfico en función de nombres de dominio completos (FQDN), direcciones URL y protocolos HTTP/HTTPS.

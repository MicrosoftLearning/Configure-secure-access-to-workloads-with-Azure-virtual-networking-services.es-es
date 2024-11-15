---
lab:
  title: 'Ejercicio 05: Creación de zonas DNS y configuración de DNS'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Ejercicio 05: Creación de zonas DNS y configuración de DNS

## Escenario

La organización necesita que las cargas de trabajo usen nombres de dominio en lugar de direcciones IP para las comunicaciones internas.  La organización no quiere agregar una solución DNS personalizada. Identificas estos requisitos.
+ Se requiere una **zona DNS privada** para contoso.com.
+ El DNS usará un **vínculo de red virtual** a app-vnet. 
+ Se requiere un nuevo **registro DNS** para la subred de back-end. 

## Tareas de aptitudes

+ Creación y configuración de una zona DNS privada.
+ Creación y configuración de registros DNS.
+ Configuración de DNS en una red virtual.
  
## Diagrama de la arquitectura

![Diagrama de Azure DNS vinculado a una red virtual.](../Media/task-5.png)



## Instrucciones del ejercicio

**Nota:** este ejercicio requiere que se instalen las redes y subredes virtuales del laboratorio 01. Si necesitas implementar esos recursos, se proporciona una [plantilla](https://github.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/blob/main/Allfiles/Labs/All-Labs/create-vnet-subnets-template.json).

### Creación de una zona DNS privada

El [DNS privado de Azure](https://learn.microsoft.com/azure/dns/private-dns-overview) proporciona un servicio DNS confiable y seguro para administrar y resolver los nombres de dominio en una red virtual sin necesidad de agregar una solución DNS personalizada. Al usar zonas DNS privadas, puedes usar nombres de dominio personalizados en lugar de los nombres proporcionados por Azure.

1. En Azure Portal, busca y selecciona `Private dns zones`.

1. Selecciona **+ Crear** y configure la zona DNS. 

    | Propiedad       | Valor                        |
    | :------------- | :--------------------------- |
    | Suscripción   | **Selecciona la suscripción** |
    | Grupo de recursos | **RG1**                      |
    | Nombre           | `private.contoso.com`              |
    | Región         | **Este de EE. UU.**                  |

1. Selecciona **Revisar y crear** y luego **Crear**.

1. Espera a que se implemente la zona DNS y selecciona **Ir al recurso**. 

### Creación de un vínculo de red virtual para tu zona DNS privada

Para resolver registros DNS en una zona DNS privada, los recursos deben estar vinculados a la zona privada. Un [vínculo de red virtual](https://learn.microsoft.com/azure/dns/private-dns-virtual-network-links) asocia la red virtual a la zona privada.

1. En el portal, sigue trabajando en la zona DNS de **private.contoso.com**. 

1. En la hoja **Administración de DNS**, selecciona **+ Vínculos de red virtual**.

1. Selecciona **+ Agregar"** y configura el vínculo de red virtual. 

    | Propiedad                 | Valor             |
    | :----------------------- | :---------------- |
    | Nombre del vínculo                | `app-vnet-link` |
    | Red virtual          | **app-vnet**      |
    | Habilitación del registro automático | **Habilitado**       |

1. Selecciona **Crear** y espera a que se complete la implementación. Si es necesario, **actualiza** la página. 

### Creación de un conjunto de registros de DNS

Los [registros DNS](https://learn.microsoft.com/en-us/azure/dns/dns-zones-records#dns-records) proporcionan información sobre la zona DNS. 

1. En el portal, sigue trabajando en la zona DNS de **private.contoso.com**. 

1. En la hoja **Administración de DNS**, selecciona **+ Conjuntos de registros**.

1. Observa que se han creado automáticamente dos registros A para cada una de las máquinas virtuales. 

1. Selecciona **+ Agregar** y configura un conjunto de registros. Cuando termines, selecciona **Agregar**. 
   
    | Propiedad   | Valor        |
    | :--------- | :----------- |
    | Nombre       | `backend`    |
    | Tipo       | **A**
                  |
    | TTL        | **1**        |
    | Dirección IP | **10.1.1.5** |

**Nota:** este conjunto de registros implica que hay una máquina virtual en app-vnet con una dirección IP privada de 10.1.1.5.

### Aprende más con el curso en línea

+ [Introducción a Azure DNS](https://learn.microsoft.com/training/modules/intro-to-azure-dns/). En este módulo se explica Azure DNS, cómo funciona y cuándo debes elegir usar Azure DNS como solución para satisfacer las necesidades de tu organización.
+ [Hospeda el dominio en Azure DNS](https://learn.microsoft.com/training/modules/host-domain-azure-dns/). En este módulo, aprenderás cómo crear una zona y un registro DNS.

### Puntos clave

Enhorabuena por completar este ejercicio. Estos son los puntos clave:

+ Azure DNS es un servicio en la nube que permite hospedar y administrar dominios de sistema de nombres de dominio (DNS), también conocidos como zonas DNS. 
+ Los datos de las zonas de nombres de dominio de host de zona pública de Azure DNS para los registros que pretende que resuelva cualquier host de Internet.
+ Las zonas de DNS privado de Azure te permiten configurar un espacio de nombres de zona DNS privada para recursos privados de Azure.
+ Una zona DNS es una colección de registros DNS. Los registros DNS proporcionan información sobre el dominio.

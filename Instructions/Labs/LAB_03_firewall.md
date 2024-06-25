---
lab:
  title: 'Ejercicio: Protección de la aplicación web frente al tráfico malintencionado y bloqueo del acceso no autorizado'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Laboratorio: Protección de la aplicación web frente al tráfico malintencionado y bloqueo del acceso no autorizado

## Escenario

Su organización busca proteger la aplicación web frente al tráfico malintencionado y bloquear el acceso no autorizado.

Además de los grupos de seguridad de red y los grupos de seguridad de aplicaciones, se puede configurar un firewall para agregar otra capa de seguridad a la aplicación web. El firewall protege la aplicación web del tráfico malintencionado y bloquea el acceso no autorizado con las directivas que configure.

La directiva de Azure Firewall es un recurso general con configuración operativa y de seguridad para Azure Firewall. Permite definir una jerarquía de reglas y exigir el cumplimiento. En esta tarea configurará las reglas de aplicación y de red para el firewall mediante la directiva de Firewall. Puede usar la directiva de Azure Firewall para administrar conjuntos de reglas que Azure Firewall use para filtrar el tráfico.

### Diagrama de la arquitectura

![Diagrama en el que se muestra una red virtual con un firewall y una tabla de rutas.](../Media/task-3.png)

### Tareas de aptitudes

- Cree una instancia de Azure Firewall.
- Cree y configure una directiva de firewall.
- Cree una colección de reglas de aplicación.
- Cree una colección de reglas de red.
  
## Instrucciones del ejercicio

### Creación de una subred de Azure Firewall en nuestra red virtual existente

1. En el cuadro de búsqueda de la parte superior del portal, escriba **Redes virtuales**. En los resultados de la búsqueda, seleccione **Redes virtuales**.

1. Seleccione **app-vnet**.

1. Seleccione **Subredes**.

1. Seleccione **+Subred**.

1. Escriba la información siguiente y seleccione **Guardar**.

    | Propiedad      | Valor                   |
    | :------------ | :---------------------- |
    | Nombre          | **AzureFirewallSubnet** |
    | Intervalo de direcciones | **10.1.63.0/24**        |

    > **Nota**: Deje los demás valores de configuración en sus valores predeterminados.

### Creación de una instancia de Azure Firewall

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba **Firewall**. Seleccione **Firewall** en los resultados de la búsqueda.

1. Seleccione **+ Create** (+ Crear).

1. Cree un firewall con los valores de la tabla siguiente. Para cualquier propiedad que no se especifique, use el valor predeterminado.
    >**Nota:** Azure Firewall puede tardar unos minutos en implementarse.

    | Propiedad                 | Valor                                             |
    | :----------------------- | :------------------------------------------------ |
    | Grupo de recursos           | **RG1**                                           |
    | Nombre                     | **app-vnet-firewall**                             |
    | SKU del firewall             | **Estándar**                                      |
    | Administración del firewall      | **Usar una directiva de firewall para administrar este firewall** |
    | Directiva de firewall          | Seleccione **Agregar nuevo**.                                |
    | Nombre de la directiva              | **fw-policy**                                     |
    | Region                   | **Este de EE. UU.**                                       |
    | Nivel de directiva              | **Estándar**                                      |
    | Elegir una red virtual | **Utilizar existente**                                  |
    | Red virtual          | **app-vnet** (RG1)                                |
    | Dirección IP pública        | Agregar nuevo: **fwpip**                                |

    [Obtenga más información sobre cómo crear un firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal).

1. Seleccione **Revisar y crear** y luego **Crear**.

### Actualización de la directiva de firewall

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba **Firewall**. Seleccione **Directivas de firewall** en los resultados de la búsqueda.

1. Seleccione **fw-policy**.

1. Seleccione **Application rules** (Reglas de aplicación).

1. Seleccione **+ Colección de reglas de aplicación**.

1. Use los valores de la tabla siguiente: Para cualquier propiedad que no se especifique, use el valor predeterminado.

    | Propiedad               | Valor                                     |
    | :--------------------- | :---------------------------------------- |
    | Nombre                   | **app-vnet-fw-rule-collection**           |
    | Tipo de colección de reglas   | **Aplicación**                           |
    | Prioridad               | **200**                                   |
    | Acción de colección de reglas | **Permitir**                                 |
    | Grupo de colección de reglas  | **DefaultApplicationRuleCollectionGroup** |

    1. En **reglas**, use los valores de la tabla siguiente:

        | Propiedad         | Valor                                  |
        | :--------------- | :------------------------------------- |
        | Nombre             | **AllowAzurePipelines**                |
        | Tipo de origen      | **Dirección IP**                         |
        | Source           | **10.1.0.0/23**                        |
        | Protocolo         | **https**                              |
        | Tipo de destino | FQDN                                   |
        | Destino      | **dev.azure.com, azure.microsoft.com** |

        y seleccione **Agregar**

> **Nota**: La regla **AllowAzurePipelines** permite que la aplicación web acceda a Azure Pipelines. La regla permite que la aplicación web acceda al servicio Azure DevOps y al sitio web de Azure.

1. Cree una **colección de reglas de red** que contenga una sola regla para las direcciones IP con los valores de la tabla siguiente. Para cualquier propiedad que no se especifique, use el valor predeterminado.

1. Seleccione **Reglas de red**.

1. Seleccione **+ Colección de reglas de red**.

1. Use los valores de la tabla siguiente: Para cualquier propiedad que no se especifique, use el valor predeterminado.

    | Propiedad               | Valor                                 |
    | :--------------------- | :------------------------------------ |
    | Nombre                   | **app-vnet-fw-nrc-dns**               |
    | Tipo de colección de reglas   | **Network**                           |
    | Prioridad               | **200**                               |
    | Acción de colección de reglas | **Permitir**                             |
    | Grupo de colección de reglas  | **DefaultNetworkRuleCollectionGroup** |

    1. En **reglas**, use los valores de la tabla siguiente:

        | Propiedad              | Valor                |
        | :-------------------- | :------------------- |
        | Regla                  | **AllowDns**         |
        | Source                | **10.1.0.0/23**      |
        | Protocolo              | **UDP**              |
        | Puertos de destino     | **53**               |
        | Direcciones de destino | **1.1.1.1, 1.0.0.1** |

        Y seleccione **Agregar**.

    Más información sobre la [creación de una regla de aplicación](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal#configure-an-application-rule) y la [creación de una regla de red](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal#configure-a-network-rule).

1. Para comprobar que el estado de aprovisionamiento de Azure Firewall y directiva de firewall se muestra **Correcto**.

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba **Firewall**. Seleccione **Firewall** en los resultados de la búsqueda.

1. Seleccione **app-vnet-firewall**.

1- Compruebe que el **estado de aprovisionamiento** es **Correcto**.

1- En el cuadro de búsqueda que aparece en la parte superior del portal, escriba **Directivas de firewall**. Seleccione **Directivas de firewall** en los resultados de la búsqueda.

1. Seleccione **fw-policy**.

1- Compruebe que el **estado de aprovisionamiento** es **Correcto**.

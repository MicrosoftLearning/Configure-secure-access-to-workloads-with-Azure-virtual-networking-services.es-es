---
lab:
  title: 'Ejercicio: Enrutamiento del tráfico al firewall'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Laboratorio: Enrutamiento del tráfico al firewall

## Escenario

Ahora que hay un firewall en vigor con directivas que aplican los requisitos de seguridad de las organizaciones, debe enrutar el tráfico de red a la subred del firewall para que pueda filtrar e inspeccionar el tráfico. Las tablas de rutas proporcionan control sobre el enrutamiento del tráfico de red hacia y desde la aplicación web. El tráfico está sujeto a las reglas de firewall cuando enruta el tráfico al firewall como puerta de enlace predeterminada de la subred.

### Diagrama de la arquitectura

![Diagrama en el que se muestra una red virtual con un firewall y una tabla de rutas.](../Media/task-3.png)

### Tareas de aptitudes

- Cree y configure una tabla de rutas.
- Vincule una tabla de rutas a una subred.
  
## Instrucciones del ejercicio

### Creación de una tabla de rutas

1. Registre la dirección IP privada y pública de **app-vnet-firewall**.

    1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba **Firewall**. Seleccione **Firewall** en los resultados de la búsqueda.

    1. Seleccione **app-vnet-firewall**.

    1. Seleccione **Información general**.

        1. Registre la **dirección IP privada**.

    1. En el panel Información general, seleccione **fwpip**.

    1. Registre la **dirección IP pública**.

1. En el cuadro de búsqueda, escriba **Tabla de rutas**. Cuando Tabla de enrutamiento aparezca en los resultados de la búsqueda, selecciónelo.

1. En la página Tabla de rutas, seleccione **+ Crear**.

1. En la pestaña **Datos básicos**, cree una tabla de rutas con la información de la tabla que aparece a continuación.

    | Propiedad       | Valor                        |
    | :------------- | :--------------------------- |
    | Suscripción   | **Seleccione la suscripción** |
    | Grupo de recursos | **RG1**                      |
    | Region         | **Este de EE. UU.**                  |
    | Nombre           | **app-vnet-firewall-rt**     |

1. Seleccione **Revisar y crear** y luego **Crear**.

    [Más información sobre la creación de tablas de rutas](https://docs.microsoft.com/azure/virtual-network/manage-route-table) y la [asociación de una tabla de rutas a una subred](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#associate-a-route-table-to-a-subnet).

### Asociación de la tabla de rutas a la subred

1. En el cuadro de búsqueda, escriba **Tabla de rutas**. y seleccione Tablas de rutas en los resultados de la búsqueda.

1. Seleccione **app-vnet-firewall-rt**.

1. Seleccione **Subredes**.

1. Seleccione **+ Asociar.**

1. En la página **Asociar subred**, escriba la información como se muestra en la tabla siguiente:

    | Propiedad        | Valor              |
    | :-------------- | :----------------- |
    | Red virtual | **app-vnet** (RG1) |
    | Subnet          | **Front-end**       |

1. Seleccione **Aceptar**.

1. Repita los pasos anteriores para asociar la tabla de rutas **app-vnet-firewall-rt** a la subred **backend** en **app-vnet**.

### Creación de una ruta en la tabla de rutas

1. En el cuadro de búsqueda, escriba **Tabla de rutas**. y seleccione Tablas de rutas en los resultados de la búsqueda.

1. Seleccione **app-vnet-firewall-rt**.

1. Seleccione **Rutas**.

1. Seleccione **+Agregar**.

1. En la página **Agregar ruta**, escriba la información como se muestra en la tabla siguiente:

    | Propiedad                            | Valor                                                   |
    | :---------------------------------- | :------------------------------------------------------ |
    | Nombre de ruta                          | **outbound-firewall**                                   |
    | Tipo de destino                    | **Direcciones IP**                                        |
    | Intervalos de direcciones IP de destino y CIDR | **0.0.0.0/0**                                           |
    | Tipo de próximo salto                       | **Aplicación virtual**                                   |
    | Siguiente dirección de salto                    | **dirección IP privada del firewall registrado anteriormente** |

1. Seleccione **Agregar**.

[Obtenga más información sobre la creación de rutas](https://docs.microsoft.com/azure/virtual-network/manage-route-table#add-a-route).

Ahora el tráfico saliente de la subred front-end y back-end se enrutará al firewall.

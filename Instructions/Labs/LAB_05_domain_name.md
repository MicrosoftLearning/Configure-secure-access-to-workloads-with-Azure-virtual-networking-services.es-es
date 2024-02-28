---
lab:
  title: 'Ejercicio: Registro y resolución interna de nombres de dominio'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Laboratorio: Registro y resolución interna de nombres de dominio

## Escenario

La organización requiere que las cargas de trabajo registren y resuelvan los nombres de dominio internamente en redes virtuales. Las máquinas virtuales de las redes virtuales pueden usar el nombre de dominio en lugar de las direcciones IP para la comunicación interna. En ese caso, los nombres de dominio se resolverán con una zona DNS privada mediante un vínculo de red virtual. 



### Diagrama de la arquitectura

![Diagrama de Azure DNS vinculado a una red virtual.](../Media/task-5.png)

### Tareas de aptitudes
- Crear y configurar una zona DNS privada. 
- Cree y configure registros DNS.
- Configure DNS en una red virtual.

## Instrucciones del ejercicio

### Crear una zona DNS privada

DNS privado de Azure proporciona un servicio DNS confiable y seguro para administrar y resolver los nombres de dominio en una red virtual sin necesidad de agregar una solución DNS personalizada. Al utilizar zonas DNS privadas, puede usar sus propios nombres de dominio personalizados en lugar de los nombres proporcionados por Azure que están disponibles actualmente.

1. En la barra de búsqueda del portal, escriba **Zonas dns privadas** en el cuadro de texto de búsqueda y seleccione Zonas DNS privadas en los resultados.

1. Seleccione **+ Create** (+ Crear).

1. En la pestaña **Datos básicos** de Crear zona DNS privada, escriba la información como se muestra en la tabla siguiente:

    | Propiedad | Valor    |
    |:---------|:---------|
    |Suscripción|**Seleccione la suscripción**|
    |Grupo de recursos|**RG1**|
    |Nombre|**contoso.com**|
    |Region|**Este de EE. UU.**|

1. Seleccione **Revisar y crear** y, luego, **Crear**.

### Cree un vínculo de red virtual para su zona de DNS privada

1. En la barra de búsqueda del portal, escriba **Zonas dns privadas** en el cuadro de texto de búsqueda y seleccione Zonas DNS privadas en los resultados.

1. Seleccione **contoso.com**.

1. Seleccione **+ Vínculo de la red virtual**.

1. Seleccione **+ Agregar**.

1. En la pestaña **Datos básicos** de Crear red virtual, escriba la información como se muestra en la tabla siguiente:

    | Propiedad | Valor    |
    |:---------|:---------|
    |Nombre del vínculo|**app-vnet-link**|
    |Red virtual|**app-vnet**|
    |Habilitación del registro automático|**Habilitado**|

1. Seleccione **Aceptar**.

### Creación de un conjunto de registros de DNS

1. En la barra de búsqueda del portal, escriba **Zonas dns privadas** en el cuadro de texto de búsqueda y seleccione Zonas DNS privadas en los resultados.

1. Seleccione **contoso.com**.

1. Seleccione **+ Conjunto de registros**.

1. En la pestaña **Datos básicos** de Crear conjunto de registros, escriba la información como se muestra en la tabla siguiente:

    | Propiedad | Valor    |
    |:---------|:---------|
    |Nombre|**backend**|
    |Tipo|**A**
          |
    |TTL|**1**|
    |Dirección IP|**10.1.1.4**|


1. Seleccione **Aceptar**.

1. Compruebe que **contoso.com** tenga un conjunto de registros denominado **back-end**.

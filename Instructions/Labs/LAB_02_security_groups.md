---
lab:
  title: 'Ejercicio: Control del tráfico hacia la aplicación web y desde ella'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Laboratorio: Control del tráfico hacia la aplicación web y desde ella

## Escenario

Su organización requiere el control del tráfico desde la aplicación web y hacia ella. Para mejorar aún más la seguridad de la aplicación web, se pueden configurar los grupos de seguridad de red y grupos de seguridad de aplicaciones (ASG). Los grupos de seguridad de red son una capa de seguridad que filtra el tráfico desde los recursos de Azure y hacia ellos, mientras que los grupos de seguridad de aplicaciones permiten la agrupación de recursos para administrarlos colectivamente. Estos grupos de seguridad proporcionan un control específico sobre el tráfico desde los componentes de la aplicación web y hacia ellos.

### Diagrama de la arquitectura

![Diagrama en el que se muestra un grupo de seguridad de red y un grupo de seguridad de aplicaciones en una red virtual.](../Media/task-2.png)

### Tareas de aptitudes

- Cree un grupo de seguridad de red.
- Cree reglas para los grupos de seguridad de red.
- Asocie un grupo de seguridad de red a una subred.
- Cree y use grupos de seguridad de aplicaciones en las reglas para los grupos de seguridad de red.

## Instrucciones del ejercicio

### Creación de grupos de seguridad de aplicaciones

Un grupo de seguridad de aplicaciones (ASG) permite agrupar servidores con funciones similares, como servidores web.

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba **Grupo de seguridad de aplicaciones**. Seleccione Grupos de seguridad de aplicaciones en los resultados de la búsqueda.

1. Seleccione **+ Create** (+ Crear).

    En la pestaña **Datos básicos** de Crear un grupo de seguridad, escriba la información tal y como aparece en la siguiente tabla:

    | Propiedad | Valor    |
    |:---------|:---------|
    |Suscripción|**Seleccione la suscripción**|
    |Grupo de recursos|**RG1**|
    |Nombre|**app-backend-asg**|
    |Region|**Este de EE. UU.**|

1. Seleccione **Revisar y crear** y, luego, **Crear**.

[Más información sobre la creación de un grupo de seguridad de aplicaciones](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-application-security-groups).

>**Nota**: Creará el grupo de seguridad de aplicaciones en la misma región de la red virtual existente.

### Cree la subred y asóciela con el grupo de seguridad de red.

Un grupo de seguridad de red (NSG) protege el tráfico de red de una red virtual. Un NSG contiene una lista de reglas de seguridad que permiten o deniegan el tráfico de red a los recursos conectados a Azure Virtual Network. Los grupos de seguridad de red se pueden asociar a subredes, o interfaces de red (NIC) individuales conectadas a máquinas virtuales.

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba **Grupo de seguridad de red**. En los resultados de la búsqueda, seleccione **Grupos de seguridad de red**.

1. Seleccione **+ Create** (+ Crear).

    En la pestaña **Datos básicos** de Crear un grupo de seguridad de red, escriba la información tal y como aparece en la siguiente tabla:

    | Propiedad | Valor    |
    |:---------|:---------|
    |Suscripción|**Seleccione la suscripción**|
    |Grupo de recursos|**RG1**|
    |Nombre|**app-vnet-nsg**|
    |Region|**Este de EE. UU.**|

    [Más información sobre la creación de un grupo de seguridad de red](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-a-network-security-group).

1. Seleccione **Revisar y crear** y, luego, **Crear**.

En esta sección, asociará el grupo de seguridad de red con la subred de la red virtual que creó anteriormente.

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba **Grupo de seguridad de red**. En los resultados de la búsqueda, seleccione Grupos de seguridad de red.

1. En la lista de grupos de seguridad de red, seleccione **app-vnet-nsg**.

1. Seleccione **Subredes** en la sección Configuración de **app-vnet-nsg**.

1. En la página Subredes, seleccione **+ Asociar**

1. En **Asociar subred**, seleccione **app-vnet (RG1)** en Red virtual. y seleccione **Backend** para Subred y, después, seleccione Aceptar.

    [Obtenga más información sobre cómo asociar un grupo de seguridad de red a una subred](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#associate-a-network-security-group-to-a-subnet).

### Creación de reglas de grupo de seguridad de red
Un grupo de seguridad de red (NSG) protege el tráfico de red de una red virtual.

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba **Grupo de seguridad de red**. En los resultados de la búsqueda, seleccione Grupos de seguridad de red.

1. En la lista de grupos de seguridad de red, seleccione **app-vnet-nsg**.

1. Seleccione **Reglas de seguridad entrantes** en la sección Configuración de **app-vnet-nsg**.

1. Seleccione **+Agregar**.

1. En la página **Agregar regla de seguridad entrante**, escriba la información como se muestra en la tabla siguiente:

    | Propiedad | Valor    |
    |:---------|:---------|
    |Origen|**Cualquiera**|
    |Rangos del puerto origen|**\***|
    |Destino|**Grupo de seguridad de aplicaciones**|
    |Grupo de seguridad de aplicación de destino|**app-backend-asg**|    
    |Servicio|**SSH**|
    |Acción|**Permitir**|
    |Prioridad|**100**|
    |Nombre|**AllowSSH**|

    [Más información sobre la creación de una regla para un grupo de seguridad de red](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-a-network-security-group).

### Implemente una plantilla de ARM mediante Cloud Shell para crear las máquinas virtuales necesarias para este ejercicio

1. Haga clic en el icono de la esquina superior derecha de Azure Portal para abrir **Azure Cloud Shell**.

1. Si se le pide que seleccione **Bash** o **PowerShell**, seleccione **PowerShell**.

    >**Nota**: Si es la primera vez que inicia **Cloud Shell** y aparece el mensaje **No tiene ningún almacenamiento montado**, seleccione la suscripción que utiliza en este laboratorio y seleccione **Crear almacenamiento**.

1. Implemente la siguiente plantilla de ARM mediante Cloud Shell para crear las máquinas virtuales necesarias para este ejercicio:

>**Nota**: Puede seleccionar el texto de la sección siguiente y copiarlo y pegarlo en Cloud Shell.

   ```powershell
   $RGName = "RG1"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateUri https://raw.githubusercontent.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/main/Instructions/Labs/azuredeploy.json
   ```
  
1. Para comprobar que las máquinas virtuales **VM1** y **VM2** se están ejecutando, vaya al grupo de recursos **RG1** y seleccione **VM1**.

1. Compruebe que el estado de la máquina virtual es **En ejecución**.

1. Repita el paso anterior para **VM2**.

### Asociación del grupo de seguridad de aplicaciones a la interfaz de red de la máquina virtual
Cuando se crearon las máquinas virtuales, Azure creó una interfaz de red para cada una y la asoció a estas.

Agregue la interfaz de red de cada máquina virtual a uno de los grupos de seguridad de aplicaciones que creó anteriormente:

1. En Azure Portal, vaya al grupo de recursos **RG1** y seleccione **VM2**.

1. Vaya a la pestaña Redes de la máquina virtual y seleccione **+ Agregar grupos de seguridad de aplicaciones** en la sección **Grupos de seguridad de aplicaciones**. 

1. Seleccione **app-backend-asg** en la lista de grupos de seguridad de aplicaciones.

1. Seleccione **Agregar**.

  [Obtenga más información sobre cómo agregar una NIC a un grupo de seguridad de aplicaciones](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface?tabs=azure-portal#add-or-remove-from-application-security-groups).


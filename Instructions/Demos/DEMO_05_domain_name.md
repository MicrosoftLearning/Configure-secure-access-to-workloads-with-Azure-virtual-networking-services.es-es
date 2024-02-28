---
demo:
  title: 'Demostración: Creación y configuración de Azure DNS'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demostración: Creación y configuración de Azure DNS

En esta demostración, se explorará Azure DNS.

[Tutorial: Hospedar el dominio y el subdominio: Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)


**Crear una zona DNS privada**

1. Acceda a Azure Portal.

1. Busque el servicio  **Zonas DNS**.

1. Cree una **Zona DNS** y explique el propósito de la zona. Para el nombre, puede usar contoso.internal.com.

1.  Espere a que se cree la zona DNS. Es posible que tenga que  **Actualizar** la página.

**Adición de un conjunto de registros de DNS**


[Tutorial: Creación de un registro de alias para hacer referencia a un registro de recursos de zona](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. Una vez creada la zona, seleccione  **+Conjunto de registros**.

1. Use la lista desplegable  **Tipo** para ver los distintos tipos de registros. Revise cómo se usan los distintos tipos de registros. Observe cómo cambia la información del registro a medida que selecciona los diferentes tipos de registros.

1. Cree un registro **A** como ejemplo. 

**Vinculación de red virtual para el registro automático**

1.  Una vez implementada la zona DNS, revise la página de información general con los alumnos.
1.  Vincule la zona DNS privada a una red virtual creando un vínculo de red virtual.
1.  En el panel izquierdo, selecciona Virtual network links (Vínculos de red virtual).
1.  Seleccione Agregar.
1.  Escribe myLink para el Nombre del vínculo.
1.  Como Red virtual, selecciona myAzureVNet.
1.  Selecciona la casilla Enable auto registration (Habilitar registro automático).
1.  Seleccione Aceptar.

>**Nota**: Los alumnos ahora deberían poder completar LAB_05
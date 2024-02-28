---
demo:
  title: 'Demostración: Crear y configurar enrutamiento de red'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demostración: Crear y configurar enrutamiento de red

En esta demostración, aprenderá a crear una tabla de rutas, a definir una ruta personalizada y a asociarla a una subred. 


**Nota:**  Para esta demostración se necesita una red virtual con al menos una subred.

[Enrutamiento del tráfico de red (tutorial): Azure Portal](https://learn.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#create-a-route-table)


### Creación de una tabla de rutas 

1. Como tiene tiempo, revise el diagrama del tutorial. Explique por qué necesita crear una ruta definida por el usuario. 

1. Acceda a Azure Portal.

1. Busque y seleccione **Tablas de rutas**. Analice cuándo se deben usar las **rutas de puerta de enlace propagación**. 

1. Cree una tabla de enrutamiento y explique cualquier configuración poco común. 

1. Espere a que se implemente la nueva tabla de rutas.

**Agregar una ruta**

1.  Seleccione la nueva tabla de enrutamiento y después  **Rutas**.

1.  Cree una **ruta**. Analice los diferentes **tipos de salto** que están disponibles. 

1.  Cree la ruta y espere a que el recurso se implemente.
 
### Asociación de una tabla de rutas a una subred
Una tabla de rutas se puede asociar a varias subredes o a ninguna. Las tablas de rutas no se asocian a las redes virtuales. Debe asociar una tabla de rutas a cada subred a la que desee asociar la tabla de rutas.


1.  Vaya a la subred que quiera asociar a la tabla de rutas.

1.  Seleccione **Tabla de enrutamiento* y elija la nueva tabla de enrutamiento. 

1.  **Guarde** los cambios.

 
>**Nota**: Solo puede asociar una tabla de rutas a las subredes de las redes virtuales que existen en la misma suscripción y ubicación de Azure de la tabla de rutas.

### Probar el firewall
Ahora, pruebe el firewall para confirmar que las directivas de enrutamiento y firewall funcionan según lo previsto. 

1.  Conecte un escritorio remoto a la dirección IP pública del firewall e inicie sesión en la máquina virtual Srv-Work.
2.  Abra Internet Explorer y vaya a https://www.google.com.
3.  Seleccione Aceptar > Cerrar en las alertas de seguridad de Internet Explorer. Debería ver la página principal de Google.
4.  Vaya a https://www.microsoft.com. El firewall debería bloquearle.

Con ello, ha comprobado que las reglas de firewall funcionan:
- Puede navegar al FQDN permitido pero no a ningún otro.
- Puede resolver nombres DNS con el servidor DNS externo configurado.
 
>**Nota**: Los alumnos ahora deberían poder completar LAB_03




>**Nota**: Los alumnos ahora deberían poder completar LAB_04
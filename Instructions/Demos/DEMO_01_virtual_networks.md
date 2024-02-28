---
demo:
  title: 'Demostración: Creación y configuración del emparejamiento de redes virtuales'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demostración: Creación y configuración del emparejamiento de redes virtuales


En esta demostración, creará redes virtuales.

**Nota:**  Puede usar los valores sugeridos para la configuración, o bien valores personalizados propios si lo prefiere.

**Nota:** Hay disponible una **[simulación de laboratorio interactiva de redes virtuales](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204?azure-portal=true)** y **[emparejamiento de redes](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209?azure-portal=true)** que le permiten recorrer un laboratorio similar si no puede realizar una demostración en vivo. Es posible que encuentre pequeñas diferencias entre la simulación interactiva y la demostración sugerida, pero las ideas y los conceptos básicos que se muestran son los mismos. 


[Inicio rápido: Creación de una red virtual mediante Azure Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

### Creación de una red virtual en el portal


   
1.  [Diapositiva auxiliar] Antes de comenzar la demostración, vamos a revisar qué son las redes virtuales y los conceptos clave de las redes virtuales de Azure. Use esta diapositiva para resaltar las funcionalidades de Azure Virtual Networks. Además, también puede destacar los conceptos y las prácticas recomendadas de Azure Virtual Network. A medida que muestra la creación de una red virtual, puede explicar los conceptos básicos del espacio de direcciones, las subredes, las regiones y las suscripciones. También puede discutir estas diapositivas al final y pasar directamente a la demostración.
   
2.  Inicie sesión en Azure Portal y busque  **Redes virtuales**.
   
3.  Cree una red virtual y explique la configuración básica a medida que avance. Asegúrese de que se crea al menos una subred. 
   
4.  Explique que Azure Portal proporciona una interfaz fácil de usar. Los elementos marcados con un asterisco rojo son obligatorios.
   
5.  [Diapositiva auxiliar] Seleccione la pestaña Seguridad. Use esta diapositiva para resaltar brevemente los servicios de seguridad, estos temas se tratarán con más detalle más adelante en el curso. Más información: Servicios que pueden implementarse en una red virtual. 
   
6.  [Diapositiva auxiliar] Seleccione la pestaña Direcciones IP. Use esta diapositiva para revisar: Planificar redes virtuales y subredes. Agregue o modifique una subred para demostrar a los alumnos cómo configurar subredes. 
7.  Asegúrese de que no haya errores de validación y, después, haga clic en Crear.
8.  Haga clic en Crear y espere a que la red virtual se implemente. Señale los mensajes de notificación. 
9.  Muestre cómo ir al recurso.
10. Repita el proceso de creación de otra red virtual para que pueda demostrar el emparejamiento de VNet.

## Configuración del emparejamiento de red virtual

**Nota:**  Para esta demostración necesitará dos redes virtuales.

[Tutorial sobre conexión de redes virtuales con emparejamiento de VNet](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**Configuración del emparejamiento de VNet en la primera red virtual**

1. En  **Azure Portal**, seleccione la primera red virtual. Revise el valor del emparejamiento. 

1. En  **Configuración**, seleccione  **Emparejamientos** y **+ Agregar** un nuevo emparejamiento.

1. Configure el emparejamiento de la segunda red virtual. Use los iconos de información para revisar la configuración diferente. 

1. Una vez completado el emparejamiento, revise el **Estado de emparejamiento**. 

**Confirmación del emparejamiento de VNet en la segunda red virtual**

1. En  **Azure Portal**, seleccione la segunda red virtual.

1. En  **Configuración**, seleccione  **Emparejamientos**.

1. Observe que se ha creado un emparejamiento de forma automática. Observe que el  **Estado de emparejamiento** es  **Conectado**.


>**Nota**: Los alumnos ahora deberían poder completar LAB_01


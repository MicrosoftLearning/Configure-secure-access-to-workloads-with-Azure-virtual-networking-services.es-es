---
demo:
  title: 'Demostración: Crear y configurar grupos de seguridad de red'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demostración: Crear y configurar grupos de seguridad de red


En esta demostración, exploraremos los grupos de seguridad. 

**Nota:** Hay disponible una **[simulación de laboratorio interactiva de redes virtuales](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013?azure-portal=true)** que le permite recorrer un laboratorio similar si no puede realizar una demostración en vivo. Es posible que encuentre pequeñas diferencias entre la simulación interactiva y la demostración sugerida, pero las ideas y los conceptos básicos que se muestran son los mismos. 

[Restricción del acceso a los recursos de PaaS (tutorial): Azure Portal](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)

### Crear un grupo de seguridad de red

1. Acceda a Azure Portal.

1. Busque y seleccione  **Grupos de seguridad de red**.

1. [Diapositiva auxiliar] Cree un grupo de seguridad de red para explicar la configuración a medida que avance. 
 
1. Espere a que se implemente el nuevo NSG.

**Exploración de reglas de entrada y salida**

1. Seleccione el nuevo grupo de seguridad de red.

1. [Diapositiva auxiliar] Analice cómo se puede asociar el grupo de seguridad de red a subredes o interfaces de red.

1. Analice el propósito de las reglas de entrada y salida.  

1. Revise las reglas de entrada y salida predeterminadas. 

1. Cree una regla para explicar la configuración a medida que avance. En concreto, analice la selección del servicio (como HTTPS) y la configuración de prioridad. 
 

### Crear ASG
 
1. [Diapositiva auxiliar] Busque y seleccione  **Grupos de seguridad de red**.

1. Cree un ASG para explicar la configuración a medida que avance. 
 
1. Espere a que se implemente el nuevo ASG.

1. Analice cómo se puede asociar el ASG con reglas de NSG.


### Asocie los NSG 
1.  Vaya al NSG creado
1.  Seleccione Subredes en la sección Configuración.
1.  En la página Subredes, seleccione +Asociar
1.  En Asociar subred, seleccione su red virtual.


>**Nota**: Los alumnos ahora deberían poder completar LAB_02


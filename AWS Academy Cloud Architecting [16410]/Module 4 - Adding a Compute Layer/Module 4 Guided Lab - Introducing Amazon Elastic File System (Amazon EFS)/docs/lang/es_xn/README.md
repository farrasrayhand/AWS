# Módulo 4 - Laboratorio guiado: Introducción a Amazon Elastic File System (Amazon EFS)
[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: SPL-151"

## Información general y objetivos del laboratorio
En este laboratorio, se presenta Amazon Elastic File System (Amazon EFS) con la consola de administración de AWS.

Después de completar este laboratorio, debería ser capaz de lo siguiente:

- Iniciar sesión en la consola de administración de AWS

- Crear un sistema de archivos de Amazon EFS

- Iniciar sesión en una instancia de Amazon Elastic Compute Cloud (Amazon EC2) que ejecuta Amazon Linux

- Montar el sistema de archivos en su instancia EC2

- Analizar y monitorear el rendimiento del sistema de archivos

<br/>
## Duración

El tiempo estimado para completar este laboratorio es de **20 minutos**.

<br/>
## Restricciones de los servicios de AWS

En este entorno de laboratorio, el acceso a los servicios y las acciones de los servicios de AWS puede restringirse a los que se necesiten para cumplir las instrucciones del laboratorio. Es posible que se produzcan errores si intenta acceder a otros servicios o hacer acciones que no sean las que se describen en este laboratorio.

<br/>

## Acceso a la consola de administración de AWS

1. En la parte superior de estas instrucciones, elija <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratorio) y comience a trabajar en él.

   Se abrirá el panel **Start Lab** (Iniciar laboratorio), donde se muestra el estado del laboratorio.

   <i class="fas fa-info-circle"></i> **Sugerencia**: Si necesita más tiempo para completar el laboratorio, vuelva a hacer clic en el botón <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratorio) para reiniciar el temporizador del entorno.

2. Espere hasta que el panel **Start Lab** (Iniciar laboratorio) muestre el mensaje *Lab Status: ready (Estado del laboratorio: listo)* y, luego, cierre el panel haciendo clic en la **X**.

3. En la parte superior de estas instrucciones, elija <span id="ssb_voc_grey">AWS</span>.

   Con esta acción, se abrirá la consola de administración de AWS en una nueva pestaña del navegador, y el sistema iniciará su sesión de forma automática.

   <i class="fas fa-exclamation-triangle"></i> **Sugerencia**: Si no se abre una nueva pestaña del navegador, por lo general habrá un aviso o un icono en la parte superior, el cual indicará que el navegador impide que el sitio abra ventanas emergentes. Haga clic en el aviso o el icono y elija **Allow pop ups** (Permitir ventanas emergentes).

4. Ubique la pestaña de la **consola de administración de AWS** de modo que aparezca junto con estas instrucciones. El método más óptimo sería tener ambas pestañas del navegador abiertas al mismo tiempo para que pueda seguir los pasos del laboratorio más fácilmente.

   <i class="fas fa-exclamation-triangle"></i> **No cambie la región a menos que se le indique expresamente que debe hacerlo**.

<br/>

## Tarea 1: Configurar un grupo de seguridad para acceder a su sistema de archivos de EFS

El grupo de seguridad que asocia a un destino de montaje *debe permitir el acceso de entrada para TCP en el puerto 2049 para Network File System (NFS)*. Este es el grupo de seguridad que ahora creará, configurará y asociará a los destinos de montaje de EFS.


5. En la **consola de administración de AWS**, en el menú <span id="ssb_services">Services</span> (Servicios), elija **EC2**.

6. En el panel de navegación de la izquierda, elija **Security Groups** (Grupos de seguridad).

7. Copie el **ID del grupo de seguridad** del grupo de seguridad *EFSClient* en el editor de texto.

   El ID del grupo debería ser similar a *sg-03727965651b6659b*.

8. Elija <span id="ssb_orange">Create security group</span> (Crear grupo de seguridad) y, a continuación, configure lo siguiente:

   <a id='securitygroup'></a>

   * **Security group name** (Nombre del grupo de seguridad): `EFS Mount Target (Destino de montaje de EFS)`
   * **Description** (Descripción): `Inbound NFS access from EFS clients (Acceso de NFS de entrada de clientes EFS)`
   * **VPC:** *Lab VPC*

9. En la sección **Inbound rules** (Reglas de entrada), elija <span id="ssb_white">Add rule</span> (Agregar regla) y, a continuación, configure lo siguiente:

   * **Type** (Tipo): *NFS*
   * **Source** (Origen):
      * *Custom (Personalizar)*
      * En el cuadro *Custom (Personalizar)*, pegue el **ID del grupo de seguridad** del grupo de seguridad que copió en el editor de texto
   * Elija <span id="ssb_orange">Create security group</span> (Crear grupo de seguridad).

<br/>

## Tarea 2: Crear un sistema de archivos de EFS

Los sistemas de archivos de EFS se pueden montar en varias instancias EC2 que se ejecutan en diferentes zonas de disponibilidad en la misma región. Estas instancias utilizan *destinos de montaje* que se crean en cada *zona de disponibilidad* para montar el sistema de archivos con la semántica estándar de NFSv4.1. Puede montar el sistema de archivos en instancias en una sola nube privada virtual (VPC) a la vez. Tanto el sistema de archivos como la VPC deben estar en la misma región.


10. En el menú <span id="ssb_services">Services</span> (Servicios), elija **EFS**.

11. Elija <span id="ssb_orange">Create File System</span> (Crear sistema de archivos)

12. En la ventana **Create file system** (Crear sistema de archivos), elija <span id="ssb_white">Customize</span> (Personalizar)

13. En el **paso 1**:

    - Desactive <i class="far fa-square"></i> Enable automatic backups (Habilitar copias de seguridad automáticas).
    - **Lifecycle management** (Administración del ciclo de vida): Seleccione *None (Ninguna)*
    - En la sección **Tags** (Etiquetas), configure:
       - **Key** (Clave): `Name (Nombre)`
       - **Value** (Valor): `My First EFS File System (Mi primer sistema de archivos EFS)`

14. Elija <span id="ssb_orange">Next</span> (Siguiente)

15. En **VPC**, seleccione *Lab VPC*.

16. Seleccione la casilla de verificación <i class="fas fa-times"></i> de cada grupo de seguridad predeterminado para desconectar el grupo de seguridad predeterminado de cada destino de montaje de la *zona de disponibilidad*.

17. Asocie el grupo de seguridad del **destino de montaje de EFS** a cada destino de montaje de la *zona de disponibilidad* con los siguientes pasos:

    * Seleccione cada casilla de verificación **Security groups** (Grupos de seguridad).
    * Elija **EFS Mount Target** (Destino de montaje de EFS).

    Se crea un destino de montaje para cada subred.

    Sus destinos de montaje deberían ser similares al siguiente ejemplo. El diagrama muestra dos destinos de montaje en la **Lab VPC** que utilizan el grupo de seguridad **EFS Mount Target** (Destino de montaje de EFS). En este laboratorio, debe usar la Lab VPC.

    <img src="images/mount-targets-security-groups.png" alt="Grupos de seguridad de destino" width="600" >

18. Elija <span id="ssb_orange">Next</span> (Siguiente)

19. En el **paso 3**, elija <span id="ssb_orange">Next</span> (Siguiente)

20. En el **paso 4:**

    * Revise la configuración.
    * Elija <span id="ssb_orange">Create</span> (Crear)

    <i class="far fa-thumbs-up"></i> ¡Felicitaciones! Ha creado un nuevo sistema de archivos de EFS en su Lab VPC y destinos de montaje en cada subred de Lab VPC. En unos segundos, el **File system state** (Estado del sistema de archivos) del sistema de archivos cambiará a *Available (Disponible)*, y 2 o 3 minutos más tarde lo harán los destinos de montaje.

    Continúe con el siguiente paso después de que el **Mount target state** (Estado del destino de montaje) de cada destino de montaje cambie a *Available (Disponible)*. 2 o 3 minutos más tarde, elija el botón de actualización de la pantalla para comprobar su progreso.

<br/>

## Tarea 3: Conectarse a su instancia EC2 mediante SSH

En esta tarea, se conectará a la instancia EC2 mediante Secure Shell (SSH).

### <i class="fab fa-windows"></i> Usuarios de Microsoft Windows

<i class="fas fa-comment"></i> Estas instrucciones se dirigen específicamente a usuarios de Microsoft Windows. Si utiliza macOS o Linux,<a href="#ssh-MACLinux">pase a la siguiente sección</a>.


21. Encima de estas instrucciones, elija el menú desplegable <span id="ssb_voc_grey">Details</span> (Detalles) y, a continuación, seleccione <span id="ssb_voc_grey">Show</span> (Mostrar).

    Aparece la ventana **Credentials** (Credenciales).

22. Haga clic en el botón **Download PPK** (Descargar PPK) y guarde el archivo **labsuser.ppk**.

    **Nota:** Por lo general, el navegador guarda el archivo en el directorio **Downloads** (Descargas).

23. Tome nota de la dirección **EC2Publicip** si se muestra.

24. Para salir del panel **Details** (Detalles), haga clic en la **X**.

25. Para acceder mediante SSH a la instancia EC2, debe utilizar ***\*PuTTY\****. Si no tiene instalado PuTTY en su equipo,<a href="https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe">descárguelo</a>.

26. Abra **putty.exe**.

27. Configure el tiempo de espera de PuTTY de manera que se mantenga la sesión abierta durante un periodo más prolongado:

    * Elija **Connection** (Conexión).
    * **Seconds between keepalives** (Segundos entre señales de conexión persistente): `30`

28. Configure la sesión de PuTTY con los siguientes ajustes:

    * Elija **Session** (Sesión).
    * **Host Name** (or IP address) (Nombre de alojamiento [o dirección IP]): pegue el valor de **EC2PublicIP** de la instancia que anotó antes
       * Como alternativa, vuelva a la consola de Amazon EC2 y elija **Instances** (Instancias).
       * Seleccione la instancia a la que desea conectarse.
       * En la pestaña *Description (Descripción)*, copie el valor **IPv4 Public IP** (IP pública IPv4).
    * De vuelta en PuTTY, en la lista **Connection** (Conexión), expanda <i class="far fa-plus-square"></i> **SSH**
    * Elija **Auth** (Autenticación) (pero no lo amplíe).
    * Elija **Browse** (Examinar)
    * Busque el archivo *labsuser.ppk* que descargó, selecciónelo y elija **Open** (Abrir)
    * Elija **Open** (Abrir) nuevamente


29. Para validar el alojamiento y conectarse a él, elija **Yes** (Sí).

30. Cuando aparezca **login as** (Iniciar sesión como), escriba `ec2-user`.

    Esta acción lo conecta a la instancia EC2.

31. Si es usuario de Microsoft Windows:<a href="#ssh-after">Haga clic aquí para pasar a la siguiente tarea.</a>



<a id='ssh-MACLinux'></a>

### Si es usuario de macOS <span style="font-size: 30px; color: #808080;"><i class="fab fa-apple"></i></span> y Linux <span style="font-size: 30px; "><i class="fab fa-linux"></i></span>

Estas instrucciones se dirigen específicamente a usuarios de macOS o Linux. Si es usuario de Windows,<a href="#ssh-after">pase a la siguiente tarea.</a>

32. Encima de estas instrucciones, elija el menú desplegable <span id="ssb_voc_grey">Details</span> (Detalles) y, a continuación, seleccione <span id="ssb_voc_grey">Show</span> (Mostrar).

    Aparece la ventana **Credentials** (Credenciales).

33. Haga clic en el botón **Download PEM** (Descargar PEM) y guarde el archivo **labsuser.pem**.

34. Tome nota de la dirección **EC2Publicip** si se muestra.

35. Para salir del panel **Details** (Detalles), haga clic en la **X**.

36. Abra una ventana de terminal y cambie el directorio por aquel en el que se descargó el archivo *labsuser.pem* con el comando `cd`.

    Por ejemplo, si guardó el archivo *labuser.pem* en el directorio **Downloads** (Descargas), ejecute este comando:

    ```bash
    cd ~/Downloads
    ```

37. Ejecute este comando para cambiar los permisos de la clave a fin de que sean de solo lectura:

    ```bash
    chmod 400 labsuser.pem
    ```

38. Ejecute el siguiente comando (reemplace **<public-ip\>** por la dirección **EC2PublicIP** que copió anteriormente).

    * Como alternativa, para encontrar la dirección IP de la instancia en las instalaciones, vuelva a la consola de Amazon EC2 y seleccione **Instances** (Instancias).
    * Seleccione la instancia a la que desea conectarse.
    * En la pestaña **Description** (Descripción), copie el valor **IPv4 Public IP** (IP pública IPv4).

    ```bash
    ssh -i labsuser.pem ec2-user@<public-ip>
    ```

39. Cuando se le pregunte si desea permitir la primera conexión a este servidor SSH remoto, escriba `yes (sí)`.

    Como está usando un par de claves para la autenticación, no se le pide una contraseña.

<a id='ssh-after'></a>

<br/>
## Tarea 4: Crear un directorio nuevo y montar el sistema de archivos de EFS

<i class="fas fa-info-circle" aria-hidden="true"></i> Amazon EFS admite los protocolos NFSv4.1 y NFSv4.0 cuando monta los sistemas de archivos en instancias EC2. Aunque NFSv4.0 es compatible, le recomendamos que utilice NFSv4.1. Cuando monte el sistema de archivos de EFS en la instancia EC2, también debe utilizar un cliente de NFS que admita el protocolo NFSv4 elegido. La instancia EC2 que se lanzó como parte de este laboratorio incluye un cliente de NFSv4.1, que ya está instalado en ella.


40. En el menú <span id="ssb_services">Services</span> (Servicios) que se encuentra en la **consola de administración de AWS**, haga clic en **EFS**.

41. Elija **My First EFS File System**.

42. En la **consola de Amazon EFS**, en la esquina superior derecha de la página, elija <span id="ssb_orange">Attach</span> (Asociar) para abrir las instrucciones de montaje de Amazon EC2.

43. En la sesión SSH, siga las instrucciones de la sección **To set up your EC2 instance** (Para configurar la instancia EC2). Copie y pegue (o escriba manualmente) el comando para instalar las utilidades necesarias.

    El comando debería ser similar al siguiente ejemplo:

    ```
    sudo yum install -y amazon-efs-utils
    ```

    *Sugerencia:* Para pegarlo en el terminal del navegador, coloque el cursor justo a la derecha del símbolo del sistema y haga clic con el botón derecho para ver la opción de pegar.

44. Desplácese hacia abajo hasta la sección **Mounting your file system** (Montaje del sistema de archivos).

45. En su sesión SSH, escriba `sudo mkdir efs` para crear un directorio nuevo

46. Copie el comando completo en la sección **Using the NFS client** (Uso del cliente de NFS).

    El montaje debería ser similar al siguiente ejemplo:

    `sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-bce57914.efs.us-west-2.amazonaws.com:/ efs`

    <i class="fas fa-comment" aria-hidden="true"></i> El comando proporcionado `sudo mount…` utiliza las opciones de montaje predeterminadas de Linux.

47. En su sesión SSH de Linux, monte el sistema de archivos de Amazon EFS de la siguiente manera:

    * Pegue el comando.
    * Presione INTRO.


48. Obtenga un resumen completo del uso de espacio en disco disponible y utilizado. Para ello, escriba:

    `sudo df -hT`

    La siguiente captura de pantalla es un ejemplo del resultado del siguiente comando del *sistema de archivos del disco*:

    `df -hT`

    Observe el *tipo* y el *tamaño* del sistema de archivos de EFS montado.

    <img src="images/disk-space.png" alt="espacio en disco" width="600">


<br/>
## Tarea 5: Examinar el rendimiento del nuevo sistema de archivos de EFS


### Examinar el rendimiento con Flexible I/O

<i class="fas fa-info-circle"></i> Flexible I/O (fio) es una utilidad sintética de punto de referencia de E/S para Linux. Se utiliza para comparar y probar subsistemas de E/S de Linux. Durante el arranque, *fio* se instaló automáticamente en la instancia EC2.

49. Para examinar las características de rendimiento de escritura del sistema de archivos escriba:

    ```
    sudo fio --name=fio-efs --filesize=10G --filename=./efs/fio-efs-test.img --bs=1M --nrfiles=1 --direct=1 --sync=0 --rw=write --iodepth=200 --ioengine=libaio
    ```

    <i class="fas fa-comment"></i> El comando `fio` tardará entre 5 y 10 minutos en completarse. El resultado debe verse como el ejemplo de la siguiente captura de pantalla. Asegúrese de examinar el resultado del comando `fio`, en especial la información de estado de resumen para esta prueba de escritura.

    <img src="images/fio.png" alt="fio" width="600" >

<br/>
### Monitorear el rendimiento con Amazon CloudWatch

50. En la **consola de administración de AWS**, en el menú <span id="ssb_services">Services</span> (Servicios), elija **CloudWatch**.

51. En el panel de navegación de la izquierda, elija **Metrics** (Métricas).

52. En la pestaña **All metrics** (Todas las métricas), elija **EFS**.

53. Elija **File System Metrics** (Métricas del sistema de archivos).

54. Seleccione la fila que tiene el nombre de métrica **PermittedThroughput** (Rendimiento permitido).

    <i class="fas fa-comment"></i> Es posible que deba esperar de 2 a 3 minutos y actualizar la pantalla varias veces antes de que todas las métricas disponibles, incluida **PermittedThroughput** (Rendimiento permitido), se calculen y rellenen.

55. En el gráfico, elija y arrastre la línea de datos. Si no ve el gráfico de líneas, ajuste el intervalo de tiempo del gráfico para que se muestre el periodo durante el cual ejecutó el comando `fio`.

    <img src="images/graph.png" alt="elegir arrastrar" width="600" >


56. Detenga el puntero en la línea de datos del gráfico. El valor debe ser *105M*.

    <img src="images/throughput.png" alt="Rendimiento" width="600" >

    El rendimiento de Amazon EFS aumenta a medida que crece el sistema de archivos. Las cargas de trabajo basadas en archivos suelen tener picos. Impulsan altos niveles de rendimiento durante periodos cortos y bajos niveles de rendimiento el resto del tiempo. Debido a este comportamiento, Amazon EFS está diseñado para ampliar a niveles elevados de rendimiento durante ciertos periodos. Todos los sistemas de archivos, independientemente del tamaño, pueden aumentar el rendimiento a 100 MiB/s. Para obtener más información acerca de las características de rendimiento del sistema de archivos de EFS, consulte la<a href="http://docs.aws.amazon.com/efs/latest/ug/performance.html" target="_blank">documentación de Amazon Elastic File System</a>.

57. En la pestaña **All metrics** (Todas las métricas)** *desmarque* la casilla **PermittedThroughput** (Rendimiento permitido).

58. Seleccione la casilla de verificación **DataWriteIOBytes** (Bytes de E/S de escritura de datos).

    <i class="fas fa-comment"></i> Si no ve *DataWriteIOBytes (Bytes de E/S de escritura de datos)* en la lista de métricas, utilice la búsqueda **File System Metrics** (Métricas del sistema de archivos) para encontrarla.

59. Elija la pestaña **Graphed metrics** (Métricas diagramadas).

60. En la columna **Statistics** (Estadísticas), seleccione **Sum** (Suma).

61. En la columna **Period** (Periodo), seleccione **1 Minute** (1 minuto).

62. Detenga el puntero en el pico del gráfico de líneas. Tome este número (en bytes) y divídalo por la duración en segundos (60 segundos). El resultado le da el rendimiento de escritura (B/s) de su sistema de archivos durante la prueba.

    <img src="images/Sum-1-minute.png" alt="Suma 1 minuto" width="600" >

    El rendimiento que está disponible para un sistema de archivos aumenta a medida que crece un sistema de archivos. Todos los sistemas de archivos ofrecen un rendimiento de referencia constante de 50 MiB/s por TiB de almacenamiento. Además, todos los sistemas de archivos (independientemente del tamaño) pueden aumentarlo a 100 MiB/s. Los sistemas de archivos que son mayores que 1 TB pueden aumentarlo 100 MiB/s por TiB de almacenamiento. A medida que agrega datos al sistema de archivos, su rendimiento máximo disponible aumenta de forma lineal y automática con el almacenamiento.

    El rendimiento del sistema de archivos se comparte entre todas las instancias EC2 conectadas a un sistema de archivos. Para obtener más información acerca de las características de rendimiento del sistema de archivos de EFS, consulte la <a href="http://docs.aws.amazon.com/efs/latest/ug/performance.html" target="_blank">documentación de Amazon Elastic File System</a>.

    <i class="far fa-thumbs-up" style="color:blue"></i> ¡Felicitaciones! Creó un sistema de archivos de EFS, lo montó en una instancia EC2 y ejecutó una prueba de referencia de E/S para examinar sus características de rendimiento.


<br/>
## Envío de su trabajo

63. En la parte superior de estas instrucciones, elija <span id="ssb_blue">Submit</span> (Enviar) para registrar su progreso y, cuando se le indique, elija **Yes** (Sí).

64. Si los resultados no se muestran después de algunos minutos, vuelva a la parte superior de estas instrucciones y elija <span id="ssb_voc_grey">Grades</span> (Resultados).

    **Sugerencia**: Puede enviar su trabajo varias veces. Después de realizar las modificaciones pertinentes, vuelva a elegir **Submit** (Enviar). Su último envío es el que se registrará para este laboratorio.

65. Para encontrar comentarios detallados sobre su trabajo, elija <span id="ssb_voc_grey">Details</span> (Detalles) seguido de <i class="fas fa-caret-right"></i> **View Submission Report** (Ver informe de envío).


<br/>
## Fin del laboratorio <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> ¡Felicitaciones! Ha completado el laboratorio.


66. Elija <span id="ssb_voc_grey">End Lab</span> (Finalizar laboratorio), en la parte superior de esta página, y, a continuación, seleccione <span id="ssb_blue">Yes</span> (Sí) para confirmar que desea finalizar el laboratorio.

    Debería aparecer un panel con el siguiente mensaje: *DELETE has been initiated… You may close this message box now (Se ha iniciado la ELIMINACIÓN… Ya puede cerrar este cuadro de mensaje)*.

67. Seleccione la **X** de la esquina superior derecha para cerrar el panel.





*© 2020 Amazon Web Services, Inc. y sus empresas afiliadas. Todos los derechos reservados. Este contenido no puede reproducirse ni redistribuirse, total ni parcialmente, sin el permiso previo por escrito de Amazon Web Services, Inc. Queda prohibida la copia, el préstamo y la venta de carácter comercial.*
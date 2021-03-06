<header>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.5.0/css/all.css" integrity="sha384-B4dIYHKNBt8Bc12p+WXckhzcICo0wtJAoU8YZTY5qE0Id1GSseTk6S+L3BlXeVIU" crossorigin="anonymous">
    <!-- Latest compiled and minified CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <!-- Optional theme -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">
    <!-- Latest compiled and minified JavaScript -->
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
 </header>
 <!--include:Logo-->
 <style type="text/css">
    body {
    font-family:  "Roboto", "Helvetica", sans-serif;
    font-size: 12pt;
    font-color: Gray;
    line-height: 1.6;
    margin: 50px;
    }
    p {
    list-style-position: inside;
    }
    #ssb_blue {
    background-color: #257ACF;
    font-weight: bold;
    font-size: 90%;
    color: white;
    border-radius: 5px;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    white-space: nowrap;
    }
    #ssb_s3_blue {
    background-color: #329ad6;
    font-weight: bold;
    font-size: 90%;
    color: white;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    }
    #ssb_voc_grey {
    background-color: #F2F3F4;
    font-weight: normal;
    font-size: 90%;
    color: black;
    border-radius: 3px;
    border: 1px solid gray;
    padding-top: 5px;
    padding-bottom: 5px;
    padding-left: 6px;
    padding-right: 6px;
    white-space: nowrap;
    }
    #ssb_grey {
    background-color: #DEDEDE;
    font-weight: bold;
    font-size: 90%;
    color: #444;
    position: relative;
    top:-1px;
    border-radius: 5px;
    border-width: 1px;
    border-style: solid;
    border-color: #444;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    white-space: nowrap;
    }
    #ssb_white {
    background-color: white;
    font-weight: bold;
    font-size: 90%;
    color: #545b64;
    position: relative;
    top: -1px;
    border-color: #545b64;
    border-radius: 2px;
    border-width: 1px;
    border-style: solid;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    }
    #ssl_alexa_ocean {
    color: #00a0d2;
    font-weight: bold;
    }
    #ssb_services {
    background-color: #232f3e;
    font-weight: bold;
    font-size: 90%;
    color: white;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    }
    #ssb_orange {
    background-color:#ec7211;
    font-weight:bold;
    font-size:90%;
    color:white;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    white-space:
    nowrap;
    }
    #ssbox_cloudformation_blue {
    font-weight:bold;
    background-color:#f1faff;
    font-size:90%;
    border-color:#00A1C9;
    border-width:1px;
    border-style:solid;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    }
    #ssb_ssm_white {
    background-color:white;
    font-weight:bold;
    font-size:90%;
    color:#545b64;
    border-color:#545b64;
    border-radius:2px;
    border-width:1px;
    border-style:solid;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    }
 </style>


# Laboratorio 6: Escalado y balanceo de carga para la arquitectura

<!-- Note to translators: This is based on Technical Essentials Lab 3. Copy the translation from there. Do not re-translate the whole document. -->

&nbsp;&nbsp;

**Versi??n 4.6.6 (TESS3) + cambio personalizado**

Este laboratorio le explica c??mo usar los servicios de Elastic Load Balancing (ELB) y Auto Scaling para balancear la carga y escalar la infraestructura autom??ticamente.

**Elastic Load Balancing** distribuye autom??ticamente el tr??fico entrante de las aplicaciones entre varias instancias de Amazon EC2 Adem??s, le permite obtener tolerancia a errores en las aplicaciones, ya que proporciona de forma constante la capacidad de balanceo de carga necesaria para dirigir el tr??fico de estas.

**Auto Scaling** permite mantener la disponibilidad de las aplicaciones y aumentar o reducir autom??ticamente la capacidad de Amazon EC2 seg??n las condiciones que se definan. Puede utilizar Auto Scaling para asegurarse de que se ejecuta la cantidad deseada de instancias de Amazon EC2. Con Auto Scaling, tambi??n se puede aumentar autom??ticamente la cantidad de instancias de Amazon EC2 durante los picos de demanda para mantener el rendimiento y reducir la capacidad durante los per??odos de baja demanda con el objeto de minimizar los costos. Auto Scaling es adecuado para aplicaciones con patrones de demanda estables o para aquellas cuyo uso var??a cada hora, d??a o semana.

&nbsp;

**Objetivos**

Despu??s de completar este laboratorio, podr?? hacer lo siguiente:

- Crear una imagen de Amazon Machine (AMI) a partir de una instancia en ejecuci??n
- Crear un balanceador de carga
- Crear una configuraci??n de lanzamiento y un grupo de Auto Scaling
- Escalar autom??ticamente instancias nuevas dentro de una subred privada
- Crear alarmas de Amazon CloudWatch y monitorear el rendimiento de su infraestructura

&nbsp;

**Duraci??n**

La duraci??n estimada de este laboratorio es de **30 minutos** aproximadamente.

&nbsp;

**Situaci??n**

Comenzar?? con la siguiente infraestructura:

<img src="images/starting-architecture.png" alt="Arquitectura inicial" width="800">

&nbsp;

El estado final de la infraestructura es el siguiente:

&nbsp;

<img src="images/final-architecture.png" alt="Arquitectura final" width="800">

&nbsp;

&nbsp;
___
## Acceso a la consola de administraci??n de AWS

1. En la parte superior de estas instrucciones, haga clic en <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratorio) para lanzar su laboratorio.

   Se abrir?? el panel ???Start Lab??? (Iniciar laboratorio), donde se muestra el estado del laboratorio.

2. Espere hasta que aparezca el mensaje ???**Lab status: in creation**??? (Estado del laboratorio: en creaci??n) y, a continuaci??n, haga clic en la **X** para cerrar el panel ???Start Lab (Iniciar laboratorio)???.

   **Nota**: Es posible que el cambio del estado del laboratorio a ???Listo??? tarde 10??minutos o m??s.

3. En la parte superior de estas instrucciones, haga clic en <span id="ssb_voc_grey">AWS</span>.

   La consola de administraci??n de AWS se abrir?? en una nueva pesta??a del navegador. El sistema iniciar?? su sesi??n autom??ticamente.

   **Sugerencia**: Si no se abre una pesta??a nueva del navegador, deber??a aparecer un banner o un icono en la parte superior de este, el cual indique que el navegador no permite que se abran ventanas emergentes en el sitio. Haga clic en el banner o en el icono, y elija ???Allow pop ups??? (Permitir ventanas emergentes).

4. Ubique la pesta??a de la consola de administraci??n de AWS en un lugar donde aparezca al lado de estas instrucciones. Idealmente, deber??a poder ver ambas pesta??as del navegador al mismo tiempo para que sea m??s sencillo seguir los pasos del laboratorio.

&nbsp;
___
## Tarea??1: crear una AMI para Auto Scaling

En esta tarea, crear?? una AMI a partir del _Servidor web??1_ existente. Esto le permitir?? guardar el contenido del disco de arranque para que se puedan lanzar instancias nuevas con contenido id??ntico.

5. En la **consola de administraci??n de AWS**, en el men?? <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Servicios), haga clic en **EC2**.

6. En el panel de navegaci??n de la izquierda, haga clic en **Instances** (Instancias).

   Primero, confirmar?? que la instancia est?? en ejecuci??n.

7. Espere hasta que en **Status Checks** (Comprobaciones de estado) del **Servidor web??1** se muestre el mensaje *2/2 checks passed* (2/2 comprobaciones aprobadas). Haga clic en ???Refresh??? (Actualizar) <i class="fas fa-sync"></i> para la actualizaci??n.

   Ahora, crear?? una AMI basada en esta instancia.

8. Seleccione <i class="far fa-check-square"></i> **Servidor web??1**.

9. En el men?? <span id="ssb_grey">Actions<i class="fas fa-angle-down"></i></span> (Acciones), haga clic en **Image** (Imagen) y en **Create Image** (Crear imagen) y, a continuaci??n, configure lo siguiente:

   - **Image name** (Nombre de la imagen): `Web Server AMI (AMI del servidor web)`
   - **Image description** (Descripci??n de la imagen): `Lab AMI for Web Server (AMI de laboratorio para servidor web)`

10. Haga clic en <span id="ssb_blue">Create Image</span> (Crear imagen).

   En la pantalla de confirmaci??n, se muestra **AMI ID** (ID de la AMI) de la AMI nueva.

11. Haga clic en <span id="ssb_blue">Close</span> (Cerrar).

   Utilizar?? esta AMI cuando lance el grupo de Auto Scaling m??s adelante en el laboratorio.

&nbsp;
___
## Tarea??2: crear un balanceador de carga

En esta tarea, crear?? un balanceador de carga para balancear el tr??fico en varias instancias EC2 y zonas de disponibilidad.

12. En el panel de navegaci??n izquierdo, haga clic en **Load Balancers** (Balanceadores de carga).

13. Haga clic en <span id="ssb_blue">Create Load Balancer</span> (Crear balanceador de carga).

   A continuaci??n, se muestran diferentes tipos de balanceadores de carga. Utilizar?? un _balanceador de carga de aplicaciones_ que funciona a nivel de solicitud (capa??7). Este dirige el tr??fico a los objetivos (instancias EC2, contenedores, direcciones IP y funciones de Lambda) seg??n el contenido de la solicitud. Para obtener m??s informaci??n, consulte: <a href="https://aws.amazon.com/elasticloadbalancing/features/#compare" target="_blank">Comparaci??n de balanceadores de carga</a>

14. En **Application Load Balancer** (Balanceador de carga de aplicaciones), haga clic en <span id="ssb_blue">Create</span> (Crear) y configure lo siguiente:

   - **Name** (Nombre): `LabELB`
   - **VPC:** _Lab VPC_ (en la secci??n **Availability Zones** [Zonas de disponibilidad])
   - **Availability Zones:** (Zonas de disponibilidad): seleccione <i class="far fa-check-square"></i> ambas para ver las subredes disponibles.
   - Seleccione **Public Subnet??1** (Subred p??blica??1) y **Public Subnet 2** (Subred p??blica??2).

   Esto configura el balanceador de carga para que funcione en varias zonas de disponibilidad.

15. Haga clic en <span id="ssb_grey">Next: Configure Security Settings</span> (Siguiente: definir la configuraci??n de seguridad).

   <i class="fas fa-comment"></i> Puede ignorar la advertencia _???Improve your load balancer's security???_ (Mejore la seguridad del balanceador de carga).

16. Haga clic en <span id="ssb_grey">Next: Configure Security Groups</span> (Siguiente: configurar grupos de seguridad).

   Ya se cre?? un _Grupo de seguridad web_ que permite acceder mediante HTTP.

17. Seleccione <i class="far fa-check-square"></i> **Web Security Group** (Grupo de seguridad web) y anule la selecci??n de <i class="far fa-square"></i> **predeterminada**.

18. Haga clic en <span id="ssb_grey">Next: Configure Routing</span> (Siguiente: configurar el direccionamiento).

   El direccionamiento configura el destino de las solicitudes enviadas al balanceador de carga. Crear?? un _grupo de destino_ que Auto Scaling utilizar??.

19. En **Name** (Nombre), escriba: `LabGroup`.

20. Haga clic en <span id="ssb_grey">Next: Register Targets</span> (Siguiente: registrar objetivos).

   M??s adelante en este laboratorio, Auto Scaling registrar?? autom??ticamente las instancias como objetivos.

21. Haga clic en <span id="ssb_grey">Next: Review</span> (Siguiente: revisar).

22. Haga clic en <span id="ssb_blue">Create</span> (Crear) y, posteriormente, en <span id="ssb_grey">Close</span> (Cerrar).

   En el balanceador de carga, se mostrar?? el estado de _aprovisionamiento_. No es necesario esperar hasta que est?? listo. Contin??e con la siguiente tarea.

&nbsp;
___
## Tarea??3: crear una configuraci??n de lanzamiento y un grupo de Auto Scaling

En esta tarea, crear?? una _configuraci??n de lanzamiento_ para el grupo de Auto Scaling. Una configuraci??n de lanzamiento es una plantilla utilizada por un grupo de Auto Scaling para lanzar instancias EC2. Cuando se crea una configuraci??n de lanzamiento, se especifica la informaci??n de las instancias, como la AMI, el tipo de instancia, un par de claves, un grupo de seguridad y los discos.

23. En el panel de navegaci??n de la izquierda, haga clic en **Launch Configurations** (Configuraciones de lanzamiento).

24. Haga clic en <span id="ssb_orange">Create launch configuration</span> (Crear configuraci??n de lanzamiento).

25. Configure los siguientes ajustes:

   - **Launch configuration name**  (Nombre de configuraci??n de lanzamiento): `LabConfig`

   - **Imagen de Amazon Machine (AMI)** Elija *Web Server AMI*

   - **Instance type** (Tipo de instancia):

      - Elija <span id="ssb_white">Choose instance type</span> (Elegir un tipo de instancia)
      - Seleccione *t3.micro*
      - Elija <span id="ssb_orange">Choose</span> (Elegir)

      **Nota:** Si ha iniciado el laboratorio en la regi??n us-este--1, seleccione el tipo de instancia **t2.micro** . Para encontrar la regi??n, busque en la esquina superior derecha de la consola de Amazon EC2.

      **Nota:** Si recibe el mensaje de error ???Something went wrong. Please refresh and try again??? (Algo sali?? mal. Actualice e int??ntelo de nuevo), puede ignorarlo y continuar con el ejercicio.

   - **Configuraci??n adicional**

      - **Monitoring** (Monitoreo): </i> seleccione <i class="far fa-check-square"></i> *Enable EC2 instance detailed monitoring within CloudWatch (Habilitar monitoreo detallado de instancia EC2 dentro de CloudWatch)*

      Esto permite que Auto Scaling reaccione r??pidamente a los cambios en la utilizaci??n.

26. En **Security groups** (Grupos de seguridad), establecer?? la configuraci??n de lanzamiento para utilizar el _Grupo de seguridad web_ que ya se cre?? para usted.

   - Elija **Select an existing security group** (seleccione un grupo de seguridad existente)
   - Seleccione <i class="far fa-check-square"></i> **Grupo de seguridad web**

27. En **Key pair** (Par de claves) configure:

   - **Opciones de par de claves:** *Choose an existing key pair (Elegir un par de claves existente)*
   - **Par de claves existente:** vockey
   - Seleccione <i class="far fa-check-square"></i> **I acknowledge...** (Acepto...)
   - Haga clic en <span id="ssb_orange">Create launch configuration</span> (Crear configuraci??n de lanzamiento).

   Ahora, crear?? un grupo de Auto Scaling que utilice esta configuraci??n de lanzamiento.

28. Seleccione la casilla de verificaci??n para *LabConfig* Launch Configuration.

29. En el men?? <span id="ssb_white">Actions<i class="fas fa-caret-down"></i></span> (Acciones), elija el *Create Auto Scaling group* (Crear grupo de Auto Scaling)

30. Escriba el nombre del grupo de Auto??Scaling:

   - **Name** (Nombre): `Lab Auto Scaling Group (Grupo de Auto Scaling del laboratorio)`

31. Elija <span id="ssb_orange">Next</span> (Siguiente)

32. En la p??gina **Network** (Red), configure

   - **Network** (Red): _Lab VPC_

      <i class="fas fa-comment"></i> Puede ignorar el mensaje ???No public IP address??? (No hay una direcci??n IP p??blica).

   - **Subnet** (Subred): seleccione _Private Subnet??1 (10.0.1.0/24)_ **(Subred privada??1 [10.0.1.0/24]) y** _Private Subnet??2 (10.0.3.0/24) (Subred privada??2 [10.0.3.0/24])_

   Esto lanzar?? las instancias EC2 en subredes privadas en ambas zonas de disponibilidad.

33. Elija <span id="ssb_orange">Next</span> (Siguiente)

34. En **Load balancing** (Equilibrio de cargas):

   - Seleccione <i class="far fa-check-square"></i> **Enable load balancing** (Habilitar el equilibrio de carga)
   - Elija el **Application Load Balancer** (Equilibrador de carga de aplicaciones) o **Network Load Balancer** (Equilibrador de carga de red)
   - **Elija un grupo de destino para su equilibrador de carga:** LabGroup

35. En **Additional settings** (Configuraci??n adicional), seleccione <i class="far fa-check-square"></i> **Activar la recopilaci??n de m??tricas de grupo en CloudWatch**

   De este modo, se capturar??n m??tricas en intervalos de 1??minuto, lo que permite que Auto Scaling reaccione r??pidamente a los patrones cambiantes de uso.

36. Elija <span id="ssb_orange">Next</span> (Siguiente)

37. En **Group Size** (Tama??o de grupo), configure lo siguiente:

   - **Desired capacity** (Capacidad deseada): 2
   - **Minimum capacity** (Capacidad m??nima): 2
   - **Maximum capacity** (Capacidad m??xima): 6

   Esto permite que Auto Scaling agregue o elimine instancias autom??ticamente, y siempre mantenga entre 2 y 6 instancias en ejecuci??n.

38. En **Scaling policies** (Pol??ticas de escalado), elija *Target tracking scaling policy (Pol??ticas de escalado de seguimiento de destino)* y configure:

   - **Lab policy name** (Nombre de directiva de laboratorio): `LabScalingPolicy`
   - **Metric type** (Tipo de m??trica): _Average CPU Utilization_ (Uso promedio de CPU)
   - **Target value** (Valor objetivo): `60`

   De este modo, se le indica a Auto Scaling que se debe mantener un uso _promedio_ de la CPU del 60??% _en todas las instancias_. Auto Scaling aumenta o reduce autom??ticamente la capacidad seg??n sea necesario para mantener la m??trica en el valor objetivo especificado o en un valor pr??ximo. Se ajusta a las fluctuaciones de la m??trica debido a un patr??n de carga fluctuante.

39. Elija <span id="ssb_orange">Next</span> (Siguiente)

   Auto Scaling puede enviar una notificaci??n cuando se produce un evento de escalado. Deber?? utilizar los valores predeterminados.

40. Elija <span id="ssb_orange">Next</span> (Siguiente)

   Las etiquetas que se apliquen al grupo de Auto Scaling se propagar??n autom??ticamente a las instancias que se lancen.

41. Elija <span id="ssb_orange">Add tag</span> (Agregar etiqueta) y configure lo siguiente:

   - **Key** (Clave): `Name (Nombre)`
   - **Value** (Valor): `Lab Instance` (Instancia de laboratorio)

42. Haga clic en <span id="ssb_orange">Next</span> (Siguiente).

43. Revise los detalles del grupo de Auto Scaling y, a continuaci??n, haga clic en <span id="ssb_orange">Create Auto Scaling group</span> (Crear grupo de Auto Scaling). Si aparece el mensaje de error **Failed to create Auto Scaling group** (No se pudo crear el grupo de Auto Scaling), haga clic en <span id="ssb_blue">Retry Failed Tasks</span> (Reintentar tareas con errores).

   En el grupo de Auto Scaling, al principio, se mostrar?? un recuento de instancias de cero, pero se lanzar??n instancias nuevas para alcanzar el recuento **esperado** de 2??instancias.

&nbsp;
___
## Tarea??4: verificar el funcionamiento del balanceo de carga

En esta tarea, comprobar?? el funcionamiento correcto del balanceo de carga.

44. En el panel de navegaci??n de la izquierda, haga clic en **Instances** (Instancias).

   Deber??a ver dos instancias nuevas denominadas **Lab Instance** (Instancia de laboratorio). Estas fueron lanzadas por Auto Scaling.

   <i class="fas fa-comment"></i> Si no se muestran las instancias o los nombres, espere 30 segundos y haga clic en ???Refresh??? (Actualizar) <i class="fas fa-sync"></i> en la esquina superior derecha.

   Primero, aseg??rese de que las nuevas instancias hayan superado la comprobaci??n de estado.

45. En el panel de navegaci??n izquierdo, haga clic en **Target Groups** (Grupos de destino) de la secci??n _Load Balancing_ (Balanceo de carga).

46. Elegir *LabGroup*

47. Haga clic en la pesta??a **Targets** (Destinos).

   A continuaci??n, se mostrar??n dos objetivos de instancias **Lab Instance** (Instancia de laboratorio) para este grupo de destino.

48. Espere a que el **Estado** de ambas instancias cambie a *healthy* (buen estado). Haga clic en ???Refresh??? <i class="fas fa-sync"></i> (Actualizar), en la esquina superior derecha para verificar si hay actualizaciones.

   Si el estado de una instancia es _Healthy_ (buen estado), significa que ha superado la comprobaci??n de estado del balanceador de carga. Esto quiere decir que el balanceador de carga enviar?? tr??fico a la instancia.

   Ahora, puede acceder al grupo de Auto Scaling mediante el balanceador de carga.

49. En el panel de navegaci??n izquierdo, haga clic en **Load Balancers** (Balanceadores de carga).

50. En el panel inferior, copie el valor de **DNS name** (nombre DNS) del balanceador de carga y aseg??rese de omitir ???(A Record)??? (Registro A).

   Deber??a ser similar a _LabELB-1998580470.us-west-2.elb.amazonaws.com_.

51. Abra una nueva pesta??a en el navegador web, pegue el nombre DNS que acaba de copiar y presione ???Enter??? (Intro).

   La aplicaci??n debe aparecer en el navegador. Esto indica que el balanceador de carga recibi?? la solicitud, la envi?? a una de las instancias EC2 y arroj?? el resultado.

&nbsp;
___
## Tarea??5: realizar pruebas de Auto Scaling

Ha creado un grupo de Auto Scaling con un m??nimo de dos instancias y un m??ximo de seis. Actualmente, hay dos instancias en ejecuci??n porque ese es el tama??o m??nimo, y el grupo no est?? sujeto a una carga. Ahora, aumentar?? la carga para que Auto Scaling agregue instancias adicionales.

52. Regrese a la consola de administraci??n de AWS, pero no cierre la pesta??a de la aplicaci??n, ya que pronto tendr?? que volver a ella.

53. En el men?? <span id="ssb_services">Services <i class="fas fa-angle-down"></i></span> (Servicios), haga clic en **CloudWatch**.

54. En el panel de navegaci??n izquierdo, haga clic en **Alarms** (Alarmas) (*no* en **ALARMA**).

   Se mostrar??n dos alarmas. El grupo de Auto Scaling cre?? estas alarmas autom??ticamente. De la misma manera, dichas alarmas mantendr??n la carga promedio de la CPU cerca del 60??% y, al mismo tiempo, respetar??n la limitaci??n de tener entre dos y seis instancias.

   ???    <i class="fas fa-exclamation-triangle" style="color:red"></i> **Nota**: Siga estos pasos solo si no ve las alarmas en 60??segundos.

   - En el men?? <span id="ssb_services">Services (Servicios), <i class="fas fa-angle-down"></i></span> haga clic en **EC2**.
   - En el panel de navegaci??n izquierdo, haga clic en **Auto Scaling Groups** (Grupos de Auto Scaling) y, a continuaci??n, en **Scaling Policies** (Pol??ticas de escalado).
   - Haga clic en <span id="ssb_grey">Actions</span> (Acciones) y en **Edit** (Editar).
   - Cambie el **valor objetivo** a `50`.
   - Haga clic en <span id="ssb_blue">Save</span> (Guardar).
   - En el men?? <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Servicios), haga clic en **CloudWatch**.
   - En el panel de navegaci??n izquierdo, haga clic en **Alarms** (Alarmas) (*no* en **ALARMA**) y verifique que ve dos alarmas.



55. Haga clic en la alarma **OK**, que en su nombre contiene _AlarmHigh_ (Alarma Alta).

   <i class="fas fa-comment"></i> Si en ninguna alarma aparece **OK**, espere un minuto y haga clic en ???Refresh??? <i class="fas fa-sync"></i> (Actualizar), en la esquina superior derecha, hasta que cambie el estado de la alarma.

   **OK** indica que la alarma _no_ se ha activado. Se trata de la alarma para **CPU Utilization > 60** (Uso de CPU > 60), que agrega instancias cuando el uso promedio de CPU es alto. En este momento, el gr??fico deber??a mostrar niveles muy bajos de uso de CPU.

   Ahora, le indicar?? a la aplicaci??n que realice c??lculos que deber??an aumentar el nivel de uso de CPU.

56. Regrese a la pesta??a del navegador donde est?? la aplicaci??n web.

57. Haga clic en **Load Test** (Prueba de carga) junto al logotipo de AWS.

   Esto har?? que la aplicaci??n genere cargas elevadas. La p??gina del navegador se actualizar?? autom??ticamente y todas las instancias del grupo de Auto Scaling producir??n carga. No cierre esta pesta??a.

58. Regrese a la pesta??a del navegador con la consola de **CloudWatch**.

   En menos de 5??minutos, la alarma **AlarmLow** (Alarma baja) deber??a cambiar a **OK** y el estado de la alarma **AlarmHigh** (Alarma alta) deber??a cambiar a *ALARM* (ALARMA).

   <i class="fas fa-comment"></i> Puede hacer clic en ???Refresh??? (Actualizar) <i class="fas fa-sync"></i> en la parte superior derecha cada 60??segundos para actualizar la pantalla.

   En el gr??fico **AlarmHigh** (Alarma alta), deber??a indicarse un aumento del porcentaje de utilizaci??n de la CPU. Cuando se atraviese la l??nea de 60??% por m??s de 3??minutos, Auto Scaling agregar?? m??s instancias.

59. Espere hasta que la alarma **AlarmHigh** (Alarma alta) se encuentre en estado _ALARM_ (ALARMA).

   Ahora puede ver las instancias adicionales que se lanzaron.

60. En el men?? <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Servicios),  haga clic en **EC2**.

61. En el panel de navegaci??n de la izquierda, haga clic en **Instances** (Instancias).

   Deber??a haber m??s de dos instancias etiquetadas como **Lab Instance** (Instancia de laboratorio) en ejecuci??n en este momento. Las nuevas instancias se crearon con Auto Scaling como respuesta a la alarma.

&nbsp;
___
## Tarea??6: terminar el servidor web??1

En esta tarea, terminar?? el _Web Server 1_ (Servidor web??1). Esta instancia se utiliz?? para crear la AMI que us?? el grupo de Auto Scaling, pero ya no se la necesita.

62. Seleccione <i class="far fa-check-square"></i> **Web Server 1** (Servidor web??1) (y aseg??rese de que sea la ??nica instancia seleccionada).

63. En el men?? <span id="ssb_grey">Actions <i class="fas fa-angle-down"></i></span> (Acciones), haga clic en **Instance State** (Estado de la instancia) > **Terminate** (Terminar).

64. Elija <span id="ssb_blue">Yes, Terminate</span> (S??, terminar).

&nbsp;
___
## Fin del laboratorio

<i class="icon-flag-checkered"></i> ??Felicitaciones! Ha completado el laboratorio.

65. Haga clic en <span id="ssb_voc_grey">End Lab</span> (Finalizar laboratorio) en la parte superior de esta p??gina y, a continuaci??n, en <span id="ssb_blue">Yes</span> (S??) para confirmar que desea finalizar el laboratorio.

   Aparecer?? un panel en el que se indica: ???DELETE has been initiated??? You may close this message box now???. (Se ha iniciado la ELIMINACI??N??? Ya puede cerrar este cuadro de mensajes).

66. Haga clic en la **X** de la esquina superior derecha para cerrar el panel.

   Env??enos sus comentarios, sugerencias o correcciones por email a *aws-course-feedback@amazon.com*

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
  #ssl_alexa_ocean {
    color: #00a0d2;
    font-weight: bold;
  }
</style>

# Actividad: AWS Elastic Beanstalk

<!-- Note to translators: This activity is unique to this course. -->

&nbsp;
&nbsp;
## Información general

Esta actividad le proporciona una cuenta de Amazon Web Services (AWS) en la que se ha creado, con anterioridad, un entorno de AWS Elastic Beanstalk para usted. Implementará código en él y observará los recursos de AWS que componen el entorno de Elastic Beanstalk.

&nbsp;

### Duración

La duración estimada de esta actividad es de **30 minutos** aproximadamente.

&nbsp;
&nbsp;
## Acceso a la consola de administración de AWS

1. En la parte superior de estas instrucciones, haga clic en <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratorio) para lanzarlo.

   Se abrirá el panel **Start Lab** (Iniciar laboratorio), donde se muestra el estado del laboratorio.

2. Espere hasta que aparezca el mensaje *Lab status: in creation* (Estado del laboratorio: creándose). Para cerrar el panel **Start Lab** (Iniciar laboratorio), haga clic en **X**.

3. En la parte superior de estas instrucciones, haga clic en <span id="ssb_voc_grey">AWS</span>.

   La consola de administración de AWS se abrirá en una pestaña nueva del navegador. El sistema iniciará su sesión de forma automática.

   **Sugerencia**: si no se abre una pestaña del navegador nueva, debería aparecer un banner o un icono en la parte superior de este, el cual indique que el navegador no permite que se abran ventanas emergentes en el sitio web. Haga clic en el banner o en el icono, y elija **Allow pop ups** (Permitir ventanas emergentes).

4. Ubique la pestaña de la **consola de administración de AWS** en un lugar donde aparezca al lado de estas instrucciones. Idealmente, debería poder ver ambas pestañas del navegador al mismo tiempo para que sea más sencillo seguir los pasos de la actividad.

&nbsp;
&nbsp;
## Tarea 1: acceder al entorno de Elastic Beanstalk

5. En la **consola de administración de AWS**, encontrará el menú **Services** (Servicios), donde debe elegir **Elastic Beanstalk**.

   Se debería abrir una página con el título **All Applications** (Todas las aplicaciones), en la cual se muestre un cuadro verde con los detalles de una aplicación existente de Elastic Beanstalk.

   **Nota**: Si el cuadro no es verde, es porque aún no ha completado el inicio. Luego de esperar unos instantes, el cuadro debería cambiar a *Health status: Green* (Estado: verde).

   <img src="images/application.png" alt="elastic-beanstalk-app" width="600" style="border:1px solid black">

6. En el cuadro verde de detalles de la aplicación, haga clic en el valor de la dirección URL que se muestra.

   Se abrirá la página **Dashboard** (Panel) del entorno de Elastic Beanstalk.

7. Observe que la página muestra que el estado de la aplicación es verde (bueno).

   El entorno de Elastic Beanstalk está listo para alojar una aplicación. Sin embargo, aún no tiene código en ejecución.

8. Cerca de la parte superior de la página, haga clic en la dirección URL (que termina en *elasticbeanstalk.com*).

   Cuando haga clic en la dirección URL, se abrirá una nueva pestaña del navegador. Sin embargo, debería ver que muestra el mensaje *“HTTP Status 404 - Not Found”* (Estado HTTP 404: no encontrado). *Este comportamiento es esperable* debido a que este servidor de aplicaciones todavía no tiene una aplicación ejecutándose en él. Vuelva a la consola de Elastic Beanstalk.

   En el siguiente paso, implementará código en su entorno de Elastic Beanstalk.

&nbsp;
&nbsp;
## Tarea 2: implementar una aplicación de muestra en Elastic Beanstalk

9. Para descargar una aplicación de muestra, haga clic en el siguiente enlace:
https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/tomcat.zip

<!--the zip file is linked in this documentation page: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-getstarted.html-->

10. Otra vez en la consola de Elastic Beanstalk, haga clic en **Upload and Deploy** (Cargar e implementar).

11. Haga clic en **Browse or Choose File** (Buscar o elegir un archivo) y, a continuación, busque y abra el archivo **java-tomcat-v3.zip** que acaba de descargar.

12. Haga clic en **Deploy** (Implementar).

    Elastic Beanstalk tardará uno o dos minutos en actualizar el entorno e implementar la aplicación.

    **Nota**: Si en la página del panel de Elastic Beanstalk aparece una advertencia, la cual indica que se debe integrar un perfil de instancia con el servicio AWS X-Ray, puede ignorarla.

13. Una vez completada la implementación, haga clic en el valor de la dirección URL situado cerca de la parte superior de la pantalla. O bien, si todavía tiene abierta la pestaña del navegador que muestra el estado 404, actualice dicha página.

    A continuación, se muestra la aplicación web implementada.

    <img src="images/web-app.png" alt="web-app" width="600" style="border:1px solid black">

    ¡Felicitaciones! Ha implementado correctamente una aplicación en Elastic Beanstalk.

14. Vuelva a la consola de Elastic Beanstalk y haga clic en **Configuration** (Configuración).

    Observe los detalles aquí.

    Por ejemplo, en la fila **Instances** (Instancias), se indica el tipo de instancia, el intervalo de monitoreo y los detalles del grupo de seguridad de las instancias de Amazon Elastic Compute Cloud (Amazon EC2) que alojan su aplicación web.

15. Desplácese hasta la parte inferior de la página para encontrar la fila **Database** (Base de datos).

    La fila **Database** (Base de datos) no tiene ninguna información porque el entorno no incluye una base de datos.

16. En la fila **Database** (Base de datos), haga clic en **Modify** (Modificar).

    Tenga en cuenta que podría agregar fácilmente una base de datos a este entorno si así lo deseara; solo tendría que establecer algunas configuraciones básicas y hacer clic en **Apply** (Aplicar). (Sin embargo, a los efectos de esta actividad, no es necesario agregar una base de datos).

17. Haga clic en **Monitoring** (Monitoreo).

    Examine los gráficos para ver los tipos de información que tiene a su disposición.

&nbsp;
&nbsp;
## Tarea 3: explorar los recursos de AWS que admiten su aplicación

18. En el menú **Services** (Servicios), elija **EC2**.

19. Haga clic en **Instances** (Instancias).

    Tenga en cuenta que hay dos instancias en ejecución y que ambas incluyen *samp* en sus nombres. Ambas instancias admiten su aplicación web.

20. Si desea continuar aprendiendo acerca de los recursos del servicio Amazon EC2 que se crearon con Elastic Beanstalk, no dude en explorarlos. Encontrará lo siguiente:

    - Un *grupo de seguridad* con el puerto 80 abierto
    - Un *balanceador de carga* al que pertenecen ambas instancias
    - Un *grupo de Auto Scaling* que ejecuta de dos a seis instancias, según la carga de la red

    Aunque Elastic Beanstalk se haya encargado de crear estos recursos, usted puede acceder a ellos igualmente.

&nbsp;
&nbsp;
## Fin de la actividad

<i class="icon-flag-checkered"></i> ¡Felicitaciones! Ha completado la actividad.

21. En la parte superior de esta página, haga clic en <span id="ssb_voc_grey">End Lab</span> (Finalizar laboratorio) y, a continuación, en <span id="ssb_blue">Yes</span> (Sí) para confirmar que desea finalizar la actividad.

    Aparecerá un panel con un mensaje que indica: *DELETE has been initiated… You may close this message box now* (Se ha iniciado la ELIMINACIÓN… Ya puede cerrar este cuadro de mensajes).

22. Para cerrar el panel, vaya a la esquina superior derecha y haga clic en **X**.

Para hacer comentarios, sugerencias o correcciones, envíe un email a *aws-course-feedback@amazon.com*

&nbsp;
&nbsp;

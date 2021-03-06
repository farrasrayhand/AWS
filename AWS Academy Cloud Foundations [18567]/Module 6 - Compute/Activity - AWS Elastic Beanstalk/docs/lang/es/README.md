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
## Informaci??n general

Esta actividad le proporciona una cuenta de Amazon Web Services (AWS) en la que se ha creado, con anterioridad, un entorno de AWS Elastic Beanstalk para usted. Implementar?? c??digo en ??l y observar?? los recursos de AWS que componen el entorno de Elastic Beanstalk.

&nbsp;

### Duraci??n

La duraci??n estimada de esta actividad es de **30??minutos** aproximadamente.

&nbsp;
&nbsp;
## Acceso a la consola de administraci??n de AWS

1. En la parte superior de estas instrucciones, haga clic en <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratorio) para lanzarlo.

   Se abrir?? el panel **Start Lab** (Iniciar laboratorio), donde se muestra el estado del laboratorio.

2. Espere hasta que aparezca el mensaje *Lab status: in creation* (Estado del laboratorio: cre??ndose). Para cerrar el panel **Start Lab** (Iniciar laboratorio), haga clic en **X**.

3. En la parte superior de estas instrucciones, haga clic en <span id="ssb_voc_grey">AWS</span>.

   La consola de administraci??n de AWS se abrir?? en una pesta??a nueva del navegador. El sistema iniciar?? su sesi??n de forma autom??tica.

   **Sugerencia**: si no se abre una pesta??a del navegador nueva, deber??a aparecer un banner o un icono en la parte superior de este, el cual indique que el navegador no permite que se abran ventanas emergentes en el sitio web. Haga clic en el banner o en el icono, y elija **Allow pop ups** (Permitir ventanas emergentes).

4. Ubique la pesta??a de la **consola de administraci??n de AWS** en un lugar donde aparezca al lado de estas instrucciones. Idealmente, deber??a poder ver ambas pesta??as del navegador al mismo tiempo para que sea m??s sencillo seguir los pasos de la actividad.

&nbsp;
&nbsp;
## Tarea??1: acceder al entorno de Elastic Beanstalk

5. En la **consola de administraci??n de AWS**, encontrar?? el men?? **Services** (Servicios), donde debe elegir **Elastic Beanstalk**.

   Se deber??a abrir una p??gina con el t??tulo **All Applications** (Todas las aplicaciones), en la cual se muestre un cuadro verde con los detalles de una aplicaci??n existente de Elastic Beanstalk.

   **Nota**: Si el cuadro no es verde, es porque a??n no ha completado el inicio. Luego de esperar unos instantes, el cuadro deber??a cambiar a *Health status: Green* (Estado: verde).

   <img src="images/application.png" alt="elastic-beanstalk-app" width="600" style="border:1px solid black">

6. En el cuadro verde de detalles de la aplicaci??n, haga clic en el valor de la direcci??n URL que se muestra.

   Se abrir?? la p??gina **Dashboard** (Panel) del entorno de Elastic Beanstalk.

7. Observe que la p??gina muestra que el estado de la aplicaci??n es verde (bueno).

   El entorno de Elastic Beanstalk est?? listo para alojar una aplicaci??n. Sin embargo, a??n no tiene c??digo en ejecuci??n.

8. Cerca de la parte superior de la p??gina, haga clic en la direcci??n URL (que termina en *elasticbeanstalk.com*).

   Cuando haga clic en la direcci??n URL, se abrir?? una nueva pesta??a del navegador. Sin embargo, deber??a ver que muestra el mensaje *???HTTP Status 404 - Not Found???* (Estado??HTTP??404: no encontrado). *Este comportamiento es esperable* debido a que este servidor de aplicaciones todav??a no tiene una aplicaci??n ejecut??ndose en ??l. Vuelva a la consola de Elastic Beanstalk.

   En el siguiente paso, implementar?? c??digo en su entorno de Elastic Beanstalk.

&nbsp;
&nbsp;
## Tarea??2: implementar una aplicaci??n de muestra en Elastic Beanstalk

9. Para descargar una aplicaci??n de muestra, haga clic en el siguiente enlace:
https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/tomcat.zip

<!--the zip file is linked in this documentation page: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-getstarted.html-->

10. Otra vez en la consola de Elastic Beanstalk, haga clic en **Upload and Deploy** (Cargar e implementar).

11. Haga clic en **Browse or Choose File** (Buscar o elegir un archivo) y, a continuaci??n, busque y abra el archivo **java-tomcat-v3.zip** que acaba de descargar.

12. Haga clic en **Deploy** (Implementar).

    Elastic Beanstalk tardar?? uno o dos minutos en actualizar el entorno e implementar la aplicaci??n.

    **Nota**: Si en la p??gina del panel de Elastic Beanstalk aparece una advertencia, la cual indica que se debe integrar un perfil de instancia con el servicio AWS X-Ray, puede ignorarla.

13. Una vez completada la implementaci??n, haga clic en el valor de la direcci??n URL situado cerca de la parte superior de la pantalla. O bien, si todav??a tiene abierta la pesta??a del navegador que muestra el estado??404, actualice dicha p??gina.

    A continuaci??n, se muestra la aplicaci??n web implementada.

    <img src="images/web-app.png" alt="web-app" width="600" style="border:1px solid black">

    ??Felicitaciones! Ha implementado correctamente una aplicaci??n en Elastic Beanstalk.

14. Vuelva a la consola de Elastic Beanstalk y haga clic en **Configuration** (Configuraci??n).

    Observe los detalles aqu??.

    Por ejemplo, en la fila **Instances** (Instancias), se indica el tipo de instancia, el intervalo de monitoreo y los detalles del grupo de seguridad de las instancias de Amazon Elastic Compute Cloud (Amazon??EC2) que alojan su aplicaci??n web.

15. Despl??cese hasta la parte inferior de la p??gina para encontrar la fila **Database** (Base de datos).

    La fila **Database** (Base de datos) no tiene ninguna informaci??n porque el entorno no incluye una base de datos.

16. En la fila **Database** (Base de datos), haga clic en **Modify** (Modificar).

    Tenga en cuenta que podr??a agregar f??cilmente una base de datos a este entorno si as?? lo deseara; solo tendr??a que establecer algunas configuraciones b??sicas y hacer clic en **Apply** (Aplicar). (Sin embargo, a los efectos de esta actividad, no es necesario agregar una base de datos).

17. Haga clic en **Monitoring** (Monitoreo).

    Examine los gr??ficos para ver los tipos de informaci??n que tiene a su disposici??n.

&nbsp;
&nbsp;
## Tarea??3: explorar los recursos de AWS que admiten su aplicaci??n

18. En el men?? **Services** (Servicios), elija **EC2**.

19. Haga clic en **Instances** (Instancias).

    Tenga en cuenta que hay dos instancias en ejecuci??n y que ambas incluyen *samp* en sus nombres. Ambas instancias admiten su aplicaci??n web.

20. Si desea continuar aprendiendo acerca de los recursos del servicio Amazon??EC2 que se crearon con Elastic Beanstalk, no dude en explorarlos. Encontrar?? lo siguiente:

    - Un *grupo de seguridad* con el puerto??80 abierto
    - Un *balanceador de carga* al que pertenecen ambas instancias
    - Un *grupo de Auto Scaling* que ejecuta de dos a seis instancias, seg??n la carga de la red

    Aunque Elastic Beanstalk se haya encargado de crear estos recursos, usted puede acceder a ellos igualmente.

&nbsp;
&nbsp;
## Fin de la actividad

<i class="icon-flag-checkered"></i> ??Felicitaciones! Ha completado la actividad.

21. En la parte superior de esta p??gina, haga clic en <span id="ssb_voc_grey">End Lab</span> (Finalizar laboratorio) y, a continuaci??n, en <span id="ssb_blue">Yes</span> (S??) para confirmar que desea finalizar la actividad.

    Aparecer?? un panel con un mensaje que indica: *DELETE has been initiated??? You may close this message box now* (Se ha iniciado la ELIMINACI??N??? Ya puede cerrar este cuadro de mensajes).

22. Para cerrar el panel, vaya a la esquina superior derecha y haga clic en **X**.

Para hacer comentarios, sugerencias o correcciones, env??e un email a *aws-course-feedback@amazon.com*

&nbsp;
&nbsp;

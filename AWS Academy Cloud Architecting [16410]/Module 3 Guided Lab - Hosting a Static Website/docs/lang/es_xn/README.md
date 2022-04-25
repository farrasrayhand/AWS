# Módulo 3 - Laboratorio guiado: Alojamiento de un sitio web estático
[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: ILT-TF-100-ARCHIT-6 branch dev_65"

## Información general y objetivos del laboratorio
Los sitios web estáticos tienen contenido fijo sin procesamiento de backend. Pueden contener páginas HTML, imágenes, hojas de estilo y todos los archivos necesarios para representar un sitio web. Sin embargo, los sitios web estáticos no utilizan scripts del lado del servidor ni una base de datos. Si desea que sus páginas web estáticas ofrezcan interactividad y ejecuten la lógica de programación, puede utilizar JavaScript, el cual se ejecuta en el navegador web del usuario.

Puede alojar fácilmente un sitio web estático en Amazon Simple Storage Service (Amazon S3) subiendo el contenido y haciéndolo accesible públicamente. No se necesitan servidores y puede utilizar Amazon S3 para almacenar y recuperar cualquier cantidad de datos en cualquier momento y desde cualquier lugar de la web.

Después de completar este laboratorio, debería ser capaz de lo siguiente:

- Crear un bucket en Amazon S3
- Cargar contenido a su bucket
- Habilitar el acceso a los objetos del bucket
- Actualizar el sitio web

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

2. Espere hasta que el panel **Start Lab** (Iniciar Laboratorio) muestre el mensaje *Lab Status: ready (estado del laboratorio: listo)*, y, luego, cierre el panel haciendo clic en la **X**.

3. En la parte superior de estas instrucciones, elija <span id="ssb_voc_grey">AWS</span>.

   Con esta acción, se abrirá la consola de administración de AWS en una nueva pestaña del navegador, y el sistema iniciará su sesión de forma automática.

   <i class="fas fa-exclamation-triangle"></i> **Sugerencia**: Si no se abre una nueva pestaña del navegador, por lo general habrá un aviso o un icono en la parte superior, el cual indicará que el navegador impide que el sitio abra ventanas emergentes. Haga clic en el aviso o el icono y elija **Allow pop ups** (Permitir ventanas emergentes).

4. Ubique la pestaña de la **consola de administración de AWS** de modo que aparezca junto con estas instrucciones. El método más óptimo sería tener ambas pestañas del navegador abiertas al mismo tiempo para que pueda seguir los pasos del laboratorio más fácilmente.

   <i class="fas fa-exclamation-triangle"></i> **No cambie la región a menos que se le indique expresamente que debe hacerlo**.

<br/>
## Tarea 1: Crear un bucket de Amazon S3

En esta tarea, creará un bucket de Amazon S3 y lo configurará para el alojamiento de sitios web estáticos.

5. En la **consola de administración de AWS**, en el menú <span id="ssb_services">Services (Servicios)<i class="fas fa-angle-down"></i></span>, haga clic en **S3**.

6. Elija <span id="ssb_orange">Create bucket</span> (Crear bucket).

   Un nombre de bucket de S3 es único globalmente y todas las cuentas de AWS comparten el espacio de nombres. Después de crear un bucket, el nombre de dicho bucket no podrá ser utilizado por otra cuenta de AWS en ninguna región de AWS a menos que elimine el bucket.

   Por lo tanto, para este laboratorio, utilizará un nombre de bucket que incluya un número aleatorio, como: _website-123_

7. Para **Bucket name** (Nombre del bucket), escriba: `website-<123>` (y reemplace <_123_> por un número aleatorio)

   El acceso público a los buckets está bloqueado de forma predeterminada. Dado que los archivos de su sitio web estático deben ser accesibles a través de Internet, debe permitir el acceso público.

8. Desactive la opción **Block all public access** (Bloquear todo el acceso público) y, luego, seleccione la casilla que indica **I acknowledge that the current settings may result in this bucket and the objects within becoming public** (Reconozco que la configuración actual puede dar lugar a que este bucket y los objetos dentro se conviertan en públicos).

9. Elija <span id="ssb_orange">Create bucket</span> (Crear bucket).

   Puede utilizar etiquetas para agregar información adicional a un bucket, como un código de proyecto, un centro de costos o un propietario.

10. Elija el nombre del nuevo bucket.

11. Elija la pestaña **Properties** (Propiedades).

12. Desplácese hasta el panel **Tags** (Etiquetas).

13. Elija <span id="ssb_white">Edit</span> (Editar) y, luego, <span id="ssb_white">Add tag</span> (Agregar etiqueta) y escriba lo siguiente:

    * **Key** (Clave): `Department (Departamento)`
    * **Value** (Valor): `Marketing`

14. Elija <span id="ssb_orange">Save Changes</span> (Guardar cambios) para guardar la etiqueta.

    Ahora configurará el bucket para el alojamiento de sitios web estáticos.

15. Permanezca en la consola **Properties** (Propiedades).

16. Desplácese hasta el cuadro **Static website hosting** (Alojamiento de sitios web estáticos).

17. Elija <span id="ssb_white">Edit</span> (Editar).

18. Realice las siguientes configuraciones:

    - **Static web hosting** (Alojamiento web estático): Enable (Habilitar)
    - **Hosting type** (Tipo de alojamiento): Host a static website (Alojamiento de un sitio web estático).
    - **Index document** (Documento de índice): `index.html`
       - **Nota**: Debe escribir este valor, aunque ya sea el que se muestra.
    - **Error document** (Documento de error): `error.html`

19. Elija <span id="ssb_orange">Save Changes</span> (Guardar cambios).

20. Elija el vínculo en **Bucket website endpoint** (Punto de enlace del sitio web del bucket).

    Recibirá un mensaje *403 Forbidden* porque los permisos del bucket aún no están configurados. Mantenga esta pestaña abierta en su navegador web para que pueda volver a ella más tarde.

    Ahora, el bucket está configurado para alojar un sitio web estático.

<br/>
## Tarea 2: Cargar contenido al bucket

En esta tarea, cargará los archivos estáticos en el bucket.

21. Haga clic con el botón derecho en cada uno de estos enlaces y descargue los archivos a su equipo:

    <i class="fas fa-exclamation-triangle"></i> Asegúrese de que cada archivo mantenga su nombre, incluida la extensión.

    - [index.html](../../../scripts/index.html)
    - [script.js](../../../scripts/script.js)
    - [style.css](../../../scripts/style.css)

22. Vuelva a la consola de administración de Amazon S3 y haga clic en la pestaña **Objects** (Objetos).

23. Elija <span id="ssb_orange">Upload</span> (Cargar).

24. Elija <span id="ssb_white">Add files</span> (Agregar archivos).

25. Elija los tres archivos que descargó.

26. Elija <i class="far fa-check-square"></i>I acknowledge that existing objects with the same name will be overwritten (Confirmo que se sobrescribirán los objetos existentes con el mismo nombre).

27. Elija <span id="ssb_orange">Upload</span> (Cargar).

Los archivos se cargan en el bucket.

<br/>
## Tarea 3: Habilitar el acceso a los objetos

Los objetos almacenados en Amazon S3 son privados de forma predeterminada. Esto garantiza que los datos de su organización permanezcan seguros.

En esta tarea, logrará que los objetos cargados tengan acceso público.

En primer lugar, confirme que actualmente sean privados.

28. Vuelva a la pestaña del navegador que muestra el mensaje *403 Forbidden*.

29. Actualice <i class="fas fa-sync"></i>la página web.

    <i class="fas fa-comment"></i> Si cerró accidentalmente esta pestaña, diríjase a la pestaña **Properties** (Propiedades) y, en el panel **Static website hosting** (Alojamiento de sitios web estáticos), vuelva a elegir el enlace **Endpoint** (Punto de enlace).

    Aún debería ver un mensaje *403 Forbidden*. Esta respuesta es la esperada. Este mensaje indica que Amazon S3 aloja su sitio web estático, pero el contenido es privado.

    Puede hacer públicos los objetos de Amazon S3 de dos maneras diferentes:

    - Para hacer público un bucket completo, o un directorio específico en un bucket público, utilice una *política de bucket*.
    - Para hacer públicos objetos individuales de un bucket, utilice una *lista de control de acceso (ACL)*.

    Normalmente es más seguro hacer públicos _los objetos individuales_ porque esto evita hacer públicos otros objetos accidentalmente. Sin embargo, si sabe que no existe información confidencial en todo el bucket, puede utilizar una _política de bucket_.

    Ahora configurará los objetos individuales para que sean accesibles públicamente.

30. Vuelva a la pestaña del explorador web con la consola de Amazon S3 (pero no cierre la pestaña del sitio web).

31. Seleccione <i class="far fa-check-square"></i>los tres objetos.

32. En el menú <span id="ssb_s3_blue">Actions (Acciones)<i class="fas fa-angle-down"></i></span>, elija **Make public** (Hacer público).

Se muestra una lista de los tres objetos.

33. Elija <span id="ssb_s3_blue">Make public (Hacer público)</span>.

Su sitio web estático ahora es accesible públicamente.

34. Vuelva a la pestaña del navegador web que tiene el mensaje *403 Forbidden*.

35. Actualice <i class="fas fa-sync"></i>la página web.

Ahora debería ver el sitio web estático alojado por Amazon S3.

<br/>
## Tarea 4: Actualizar el sitio web

Puede modificar el sitio web editando el archivo HTML y cargándolo de nuevo en el bucket de S3.

Amazon S3 es un _servicio de almacenamiento de objetos_, por lo que debe cargar el archivo completo. Esta acción reemplaza el objeto existente en el bucket. No se puede editar el contenido de un objeto; en su lugar, se debe reemplazar el objeto completo.

36. En su equipo, cargue el archivo **index.html** en un editor de texto (por ejemplo, Bloc de notas o TextEdit).

37. Busque el texto **Served from Amazon S3 (Entregado desde Amazon S3)** y sustitúyalo por `Created by <YOUR-NAME> (Creado por <YOUR-NAME>)`, reemplazando <*YOUR-NAME*> por su nombre (por ejemplo, _Created by Jane_).

38. Guarde el archivo.

39. Vuelva a la consola de Amazon S3 y cargue el archivo **index.html** que acaba de editar.

40. Seleccione <i class="far fa-check-square"></i>**index.html** y utilice el menú **Actions** (Acciones) para volver a elegir la opción **Make public** (Hacer público).

41. Vuelva a la pestaña del explorador web con el sitio web estático y actualice <i class="fas fa-sync"></i>la página.

Su nombre debería estar ahora en la página.



Ahora es posible acceder a su sitio web estático en Internet. Debido a que está alojado en Amazon S3, el sitio web tiene alta disponibilidad y puede admitir grandes volúmenes de tráfico sin utilizar ningún servidor.

También puede utilizar su propio nombre de dominio para dirigir a los usuarios a un sitio web estático alojado en Amazon S3. Para ello, puede utilizar el servicio de sistema de nombres de dominio (DNS) de Amazon Route 53 en combinación con Amazon S3.

<br/>
## Envío de su trabajo

42. En la parte superior de estas instrucciones, elija <span id="ssb_blue">Submit</span> (Enviar) para registrar su progreso y, cuando se le indique, elija **Yes** (Sí).

43. Si los resultados no se muestran después de algunos minutos, vuelva a la parte superior de estas instrucciones y elija <span id="ssb_voc_grey">Grades</span> (Resultados).

**Sugerencia**: Puede enviar su trabajo varias veces. Después de realizar las modificaciones pertinentes, vuelva a elegir **Submit** (Enviar). Su último envío es el que se registrará para este laboratorio.

44. Para encontrar comentarios detallados sobre su trabajo, elija <span id="ssb_voc_grey">Details</span> (Detalles) seguido de <i class="fas fa-caret-right"></i> **View Submission Report** (Ver informe de envío).

<br/>

## Fin del laboratorio <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> ¡Felicitaciones! Ha completado el laboratorio.

45. Elija <span id="ssb_voc_grey">End Lab</span> (Finalizar laboratorio) en la parte superior de esta página y, a continuación, seleccione <span id="ssb_blue">Yes</span> (Sí) para confirmar que desea finalizar el laboratorio.

    Un panel presentará el mensaje *DELETE has been initiated… You may close this message box now (Se ha iniciado la ELIMINACIÓN… Ya puede cerrar este cuadro de mensaje)*.

46. Seleccione la **X** de la esquina superior derecha para cerrar el panel.



*© 2020 Amazon Web Services, Inc. y sus empresas afiliadas. Todos los derechos reservados. Este contenido no puede reproducirse ni redistribuirse, total ni parcialmente, sin el permiso previo por escrito de Amazon Web Services, Inc. Queda prohibida la copia, el préstamo y la venta de carácter comercial.*
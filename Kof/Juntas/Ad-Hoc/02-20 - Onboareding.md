# Onboarding - [02/20/2024]

## Información General
- **Facilitadora:** Liliana Márquez, Scrum Master
- **Líder Técnico:** Alex Tenorio
- **Participantes Especiales:** Sebastián (Orden Entry), Francisco Mercado, **Juan Pablo** (Orden Entry)
- **Contexto del Proyecto:** Advisor se compone por siete módulos, enfocados en facilitar a los pre-ventas la toma de pedidos y datos relevantes durante sus visitas a tiendas.

## Presentaciones Personales
- **Francisco Mercado:** Experiencia en tecnología de la información desde 1984, enfocado en data y backend. Casado, con dos hijos.
- **Juan Pablo:** 7 años de experiencia en .NET, con habilidades en diversas tecnologías y lenguajes. Casado, sin hijos. **Preferencia de nombre:** Pablo.

## Descripción del Proyecto
- **Objetivo:** Mejorar la experiencia de pre-ventas con una aplicación más dinámica y tecnológicamente avanzada.
- **Módulos Principales:** Incluyen perfil de usuario, mapa de ruta, dashboards, 360 del cliente, guided missions, loyalty, y order entry.
- **Especial Enfoque en Order Entry:** Juan Pablo estará enfocado en este módulo, crucial para la gestión de pedidos.

## Arquitectura y Tecnología
- **Plataforma:** Basada en Azure, utilizando React Native, con un enfoque en microservicios para soportar tanto operaciones online como offline.
- **Integraciones:** Con otras aplicaciones y servicios de Coca-Cola FEMSA, incluyendo el manejo de datos a través de EDB y SAP.

## Proceso de Trabajo
- **Metodología:** Scrum, con sprints de dos semanas y PI plannings cada tres o cuatro meses.
- **Herramientas:** Uso de Jira para gestión de tareas, con la posibilidad de acceder a Teams y carpetas compartidas para una comunicación y documentación efectiva.

## Acciones a Seguir
- [ ] **Juan Pablo:** Enfocarse en el desarrollo y mejoras del módulo de Order Entry.
- [ ] **Todos:** Participar activamente en las sesiones de daily y técnicas, así como en los PI plannings presenciales en Ciudad de México.
- [ ] **Liliana Márquez:** Facilitar el acceso a herramientas y documentación necesaria, y organizar las sesiones de trabajo y plannings.

## Puntos Pendientes
- Definición de la fecha exacta para el MVP y la extensión de Order Entry.
- Clarificación de rutas y funcionalidades específicas para los pre-ventas.

## Comentarios Adicionales
- Se destaca la importancia de la comunicación abierta y la claridad en las estimaciones y compromisos del equipo.

## Transcript


Liliana
01:16
Perdón.


Liliana
01:34
Después de que nos presentemos, bueno, que.


Francisco
01:41
Se presenten ustedes, ¿va?


Francisco
01:42
bueno, entonces también que terminemos recargo.


Liliana
01:45
Bueno, listo.


Francisco
01:46
Adelante, Francisco.


Francisco
01:46
Hola, ¿qué tal?


Francisco
01:46
Bueno, soy Francisco Mercado.


Francisco
01:47
He trabajado en el área de tecnología de la información desde 1984.


Francisco
01:50
En los últimos años, pues, he trabajado prácticamente en el área de data.


Francisco
02:07
Y antes de eso, pues prácticamente he desarrollado, estuve desarrollando en .NET, prácticamente Endpoints, con MVC 5 aplicaciones en el frontend, pero soy de más backend.


Francisco
02:28
Me gusta toda la parte de...


Francisco
02:30
Bueno, sí me gustaba mucho la parte desarrollo, pero llegó el momento en que, como que después de tantos años de estar desarrollando, ya necesitaba o tenía la necesidad de cambiar de área y me llamó el área de datos y salté al área de datos.


Francisco
02:46
Estoy casado, tengo dos hijos ya, los dos también están casados, ellos dos están casados y viven uno en Michoacán y otro vive aquí como media hora de la casa.


Francisco
02:57
Muy bien, muy bien, eso es bueno.


Liliana
04:25
Bueno, a mí me gusta que me.


Francisco
04:28
Digan Paco, pero si se sienten cómodos, pues Francisco.


Liliana
04:59
Y, pues, bueno, Alex.


Liliana
05:00
Bueno, pues, mi nombre es Alejandro Tenorio.


Liliana
05:01
Soy parte del equipo de arquitectura de aquí en Coca-Cola FEMSA.


Liliana
05:05
En este caso, pues, fui asignado para este proyecto que es Vizor.


Liliana
05:12
Bueno, pues, estaremos aquí apoyando Vamos a tener un Tech Lead específicamente para temas de Order Entry.


Liliana
05:22
Pero voy a estar en coordinación con él.


Liliana
05:25
Para el tema del Advisor.


Liliana
05:28
Y con esa premisa...


Liliana
05:31
Bueno, soy casado, tengo dos hijos...


Liliana
05:35
Dos perrijos...


Liliana
05:39
No, tres perrijos, dos gatijos...


Liliana
05:43
Hobbistos, me gusta...


Liliana
05:45
El golf, lo practico cada fin de semana, jugando cada cuando puedo, me gusta el ajedrez, tener torneos, siempre me gusta reparar la mente y los acertijos, libros de acertijos y temas y muchas otras cosas que ya después en su momento les compartiré.


Liliana
06:05
Bueno, un poquito de mi carrera, bueno pues ya llevo prácticamente casi Ya van a ser como 19, 18 años en el ramo.


Liliana
06:19
Desde...


Liliana
06:21
¡Uf!


Liliana
06:22
Ya, ya, ya corrió la ardilla .


Liliana
06:25
Este...


Liliana
06:27
Se queda corriendo...


Liliana
06:28
¡Ay, mi necio!


Liliana
06:29
Me quedé como pasmado, porque estoy intentando recordar desde dónde.


Liliana
06:33
O sea, desde Flash, mis primeros piñonillos en Flash, en...


Liliana
06:38
En este...


Liliana
06:40
En Visual.


Liliana
06:42
Hizo el estudio, en esos años el Visual 6, vi algo de BASIC y desde he venido con tecnologías back y front.


Liliana
06:52
Las dos las manejo, pero tanto Azure, AWS, Google como principales nubes.


Liliana
07:00
Pero tengo especial predilección por el front.


Liliana
07:04
Soy más front-end.


Liliana
07:08
Como tal, siempre tenemos una especial predilección en donde nos manejamos.


Liliana
07:13
Lo mío es el frontend.


Liliana
07:15
Siempre he sido back-end.


Liliana
07:18
Pero no me gusta mucho.


Liliana
07:21
Me gusta más el tema visual.


Liliana
07:23
Entonces, pues, usted soy yo.


Liliana
07:27
Cualquier cosa o tema que se tenga a nivel de célula.


Liliana
07:30
Ya sea que lo vean con Lili o conmigo.


Liliana
07:32
Estamos para apoyarles.


Liliana
07:36
Les voy a compartir una presentación y les voy a decir que es el proyectito bonito que tenemos por delante.


Liliana
07:49
Entonces voy a apagar mi cámara porque al igual que a Juan, a mí se me frisea un poquito por el Teams por su memoria que consume.


Liliana
07:57
Entonces voy a quitar mi cámara.


Liliana
08:15
Bueno, estoy abriendo la presentación.


Liliana
08:39
Exactamente.


Liliana
08:41
Ahora tengo otra en 20 minutos entonces les voy a robar 20 minutos más de su valioso y apreciable tiempo a dónde quedó la bolita hasta acá en la arquitectura bueno a ver les voy a compartir un segundito en un segundito nomás de una instrucción en lo que.


Liliana
10:17
Pues, estos son los siete módulos principales de nuestro primer MVP, como dice Lili.


Liliana
10:24
De la SFA dentro de nuestro MVP, vamos a estar trabajando el Order Entry, que te lo voy a explicar.


Liliana
10:30
Pero no hay un...


Liliana
10:33
La fecha del MVP no es la fecha del Order Entry.


Liliana
10:36
Pero sí va a haber actividades anidadas al Order Entry mientras estamos nosotros lanzando el primer MVP, ¿no?


Liliana
10:46
Entonces, ahorita lo comento.


Liliana
10:49
Primero que nada, tenemos...


Liliana
10:51
Es una aplicación para preventas.


Liliana
10:56
Ese vendedor que sale de KOF temprano o de las diferentes...


Liliana
11:00
De la mañana en las diferentes unidades trae una aplicación que le llaman la Handheld en donde se puede ver muchas cosas.


Liliana
11:14
La misión de esta plataforma es hacer una versión mejorada de esa con módulos más dinámicos y con mejor tecnología.


Liliana
11:25
Dicho eso, y terminando ese concepto, Lily platicaba de una aplicación para clientes.


Liliana
11:32
La aplicación para clientes se llama Juntos Plus.


Liliana
11:35
Esta aplicación también se llama Juntos Plus SFA Advisor o Juntos Plus Advisor.


Liliana
11:41
Van a escuchar a lo largo de las reuniones, temas, etcétera, que pueden decir SFA, pueden decir Advisor, pueden decir Juntos Plus Advisor o JAdvisor, cualquiera de esos, estamos hablando de nuestra plataforma.


Liliana
11:55
Pero hay una aplicación hermana que es la del cliente.


Liliana
12:01
El tendero o la tiendita de la esquina donde tenemos operación, agarra y dice, tiene su aplicación que se llama Juntos Plus, y le llaman v4 porque es la versión 4 a nivel de kof pero el cliente la utiliza o la visualiza como juntos plus pero nosotros le llamamos v4 o j4 o v4 es la del cliente y advisor es la del pre-venta que es la que vamos a hacer teniendo esa diferencia bien marcada nuestro principal módulo en el que estamos ahorita es el profile El profiling, en este caso, el logging se hace a través del B2C.


Liliana
12:50
Como ya saben, el B2C es un componente dentro de Azure para hacer tokenización.


Liliana
12:56
Entonces, el equipo de Microsoft internalizado está haciendo ese desarrollo actualmente y lo estamos integrando a nuestra plataforma.


Liliana
13:04
Aquí vamos a estar trabajando temas de datos de los clientes.


Liliana
13:09
Perdón, de las preventas.


Liliana
13:10
Este es un módulo para preventa en donde él hace su login y automáticamente nos da un token y podemos seguir al siguiente módulo.


Liliana
13:19
El siguiente módulo, que es uno de los módulos principales, es el Roadmap.


Liliana
13:24
Mapa de ruta o también conocido como plan de visita.


Liliana
13:30
El plan de visita Roadmap es básicamente.


Liliana
13:34
Mapa donde van a estar los diferentes clientes a visitar del día del pre-venta.


Liliana
13:43
Entonces, en este módulo lo va a usar todo el tiempo el pre-venta para seguir su ruta, ver información del cliente, ver temas de Dashboards.


Liliana
13:54
Se dan cuenta de que aquí tenemos otros dos módulos anidados que es Dashboards.


Liliana
13:57
Dashboards son los indicadores o reportes que necesita el preventa de su ruta.


Liliana
14:05
Cómo va su ruta, qué indicadores tiene, cuántas ventas, remuneración y ejecución.


Liliana
14:11
Básicamente, los reportes de la ruta.


Liliana
14:15
Todos van juntitos.


Liliana
14:17
Dentro de la ruta, una ruta se compone de clientes a visitar.


Liliana
14:22
Ya que tienen los indicadores, el usuario preventa puede seleccionar o puede llegar a la tiendita de un usuario.


Liliana
14:30
En ese momento, o antes, puede abrir el 360 del cliente.


Liliana
14:35
En este caso, este módulo traerá indicadores, los KPIs, el performa, cómo se va comportando este cliente y su información general.


Liliana
14:44
Dentro de este está conectado el módulo de Guided Missions y el módulo de Loyalty, así como el del Order Entry.


Liliana
14:52
De aquí se detona que esta visualización de información, vamos a llamarlo así, un X-Ray, de el cliente.


Liliana
15:03
Indicadores...


Liliana
15:05
Las Data Emissions es una célula desacoplada, pero que va a vivir con nosotros, así como Order Entry está desacoplando, Data Emissions tiene su propia célula.


Liliana
15:14
Básicamente, estos son indicadores o iniciativas de la compañía dirigidas hacia un cliente en específico, donde el pre-venta tendrá que cumplir ciertas tareas.


Liliana
15:29
Por ejemplo, proponer algún tipo de producto, generar alguna acción a nivel de la tiendita o del cliente, entonces aquí están.


Liliana
15:40
Esas ganan Emissions dentro de 360 o estas complementan.


Liliana
15:44
El tema de loyalty es básicamente, en Brasil lo conocen como premia y en México es un gana-gana, que son plataformas en donde tenemos el manejo de puntos cada que compra a un usuario a través de v4 si cada compra le genera puntos y el loyalty los puede ver dentro de v4 sale así mismo.


Liliana
16:18
Puntos que tiene exacto, se levanta que tiene su propia tienda de canje y ellos pueden seleccionar productos en la V4 para canjear con los puntos que ya ganaron.


Liliana
16:29
Entonces, poniendo eso en perspectiva, tenemos la información global del cliente, las ejecuciones que tengo que hacer, los puntos que tiene ese cliente, ¿sí?


Liliana
16:45
Pero un punto importante en la misión de un pre-venta es vender.


Liliana
16:52
Precisamente por eso aquí está uno de los módulos principales, que es el Order Entry.


Liliana
16:57
El Order Entry básicamente es el flujo de compra del cliente, donde vamos a tener una lista de precios.


Liliana
17:04
Esa lista de precios viene por cliente específico.


Liliana
17:07
Por eso es bien importante esta validación de la ruta, el ID del cliente, y del ID del cliente se detona toda la información que vamos a estar usando en la tiendita o en esa visita.


Liliana
17:21
El order entry, básicamente, lo que hace es enviar un pedido.


Liliana
17:26
Actualmente, tenemos la v4 y la v4 tiene una serie de endpoints que vamos a utilizar nosotros y vamos a ir a añadir más usabilidades.


Liliana
17:42
Porque la v4 fue diseñada para que el usuario pusiera un pedido.


Liliana
17:47
Vamos a ponerlo así.


Liliana
17:49
Con algunas promociones, con algunos temas.


Liliana
17:51
Pero en la pre-venta, o en la Handheld, o en la Advisor, tenemos que ir a completar más valor al módulo, como son las promociones on-going, las escalonadas.


Liliana
18:08
Ya, ahorita les presento el otro slider para que lo tengan más claro.


Liliana
18:13
¿Qué hora es?


Liliana
18:13
Para no pasarme el tiempo.


Liliana
18:14
Ya tenemos un ratito.


Liliana
18:16
Entonces, es el módulo principal o uno de los módulos principales que vamos a trabajar.


Liliana
18:22
Este está desacopladito.


Liliana
18:24
¿Qué significa?


Liliana
18:25
Al igual que Guided Missions está dentro de nuestro MVP.


Liliana
18:29
Nuestro MVP es finales de julio, ¿sale?


Liliana
18:35
Y el órgano Entity, su MVP, es en inicios de mediados, inicios de noviembre, ¿sale?


Liliana
18:44
Entonces va a vivir dentro de la de advisor pero se concluye no es un módulo con el que salimos en el primer MVP sale después pero va a vivir dentro de dicho eso vamos a ver rápido un modelo en alto nivel de arquitectura nosotros estamos en un terminal de Azure que es un terminal que está dentro del cloud de Azure como tal, vamos a tener una sola aplicación que consuma una infraestructura ya definida.


Liliana
19:23
Vamos a tener por aquí, por , Android B2C, FormDoor, el Firewall, va a estar una AppManagement que va a tener comunicación con un AKS, con sus pods, sus microservicios, los cuales van a poder ser desplegados a través de un junk box con su agente de compilación.


Liliana
19:40
Vamos a estar utilizando los pods, como les dije, a través de microservicios para manejar microservicios y estos puedan estar accediendo a la parte de base de datos, así como cuidando un poquito con Key Vault las conexiones.


Liliana
19:57
Esta arquitectura se conecta con arquitecturas o con temas adicionales.


Liliana
20:04
Por aquí ustedes pueden empezar a ver juntos Plus V4, Plus API, vamos a utilizar esas APIs, algunas, para el modelo del Order Entry.


Liliana
20:26
Y algunas a Juntos Plus v4.


Liliana
20:28
Véanlo así.


Liliana
20:30
Nosotros somos una como esta.


Liliana
20:32
Esta es una como esta.


Liliana
20:42
¿El EDB qué significa?


Liliana
20:45
Aquí están todos los servicios.


Liliana
20:47
Vamos a llamarlo así.


Liliana
20:49
Es un bridge de datos para manejo de información, el cual se conecta a SAP.


Liliana
21:37
Pero yo hago aquí un stop para no llenarlos más de información.


Liliana
21:42
La idea es que hay una carpeta compartida, que Lily les va a dar acceso, donde está esta presentación, para que les den un ojo.


Liliana
21:52
Donde cómo vamos a estar trabajando los microservicios, etcétera.


Liliana
21:56
En este caso para MVP de la Advisor.


Liliana
22:02
Y para el tema de Order Entry, muchos vivirán acá.


Liliana
22:05
¿Por qué?


Liliana
22:06
Porque vamos a compartir con B4, o compartimos con B4, parte de estas APIs.


Liliana
22:12
Entonces, por eso está, por aquí ven esta conexión que va por acá, por acá, y consume el API Management de este...


Liliana
22:19
Del ADB.


Liliana
22:20
O, en su caso, nosotros vamos a tener nuestro propio API Management, donde vamos a estar teniendo nuestros propios servicios y, al final de cuentas, vamos a tener aquí una comunicación del Databricks hacia el ADB, ¿no?


Liliana
22:34
Nosotros tenemos por aquí nuestro Data Lake y algunas otras cosas.


Liliana
22:59
Cuanto tengan acceso al drive que Lili comparte esta presentación está Vienen por cada uno de los módulos.


Liliana
23:09
Viene la arquitectura de nuestro frontend.


Liliana
23:12
Estamos usando React Native como framework principal.


Liliana
23:18
Bueno, tienen algunos de sus módulos y vamos a tener una aplicación con base de datos.


Liliana
23:23
Porque esta aplicación tiene que funcionar offline y online en muchos de sus módulos como tal.


Liliana
23:33
Aquí una breve explicación de algunas...


Liliana
23:37
Esto me lo pidieron para temas de dirección en donde están viendo los beneficios.


Liliana
23:43
Y saber que muchas de las iniciativas que tenemos a nivel de KOF están hechas sobre React o React Native como tal.


Liliana
23:57
Esa es y les quería presentar...


Liliana
24:00
A ver si me da tiempo...


Liliana
24:01
Si tengo 4 minutos.


Liliana
24:03
Este muy rápido la otra esta es en dp con order entry tenemos el order entry en donde este.


Liliana
24:23
El que habíamos hablado de noviembre por acá está noviembre si Actualmente nosotros vamos a estar trabajando en estos.


Liliana
24:31
Estos ya están y viven en la v4.


Liliana
24:33
De hecho los vamos a invitar a una presentación que va a hacer este Sebastián.


Liliana
24:36
Lo había invitado pero tiene una asignación pendiente de mi parte.


Liliana
24:40
Por eso le pedí que bajara.


Liliana
24:43
Pero porque no iba a participar prácticamente entonces me pidió que si se podía apurar.


Liliana
24:49
Entonces él va a presentar la v4 como tal.


Liliana
24:53
Actualmente como funciona.


Liliana
24:54
Y vamos a ver estos módulos que nos tenemos que traer al Bison.


Liliana
24:59
Adicionalmente que son estos, ¿no?


Liliana
25:01
Lista de precios, categoría, shopping cart, checkout, promo push.


Liliana
25:05
Básicamente, ¿sí?


Liliana
25:07
En el tiempo en el que también vamos a estar trabajando esto, el equipo de LDB y SAP van a estar trabajando en estos módulos que son el buscador por SKUs, envase, cauciamiento, promo ongoing, polarizado, escalonada, canje.


Liliana
25:24
Todos estos son temas que se van a estar aprovisionando en la aplicación.


Liliana
25:33
Después explicamos todo esto, qué es cada módulo, para qué ustedes deben conocerlo.


Liliana
25:38
Y en la parte de arriba podemos ver después las integraciones que vamos a hacer a nivel del AVE4.


Liliana
25:52
Y nuestro equipo que es ADVISOR.


Liliana
25:55
Y bueno, algunas definiciones.


Liliana
25:58
Y por acá viene un plan.


Liliana
26:01
Donde ya está más en bajo nivel.


Liliana
26:03
gracias a Lidia nos apoyó mucho en poder juntar los equipos y estar definiendo todas estas tareas.


Liliana
26:22
Eso es parte del onboarding.


Liliana
26:25
Espero no haberlo llenado de mucha información.


Liliana
26:27
La idea es que se lleven la idea de un pre-venta, las actividades del mapa de ruta, los dashboards 360 del cliente, qué es el 360, pues información del cliente como tal.


Liliana
26:41
De se detona el order entry, puedo ver puntos loyalty, o sea, toda esta información que necesita el pre-venta en una Hanghill para poder, ¿qué cosa?


Liliana
26:51
Vender.


Liliana
26:52
Sale ver indicadores que le vendo que promociones tiene si tiene envase si no tiene envase ven que hay veces en las que tenemos hay canje de envases etcétera vamos a la tiendita no sé si les he tocado en algún momento bueno a eso se refiere todo esto cuando hacemos un canje o cuando el cliente quiere hacer un canje de producto o vende envase cuando tiene promociones toda esta parte que el perventa de coca-cola fensa le hace a su cliente en la tiendita, o lo atiende, pues viene a ser impactado en esta presentación que les acabo de mostrar.


Liliana
27:31
Esa es la oferta de valor que le damos nosotros a nuestro cliente final, que es, aquí me doy cuenta, ya lo estamos atacando con una aplicación donde él puede comprar, pero también cuando va en avanzada el pre-vendedor, le puede ir haciendo otros otra serie de temas, ¿no?


Liliana
27:55
Pero bueno, de mi parte, que concluimos.


Liliana
27:57
Benitas, espero no haya sido...


Liliana
27:59
Me iré.


Liliana
28:07
Ayúdenme nada más con sus datos.


Liliana
28:09
Necesito nombre completo, correo y compañía.


Liliana
28:14
Para añadirlos al documento de seguridad lo voy a mandar hoy y de una vez aprovecho para que se vayan ellos y ya tengan acceso a nuestro redoble vale salen yo me despido chicos porque tengo otra reunión pero este cualquier cosa estamos en contacto muchas gracias muchas.


Liliana
28:46
No, no.


Francisco
29:10
De sesión técnica SFA, señor.


Francisco
29:14
No sé si ya las tengan las dos.


Francisco
29:15
No, solamente tengo la daily interna, bueno, que es de 8 a 20, 8 a 40.


Francisco
29:18
Y luego una de las 9.30 a las 10, que es la del daily advisor.


Francisco
29:26
Son las que tengo.


Francisco
29:28
Aquí en mi calendario.


Francisco
33:46
Bueno, no es tanto sobre la aplicación, sino más bien me surgió la duda.


Francisco
33:50
Ya ves que en el mapa mencionaste acerca del GPS.


Francisco
33:54
Mi pregunta aquí es, ¿van a seguir una ruta fija los vendedores?


Francisco
33:59
¿O ellos tienen la libertad definir su ruta y nada más llegar con el cliente?


Liliana
43:13
En Ciudad de México.


Liliana
46:08
Claro.


Liliana
46:09
¿Vale?


Liliana
46:44
Muy bien.


Liliana
46:51
Muy bien.


Francisco
47:06
Ok, muchísimas gracias.


Liliana
50:32
Es por la cama, está atrás de.


Liliana
52:11
Van a dar nada.


Liliana
52:22
No me van a ocurrir.


Liliana
52:23
No me van a ocurrir.


Liliana
53:05
¿Pero cómo?


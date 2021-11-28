# Unreal Survival Arena


## Prototipo desarrollado en Unreal Engine 4 mediante el uso de Blueprints para el Máster de Videojuegos de la UCM


Autor: Carlos Jiménez Álvarez</p>



# Descripción del prototipo

El prototipo desarrollado es un FPS (First Person Shooter) desarrollado en Unreal Engine 4 mediante el uso exclusivo de programación visual con Blueprints. Basado en el proyecto de prueba que se ha dado durante las clases, se han aprovechado varias de las características explicadas por el Prof. D. Federico Peinado al mismo tiempo que se han descartado otras. De igual manera, varias características pensadas inicialmente no se han podido incluir por falta de tiempo, dificultad de implementación o limitación de los assets.

En el prototipo, el personaje manejado por el jugador se encuentra en un mapa amplio en el que hay enemigos tanto pregenerados como siendo generados periódicamente. El jugador tiene a su disposición 3 armas para enfrentarse a los enemigos:



* Pistola semiautomática, con una velocidad de disparo tan rápido como se pueda hacer clic con el ratón o se pulse el botón del gamepad, un daño medio y una probabilidad elevada de causar daño crítico. El retroceso es manejable.
* Escopeta de corredera, que dispara multitud de “proyectiles” con una propagación elevada, lo que hace que haga mucho daño a corta distancia pero poca a media o larga. Se debe actuar la corredera manualmente en cada disparo, lo que hace que la velocidad de disparo sea lenta y de tiempo a compensar el retroceso.
* Rifle automático, con alta velocidad de disparo y daño por bala inferior, además de un retroceso mayor.

Los enemigos tienen un comportamiento muy básico. Cuándo pueden moverse, buscan el camino más corto al jugador y se dirigen a él. A partir de cierta distancia, le disparan con el rifle. Deben recargar el arma tras varios disparos. Tras recibir suficiente daño, caen y desaparecen. Con cada enemigo derrotado, el jugador recibe una cantidad de dinero a modo de recompensa.

Si el jugador se va quedando sin munición, puede pulsar la tecla B del teclado o los botones Y o triángulo para abrir la tienda, que le permite gastar dinero por más munición.

El juego no tiene condición de victoria ni derrota, ya que los enemigos son generados infinitamente y el jugador no recibe daño en la versión actual del prototipo.


## Controles

Siguiendo el ejemplo del proyecto inicial de Unreal, se han mantenido y añadido controles para ratón y teclado y gamepad.


### Ratón y teclado



* Con el movimiento del ratón se gira la cámara.
* Teclas W, A, S y D, o flechas, para mover al personaje.
* Clic izquierdo para disparar.
* Tecla R para recargar.
* Barra espaciadora para saltar.
* Tecla B para abrir y cerrar la tienda.
* Teclas 1, 2 y 3 para cambiar a la pistola, la escopeta y el rifle respectivamente.


### Gamepad



* El joystick derecho gira la cámara.
* El joystick izquierdo mueve al personaje.
* Gatillo derecho para disparar.
* Botón X o cuadrado para recargar.
* Botón A o equis para saltar.
* Botón Y o triángulo para abrir y cerrar la tienda.


## Objetivos iniciales

A continuación, se listan los objetivos propuestos al inicio del desarrollo del prototipo:



* Incluir 4 armas: arma cuerpo a cuerpo, pistola, escopeta y rifle, cada una con sus diferentes mecánicas.
* Añadir un escudo adicional a la vida del jugador que se pudiese regenerar.
* Posibilidad de apuntar a través de la mira del arma que se esté usando, excepto para el arma cuerpo a cuerpo.
* Obtener dinero al derrotar a enemigos que se pueda gastar en una tienda para conseguir armas, munición y mejoras.
* Realizar distinto daño a los enemigos dependiendo de dónde se les impacte.
* Modo de rondas, por las que el jugador debe sobrevivir a oleadas de enemigos con pequeños descansos entre cada ronda. Al llegar a la ronda final y sobrevivir, se daría la victoria.
* Pickups aleatorios por el mapa que otorguen vida, munición o dinero.


# 


# Desarrollo

A lo largo del desarrollo, se han cambiado varias veces los assets a utilizar según se iba aprendiendo a usar la herramienta de UE4, ya que no todos los assets daban las mismas facilidades para manipularlos, aplicarles efectos, animaciones, etc. Inicialmente, se usa un paquete gratuito del Marketplace llamado Prototype Weapons que incluye multitud de armas con efectos, sonidos y animaciones. 

Se decide colocar el arma sin brazos ya que manipulando fácilmente la posición y la rotación del Skeletal Mesh del arma se podía conseguir un efecto de apuntado bastante satisfactorio. De forma similar a como se implementó en clase el Zoom, se usa un Timeline para mover el arma con interpolación de una posición a otra, e igual al soltar el ratón. A pesar de ser rápido de implementar, se investiga la inclusión de brazos y el uso de Sockets para conectar el arma a las manos, lo que hace mucho más difícil esta mecánica de apuntado.

Consultado varios tutoriales y vídeos, se encuentra la forma de manipular el Animation Blueprint de los brazos que trae UE4 por defecto para poder modificar la posición de los huesos y darle una mejor forma al agarre de las armas. Esto permite también utilizar las animaciones que traen los brazos y que dan mucha más calidad visual al proyecto.

Tras esto, se decide cambiar de los assets de Prototype Weapons a Military Weapons Dark, también gratuito, que trae assets más que suficientes y la calidad es superior. En el proceso, se descubre la existencia de un proyecto demo realizado por Epic hace años llamado Shooter Game Example, el cual contiene muchísimos assets que se han aprovechado y algunas de las mecánicas que se querían implementar en este prototipo, pero implementadas en C++ en lugar de Blueprints. De entre los assets, el cambio más considerable es el uso de los modelos de brazos para el jugador y del personaje completo para los enemigos.

Investigando sobre el uso de los proyectiles que tiene UE4 por defecto, y haciendo uso de lo aprendido durante las clases de Unity impartidas por Daniel González al principio del curso, se decide implementar la mecánica de disparo mediante rayos en lugar de actores manipulados por física. Siguiendo algunos tutoriales, se consigue hacer que la pistola, y más adelante el resto de armas, calculen de forma eficiente la interpolación entre los rayos que se dibujan desde el cañón del arma al punto que se marca desde la cámara del personaje. Esto se puede observar fácilmente gracias al FX de Trail asignado a los disparos y que deja un rastro desde el cañón al punto de impacto.

Por otro lado, las animaciones de retroceso de las armas y el mecanismo de corredera de la escopeta están hechos a mano manipulando los huesos de los brazos mediantes modificaciones de las posiciones, poses mezcladas (Blend poses) o cinemáticas inversas (IK, inverse kinematics). Resultó ser todo un reto debido al escaso conocimiento de animación y de uso de la herramienta, pero gracias a varios tutoriales y guías se ha llegado a un resultado bastante satisfactorio.

El retroceso de cada arma también afecta a la puntería y la trayectoria del disparo, viéndose claramente al cambiarse el tamaño del punto de mira cuando se dispara rápido. El tamaño vuelve a su posición original con el paso del tiempo, y es distinto para cada arma.

La recarga de la pistola y el rifle, al no contar con animaciones adecuadas por parte de los brazos, se realiza haciendo un gesto de inclinación sutil del cuerpo, por lo que el arma sale de la vista del jugador, se realiza el cambio y vuelve a subir.

Para dañar a los enemigos, primero se hizo uso de las funcionalidades de daño incorporadas en UE4, pero tras conocer de la existencia de los materiales físicos (Physical Materials) que pueden ser asignados a distintas partes de un esqueleto para darles valores diferentes, se desarrolló una interfaz Damageable que es la que implementan los actores que pueden recibir daño. De esta manera, cada clase Blueprint implementadora gestiona su propio daño y se puede comprobar fácilmente la superficie de impacto del disparo para ver si el daño debe aumentarse o disminuirse.

La IA de los enemigos se ha implementado mediante el uso de Behavior Trees, aunque el resultado es más simple de lo deseado. Mediante el uso de una pizarra (Blackboard), se gestiona el flujo del enemigo al guardarse variables de control como IsDead o IsReloading, además de del punto en el mundo al que debe moverse el personaje.

Actualmente, el enemigo, una vez asignada una posición de destino, se mueve a esa posición independientemente de si el jugador se ha ido o no y, una vez llegado a la posición, se queda para siempre o hasta que sea derrotado. Se ha intentado cambiar el árbol mediante algunos Blueprints de BTTasks adicionales, comprobando si el personaje tiene a la vista al jugador mediante AIPerception o rayos, pero el funcionamiento no era bueno y el coste en recursos era alto. Como nota a futuro, sería interesante revisar este apartado y repensar el árbol de comportamiento para que los enemigos sean más desafiantes y explorar las posibilidades de los BTs.

Además, los enemigos no tienen implementados el daño al jugador por falta de tiempo. Aunque la idea original era que usaran rayos aleatorizados dentro de un cono, lo que permite usar el sistema ya existente para el daño, más adelante se pensó que era más fácil simplemente sacar un valor aleatorio y reducir la salud del personaje en una cantidad fija. Desafortunadamente, no se ha llegado a implementar.

Por parte del personaje principal, se ha implementado que, si recibiese daño, antes de perder vida se activarían unos escudos que absorben el daño. Esto se hace mediante una barra secundaria de vida que, además, se regenera cada varios segundos, con el objetivo de tener una barra principal que, al agotarse, supone perder la partida y una barra secundaria que anime al jugador a huir de forma puntual para recuperarse y poder volver al combate.

Por último, el mapa está montado usando los assets que trae el Starter Content. Es muy básico pero cumple su cometido.


# 


# Objetivos cumplidos y no cumplidos.



* Incluir 4 armas: arma cuerpo a cuerpo, pistola, escopeta y rifle, cada una con sus diferentes mecánicas.
    * Aunque no se haya incluido el arma cuerpo a cuerpo, el resto de armas sí están implementadas y se diferencian bastante entre ellas.
* Añadir un escudo adicional a la vida del jugador que se pudiese regenerar.
    * Se ha implementado, aunque no se pueda probar la funcionalidad al no haber daño al jugador.
* Posibilidad de apuntar a través de la mira del arma que se esté usando, excepto para el arma cuerpo a cuerpo.
    * Se implementó en una primera versión, antes de introducir los modelos de los brazos. La versión actual no lo implementa.
* Obtener dinero al derrotar a enemigos que se pueda gastar en una tienda para conseguir armas, munición y mejoras.
    * Se puede conseguir dinero y usarlo para conseguir munición en la tienda, pero todas las armas están desbloqueadas desde el inicio y no hay mejoras implementadas.
* Realizar distinto daño a los enemigos dependiendo de dónde se les impacte.
    * Implementado mediante el uso de Physical Materials.
* Modo de rondas, por las que el jugador debe sobrevivir a oleadas de enemigos con pequeños descansos entre cada ronda. Al llegar a la ronda final y sobrevivir, se daría la victoria.
    * No implementado, a pesar de que existen spawners que crean enemigos, no existen rondas ni condición de victoria.
* Pickups aleatorios por el mapa que otorguen vida, munición o dinero.
    * No implementado, aunque se podría usar el implementado durante clase.


# Bibliografía

[https://www.youtube.com/watch?v=SjrLLXBY3C4](https://www.youtube.com/watch?v=SjrLLXBY3C4&t=1s)

[https://www.youtube.com/watch?v=evYE7tfWXUY](https://www.youtube.com/watch?v=evYE7tfWXUY)

[https://www.youtube.com/watch?v=NZZtMNdJk5o](https://www.youtube.com/watch?v=NZZtMNdJk5o&t=1s)

[https://www.youtube.com/watch?v=bz06uc-HvNQ](https://www.youtube.com/watch?v=bz06uc-HvNQ&t=74s)

[https://www.youtube.com/watch?v=FwVYXlsTPD4](https://www.youtube.com/watch?v=FwVYXlsTPD4&t=1158s)

[https://www.youtube.com/watch?v=Fdp5k1lxUAI](https://www.youtube.com/watch?v=Fdp5k1lxUAI&t=49s)

[https://www.youtube.com/watch?v=PkzF0i61j1I](https://www.youtube.com/watch?v=PkzF0i61j1I&t=26s)

[http://shootertutorial.com/](http://shootertutorial.com/)

[https://docs.unrealengine.com/4.27/en-US/Resources/SampleGames/ShooterGame/](https://docs.unrealengine.com/4.27/en-US/Resources/SampleGames/ShooterGame/)

[https://www.gamedev.tv/p/unreal-blueprint/](https://www.gamedev.tv/p/unreal-blueprint/?coupon_code=HUZZAH)

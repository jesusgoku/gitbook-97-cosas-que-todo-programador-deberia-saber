# 73. Resiste la tentación del patrón Singleton

Autor: Sam Saariste

El patrón [Singleton](https://es.wikipedia.org/wiki/Singleton) resuelve muchos de tus problemas. Sabes que sólo necesitas una sola instancia. Tienes una garantía de que esta instancia fue inicializada antes de ser usada. Mantiene tu diseño simple y con un sólo punto de acceso global. Todo está bien. ¿Qué es lo que no me gusta de este clásico patrón de diseño?

Pues mucho, resulta ser. Puede ser tentador, pero la experiencia me mostró que la mayoría de los Singleton hacen realmente más daño que bien. Dificultan las pruebas y dañan la capacidad de mantenimiento. Desafortunadamente, esta sabiduría adicional no está tan propagada como debería y los Singletons continúan siendo irresistibles para la mayoría de los programadores. Pero vale la pena resistirse:

- El requisito de instancia única es con frecuencia imaginado. En muchos casos es pura especulación de que no se necesitarán instancias adicionales en el futuro. Difundir tales propiedades especulativas, a través del diseño de la aplicación, está destinado a causar dolor en algún momento. Los requerimientos cambiarán. El buen diseño lo adopta. Los Singleton no.
- Los Singleton causan dependencia implícita entre unidades de código conceptualmente independientes. Esto es problemático porque están ocultos e introducen acoplamiento innecesario entre las unidades. Este olor del código se pudre cuando intentas escribir pruebas unitarias, las cuales dependen de soltar el acoplamiento y de la habilidad de sustituir selectivamente una implementación simulada (mock) de una real. Los Singleton previenen la simulación directa.
- Los Singleton también llevan estado persistente implícito, lo que dificulta más las pruebas unitarias. Las pruebas unitarias dependen de que sean independientes entre sí, así las pruebas pueden ser ejecutadas en cualquier orden y el programa puede ser configurado a un estado conocido antes de la ejecución de cada prueba unitaria. Una vez que hayas introducido Singleton con estado mutable, esto puede ser más difícil de llevar a cabo. Además, dicho estado globalmente accesible hace más difícil razonar sobre el código, especialmente en ambientes multihilos.
- Los multihilos introducen futuras fallas en el patrón Singleton. El bloqueo directo al acceso no es muy eficiente, así es como el llamado patrón de doble revisión de bloqueo (DCLP) ha ganado popularidad. Desafortunadamente, esto puede llevar una forma adicional de atracción fatal. Resulta ser que muchos lenguajes DCLP no son thread-safe e, incluso cuando lo son, aún hay oportunidades de sutiles errores.

El limpiado de Singleton puede presentar un reto final:

- No hay soporte para matar explícitamente a un Singleton, lo cuál puede ser un problema delicado en algunos contextos. Por ejemplo, en una arquitectura de plug-ins en la que un plug-in sólo puede ser descargado de forma segura después de que todos sus objetos han sido limpiados.
- No hay orden implícito de limpieza de Singleton al salir del programa. Esto puede ser problemático para aplicaciones que contienen Singleton con interdependencias. Cuando se cierra dicha aplicación, un Singleton puede acceder a otra que ya ha sido destruida.

Algunas de estas deficiencias pueden ser superadas mediante la introducción de mecanismos adicionales. Sin embargo, esto viene con el costo de complejidad adicional en código que se podría haber evitado escogiendo un diseño alternativo.

Por lo tanto, restringe el uso del patrón Singleton a las clases que realmente nunca deber ser instanciadas más de una vez. No uses un Singleton como punto de acceso global desde código arbitrario. En vez de ello, el acceso directo al Singleton debería ser desde sólo unos pocos lugares definidos, donde pueda dársele vuelta vía una interfaz hacia otro código. Este otro código no lo sabe y así no depende de si un Singleton o cualquier otro tipo de clase implementa la interfaz. Esto rompe la dependencia que impide las pruebas unitarias y mejora la capacidad de mantenimiento. Así que, la próxima vez que estés pensando en implementar o acceder a un Singleton espero que hagas una pausa y lo pienses de nuevo.

Traducción: Espartaco Palma

---
title: Firmware de arranque BIOS/UEFI libre y de código abierto
x-toc-enable: true
...

*Libreboot* es una *[distribución de coreboot](docs/maintain)* (distro de coreboot),
del mismo modo que Debian es una *Distribución de Linux*. Libreboot proporciona
firmware de arranque [libre y de código abierto](https://writefreesoftware.org/)
basado en coreboot, reemplazando firmware BIOS/UEFI propietario
para [placas madre específicas basadas en arquitecturas Intel/AMD x86 y ARM](docs/install/#which-systems-are-supported-by-libreboot)
incluyendo laptops y computadoras de escritorio. Inicializa el hardware (p.e., controladores
de memoria, CPU, periféricos) e inicia un gestor de arranque (bootloader) para tu sistema
operativo (OS). [Linux](docs/linux/) y [BSD](docs/bsd/) están bien soportados. La ayuda está 
disponible vía `#libreboot`
en el chat de IRC de [Libera](https://libera.chat/).

<img tabindex=1 class="r" alt="ThinkPad T480 running Libreboot" title="ThinkPad T480" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img alt="ThinkPad T480 running Libreboot" src="https://av.libreboot.org/t480/t480.jpg" /></span>

El [8 de septiembre del 2025](news/spi.html), Libreboot se volvió un [Proyecto
Asociado](https://www.spi-inc.org/projects/libreboot/) de manera oficial en el "SPI" por sus siglas en inglés
([Software en el interés público](https://www.spi-inc.org/)).

Libreboot proporciona el gestor de arranque de GNU "[GRUB](docs/linux/)" y SeaBIOS
como payloads en placas madre de arquitectura Intel/AMD x86/x86_64, y 
[U-Boot como payload de UEFI](docs/uboot/) *para coreboot* en placas madre de
arquitectura ARM64(Aarch64).
[U-Boot como Payload para UEFI en x86/x86_64] también está disponible para
algunas placas. El payload U-Boot para x86, x86\_64 y arm64 proporciona una
implementación de arranque UEFI ligera. El [diseño](docs/maintain/) de Libreboot
incorpora todos estos payloads en una sola imagen, y tú escoges una para arrancar.

**Nuevo lanzamiento: El lanzamiento más reciente es Libreboot 26.01, publicado
el 30 de Enero del 2026.**
Véase: [El anuncio de lanzamiento de Libreboot 26.01](news/libreboot2601.md).

También puedes [comprar Libreboot preinstalado](https://minifree.org/) desde
Minifree Ltd, en hardware seleccionado, así como también puedes enviar tu hardware
compatible para un [servicio de instalación de Libreboot](https://minifree.org/product/installation-service/).
Leah Rowe, fundadora y desarrolladora principal de Libreboot, también es propietaria
y opera Minifree; las ventas sirven para financiar Libreboot.

*Nosotros* creemos que la libertad de [estudiar, compartir, modificar y
usar el software](https://writefreesoftware.org), sin ningún tipo de
restricciones, es uno de los derechos humanos fundamentales que todo humano
debería tener. Bajo este contexto, *la libertad del software* importa, tu libertad
importa, la educación importa y [el derecho a reparar](https://en.wikipedia.org/wiki/Right_to_repair)
importa. Libreboot te permite continuar usando tu hardware, con 
actualizaciones de firmware frecuentes. Todo esto es el *porqué* Libreboot existe.

Resumen del diseño de Libreboot
----------------------------

<img tabindex=1 class="l" alt="HP EliteBook 2560p and Folio 9470m running Libreboot" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

Libreboot proporciona [coreboot](https://coreboot.org/) para la [inicialización
de maquina](https://doc.coreboot.org/getting_started/architecture.html),
que después salta a un [payload](https://doc.coreboot.org/payloads.html) almacenado en
la memoria flash de arranque; coreboot funciona con muchos payloads, los cuales
inicializan tu sistema operativo p.e., Linux y BSD.

Libreboot hace coreboot fácil de usar para usuario no técnicos, esto se logra
proporcionando un [sistema de compilación totalmente automatizado](docs/maintain),
además de [lanzamientos regulares de binarios](download.md) que proporcionan
imágenes ROM pre-compiladas para la instalación en el hardware soportado. Sin una
automatización como la proporcionada por Libreboot, coreboot sería inaccesible
para muchos usuarios; pero eso no te impide seguir [reconfigurando](docs/maintain)
Libreboot a como más desees/prefieras.

¿Por qué debería utilizar Libreboot?
------------------

<img tabindex=1 class="r" alt="Various Lenovo ThinkPads running Libreboot" style="max-width:25%;" src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /><span class="f"><img src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /></span>

Si ya cuentas con preferencia por el software libre ó tal vez eres un usuario
de coreboot, Libreboot hace mucho más fácil tanto iniciarte como dar soporte de
coreboot a tu maquina gracias a los procesos automatizados.
Libreboot proporciona lanzamientos continuos bien testeados ya pre-ensamblados,
a menudo con parches por encima a coreboot (y demás código) para asegurar
la estabilidad. En comparación, coreboot cuenta con un modelo de lanzamiento
rolling-release, con un snapshot de la base del código que se lanza cada poco meses;
esto lo hace que su enfoque sean principalmente los desarrolladores, mientras que Libreboot,
está pensado específicamente para usuarios comunes. En pocas palabras,
el propósito de Libreboot es que todo *simplemente funcione*.
Claro, configurar coreboot desde cero no es imposible, pero Libreboot lo hace
*"mucho"* más fácil.

Libreboot te da [Software Libre](https://writefreesoftware.org) que de otro modo
no puedes obtener con la mayoría de firmwares de arranque comercial, además
de tiempos de arranque más rápidos y [seguridad superior](docs/linux/grub_hardening.md).
Esto es tan [configurable](docs/maintain/) como poderoso para diversos casos de uso.
Si no estás a gusto con las restricciones (sin mencionar las fallas de seguridad)
impuestas por el fabricante de tu BIOS propietario, entonces Libreboot es opción a tener
en cuenta. Dado que Libreboot se hereda de coreboot, no tiene porqué tener
ningún backdoor/fallo de seguridad en su código, ni contiene código obsoleto olvidado desde
los 80's. Libreboot proporciona una experiencia refinada, veloz y basada en coreboot
de arranque para sistemas Linux y BSD, la cual es auditada y mejorada con regularidad.

Libreboot es más seguro y confiable que el firmware propietario. Muchas personas usan
un firmware de arranque propietario/privativo (no libre pues), incluso al momento
de optar por un [Sistema Operativo libre](https://www.openbsd.org/).
El firmware propietario con frecuencia contiene backdoors (ni [intel](faq.html#intel)
ni [AMD](faq.html#amd) se salvan)
además de estar lleno de bugs. El proyecto Libreboot fue fundado en Diciembre del año 2013,
con el propósito de hacer el firmware de coreboot accesible para usuario no técnicos.

Libreboot es un proyecto de *software libre*, con el enfoque de ayudar a los usuarios
a escapar del firmware de arranque propietario; Nosotros mismos queremos vivir en un
mundo donde todo el software sea [libre](https://writefreesoftware.org), y por ello,
Libreboot es un esfuerzo por lograr ese mundo. A diferencia de las grandes corporaciones,
no buscamos limitarte de ningún modo, ni te vemos como una amenaza de seguridad;
Consideramos que la capacidad de estudiar, modificar y redistribuir el
software de manera libre debe ser un derecho humano que todos deberían tener.
Aplicado a las computadoras, estos son productos que ya compraste, y por lo tanto
deberías tener la libertad de modificarlas de la manera que te apetezca.
Cuando vez a Intel hablar de su *Boot Guard* (que previene coreboot de funcionar computadores
modernos, ya que solo permite ejecutar firmware firmados por ellos) ú otros fabricantes
imponiendo reestricciones similares (te estoy viendo PSB de AMD), y les escuchas hablar de "seguridad",
solo se refieren a la seguridad de "ellos", no a la tuya. En el proyecto Librebot,
es al revés; Vemos el Intel Boot Guard y tecnologías similares como un ataque
a tu libertad y tu propiedad (en este caso tu computadora), y por ello, hicimos
nuestra misión el ayudarte a [recuperar](docs/install/deguard.html) [de vuelta](https://trmm.net/TOCTOU/)
el control de dicha libertad.

¡Libreboot no es un fork de coreboot!
-----------------------------------

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.vimuser.org/uboot.png" /><span class="f"><img src="https://av.vimuser.org/uboot.png" /></span>

De hecho, Libreboot intenta mantenerse tan cercano al coreboot de *stock*
como nos es posible para cada placa soportada, pero con distintos tipos de
configuraciones proporcionadas automaticamente por el sistema de compilación
de Libreboot (lbmk)

Del mismo modo que *Alpine Linux* es una *Distribución de Linux* (tambien llamadas distros de Linux),
Libreboot es una *distribución de coreboot*. Si quieres compilar una ROM desde cero,
tendrías que realizar configuraciones de nivel experto de coreboot, GRUB y
cualquier otro software que necesites. Con *Libreboot*, puedes literalmente solo
descargar el código fuente de GIT (u otra alternativa), y correr un simple script que
compilará ROMs completas. Un sistema de compilación automatizada, llamado
`lbmk` (Libreboot MaKe), compila estas estas ROMs automaticamente, sin la necesidad
de ningúna internvención del usuario. El trabajo duro ya fué hecho.

Si fueras a compilar con el coreboot regular, sin usar el sistema de compilación
automatizada de Libreboot (lbmk), requerirías de mucha más intervención y una
base de conocimiento técnico para generar una configuración funcional.
Eso sin contar que aún tienes el riesgo de que algo salga mal en el proceso.

Los lanzamientos regulares de binarios de Libreboot proporcionan estas
ROMs pre-compiladas, y pues simplemente instalarlas, sin la necesidad
de conocimiento especial o habilidades extraordinarias, excepto por
la habildad de seguir [instrucciones simplificadas, escritas para usuarios
finales/no técnicos](docs/install/).

### ¿Cómo puedo ayudar/involucrarme en el proyecto?

Hay una [página de tareas](tasks/) que lista las tareas que podrían (y serán) trabajadas.
Esta sera actualizada con el paso del tiempo conforme más tareas sean añadidas/completadas.
Si quieres ayudar puedes elegir una de estas y trabajar en ella.

<img tabindex=1 class="l" style="max-width:15%;" alt="GRUB boot loader in Libreboot" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

La *"manera"* definitiva en la que puedes ayudar es *agregar* nuevas placas madre a Libreboot,
esto se logra subiendo una configuración. Todo lo que esté soportado por coreboot puede ser
integrado a Libreboot, a modo de ROMs finales para los lanzamientos.

Revisa:

* [Aplica para ser un encargado/tester de placa](docs/maintain/testing.md)
* [Guía para portear nuevas placas madre](docs/maintain/porting.md)
* [Documentación del sistema automatizado de compilación de Libreboot (lbmk)](docs/maintain/)

Después de ello, viene el mantenimiento del sistema de compilación (revisar más arriba), y la
*documentación* que nos tomamos muy en serio. La documentación es crítica para cualquier
proyecto.

El *soporte de usuario* también es crítico. Date una vuelta por las salas de chat de IRC,
y si eres lo suficientemente competente para ayudar a alguien con los problemas que tengan
(o estás dispuesto a aprender junto a ellos), dar apoyo también es un gran servicio que ofrecer
al proyecto.

Puedes revisar la lista de bugs en el
[sistema de seguimiento de bugs](https://codeberg.org/libreboot/lbmk/issues).

Si encuentras un bug y sabes como arreglarlo, [hay instrucciones para enviar
parches](git.md), tambien es posible solo reportarlos. Además, este sitio web
está escrito enteramente en Markdown y hosteado en un 
[repositorio por separado](https://codeberg.org/libreboot/lbwww)
donde puedes enviar parches.

Cualquier discusión acerca del desarrollo y soporte de usuario se hacen mediante
los canales de IRC. Más información en la [página de contacto](contact.md).

### libreboot.org necesita traducciones.

Por el momento, Libreboot tiene paginas con traducción al ucraniano y francés
(pero no para todas las páginas en cualquiera de los lenguajes).

Si quieres ayudar con traducciones, puedes: traducir páginas sin soporte al idioma,
actualizar traducciones ya existentes, y subir tus propias versiones de traducciones.
Para obtener instrucciones, por favor lee:

[¿Cómo subir traducciones para libreboot.org?](news/translations.md)

Incluso si alguien ya está trabajando en traducciones para algún lenguaje específico,
siempre viene bien el apoyo de más personas. ¡Entre más mejor!

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"

Un fork paralelo a Libreboot, [Canoeboot](https://canoeboot.org) existe.
Tanto Canoeboot como Libreboot son mantenidos por la misma desarrolladora (Leah Rowe).

La versión de Canoeboot de esta página es: <https://canoeboot.org/>

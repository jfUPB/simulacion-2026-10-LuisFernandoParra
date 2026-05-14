# Unidad 8

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 
# Herramienta elegida

La herramienta principal utilizada para el desarrollo de la pieza fue **Three.js**, una librería de JavaScript orientada a la creación de experiencias visuales e interactivas en tiempo real dentro del navegador mediante WebGL.

La elección de esta herramienta fue una decisión consciente relacionada directamente con mi interés profesional en el diseño de experiencias interactivas e inmersivas. Me interesaba trabajar una pieza que combinara narrativa audiovisual, movimiento espacial, audio-reactividad y exploración tridimensional dentro de un entorno interactivo.

Three.js me permitió integrar múltiples elementos vistos durante el semestre, como sistemas dinámicos, partículas, oscilaciones, movimiento procedural y comportamiento audiovisual reactivo, dentro de una experiencia continua y cinematográfica.

Además, la herramienta ofrece un alto nivel de control sobre:
- cámaras,
- iluminación,
- sistemas de partículas,
- modelos 3D,
- audio,
- transformaciones espaciales,
- y renderizado en tiempo real.

Esto fue importante porque la intención del proyecto no era solamente construir una animación, sino diseñar una experiencia inmersiva donde el usuario sintiera una transición progresiva desde un espacio cotidiano hacia un viaje audiovisual espacial.

También se utilizaron herramientas complementarias como:
- JavaScript para la lógica interactiva,
- WebGL mediante Three.js para renderizado,
- modelos `.glb` para los entornos 3D,
- y análisis de audio en tiempo real mediante `THREE.AudioAnalyser`.

La selección de esta herramienta dialoga directamente con mis intereses en experiencias digitales interactivas, visuales en tiempo real y construcción de entornos audiovisuales inmersivos.
# Sistema transferido

El sistema transferido corresponde principalmente a un sistema de movimiento dinámico y audio-reactivo basado en vectores, oscilación y sistemas de partículas, originalmente estudiados durante el curso mediante ejercicios de motion systems, fuerzas, comportamiento procedural y visualización reactiva al sonido.

Durante el proyecto, estos conceptos fueron reconstruidos y adaptados dentro de Three.js para crear una experiencia espacial inmersiva en tiempo real.

Los principales sistemas transferidos fueron:

- movimiento basado en vectores y desplazamiento en ejes XYZ,
- oscilaciones sinusoidales aplicadas a cámara y entorno,
- sistemas de partículas dinámicas,
- comportamiento procedural dependiente del tiempo,
- y audio-reactividad mediante análisis de frecuencias.

Por ejemplo, el sistema de partículas RGB utiliza comportamiento colectivo y actualización constante de posición para generar sensación de velocidad y viaje espacial. A su vez, los movimientos orbitales de cámara utilizan funciones sinusoidales (`Math.sin` y `Math.cos`) para producir desplazamientos orgánicos y sensación de flotación.

También se transfirió el concepto de audio-reactividad trabajado en clase mediante el uso de `THREE.AudioAnalyser`, permitiendo que variables visuales como blur, brillo, contraste, velocidad y profundidad reaccionaran dinámicamente a las frecuencias de la música.

La transferencia no consistió en copiar literalmente los ejercicios realizados en clase, sino en reinterpretar sus principios dentro de una experiencia audiovisual narrativa y cinematográfica desarrollada en un entorno tridimensional interactivo.
# Contexto profesional concreto

La pieza fue diseñada como una experiencia audiovisual inmersiva pensada para funcionar como visual introductoria o interludio escénico dentro de un concierto, festival musical o performance en vivo relacionado con música electrónica, trap psicodélico o espectáculos audiovisuales de gran formato.

El proyecto toma inspiración de visuales utilizados en conciertos y experiencias performativas contemporáneas, donde la combinación entre sonido, movimiento, iluminación y entorno digital construye una narrativa sensorial más allá de una simple proyección decorativa.

La intención fue desarrollar una pieza que pudiera integrarse profesionalmente en contextos como:
- apertura de shows musicales,
- visuales de escenario,
- instalaciones inmersivas,
- experiencias de mapping,
- o piezas experimentales para portafolio audiovisual interactivo.

La experiencia está construida para generar una transición progresiva desde un espacio cotidiano hacia un entorno espacial abstracto y psicodélico, utilizando movimiento procedural, sistemas reactivos al audio y composición cinematográfica para producir sensación de inmersión.

Además de funcionar como pieza artística, el proyecto también responde a un interés profesional relacionado con el diseño de experiencias interactivas y visuales en tiempo real, explorando posibilidades cercanas al diseño escénico digital, visuales para conciertos y experiencias inmersivas audiovisuales.
# Concepto visual

El concepto visual de la pieza parte de la transformación progresiva de un entorno cotidiano hacia una experiencia espacial psicodélica e inmersiva, utilizando movimiento, sonido y deformación visual como elementos narrativos.

La propuesta busca representar la sensación de abandonar la realidad física y entrar en un viaje audiovisual construido mediante velocidad, partículas, oscilaciones y reacción al sonido. El espacio deja de funcionar como un lugar estable y comienza a comportarse como un sistema dinámico influenciado por la música y el movimiento de cámara.

Visualmente, la pieza toma referencias de:
- visuales de conciertos,
- ciencia ficción,
- estética psicodélica,
- simulaciones espaciales,
- y experiencias audiovisuales inmersivas.

La narrativa no está construida mediante personajes o diálogos, sino mediante transformaciones visuales progresivas:
- el salón comienza a deformarse,
- la iluminación cambia,
- aparecen partículas y líneas RGB,
- el entorno desaparece,
- y finalmente el usuario atraviesa un viaje espacial hasta llegar a una transición cinematográfica final.

El uso de blur, contrastes intensos, movimientos orbitales y partículas audio-reactivas busca generar sensación de vértigo, inmersión y expansión espacial.

Más que representar un lugar específico, el proyecto intenta construir una experiencia emocional y sensorial basada en el comportamiento dinámico de la imagen y el sonido en tiempo real.
# Explicación de transferencia

La transferencia consistió en adaptar los principios de movimiento, sistemas dinámicos y audio-reactividad trabajados inicialmente en ejercicios de p5.js hacia un entorno tridimensional desarrollado en Three.js.

En p5.js, muchos de los ejercicios del curso se enfocaban en comportamiento procedural mediante vectores, oscilaciones, partículas y relaciones entre movimiento y sonido dentro de un espacio principalmente bidimensional. Durante este proyecto, esos mismos principios fueron reinterpretados y reconstruidos en un entorno 3D interactivo y cinematográfico.

Por ejemplo, los sistemas de partículas vistos en clase fueron transferidos mediante la creación de miles de líneas y puntos distribuidos en el espacio tridimensional, actualizados constantemente en el loop principal para generar sensación de velocidad y profundidad espacial.

Los movimientos oscilatorios trabajados mediante funciones sinusoidales (`Math.sin` y `Math.cos`) también fueron transferidos al comportamiento de cámara y entorno, permitiendo crear movimientos orbitales y sensación de flotación dentro de la exploración espacial.

El principio de audio-reactividad se trasladó utilizando `THREE.AudioAnalyser`, que permitió convertir frecuencias musicales en datos numéricos utilizados para modificar:
- blur,
- brillo,
- contraste,
- velocidad de partículas,
- intensidad visual,
- y percepción espacial en tiempo real.

La lógica central del sistema se mantuvo:
- lectura de datos,
- actualización constante,
- modificación de variables,
- y comportamiento procedural dependiente del tiempo y del audio.

Sin embargo, la principal transformación ocurrió en el contexto visual y técnico, pasando de ejercicios experimentales aislados en p5.js a una experiencia audiovisual inmersiva desarrollada dentro de un entorno tridimensional en tiempo real usando Three.js y WebGL.
# Pieza final resuelta

La pieza final consiste en una experiencia audiovisual inmersiva desarrollada en Three.js en tiempo real, diseñada como una visual cinematográfica para concierto o performance audiovisual.

La experiencia inicia en un entorno cotidiano representado por un salón de clase tridimensional y, progresivamente, transforma el espacio mediante alteraciones visuales, partículas, cambios de iluminación, oscilaciones de cámara y sistemas audio-reactivos hasta convertirse en un viaje espacial psicodélico.

La pieza integra:
- modelos 3D,
- movimiento procedural,
- sistemas de partículas,
- audio-reactividad,
- transiciones temporales,
- efectos visuales dinámicos,
- y composición espacial cinematográfica.

A lo largo de la experiencia, la música modifica distintos parámetros visuales como:
- blur,
- contraste,
- brillo,
- velocidad de partículas,
- y percepción espacial,
generando una relación constante entre sonido e imagen.

El recorrido culmina en una transición fullscreen hacia una pieza audiovisual final integrada como cierre narrativo y escénico de la experiencia.

La obra fue resuelta como una experiencia completa y funcional en tiempo real, priorizando:
- inmersión,
- narrativa visual,
- comportamiento dinámico,
- estabilidad técnica,
- y coherencia audiovisual.

Más allá de un ejercicio técnico aislado, la pieza busca funcionar como una propuesta audiovisual aplicable a contextos reales de visuales escénicos, instalaciones inmersivas o portafolio profesional orientado a experiencias interactivas.
# Estrategia de presentación coherente con la herramienta

La pieza será presentada mediante una ejecución en tiempo real directamente desde el navegador utilizando Three.js, permitiendo mostrar el comportamiento dinámico e interactivo del sistema tal como fue diseñado originalmente.

La presentación está pensada como una experiencia audiovisual continua donde el público pueda observar:
- la transformación progresiva del entorno,
- los sistemas de partículas,
- el movimiento espacial de cámara,
- la reacción visual al sonido,
- y las transiciones cinematográficas en tiempo real.

Debido a que la pieza depende de audio-reactividad, movimiento procedural y renderizado dinámico, se decidió evitar una exportación prerenderizada completa y mantener la ejecución en vivo para conservar el comportamiento reactivo del sistema.

La estrategia de presentación incluye:
- ejecución fullscreen,
- reproducción sincronizada de audio y visuales,
- navegación automática de la experiencia,
- y transición final hacia video cinematográfico integrado.
# Moodboard o referencias
<img width="1080" height="1350" alt="Black White and Red Modern Fashion Moodboard Instagram Post (1)" src="https://github.com/user-attachments/assets/2eda37fc-a8bb-4b42-91bd-734b10bb5c48" />

# Mapa de decisiones

| Decisión | Razón conceptual | Relación con el sistema |
|---|---|---|
| Uso de Three.js como herramienta principal | Permitir una experiencia inmersiva tridimensional en tiempo real | Transferencia de sistemas dinámicos y movimiento espacial hacia un entorno 3D |
| Construcción de un viaje espacial audiovisual | Generar una narrativa sensorial e inmersiva | Uso de vectores, desplazamiento y comportamiento procedural |
| Uso de partículas RGB | Representar velocidad, profundidad y transición espacial | Aplicación de sistemas de partículas dinámicas |
| Movimiento orbital de cámara | Crear sensación de flotación y exploración | Uso de oscilaciones sinusoidales (`Math.sin` y `Math.cos`) |
| Audio-reactividad mediante `AudioAnalyser` | Relacionar sonido e imagen en tiempo real | Aplicación de análisis de frecuencias y comportamiento reactivo |
| Uso de blur, brillo y contraste dinámico | Intensificar percepción psicodélica y emocional | Variables visuales controladas por el audio |
| Transición progresiva del salón al espacio | Construir una transformación narrativa visual | Narrativa generativa basada en reglas temporales |
| Uso de modelos 3D `.glb` | Aumentar inmersión y profundidad espacial | Integración de entornos tridimensionales interactivos |
| Transición final hacia video fullscreen | Crear un cierre cinematográfico y performativo | Integración audiovisual y continuidad narrativa |
| Ejecución en tiempo real desde navegador | Mantener comportamiento dinámico y reactivo | Coherencia con sistemas interactivos audiovisuales |
# Mapa de presentación

| Elemento | Función dentro de la presentación |
|---|---|
| Computador portátil | Ejecutar la experiencia desarrollada en Three.js desde el navegador |
| Proyector | Mostrar la pieza audiovisual en gran formato para aumentar la inmersión visual |
| Sistema de sonido / bafle | Reproducir la música y el audio del concierto sincronizados con la experiencia visual |
| Navegador web | Ejecutar el renderizado en tiempo real y las transiciones de la pieza |
| Botón de activación de audio | Permitir la reproducción del audio del video final debido a restricciones de autoplay del navegador |
| Ejecución fullscreen | Mantener una experiencia visual continua y cinematográfica |
| Reproducción lineal de la pieza | Mostrar el recorrido completo desde el salón inicial hasta la transición audiovisual final |
| Integración audio + imagen | Generar sincronización entre música, movimiento, partículas y efectos visuales |
| Presentación pública proyectada | Adaptar la pieza a un formato cercano a visuales de concierto o performance audiovisual |

# Uso explícito de IA como materializador

Durante el desarrollo del proyecto se utilizó inteligencia artificial principalmente como herramienta de apoyo técnico para:
- corrección de errores,
- comprensión de funciones de Three.js,
- y búsqueda de soluciones relacionadas con renderizado y reproducción multimedia.

La IA también fue utilizada ocasionalmente para consultar referencias visuales y resolver diferencias técnicas entre ciertos comportamientos de p5.js y Three.js durante el proceso de transferencia del sistema.

Sin embargo, la integración completa del proyecto, la estructura de la experiencia, la implementación general del código, las decisiones visuales, el comportamiento de la pieza y la construcción conceptual fueron realizadas personalmente.

La inteligencia artificial funcionó únicamente como soporte técnico puntual dentro del proceso de desarrollo y depuración del proyecto.
# Código, archivo, proyecto o documentación técnica según la herramienta
https://drive.google.com/drive/folders/1iumycOTo4R-bRjA_OUQpz-KJHl9qhv7i?usp=sharing

# Registro visual de la pieza
https://youtu.be/xS5F1IJB_UU

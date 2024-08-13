## Cifrado en un sistema distribuido de intercambio de archivos
Ouisync ofrece a los usuarios una forma segura de compartir y sincronizar datos
entre dispositivos. Debido a la naturaleza distribuida (peer-to-peer) de
Ouisync, donde es posible realizar cambios simultáneos en archivos y
directorios, la estructura de directorios de Ouisync es bastante elaborada.
Siempre que dos o más usuarios modifiquen un archivo en un directorio
simultáneamente, la arquitectura de Ouisync garantiza que no se pierda
información. Además, Ouisync también protege el contenido (archivos y
repositorios) y la estructura de sus sistemas de archivos mediante la
implementación de cifrado de extremo a extremo.

Al compartir un repositorio en un modo específico (escritura, lectura o modo
oculto), Ouisync genera claves (llamadas "tokens") que puedes compartir con tus
peers/nodos (dispositivos), ya sea como enlaces o como códigos QR. Importar un
repositorio mediante un token le da a tu dispositivo la capacidad de descifrar
sus directorios y archivos (excepto en el modo oculto).

Los datos se cifran tanto _en reposo_ (cuando simplemente están almacenados)
como _en tránsito_ (durante la transferencia de datos). Es importante destacar
que Ouisync puede sincronizar sin descifrar y ningún dispositivo necesita
conocer la clave de descifrado para realizar la sincronización. Los nombres de
los archivos, el contenido de los archivos e incluso los tamaños de los archivos
y la estructura de los directorios se ocultan a los peers/nodos (dispositivos)
que no poseen una clave de cifrado. Por lo tanto, los peers /nodos
(dispositivos) que solo tienen acceso en modo oculto a sus repositorios no
pueden ver ni el contenido de sus repositorios ni su estructura.

## ¿Qué algoritmos de cifrado se utilizan?
* _En tránsito_, Ouisync utiliza el framework del [protocolo
  Noise](https://noiseprotocol.org/), en particular el [patrón
  NNpsk0](https://noiseprotocol.org/noise.html#pattern-modifiers). Esto permite
  a Ouisync generar claves efímeras con una clave precompartida. La clave
  precompartida en Ouisync es el ID del repositorio. Noise admite autenticación
  mutua y opcional, ocultamiento de identidad, secreto de reenvío, cifrado cero
  de ida y vuelta (zero round-trip encryption) y otras funciones criptográficas
  avanzadas.
*   _En reposo_, Oyisync cifra los datos utilizando
    [ChaCha20](https://en.wikipedia.org/wiki/Salsa20#ChaCha_variant). En este
    caso, se utiliza la "Clave de lectura" como clave simétrica de
    cifrado/descifrado. Las claves se autentican utilizando firmas Ed25519, con
    la "Clave de escritura" como clave privada.
*   Para el _hashing_, Ouisync se basa en la función hash
    [BLAKE3](https://en.wikipedia.org/wiki/BLAKE_(hash_function)#BLAKE3), que se
    [considera](https://github.com/BLAKE3-team/BLAKE3-specs/blob/master/blake3.pdf)
    como consistentemente más rápida en diferentes plataformas y tamaños de
    entrada.

## ¿Qué es un Bloque?
Cada archivo y cada directorio almacenado en Ouisync se divide en bloques
relativamente pequeños (p. ej., 32 KB) de tamaño constante. Cada bloque tiene un
ID de bloque (generado mediante un generador de números aleatorios) que ayuda a
Ouisync a identificar estos bloques. Todos los bloques se almacenan junto con un
archivo llamado "localizador". El localizador es una especie de "mapa" que
indica dónde se encuentra cada bloque con respecto a otros bloques. Sin embargo,
para no revelar esta estructura a los agentes que no poseen la clave secreta,
los localizadores no se almacenan directamente, sino que se codifican.

_Imagina que organizas una gran fiesta de bodas, a la que invitas a muchos
invitados. Quienes ya han organizado este tipo de eventos saben lo difícil que
es asignar los asientos correctos a todos los invitados, teniendo en cuenta sus
relaciones, intereses, etc. Por cierto, también tienes que comunicar la
información a los camareros, que tienen que estar atentos y recordar qué
invitados tienen alergias o preferencias en la dieta. Y como tus invitados son
VIP, no quieres revelar sus nombres reales a los camareros, así que inventas
seudónimos aleatorios y los escribes en esas bonitas tarjetas de asignación de
asientos. Así que, si nos atenemos a esta metáfora, el ID del bloque sería un
seudónimo escrito en una tarjeta al lado del asiento de tu invitado, y el
“localizador” sería un mapa de todas las mesas con asientos correctamente
asignados._

![image](https://github.com/willow446/willow446.github.io/assets/1790886/06985a87-2dac-49a2-99ae-37725bd8e2ce)


## ¿Qué es un blob?
Un conjunto lineal de bloques se denominará blob. Los blobs pueden representar
archivos y directorios. El blob de archivo es el más simple: consta de un
encabezado que contiene el tamaño del archivo, los permisos y una marca de
tiempo. El blob de directorio representa una lista de nombres de archivos
presentes en un directorio, así como localizadores que apuntan a los blobs de
archivo individuales.

## ¿Cómo se realiza la sincronización?
Cuando compartes un repositorio con tu peer/nodo (dispositivo), esto crea una
"réplica" de tu repositorio. La estructura del repositorio se almacena en los
llamados archivos de "índice/index": cuando los dispositivos peers/nodos se
conectan, primero intercambian esos índices. Si se ha modificado algo en una de
las réplicas, Ouisync descargará los bloques faltantes. Ouisync siempre descarga
primero los directorios y solo después los archivos en sí. Esto ayuda a Ouisync
a reconstruir correctamente tus datos a partir de bloques sin estropearlos.
Además, esto se hace sin filtrar información a los usuarios que no tienen acceso
de "lectura" a tus repositorios.

No tienes que preocuparte por los conflictos entre las distintas réplicas: en el
backend, la sincronización se realiza de tal manera que se evitan conflictos y
divergencias. Lo que ve cuando abre Ouisync es lo que llamamos una
“instantánea”: su vista de todo el árbol de directorios en un momento
determinado. Cada modificación del sistema de archivos (en su dispositivo o en
los dispositivos de sus peers/nodos) da como resultado una nueva “instantánea”.

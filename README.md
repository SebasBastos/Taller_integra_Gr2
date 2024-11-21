# Sistema de rastreo de bicicletas utilizando un APRS Tracker
- Grupo 2
- Sebastián Bastos Salas, Brayan Montenegro Elizondo y Andrew Quirós Rodríguez

## Introducción 

Este proyecto tiene como objetivo implementar un sistema de rastreo en tiempo real para bicicletas dentro del campus del Instituto Tecnológico de Costa Rica (ITCR), utilizando la tecnología APRS (Automatic Packet Reporting System) y dispositivos de rastreo Lilygo model: LORA32 v1.2. Esta iniciativa busca facilitar el monitoreo y la localización precisa de las bicicletas en cualquier momento, permitiendo gestionar el sistema de bicicletas compartidas de manera más eficiente y segura.

Para el desarrollo de este sistema, se han establecido una serie de requerimientos técnicos que incluyen la utilización de baterías de litio de alta capacidad para extender la autonomía, antenas de alta ganancia para asegurar la cobertura de señal en todo el campus, y carcasas resistentes al clima para proteger los componentes electrónicos. Además, el proyecto contempla la integración de un sistema de alquiler para maximizar el acceso y uso de las bicicletas en la comunidad universitaria.

Durante la fase de pruebas, se realizarán mediciones de tiempos de carga y descarga de las baterías para optimizar la autonomía del sistema, así como pruebas de rastreo que permitirán verificar la precisión y efectividad de la localización en la página de APRS. Esta implementación no solo ofrece una herramienta de movilidad sostenible para el campus, sino también una experiencia práctica en el uso de tecnologías de comunicación IoT para los involucrados en el proyecto.

### Funcionamiento del Tracker

Para comprobar el funcionamiento del dispositivo de rastreo se inicia configurando el dispositivo mediante la plataforma Visual Studio Code con la extensión de PlatformIO para ingresar el código que establecerá los parámetros necesarios del sispositivo en cuanto a frecuencia, nombre y comunicación. Este código es tomado desde el repositorio de Ricardo Guzman.

Se observa en la figura 1 el funcionamiento correcto del dispositivo de rastreo a lo largo de las afueras del ITCR, este pierde presición en los momentos que hay interferencia de edificios o arboles cerca que no permiten una comunicación correcta con la estación ubicada en el volcán Irazú.

## Diagramas de Nivel

En esta sección se presentan los diagramas que describen la arquitectura y el funcionamiento del sistema de rastreo de bicicletas utilizando APRS. Cada nivel del diseño detalla los componentes importantes, sus interacciones y el flujo de información, proporcionando una visión jerárquica y estructurada del sistema.

### Primer Nivel

Aquí se presenta una vista generalizada del sistema. Donde se puede observar en la figura 2 como el bloque del sistema APRS recibe información de la antena y se alimenta con una fuente de poder y como la salida se presenta como el mapa en linea para observar la ubicación de las bicicletas.

### Segundo Nivel

En este nivel se profundiza en el sistema descrito en el primer nivel. En este caso, el dispositivo de rastreo se descompone en sus componentes principales. La información es recibida a través de la antena el cual conecta con el receptor GPS el cual lo envía al demodulador APRS para que la computadora pueda procesar la información y finalmente transmitirla.

### Tercer Nivel

En este punto se entra en mayor detalle de los componentes que componen los bloques principales del sistema. En el bloque LoRa32 se observa como al receptor GPS ingresa la información recibida de la antena donde luego se demodula y se interpreta en el MPU (Main processing unit). Luego en el RPU (Real-time processing unit) se hace el muestreo de los datos para que se puedan procesar más adelante y ser transmitidas al servidor.

### Cuarto Nivel

Por último aquí se tiene un enfoque más orientado a la aplicación donde se incluye la bicicleta con el sistema de anclaje y la caja protectora para el tracker. El cual ahora también se muestra como debe contener una batería externa para alargar el tiempo de funcionamiento del tracker. Esta caja debe asegurarse de proteger de impactos y de mantener el agua fuera del sistema electrónico. Una vez esta es procesada como se describió en el nivel anterior, se transmite a un servidor que se maneja a través de una aplicación que solicitará la autenticación para finalmente observar la ubicación en tiempo real de la bicicleta.

### Diagrama de Flujo

El diagrama de flujo presentado en la Figura 6 detalla el funcionamiento lógico del sistema de rastreo. Este comienza con la verificación del estado funcional de la bicicleta (incluyendo batería y cobertura). Si todos los componentes están operativos, el sistema utiliza el tracker para iniciar la transmisión de datos mediante una API. En caso de fallas, como la falta de cobertura o batería, el usuario debe reportar el problema. El sistema realiza un seguimiento continuo de la ubicación hasta que la sesión es terminada, permitiendo un monitoreo preciso.

### Diagrama de Estados

El diagrama de estados en la Figura 7 describe el ciclo de vida de una bicicleta dentro del sistema. Comienza en estado Stand By hasta que se detecta un usuario válido, momento en el que se desbloquea y activa la bicicleta. Durante su uso, el sistema monitorea estados como batería baja, movimiento no autorizado o salida de cobertura. Si la bicicleta vuelve a la cobertura o el usuario termina su uso, se regresa al estado inicial.

## Bill of Materials

### Componentes Clave del Sistema APRS 

El sistema de rastreo de bicicletas está compuesto por los siguientes materiales principales:

- **TTGO Meshtastic T-Beam:** Es el dispositivo de rastreo GPS principal, equipado con tecnología LoRa y GPS, lo que permite un rastreo preciso y transmisión a largas distancias.
- **Batería Externa y Sistema BMS:** Una batería de ion de litio 18650, acompañada por un sistema de gestión de baterías (BMS) que protege contra sobrecargas y sobrecalentamientos, asegurando un funcionamiento seguro y eficiente.
- **Demodulador LoRa32:** Encargado de recibir las señales del dispositivo APRS y procesarlas para su transmisión eficiente hacia la base de datos central.
- **Unidades de Procesamiento MPU y RPU:** El MPU procesa los datos de sensores de movimiento, mientras que el RPU gestiona el análisis y almacenamiento, permitiendo la integración entre rastreo y comunicación.
- **Caja de Protección y Anclaje para Bicicleta:** Una caja metálica impermeable y resistente a impactos para proteger el TTGO y la batería externa en condiciones adversas.
- **Servidor Backend y Frontend:** El backend recopila los datos enviados por dispositivos LoRa, mientras que el frontend permite visualizar las posiciones en tiempo real mediante APIs de mapas como Google Maps o Leaflet.
- **API de Mapas en Tiempo Real:** Permite a los usuarios visualizar el movimiento de los dispositivos con actualizaciones en tiempo real en un mapa interactivo.

## Infraescructura Disponible 

La infraestructura a utilizar es la del proyecto BiciTec, esta se escoge por la facilidad de no tener que invertir en una nueva infraestructura y reutilizar la existente que fue diseñado para bicicletas, se utiliza tanto las estaciones de carga y reparación como las zonas de parqueo de bicicletas que se encuentran al rededor de todo el campus del ITCR, esto se observa en las figuras 8 y 9 respectivamente.

Para las estaciones de carga que se observa en la figura 8 se actualiza la plataforma y revisa el estado actual, se le añade cables para la carga de baterías de las bicicletas y se agregan adaptadores para corriente continua a la tensión necesaria de las baterías 18650.

Para los puestos de estacionamiento de bicicletas que no poseen puesto de carga, se utilizan para tener una mejor movilidad por todo el campus y que se pueda tener acceso desde más puntos, una vez que la bicicleta se encuentre con baja batería deberá emitir una alerta al usuario para que este pueda llevarlo a un puesto de carga cercano.

## Presupuesto

Para la implementación del proyecto se toma en cuenta el presupuesto de este, para ello se hace una tabla con los costos y cantidad de materiales que se van a utilizar por bicicleta. Adicional a estos costos de materiales se añaden costos por la hora de ingenieros, técnicos, mantenimiento y facturas de luz.

Al contemplar todos los gastos por materiales se da un costo de 669mil colones, 1 027 759 por temas de pagos de trabajo y mantenimiento, al agregar un 20\% por imprevistos da un total de 2 millones de colones por el diseño inicial del proyecto, esto da una perspectiva de si este proyecto puede ser viable o no. 

A este presupuesto anterior se le debe añadir el proceso de pruebas e implementación final. Donde las pruebas pueden durar más o menos dependiendo de como se encuentre la infraestructura y las baterías a utilizar, el tiempo de pruebas de la aplicación y de la plataforma APRS se trabajan por medio de una beta de la aplicación para probar como esta se comporta con el usuario final una vez hecho las pruebas por parte del equipo.

La implementación es la fase que más horas necesita, ya que esta debe de tener los requerimientos mínimos para que todos los usuarios puedan utilizarla, aquí se deben de contemplar las normas y estándares que son necesarios para el lanzamiento de un proyecto como este.

## Testbench

Las siguientes pruebas son fundamentales para evaluar el rendimiento del sistema:

- **Pruebas de Carga de la Batería:** Determinan el tiempo necesario para cargar completamente la batería 18650 y validan la efectividad del sistema BMS.
- **Pruebas de Descarga de la Batería:** Miden la duración de funcionamiento continuo del dispositivo TTGO con una carga completa.
- **Capacidad de Dispositivos Soportados:** Evalúan cuántos trackers pueden conectarse simultáneamente al sistema sin congestión o pérdida de datos.
- **Transmisión de Datos LoRa:** Verifican la capacidad de transmisión en diferentes distancias y condiciones climáticas.
- **Consumo Energético:** Analizan el consumo de energía en modo de transmisión para identificar oportunidades de optimización.

## Casos Extremos

Para cubrir los posibles escenarios en los que se puede enfrentar el proyecto en un entorno real, se hace una lista de los posibles casos extremos a los que se puede enfrentar.

- **Pérdida de comunicación por cobertura insuficiente:** Esto se puede dar en caso de que el tracker se encuentre cerca de algún edificio, arboles o infraestructura que no permita una comunicación directa con la antena del volcán Irazú, el proyecto debe ser capaz de guardar la ultima ubicación de la bicicleta y seguir enviando datos hasta que se reanude la conexión.
- **Daño físico al tracker (por accidente o vandalismo):** En caso de que suceda algún daño en el dispositivo, este debe ser capaz de enviar datos de diagnostico para saber qué error tiene o si se abrió el case donde se encuentra el dispositivo de rastreo. Para evitar estas situaciones se agrega un case de seguridad al dispositivo y se esconde dentro de la bicicleta.
- **Fallo de la batería:** El dispositivo debe enviar datos del estado de la batería, saber si está con baja batería o si se generó un error en la carga de la batería que ocasione un posible deterioro de la batería.
- **Fallo del GPS:** Tener un sistema redundante para la ubicación del dispositivo y tener antenas con mayor ganancia y potencia para compensar las perdidas de señal del GPS.
- **Sobrecarga o fallo del servidor APRS:** Contar con un servidor por aparte en caso de fallas, y hacer pruebas de sobre carga al servidor para determinar el límite que puede soportar de usuarios en un lapso de tiempo.
- **Acceso no autorizado o sabotaje:** Tener un sistema de protección en caso de que se quiera reprogramar el dispositivo de rastreo o cambiar su configuración.
- **Cambio en las condiciones ambientales (clima extremo):** En caso de fuertes lluvias, temperaturas o condiciones de humedad se debe proteger al equipo, por este motivo se hace  un case que pueda protegerlo de esto.

## Conlusiones

El sistema de rastreo APRS diseñado cumple con los objetivos planteados de monitoreo en tiempo real y gestión eficiente de las bicicletas compartidas en el campus del ITCR. Los componentes seleccionados, como el TTGO Meshtastic T-Beam y el sistema LoRa32, han demostrado ser adecuados para garantizar un rastreo estable y preciso, incluso en condiciones adversas. 

Las pruebas realizadas, que incluyeron evaluaciones de carga y descarga de la batería, capacidad de transmisión y consumo energético, validaron la operatividad del sistema, confirmando su viabilidad para un uso continuo y confiable. Además, la implementación de una API de mapas en tiempo real proporciona una experiencia de usuario mejorada, permitiendo una visualización clara y actualizada de la ubicación de las bicicletas.

En conclusión, este proyecto no solo demuestra el potencial de las tecnologías IoT aplicadas a la movilidad sostenible, sino que también ofrece una solución práctica, eficiente y escalable para la comunidad universitaria. El diseño propuesto es una herramienta innovadora que contribuye significativamente a mejorar la experiencia de uso y administración de bicicletas compartidas en el campus.

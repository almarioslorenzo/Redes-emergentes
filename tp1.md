## UNIVERSIDAD AUTÓNOMA DE ENTRE RÍOS - FACULTAD DE CIENCIA Y TECNOLOGÍA - INGENIERÍA EN TELECOMUNICACIONES

# TRABAJO PRÁCTICO 1
# REDES EMERGENTES Y CONECTIVIDAD MODERNA
| Concepto | Detalle |
| :--- | :--- |
| **Materia** | Servicios de Telecomunicaciones y Redes |
| **Año** | 2026 |
| **Modalidad** | Grupal (2-3 integrantes) |
| **Duración** | 3 semanas |
| **Entrega** | Martes 31/3 (defensa: miércoles 1/4) |
| **Integrantes** | Alma Ríos, Gastón Barreto |

### FASE 1: Problema disparador
------------------------
#### **Preguntas al cliente**
1. **Tenemos zonas con 50 personas en 100m2. ¿WiFi 6 alcanza o necesitamos WiFi 7?**

Para determinar la mejor opción, primero debemos entender que 50 personas en un espacio de 100 m2 clasifica claramente como un entorno de alta densidad.

Si bien Wi-Fi 7 es la novedad, Wi-Fi 6 (802.11ax) fue diseñado precisamente con el objetivo principal de mejorar la eficiencia en escenarios saturados, no solo de aumentar la velocidad punta. Gracias a tecnologías como OFDMA y MU-MIMO bidireccional, Wi-Fi 6 maneja múltiples dispositivos simultáneos de forma excelente. Hoy en día, casi todos los smartphones y laptops modernos son compatibles, lo que garantiza que aprovecharemos la red de inmediato.

Aunque Wi-Fi 7 ofrece mejoras increíbles como canales de 320 MHz y la modulación 4096-QAM, en este escenario específico presenta dos barreras críticas:
* Costo-Beneficio: El presupuesto requerido es considerablemente mayor y, para un grupo de 50 personas, el ancho de banda extra de Wi-Fi 7 podría quedar ocioso.
* Ecosistema: De nada sirve tener un router Wi-Fi 7 si los dispositivos de los usuarios no soportan el estándar; terminarían conectándose usando protocolos anteriores, desperdiciando la inversión.
![Comparación Wifi 6-7](https://www.profesionalreview.com/wp-content/uploads/2025/08/wifi-7-specs.png)


Para el caso concreto de TechHub Coworking Wi-Fi 6 es más que suficiente. Es una tecnología madura, diseñada para la alta densidad y mucho más accesible económicamente. Wi-Fi 7 es el futuro, pero para cubrir 100 m2 con 50 usuarios, estaríamos haciendo un desperdicio de recursos. Con Wi-Fi 6 cubrimos la necesidad técnica con eficiencia y responsabilidad presupuestaria.

2. **¿Cómo conecto las 3 sucursales de forma segura sin pagar un MPLS?**

El MPLS (Multi-Protocol Label Switching) funciona como una red privada física contratada a un proveedor. Es increíblemente rápida y segura porque el tráfico nunca toca la internet pública, pero tiene dos grandes problemas:
* Costo elevado: Requiere infraestructura dedicada y mantenimiento gestionado por el proveedor.
* Rigidez: Escalar o mover una sucursal es lento y costoso.

Para conectar nuestras sedes de forma económica, crearemos túneles cifrados sobre la conexión a internet que ya tenemos.
* Puertas de enlace: En cada sucursal instalamos un router o firewall que soporte VPN.
* Protocolo IPsec: Este es el estándar que usaremos. Se encarga de tres cosas vitales:
    * Cifrado: Los datos se vuelven ilegibles antes de salir a internet.
    * Integridad: Asegura que nadie modifique los datos en el camino.
    * Autenticación: Confirma que el remitente es realmente una de nuestras sucursales.

| Característica | MPLS | VPN |
| ---------- | ---------- | --------- |
| Costo | Muy alto (mensualidad + equipo) | Bajo (usa el internet actual) |
| Seguridad | Máxima (red privada aislada) | Alta (vía cifrado IPsec) |
| Velocidad | Garantizada (QoS) | Depende del ancho de banda de internet |
| Implementación | Compleja y lenta | Rápida y flexible |

Para el caso concreto de TechHub Coworking la VPN Site-to-Site es la mejor opción. Aunque el MPLS ofrece una privacidad superior por ser una conexión física dedicada, el cifrado moderno de las VPN es lo suficientemente robusto para proteger los datos corporativos a una fracción del costo. Para 3 sucursales, es la opción más inteligente y eficiente.

3. **¿Quiero automatizar las luces y el aire. Qué protocolo IoT me conviene? ¿Zigbee?  ¿Matter?**

Para automatizar un espacio profesional como un coworking, no solo buscamos que las luces "prendan y apaguen"; necesitamos un sistema estable, privado y escalable que no dependa de si el Wi-Fi está saturado o si la nube del fabricante se cae.

Zigbee lleva más de una década en el mercado y es, posiblemente, el protocolo más popular en el mundo IoT por varias razones:
* Bajo Consumo: Ideal para interruptores de luz o sensores que funcionan con batería.
* Red en Malla (Mesh): Cada bombilla inteligente conectada actúa como un repetidor, extendiendo la señal por toda la casa.
* Independencia del Wi-Fi: No satura tu router principal, lo que evita que el internet se ponga lento cuando conectas muchas luces.
* Madurez: Ya existen miles de productos económicos y probados que funcionan con Zigbee.
![Zigbee](https://www.profesionalreview.com/wp-content/uploads/2025/04/zigbee-1.jpg)

Matter no es exactamente un reemplazo de hardware, sino un estándar que usa el Protocolo de Internet (IP) para que dispositivos de distintas marcas (Apple, Google, Amazon) hablen el mismo idioma.
* Interoperabilidad: Promete que no necesitaremos un "hub" o concentrador diferente para cada marca.
* Seguridad: Al estar basado en IP, incorpora capas de seguridad muy modernas.
* El reto: Al ser un estándar lanzado apenas a finales de 2022, todavía está en una etapa de "pulido". Muchos dispositivos Matter siguen siendo más caros o difíciles de conseguir que los Zigbee.

Para que esta mezcla de protocolos funcione como un solo reloj suizo, necesitamos a Home Assistant como plataforma de gestión local.
* Adiós a las "islas tecnológicas": HA permite que una luz Zigbee de IKEA y un aire acondicionado Matter de otra marca se entiendan en un solo panel de control.
* Privacidad y control local: A diferencia de soluciones comerciales, HA no necesita internet para funcionar. Las automatizaciones (como apagar el aire si no hay nadie en una sala de reuniones) ocurren dentro de nuestra red, protegiendo los datos del coworking.
* Automatización inteligente: Nos permite crear reglas complejas, como: "Si el sensor de presencia no detecta movimiento en 15 minutos Y es después de las 20:00, apaga todo y activa la alarma".
![Home Assistant](https://www.redeszone.net/app/uploads-redeszone.net/2024/04/Home_assistant_domotica_casa.jpg)

Para el caso concreto de TechHub Coworking, siendo la prioridad la estabilidad y el presupuesto, la combinación ganadora es un servidor de Home Assistant con un adaptador Zigbee profesional. Esto nos da lo mejor de los tres mundos: la economía y estabilidad de Zigbee, la compatibilidad futura de Matter y la inteligencia centralizada de Home Assistant.

4. **¿Cómo separo el tráfico de invitados del de empleados y del IoT?**

Cuando tenemos una sola red para todo, un dispositivo de IoT (como una cámara o un termostato) con poca seguridad podría ser la puerta de entrada para que alguien acceda a las computadoras de los empleados. Para evitar esto, usamos las VLAN (Virtual LAN).

Una VLAN nos permite crear "muros lógicos" dentro de nuestra red. Aunque todos los dispositivos estén conectados al mismo switch físico, el switch los trata como si estuvieran en redes totalmente separadas.
![VLAN](https://acf.geeknetic.es/imgw/imagenes/auto/2025/5/15/epm-vlan-redes-empresariales.png?f=webp)

Para resolver este caso concreto, lo ideal es configurar tres redes independientes con reglas de tráfico específicas:
* VLAN 10 – Empleados: Es la red con mayor prioridad. Tiene acceso a los servidores internos, bases de datos e impresoras.
* VLAN 20 – Invitados: Solo tiene salida a Internet. Gracias a las VLAN, podemos aplicar políticas para que los invitados no saturen la red (por ejemplo, limitando el ancho de banda para streaming) y, lo más importante, que nunca puedan ver los equipos de la oficina.
* VLAN 30 – IoT: Reservada para sensores, aires acondicionados y cámaras. Esta red suele estar muy restringida: los dispositivos pueden reportar datos, pero no pueden iniciar conexiones hacia la red de empleados.

Ventajas técnicas de esta solución: 
* Seguridad: Si un dispositivo IoT es hackeado, el atacante se queda "atrapado" en esa VLAN sin poder saltar a la red contable o administrativa.
* Rendimiento: Reducimos el tráfico de "broadcast" (mensajes que los equipos mandan a toda la red), lo que hace que la conexión sea más ágil.
* Control de políticas: Podemos decidir, por ejemplo, que los invitados solo tengan internet de 9:00 a 18:00.

Para TechHub Coworking, implementar VLANs es la mejor forma de profesionalizar una red. No necesitamos comprar cables o switches por separado para cada grupo; con un switch gestionable, podemos dividir el hardware físico en tres redes virtuales seguras y eficientes. Es la solución estándar en la industria por su bajo costo y alta efectividad.

5. **Si se cae la VPN entre sucursales, ¿cómo nos enteramos rápido?**

Cuando una VPN entre sucursales se cae, el impacto es inmediato. No podemos esperar a que un usuario llame a quejarse; necesitamos automatización. Analizamos tres niveles de soluciones:

| Característica | Netwatch (Mikrotik) | Grafana + Prometheus|
| -------| -------- | --------- |
| Complejidad | Baja (configuración rápida) | Alta (requiere servidor dedicado) |
| Método | Verificación por ping | Recolección de métricas |
| Visualización | Consola | Dashboards interactivos |
| Notificación | Scripts (Telegram, Email) | Email | Web| Multiplataforma |
| Uso ideal | Reacciones rápidas en el router | Control de inventario y hardware | Monitoreo profesional de toda la IT |

Análisis de las opciones:
* Netwatch: Es ideal con routers MikroTik. Funciona enviando un "ping" constante a la IP del otro extremo de la VPN. Si falla, el router ejecuta un comando automático. Es la opción más liviana porque no requiere servidores externos para detectar el fallo.
![Netwatch](https://www.mikrotiklabs.com/wp-content/uploads/2019/08/Uso-de-Netwatch-1024x388.png)
* Grafana: Es el estándar en la industria para la visualización de datos. No solo nos avisa si la VPN cayó, sino que nos muestra gráficas de latencia y consumo de ancho de banda. Si la VPN se vuelve lenta antes de caerse, Grafana nos permitirá ver esa tendencia en tiempo real.
![Grafana](https://cdn.buttercms.com/9GGT1pHwQczrkjLCkWxc)

Para el caso concreto TechHub Coworking lo ideal es un enfoque híbrido:
* Usar Netwatch para la reacción inmediata: configurar un bot de Telegram que nos avise en menos de 10 segundos si el túnel cae. Es económico, rápido y eficiente.
* Implementar Grafana para la gestión a largo plazo: esto nos permitirá entender por qué se caen las conexiones (¿es por saturación?, ¿es siempre a la misma hora?) y así presentar informes profesionales a la dirección.

### FASE 2: Mapa de aprendizaje
| Ya sabemos  | Necesitamos aprender  | Cómo lo vamos a aprender |
| ------- | ------- | --------- |
| Arquitectura de Red Segura: Concepto de VPN Site-to-Site frente a MPLS y uso de protocolos IPsec | Implementación de Túneles IPsec: Configuración real de las fases de seguridad y el intercambio de llaves entre sucursales | Laboratorios prácticos usando máquinas virtuales o equipos de prueba |
| Segmentación de Tráfico: El concepto lógico de las VLANs para separar Invitados, Empleados e IoT | Administración de Switches Gestionables: Creación de etiquetas y configuración de puertos de acceso y troncales | Cursos rápidos en YouTube sobre "VLAN" y guías de configuración de switches |
| Protocolos Domóticos: La diferencia entre Zigbee (red mesh) y Matter (estándar IP) | Gestión de Home Assistant | Explorar la comunidad de Home Assistant |
| Estrategias de Disponibilidad: Herramientas de diagnóstico como Ping y Netwatch | Scripting y Automatización de Alertas: Cómo escribir el código para que Netwatch envíe un mensaje automático a un Bot de Telegram | Ver tutoriales sobre "MikroTik Scripting para Telegram" y documentación de la API de Bots de Telegram |
| Topologías básicas: Diseño de red en estrella y esquemas de conexión entre sucursales | Uso de simuladores avanzados (GNS3: Cómo montar el entorno virtual para testear la red antes de comprar el hardware | Documentación oficial de GNS3, foros de la comunidad y videotutoriales paso a paso |
| Fundamentos de Servidores: Diferencia entre hardware físico y servicios en la nube. | Virtualización de máquinas (VM Ware Fusion): Cómo virtualizar el router (MikroTik CHR) | Instructivos técnicos de virtualización y videos sobre configuración de máquinas virtuales en ARM64/x86 |

### FASE 3: Investigación dirigida
#### WiFi: estándares actuales
| Estándar | Nombre | Banda | Ancho max | Veloc. max | MIMO | Clave |
| ----- | ----- | ----- | ------- | -------- | ----- | -------- |
| **802.11n / WiFi 4** | WiFi 4 | 2.4/ 5 GHz | 40MHz | -600 Mbps | Si | Primer MIMO, mejora velocidad |
| **802.11ac** | WiFi 5 | 5 GHz | 160 MHz | ~3.5 Gbps | MU-MIMO | Mejor rendimiento en 5 GHz |
| **802.11ax** | WiFi 6 | 2.4 / 5 GHz | 160 MHz | ~9.6 Gbps | OFDMA + MU-MIMO | Mejor con muchos dispositivos |
| **802.11ax** | WiFi 6E | 2.4 / 5 / 6 GHz | 160 MHz | ~9.6 Gbps | OFDMA | Nueva banda 6 GHz |
| **802.11be** | WiFi 7 | 2.4 / 5 / 6 GHz | 320 MHz | ~46 Gbps | Multi-link | Ultra velocidad y baja latencia |

**Para el coworking de TechHub con 200 personas en 3 pisos: qué generación de WiFi  recomiendan y por qué? Investiguen las diferencias principales entre WiFi 6 y WiFi 7 desde  el punto de vista práctico: cuántos dispositivos soportan, qué bandas usan, cuánto cuestan  los APs. No necesitan entender la modulación interna; necesitan saber elegir.**

Tenemos 200 personas distribuidas en 3 niveles. Esto no es solo una cuestión de "señal", sino de capacidad de gestión de usuarios simultáneos.
| Característica | WiFi 6/6E | WiFi 7 |
| :--- | :--- | :--- |
| **Bandas de frecuencia** | 2.4 GHz y 5 GHz (6E suma la de 6 GHz) | Usa las tres simultáneamente |
| **Tecnología clave** | Los dispositivos eligen una banda | MLO (Multi-Link Operation): usa todas a la vez |
| **Ancho de canal** | Hasta 160 MHz | 320 MHz |
| **Compatibilidad** | Estándar universal. Casi cualquier dispositivo lo tiene | Solo dispositivos de alta gama |
| **Costo aprox. (pack x3)** | €160 (WiFi 6) - €320 (WiFi 6E) | €530+ (WiFi 7) |

WiFi 7 es, técnicamente, muy potente. Al usar MLO, si una banda tiene interferencia, el tráfico sigue fluyendo por las otras sin cortes. Además, el canal de 320 MHz es como pasar de una avenida de 2 carriles a una de 4.
Sin embargo, hay una trampa: el ecosistema. Para que WiFi 7 funcione al máximo, tanto el AP como el celular del cliente deben ser WiFi 7. En un coworking, la mayoría de los usuarios vendrán con laptops de hace 2 o 3 años que son WiFi 6. Un router WiFi 7 les dará estabilidad, pero no irán más rápido de lo que su propia placa de red les permita.

La diferencia de precio es notable:
* WiFi 6 (Deco X10): €160. Es una opción económica, pero quizás justa para 200 personas haciendo videollamadas.
* WiFi 6E (Deco XE75): €320. Es el punto medio ideal, ya que abre la banda de 6 GHz (menos congestionada).
* WiFi 7 (Deco BE65): €530. Es una inversión a futuro, pero hoy cuesta casi el triple que el modelo base.

Para el caso concreto TechHub Coworking, lo ideal es el WiFi 6E.
* Madurez: WiFi 6/6E es el estándar que el 90% de nuestros clientes ya tiene.
* Banda de 6 GHz: Al elegir 6E, ya estamos desbloqueando la "vía rápida" de los 6 GHz, evitando las interferencias del 2.4 GHz de los microondas o el Bluetooth de la oficina.
* Eficiencia de costos: Con lo que ahorramos al no ir por WiFi 7 (unos €210), podemos invertir en una mejor conexión de fibra simétrica o en más repetidores para asegurar que no haya "zonas muertas" entre los 3 pisos.

Conclusión: WiFi 7 es el futuro, pero para la realidad actual de TechHub, WiFi 6E ofrece el mejor equilibrio entre capacidad de dispositivos y responsabilidad financiera.

#### VPN modernas
| Aspecto | WireGuard | OpenVPN | IPsec | Tailscale |
| :--- | :--- | :--- | :--- | :--- |
| **Velocidad** | MUY alta | Media | Alta | Alta |
| **Seguridad** | Muy alta | Alta | Alta | Muy alta |
| **Facilidad de config.** | Fácil | Difícil | Compleja | Muy fácil |
| **NAT Traversal** | Sí | Sí | Limitado | Sí |
| **Mejor uso** | VPN moderna | Compatibilidad | Empresas grandes | Zero trust fácil |

**Investiguen qué es Zero Trust Network Access y cómo se diferencia del modelo de VPN  tradicional.**
Las empresas solían usar el modelo de VPN: una vez que la lograbas cruzar, tenías acceso a todo el sistema. Hoy, con el Zero Trust Network Access (ZTNA), la filosofía cambia a "nunca confiar, siempre verificar".

La VPN suele operar en la Capa 3 (Red) del modelo OSI.
* Acceso amplio: Cuando un empleado se conecta por VPN a una sucursal, el sistema confía en él y le da visibilidad de toda la red local. Puede ver servidores, impresoras y otras computadoras.
* El riesgo: Si las credenciales de un usuario son robadas, el atacante tiene "vía libre" para moverse lateralmente por toda nuestra infraestructura.

ZTNA es la tecnología que aplica el concepto de Confianza Cero. A diferencia de la VPN, ZTNA opera normalmente en la Capa 7 (Aplicación).
* Acceso granular: El usuario no se conecta "a la red", sino a una aplicación específica. Si un administrativo necesita entrar al sistema de gestión, solo ve eso; el resto de la red (servidores de cámaras, archivos de gerencia) es invisible para él.
* Verificación constante: No basta con loguearse una vez. El sistema verifica el dispositivo, la ubicación y el comportamiento del usuario en todo momento.

| Característica | VPN Tradicional | ZTNA |
| :--- | :--- | :--- |
| **Punto de acceso** | A la red completa (capa 3). | A aplicaciones específicas (capa 7). |
| **Visibilidad** | El usuario ve todos los recursos IP. | Los recursos están ocultos por defecto. |
| **Confianza** | Se confía tras el primer login. | No se confía en nadie, nunca. |
| **Seguridad** | Riesgo de movimiento bilateral. | Ataques contenidos en una sola app. |

Para el caso TechHub Coworking, donde hay cientos de personas entrando y saliendo, la VPN tradicional puede ser un riesgo si un invitado logra entrar en la red de empleados. Implementar ZTNA sería lo ideal a largo plazo: permitiría que cada miembro del coworking acceda solo a lo que pagó (por ejemplo, la impresora o el servidor de archivos común) sin comprometer la seguridad de toda la infraestructura.

Ya sabemos que el modelo Zero Trust (ZTNA) es superior al de la VPN tradicional por su granularidad. Pero, ¿qué herramienta elegimos?
Las opciones de VPN "Tradicional" (Capa 3):
* IPsec: Es el estándar para conectar las sucursales (Site-to-Site). Es complejo de configurar y su NAT Traversal es limitado, lo que puede dar problemas si los routers de las sucursales tienen firewalls restrictivos.
* OpenVPN: Es muy compatible, pero su velocidad es media. Para 200 personas consumiendo ancho de banda, se nos quedaría corto rápidamente.
* WireGuard: Es la opción ganadora si decidimos quedarnos en el modelo VPN. Es muy rápida y fácil, perfecta para una "VPN moderna" que no retrase la conexión de los usuarios.

El salto a Zero Trust: Tailscale
* Tailscale es la opción con mejor seguridad y la más sencilla de implementar.
* Zero Trust: Tailscale utiliza el protocolo WireGuard por debajo, pero añade una capa de gestión que permite aplicar políticas de "confianza cero" sin ser un experto en redes.
* NAT Traversal: A diferencia de IPsec, Tailscale brilla en conectar dispositivos aunque estén detrás de firewalls complicados (como los de un coworking), asegurando que la conexión nunca se caiga.

Para el caso TechHub Coworking, lo ideal es usar WireGuard o IPsec únicamente para la infraestructura base entre sucursales (donde tenemos control total de los routers) e implementar Tailscale para el acceso de los empleados y recursos críticos.
#### Protocolos IoT

| Protocolo | Capa | Alcance | Velocidad | Consumo | Uso ideal |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Zigbee** | Red y aplicación. | 10-100 m (Mesh) | Baja (250 kbps) | Muy bajo | Bombillas, sensores, cerraduras. |
| **Thread** | Red (IPv6). | 10-100 m (Mesh) | Baja (250 kbps) | Muy bajo. | Base robusta para Matter. |
| **Matter** | Aplicación. | Depende del medio. | Alta (vía WiFi-Ethernet) | Variable | Unificas marcas (Apple, Google). |
| **LoRaWAN** | Red (sobre LoRa físico). | 2-15 km | Muy baja (0.3-50 kbps) | Extremadamente bajo | Sensores en campos o ciudades inteligentes. |
| **MQTT** | Aplicación (mensajería). | Global (vía IP) | Alta (eficiente) | Bajo | Comunicación sensor a nube. |

Zigbee vs. Thread:
Ambos crean redes donde cada dispositivo repite la señal. La gran diferencia es que Thread habla el idioma de internet (IPv6) de forma nativa, mientras que Zigbee es un ecosistema cerrado que necesita un "puente" para salir a la red. Para el TechHub, Thread es más "futurista", pero Zigbee es más barato hoy.

Matter (El traductor universal):
Matter es un idioma. Matter puede viajar sobre Wi-Fi (para mucha velocidad, como una cámara) o sobre Thread (para bajo consumo, como un sensor de puerta). Su objetivo es que no necesites 20 apps en el celular.

LoRaWAN (Larga distancia):
A diferencia de los anteriores, LoRaWAN no es para una oficina, sino para distancias enormes. Usa frecuencias bajas que atraviesan paredes y edificios. Es perfecto si el TechHub quisiera, por ejemplo, monitorear el nivel de agua de un tanque en el techo o sensores de humedad en un jardín a 5 cuadras de distancia.

MQTT (El mensajero liviano):
MQTT no es una conexión inalámbrica, sino un protocolo de publicación/suscripción. Es increíblemente eficiente: en lugar de que el servidor pregunte "¿está prendida la luz?" cada segundo, el dispositivo solo envía un mensaje pequeñito cuando hay un cambio. Es el estándar de oro para enviar datos desde Home Assistant hacia la nube.

Para el caso concreto TechHub Coworking, el plan ideal estaría distribuido:
* Para el interior (Luces/aire): Usaremos Zigbee por costo o Thread si buscamos compatibilidad nativa con Matter.
* Para la gestión de datos: Usaremos MQTT, ya que permite que miles de sensores reporten a nuestro panel de control sin saturar el ancho de banda.
* Para expansión externa: Si el coworking crece a otros edificios cercanos, LoRaWAN sería la única forma económica de conectarlos sin pagar una fibra óptica o un enlace dedicado.

### FASE 4: Implementación
Topologia propuesta: 
<img width="1600" height="949" alt="image" src="https://github.com/user-attachments/assets/bf91802c-d1e6-4d1f-adb5-b0f76886c893" />

Verificación del Direccionamiento (IPs)
Cada VLAN debe tener su propia puerta de enlace (Gateway):
<img width="816" height="370" alt="image" src="https://github.com/user-attachments/assets/3741da51-5d69-48fa-8c09-5b1b02cf0738" />

Verificación de Interfaces y VLANs:
<img width="890" height="374" alt="image" src="https://github.com/user-attachments/assets/95d41d5c-bf79-4b6d-9bb3-db1f6315b0d8" />

Verificación del Servidor DHCP
Este es el motor que le da vida a las PCs:
<img width="866" height="340" alt="image" src="https://github.com/user-attachments/assets/d69e2f07-2e62-4a77-b163-85007a0fb41d" />

Verificación del Servidor DHCP
Este es el motor que le da vida a las PCs:
<img width="866" height="340" alt="image" src="https://github.com/user-attachments/assets/6323b24e-87ae-4b18-b910-6d9a4b417466" />

Verificación del "Puente" al Mundo Real
Para que el internet y las VLANs lleguen al switch, la conexión física debe estar activa. La ether2 (que va al switch) DEBE estar en la lista vinculada al bridge-LAN:
<img width="964" height="124" alt="image" src="https://github.com/user-attachments/assets/b19bf7a0-b35f-4f8d-a9d1-db25d7cb63b7" />




#### CONEXIÓN ZEROTIER
ZeroTier es una solución de red de confianza cero (Zero Trust) que actúa como un switch virtual basado en software, creando una Red de Área Local Virtual (VLAN) de capa 2 sobre la infraestructura de internet existente. Mediante servidores centrales que coordinan los nodos y el uso de cifrado de extremo a extremo, permite que dispositivos remotos se comuniquen de forma segura como si estuvieran conectados a un mismo router físico.
Para el desarrollo de este trabajo práctico, implementamos ZeroTier con el fin de establecer una conexión remota eficiente entre nuestras laptops y una PC de escritorio que actúa como servidor principal. El esquema se estructura de la siguiente manera:
Entorno de Simulación: El servidor de escritorio aloja una máquina virtual donde se ejecuta GNS3 (v3.0), plataforma encargada de procesar las topologías y el tráfico de red del proyecto.
Acceso y Gestión: Mediante un túnel de capa 2 definido por una Network ID y la asignación de IPs virtuales privadas, la interfaz de GNS3 en las laptops se conecta de forma transparente al motor de simulación remoto.
Esta configuración permite centralizar la alta carga de procesamiento en el hardware del servidor, evitando limitaciones de rendimiento en las laptops y garantizando un entorno de colaboración seguro y de baja latencia para la ejecución del TP. 

### FASE 5: Reflexión y defensa
#### Reflexión individual

### Recursos
<https://www.netgear.com/hub/technology/wifi-7-vs-wifi-6/>
<https://network-switch.com/es/blogs/wireless/what-is-wifi-7-and-which-should-you-choose>
<https://olin.es/es/blog/wifi-7-vs-wifi-6/>
<https://surfshark.com/es/blog/site-to-site-vpn>
<https://www.mokosmart.com/es/iot-in-smart-home/>
<https://reolink.com/blog/matter-vs-zigbee/>
<https://www.redeszone.net/tutoriales/redes-cable/vlan-tipos-configuracion/>
<https://www.institucional.frc.utn.edu.ar/sistemas/Areas/noticias/Detalle.asp?1877>
<https://www.mikrotiklabs.com/2019/08/13/uso-de-netwatch-en-mikrotik/>
<https://youtu.be/u1H676WhzQc?si=18u_xJVyj4hdB252>
<https://ausum.cloud/que-es-grafana-y-como-se-usa-en-la-monitorizacion/>
<https://www.tp-link.com/en/wifi7/#4K-QAM>
<https://www.elcorteingles.es/electronica/A51900708-sistema-wi-fi-6-mesh-tp-link-deco-x10-ax1500-ia-seguridad-avanzada-puertos-gigabit-pack-de-3/?stype=text_box&color=Blanco>
<https://www.amazon.es/Nuevo-TP-Link-Deco-XE75-3-Pack/dp/B09ZRY9YHB?maas=maas_adg_A4036A60CCA4BDB4097756EE13ECBAE5_afap_abs&ref_=aa_maas&tag=maas>
<https://www.amazon.es/dp/B0BRRH519K?maas=maas_adg_C745890868FB6D42C2B7AEB11684FB16_afap_abs&ref_=aa_maas&tag=maas&th=1>
<https://youtu.be/CvUBCouzKtU?si=8qR66IdAsss-3Icf>
<https://www.netgear.com/hub/technology/wifi-7-vs-wifi-6/#:~:text=With%20its%20faster%20speeds%2C%20lower%20latency%2C%20and,significant%20improvements%20to%20meet%20our%20growing%20requirements>
<https://www.profesionalreview.com/2024/05/26/wi-fi-7-vs-wi-fi-6e/>
<https://www.rtings.com/router/learn/wifi-6-vs-wifi-7>
<https://www.cloudflare.com/learning/access-management/what-is-ztna/>
<https://www-zerotier-com.translate.goog/blog/zerotier-review-everything-you-need-to-know-about-zerotier-in-2023/?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc> 
<https://www.aqara.com/en/blog/zigbee-vs-thread-vs-matter-whats-the-difference/>
<https://reolink.com/blog/matter-vs-zigbee/>

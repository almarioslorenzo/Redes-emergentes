# TRABAJO PRÁCTICO 1
# REDES EMERGENTES Y CONECTIVIDAD MODERNA

### FASE 1: Problema disparador
------------------------
#### **Preguntas al cliente**
1. **Tenemos zonas con 50 personas en 100m2. ¿WiFi 6 alcanza o necesitamos WiFi 7?**

Para determinar la mejor opción, primero debemos entender que 50 personas en un espacio de 100 m2 clasifica claramente como un entorno de alta densidad.

Si bien Wi-Fi 7 es la novedad, Wi-Fi 6 (802.11ax) fue diseñado precisamente con el objetivo principal de mejorar la eficiencia en escenarios saturados, no solo de aumentar la velocidad punta. Gracias a tecnologías como OFDMA y MU-MIMO bidireccional, Wi-Fi 6 maneja múltiples dispositivos simultáneos de forma excelente. Hoy en día, casi todos los smartphones y laptops modernos son compatibles, lo que garantiza que aprovecharemos la red de inmediato.

Aunque Wi-Fi 7 ofrece mejoras increíbles como canales de 320 MHz y la modulación 4096-QAM, en este escenario específico presenta dos barreras críticas:
* Costo-Beneficio: El presupuesto requerido es considerablemente mayor y, para un grupo de 50 personas, el ancho de banda extra de Wi-Fi 7 podría quedar ocioso.
* Ecosistema: De nada sirve tener un router Wi-Fi 7 si los dispositivos de los usuarios no soportan el estándar; terminarían conectándose usando protocolos anteriores, desperdiciando la inversión.

Para el caso concreto de TechHub Coworking Wi-Fi 6 es más que suficiente. Es una tecnología madura, diseñada para la alta densidad y mucho más accesible económicamente. Wi-Fi 7 es el futuro, pero para cubrir 100 m2 con 50 usuarios, estaríamos haciendo un desperdicio de recursos. Con Wi-Fi 6 cubrimos la necesidad técnica con eficiencia y responsabilidad presupuestaria.

2. **¿Cómo conecto las 3 sucursales de forma segura sin pagar un MPLS?**

Si tenemos 3 sucursales y el presupuesto es una limitante, la respuesta técnica definitiva es implementar una VPN Site-to-Site (Sitio a Sitio). Aunque el MPLS es el “estándar” en rendimiento, para muchas organizaciones el costo simplemente no se justifica.

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

Matter no es exactamente un reemplazo de hardware, sino un estándar que usa el Protocolo de Internet (IP) para que dispositivos de distintas marcas (Apple, Google, Amazon) hablen el mismo idioma.
* Interoperabilidad: Promete que no necesitaremos un "hub" o concentrador diferente para cada marca.
* Seguridad: Al estar basado en IP, incorpora capas de seguridad muy modernas.
* El reto: Al ser un estándar lanzado apenas a finales de 2022, todavía está en una etapa de "pulido". Muchos dispositivos Matter siguen siendo más caros o difíciles de conseguir que los Zigbee.

Para que esta mezcla de protocolos funcione como un solo reloj suizo, necesitamos a Home Assistant como plataforma de gestión local.
* Adiós a las "islas tecnológicas": HA permite que una luz Zigbee de IKEA y un aire acondicionado Matter de otra marca se entiendan en un solo panel de control.
* Privacidad y control local: A diferencia de soluciones comerciales, HA no necesita internet para funcionar. Las automatizaciones (como apagar el aire si no hay nadie en una sala de reuniones) ocurren dentro de nuestra red, protegiendo los datos del coworking.
* Automatización inteligente: Nos permite crear reglas complejas, como: "Si el sensor de presencia no detecta movimiento en 15 minutos Y es después de las 20:00, apaga todo y activa la alarma".

Para el caso concreto de TechHub Coworking, siendo la prioridad la estabilidad y el presupuesto, la combinación ganadora es un servidor de Home Assistant con un adaptador Zigbee profesional. Esto nos da lo mejor de los tres mundos: la economía y estabilidad de Zigbee, la compatibilidad futura de Matter y la inteligencia centralizada de Home Assistant.

4. **¿Cómo separo el tráfico de invitados del de empleados y del IoT?**

Cuando tenemos una sola red para todo, un dispositivo de IoT (como una cámara o un termostato) con poca seguridad podría ser la puerta de entrada para que alguien acceda a las computadoras de los empleados. Para evitar esto, usamos las VLAN (Virtual LAN).

Una VLAN nos permite crear "muros lógicos" dentro de nuestra red. Aunque todos los dispositivos estén conectados al mismo switch físico, el switch los trata como si estuvieran en redes totalmente separadas.

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

| Característica | Netwatch (Mikrotik) | Software de alertas automáticas | Grafana + Prometheus|
| -------| -------- | ---------- | --------- |
| Complejidad | Baja (configuración rápida) | Media | Alta (requiere servidor dedicado) |
| Método | Verificación por ping | Agentes de software | Recolección de métricas |
| Visualización | Consola | Web básica | Dashboards interactivos |
| Notificación | Scripts (Telegram, Email) | Email | Web| Multiplataforma |
| Uso ideal | Reacciones rápidas en el router | Control de inventario y hardware | Monitoreo profesional de toda la IT |

Análisis de las opciones:
* Netwatch: Es ideal con routers MikroTik. Funciona enviando un "ping" constante a la IP del otro extremo de la VPN. Si falla, el router ejecuta un comando automático. Es la opción más liviana porque no requiere servidores externos para detectar el fallo.
* Software de Monitoreo General: Se enfoca en el estado del hardware. Es muy útil para saber si la VPN cayó porque el servidor o el router se apagaron o fueron reemplazados, generando alertas automáticas a los responsables.
* Grafana: Es el estándar en la industria para la visualización de datos. No solo nos avisa si la VPN cayó, sino que nos muestra gráficas de latencia y consumo de ancho de banda. Si la VPN se vuelve lenta antes de caerse, Grafana nos permitirá ver esa tendencia en tiempo real.

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


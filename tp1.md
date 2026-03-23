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

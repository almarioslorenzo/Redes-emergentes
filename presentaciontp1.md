---
marp: true
theme: uncover
paginate: true
header: 'Propuesta Técnica: Infraestructura TechHub Coworking'
footer: 'Cátedra de Redes y Servicios de Telecomunicaciones - TPI 1'
backgroundColor: #ffffff
style: |
  section {
    font-size: 22px;
    font-family: 'Arial', sans-serif;
    text-align: left;
    color: #1a1a1a;
  }
  h1 { color: #002855; text-align: center; font-size: 1.6em; }
  h2 { color: #002855; border-bottom: 1px solid #002855; padding-bottom: 8px; margin-bottom: 20px; }
  h4 { color: #800000; margin-bottom: 5px; text-transform: uppercase; }
  strong { color: #002855; }
  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
    align-items: start;
  }
  .highlight {
    background: #f4f4f4;
    padding: 15px;
    border-left: 4px solid #002855;
  }
---

# Consultoría de Infraestructura Tecnológica
## **TechHub Coworking - Nodo Paraná**
### Proyecto de Reingeniería de Redes de Alta Densidad

---

## 1. Definición del Escenario Crítico
**Contexto del Proyecto:** Renovación integral de la infraestructura de conectividad.

<div class="grid">
<div class="highlight">

**Infraestructura de Planta Física**
* Edificio corporativo de tres niveles.
* Backbone de fibra óptica de 500 Mbps.
* Canalización mediante Categoría 6 (GbE).
* Capacidad para 200 nodos concurrentes.
</div>

<div class="highlight">

**Objetivos de Diseño**
* Interconexión segura de tres sucursales.
* Implementación de estándar Wi-Fi 6.
* Segmentación lógica de tráfico (VLANs).
* Adopción de soluciones Open-Source.
</div>
</div>

---

## 2. Estándar de Acceso Inalámbrico
#### Selección de Protocolo IEEE 802.11ax (Wi-Fi 6)

Se determina el uso de Wi-Fi 6 como solución técnica para gestionar la alta densidad de usuarios en el edificio:
<img width="607" height="293" alt="image" src="https://github.com/user-attachments/assets/3f13d281-8777-4894-9e6d-5e1179f4ca39" /> 

WiFi 7 es el futuro, pero para la realidad actual de TechHub, WiFi 6E ofrece el mejor equilibrio entre capacidad de dispositivos y responsabilidad financiera.

---

## 3. Segmentación Jerárquica de Red (VLANs)
#### Optimización del Tráfico y Seguridad de Capa 2

Sobre la infraestructura física, se establece un esquema de segmentación para mitigar dominios de broadcast y garantizar la seguridad perimetral:

* **VLAN 10 - Empleados:** Es la red con mayor prioridad. Tiene acceso a los servidores internos, bases de datos e impresoras.
* **VLAN 20 -Invitados:** Solo tiene salida a Internet. Nunca puedan ver los equipos de la oficina.
* **VLAN 30 - IoT:** IoT: Reservada para sensores, aires acondicionados y cámaras.
<img width="890" height="374" alt="image" src="https://github.com/user-attachments/assets/3e2956b7-9198-48b3-bf07-a88348ad428f" />
Internet
   │
Router MikroTik
   │
bridge-LAN
   ├── VLAN 11 (empleados p1)
   ├── VLAN 21 (invitados p1)
   ├── VLAN 31 (IoT p1)
   │
   ├── VLAN 12 (empleados p2)
   ├── VLAN 22 (invitados p2)
   ├── VLAN 32 (IoT p2)
---

## 4. Arquitectura de Seguridad y Enlace
#### Implementación de Soluciones Basadas en Software Libre

<div class="grid">
<div class="highlight">

**Gateway y Firewall Perimetral**
* Implementación de **pfSense** como sistema de gestión de amenazas y ruteo centralizado.
</div>

<div class="highlight">

**Interconexión Site-to-Site (VPN)**
* Configuración de túneles mediante **WireGuard**, garantizando cifrado de grado empresarial sobre internet pública.
</div>
</div>

> El Nodo Paraná actúa como punto de concentración de servicios para la interconexión con las sucursales remotas, eliminando costos fijos de líneas dedicadas.

---

## 5. Automatización e Infraestructura IoT
#### Gestión Local e Interoperable

Para la gestión de clima e iluminación, se propone un ecosistema independiente de la red de datos principal:

* **Protocolos:** Uso de **Zigbee** para crear una red en malla (Mesh) de bajo consumo que no sature el espectro Wi-Fi.
* **Plataforma de Gestión:** **Home Assistant** bajo entorno local, garantizando privacidad y control total sin dependencia de nubes externas.
* **Eficiencia:** Automatización basada en reglas de presencia para la optimización del consumo eléctrico.

---

## 6. Monitoreo y Disponibilidad
#### Gestión Proactiva del Servicio

1. **Detección de Fallas (Netwatch):**
   * Scripts automatizados para monitoreo de estados mediante ICMP.
   * Notificaciones inmediatas vía API (Telegram/Email) ante caídas de enlace.

2. **Visualización de Métricas (Grafana + Prometheus):**
   * Dashboards para análisis de latencia, tráfico y consumo de recursos en tiempo real.
   * Identificación predictiva de cuellos de botella en la infraestructura.

---


## 7. Metodología de Validación
#### Entorno de Simulación y Laboratorio Virtual

Para la validación de la propuesta técnica, se desplegó un entorno controlado que permite replicar el comportamiento real de la red:

* **Virtualización:** Uso de **GNS3 3.0** sobre máquinas virtuales (**VMware/VirtualBox**) para aislar el cómputo de la red y emular el hardware de red de forma precisa.
* **Conectividad Remota:** Implementación de **ZeroTier** como SDN (Software Defined Network) para establecer una capa de red virtual de Capa 2. Esto permitió:
    * El trabajo colaborativo entre integrantes en diferentes ubicaciones.
    * La interconexión de nodos de simulación con dispositivos físicos externos.
* **Control de Versiones:** Documentación de configuraciones y topologías en **GitHub**, garantizando la trazabilidad del diseño.
<img width="903" height="253" alt="image" src="https://github.com/user-attachments/assets/22699d68-510e-4cfb-8ac2-2a1ba3f9676a" />
<img width="1566" height="249" alt="image" src="https://github.com/user-attachments/assets/2ef0afb0-d619-4e6b-8074-6c7a78341ab8" />


---

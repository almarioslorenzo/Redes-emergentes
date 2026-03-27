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

* **VLAN 10 - Gestión (Management):** Acceso restringido a switches, APs y gateways.
* **VLAN 20 - Producción/Empleados:** Acceso a servidores internos y recursos corporativos con prioridad QoS.
* **VLAN 30 - Automatización/IoT:** Red restringida y unidireccional para sensores y sistemas de control.
* **VLAN 99 - Invitados:** Acceso exclusivo a Internet con aislamiento de cliente (*Client Isolation*).

---

## 4. Arquitectura de Seguridad y Enlace
#### Implementación de Soluciones Basadas en Software Libre

<div class="grid">
<div class="highlight">

**Gateway y Firewall Perimetral**
* Implementación de **pfSense** como sistema de gestión de amenazas y ruteo centralizado.
</div>

<div class="highlight">

**Interconexión Multi-Sitio (VPN)**
* Configuración de túneles mediante **WireGuard** o **IPsec**, garantizando cifrado de grado empresarial sobre internet pública.
</div>
</div>

> El Nodo Paraná actúa como punto de concentración de servicios para la interconexión con las sucursales remotas, eliminando costos fijos de líneas dedicadas.

---

## 5. Automatización e Infraestructura IoT
#### Gestión Local e Interoperable

Para la gestión de clima e iluminación, se propone un ecosistema independiente de la red de datos principal:

* **Protocolos:** Uso de **Zigbee/Matter** para crear una red en malla (Mesh) de bajo consumo que no sature el espectro Wi-Fi.
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
#### Simulación y Control de Versiones

* **Entorno de Simulación:** Modelado de la topología lógica en **GNS3 3.0**.
* **Servicios de Red:** Configuración de servicios críticos (DNS, DHCP, NTP) bajo entornos Linux.
* **Documentación:** Repositorio técnico y control de versiones en **GitHub**.

---

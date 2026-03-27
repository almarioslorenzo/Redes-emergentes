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

**Requerimientos de Planta Física**
* Edificio corporativo de tres niveles.
* Backbone de fibra óptica de 500 Mbps.
* Canalización mediante Categoría 6 (GbE).
* Capacidad para 200 nodos concurrentes.
</div>

<div class="highlight">

**Objetivos de Diseño**
* Interconexión segura de tres sucursales.
* Segmentación lógica de tráfico (VLANs).
* Integración de domótica (Control de clima).
* Adopción de soluciones Open-Source.
</div>
</div>

---

## 2. Segmentación y Jerarquía de Red (VLANs)
#### Optimización del Tráfico y Seguridad de Capa 2

Se establece un esquema de segmentación para mitigar dominios de broadcast y garantizar la seguridad perimetral entre perfiles de usuario:

* **VLAN 10 - Empleados:** Tiene acceso a los servidores internos, bases de datos e impresoras.
* **VLAN 20 - Invitados:** Solo tiene salida a Internet.  Nunca puedan ver los equipos de la oficina.
* **VLAN 30 - IoT:**  Reservada para sensores, aires acondicionados y cámaras.

---

## 3. Estándar de Acceso Inalámbrico
#### Implementación de Protocolo IEEE 802.11ax (Wi-Fi 6)

La selección tecnológica responde a la necesidad de gestionar alta densidad de usuarios en espectros saturados:

* **OFDMA:** Subdivisión de canales para atención simultánea de múltiples estaciones, reduciendo la latencia de red.
* **MU-MIMO 4x4:** Transmisión espacial múltiple para dispositivos de alta demanda de ancho de banda.
* **BSS Coloring:** Mitigación de interferencia de co-canal en el despliegue vertical del edificio.
* **Eficiencia Energética:** Uso de Target Wake Time (TWT) para dispositivos finales.

---

## 4. Arquitectura de Seguridad y Enlace
#### Implementación de Soluciones Basadas en Software Libre

La infraestructura se sustenta en estándares abiertos para garantizar escalabilidad y auditoría de seguridad:

<div class="grid">
<div class="highlight">

**Gateway y Firewall Perimetral**
* Implementación de **pfSense** como sistema de gestión de amenazas y ruteo centralizado.
</div>

<div class="highlight">

**Interconexión Multi-Sitio**
* Configuración de túneles VPN mediante **WireGuard**, priorizando el rendimiento y cifrado moderno.
</div>
</div>

> El Nodo Paraná actúa como punto de concentración de servicios para la interconexión con las sucursales remotas.

---

## 5. Metodología de Validación
#### Simulación y Despliegue Técnico

1. **Entorno de Simulación:** Modelado de la topología lógica en **GNS3 3.0**.
2. **Servicios de Red:** Configuración de servicios críticos (DNS, DHCP, NTP) bajo entornos Linux/Open-Source.
3. **Control de Versiones:** Documentación técnica y repositorio de configuraciones en **GitHub**.

---
## 🔒 Interconexión de Sedes
#### VPN Site-to-Site vs. MPLS

**Propuesta: Túneles Cifrados IPsec**

* **Seguridad:** Cifrado, integridad y autenticación de grado empresarial sobre internet pública.
* **Ahorro Operativo:** Eliminación de costos fijos de líneas dedicadas (MPLS).
* **Flexibilidad:** Escalabilidad inmediata y gestión autónoma mediante software.

---

## 🏠 Automatización e IoT
#### Ecosistema Local e Interoperable

<div class="grid">
<div class="highlight">

**Protocolos: Zigbee + Matter**
* Red en malla (Mesh) de bajo consumo.
* No satura el Wi-Fi principal.
* Garantía de futuro (Matter).
</div>

<div>

**Gestión: Home Assistant**
* **Privacidad:** Procesamiento 100% local.
* **Control Total:** Unificación de marcas y ahorro energético mediante reglas de presencia.
</div>
</div>

---

## 🧱 Segmentación de Red
#### Arquitectura de VLANs

* **VLAN Empleados:** Acceso corporativo total y prioridad de tráfico.
* **VLAN Invitados:** Acceso exclusivo a internet con aislamiento (*Client Isolation*).
* **VLAN IoT:** Red restringida y unidireccional para sensores/cámaras.

> **Resultado:** Reducción de broadcast, mejora de rendimiento y contención de amenazas ante dispositivos vulnerables.

---

## 🛠️ Monitoreo y Disponibilidad
#### ¿Cómo nos enteramos si algo falla?

1. **Reacción Inmediata (Netwatch - MikroTik):**
   * Scripts automáticos por caída de *ping*.
   * Alerta vía **Telegram** en < 10 segundos.

2. **Visibilidad Avanzada (Grafana + Prometheus):**
   * Dashboards para análisis de latencia y consumo.
   * Identificación proactiva de cuellos de botella.

---




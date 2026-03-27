---
marp: true
theme: uncover
paginate: true
header: 'Propuesta Técnica: TechHub Coworking'
footer: 'Servicios de Telecomunicaciones y Redes - TPI 1'
backgroundColor: #f8f9fa
style: |
  section {
    font-size: 24px;
    font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    text-align: left;
    color: #333;
  }
  h1 { color: #003366; text-align: center; }
  h2 { color: #00509d; border-bottom: 2px solid #00509d; padding-bottom: 10px; }
  h4 { color: #d62828; margin-bottom: 10px; text-transform: uppercase; letter-spacing: 1px; }
  strong { color: #00509d; }
  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 25px;
    align-items: start;
  }
  .highlight {
    background: #e9ecef;
    padding: 15px;
    border-left: 5px solid #00509d;
    border-radius: 4px;
  }
  blockquote {
    font-style: italic;
    color: #555;
    background: #f1f1f1;
    border-left: 10px solid #ccc;
    padding: 0.5em 10px;
  }
---

# Consultoría Tecnológica
## **TechHub Coworking**

---

##  Problema disparador 
**Proyecto:** Renovación de infraestructura en Paraná.

<div class="grid">
<div class="highlight">

**Infraestructura Base**
* Edificio 3 pisos.
* Fibra 500 Mbps / Cat6.
* 200 dispositivos simultáneos.
</div>

<div>

**Requerimientos Críticos**
* Interconexión de 3 sucursales.
* Segmentación estricta de tráfico.
* Automatización (Luces/Clima).
* **Premisa:** Soluciones *Open-Source*.
</div>
</div>

---

## 📶 Conectividad Inalámbrica
#### ¿Wi-Fi 6 o Wi-Fi 7?

**Decisión Técnica: Wi-Fi 6 (802.11ax)**

* **Densidad Inteligente:** OFDMA y MU-MIMO para manejar 50+ usuarios por zona sin degradación.
* **Compatibilidad Real:** Aprovechamiento inmediato por dispositivos actuales de los coworkers.
* **Eficiencia Presupuestaria:** Máximo rendimiento sin el sobrecosto innecesario de Wi-Fi 7 para una WAN de 500 Mbps.

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




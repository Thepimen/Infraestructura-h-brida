# 🛡️ Arquitectura de Infraestructura Híbrida: SOC con Wazuh

> **Proyecto de Ciberseguridad:** Implementación de un ecosistema de detección y respuesta (XDR) simulando un entorno corporativo real mediante virtualización, contenerización y redes híbridas .

---

## 📋 Tabla de Contenidos
1. [Descripción del Proyecto](#-descripción-del-proyecto)
2. [Arquitectura de la Infraestructura](#-arquitectura-de-la-infraestructura)
3. [Tecnologías y Herramientas](#-tecnologías-y-herramientas)
4. [Configuración de Red](#-configuración-de-red)
5. [Justificación Técnica](#-justificación-técnica)
6. [Equipo](#-equipo)

---

## 🚀 Descripción del Proyecto

Este proyecto despliega un **Security Operations Center (SOC)** funcional utilizando **Wazuh** . El objetivo es monitorizar, detectar y responder a amenazas en un entorno híbrido que combina máquinas virtuales y contenedores Docker .

El sistema simula un escenario real donde conviven :
* **Servidores Críticos:** Alojados en contenedores .
* **Endpoints Corporativos:** Estaciones de trabajo Windows .
* **Auditoría Ofensiva:** Equipos atacantes externos (Red Team) .

---

## 🏗️ Arquitectura de la Infraestructura

La infraestructura se divide en tres capas lógicas:

### 1. Infraestructura Core (Docker) 🐳
El núcleo del sistema gestionado con **Docker Compose** para asegurar que los servicios arranquen con las dependencias correctas y eficiencia de recursos .
* **Wazuh Stack:** Contenedores para el *Manager*, *Indexer* y *Dashboard* .
* **DVWA (Damn Vulnerable Web Application):** Servidor web vulnerable (PHP/MySQL) que actúa como *Honeypot* dentro de la red interna .

### 2. Capa de Auditoría Ofensiva (VMs) ⚔️
Máquinas virtuales que simulan ataques externos :
* **BlackArch Linux:** Repositorio de seguridad *Rolling Release* con más de 2,800 herramientas de pentesting instaladas modularmente .
* **NixOS:** Utilizado por su configuración determinista y declarativa, garantizando entornos de prueba reproducibles y aislados .

### 3. Endpoints Vulnerables 💻
* **Windows 11:** Simula la estación de trabajo de un usuario estándar . Incluye el agente de Wazuh y **Sysmon** para monitorizar procesos, red y cambios en el sistema de archivos .

---

## 🌐 Configuración de Red

El proyecto utiliza un esquema de red de dos niveles para gestionar el tráfico :

| Tipo de Red | Nombre | Descripción |
| :--- | :--- | :--- |
| **Interna (Docker)** | `security-net` | Red tipo *Bridge*. Comunicación aislada entre el Manager, Indexer y DVWA. Usa tráfico mTLS para seguridad . |
| **Híbrida (LAN)** | `Host / LAN` | Permite la conexión de las VMs (atacantes y víctimas) con el Host. El Manager expone los puertos **1514** (Logs) y **1515** (Auth) . |

**Flujo de Datos:**
1. Agente detecta evento ➔ 2. Envío cifrado a puerto 1514 (Host) ➔ 3. Redirección al contenedor Wazuh ➔ 4. Visualización en Dashboard .

---

## 🛠️ Tecnologías y Herramientas

### Stack Defensivo (Blue Team)
* **Wazuh (XDR/SIEM):** Elegido por su capacidad de visión 360° (Red + Host) .
* **Sysmon:** Telemetría avanzada en Windows .
* **Docker:** Orquestación de servicios y persistencia de datos .

### Stack Ofensivo (Red Team)
* **BlackArch:** Instalación personalizada (sin bloatware) usando *Window Managers* ligeros .
* **Metasploit & Scripts:** Para inyección SQL, XSS y fuerza bruta contra DVWA .

---

## ⚖️ Justificación Técnica

¿Por qué elegimos **Wazuh** frente a otras alternativas? 

| Alternativa | Comparativa con Wazuh |
| :--- | :--- |
| **Suricata (IDS)** | Suricata es excelente para red, pero es "ciego" a lo que ocurre dentro del SO. Wazuh ofrece **XDR** (Red + Archivos + Procesos) . |
| **ELK Stack** | ELK puro requiere crear reglas desde cero. Wazuh incluye miles de reglas de seguridad preconfiguradas y decodificadores listos para usar . |
| **Splunk** | Splunk tiene costes de licencia prohibitivos. Wazuh es **Open Source** y permite un despliegue profesional sin coste . |

---

## 👥 Equipo

* **Mateo García** 
* **Luis Lázaro** 
* **Noelia Cantador** 
* **Fabio Rieker**

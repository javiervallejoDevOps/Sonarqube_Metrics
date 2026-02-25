# SonarQube Metrics Stack

Este repositorio contiene una **pila completa de observabilidad** orientada a la **explotación de métricas de SonarQube**, almacenadas de forma escalable y visualizadas mediante Grafana.

El objetivo principal es demostrar una arquitectura realista de **recolección, almacenamiento y visualización de métricas** que podría encontrarse en un entorno DevOps moderno.

---

## 🧱 Arquitectura general

El flujo de datos es el siguiente:

```
SonarQube → Grafana Agent → Mimir → Grafana
```

1. **SonarQube** expone métricas de calidad de código.
2. **Grafana Agent** recolecta estas métricas mediante scraping.
3. **Mimir** actúa como backend de series temporales (compatible con Prometheus).
4. **Grafana** consulta Mimir y presenta los datos en dashboards.

Esta arquitectura desacopla completamente la **ingesta**, el **almacenamiento** y la **visualización**, permitiendo escalar cada componente de forma independiente.

---

## 📦 Componentes incluidos

### SonarQube

* Instancia local para análisis de calidad de código.
* Expone métricas en formato Prometheus.
* Punto de origen de los datos observados.

### PostgreSQL

* Base de datos utilizada por SonarQube.
* Necesaria para persistencia de proyectos, análisis y configuraciones.

### Grafana Agent

* Encargado del scraping de métricas.
* Envia los datos a Mimir usando remote_write.
* Configuración ligera y pensada para producción.

### Mimir

* Backend de métricas altamente escalable.
* Compatible con PromQL.
* Permite retención y consulta eficiente de series temporales.

### Grafana

* Herramienta de visualización.
* Conectada a Mimir como datasource.
* Permite crear dashboards de métricas de calidad de código.

---

## 📁 Estructura del repositorio

```
.
├── README.md
├── postgres.yml
├── sonarqube-values.yml
├── grafana-agent.yml
├── mimir-values.yml
└── grafana-values.yml
```

### Descripción de archivos

* **postgres.yml**

  * Despliegue de PostgreSQL para SonarQube.

* **sonarqube-values.yml**

  * Valores de configuración de SonarQube.
  * Incluye parámetros de base de datos y exposición de métricas.

* **grafana-agent.yml**

  * Configuración del scraping de métricas.
  * Define targets y remote_write hacia Mimir.

* **mimir-values.yml**

  * Configuración del backend de métricas.
  * Ajustes de almacenamiento y retención.

* **grafana-values.yml**

  * Configuración de Grafana.
  * Datasource apuntando a Mimir.

---

## 🚀 Orden de despliegue

Es importante respetar el orden para evitar errores de dependencia:

1. **PostgreSQL**
2. **SonarQube**
3. **Mimir**
4. **Grafana Agent**
5. **Grafana**

Este orden garantiza que cada servicio tenga disponibles sus dependencias en el momento del arranque.

---

## 📊 Métricas disponibles

Algunas de las métricas que pueden visualizarse:

* Calidad del código
* Code smells
* Bugs y vulnerabilidades
* Cobertura de tests
* Deuda técnica

Todas estas métricas son consultables mediante **PromQL** desde Grafana.

---

## 🎯 Casos de uso

* Monitorización continua de la calidad del código
* Detección temprana de degradaciones técnicas
* Ejemplo práctico de arquitectura de observabilidad
* Proyecto demostrativo para perfiles DevOps / SRE

---

## 🛠 Requisitos

* Docker / Kubernetes (según despliegue)
* Conocimientos básicos de Prometheus / Grafana
* Entorno local o cluster de pruebas

---

## 📌 Notas finales

Este repositorio está pensado como **proyecto demostrativo**, pero la arquitectura es totalmente extrapolable a entornos productivos.

Cualquier mejora o extensión (alerting, long-term storage, multi-tenant, etc.) puede añadirse fácilmente sobre esta base.

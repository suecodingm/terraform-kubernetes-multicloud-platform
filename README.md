# Multi-Cloud Terraform & Kubernetes Platform 🚀

Este proyecto despliega una infraestructura multi-cloud altamente disponible y automatizada utilizando **Infrastructure as Code (IaC)**, **GitOps**, y prácticas avanzadas de **CI/CD**. 
Sirve como mi proyecto de fin de Máster en DevOps, simulando un entorno empresarial real de producción.

## 🏗️ Arquitectura del Sistema
El proyecto se compone de una arquitectura desacoplada con un `frontend` y un `backend` contenerizados, desplegados sobre clústeres gestionados de Kubernetes.

*   **IaC:** Módulos reutilizables de Terraform para aprovisionar redes y clústeres de Kubernetes.
*   **Orquestación:** Kubernetes (K8s) gestionando el ciclo de vida de los contenedores, auto-escalado y balanceo de carga.
*   **CI/CD:** Pipelines automatizados que ejecutan pruebas unitarias, análisis de código, construyen imágenes de Docker y actualizan los manifiestos.

---

## 🛠️ Tecnologías Utilizadas

*   **Proveedores Cloud:** AWS & Google Cloud Platform (GCP).
*   **Infraestructura como Código:** Terraform / OpenTofu.
*   **Contenedores y Orquestación:** Docker, k3s (Kubernetes).
*   **Base de Datos:** PostgreSQL.
*   **Lenguaje y Runtime:** Node.js.
*   **CI/CD:** GitHub Actions.
*   **Pruebas de Carga:** K6.
*   **Monitoreo:** Grafana & Prometheus.

---

## 📂 Estructura del Repositorio

*   `/.github/workflows/`: Pipelines automatizados de CI/CD.
*   `/terraform/`: Código de infraestructura estructurado para múltiples proveedores cloud y operaciones.
*   `-/aws/`: Recursos de red e infraestructura en Amazon.
*   `-/gcp/` : Recursos de red e infraestructura en Google Cloud.
*   `-/monitoring/` : Código de Terraform encargado de desplegar Prometheus y Grafana una vez el clúster está listo.
*   `/kubernetes/`: Manifiestos YAML (Deployments, Services, Ingress, HPA).
*   `/backend/` & `/frontend/`: Código fuente de las aplicaciones y sus respectivos `Dockerfiles`.
*   `/tests/`: Scripts automatizados para validar la funcionalidad del sitio e inyectar ráfagas de carga simulando tráfico real.

---

## 🚀 Configuración y Despliegue

### Requisitos Previos
*   Terraform >= 1.x
*   Kubectl & k3s configurado local o remotamente.
*   AWS CLI y gcloud CLI configurados con credenciales válidas.

### 1. Aprovisionamiento de Infraestructura
```bash
cd terraform
# Configurar y aplicar recursos en AWS
cd aws && terraform init && terraform apply
# Configurar y aplicar recursos en GCP
cd ../gcp && terraform init && terraform apply
```

### 2. Despliegue Automatizado de la Aplicación (CI/CD) mediante Pipeline (GitHub Actions)
No es necesario aplicar los manifiestos de Kubernetes manualmente. La plataforma implementa un flujo de **Continuous Deployment (CD)** completo:
Cada `push`  hacia la rama principal dispara el flujo automatizado que gestiona todo el ciclo de vida:

1. El pipeline de GitHub Actions utiliza las credenciales seguras almacenadas en los **GitHub Secrets** para autenticarse con el proveedor cloud.
2. Al realizar un `push` a la rama principal, el workflow compila las imágenes de Docker para el `frontend`, `backend` y `database`.
3. Las imágenes se suben al registro de contenedores y el pipeline actualiza automáticamente el clúster de **k3s**, desplegando la aplicación de extremo a extremo de forma transparente.


### 3. Aprovisionamiento de la Plataforma de Observabilidad (Día 2)
Una vez que el clúster de Kubernetes está desplegado exitosamente y la aplicación se encuentra en ejecución, las herramientas de monitoreo se aprovisionan de forma semi-automatizada utilizando un módulo dedicado de Terraform.

Este paso se ejecuta de manera independiente para respetar el ciclo de dependencias lógicas (asegurando la existencia y disponibilidad del clúster antes de inyectar las herramientas de telemetría):

```bash
cd terraform/monitoring
terraform init
terraform apply
```

*Este módulo se encarga de instalar y configurar **Prometheus** (para la recolección de métricas del clúster y de la base de datos PostgreSQL) y **Grafana** (para la capa de visualización de datos), conectándolos automáticamente.*


## 📈 Próximas Mejoras (Roadmap)
*   [ ] Implementar GitOps utilizando ArgoCD.
*   [ ] Configurar Service Mesh con Istio para control de tráfico avanzado.

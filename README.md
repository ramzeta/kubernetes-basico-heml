# Curso de Kubernetes Básico + Helm para Desarrolladores

## 🔍 Objetivo

Aprender a desplegar aplicaciones en Kubernetes de forma profesional, utilizando Helm como herramienta para simplificar la gestión de recursos.

---

## ✅ Nivel: Básico-Intermedio

Este curso está diseñado para desarrolladores como tú, que ya han trabajado con YAML y despliegues básicos, y quieren profesionalizar su flujo con Helm.

---

## 📊 Temario

### 1. Fundamentos de Recursos Kubernetes

**Recursos básicos:**

* **Pod:** Unidad mínima de ejecución.
* **Deployment:** Define replicas, actualizaciones, y rollout.
* **Service:** Expone Pods dentro o fuera del cluster.
* **Secret:** Almacena datos sensibles como contraseñas.
* **PVC (PersistentVolumeClaim):** Garantiza almacenamiento persistente.

**Ejemplo de Deployment manual (sin Helm):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: petclinic
spec:
  replicas: 1
  selector:
    matchLabels:
      app: petclinic
  template:
    metadata:
      labels:
        app: petclinic
    spec:
      containers:
        - name: petclinic
          image: springcommunity/spring-petclinic-rest
          ports:
            - containerPort: 8080
```

---

### 2. Diagnóstico y Troubleshooting

**Errores comunes:**

* `CrashLoopBackOff`: Problemas al iniciar contenedor.
* `Pending`: Kubernetes no encuentra nodo disponible.

**Comandos últiles:**

```bash
kubectl get pods
kubectl describe pod <nombre>
kubectl logs <nombre>
kubectl exec -it <nombre> -- bash
```

---

### 3. Persistencia de datos con PVCs

**YAML de PVC ejemplo:**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

Con esto, garantizas que tu base de datos no pierda datos al reiniciar.

---

### 4. Introducción a Helm — El Gestor de Paquetes de Kubernetes

**¿Qué es Helm?**
Helm es como `apt` o `npm`, pero para Kubernetes. Te permite empaquetar, reutilizar, y versionar tus despliegues.

**Beneficios frente a YAML nativo:**

* Plantillas reutilizables (con variables).
* Fácil de actualizar (“upgrade”) y revertir cambios.
* Organiza todo en un chart.
* Mejor integración con CI/CD.

**Estructura de un Chart de Helm:**

```
petclinic/
  Chart.yaml
  values.yaml
  templates/
    deployment.yaml
    service.yaml
    pvc.yaml
```

**Instalación:**

```bash
helm install petclinic ./petclinic
```

**Desinstalar:**

```bash
helm uninstall petclinic
```

**Variables en `values.yaml`:**

```yaml
image:
  repository: springcommunity/spring-petclinic-rest
  tag: latest
  port: 8080

resources:
  limits:
    cpu: 500m
    memory: 512Mi
```

**Y plantilla con variables (`templates/deployment.yaml`):**

```yaml
spec:
  containers:
    - name: petclinic
      image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
      ports:
        - containerPort: {{ .Values.image.port }}
      resources:
        {{- toYaml .Values.resources | nindent 8 }}
```

---

## 📈 Buenas prácticas

* Usa `livenessProbe` y `readinessProbe` para saber si tu app está viva.
* Usa `requests` y `limits` para control de recursos.
* Versiona tus charts.
* Usa `helm upgrade` y `helm rollback` en lugar de `kubectl apply`.

---

## 📄 Recursos adicionales

* [Documentación oficial de Helm](https://helm.sh/docs/)
* [Kubernetes básico - Tutorial oficial](https://kubernetes.io/docs/tutorials/)
* [ArtifactHub (Helm Charts)](https://artifacthub.io/)

---

## 📖 Tarea Final

1. Crear un Chart para tu app y base de datos.
2. Deployarlo en Minikube o Kubernetes local.
3. Asegurar que los datos persisten con PVC.
4. Simular fallos y analizar los logs.
5. Revertir a una versión anterior usando `helm rollback`.

---

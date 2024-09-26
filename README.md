# Despliegue de WordPress y MySQL en Kubernetes

Este repositorio contiene la configuración necesaria para desplegar WordPress y MySQL en un clúster de Kubernetes utilizando archivos YAML.

## Requisitos Previos

Antes de comenzar, asegúrate de tener instalado y configurado lo siguiente:

- [Docker](https://docs.docker.com/get-docker/)
- [Kubernetes](https://kubernetes.io/docs/setup/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Git](https://git-scm.com/)

## Despliegue

### Paso 1: Clonar el Repositorio

Clona este repositorio en tu máquina local:

```bash
git clone https://github.com/dcjreggae/wordpress-k8s-config.git
cd wordpress-k8s-config
```

### Paso 2: Configurar Persistent Volumes

Asegúrate de que los `PersistentVolumes` (PV) y `PersistentVolumeClaims` (PVC) estén correctamente configurados en tu entorno NFS. Revisa y ajusta los archivos `pv-mysql.yaml` y `pv-wordpress.yaml` según sea necesario.

### Paso 3: Desplegar MySQL

Aplica la configuración de MySQL:

```bash
kubectl apply -f pv-mysql.yaml
kubectl apply -f pvc-mysql.yaml
kubectl apply -f mysql-deployment.yaml
```

### Paso 4: Desplegar WordPress

Aplica la configuración de WordPress:

```bash
kubectl apply -f pvc-wordpress.yaml
kubectl apply -f wordpress-deployment.yaml
```

### Paso 5: Verifica el Despliegue

Verifica que todos los pods se estén ejecutando correctamente:

```bash
kubectl get pods -n wordpress
```

### Acceso a WordPress

Una vez que los pods estén en estado `Running`, puedes acceder a tu instancia de WordPress mediante la dirección IP del servicio. Para obtener la dirección IP, utiliza:

```bash
kubectl get svc -n wordpress
```

### Construcción y Subida de Imágenes

Si has realizado cambios en las imágenes de Docker, asegúrate de construirlas y subirlas a DockerHub:

```bash
# Construir imágenes
docker build -t dcjreggae/mysql:5.6 .
docker build -t dcjreggae/wordpress:4.8-apache .

# Subir imágenes a DockerHub
docker push dcjreggae/mysql:5.6
docker push dcjreggae/wordpress:4.8-apache
```

## Contribuciones

Si deseas contribuir a este proyecto, no dudes en abrir un *issue* o enviar un *pull request*.

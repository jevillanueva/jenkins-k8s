# Jenkins en Kubernetes

Este proyecto contiene los archivos de configuración básicos para levantar una instancia de jenkins en Kubernetes

Existen un  orden para crear los recursos

## 1. Crear el "namespace"

Para crear el namespace que almacenara los recursos de nuestro Jenkins

```bash
kubectl create -f namespace.yaml
```

## 2. Crear el volumen

Crear el volumen y su claim para la persistencia de la información del servicio para esta demo se utiliza un volumen del 10Gi

```bash
kubectl create -f volume.yaml
kubectl create -f volume_claim.yaml
```

## 3. Crear el deployment

Creamos el deploy que contiene la imagen de Jenkins en  su version LTS y el acceso persistente a los volúmenes

```bash
kubectl apply -f deploy.yaml
```

## 4. 	Exponer el acceso http

Crear los servicios para exponer tanto interna como externamente los servicios de Jenkins:

Utiliza el puerto 30000 para acceder al servicio de manera externa mediante web

Utiliza el puerto 50000 para el acceso interno del cluster y hacer uso del JNLP

```bash
kubectl create -f service.yaml
```

# Utilizando Docker Compose para levantar Jenkins

Otra forma de instanciar rapidamente Jenkins seria utilizando Docker Compose

```bash
docker-compose up -d
```

Utilizando traefik para la asignacion de dominio es necesario utilizar el archivo docker-compose.traefik.yaml y copiar las variables de entorno a la carpeta

traefik.env a .env y llenar las variables de Traefik

```bash
cp traefik.env .env
#Edit .env
docker-compose -f docker-compose.traefik.yaml up -d 
```

Posterior a levantar es necesario instalar los binarios de docker para poder usar el sock desde el contenedor

```bash
docker exec -it <contaienr id> bash
curl https://get.docker.com > dockerinstall && chmod 777 dockerinstall && ./dockerinstall
sudo chmod 666 /var/run/docker.sock
```

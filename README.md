<!-- markdownlint-disable-next-line -->
# OpenTelemetry Demo

## Descripción

Esta es una Demo para mostrar la instrumentación de trazas de librerías en Java no soportadas en Dynatrace (Redisson).

Referencia - [Dynatrace](https://www.dynatrace.com/news/blog/opentelemetry-demo-application-with-dynatrace/)

## Arquitectura
El lab está basado en la Demo de OpenTelemetry, modificada para agregar un servicio de Redis que se comunica con el adService (JAVA):

![Captura de pantalla 2023-12-29 144351](https://github.com/EricYuY/otel-demo-redis/assets/73684844/4329ceb3-c94c-4cc8-8edd-0c5cc6ce2da3)


## Prequesitos:
- Docker (https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)
- Tenant Dynatrace
  - Features (https://docs.dynatrace.com/docs/shortlink/otel-wt-java-auto)

Pasos para replicar:

## 1. Clonar repositorio
```
git clone https://github.com/EricYuY/otel-demo-redis.git
```
## 2. Instalar agente DT, para Docker: 
```
docker run -d \
--restart=on-failure:5 \
--pid=host \
--net=host \
--cap-drop ALL \
--cap-add CHOWN \
--cap-add DAC_OVERRIDE \
--cap-add DAC_READ_SEARCH \
--cap-add FOWNER \
--cap-add FSETID \
--cap-add KILL \
--cap-add NET_ADMIN \
--cap-add NET_RAW \
--cap-add SETFCAP \
--cap-add SETGID \
--cap-add SETUID \
--cap-add SYS_ADMIN \
--cap-add SYS_CHROOT \
--cap-add SYS_PTRACE \
--cap-add SYS_RESOURCE \
--security-opt apparmor:unconfined \
-v /:/mnt/root \
-e ONEAGENT_INSTALLER_SCRIPT_URL=https://www.{id}.live.dynatrace.com/api/v1/deployment/installer/agent/unix/default/latest?arch=x86 \
-e ONEAGENT_INSTALLER_DOWNLOAD_TOKEN=dt0c01.XXXXXXXXXXXX.XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX \
dynatrace/oneagent --set-infra-only=false --set-app-log-content-access=true --set-host-group={host-group}
```
## 3. Desplegar app
```
docker compose up
```
## Opcional:

Variables de entorno (Si se va usar OTEL exporter):
```
export DT_API_TOKEN=dt0c01.XXXXXXXXXXXX.XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
export DT_OTLP_ENDPOINT=https://www.{id}.live.dynatrace.com/api/v2/otlp
export OTEL_EXPORTER_OTLP_METRICS_TEMPORALITY_PREFERENCE=delta
```

Comandos adicionales:
```
Detener contenedores:   docker stop $(docker ps -a -q)
Eliminar contenedores:  docker container prune
Eliminar imagenes:      docker image prune -a
```

Debug (adicional):
```
sudo docker exec -it redis-test redis-cli -h localhost -p 6380
sudo docker exec -it redis-cart sh
sudo apt install openjdk-17-jdk openjdk-17-jre
sudo snap install --classic code
https://linuxize.com/post/how-to-install-gradle-on-ubuntu-20-04/
```

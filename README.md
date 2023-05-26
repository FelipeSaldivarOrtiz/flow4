# flow4
Estación de clima con docker compose

# Ejercicios de NodeRed y MySQL

## Requisitos previos

1. Arrancar el sistema de docker
- docker ps -a
- docker start $(docker ps -a -q)
- netstat -an | grep tcp
2. El archivo compose debe hacer uso de volumenes en el contenedor de mosquitto para que haga uso de un archivo mosquitto.conf personalizado que permita las conexiones externas

- Detener el contenedor de mosquitto

	```docker stop [id_contenedor]```
	
- Eliminar el contenedor de mosquitto

	```docker rm [id_contenedor]```
	
- Eliminar la imagen de mosquitto

	```
	docker images
	docker rmi [id_cimagen]
	```
- Detener docker compose. Hay que entrar al directorio de compose

	```docker compose stop```
	

- Actualizar el archivo compose para que use el volumen que apunta al archivo mosquitto.conf y el archivo YAML

- Levantar docker compose

	```docker compose up -d``

## [Flow4: Estación climatica distribuida](https://edu.codigoiot.com/mod/lesson/view.php?id=3899)

### Requisitos

- Tener funcionando NodeRed

## Instrucciones

1. Crear un nuevo flow
2. Agregar un nodo mqtt
	- Agregar un nuevo broker y agregar únicamente la url
	- Server: mosquitto
	- Tema: ```codigoIoT/mqtt/clima```
	- Output: a String
3. Agregar un nodo JSON. Siempre convertir a JavaScript Object
4. Agregar dos nodos function

Nodo function temperatura

```
msg.payload = msg.payload.temp;
msg.topic = "Temperatura";
return msg;
```

Nodo function humedad

```
msg.payload = msg.payload.hum;
msg.topic = "Humedad";
return msg;
```

5. Crear una pestaña y dos grupos de informacion
- Pestaña: Clima local
- Grupo1: Indicadores
- Grupo2: Gráfica
6. Agregar 2 nodos gauge y configurarlos
- Asociarlos al grupo indicadores
- Determine etiquetas y rangos
7. Agregar el nodo chart
- Asociarlos al grupo indicadores
8. Probar el flow enviando el siguiente JSON por terminal con mosquitto

```
{
	"temp":18,
	"hum":51
}
```

	Comando de docker

	```
	docker exec -it [id_contenedor] mosquitto_pub -t codigoIoT/mqtt/clima -m '{"temp":18;"hum":51}'
	``

## Resultados

![](flow4/flow4.png)
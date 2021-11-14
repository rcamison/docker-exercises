## 5.- Crear un fichero docker-compose que use el motor de indexación (ElasticSearch) y la herramienta de visualización (Kibana).

### Paso 1: Crear el fichero docker-compose.yml
```
version: '3.6'

services:
 elasticsearch:
    # Utilizar la imagen de elasticsearch v7.9.3
    image: elasticsearch:7.9.3  
    # Asignar un nombre al contenedor
    container_name: els
    # Define las siguientes variables de entorno:
    # discovery.type=single-node
    environment: 
      - discovery.type=single-node    
    # Mapea el Puerto externo 9200 al puerto interno del contenedor 9200
    # Idem para el puerto 9300
    ports:
      - "9200:9200"
      - "9300:9300"
    # Emplazar el contenedor a la red de elastic  
    networks:
      - elastic    
 
 kibana:
    # Utilizar la imagen kibana v7.9.3
    image: kibana:7.9.3  
    # Asignar un nombre al contenedor
    container_name: kib    
    # Define las siguientes variables de entorno:
    # ELASTICSEARCH_HOST=elasticsearch
    # ELASTICSEARCH_PORT=9200
    environment: 
      - ELASTICSEARCH_HOST=elasticsearch    
      - ELASTICSEARCH_PORT=9200   
    # Mapear el puerto externo 5601 al puerto interno 5601  
    ports:
      - "5601:5601"
    # El contenedor Kibana depe esperar a la disponibilidad del servicio elasticsearch  
    depends_on:
      - elasticsearch
    # Emplazar el contenedor a la red de elastic  
    networks:
      - elastic   

# Definir la red elastic (bridge)    
networks:
 elastic:
# no hace falta especificar 'bridge' porque por defecto es la que se usa. Si explicitamente se quiere especificar hay que descomentar
# la siguiente instrucción
#    driver: bridge    
```

### Paso 2: Levantar el contenedor mediante la instrucción docker-compose up en el CLI de Docker situados en el directorio que tiene el fichero docker-compose.yml
	docker-compose up -d
	Los contenedores se llaman els y kib

	![image](https://user-images.githubusercontent.com/85695417/141696697-d7988aaf-27b1-4103-97ab-aee0ab1c1b19.png)
	
### Paso 3: comprobar

![image](https://user-images.githubusercontent.com/85695417/141696766-9ff84e24-98c7-4ba5-a85f-5885336f5521.png)
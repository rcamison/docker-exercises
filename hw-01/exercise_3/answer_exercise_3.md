## 3.- Crea un contenedor con las siguientes especificaciones:
	a. Utilizar la imagen base NGINX haciendo uso de la versión 1.19.3
	b. Al acceder a la URL localhost:8080/index.html aparecerá el mensaje HOMEWORK 1
	c. Persistir el fichero index.html en un volumen llamado static_content

### Paso 1: Crear el fichero Dockerfile. Las instrucciones que se han utilizado para el fichero son:
	- Utilizar la imagen de nginx con la versión requerida
	FROM nginx:1.19.3
	
	- Copiar el archivo index.htm de la carpeta src desde el host a la carpeta del contenedor
	COPY /src/index.html /usr/share/nginx/htmln
	
### Paso 2: Crear un volumen 'static_content' en el CLI de Docker mediante la siguiente instrucción
	docker volume create static_content 
	
### Paso 3: Construir la imagen del contenedor. El nombre de la imagen es 'raul_nginx'
	docker build -t raul_nginx .
	
### Paso 4: Crear el contenedor	con nombre 'raul_container' utilizando la imagen construida en el paso 3
	- Mediante el parámetro -v hacemos que el volumen 'static_content' creado en el paso 2 apunte al directorio del contenedor donde se encuentra el archivo index.html. De esta forma persistimos todo el contenido del directorio /usr/share/nginx/html del contenedor en el volumen
	docker run -d --name raul_container -v static_content:/usr/share/nginx/html -p 8080:80 raul_nginx
	
### Paso 5: Acceder a la URL http://localhost/8080/index.html y comprobar que aparece la página deseada
![image](https://user-images.githubusercontent.com/85695417/141694745-1953f3d8-071e-4515-bc9d-caae8ffb688a.png)

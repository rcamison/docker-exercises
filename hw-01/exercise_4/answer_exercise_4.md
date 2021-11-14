## 4.- Crea una imagen docker a partir de un Dockerfile. Esta aplicaci�n expondr� un servicio en el puerto 8080 y se deber� hacer uso de la instrucci�n HEALTHCHECK para comprobar si la aplicaci�n est� ofreciendo el servicio o por si el contrario existe un problema.
	- La prueba se realizar� cada 45 segundos
	- Por cada prueba realizada, se esperar� que la aplicaci�n responda en menos de 5 segundos. Si tras 5 segundos no se obtiene respuesta, se considera que la prueba habr� fallado
	- Ajustar el tiempo de espera de la primera prueba (Ejemplo: Si la aplicaci�n del contenedor tarda en iniciarse 10s, configurar el par�metro a 15s)
	- El n�mero de reintentos ser� 2. Si fallan dos pruebas consecutivas, el contenedor deber� cambiar al estado �unhealthy�)

### Paso 1: Utilizando como base el fichero Dockerfile del punto 3 se a�ade la instrucci�n HEALTHCHECK
	- Con el par�metro 'interval' indicamos cada cuantos segundos se realiza la prueba
	- Con el par�metro 'timeout' indicamos los segundos en los que debe responder la aplicaci�n para que la prueba no se considere fallida
	- Con el par�metro 'start-period' indicamos el tiempo de espera para la primera prueba
	- Con el par�metro 'retries' indicamos el n�mero de reintentos para cambiar el estado a "unhealthy"
	- Mediante el CMD curl... hacemos una llamada a http://localhost/index.html para ver si la aplicaci�n responde
	HEALTHCHECK --interval=45s  --timeout=5s  --start-period=5s  --retries=2 CMD curl --fail http://localhost/index.html || exit 1
	
## Paso 2: construir la imagen
	 docker build -t raul_healthcheck .

## Paso 3: ejecutar el contenedor
	docker run --name raul_healthcheck_container -p 8080:80 raul_healthcheck

## Paso 4: una vez que el contenedor pasa a un estado "healthy" se comprueba que la prueba se ejecuta cada 45 segundos
![image](https://user-images.githubusercontent.com/85695417/141695628-5290bc4c-627a-4669-8de3-c711cdc8f31e.png)

## Paso 5: comprobar que tras dos fallos el contenedor pasa a un estado "unhealthy". Para provocar el error se ha renombrado el archivo index.html como index2.html. De esta forma la llamada fallar�
![image](https://user-images.githubusercontent.com/85695417/141695672-f0617442-320c-46bb-adb3-3197a58c021f.png)

![image](https://user-images.githubusercontent.com/85695417/141695724-57c0aeae-2fce-4667-a393-abff42586a77.png)

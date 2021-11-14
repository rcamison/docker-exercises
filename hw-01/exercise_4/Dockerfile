#Contenedor ngnix de Raul Camison
FROM nginx:1.19.3

#Copiar el fichero index al directorio de nginx del contenedor
COPY /src/index.html /usr/share/nginx/html

#Parametrizar la instrucción HEALTHCHECK. Se hace una llamada a index.html
HEALTHCHECK --interval=45s  --timeout=5s  --start-period=5s  --retries=2 CMD curl --fail http://localhost/index.html || exit 1

# Proyecto Nginx

## Configuración inicial

Una vez realizado nuestro despliegue del servicio **Ngix** correspondientes con el servicio de múltilples sitios web seguiremos añadiendo pequeñas configuraciones en este mismo servicio.

## Bloqueo de acceso de IP concretas

Para ello debemos crear un fichero llamado [```blacklist.conf```]() donde añadimos las políticas restrictivas sobre las IPS que querramos indicar.

```yml

deny 192.168.1.0/24;
allow all;

```
Para ello debemos ir al fichero de configuración [```nginx.conf```](https://github.com/ndiazdossantos/nginx/blob/master/nginx.conf) y en el apartado server añadir un include del fichero de de la blacklist creada previamente.

```yml

    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        #root         /usr/share/nginx/html;
        root /home/ec2-user/html;
        # Load configuration files for the default server block.
        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/blacklist.conf

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }
```

Posteriormente debemos dar permisos al usuario de poder leer ficho fichero para ello podemos cambiarle la propiedad del mismo con el comando:

`$ chown ec2-user /etc/nginx/blacklist.conf`

Seguidamente de ejecutar el comando reiniciamos el servicio de nginx y ya estaría activo el bloqueo de las IPS indicadas, cada vez que modifiquemos el archivo debemos reiniciar el servicio.

# README  Imagen Base Contabilidad PRO QubiQ V.12 #



### Configuración Inicial del proyecto  ###

Basada en : https://github.com/Tecnativa/docker-odoo-base/tree/scaffolding

	Configurar ./src/repos.yaml
	Configurar .src/addons.yml

	Descargamos la imagen base:
		- docker-compose build --pull

	Actualizamos los repos y actualizamos la carpeta final de addons:
		- docker-compose -f setup-devel.yaml run --rm odoo

	###  Desarrollo --> Arrancar el contenedor final: ###
		- docker-compose up

	###  Producción --> Arrancar el contenedor final: ###
		- ./production_up.sh

### Acceso a la instancia  ###

	El puerto, en entornos de desarrollo, se define según la versión de odoo:
		- v8: http://localhost:08069
		- v9: http://localhost:09069
		- v10: http://localhost:10069
		- v11: http://localhost:11069
		- v12: http://localhost:12069

	La opción PGDATABASE (que aparece tanto en prod.yaml como en docker-compose.yml) 
	define la base de datos que cargará el docker.

	Una vez restoreada la nueva BBDD hay que parar el docker y cambiar el valor de PGDATABASE

### Configuración de la instancia ###

Cambios y configuraciones a tener en cuenta cuando se crea una nueva instancia (GET OUT DEVELOPERS!)

	Realizar, mediante command en docker, un actualización de todos los módulos del sistema:
		command:
            - odoo
            - --update=all

### Como restaurar un filestore ###
Introducimos el filestore en ./tmp/restore/filestore
Renombramos el nombre de la carpeta por el nombre de la db ./tmp/restore/filestore/<db_name>

1. CONTAINER=<nombre_contenedor_odoo>
1. docker cp ./tmp/filestore $CONTAINER:/var/lib/odoo
2. docker exec -u 0 $CONTAINER bash -c "chown -R odoo /var/lib/odoo/*"
3. docker exec -u 0 $CONTAINER bash -c "chgrp -R odoo /var/lib/odoo/*"
4. docker exec -u 0 $CONTAINER chmod -R 751 /var/lib/odoo/filestore


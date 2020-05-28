![GLPI Logo](https://raw.githubusercontent.com/glpi-project/glpi/master/pics/logos/logo-GLPI-250-black.png)

# GLPI Docker Container


## About GLPI

GLPI stands for **Gestionnaire Libre de Parc Informatique** is a Free Asset and IT Management Software package, that provides ITIL Service Desk features, licenses tracking and software auditing.


## License

![license](https://img.shields.io/github/license/glpi-project/glpi.svg)


## Install on docker container 

[![asciicast](https://asciinema.org/a/277490.svg)](https://asciinema.org/a/277490)


### Download and Install


    curl -sSL https://raw.githubusercontent.com/fametec/glpi/9.4.4-nginx/docker/docker-compose.yml -o docker-compose.yml


### docker-compose.yaml

    version: "3.5"
    services:
        mariadb-glpi: 
            image: fametec/mariadb-glpi:9.4.6
            restart: unless-stopped
            volumes:
              - mariadb-glpi-volume:/var/lib/mysql:rw
            environment: 
              MYSQL_DATABASE: glpi
              MYSQL_USER: glpi 
              MYSQL_PASSWORD: glpi 
              MYSQL_RANDOM_ROOT_PASSWORD: 1 
              #        ports:
              #- 3306:3306
            networks: 
              - glpi-backend
    #
    #
        glpi: 
            image: fametec/glpi:9.4.6-nginx
            restart: unless-stopped
            volumes:
              - glpi-volume-files:/usr/share/nginx/html/glpi/files:rw
              - glpi-volume-plugins:/usr/share/nginx/html/glpi/plugins:rw
            depends_on: 
              - mariadb-glpi
              - php-fpm
            ports: 
              - 80:80
            networks: 
              - glpi-frontend
              - glpi-backend

        php-fpm: 
            image: fametec/glpi:9.4.6-php-fpm
            restart: unless-stopped
            volumes:
              - glpi-volume-files:/usr/share/nginx/html/glpi/files:rw
              - glpi-volume-plugins:/usr/share/nginx/html/glpi/plugins:rw
            depends_on:
              - mariadb-glpi
            networks:
              - glpi-backend
    #        ports:
    #          - 9000:9000
    #
        # cron:
        #     image: fametec/glpi-cron-php-fpm:latest
        #     restart: unless-stopped
        #     volumes:
        #       - glpi-volume-files:/usr/share/nginx/html/glpi/files:rw
        #       - glpi-volume-plugins:/usr/share/nginx/html/glpi/plugins:rw
        #     depends_on:
        #       - mariadb-glpi
        #     networks:
        #       - glpi-backend
    #
    networks: 
        glpi-frontend: 
        glpi-backend:
    #
    volumes:
        glpi-volume-files:
        glpi-volume-plugins:
        mariadb-glpi-volume:




### Deploy with docker-compose


    docker-compose up



## Courses e-Learning

https://www.fametreinamentos.com.br


## Support

https://www.fametec.com.br
    
contato@fametec.com.br


# Blueprint for docker-flask-gunicorn-nginx web application

![Example App](app_example.jpg?raw=true "Example App")

Bootstrap example of a *Flask/Dash* app served via *Gunicorn* and *Nginx* using Docker containers

Guildeline article can be found at https://sladkovm.github.io/webdev/2017/10/16/Deploying-Plotly-Dash-in-a-Docker-Container-on-Digitital-Ocean.html

## Run

```bash
make build
```

In your browser (assuming the docker-machine runs on 192.168.99.100) go to:

    http://192.168.99.100

To clean up the container mess, run
```
make clean
```

It will shut down all container and remove all images

## Prominent features:

1. Dockerized application orchestrated by docker-compose
2. Gunicorn as a WSGI and Nginx as a reverse proxy are included as services
3. Nginx is configured to serve static files, e.g. images, css etc.
4. Example of routing implementation in *Dash* app is shown
5. Build process uses *requirements.txt*, but *Pipenv* files are included to ease the development process
6. Bootstrap css is included
7. Standard Single Page App Layout with Header, Main and Footer is set up

# Docker deploy  
from E:\GitHub\osteotomy_docker\docs-old\README.md

## Installation von docker
Achtung, Fehler bei Installation von docker über Snap. Siehe meinen Kommentar in stack overflow:
https://stackoverflow.com/questions/51729836/error-response-from-daemon-cannot-stop-container/64120350#64120350
   
# after changes: create a new image
 docker build -t yetigo/osteotomy:latest .  
 docker build -t yetigo/osteotomy_ssl:latest . 

# test the new image locally   
 docker run --name pum -d -p 8000:5000 --rm yetigo/osteotomy:latest  
 docker run --name pum -d -p 8000:5000 --rm yetigo/osteotomy_ssl:latest  

# download docker SSL container image and run it locally
 docker pull yetigo/osteotomy_ssl:latest    
 docker run -v E:\Strato_osteotomy.eu_Zertifikat\osteotomy_tool\:/ssl/private -d --name pum yetigo/osteotomy_ssl:latest 
 
# upload docker container image  
 docker push yetigo/osteotomy:latest  
 docker push yetigo/osteotomy_ssl:latest 

# download docker container image 
 docker pull yetigo/osteotomy:latest  
 docker run --name pum -d -p 8000:5000 --rm yetigo/osteotomy:latest   

 
# deploy on server: download docker SSL container image 
# in Windows PowerShell (sollte auch im Linux Terminal funktionieren):  
 ssh uwe@85.214.33.21
 password:**********
 docker pull yetigo/osteotomy_ssl:latest    
 docker run --restart=always -v //etc/ssl/certs:/ssl/private -d --name pum yetigo/osteotomy_ssl:latest 

# display active containers 
 docker ps  
 docker container ls  
 
# stop Container:
docker container stop pum   

# remove all docker containers 
docker rm $(docker ps -a -q)

# remove Container:
docker container rm pum  

# display Images:
docker images

# remove Image:
docker image rmi yetigo/osteotomy:latest

# remove all docker images 
docker rmi $(docker images -q)

# debugging docker run -> exit
source: https://stackoverflow.com/questions/38112968/how-to-know-the-reason-why-a-docker-container-exits  
docker logs $container_id / docker logs pum
docker inspect $container_id / docker inspect pum

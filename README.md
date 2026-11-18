docker pull mysql:8.0
docker pull wordpress:latest
docker images
docker network create library-net
docker network ls
docker volume create mysql-data
docker volume ls
create docker-compose.yml 


docker compose up -d
docker stop library-db
docker start library-db
docker kill library-db

mvn clean package
FROM tomcat:9.0
COPY target/metro-reservation-1.0.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080

services:

  db:
    image: mysql:8.0
    container_name: library-db
    restart: always

    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: library
      MYSQL_USER: libraryuser
      MYSQL_PASSWORD: lib123

    volumes:
      - mysql-data:/var/lib/mysql

    networks:
      - library-net

  wordpress:
    image: wordpress:latest
    container_name: library-wp
    restart: always

    ports:
      - "8080:80"

    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: libraryuser
      WORDPRESS_DB_PASSWORD: lib123
      WORDPRESS_DB_NAME: library

    depends_on:
      - db

    networks:
      - library-net

volumes:
  mysql-data:
    external: true

networks:
  library-net:
    external: true
----------------------------
services:

  db:
    image: mysql:8.0
    container_name: metro-db
    restart: always

    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: metro
      MYSQL_USER: metrouser
      MYSQL_PASSWORD: metro123

    volumes:
      - mysql-data:/var/lib/mysql

    networks:
      - metro-net

  app:
    build: .
    container_name: metro-app
    restart: always

    ports:
      - "8080:8080"

    depends_on:
      - db

    networks:
      - metro-net

volumes:
  mysql-data:
    external: true

networks:
  metro-net:
    external: true
---------
services:

  web:
    image: nginx:latest
    container_name: web-app
    restart: always

    ports:
      - "8081:80"

    networks:
      - monitoring-net

  nagios:
    image: jasonrivers/nagios:latest
    container_name: nagios-server
    restart: always

    ports:
      - "8080:80"

    depends_on:
      - web

    networks:
      - monitoring-net

networks:
  monitoring-net:
    external: true
------------------------------------------------------

H/5 * * * *  target/*.jar
node {
    stage('Git Checkout') {
        git branch: 'master',
            url: ''
    }
    stage('Maven Clean') {
        bat 'mvn clean'
    }
    stage('Maven Build') {
        bat 'mvn compile'
    }
    stage('Maven Test') {
        bat 'mvn test'
    }

    stage('Maven Package') {
        bat 'mvn package'
    }

}


icacls "library-key.pem" /inheritance:r
icacls "library-key.pem" /remove:g *S-1-5-32
icacls "library-key.pem" /grant:r "$('Aluru Unnathi'):(R)"
ssh -i "library-key.pem" ubuntu@<EC2-PUBLIC-IP>
ssh -i "library-key.pem" ubuntu@54.91.216.46

sudo apt update
sudo apt install openjdk-21-jdk -y
sudo apt install git maven -y
git clone
cd library-management
jar
 mvn clean package
ls target/
java -jar target/library-management-1.0-SNAPSHOT.jar
find src -name "*.java"
java -cp target/classes com.library.LibraryManagementt

war
mvn clean package
ls target/
sudo apt update
sudo apt install tomcat9 -y
sudo cp target/LibraryManagement.war /var/lib/tomcat9/webapps/
sudo systemctl restart tomcat9
sudo systemctl status tomcat9
http://<EC2-PUBLIC-IP>:8080/LibraryManagement




docker build -t metro-app1:latest .
minikube image load metro-app1:latest
kubectl create deployment metro-app --image=metro-app1:latest
kubectl patch deployment metro-app -p "{\"spec\":{\"template\":{\"spec\":{\"containers\":[{\"name\":\"metro-app\",\"imagePullPolicy\":\"Never\"}]}}}}"
kubectl get pods
kubectl scale deployment metro-app --replicas=4
kubectl get deployments
kubectl get pods
kubectl delete pod POD_NAME
kubectl get pods -w
kubectl get deployments
kubectl get replicasets
kubectl get pods





flask folder:
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Food Ordering Flask API is running!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

FROM python:3.12
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY app.py .
EXPOSE 5000
CMD ["python", "app.py"]

services:

  flask:
    build: ./flask
    container_name: flask-api
    restart: always
    ports:
      - "5000:5000"
    networks:
      - restaurant-net

  nginx:
    image: nginx:latest
    container_name: nginx-server
    restart: always
    ports:
      - "80:80"
    depends_on:
      - flask
    networks:
      - restaurant-net

networks:
  restaurant-net:
    driver: bridge

5000 , 80


# Student List
This projet contains an api and a simple website which request data from the api. It will be divided in two microservices: The api microservice and the website microservice.
We consider each directory as a microservice. To create the api microservice we add a [Dockerfile](https://github.com/yoeko/student-list/blob/dev-yoeko/simple_api/Dockerfile)

## How to launch the app

### Clone this repository
```git clone -b dev-yoeko https://github.com/yoeko/student-list.git```

### Build api image
```cd ./student-list/simple_api && docker build -t student:v1 .```

### Launch all containers with docker-compose
```cd ../ && docker-compose up -d```

### Access the website
[http://localhost:8080/](http://localhost:8080/)



## The process of making this work
### Build the api image
```docker build -t student:v1 .```

### Run the image to create a container
```docker run --name student-api -d -v ${PWD}:/data/ -p 8000:5000 student:v1```

## WEBSITE
### Create the docker-compose.yml file

### Update the index.php file to make the GET request works
When access the website we have this error 
![Load data Error](https://github.com/yoeko/student-list/blob/dev-yoeko/Screenshot%20from%202023-05-16%2000-00-37.png)

Replace line 29:
```$url = 'http://<api_ip_or_name:port>/pozos/api/v1.0/get_student_ages';```

by

```$url = 'http://simple_api:5000/pozos/api/v1.0/get_student_ages';```

Relaunch the services
```docker-compose up -d```

Then hit the button "List Student"

The error has been corrected

![Student list screen](https://github.com/yoeko/student-list/blob/dev-yoeko/Screenshot%20from%202023-05-15%2023-58-44.png)


## Registry
### Add registry service in the docker-compose.yml
```yml
registry:
  image: registry:2
  container_name: registry
  ports:
    - '5000:5000'
  networks:
    - registry_network
docker-registry-ui:
  image: joxit/docker-registry-ui
  container_name: registry-frontend
  ports:
    - '8090:80'
  networks:
    - registry_network
  environment:
    - REGISTRY_URL=http://registry:5000
    - DELETE_IMAGES=true
    - REGISTRY_TITLE=registry_ui
```

### Retag the image to push
```docker tag "IMAGE_ID" localhost:5000/student:v1```

### Push the image to the local registry
```docker push localhost:5000/student:v1```

### Access the registry
[http://localhost:8090/](http://localhost:8090/)

## API
### Build the image
docker build -t student:v1 .

### Run the image to create a container (no need to execute this for the exercice, we use docker compose instead)
docker run --name student-api -d -v ${PWD}:/data/ -p 8000:5000 student:v1

## WEBSITE
### Write the ../website/docker-compose.yml file
cd ../website && docker compose up -d

### Access to the website url
[http://localhost:8080/](http://localhost:8080/)

### Update the ../index.php file to make the GET request works
Line 29: 
Replace
$url = 'http://<api_ip_or_name:port>/pozos/api/v1.0/get_student_ages';
by
$url = 'http://simple_api:5000/pozos/api/v1.0/get_student_ages';

Then hit the button "List Student"


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
docker tag "IMAGE_ID" localhost:5000/student:v1

### Push the image to the local registry
docker push localhost:5000/student:v1

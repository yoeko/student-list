## API
### Build the image
docker build -t student:v1 .

### Run the image to create a container (no need to execute this for the exercice, we use docker compose instead)
docker run --name student-api -d -v ${PWD}:/data/ -p 8000:5000 student:v1

## Registry
### run the registry
docker run -d -p 5000:5000 -e REGISTRY_STORAGE_DELETE_ENABLED=true --net student-list_api_network --name registry registry:2

### run the registry ui
docker run -d -p 8090:80 --net student-list_api_network \
-e REGISTRY_URL=http://registry:5000 \
-e DELETE_IMAGES=true \
-e REGISTRY_TITLE=my_registry \
--name registry-frontend joxit/docker-registry-ui

### Retag the image to push
docker tag "IMAGE_ID" localhost:5000/student:v1

### Push the image to the local registry
docker push localhost:5000/student:v1

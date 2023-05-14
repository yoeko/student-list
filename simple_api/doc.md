### Build the image
docker build -t student:v1 .

### Run the image to create a container
docker run --name student-api -d -v ${PWD}:/data/ -p 8001:5000 student:v1

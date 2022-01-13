# mySampleMicroserviceWithDocker

There are 2 services are configure in internal and external directories.
Express Web Service using nodejs on two microservices
The internal service receives REST requests on port 8082 and returns mock data.
The external service inserts JSON from the server into an html template in the Views folder and returns it to the requestor on port 8080.
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
# WebUser ====Port8080===> ExternalService =====RESTPort8082===> InternalService #
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

**internal Dockerfile**
FROM node                   # pulling nodejs image from dockerhub
COPY . /tmp/internal/       # copying all the files from current dir to node image container under /tmp/internal/ dir
WORKDIR /tmp/internal       # making default working dir to /tmp/internal dir
EXPOSE 8082                 # exposing 8082 from docker container
RUN ["npm","install"]       # installing npm
CMD ["node","server.js"]    # starting server.js after installing the npm

**external Dockerfile**
FROM node
COPY . /tmp/external/
WORKDIR /tmp/external
EXPOSE 8080
RUN ["npm","install"]
CMD ["npm","start"]         # starting npm server in the runing container

Once the Dockerfile is created for both internal and external. Build image from this Dockerfile using the below command
docker build -t internal:v1.0 .   # run this command under the internal dir
docker build -t external:v1.0 .   # run this command under the external dir

Your images is created and you can check it by using **docker images** command and will expect this kind of output.
REPOSITORY   TAG       IMAGE ID       CREATED             SIZE
external     v1.0      2c3277a0df21   About an hour ago   1.03GB
internal     v1.0      1dcaf030246c   About an hour ago   1.02GB

Now run the below commands to explore sample microservices on Web and then able to access from browser using ip and 8080 port. 
docker run -d -p 8082:8082 -e PORT=8082 internal:v1.0
docker run -d -e SERVER='http://localhost:8082' --network="host" external:v1.0

### Docker
-   docker version
-   docker run hello-world

### Manipulating Containers with the Docker Client:
-   docker run hello-world
-   docker run busybox ls/ echo hi there 
-   docker run hello-world ls
***docker: Error response from daemon: OCI runtime create failed: container_linux.go:380: starting container process caused: exec: "ls": executable file not found in $PATH: unknown.
ERRO[0000] error waiting for container: context canceled*** 
-   docker ps
(base) anandramu@Anand-ka-Mini Ansible  % docker ps
CONTAINER ID   IMAGE     COMMAND                 CREATED          STATUS          PORTS     NAMES
126e1803200b   busybox   "ping www.google.com"   32 seconds ago   Up 31 seconds             nervous_shockley
-   docker ps --all (all the containers executed so far)
-   docker run = docker create + docker start
-   docker create hello-world ***Lifecycle of a container***
196db934790aa8e6d189bbbce505fbac75158559cdc6b0df97e814beb8c30d71
-   docker start -a 196db934790aa8e6d189bbbce505fbac75158559cdc6b0df97e814beb8c30d71      

Hello from Docker!

-   -a watches for docker output and print out 

- docker ps --all : displays all the output 
- docker start -a 56abb3c3a6a8 ***use the ID of the start***
- docker system prune 
    WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

- docker logs <container_ID> ***not restarting the container`, just the logs from the previous run***
- docker stop <container_ID> Hardware singal SIGTERM to stop the process in a container 
- docker kill <container_ID> SIGKILL, shutdown rite now , not additonal tasks . 10 seconds for Docker Stop , else it will fall back to kill 
- redis = in Memory Datastore

- docker run redis
    - docker exec -it <container_id> <command>
    - docker exec -it 3092e4fd6c87 redis-cli
    (base) anandramu@Anand-ka-Mini Ansible  % docker exec -it 3092e4fd6c87 redis-cli 
        127.0.0.1:6379> set myvalue 5
        OK
        127.0.0.1:6379> get myvalue
        "5"
        127.0.0.1:6379> 
    - with out -it intereactive terminal 
***Purpose of IT flag***
- STDIN , STDOUT, STDERR
    -i = terminal to the standin channel 
    -t = nicely formatted manner 
***docker exec**
- SHELL access to the container 
- docker exec -it 3092e4fd6c87 sh
        # ls
        # cd /
        # ls
        bin   data  etc   lib    mnt  proc  run   srv  tmp  var
        boot  dev   home  media  opt  root  sbin  sys  usr
- docker run -it busybox sh

### 3 Building Custom images through Docker Server

- DOCKER FILE -> Docker Client -> Docker Server -> Usable Image 
- Docker File -> base image -> run some commands to install additonal packages -> specify a command to run on container startup
- Build Kit enabled by default -> preferences -> docker engine

- Create an image that runs redis-server 
    1. Base image
        FROM alpine 
    2. download and install dependency

    3. run command when Container starts

INSTRUCTION telling Docker Server what to do
Argument to the instruction 

        (base) anandramu@Anand-ka-Mini redis_image % docker build . 
        [+] Building 7.5s (7/7) FINISHED                                                        
        => [internal] load build definition from Dockerfile                               0.0s
        => => transferring dockerfile: 242B                                               0.0s
        => [internal] load .dockerignore                                                  0.0s
        => => transferring context: 2B                                                    0.0s
        => [internal] load metadata for docker.io/library/alpine:latest                   3.7s
        => [auth] library/alpine:pull token for registry-1.docker.io                      0.0s
        => [1/2] FROM docker.io/library/alpine@sha256:635f0aa53d99017b38d1a0aa5b2082f781  0.6s
        => => resolve docker.io/library/alpine@sha256:635f0aa53d99017b38d1a0aa5b2082f781  0.0s
        => => sha256:635f0aa53d99017b38d1a0aa5b2082f7812b03e3cdb299103fe 1.64kB / 1.64kB  0.0s
        => => sha256:b4da1299e77c63f8706427cd39154e726a9b7646d29e2fc05d65b51 528B / 528B  0.0s
        => => sha256:5b8b7b6352293ecb87f49e6202b97cb9cbd63cf6bc2940e4028 1.49kB / 1.49kB  0.0s
        => => sha256:be307f383ecc62b27a29b599c3fc9d3129693a798e7fcce614f 2.72MB / 2.72MB  0.4s
        => => extracting sha256:be307f383ecc62b27a29b599c3fc9d3129693a798e7fcce614f09174  0.1s
        => [2/2] RUN apk add --update redis                                               3.1s
        => exporting to image                                                             0.0s
        => => exporting layers                                                            0.0s
        => => writing image sha256:1a6f896c8562279996053d5241cb7f52d5f1387449ae8517c09b9  0.0s

 - docker build .

 Writing a docker file == Being given a computer with NO OS and run Chrome

 1. install an OS
 2. Start up a default browser
 3. Navigate to Chrome.google.com
 4. Download installer
 5. Execute the installer
 6. Execute Chrome 

        (base) anandramu@Anand-ka-Mini redis_image % docker build .
        [+] Building 7.6s (8/8) FINISHED                                                        
        => [internal] load build definition from Dockerfile                               0.0s
        => => transferring dockerfile: 267B                                               0.0s
        => [internal] load .dockerignore                                                  0.0s
        => => transferring context: 2B                                                    0.0s
        => [internal] load metadata for docker.io/library/alpine:latest                   2.5s
        => [auth] library/alpine:pull token for registry-1.docker.io                      0.0s
        => [1/3] FROM docker.io/library/alpine@sha256:635f0aa53d99017b38d1a0aa5b2082f781  0.0s
        => CACHED [2/3] RUN apk add --update redis                                        0.0s
        => [3/3] RUN apk add --update gcc                                                 4.8s
        => exporting to image                                                             0.2s
        => => exporting layers                                                            0.2s
        => => writing image sha256:5cafedd4187712045654dc239260eac8788bc548e242ed7fafb3f  0.0s

 - updating another package
 - tag the image
    - docker build -t anandbr2005/redis_server:latest . 
    - docker build -t <docker_id/Repo>/<project_name>:<version_number>
    - redis_image % docker run anandbr2005/redis_server 
- Manual Image Generation with Docker Commit
    -   docker run -it alpine sh
    -   apk add --update redis
        - running container with file system
    - Take a snapshot of running container and assign default command
        - docker commit -c 'CMD ["redis-server"]' <docker_id>
        - docker run 


### Real Project with Docker 
1. Create a Node JS Web app
2. Create a Dockerfile
3. Build image from dockerfile
4. run image as container
5. Connect to web app from a browser 

Specify a base image  - FROM alpine
Run commands to install additonal programs - RUN npm install
specify a commnd to run on the container startup - CMD ["npm", "start"]

- package.json and index.js is natively not available in the Container 

- Port mapping 
    - docker run -p 8080 : 8080 <image_id>
- using cache order of Operations
    COPY ./package.json ./
    RUN npm install
    COPY ./ ./

### Docker Compose with MUltiple Local Container

Node APP -> Redis (number of visits)
- Setup network connection between Node APP -> Redis 
    -   using Docker CLI
    -   using Docker Compose
            startup multiple Docker Containers at same time
            AUtomates long winded arguments we were passing to 'docker run'
    - docker-compose.yml = docker build -t anandr2005/visits:latest
    - docker run myimage - docker-compose up
    - docker build/docker run myimage - docker-compose up --build
    - docker compose up
    - stopping docker compose containers
        - docker run -d redis (running in background)
        - docker stop
        - docker-compose up -d
        - docker ps
        - docker-compose down 
- docker-compose - auto start the crashed container
    - "no"  = never attempt to restart this , container if it stops 
    - always = always restart 
    - on-failure = only restart if the container stops with an error code
    - unless-stopped = always restart unless we forcibly stop
- docker status with Compose 

### Creating a Production-grade workflow

Development -> Testing -> Deployment 
***FLow Specifics***

GitHub Repo -> Feature Branch -> (pull request)-> Travis CI (Run tests) -> 
Merge to Master -> Travis CI (run tests)-> deploy to AWS Elastic BeanStalk

- npx create-react-app frontend 
- npm run start : starts up a Development server, For development use only
- npm run test : runs tests associated with the project
- npm run build : buils a production version of the application


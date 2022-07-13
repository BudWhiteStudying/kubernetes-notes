
# Running a multi-module Quarkus app on Kubernetes

## Running a multi-module Quarkus app on Docker

1. navigate to the main module
2. issue the `quarkus build` command, it should generate a `src/main/docker` directory, containing a `Dockerfile.jvm` file
3. issue the `docker build -f src/main/docker/Dockerfile.jvm -t quarkus/name-of-your-app .` command:
    - the `-f` option tells Docker where the Dockerfile is
    - the `-t` option tells Docker which tags should be associated with this Docker image
    - the `.` argument passed to the `docker build` command tells Docker  - **I don't know what**
4. issue the `docker image ls` command, the newly created `quarkus/name-of-your-app` image should show up among the local images
5. issue the `docker run -i --rm -p 8080:8080 quarkus/name-of-your-app` command:
    - the `-i` option tells Docker to keep STDIN open even if not attached (not strictly necessary, but lets you see the STDOUT logs of the Quarkus app)
    - the `--rm` option tells Docker to automatically remove the container when it exits
    - the `-p` option tells Docker to bind container port `8080` to localhost port `8080`
    - the `quarkus/name-of-your-app` argument passed to the `docker run` command indicates the name of the Docker image that should be ran
6. the Quarkus app should be available at `http://localhost:8080`

## Running a multi-module Quarkus app on Kubernetes

These procedures assumes that `Minikube` is installed.

### Without using Quarkus Kubernetes plugin

1. navigate to the main module
2. issue the `quarkus build` command, it should generate a `src/main/docker` directory, containing a `Dockerfile.jvm` file
3. issue the `docker build -f src/main/docker/Dockerfile.jvm -t quarkus/name-of-your-app .` command:
    - the `-f` option tells Docker where the Dockerfile is
    - the `-t` option tells Docker which tags should be associated with this Docker image
    - the `.` argument passed to the `docker build` command tells Docker  - **I don't know what**
4. issue the `docker image ls` command, the newly created `quarkus/name-of-your-app` image should show up among the local images
5. issue the `minikube image load quarkus/name-of-your-app:latest` command, it will load the newly created Docker image into the Minikube cluster
6. issue the `kubectl create deployment name-of-your-app --image=quarkus/name-of-your-app:latest` command, it tells Kubernetes to create a new Deployment named `name-of-your-app` based on the newly created Docker image
7. issue the `kubectl expose deployment name-of-your-app --type=NodePort --port=8080` command, it tells Kubernetes to expose (through a Service named `name-of-your-app`) the Deployment you just created on port `8080` - this port is not reachable from the local host, but only from within the Minikube cluster
8. issue the `kubectl port-forward service/name-of-your-app 7080:8080`, it tells Kubernetes to forward requests issued against `localhost` on port `7080` to port `8080` of your newly created Deployment
9. the Quarkus app should be available at `http://localhost:7080`

This will only work if the application context root is `/`. If it's anything more complicated I think we'll have to meddle with an Ingress.

### Using Quarkus Kubernetes plugin

`TODO`

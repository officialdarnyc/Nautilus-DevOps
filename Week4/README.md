# Steps taken to dockerize a simple app

Created a folder called **Simple-app** for the simple node.js app to be hosted on DockerHub. Copied over all the application code into that folder.
Created a _Dockerfile_ file so as to be able to copy the necessary files of the app to docker and run the necessary commands. Added the below
into that file:

```
FROM node:16

# Create an app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source code inside docker image
COPY . .

EXPOSE 8080

CMD ["node", "server.js"]
```
Created a CI/CD build pipeline using GitHub Actons by creating a `build.yaml` file in the directory `.github/workflow` under the `root` directory. The pipeline was designed to build and push the app to my DockerHub account `officialdarnyc`.

Link to the Docker Image: https://hub.docker.com/repository/docker/officialdarnyc/simple-app.

Ran the docker image with `-d` so that the container runs in detached mode (leaving the container running in the background). Also used the `-p` flag to redirect a public port to a private port inside the container. Ran the image using:
```
docker run -p 49160:8080 -d officialdarnyc/simple-app
```
Confirmed it was running using the `docker ps` command.
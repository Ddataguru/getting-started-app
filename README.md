# Getting started

This repository is a sample application for users following the getting started guide at https://docs.docker.com/get-started/.

The application is based on the application from the getting started tutorial at https://github.com/docker/getting-started
## Create a Docker File
1. Using nano command create a file named "Dockerfile"
2. Copy and paste the following syntax into the newly created "Dockerfile"
# syntax=docker/dockerfile:1

"FROM node:24-alpine
WORKDIR /app
COPY . .
RUN npm install --omit=dev
CMD ["node", "src/index.js"]
EXPOSE 3000"

# This Dockerfile does the following:

Uses node:24-alpine as the base image, a lightweight Linux image with Node.js pre-installed
Sets /app as the working directory
Copies source code into the image
Installs the necessary dependencies
Specifies the command to start the application
Documents that the app listens on port 3000


## Docker Command
Build the app's image
To build the image, you'll need to use a Dockerfile

The following command is used when building an Image "docker build -t getting-started ."

docker build command uses the Dockerfile to build a new image
-t - tags your image
getting-started -  The tag name i assigned my image
The . at the end of the docker build command tells Docker that it should look for the Dockerfile in the current directory.

## Start an App Container
Run your container using the docker run command and specify the name of the image you just created:
To start an app container use this command "docker run -d -p 127.0.0.1:3000:3000 getting-started"
-d - (short for --detach) runs the container in the background
-p - (short for --publish) creates a port mapping between the host and the container
127.0.0.1:3000:3000 - The command publishes the container's port 3000 to 127.0.0.1:3000 (localhost:3000) on the host

## Update Docker 
To make the following changes update the source code by changing the "empty text" when you don't have any todo list items to "You have no todo items yet! Add one above!"
1. In the src/static/js/app.js file, update line 56 to use the new empty text.
+ <p className="text-center">You have no todo items yet! Add one above!</p>
2. Build your updated version of the image, using the docker build command.
docker build -t getting-started .
3. Start a new container using the updated code.
docker run -dp 127.0.0.1:3000:3000 getting-started
An error message like this appears "docker: Error response from daemon: failed to set up container networking: driver failed programming external connectivity on endpoint crazy_goldberg (deafd797298a130d4ddebd23c81f8887fb67c5954ccdfda6ce87ccab04bf6ffe): Bind for 127.0.0.1:3000 failed: port is already allocated'

The error occurred because you aren't able to start the new container while your old container is still running. The reason is that the old container is already using the host's port 3000 and only one process on the machine (containers included) can listen to a specific port. To fix this, you need to remove the old container.

## Remove the old container

To remove a container, you first need to stop it. Once it has stopped, you can remove it. You can remove the old container using the CLI or Docker Desktop's graphical interface. Choose the option that you're most comfortable with.

Removing a container using the CLI do the following:
1. Get the ID of the container by using the "docker ps" command.
2. Use the "docker stop" command to stop the container. Replace <the-container-id> with the ID from docker ps.
docker stop <the-container-id>
3. Run the "docker ps" command to confirm if the container stopped
4. Once the container has stopped, you can remove it by using the docker rm command.
docker rm <the-container-id>

## Start the updated app container
1. start your updated app using the docker run command.
Using "docker run -dp 127.0.0.1:3000:3000 getting-started"
2. Refresh your localhost and see the newly updated line
#Note: Also notice that the container Id has changed too

## Part 3: Share the application

Now that you've built an image, you can share it. To share Docker images, you have to use a Docker registry. The default registry is Docker Hub and is where all of the images you've used have come from

To push an image, you first need to create a repository on Docker Hub.

1. Sign up or Sign in to Docker Hub.
2. Select the Create Repository button.
3. For the repository name, use "getting-started". Make sure the Visibility is Public.
4. Select Create.

## Pushing the image to the hub
1. Run this command "docker push docker/getting-started"
Notice an error message. This is because the image hasn't been tagged correctly. Your docker image and local image are tagged differently to confirm this Run "docker image ls"
2. Use the "docker tag" command to give the "getting-started" image a new name. Replace YOUR-USER-NAME with your Docker ID
  IN my case i used "docker tag getting-started ddataguru/getting-started"
3. Build and tag the image properly using "docker build -t ddataguru/getting-started:getting-started ."
4. Verify the image exist using "docker images"
5. Still in cli Login to Docker Hub using the command " docker login"
6. Now run the docker push command again using "docker push ddataguru/getting-started:getting-started"

## Persist the DB

1. Start an Alpine container and create a new file in it.
Run the command "docker run --rm alpine touch greeting.txt"
- docker run - command to run your docker
- --rm -
- alpine - The image name
- touch - command to create a file name
greeting.txt - the file name

2. Run a new Alpine container and use the stat command to check whether the file exists.
"docker run --rm alpine stat greeting.txt"
Notice an error message, this is because the "greeting.txt" file created by the first container did not exist in the second container.

# this is the dockerfile for the project
# this file is used to show docker how to build the image

FROM node:lts-alpine3.18
# Using the LTS version of node for stability and the alpine version of the image for a smaller image size
WORKDIR /app
# setting the working directory to /app **in the container**
COPY package*.json ./ 
# Copying the package.json file to the root of the container
RUN npm install
# Installing the dependencies from the package.json file
COPY . .
# Copying the rest of the files to the root of the container
ENTRYPOINT [ "npm", "run" ]
# Setting the entrypoint to npm run
CMD ["dev" ]
# Running the start script in the package.json file by default, but can be overwritten by passing a command to the docker run command

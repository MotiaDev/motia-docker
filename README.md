# Motia Docker

<p align="center">
<img src="./docs/assets/motia-docker.png" alt="motia docker">
</p>

This repository contains the Dockerfile and related files for building the motia-docker image, which can be used to run a Motia application in a Docker container. It provides all of the required dependencies to run a Motia application inside a Docker container.

## Usage

You will need to implement your own Dockerfile where you will use this image as a base image. Use the following template for your Dockerfile:

```dockerfile
# Specify platform to match your target architecture
FROM --platform=linux/amd64 motiadev/motia-docker:latest

# Install Dependencies
COPY package*.json ./
RUN npm ci --only=production

# Move application files
COPY . .

# Enable the following lines if you are using python steps!!!
# # Setup python steps dependencies
# RUN npx motia@latest install

# Expose outside access to the motia project
EXPOSE 3000

# Run your application
CMD ["npm", "run", "start"]
```

## Create a .dockerignore file

You can use the .dockerignore.sample file as a template for your .dockerignore file.

## Build the image

```bash
docker build -t motia-app .
```

## Run your Motia application

Once you've built your image, you can run it using the following command:

```bash
docker run -it --rm -p 3000:3000 motia-app
```

> Replace the port and the name of your image accordingly this is just an example

You are set to go! Your Motia application should now be running inside a Docker container, you can access it at `http://localhost:3000` (replace the port if you used a different one).


## Contributing

If you find any issues or have suggestions for improvements, please open an issue or submit a pull request. 

### Login to Docker using your credentials (you need to haver access to the motiadev org)

```bash
docker login
```

### Building the base image

To build the base image, run the following command:

```bash
docker build -t motiadev/motia-docker:<version, use the last commit sha> .
```

### Push the image to Docker Hub:

```bash
docker push motiadev/motia-docker:<version, use the last commit sha>
```

### Point to latest tag

```bash
docker tag motiadev/motia-docker:<version, use the last commit sha> motiadev/motia-docker:latest
docker push motiadev/motia-docker:latest
```

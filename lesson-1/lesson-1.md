# Lesson 1: Running a Node.js app in Docker

## Steps

1. Create a simple Node.js script `hello-docker.js`:
   ```js
   console.log("Hello Docker")
   ```

2. Create a `Dockerfile` to containerize the app:
   ```Dockerfile
   FROM node:alpine
   COPY . /lesson-1
   WORKDIR /lesson-1
   CMD ["node", "hello-docker.js"]
   ```

3. Build the Docker image:
   ```bash
   docker build -t hello-docker .
   ```

   After the build finishes, nothing is printed to confirm it worked, so list the images to check it was created:
   ```bash
   docker image ls
   ```

4. Run the container:
   ```bash
   docker run hello-docker
   ```

5. Verify the output `Hello Docker` is printed to the console.


# Lesson 3: Difference between image and container

### Image

A read-only template/blueprint. It bundles everything the app needs to run:

- A cut-down OS (e.g. `alpine`, `ubuntu`)
- Third-party libraries/dependencies
- Application files
- Environment variables

### Container

A running (or stopped) instance of an image — the same way an object is an instance of a class. Many containers can be created from the same image, each with its own writable layer, so changes made inside one container don't affect the image or any other container created from it.

### Creating the example React app

The `react-app` folder was scaffolded with Vite:

```bash
npm create vite@latest react-app -- --template react
cd react-app
npm install
```

The dev server is configured to run on port 3000 (via `server.port` in `vite.config.js`, instead of Vite's default 5173).

Run it locally to confirm it works before containerizing it:

```bash
npm run dev
```

### Containerizing the app

Created a `Dockerfile` in `react-app/`:

```dockerfile
FROM node:20.16.0-alpine3.20
RUN addgroup app && adduser -S -G app app
USER app
WORKDIR /app
COPY --chown=app:app . .
RUN npm install
ENV BACKEND_URL=http://localhost:8000
EXPOSE 3000
CMD ["npm", "run" ,"dev"]
```

- `addgroup`/`adduser` + `USER app` avoids running as `root` inside the container.
- `COPY --chown=app:app . .` copies the app files owned by that non-root user.
- `ENV BACKEND_URL=...` shows how to bake an environment variable into the image.
- `EXPOSE 3000` documents the port the app listens on (it doesn't publish it — see below).

Added a `.dockerignore` so `node_modules`, `.env` files, and git metadata aren't copied into the image:

```
node_modules/
.env
.env.*
.git/
Dockerfile
.dockerignore
```

Build the image:

```bash
docker build -t react-app .
```

### Running the container

First attempt, just to explore the image:

```bash
docker run -it react-app sh
```

This drops into a shell inside the container so you can poke around (`ls`, check `node_modules` was installed, etc.) without starting the dev server.

Running the app itself:

```bash
docker run -it react-app
```

This starts the container and runs the default `CMD` (`npm run dev`), but the dev server was unreachable from the host browser — `EXPOSE` only documents the port, it doesn't publish it. The fix is to map a host port to the container port with `-p`:

```bash
docker run -p 3000:3000 react-app
```

With the port published, `http://localhost:3000` on the host reaches the Vite dev server running inside the container. This only works because `vite.config.js` sets `server.host: true`, which makes Vite listen on `0.0.0.0` instead of just `localhost` — without that, the app is unreachable from outside the container even with the port published.

Useful inspection commands while iterating:

```bash
docker images        # list built images
docker ps             # list running containers
docker ps -a          # list all containers, including stopped ones
docker start -i <container-id>   # restart and attach to a stopped container
```

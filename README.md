# Flask App in Docker

A small Python Flask web app packaged as a Docker image and run as a container. The app returns "Hello from Dockerized Flask App!" on its home page.

## Files

| File | Purpose |
|---|---|
| `app.py` | Flask app with one route (`/`), listening on 0.0.0.0:5000 |
| `requirements.txt` | Exact versions of Flask and its dependencies |
| `Dockerfile` | Builds the image from `python:3.12-slim` |
| `.dockerignore` | Keeps the virtual environment and cache files out of the build |
| `.gitignore` | Keeps the virtual environment and cache files out of Git |

## Run it

~~~
docker build -t flask-hello .
docker run -d -p 127.0.0.1:5000:5000 --name flask-app flask-hello
~~~

Then open http://127.0.0.1:5000. To stop and remove the container:

~~~
docker stop flask-app
docker rm flask-app
~~~

## Design choices

- **Layer caching:** `requirements.txt` is copied and installed before `app.py`, so code changes rebuild in seconds without reinstalling packages.
- **Non-root user:** the app runs as `appuser` instead of root, limiting damage if it were compromised.
- **Localhost-only port:** publishing to `127.0.0.1` keeps the app reachable from this machine but not from other devices on the network.
- **Slim base image:** `python:3.12-slim` keeps the image small.
- **Small build context:** `.dockerignore` excludes the Windows virtual environment, which would be large and unusable inside a Linux container.

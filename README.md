# CST8915 Lab 4: Introduction to Docker

**Student Name**: Randa Omer

**Student ID**: 041079985

**Course**: CST8915 Full-stack Cloud-native Development

**Semester**: Fall 2026

---

## Demo Video (Part 6: Docker Compose for Multi-Container Applications)

🎥 [Watch Demo Video](https://youtu.be/QdAYm2X5Lqs)

---

## Reflection Questions

### 1. What are the main differences between a Docker image and a Docker container?

A Docker image is like a recipe or a template. It is read-only and contains everything the app needs to run, such as the code, Python, and the libraries. A Docker container is a running copy of that image. When I start a container, Docker takes the image and adds a small writable layer on top so the app can run and save changes. One image can be used to create many containers. For example, in this lab I used one image (`my-python-app:v3`) to start three containers (`app1`, `app2`, and `app3`).

### 2. Explain how Docker's layered architecture improves efficiency.

Each line in a Dockerfile creates a layer, and Docker saves (caches) these layers. When I build the image again, Docker reuses the layers that did not change and only rebuilds the ones that did. In my lab, the first build took 10.8 seconds, but after I changed only `app.py`, the new build took only 0.8 seconds because Docker reused the cached Python and Flask layers. Layers are also shared between images, which saves disk space. I had four images that each showed about 211MB, but `docker system df` showed only about 211MB in total, because they shared the same layers.

### 3. Why does each container get its own writable layer?

Each container gets its own writable layer so that containers stay separate and do not affect each other. The image layers are read-only and shared, so any change a container makes is saved only in its own writable layer. In my lab, I created `data1.txt` in `app1`, and it did not appear in `app2` or `app3`. When I changed `app.py` inside `app1`, Docker copied the file into `app1`'s writable layer first (this is called Copy-on-Write), so `app2` still had the original file. This keeps the image clean and lets many containers safely use the same image.

### 4. What are the benefits of using Docker Compose over running containers individually?

Docker Compose lets me describe my whole app in one file (`docker-compose.yml`) and start everything with one command. With `docker compose up -d`, Docker built my web app, downloaded Redis, created a network and a volume, and started both containers in the right order. Without Compose, I would need to run many long `docker` commands by hand. Compose also makes the containers talk to each other easily: my web app found Redis just by using the name `redis`. It also makes it easy to manage the app (stop, start, view logs, remove everything) and to scale it, for example running three web containers with `--scale web=3`. The volume also kept my data, so the visit counter was saved even after the containers were removed with `docker compose down`.

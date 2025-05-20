# Daniel Dirgantara – DockerHub Repositories

**Name:** Daniel Dirgantara
**NIM:** 2702255552

---

## 📦 Docker Repositories

### 🔧 C++ Development

* **Repository:** [dcc-cpp](https://hub.docker.com/repository/docker/leinnn/dcc-cpp/general)

---

### 🐍 Python / Django

* **Repository:** [dcc-python](https://hub.docker.com/repository/docker/leinnn/dcc-python/general)

---

### 🥀 Golang (Go)

* **Repository:** [dcc-go](https://hub.docker.com/repository/docker/leinnn/dcc-go)

---

### 🌐 JavaScript Frameworks

* **Repository:** [dcc-js](https://hub.docker.com/repository/docker/leinnn/dcc-js)

---

### 🐘 PHP / Laravel

* **Repository:** [dcc-php](https://hub.docker.com/repository/docker/leinnn/dcc-php/general)

---

## 🧰 How to Use

To pull any image:

```bash
docker pull leinnn/<image-name>
```

Example:

```bash
docker pull leinnn/dcc-python
```

Run the container:

```bash
docker run -it leinnn/dcc-python /bin/bash
```

Or for development:

```bash
docker run -v $(pwd):/app -p 8000:8000 leinnn/dcc-python
```

---

## 📝 Notes

* Each repository may contain sample `Dockerfile`, `README`, and usage instructions.
* These images are designed for academic and development use.
* For issues or suggestions, please contact me via GitHub or DockerHub.

---

Thank you for checking out my repositories!

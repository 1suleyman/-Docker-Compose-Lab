# 🐳 Docker Compose Lab

In this lab, I learned how to **run multi-container applications**, link containers manually, clean them up, and then rebuild the same architecture using **Docker Compose** with a `docker-compose.yml` file.

---

## 📋 Lab Overview

**Goal:**

* Run a Redis container
* Run a Click Counter app container linked to Redis
* Expose application ports
* Clean up containers
* Create a `docker-compose.yml` file
* Bring up the entire stack using `docker-compose up`

**Learning Outcomes:**

* Use `docker run` with `--link`, `-p`, and custom names
* Stop and remove containers
* Write a basic Compose file (version, services, image, ports)
* Use `docker-compose up -d` to run multiple containers together

---

## 🛠 Step-by-Step Journey

### **Step 1: Create a Redis Container**

**Task:**
Create a Redis container named **redis** using the image:

```
redis:alpine
```

**Command:**

```bash
docker run -d --name redis redis:alpine
```

✔️ Redis container created successfully.

---

### **Step 2: Create the Click Counter Application Container**

**Task:**
Create a container named **click-counter** using:

* Image: `kodekloud/click-counter`
* Link it to the Redis container
* Expose **host port 8085** → **container port 5000**

**Command:**

```bash
docker run -d \
  --name click-counter \
  --link redis:redis \
  -p 8085:5000 \
  kodekloud/click-counter
```

✔️ App connected to Redis
✔️ Accessible via host port **8085**

---

### **Step 3: Access the Application**

The Click Counter UI loads:
✔️ Showing count increments when clicked
✔️ Confirms Redis connection is working

---

### **Step 4: Clean Up Containers**

Before removing containers, stop them:

```bash
docker ps
docker stop <container_ids>
```

Then remove them:

```bash
docker rm <container_ids>
```

✔️ Both **redis** and **click-counter** removed successfully.

---

### **Step 5: Create `docker-compose.yml`**

**Location:**

```
/root/clickcounter/
```

**Command:**

```bash
vi docker-compose.yml
```

Entered **INSERT mode** and wrote:

```yaml
version: "3.8"

services:
  redis:
    image: redis:alpine

  clickcounter:
    image: kodekloud/click-counter
    ports:
      - "8085:5000"
```

Saved the file:

```
ESC :wq
```

---

### **Step 6: Run Application Using Docker Compose**

**Command:**

```bash
docker-compose up -d
```

✔️ Redis and Click Counter started automatically
✔️ Entire stack now runs from a single command
✔️ End of lab

---

## ✅ Key Commands Summary

| Task              | Command / Notes                                                                              |
| ----------------- | -------------------------------------------------------------------------------------------- |
| Run Redis         | `docker run -d --name redis redis:alpine`                                                    |
| Run Click Counter | `docker run -d --name click-counter --link redis:redis -p 8085:5000 kodekloud/click-counter` |
| View containers   | `docker ps`                                                                                  |
| Stop containers   | `docker stop <ids>`                                                                          |
| Remove containers | `docker rm <ids>`                                                                            |
| Open Compose file | `vi docker-compose.yml`                                                                      |
| Run stack         | `docker-compose up -d`                                                                       |

---

## 💡 Notes / Tips

* `--link` is legacy; Docker Compose or networks are preferred.
* Compose automatically handles networking between services.
* The `version` field ensures compatibility with Compose features.
* Using Compose reduces typing errors and improves reproducibility.
* `docker-compose up -d` starts *all* services defined in the file.

---

## 📌 Lab Summary

| Step                                  | Status | Key Takeaways                         |
| ------------------------------------- | ------ | ------------------------------------- |
| Create Redis container                | ✅      | Used `redis:alpine`                   |
| Create Click Counter with link + port | ✅      | Linked to Redis, exposed on 8085      |
| Test UI                               | ✅      | Counter increases, Redis working      |
| Clean up containers                   | ✅      | Stopped + removed                     |
| Build Compose file                    | ✅      | Defined redis + clickcounter services |
| Run with Docker Compose               | ✅      | Started full stack with one command   |

---

## ✅ References

* [Docker Compose Documentation](https://docs.docker.com/compose/)
* [Compose File Versioning](https://docs.docker.com/compose/compose-file/)
* [Docker Run Reference](https://docs.docker.com/engine/reference/run/)
* [Redis Docker Image](https://hub.docker.com/_/redis)

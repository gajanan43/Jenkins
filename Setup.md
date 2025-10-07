
# 🧰 Jenkins Setup on Docker (Windows)

## 🚀 Step 1: Pull Jenkins Image

Open PowerShell and pull the latest LTS (Long-Term Support) version of Jenkins:

```bash
docker pull jenkins/jenkins:lts
````

---

This directory will store all Jenkins data (jobs, plugins, users, etc.).

```bash
mkdir D:\Jenkins_Home
```

📁 Your Jenkins data will persist here even if you delete or recreate the container.

---

## ⚙️ Step 3: Run Jenkins Container

Now start Jenkins and mount your local directory as a Docker volume.

```bash
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v D:\Jenkins_Home:/var/jenkins_home jenkins/jenkins:lts
```


## 🧾 Step 4: Verify Jenkins is Running

List running containers:

```bash
docker ps
```

You should see something like:

```
CONTAINER ID   IMAGE                 STATUS         PORTS
abcd1234efgh   jenkins/jenkins:lts   Up 2 minutes   0.0.0.0:8080->8080/tcp
```

---

## 💾 Step 5: Check Volume Mount

To confirm your `D:\Jenkins_Home` is mounted correctly:

```bash
docker inspect jenkins
```

Look for a section like:

```json
"Mounts": [
  {
    "Type": "bind",
    "Source": "D:\\Jenkins_Home",
    "Destination": "/var/jenkins_home"
  }
]
```

✅ If it appears — your data is safely stored on your D: drive.

---

## 🌐 Step 6: Access Jenkins

Open your browser and visit:

```
http://localhost:8080
```

### 🔑 Get the Admin Password:

Run:

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Copy that password → paste it into the Jenkins setup page.

---

## 🧩 Step 7: Complete Setup Wizard

* Choose **“Install suggested plugins”**
* Create your **admin user**
* Jenkins is ready to use 🎉

---

## 🔍 Useful Docker Commands

| Action                   | Command                          |
| ------------------------ | -------------------------------- |
| Check running containers | `docker ps`                      |
| Stop Jenkins             | `docker stop jenkins`            |
| Start Jenkins            | `docker start jenkins`           |
| Restart Jenkins          | `docker restart jenkins`         |
| Remove Jenkins container | `docker rm jenkins`              |
| Remove Jenkins image     | `docker rmi jenkins/jenkins:lts` |
| Check volume list        | `docker volume ls`               |
| View logs                | `docker logs -f jenkins`         |

---

## ♻️ Optional: Auto-start Jenkins on System Reboot

If you want Jenkins to restart automatically when Docker restarts:

```bash
docker run -d --name jenkins --restart=always -p 8080:8080 -p 50000:50000 -v D:\Jenkins_Home:/var/jenkins_home jenkins/jenkins:lts
```

---

## 🧠 Notes

* All Jenkins configuration and build data is stored in:
  **`D:\Jenkins_Home`**
* You can safely delete and recreate the container — your data remains intact.
* No harm to your system: Jenkins runs fully **isolated inside Docker**.

---

## ✅ Verification Checklist

| Check                     | Command                  | Expected                            |
| ------------------------- | ------------------------ | ----------------------------------- |
| Jenkins container running | `docker ps`              | STATUS: Up                          |
| Jenkins accessible        | `http://localhost:8080`  | Login page opens                    |
| Volume mounted            | `docker inspect jenkins` | D:\Jenkins_Home → /var/jenkins_home |

---

## 🧹 Cleanup (If Needed)

To completely remove Jenkins and all data:

```bash
docker stop jenkins
docker rm jenkins
docker volume rm jenkins_home
rd /s /q D:\Jenkins_Home
```

---
---

Would you like me to include **Docker Compose** instructions too (so you can manage Jenkins with a `docker-compose.yml` file)? It makes restarting and reconfiguring easier.
```

# 🐳 Docker & Docker Compose – Cheat Sheet

## 1. **Docker Installation & Überprüfung**

### **Prüfen, ob Docker installiert ist**

```zsh
docker --version
```
Zeigt die installierte Docker-Version an.  
Wenn der Befehl nicht gefunden wird, ist Docker nicht installiert.

### **Docker Installation unter Ubuntu**

- Set up Docker's apt repository.  
```zsh
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
- Install the Docker packages.  
```zsh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
- check
```zsh
docker --version
```

- Verify that the installation is successful by running the hello-world image:  
```zsh
sudo docker run hello-world
```
***

## 2. **Docker – Wichtige Befehle**

| Aktion                                  | Befehl |
|----------------------------------------|--------|
| Container starten                       | `docker run -d --name  ` |
| Laufende Container anzeigen             | `docker ps` |
| Alle Container anzeigen                 | `docker ps -a` |
| Container stoppen                       | `docker stop ` |
| Container löschen                       | `docker rm ` |
| Images anzeigen                         | `docker images` |
| Image löschen                           | `docker rmi ` |
| Image herunterladen                     | `docker pull ` |
| Image bauen                             | `docker build -t  .` |
| Shell in Container öffnen *(zsh/bash)*  | `docker exec -it  zsh` |
| Container-Logs anzeigen                 | `docker logs ` |
| Volumes anzeigen                        | `docker volume ls` |
| Volume erstellen                        | `docker volume create ` |
| Volume in Container einbinden           | `docker run -v :/pfad ` |

***

## 3. **Docker Compose – Wichtige Befehle**

| Aktion                                   | Befehl |
|------------------------------------------|--------|
| Services starten                         | `docker-compose up -d` |
| Services stoppen                         | `docker-compose down` |
| Einen Service neu starten                | `docker-compose restart ` |
| Logs eines Service                       | `docker-compose logs ` |
| Status anzeigen                          | `docker-compose ps` |
| Image- und Container-Build               | `docker-compose build` |
| Service skalieren                        | `docker-compose up -d --scale =3` |

***

## 4. **Docker Compose – Template**

unter servises werden die einzelnen Container angegeben.

```yaml
services:
  nginx:
    image: nginx:latest
    container_name: web-nginx
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro
    environment:
      - TZ=Europe/Berlin
    restart: unless-stopped

  db:
    image: postgres:14
    container_name: postgres-db
    environment:
      POSTGRES_DB: exampledb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: strongpassword
    volumes:
      - db_data:/var/lib/postgresql/data
    restart: unless-stopped

  watchtower:
    image: containrrr/watchtower
    container_name: watchtower
    volumes:
    /var/run/docker.sock:/var/run/docker.sock \


volumes:
  db_data:
```

***

## 5. **Nützliche ZSH-Aliase für Docker**

In deine `~/.zshrc` einfügen:

```bash
alias dps="docker ps -a"
alias dcu="docker-compose up -d"
alias dcd="docker-compose down"
alias dcl="docker-compose logs -f"
alias drm="docker rm $(docker ps -aq)"
alias drmi="docker rmi $(docker images -q)"
```

***

## 6. **Tipps**

- `docker container prune` entfernt alle gestoppten Container.
- Mit **Tab-Autovervollständigung** in `zsh` blitzschnell Container- oder Image-Namen auswählen.
- Bei Fehlern: Immer zuerst `docker-compose logs ` prüfen.

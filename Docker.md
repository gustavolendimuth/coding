# Docker

## Start docker on boot WSL2

#### Adicionar na configuração do boot da distro

```bash
sudo nano /etc/wsl.conf

# Adicionar as linhas abaixo
[boot]
systemd=true
```

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

To disable this behavior, use `disable` instead.

```bash
sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```


## Docker Compose

```yaml
version: '3.9'
services:
  frontend:
    build: ./frontend
    ports:
      - 3000:3000
    working_dir: /app-frontend
    depends_on:
      backend:
        condition: service_healthy
    # Os `healthcheck` devem garantir que a aplicação
    # está operacional, antes de liberar o container
    healthcheck:
      test: ["CMD", "lsof", "-t", "-i:3000"]  # Caso utilize outra porta interna para o front, altere ela aqui também
      timeout: 10s
      retries: 5
  backend:
    container_name: app_backend
    build: ./backend
    ports:
      - 3001:3001
    working_dir: /app-backend
    command: dev
    volumes:
      - ./backend/src:/app-backend/src
    depends_on:
      db:
        condition: service_healthy
    environment:
      - APP_PORT=3001
      - JWT_SECRET=jwt_secret
      # Os dados abaixo se referem ao container `db`
      # Dica: Relembre aqui da comunicação interna entre containers
      - DB_USER=root
      - DB_PASS=123456
      - DB_HOST=db
      - DB_PORT=3306
    healthcheck:
      test: ["CMD", "lsof", "-t", "-i:3001"] # Caso utilize outra porta interna para o back, altere ela aqui também
      timeout: 10s
      retries: 5
  db:
    image: mysql:8.0.21
    container_name: db
    platform: linux/x86_64
    ports:
      - 3002:3306
    environment:
      - MYSQL_ROOT_PASSWORD=123456
    restart: 'always'
    healthcheck:
      test: ["CMD", "mysqladmin" ,"ping", "-h", "localhost"] # Deve aguardar o banco ficar operacional
      timeout: 10s
      retries: 5
    cap_add:
      - SYS_NICE # Deve omitir alertas menores
```


## Dockerfile

```dockerfile
FROM node:16.14-alpine
WORKDIR /app-backend
COPY package.json .
RUN npm install
COPY . .
ENTRYPOINT [ "npm", "run", "dev" ]
```


## Scripts para usar com docker

```json
  "scripts": {
    "postinstall": "npm run install:apps",
    "install:apps": "./apps_install.sh",
    "pretest": "(cd ./app/backend && /bin/sh tsc_eval.sh)",
    "test": "env $(cat ./app/backend/.env) jest -i --forceExit --verbose",
    "test:browser": "SHOW_BROWSER=true npm test",
    "test:debug": "DEBUG=true npm test",
    "compose:up": "cd app && docker-compose up -d --build",
    "compose:down": "cd app && docker-compose down --remove-orphans",
    "compose:up:dev": "cd app && docker-compose -f docker-compose.dev.yml up -d",
    "compose:down:dev": "cd app && docker-compose -f docker-compose.dev.yml down --remove-orphans",
    "logs": "cd app && docker-compose logs -f"
  }
```


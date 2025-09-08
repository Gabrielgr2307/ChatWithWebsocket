________________________________________
Chat con NestJS y React Native (Dockerizado)
Este es un proyecto de chat en tiempo real que utiliza Websockets con NestJS en el backend y React Native en el frontend. Toda la aplicación está orquestada con Docker Compose para un entorno de desarrollo consistente.
Estructura del Proyecto
El proyecto se divide en tres servicios principales, cada uno con su propio contenedor de Docker:
•	WebsoketInNestjs/: El backend de NestJS con el servidor de Websockets.
•	ChatExample/: El frontend de React Native.
•	mysql/: El servicio de base de datos MySQL.
________________________________________
Requisitos Previos
Antes de comenzar, asegúrate de tener instalados los siguientes programas:
•	Docker Desktop: Incluye Docker Engine y Docker Compose.
•	Git: Para clonar el repositorio.
________________________________________
Primeros Pasos
1.	Clona el repositorio:
Bash
git clone https://github.com/tu-usuario/tu-repositorio.git
cd tu-repositorio/ChatWithWebsocket
2.	Configura las dependencias locales:
Para que los contenedores puedan construir las imágenes, necesitas generar los archivos de bloqueo de dependencias en cada proyecto.
Bash
# Para el backend
cd WebsoketInNestjs
yarn install

# Para el frontend
cd ../ChatExample
yarn install

# Vuelve a la raíz del proyecto
cd ..
________________________________________
Configuración de Docker
Archivo docker-compose.yml
Este archivo centraliza la configuración de todos los servicios.
YAML
version: "3.8"
services:
  mysql:
    image: mysql:8.0
    container_name: mysql_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: db_crud
      MYSQL_USER: user_crud
      MYSQL_PASSWORD: root
    volumes:
      - ./mysql:/var/lib/mysql
    ports:
      - "3307:3306"

  back-nest:
    build:
      context: ./WebsoketInNestjs
      dockerfile: Dockerfile
    container_name: nest_serve
    restart: always
    ports:
      - "3000:3000"
    volumes:
      - ./WebsoketInNestjs:/app
      - /app/node_modules
    depends_on:
      - mysql

  front-react-native:
    build:
      context: ./ChatExample
      dockerfile: Dockerfile
    container_name: react_frontend
    restart: always
    ports:
      - "19006:19006"
      - "19000:19000"
      - "8081:8081"
    volumes:
      - ./ChatExample:/app
      - /app/node_modules
Archivos .dockerignore
Estos archivos evitan que Docker copie archivos innecesarios o que causen errores en la construcción.
WebsoketInNestjs/.dockerignore
# Directorios de node_modules
node_modules/

# Archivos de la base de datos local
mysql/

# Archivos de logs y temporales
*.log
tmp/

# Archivos de caché
dist/

# Archivos de git
.env
.git
.gitignore
ChatExample/.dockerignore
# Directorios de node_modules
node_modules/

# Directorios de caché y de expo
.expo/
.yarn/cache/

# Archivos de logs y temporales
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Archivos de git
.git/
.gitignore
Archivos Dockerfile
WebsoketInNestjs/Dockerfile
Dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile
COPY . .
EXPOSE 3000
CMD ["yarn", "run", "start:dev"]
ChatExample/Dockerfile
Dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile
COPY . .
EXPOSE 19006
EXPOSE 19000
EXPOSE 8081
CMD ["npx", "expo", "start", "--web"]
________________________________________
Levantando los Servicios
Para levantar la aplicación completa, usa el siguiente comando desde la raíz del proyecto (ChatWithWebsocket/):
Bash
docker-compose up --build
Este comando:
•	Descarga la imagen de MySQL.
•	Construye las imágenes de Docker para el backend y el frontend.
•	Inicia los tres contenedores y los conecta en una red de Docker.
Nota: La primera vez que ejecutes este comando, la instalación de dependencias puede tardar unos minutos. Si encuentras errores de caché, puedes usar el comando docker-compose up --build --no-cache para forzar una reconstrucción limpia.
________________________________________
Configuración de Conexión
Es crucial que la comunicación entre los contenedores se haga con los nombres de los servicios definidos en docker-compose.yml.
Backend (NestJS)
La configuración de la base de datos debe usar el nombre del servicio de la base de datos (mysql) como host y el puerto interno (3306), no el puerto externo 3307.
TypeScript
// WebsoketInNestjs/src/app.module.ts
TypeOrmModule.forRoot({
  type: 'mysql',
  host: 'mysql',   // Nombre del servicio de Docker
  port: 3306,     // Puerto interno del contenedor de MySQL
  username: 'user_crud',
  password: 'root',
  database: 'db_crud',
  autoLoadEntities: true,
  synchronize: true,
}),
Frontend (React Native)
La conexión del frontend al backend también debe usar el nombre del servicio.
TypeScript
// ChatExample/src/services/socketService.ts (o similar)
// Cambia la URL
await this.socket.connect('http://back-nest:3000');
________________________________________
Desarrollo y Comandos Útiles
•	Ver el estado de los contenedores:
Bash
docker-compose ps
•	Ver los logs de un servicio:
Bash
docker-compose logs -f back-nest
•	Reiniciar un servicio:
Bash
docker-compose restart back-nest
•	Detener todos los servicios:
Bash
docker-compose down


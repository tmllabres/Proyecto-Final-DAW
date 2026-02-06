# Proyecto-Final-DAW

Aplicación web para usuarios nuevos del gimnasio.

## Requisitos previos

Necesitas tener instalado esto antes de empezar:

- **Node.js v22.19+** → https://nodejs.org (descarga la versión LTS)
- **npm v10.9+** → no hace falta instalarlo, viene incluido con Node.js
- **Git v2.51+** → https://git-scm.com
- **Docker Desktop** (opción A) → https://www.docker.com/products/docker-desktop
- **PostgreSQL 15** (opción B) → https://www.postgresql.org/download

Para comprobar que lo tienes todo, abre una terminal y ejecuta:
```bash
node --version
npm --version
git --version
```

Si te salen números de versión, está instalado. Si da error, instálalo desde los enlaces de arriba.

## 1. Clonar el repositorio

Esto descarga una copia del proyecto en tu ordenador:
```bash
git clone https://github.com/tmllabres/Proyecto-Final-DAW.git
```

Entra a la carpeta del proyecto:
```bash
cd Proyecto-Final-DAW
```

## 2. Configurar la base de datos

Elige UNA de las dos opciones:

### Opción A — Con Docker (recomendado)

Docker levanta la base de datos automáticamente en un contenedor, sin instalar nada más. Ejecuta desde la raíz del proyecto:
```bash
docker compose up -d
```

El `-d` hace que corra en segundo plano para que puedas seguir usando la terminal.

Para comprobar que la base de datos está corriendo:
```bash
docker compose ps
```

Debe aparecer el servicio `db` con estado `running`.

Para parar la base de datos cuando termines de trabajar:
```bash
docker compose down
```

### Opción B — Sin Docker (PostgreSQL local)

Si no puedes usar Docker, instala PostgreSQL directamente:

1. Descarga PostgreSQL 15 desde https://www.postgresql.org/download
2. Ejecuta el instalador. Cuando te pida contraseña para el superusuario, pon: `gympass`
3. El puerto déjalo en `5432` (el que viene por defecto)
4. Termina la instalación dándole siguiente a todo
5. Abre **SQL Shell (psql)** desde el menú de inicio de Windows
6. Te va a pedir Server, Database, Port, Username → dale **Enter** a todos (usa los valores por defecto)
7. Cuando pida contraseña → escribe `gympass`
8. Ya dentro, ejecuta estos dos comandos para crear el usuario y la base de datos del proyecto:
```sql
CREATE USER gymuser WITH PASSWORD 'gympass';
CREATE DATABASE gymapp OWNER gymuser;
```

9. Para salir de psql:
```sql
\q
```

## 3. Configurar el backend

Entra a la carpeta del servidor:
```bash
cd server
```

Copia el archivo de ejemplo de variables de entorno para crear tu archivo local. Este archivo tiene las contraseñas y configuración que el servidor necesita para conectarse a la base de datos:
```bash
cp .env.example .env
```

Si estás en PowerShell y `cp` no funciona, usa:
```powershell
copy .env.example .env
```

El `.env` ya viene con los valores por defecto. Si seguiste los pasos de arriba tal cual, no hace falta cambiar nada.

Instala las dependencias del proyecto. Esto lee el `package.json` y descarga todas las librerías que usa el backend:
```bash
npm install
```

Arranca el servidor en modo desarrollo. Se reinicia solo cada vez que guardes un cambio:
```bash
npm run dev
```

Para comprobar que funciona, abre en el navegador: http://localhost:3000/api/health

Debe mostrar: `{"status":"ok"}`

## 4. Configurar el frontend

Abre **otra terminal** (no cierres la del backend) y entra a la carpeta del frontend:
```bash
cd client
```

Igual que en el backend, copia el archivo de variables de entorno:
```bash
cp .env.example .env
```

O en PowerShell:
```powershell
copy .env.example .env
```

Instala las dependencias:
```bash
npm install
```

Arranca el frontend en modo desarrollo:
```bash
npm run dev
```

Abre en el navegador: http://localhost:5173

## 5. Orden para arrancar cada día

Cada vez que te sientes a trabajar, arranca las tres cosas en este orden:

1. **Base de datos** → `docker compose up -d` desde la raíz (o asegúrate de que PostgreSQL está corriendo si lo instalaste local)
2. **Backend** → abre una terminal, `cd server`, `npm run dev`
3. **Frontend** → abre otra terminal, `cd client`, `npm run dev`

## 6. Si alguien ha subido cambios nuevos

Cuando alguien haga merge de un PR a main, antes de seguir trabajando tú, actualiza tu código:
```bash
git checkout main
git pull origin main
```

Si los cambios tocaron dependencias (modificaron `package.json`), reinstálalas:
```bash
cd client
npm install
cd ../server
npm install
```

## Reglas del equipo

- **No hacer push directo a main.** Todo cambio va por Pull Request.
- Cada PR necesita al menos **1 aprobación** de un compañero antes de hacer merge.
- Cada tarea se trabaja en su **propia rama**.

### Flujo de trabajo para cada ticket

Antes de empezar, asegúrate de estar en main y actualizado:
```bash
git checkout main
git pull origin main
```

Crea una rama nueva para tu ticket:
```bash
git checkout -b feature/nombre-del-ticket
```

Trabaja en tu tarea. Cuando termines, guarda los cambios:
```bash
git add .
git commit -m "feat: descripción de lo que hiciste"
git push origin feature/nombre-del-ticket
```

Después ve a GitHub, abre un Pull Request, espera a que un compañero lo revise y apruebe, y haz merge.
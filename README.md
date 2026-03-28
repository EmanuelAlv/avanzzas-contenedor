# Avanzzas Contenedor - Entorno local de Moodle

Este repositorio contiene la estructura y los plugins personalizados del proyecto, junto con una guía para levantar un entorno local de Moodle usando `moodle-docker`.

> Nota: este repositorio **no incluye** el repositorio completo de `moodle-docker` ni el core de Moodle como parte del versionado principal.  
> Cada integrante del equipo debe clonar esas dependencias localmente para recrear el mismo ambiente.

---

## Requisitos previos

Antes de comenzar, asegúrate de tener instalado:

- Git
- Docker
- Docker Compose (`docker-compose`)
- Linux, macOS o Windows con WSL2 recomendado

Verifica con:

```bash
git --version
docker --version
docker-compose --version
```

---

## Estructura esperada del proyecto

```bash
avanzzas-contenedor/
├── README.md
├── plugins/
│   └── local/
│       └── mi_plugin
```

---

## Clonar este repositorio

```bash
git clone <URL_DE_TU_REPOSITORIO>
cd avanzzas-contenedor
```

---

## Levantar el ambiente local de Moodle

### 1. Clonar `moodle-docker`

```bash
git clone https://github.com/moodlehq/moodle-docker.git
cd moodle-docker
```

---

### 2. Descargar el core de Moodle

```bash
mkdir -p server
git clone -b MOODLE_405_STABLE git://git.moodle.org/moodle.git server/moodle
```

---

### 3. Crear el archivo de configuración

```bash
cp config.docker-template.php server/moodle/config.php
```

---

### 4. Copiar plugins personalizados

```bash
cp -r ../avanzzas-contenedor/plugins/local/* server/moodle/local/
```

---

### 5. Definir variables de entorno

```bash
export MOODLE_DOCKER_WWWROOT=$PWD/server/moodle
export MOODLE_DOCKER_DB=pgsql
```

---

### 6. Levantar contenedores

```bash
bin/moodle-docker-compose up -d
```

---

### 7. Esperar base de datos

```bash
bin/moodle-docker-wait-for-db
```

---

### 8. Instalar Moodle (solo primera vez)

```bash
bin/moodle-docker-compose exec webserver php admin/cli/install_database.php \
--agree-license \
--fullname="Moodle Local" \
--shortname="moodle_local" \
--summary="Sitio local de pruebas" \
--adminpass="Admin123*" \
--adminemail="admin@example.com"
```

---

## Acceso

http://localhost:8000

Usuario: admin  
Password: Admin123*

---

## Uso diario

```bash
cd moodle-docker
export MOODLE_DOCKER_WWWROOT=$PWD/server/moodle
export MOODLE_DOCKER_DB=pgsql
bin/moodle-docker-compose up -d
```

---

## Apagar

```bash
bin/moodle-docker-compose down
```

---

## Reiniciar

```bash
bin/moodle-docker-compose down
bin/moodle-docker-compose up -d
```

---

## Borrar todo

```bash
bin/moodle-docker-compose down -v
```

---

## Plugins

Ruta:

```bash
moodle-docker/server/moodle/local/
```

---

## Errores comunes

### Variable no definida

```bash
export MOODLE_DOCKER_WWWROOT=$PWD/server/moodle
```

---

### Carpeta incorrecta

Ejecutar siempre dentro de:

```bash
moodle-docker/
```

---

## Recomendación

- No subir `moodle-docker` al repo
- No subir core de Moodle
- Versionar solo plugins

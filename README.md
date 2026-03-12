# Ejercicio-4 — API REST con Go y Docker

API REST en Go para gestionar equipos de la Liga Nacional de Guatemala. Los datos se almacenan en un archivo JSON local.

## Tecnologías

- Go 1.22
- Docker / Docker Compose

## Estructura del proyecto

```
Ejercicio-4/
├── main.go                    # Servidor HTTP y lógica de la API
├── Dockerfile                 # Imagen de Go
├── docker-compose.yml         # Configuración de contenedores
├── docker-compose.yml.example # Ejemplo de configuración
└── data/
    └── liga-nacional.json     # Base de datos JSON
```

## Modelo de datos

Cada equipo tiene la siguiente estructura:

```json
{
  "id": 1,
  "name": "Comunicaciones",
  "deporte": "Futbol",
  "liga": "Liga Nacional de Guatemala",
  "sede": "Ciudad de Guatemala",
  "year": 1949,
  "titulos": 32
}
```

## Endpoints

| Método   | Ruta              | Descripción                          |
|----------|-------------------|--------------------------------------|
| GET      | `/api/items`      | Listar todos los equipos             |
| GET      | `/api/items?id=1` | Buscar equipo por ID (query param)   |
| GET      | `/api/items/{id}` | Obtener equipo por ID (path param)   |
| POST     | `/api/items`      | Crear un nuevo equipo                |
| PUT      | `/api/items/{id}` | Reemplazar un equipo completo        |
| PATCH    | `/api/items/{id}` | Actualizar `name` y/o `titulos`      |
| DELETE   | `/api/items/{id}` | Eliminar un equipo                   |

## Ejecutar con Docker

```bash
# Copiar el archivo de ejemplo
cp docker-compose.yml.example docker-compose.yml

# Construir y levantar el contenedor
docker compose up --build
```

El servidor quedará disponible en `http://localhost:8080`.

## Ejecutar sin Docker

```bash
go run main.go
```

El servidor corre en el puerto `3012` → `http://localhost:3012`.

## Ejemplos de uso

```bash
# Listar todos los equipos
curl http://localhost:8080/api/items

# Obtener equipo con ID 1
curl http://localhost:8080/api/items/1

# Crear un equipo
curl -X POST http://localhost:8080/api/items \
  -H "Content-Type: application/json" \
  -d '{"id":10,"name":"Xelajú MC","deporte":"Futbol","liga":"Liga Nacional de Guatemala","sede":"Quetzaltenango","year":1928,"titulos":15}'

# Actualizar títulos de un equipo
curl -X PATCH http://localhost:8080/api/items/1 \
  -H "Content-Type: application/json" \
  -d '{"titulos": 33}'

# Eliminar un equipo
curl -X DELETE http://localhost:8080/api/items/1
```

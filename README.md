# Ejercicio-4

API REST desarrollada en Go para gestionar equipos de la Liga Nacional de Guatemala. El proyecto implementa operaciones CRUD sobre un archivo JSON local y puede ejecutarse tanto de forma nativa como con Docker.

## Resumen

- Lenguaje: Go 1.22
- Persistencia: archivo JSON local
- Contenedorización: Docker y Docker Compose
- Puerto local: `3012`
- Puerto con Docker: `8080`

## Qué hace este proyecto

- Lista todos los equipos registrados.
- Busca equipos por ID.
- Crea nuevos registros.
- Reemplaza un equipo completo con `PUT`.
- Actualiza campos puntuales con `PATCH`.
- Elimina equipos existentes.
- Devuelve respuestas JSON legibles e indentadas.

## Estructura del proyecto

```text
Ejercicio-4/
├── data/
│   └── liga-nacional.json     # Datos de la Liga Nacional
├── docs/
│   └── Ejercicio 4.pdf        # Enunciado en PDF
├── .gitignore                 # Archivos que no deben versionarse
├── Dockerfile                 # Imagen para levantar la API
├── Ejercicio 4.docx           # Enunciado editable
├── docker-compose.yml.example # Ejemplo de despliegue con Compose
├── main.go                    # Servidor HTTP y lógica CRUD
└── README.md                  # Documentación principal
```

## Modelo de datos

Cada equipo usa la siguiente estructura:

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

## Endpoints disponibles

| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/api/items` | Lista todos los equipos |
| GET | `/api/items?id=1` | Busca un equipo por ID usando query param |
| GET | `/api/items/{id}` | Obtiene un equipo por ID usando path param |
| POST | `/api/items` | Crea un nuevo equipo |
| PUT | `/api/items/{id}` | Reemplaza completamente un equipo |
| PATCH | `/api/items/{id}` | Actualiza `name` y/o `titulos` |
| DELETE | `/api/items/{id}` | Elimina un equipo |

## Cómo ejecutarlo

### Opción 1: con Docker

Si usas el archivo de ejemplo:

```bash
cp docker-compose.yml.example docker-compose.yml
docker compose up --build
```

En PowerShell:

```powershell
Copy-Item docker-compose.yml.example docker-compose.yml
docker compose up --build
```

La API quedará disponible en `http://localhost:8080`.

### Opción 2: sin Docker

```bash
go run main.go
```

La API quedará disponible en `http://localhost:3012`.

## Ejemplos de uso

```bash
# Listar todos los equipos
curl http://localhost:8080/api/items

# Obtener equipo con ID 1
curl http://localhost:8080/api/items/1

# Crear un equipo
curl -X POST http://localhost:8080/api/items \
  -H "Content-Type: application/json" \
  -d '{"id":11,"name":"Nuevo Equipo","deporte":"Futbol","liga":"Liga Nacional de Guatemala","sede":"Guatemala","year":2024,"titulos":0}'

# Actualizar solo los títulos
curl -X PATCH http://localhost:8080/api/items/1 \
  -H "Content-Type: application/json" \
  -d '{"titulos":33}'

# Eliminar un equipo
curl -X DELETE http://localhost:8080/api/items/1
```

## Ejemplo de respuesta

```json
[
  {
    "id": 1,
    "name": "Comunicaciones",
    "deporte": "Futbol",
    "liga": "Liga Nacional de Guatemala",
    "sede": "Ciudad de Guatemala",
    "year": 1949,
    "titulos": 32
  }
]
```

## Notas

- Los datos se guardan directamente en `data/liga-nacional.json`.
- El proyecto no usa base de datos relacional; la persistencia es local.
- El archivo `Ejercicio 4.docx` sigue en la raíz porque estaba bloqueado por otra aplicación al momento de reorganizar el repo.

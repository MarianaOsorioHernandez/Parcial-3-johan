# Parcial 3 – Sistema de Microservicios en Go con Docker y MongoDB

Este proyecto implementa una arquitectura basada en microservicios independientes,
donde cada operación principal del CRUD (Create, Read, Update, Delete) se desarrolla
como un servicio autónomo escrito en Golang. El almacenamiento se maneja mediante
MongoDB, y toda la solución se ejecuta en contenedores utilizando Docker, administrados
a través de Docker Compose.

El propósito del parcial es demostrar:
- Arquitectura modular basada en microservicios
- Manejo de Go junto al driver oficial mongo-go-driver
- Contenedorización con Docker
- Orquestación con Docker Compose
- Pruebas unitarias por servicio
- Buenas prácticas de desarrollo y estructura
- Generación de backups de la base de datos
- Validación mediante Postman
- Preparación para flujos CI/CD en GitHub Actions

Estructura del Proyecto:

crud-clients/
- create/ (microservicio encargado de crear registros)
- read/ (microservicio dedicado a consultas)
- update/ (microservicio para actualizaciones)
- delete/ (microservicio que elimina registros)
- backup/ (carpeta que almacena respaldos)
- postman/ (colección de pruebas para Postman)
- docker-compose.yml
- .env
- README.md

Cada microservicio contiene:
main.go, controller.go, service.go, repository.go, model.go, Dockerfile, go.mod y go.sum.

Arquitectura General:

El flujo interno de los servicios sigue el orden:
Cliente → Controller → Service → Repository → MongoDB

Esquema de Docker Compose:
- create (puerto 8001)
- read (puerto 8002)
- update (puerto 8003)
- delete (puerto 8004)
- mongo (puerto 27017)

Requisitos:
- Docker
- Docker Compose
- Archivo .env con las variables:
  MONGO_USER=root
  MONGO_PASS=secret123
  MONGO_DB=usersdb
  MONGO_COLLECTION=users

Ejecución del Proyecto:
Para iniciar los servicios: docker compose up --build
Para detenerlos: docker compose down

Endpoints:

Crear Usuario (CREATE):
POST http://localhost:8001/users
Body:
{
  "name": "Mariana",
  "email": "mariana@example.com",
  "phone": "3109987766"
}

Obtener Usuarios (READ):
GET http://localhost:8002/users

Obtener usuario por ID:
GET http://localhost:8002/users/{id}

Actualizar Usuario (UPDATE):
PUT http://localhost:8003/users/{id}
Body:
{
  "name": "Nombre Actualizado",
  "email": "actualizado@example.com",
  "phone": "3015556677"
}

Eliminar Usuario (DELETE):
DELETE http://localhost:8004/users/{id}

Pruebas Unitarias:
Para ejecutar las pruebas: go test ./...
Para ver cobertura: go test ./... -cover
Ejemplo: ok read 0.294s coverage: 89.4% of statements

Backup y Restauración de MongoDB:

Ingresar al contenedor:
docker exec -it mongo-service bash

Generar backup:
mongodump -u "root" -p "secret123" --authenticationDatabase "admin" --db "usersdb" --out "/backup"

Comprimir:
tar -czvf /backup-YYYYMMDD-HHMM.tar.gz /backup

Salir:
exit

Copiar backup al equipo:
docker cp mongo-service:/backup-20251120-1800.tar.gz ./backup/

Colección de Postman:
Se encuentra en: /postman/users-crud.postman_collection.json

Pasos:
Abrir Postman, importar la colección, ejecutar las peticiones.

Preparación para CI/CD:
El proyecto está listo para incluir:
- Workflows de GitHub Actions
- Ejecución automática de pruebas
- Build y publicación de imágenes Docker
- Escaneo de vulnerabilidades con Trivy
- Releases automáticos

(El archivo ci.yml está pendiente según el flujo que se requiera)

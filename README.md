# Backend Despacho - Spring Boot API REST

Microservicio de despachos del sistema. Desarrollado con Spring Boot 3.4.4, Java 17 y MySQL.

## Tecnologias
- **Spring Boot** 3.4.4 + **Java** 17
- **Maven** (gestion de dependencias)
- **MySQL** 8.0 (base de datos produccion)
- **H2** (base de datos tests)
- **Springdoc OpenAPI** (documentacion Swagger)
- **Docker** (contenerizacion multietapa)

## Estructura del Proyecto
```
backend_despacho_springboot-deploy/
├── Springboot-API-REST-DESPACHO/
│   ├── src/
│   │   ├── main/java/          # Codigo fuente
│   │   ├── main/resources/     # application.properties
│   │   └── test/               # Tests con H2
│   ├── pom.xml                 # Configuracion Maven
│   ├── Dockerfile              # Build multietapa
│   └── Docker-compose.yml      # Orquestacion local (MySQL + App)
├── .github/workflows/          # Pipeline CI/CD
└── .env.example                # Variables de entorno de ejemplo
```

## Ejecutar Localmente
```bash
cd Springboot-API-REST-DESPACHO
mvn spring-boot:run
```

## Ejecutar con Docker Compose
```bash
cd Springboot-API-REST-DESPACHO
cp ../.env.example ../.env
docker compose up -d --build
```
- API: http://localhost:8081
- Swagger: http://localhost:8081/swagger-ui.html

## Pipeline CI/CD (GitHub Actions)
El workflow `.github/workflows/deploy-despacho.yml` ejecuta 3 etapas:
1. **Build & Test**: Compila con Maven y ejecuta tests (`mvn clean verify`)
2. **Push**: Construye imagen Docker multietapa y publica en Docker Hub
3. **Deploy**: Conecta via SSH (proxy jump) a EC2 y despliega con `docker compose`

## Imagen Docker
- **Registro**: Docker Hub
- **Imagen**: `<DOCKERHUB_USERNAME>/despachos-backend:latest`
- **Estrategia**: Multietapa (Maven Amazon Corretto -> Amazon Corretto Al2023)
- **Seguridad**: Usuario no-root (`backdespacho`), volumen de logs persistente

## Variables de Entorno
| Variable | Descripcion |
|----------|-------------|
| `DB_ENDPOINT` | Host de la base de datos |
| `DB_PORT` | Puerto de la base de datos |
| `DB_NAME` | Nombre de la base de datos |
| `DB_USERNAME` | Usuario de BD |
| `DB_PASSWORD` | Password de BD |


За потребите на една компанија за нарачка на храна, потребно е да се развие REST API и UI апликација кои ќе овозможуваат
управување со нарачките за храна.

## Документација

1. Јавен Git репозиториум
   Апликацијата е поставена на јавен GitHub репозиториум:

https://github.com/AnastasijaTashkova/KIII-project-Food_Delivery

Структурата на репозиториумот е:

food-delivery-solution/
├── docker-compose.yml              # Оркестрација на сите сервиси
├── food-delivery-backend/          # Spring Boot REST API
│   ├── Dockerfile
│   └── src/...
└── food-delivery-frontend/         # React + Vite UI
├── Dockerfile
└── src/...
За клонирање на проектот:

git clone https://github.com/AnastasijaTashkova/KIII-project-Food_Delivery.git
cd KIII-project-Food_Delivery
2. Докеризација
   Апликацијата е докеризирана со три сервиси дефинирани во docker-compose.yml:

Сервис	Слика / Build	Порта
postgres	postgres:16	5432
backend	Dockerfile (Spring Boot)	8080
frontend	Dockerfile (Nginx)	3000 → 80
Backend Dockerfile
food-delivery-backend/Dockerfile користи multi-stage build:

Build stage — eclipse-temurin:21-jdk: компилира и пакува JAR со Maven (mvnw clean package -DskipTests)
Runtime stage — eclipse-temurin:21-jre: ја извршува апликацијата со java -jar app.jar
Frontend Dockerfile
food-delivery-frontend/Dockerfile исто така користи multi-stage build:

Build stage — node:20-alpine: инсталира зависности и го гради React проектот (npm run build)
Runtime stage — nginx:alpine: ги сервира статичките фајлови од /app/dist преку Nginx на порта 80
Покренување на апликацијата
docker compose up --build
По успешно стартување:

Frontend: http://localhost:3000
Backend API: http://localhost:8080
PostgreSQL: localhost:5432 (DB: fooddelivery, user: fd, pass: fd)
Базата на податоци автоматски се иницијализира со тест корисници (customer, owner, admin). Backend чека базата да биде целосно подготвена пред стартување (healthcheck).


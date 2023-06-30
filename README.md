# Demo build docker
## How to run project
1. Build image container
```bash
docker-compose build --no-cache
```
2. Start container
```bash
docker-compose up -d
```
3. Stop and remove container
```bash
docker-compose down
```

## Build a service
```bash
docker compose build [service_name]
```

## Docker exec to containner
```bash
docker-compose exec appb3p bash -c "cd api-design && composer require laravel/breeze --dev"
```

# Run local environment Admin (Vite, Vuejs)
```bash
cd admin-design
npm run dev
```
# Run local environment Frontend (Nextjs, Reactjs)
```bash
cd front-design
npm run dev
```

# Swagger api documentation (Breeze api, sanctum)
```bash
composer require "darkaonline/l5-swagger"
php artisan vendor:publish --provider "L5Swagger\L5SwaggerServiceProvider"
```
## Add comment documentation to controller
/**
 * @OA\Info(
 *     version="1.0",
 *     title="Example for response examples value"
 * )
 * @OA\PathItem(path="/api")
 */
 
 ```bash
php artisan l5-swagger:generate
```

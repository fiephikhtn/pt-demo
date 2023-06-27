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
## Docker exec to containner
```bash
docker-compose exec appb3p bash -c "cd api-design && composer require laravel/breeze --dev"
```


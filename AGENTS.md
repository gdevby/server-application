# Cattr Server Application

> Монорепо: общий контекст и правила Cursor — в [AGENTS.md](../AGENTS.md) и [`.cursor/rules/`](../.cursor/rules/).

## О проекте

Open-source backend для учёта времени [Cattr](https://cattr.app). Laravel 10, PHP 8.2, модульная архитектура.

## Стек

- **Backend:** PHP 8.2, Laravel 10, Eloquent, Laravel Sanctum
- **Frontend:** Vue.js в `resources/frontend/`, сборка через Laravel Mix
- **Тесты:** PHPUnit 10, фабрики в `tests/Factories/`
- **Стиль кода:** PSR-2 (`phpcs.xml`)
- **Лицензия:** SSPL-1.0

## Команды

```bash
composer install && yarn          # зависимости
php artisan key:generate            # первый запуск
php artisan migrate --seed --seeder=InitialSeeder
php artisan cattr:make:admin        # создать admin@cattr.app / password
php artisan serve                   # http://127.0.0.1:8000
yarn dev                            # фронтенд в dev-режиме
composer dumphelpers                  # IDE-хелперы
```

### Тесты

```bash
# настроить .env.testing (см. tests/README.md)
php artisan migrate --env=testing
php artisan db:seed --class=RoleSeeder --env=testing

vendor/phpunit/phpunit/phpunit --configuration phpunit.xml tests/Feature
```

## Архитектура

```
app/
  Http/
    Controllers/Api/     # API-контроллеры (наследуют ItemController)
    Requests/            # Form Request по сущностям
    Responses/           # CattrSuccessResponse, CattrErrorResponse
  Models/                # Eloquent-модели
  Policies/              # Авторизация
  Scopes/                # Ограничение выборок по ролям
  Services/              # Бизнес-логика
  Enums/                 # Role, ScreenshotsState и др.
routes/
  api.php                # API-роуты, auth:sanctum
tests/
  Feature/               # Feature-тесты по сущностям
  Factories/             # Фабрики данных
  Facades/               # Фасады фабрик
resources/frontend/core/
  modules/               # Vue-модули (Users, Tasks, Projects, …)
  store/modules/         # Vuex
```

## API

- Префикс `/api`, аутентификация через `auth:sanctum`
- CRUD-паттерн: `list`, `create`, `edit`, `show`, `remove`, `count`
- Контроллеры наследуют `ItemController`, задают `protected const MODEL`
- Валидация в Form Request (`app/Http/Requests/{Entity}/`)
- Доступ через Policies и Scopes
- Apidoc-блоки в контроллерах для документации

## Соглашения

- Общие правила монорепо — в [AGENTS.md](../AGENTS.md)
- Новые эндпоинты: Request + Policy + Feature-тест с ролями
- Cursor rules (при workspace = `cattr/`): `server-laravel.mdc`, `server-tests.mdc`, `server-frontend-vue.mdc`

## Ссылки

- [Документация](https://docs.cattr.app)
- [Backend API docs](https://docs.cattr.app/backend#/)
- [Issues](https://git.amazingcat.net/cattr/core/app/-/issues)

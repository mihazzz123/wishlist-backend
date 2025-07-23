# Структура проекта "Список желаний" (Clean Architecture)

## Бэкенд (Go)
```plaintext
wishlist-backend/
├── cmd/
│   └── main.go                 # Инициализация приложения
├── internal/
│   ├── domain/                 # Ядро системы
│   │   ├── entities/
│   │   │   ├── user.go         # User: ID, Username, Email, PasswordHash
│   │   │   ├── wish.go         # Wish: ID, UserID, Title, Description, Price, ...
│   │   │   └── category.go     # Category: ID, Name
│   │   └── repositories/       # Абстрактные интерфейсы
│   │       ├── user_repo.go    # UserRepository interface
│   │       ├── wish_repo.go
│   │       └── category_repo.go
│   │
│   ├── application/            # Бизнес-логика
│   │   ├── usecases/
│   │   │   ├── auth_usecase.go # RegisterUser, Login, ValidateToken
│   │   │   ├── wish_usecase.go # CreateWish, UpdateWish, GetUserWishes
│   │   │   └── category_usecase.go
│   │   └── dto/                # Data Transfer Objects
│   │       ├── auth_dto.go     # LoginRequest, AuthResponse
│   │       ├── wish_dto.go     # CreateWishRequest, WishResponse
│   │       └── ...
│   │
│   ├── infrastructure/         # Реализации
│   │   ├── db/
│   │   │   ├── postgres/
│   │   │   │   ├── user_repo.go # Конкретная реализация UserRepository
│   │   │   │   ├── wish_repo.go
│   │   │   │   └── ...
│   │   │   └── database.go     # DB connection
│   │   ├── http/
│   │   │   ├── handlers/
│   │   │   │   ├── auth_handler.go  # /api/auth/register, /api/auth/login
│   │   │   │   ├── wish_handler.go  # /api/wishes (CRUD)
│   │   │   │   └── ...
│   │   │   └── middleware/
│   │   │       ├── auth_middleware.go # JWT проверка
│   │   │       └── logging_middleware.go
│   │   └── auth/
│   │       └── jwt_service.go  # Генерация/верификация токенов
│   │
│   └── interfaces/             # Адаптеры
│       ├── http/
│       │   ├── router.go       # Настройка маршрутов
│       │   └── server.go       # HTTP сервер
│       └── validators/         # Валидация запросов
│           └── auth_validator.go
│
├── pkg/
│   ├── config/                 # Конфигурация
│   │   └── config.go           # Загрузка .env
│   ├── utils/
│   │   ├── password.go         # HashPassword, CheckPassword
│   │   └── errors.go           # Кастомные ошибки
│   └── logger/                 # Логирование
│       └── logger.go
│
├── migrations/                 # Миграции
│   ├── 0001_init.up.sql
│   └── 0001_init.down.sql
├── .env.example
├── go.mod
└── go.sum
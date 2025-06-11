#Control System API

![GitHub last commit](https://img.shields.io/github/last-commit/anomal3/DishgramAPI)
![API Status](https://img.shields.io/badge/status-active-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)

Управление DishAI инфраструктурой через REST API. Система позволяет контролировать ..., управлять расписанием и мониторить состояние путей.

## 🔍 Оглавление

- [Быстрый старт](#-быстрый-старт)
- [Эндпоинты API](#-эндпоинты-api)
- [Примеры запросов](#-примеры-запросов)
- [Статусы ответов](#-статусы-ответов)
- [Авторизация](#-авторизация)
- [Лимиты запросов](#-лимиты-запросов)
- [WebSocket события](#-websocket-события)
- [Разработчикам](#-разработчикам)

## 🚀 Быстрый старт

### Требования
- .NET 6.0+
- MySQL 8.0+
- Docker (опционально)

### Установка
```bash
# Клонирование репозитория
git clone https://github.com/yourname/railway-system.git
cd railway-system

# Настройка базы данных
docker-compose up -d

# Запуск приложения
dotnet run --project RailwaySystem.Api
```

### Тестовые данные
```json
{
  "username": "dispatcher@railway.com",
  "password": "P@ssw0rd123"
}
```

## 🌐 Эндпоинты API

### Поезда
| Метод | Эндпоинт                | Описание                     |
|-------|-------------------------|-----------------------------|
| GET   | `/api/trains`           | Список всех поездов         |
| POST  | `/api/trains/start`     | Запуск поезда               |
| POST  | `/api/trains/{id}/stop` | Экстренная остановка поезда |

### Станции
| Метод | Эндпоинт             | Описание               |
|-------|----------------------|-----------------------|
| GET   | `/api/stations`      | Список станций        |
| POST  | `/api/stations/sync` | Синхронизация расписания |

## 💡 Примеры запросов

### Запуск поезда
```http
POST /api/trains/start HTTP/1.1
Content-Type: application/json
Authorization: Bearer {token}

{
  "trainId": "TGV-2045",
  "route": "Paris-Lyon",
  "maxSpeed": 320
}
```

**Успешный ответ:**
```json
{
  "status": "success",
  "data": {
    "trainId": "TGV-2045",
    "currentSpeed": 0,
    "nextStation": "Lyon Part-Dieu",
    "estimatedArrival": "2023-12-15T14:30:00Z"
  }
}
```

## 📊 Статусы ответов

| Код | Описание                  |
|-----|--------------------------|
| 200 | OK                       |
| 400 | Неверный запрос          |
| 401 | Не авторизован           |
| 403 | Доступ запрещен          |
| 500 | Ошибка сервера           |

## 🔒 Авторизация

Используется JWT аутентификация. Получите токен:

```http
POST /api/auth/login HTTP/1.1
Content-Type: application/json

{
  "username": "dispatcher@railway.com",
  "password": "P@ssw0rd123"
}
```

## ⚠️ Лимиты запросов

- 100 запросов/минуту на IP
- 5 одновременных WebSocket соединений

## 🌐 WebSocket события

Подключитесь к `wss://your-api/ws` для получения событий в реальном времени:

```javascript
const socket = new WebSocket("wss://your-api/ws");

socket.onmessage = (event) => {
  console.log("Получено событие:", JSON.parse(event.data));
};
```

**Пример события:**
```json
{
  "type": "TRAIN_DEPARTURE",
  "data": {
    "trainId": "TGV-2045",
    "timestamp": "2023-12-15T12:00:00Z"
  }
}
```

## 👨‍💻 Разработчикам

### Структура проекта
```
RailwaySystem/
├── Api/           # Web API
├── Core/          # Бизнес-логика
├── Infrastructure # База данных
└── Tests/         # Юнит-тесты
```

### Сборка
```bash
dotnet build
dotnet test
```

### Контакты
- Email: dev@railway-system.com
- Telegram: @railway_support

---

> 🛠️ Разработано с использованием .NET 6 | 📄 [Лицензия MIT](LICENSE) | 🚄 Железные дороги будущего

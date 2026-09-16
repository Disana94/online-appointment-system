### 1. Получить список мастеров

- **Метод:** `GET`
- **Path:** `/api/masters`
- **Query parameters:**

  | Параметр    | Тип     | Обязательный | Описание  |
  |-------------|---------|--------------|-----------|
  | `serviceId` | integer | да           | ID услуги |

- **Полный URL:** `GET /api/masters?serviceId={serviceId}`
- **Описание:** Возвращает массив мастеров по услуге.

**Пример запроса:**

```http
GET /api/masters?serviceId=1
```

**Пример ответа (`200 OK`):**

```json
[
  { "id": 1, "name": "Иван" },
  { "id": 2, "name": "Лев" }
]
```

**Ошибки:**

| Код  | Описание                 |
|------|--------------------------|
| `400`| Некорректный `serviceId` |
| `401`| Unauthorized             |

**Пример ответа (`400 Bad Request`):**

```json
{
  "error": "Invalid serviceId"
}
```

**Пример ответа (`401 Unauthorized`):**

```json
{
  "error": "Unauthorized"
}
```

### 2. Получить слоты мастера

- **Метод:** `GET`
- **Path:** `/api/masters/{masterId}/slots`
- **Path parameters:**

  | Параметр   | Тип     | Обязательный | Описание   |
  |------------|---------|--------------|------------|
  | `masterId` | integer | да           | ID мастера |

- **Query parameters:**

  | Параметр    | Тип      | Обязательный | Описание          |
  |-------------|----------|--------------|-------------------|
  | `date`      | dateTime | да           | Дата              |
  | `serviceId` | integer  | да           | ID услуги         |

- **Полный URL:** `GET /api/masters/{masterId}/slots?date={date}&serviceId={serviceId}`
- **Описание:** Возвращает список доступных слотов мастера на указанную дату по услуге.

**Пример запроса:**

```http
GET /api/masters/2/slots?date=2026-02-01&serviceId=1
```

**Пример ответа (`200 OK`):**

```json
[
  {
    "date": "01.02.2026",
    "time": "12:35"
  },
  {
    "date": "01.02.2026",
    "time": "13:00"
  }
]
```

**Ошибки:**

| Код  | Описание              |
|------|-----------------------|
| `401`| Unauthorized          |
| `404`| Мастер не найден      |

**Пример ответа (`401 Unauthorized`):**

```json
{
  "error": "Unauthorized"
}
```

**Пример ответа (`404 Not Found`):**

```json
{
  "error": "Master not found"
}
```
### 3. Создать бронирование

- **Метод:** `POST`
- **Path:** `/api/booking`
- **Body parameters:**

  | Параметр    | Тип     | Обязательный | Описание          |
  |-------------|---------|--------------|-------------------|
  | `serviceId` | integer | да           | ID услуги         |
  | `date`      | string  | да           | Дата бронирования |
  | `masterId`  | integer | да           | ID мастера        |

- **Полный URL:** `POST /api/booking`
- **Описание:** Создаёт новое бронирование.

**Пример запроса:**

```http
POST /api/booking
Content-Type: application/json
```

```json
{
  "serviceId": 1,
  "date": "2026-09-20",
  "masterId": 2
}
```

**Пример ответа (`201 Created`):**

```json
{
  "id": 10,
  "serviceId": 1,
  "date": "2026-09-20",
  "masterId": 2
}
```

**Ошибки:**

| Код  | Описание                    |
|------|-----------------------------|
| `400`| Некорректные данные запроса |
| `401`| Unauthorized                |
| `404`| Мастер или услуга не найдены|
| `409`| Слот уже занят              |

**Пример ответа (`400 Bad Request`):**

```json
{
  "error": "Invalid request body"
}
```

**Пример ответа (`401 Unauthorized`):**

```json
{
  "error": "Unauthorized"
}
```

**Пример ответа (`404 Not Found`):**

```json
{
  "error": "Master or service not found"
}
```

**Пример ответа (`409 Conflict`):**

```json
{
  "error": "Slot already booked"
}
```

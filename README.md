    # Stark PC Components Backend Service
                                                ##Лара прочти


## Запуск
- pip install flask-migrate
- pip install flask flask-sqlalchemy flask-cors python-dotenv
- http://127.0.0.1:5000


## Назначение
REST API для интернет-магазина компьютерных комплектующих. Обработка данных о товарах, категориях и заказах.

## Технологии
- Python 3.11
- Flask
- SQLite
- SQLAlchemy

## Модели данных

### Category
| Поле        | Тип     | Описание                |
|-------------|---------|-------------------------|
| id          | INTEGER | Первичный ключ          |
| name        | TEXT    | Название категории      |
| description | TEXT    | Описание                |

### Product
| Поле           | Тип     | Описание                     |
|----------------|---------|------------------------------|
| id             | INTEGER | Первичный ключ               |
| name           | TEXT    | Наименование                 |
| description    | TEXT    | Полное описание              |
| price          | REAL    | Цена в рублях                |
| stock_quantity | INTEGER | Остаток на складе            |
| category_id    | INTEGER | Внешний ключ к Category      |
| image_url      | TEXT    | Путь к изображению           |
| specifications | TEXT    | Характеристики в JSON        |

### Order
| Поле            | Тип     | Описание                         |
|-----------------|---------|----------------------------------|
| id              | INTEGER | Первичный ключ                   |
| customer_name   | TEXT    | Имя заказчика                    |
| customer_email  | TEXT    | Email                            |
| customer_phone  | TEXT    | Телефон                          |
| order_date      | TEXT    | Дата в ISO формате               |
| status          | TEXT    | new, processing, shipped, completed, cancelled |
| total_amount    | REAL    | Сумма заказа                     |

### OrderItem
| Поле        | Тип     | Описание                         |
|-------------|---------|----------------------------------|
| id          | INTEGER | Первичный ключ                   |
| order_id    | INTEGER | Внешний ключ к Order             |
| product_id  | INTEGER | Внешний ключ к Product           |
| quantity    | INTEGER | Количество                       |
| unit_price  | REAL    | Цена на момент заказа            |

## API Endpoints

Формат ответов: JSON. Успех — 200 или 201. Ошибка — соответствующий код с `{"error": "текст"}`.

### Категории

| Метод | Путь                 | Описание                  |
|-------|----------------------|---------------------------|
| GET   | /api/categories      | Список категорий          |
| GET   | /api/categories/{id} | Категория по ID           |
| POST  | /api/categories      | Создать категорию         |
| PUT   | /api/categories/{id} | Обновить категорию        |
| DELETE| /api/categories/{id} | Удалить категорию         |

### Товары

| Метод | Путь                 | Описание                             |
|-------|----------------------|--------------------------------------|
| GET   | /api/products        | Список товаров с пагинацией и фильтром по категории |
| GET   | /api/products/{id}   | Товар по ID                          |
| POST  | /api/products        | Добавить товар                       |
| PUT   | /api/products/{id}   | Обновить товар                       |
| DELETE| /api/products/{id}   | Удалить товар                        |

Параметры GET /api/products:
- `category` (int) — ID категории
- `page` (int) — страница, по умолчанию 1
- `limit` (int) — записей на странице, по умолчанию 20

### Заказы

| Метод | Путь                     | Описание          |
|-------|--------------------------|-------------------|
| GET   | /api/orders              | Список заказов    |
| GET   | /api/orders/{id}         | Заказ по ID       |
| POST  | /api/orders              | Создать заказ     |
| PUT   | /api/orders/{id}/status  | Обновить статус   |

## Логика сервисов

**ProductService**  
- Пагинация и фильтрация товаров по категории.  
- Проверка остатков перед добавлением в заказ.  
- Валидация обязательных полей и цены.

**CategoryService**  
- Проверка уникальности названия.  
- Запрет удаления категории с товарами.

**OrderService**  
- Расчёт итоговой суммы заказа.  
- Списание товаров со склада после оформления.  
- Валидация контактных данных.


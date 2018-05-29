# Дипломный проект курса «Библиотека React»

## Взаимодействие с сервером по HTTP

Для взаимодействии с серверной частью приложения вам доступно REST API по адресу:

https://neto-api.herokuapp.com/bosa-noga

### Получение списка категорий

` GET /categories` — получить инфомрацию о категориях в JSON-формате.

В ответ приходит либо сообщение об ошибке, либо JSON-массив со списком категорий. Например:
```json
[
  {
    "id": 12,
    "title": "Детская обувь"
  },
  {
    "id": 13,
    "title": "Женская обувь"
  }
]
```

Тут:
- `id` — идентификатор категории на сервере, _число_;
- `title` — название категории, _строка_.

### Получение списка товаров

`GET /products` — получить информацию о товарах в JSON-формате.

Формат данных при отправке — `GET-параметры`. Поля для фильтрации и сортировки:

- `page` — номер страницы, _число_
- `type` — тип обуви (балетки, босоножки, ботильоны…), _строка_
- `color` — цвет обуви, _строка_
- `size` — размер обуви, _строка_
- `heelSize` — размер каблука, _строка_
- `reason` — повод (офис, вечеринка, свадьба), _строка_
- `season` — сезон, _строка_
- `brand` — ренд, _строка_
- `discounted` — наличие скидки, _логическое значение_
- `categoryId` — идентификатор категории, _число_
- `sortBy` — поле для сортировки, _строка_

В ответ приходит либо сообщение об ошибке, либо JSON-объект с данными о изображении. Например:
```json
[
  {
    "id": 1,
    "title": "Босоножки женские",
    "image": "https://picsum.photos/200",
    "manufacturer": "Damlax",
    "price": 5950
  },
  {
    "id": 2,
    "title": "Туфли-кроссовки",
    "image": "https://picsum.photos/300",
    "manufacturer": "Reebok",
    "price": 12850
  }
]
```

Тут:
- `id` — идентификатор товара на сервере, _число_;
- `title` — название товара, _строка_;
- `image` — URL-адрес главного изображения товара, по которому оно доступно в сети, _строка_;
- `manufacturer` — производитель, _строка_;
- `price` — цена товара в рублях, _число_.

### Получение новинок

`GET /featured` — получить информацию о новинках товаров в JSON-формате.

В ответ приходит либо сообщение об ошибке, либо JSON-объект с данными о изображении. Например:
```json
[
  {
    "id": 1,
    "title": "Босоножки женские",
    "image": "https://picsum.photos/200",
    "manufacturer": "Damlax",
    "price": 5950
  },
  {
    "id": 2,
    "title": "Туфли-кроссовки",
    "image": "https://picsum.photos/300",
    "manufacturer": "Reebok",
    "price": 12850
  }
]
```

### Получение информации о товаре

`GET /products/${id}` — получить информацию о товаре в JSON-формате. Тут `${id}` — идентификатор товара на сервере.

В ответ приходит либо сообщение об ошибке, либо JSON-объект с данными об изображении. Например:
```json
{
  "id": "d290f1ee-6c54-4b01-90e6-d701748f0851",
  "title": "Босоножки женские",
  "images": [
    "https://picsum.photos/200",
    "https://picsum.photos/300"
  ],
  "sku": "1333358811",
  "manufacturer": "Damlax",
  "color": "Синий",
  "material": "Натуральная кожа",
  "reason": "Офис",
  "season": "Лето",
  "sizes": [
    {
      "size": "12 US",
      "avalible": true
    },
    {
      "size": "15 US",
      "avalible": false
    }
  ]
}
```

Кроме полей, которые были уже описаны при получении списка товаров, еще некоторые свойства с ключами:
- `images` — URL-адреса изображений товара, по которому они доступны в сети, первое изображение считается галвным, _массив строк_;
- `color` — цвет товара, _строка_;
- `material` — материал, из которого произведен товар, _строка_;
- `reason` — повод, по которому предполагается носить товар, _строка_;
- `season` — сезон, для которого подходит товар, _строка_;
- `sizes` — информация о размерах и наличии их на складе, _массив объектов_.

Свойство `sizes`, которое хранит массив объетов со следующими свойствами:
- `size` — размер, _строка_;
- `avalible` — доступность размера на складе, _логическое значение_.

### Создание заказа

`POST /order` — создать новый заказ.

Формат данных при отправке `json-массив`. Пример запроса:
```json
[
  {
    "id": 1,
    "size": "13 US",
    "amount": 1
  },
  {
    "id": 2,
    "size": "15 US",
    "amount": 2
  },
]
```

Тут:
- `id` — идентификатор товара на сервере, _число_;
- `size` — размер товара, _строка_;
- `amount` — количество единиц товара, _число_.

Возможные ответы:
- `201` — заказ успешно создан, в теле ответа идентификатор заказа;
- `400` — товара такого размера нет в наличии;
- `404` — ресурас не найдет, в теле ответа:
  - `sizeNotFound` — размер не найден,
  - `productNotFound` — товар не найден:

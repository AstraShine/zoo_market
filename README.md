# ZooMarket

Интернет-магазин зоотоваров и будущая платформа зоо-услуг для сети «Зоология».

## О проекте

ZooMarket — интернет-магазин зоотоваров, который развивается на базе действующей сети магазинов «Зоология». Сегодня сеть насчитывает 20 магазинов в пяти крупных городах Дальнего Востока. Компания уже полгода продаёт товары через соцсети и мессенджеры и накопила клиентскую базу в 1500 человек. С этой базой запускается маркетплейс.

Цель проекта — начать с продажи товаров для животных и постепенно вырасти в полноценную платформу зоо-услуг. Через два года планируется выход на Центральную Россию; пока компания ориентируется на Дальний Восток.

Инвестор — Маша Собачкина, имеет большой опыт вывода маркетплейсов на рынок. Бюджет на горизонте двух лет — 15–20 млн рублей.

## Функционал

### MVP (0–3 месяца)

- Продажа еды для животных
- Продажа сопутствующих товаров (поводки, шлейки и т. д.)

### Этап 2 (3–6 месяцев)

- Донаты для приютов: раз в месяц деньги перечисляются на выбранный приют
- Продажа породистых животных от заводчиков (кошки и собаки) — объявления о продаже

### Этап 3 (около 1 года)

- Запись в партнёрские ветеринарные клиники

### Этап 4 (около 1,5 лет)

- Услуги для животных (стрижка, выгул)

### Полный функционал — 2 года

## Технические требования

**Платформа:** веб-сайт.

**Оплата:** только онлайн-оплата.

**Доставка:** интеграция с курьерской службой; доступен самовывоз из магазина или доставка курьером.

**Интеграция с 1С:** у сети 20 магазинов с 1С — необходимо выгружать каталог товаров из 1С.

**Уведомления:** push- и SMS-рассылки.

**Система лояльности:** накопительные скидки и другие механики.

**Идентификация клиента:** по номеру телефона.

**Рекомендательная система:** фиксируются действия покупателя (покупки, лайки) и на их основе формируются рекомендации.

**Инфраструктура:** собственных мощностей для поддержки сайта нет — планируется размещение в облаке.

**Нефункциональные требования:** масштабируемость, надёжность.

## Дорожная карта

| Срок | Функционал |
|------|-----------|
| 0–3 месяца | Продажа товаров для животных |
| 3–6 месяцев | Донаты в приюты, продажа щенков и котят |
| ~1 год | Запись в ветеринарные клиники |
| ~1,5 года | Услуги для животных |
| 2 года | Полный функционал; выход на Центральную Россию |

# Диаграмма вариантов использования

![Use Case Diagram](Diagramm.png)

# OpenApi

```json
openapi: 3.0.4
info: 
  title: Petshop
  version: 1.0.12
tags:
  - name: user
    description: Operations about user
paths:
  /user:
    post:
      tags:
        - user
      summary: Greate new user
      operationId: createUser
      requestBody:
          description: Created user object
          content:
            application/json:
             schema:
               $ref: '#/components/schemas/User'
            application/xml:
              schema:
                $ref: '#/components/schemas/User'
            application/x-www-form-urlencoded:
             schema:
                $ref: '#/components/schemas/User'
      responses:
        '200':
          description: successful operation
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
            application/xml:
              schema:
                $ref: '#/components/schemas/User'
        default:
          description: Unexpected error
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error"
    get:
      tags:
        - user
      summary: Get list of users
      operationId: getListOfUser
      responses:
        '200':
          description: successful operation
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
            application/xml:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
        default:
          description: Unexpected error
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error"
      
  /user/{username}:
    get:
      tags:
        - user
      summary: Get user by user name.
      description: Get user detail based on username.
      operationId: getUserByName
      parameters:
        - name: username
          in: path
          description: The name that needs to be fetched. Use user1 for testing
          required: true
          schema:
            type: string
      responses:
        '200':
          description: successful operation
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
            application/xml:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          description: Invalid username supplied
        '404':
          description: User not found
        default:
          description: Unexpected error
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error"

components:
  schemas:
    User:
      required:
       - id
       - username
       - password
      type: object
      properties:
        id:
          type: integer
          format: int64
          example: 10
        username:
          type: string
          example: theUser
        firstName:
          type: string
          example: John
        lastName:
          type: string
          example: James
        email:
          type: string
          example: john@email.com
        password:
          type: string
          example: '12345'
        phone:
          type: string
          example: '12345'
        userStatus:
          type: string
          description: User Status
          format: string
          enum:
            - online
            - offline
            - removed
      xml:
        name: user
    Error:
      type: object
      properties:
        code:
          type: string
        message:
          type: string
      required:
        - code
        - message
```

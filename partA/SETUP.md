# Part A — Setup

## Сонгосон API

**JSONPlaceholder** — https://jsonplaceholder.typicode.com

## Brief

JSONPlaceholder нь REST API тест болон прототип хийхэд зориулагдсан үнэгүй нээлттэй хуурамч API юм. Бодит database-д бичдэггүй ч CRUD үйлдлүүдийг бүгдийг дэмжиж, хариу буцаадаг.

## Боломжит Endpoints

| Resource | URL |
|----------|-----|
| Posts | `/posts` |
| Comments | `/comments` |
| Users | `/users` |
| Todos | `/todos` |

## Auth

Authentication шаардахгүй. API key болон token хэрэггүй.

## Base URL

```
https://jsonplaceholder.typicode.com
```

## Rate Limit

Тодорхой rate limit байхгүй. Олон нийтийн үнэгүй сервис учраас хэт олон request явуулахаас зайлсхийх нь зүйтэй.

## Тестлэх Resource

Энэ лабораторийн хүрээнд **Posts** resource-г үндсэн тестийн объект болгон сонгов.

- `GET /posts` — бүх posts жагсаалт (100 post)
- `GET /posts/1` — нэг post
- `POST /posts` — шинэ post үүсгэх
- `PUT /posts/1` — post бүтнээр засах
- `DELETE /posts/1` — post устгах
- `GET /posts/999999` — байхгүй post (404 тест)

# Бие даалт 14 — Integration & API Testing

**F.CSM311 Программ хангамжийн бүтээлт**  
**API:** JSONPlaceholder (https://jsonplaceholder.typicode.com)  
**Хэрэгсэл:** Postman, Newman, GitHub Actions

---

## Repository Бүтэц

```
bie-daalt-14/
├── partA/
│   ├── SETUP.md          # Сонгосон API, brief, base URL
│   └── screenshot.png    # Эхний амжилттай request
├── postman/
│   ├── collection.json   # 8 request, test script-тэй
│   ├── env.dev.json      # Dev environment
│   └── env.ci.json       # CI environment
├── .github/workflows/
│   └── api-tests.yml     # GitHub Actions Newman CI
├── reports/
│   └── api.html          # Newman HTML report
└── REFLECTION.md         # 5 асуултын хариу
```

---

## Newman Local ажиллуулах

### 1. Newman суулгах

```bash
npm install -g newman newman-reporter-htmlextra
```

### 2. Тест ажиллуулах

```bash
newman run postman/collection.json -e postman/env.dev.json
```

### 3. HTML report үүсгэх

```bash
newman run postman/collection.json \
  -e postman/env.dev.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/api.html
```

---

## Collection Агуулга

| # | Request | Method | Тайлбар |
|---|---------|--------|---------|
| 1 | List all posts | GET | 200, 100 post, schema шалгана |
| 2 | Get single post | GET | 200, id=1, chain хувьсагч |
| 3 | Create post | POST | 201, шинэ id буцаах |
| 4 | Update post | PUT | 200, өөрчлөлт хадгалагдсан |
| 5 | Delete post | DELETE | 200, хоосон object |
| 6 | Not found (404) | GET | 404, negative test |
| 7 | Empty filter result | GET | 200, хоосон массив |
| 8 | Filter by userId | GET | 200, бүгд userId=1 |

**Нийт тест assertion: 25+**

---

## GitHub Actions

Push хийх бүрт автоматаар Newman ажиллана. Actions tab-д үр дүн харагдана.

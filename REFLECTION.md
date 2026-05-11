# Reflection — Бие даалт 14

## 1. Хамгийн их үнэ цэнэтэй assertion

**Schema / property тест** хамгийн их үнэ цэнэтэй санагдсан. Status code 200 буцаасан ч response-ийн бүтэц буруу байж болно — жишээлбэл `id`, `title`, `body` field байхгүй эсвэл өөр нэртэй байвал client код эвдэрнэ. Schema тест нь "API contract" хэвээр байгааг баталгаажуулдаг учраас хамгийн практик assertion гэж үзэв.

Мөн **утгын төрлийг шалгах тест** (`pm.expect(data.id).to.be.a('number')`) чухал байсан. JavaScript-д type coercion байдаг учраас `"1"` болон `1` нь харилцан адилгүй. API `id`-г string болгон буцаавал frontend код нуугдмал алдаатай ажиллах эрсдэлтэй.

## 2. Negative test — дэлгэрэнгүй

`GET /posts/999999` — байхгүй post авах тест.

Энэ тест нь **404 status code** буцаахыг шалгана. JSONPlaceholder-т байхгүй id-р хандахад хоосон object `{}` буцаадаг. Тест нь:
- 404 status буцааж байгааг шалгана
- Response body нь хоосон object байгааг шалгана

Энэ negative тест чухал учир нь: API зөвхөн happy path-д зөв ажиллах нь хангалтгүй. Байхгүй resource-т хандахад зохих алдааны код буцааж байгааг баталгаажуулах нь API contract-ийн нэг хэсэг. Хэрэв энэ тест fail болвол client код `undefined` data-тай ажиллах болж runtime error гарна.

## 3. Postman дотор pass, Newman-д fail болсон уу?

JSONPlaceholder public API ашигласан учраас environment variable-ийн асуудал гарсангүй. Гэхдээ теоретик тайлбар: хэрэв `{{baseUrl}}` env variable Postman-д тохируулагдсан боловч Newman-д `env.json` файл зөв дамжуулаагүй бол бүх request `{{baseUrl}}/posts` гэсэн literal URL руу явж fail болно. Иймд `-e env.ci.json` параметр заавал дамжуулах хэрэгтэй.

## 4. Token болон secret зохицуулалт

JSONPlaceholder auth шаардахгүй учраас token хэрэглээгүй. Гэхдээ env-ийн соёлын хувьд: `env.dev.json`-д жинхэнэ token бичиж `.gitignore`-д нэмнэ, `env.ci.json`-д `REAL_TOKEN_HERE` гэсэн placeholder ашиглана. GitHub Actions-д `secrets.API_TOKEN` ашиглан `sed` командаар placeholder-г орлуулна:

```bash
sed -i 's/REAL_TOKEN_HERE/${{ secrets.API_TOKEN }}/g' postman/env.ci.json
```

## 5. API өөрчлөгдвөл аль хэсэг хамгийн эмзэг вэ?

**Schema assertion тест** хамгийн эмзэг хэсэг. Жишээлбэл JSONPlaceholder `userId` field-ийг `authorId` болгон нэрлэвэл `pm.expect(data).to.have.property('userId')` тест бүгд fail болно. Энэ нь "breaking change" — client код эвдэрнэ.

Эмзэг байдлыг бууруулах арга:
- Тест бичихдээ зайлшгүй шаардлагатай field-уудыг л шалгах
- `additionalProperties` зөвшөөрч, зайлшгүй биш field-уудад тест бичихгүй байх
- Collection-д `baseUrl` variable ашигласан учраас server шилжилт хялбар — энэ хэсэг эмзэг биш


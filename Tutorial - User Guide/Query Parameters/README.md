# پارامترهای Query
<br><br>
هنگامی که پارامترهای دیگری را که بخشی از پارامترهای مسیر نیستند، اعلام می‌کنید، آنها به‌طور خودکار به‌عنوان «پارامترهای query» تفسیر می‌شوند.
<br><br>
```python
from fastapi import FastAPI

app = FastAPI()

fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]


@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):
    return fake_items_db[skip : skip + limit]
```
<br><br>
query مجموعه‌ای از جفت‌های کلیدی-مقدار است که پس از کاراکتر '?' در یک URL قرار می‌گیرند و با کاراکترهای '&' از هم جدا می‌شوند.
برای مثال در URL:
```python
http://127.0.0.1:8000/items/?skip=0&limit=10
```
پارامترهای کويری اینها هستند...
* skip: با مقدار 0
* limit: با مقدار 10

از آنجایی که آنها بخشی از URL هستند، آنها «طبعاً» رشته‌ها هستند.
<br><br>

اما هنگامی که آنها را با انواع Python اعلام می‌کنید (در مثال بالا، به عنوان int)، آنها به آن نوع تبدیل می‌شوند و بر اساس آن اعتبارسنجی می‌شوند.
<br><br>
تمام فرآیند‌های مشابهی که برای پارامترهای مسیر اعمال می‌شود، همچنین برای پارامترهای پرس‌وجو نیز اعمال می‌شود:
* پشتیبانی ویرایشگر (واضح است)
* تجزیه داده‌ها
* اعتبارسنجی داده‌ها
* مستندات خودکار

# پیشفرضها

از آنجایی که پارامترهای query بخشی ثابت از مسیر نیستند، آنها اختیاری هستند و می‌توانند مقدار پیش‌فرض داشته باشند.
در مثال بالا آنها مقدار پیش‌فرض skip=0 و limit=10 دارند.
<br><br>
```python
http://127.0.0.1:8000/items/
```
"این همان چیزی است که رفتن به این URL است:

```python
http://127.0.0.1:8000/items/?skip=0&limit=10
```
اما اگر به عنوان مثال به این URL بروید:
<br>
```python
http://127.0.0.1:8000/items/?skip=20
```
در تابع شما مقادیر پارامترها به شرح زیر خواهد بود:
<br>
* skip=20: زیرا شما آن را در URL تنظیم کردید
* limit=10: زیرا مقدار پیش‌فرض آن بود

# پارامترهای انتخابی
<br><br>
به همان روش، می‌توانید پارامترهای query اختیاری را با تعیین پیش‌فرض آنها به None اعلام کنید:
<br><br>
python 3.10+
<br><br>
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: str, q: str | None = None):
    if q:
        return {"item_id": item_id, "q": q}
    return {"item_id": item_id}
```
<br><br>
در این حالت، پارامتر تابع q اختیاری خواهد بود و مقدار پیش‌فرض آن None خواهد بود.
<br><br>
>[!NOTE]
> همچنین توجه داشته باشید که FastAPI به اندازه کافی هوشمند است تا متوجه شود که پارامتر مسیر item_id یک پارامتر مسیر است و q نیست، بنابراین، یک پارامتر پرس و جو است.

# تبدیل نوع پارامتر query
<br><br>

شما همچنین می‌توانید انواع bool را اعلام کنید و آنها تبدیل خواهند شد.
<br><br>
python 3.10+
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: str, q: str | None = None, short: bool = False):
    item = {"item_id": item_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
```
در این مورد اگر شما به این به این مسیر :
```python
http://127.0.0.1:8000/items/foo?short=1
```
یا
```python
http://127.0.0.1:8000/items/foo?short=True
```
یا
```python
http://127.0.0.1:8000/items/foo?short=true
```
یا
```python
http://127.0.0.1:8000/items/foo?short=on
```
یا هر نوع تغییر حالت دیگر (حروف بزرگ، اولین حرف بزرگ و غیره)، تابع شما پارامتر short را با یک مقدار بولی True خواهد دید. در غیر این صورت False.
<br><br>

# پارامترهای مسیر و query چندگانه
<br><br>
شما می‌توانید چندین پارامتر مسیر و query را همزمان اعلام کنید، FastAPI می‌داند که کدام یک کدام هستند.
آنها براساس نام شناسایی خواهند شد:

python 3.10+

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/users/{user_id}/items/{item_id}")
async def read_user_item(
    user_id: int, item_id: str, q: str | None = None, short: bool = False
):
    item = {"item_id": item_id, "owner_id": user_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
```
<br><br>
# پارامترهای query اجباری
<br><br>
هنگامی که شما یک مقدار پیش‌فرض برای پارامترهای غیر مسیر (تاکنون فقط پارامترهای query را دیده‌ایم) مشخص می‌کنید، آن پارامتر الزامی نیست.
<br><br>
اگر نمی‌خواهید یک مقدار خاص اضافه کنید اما فقط آن را اختیاری کنید، مقدار پیش‌فرض را None تنظیم کنید.
<br><br>
اما وقتی می‌خواهید یک پارامتر پرس‌وجو را اجباری کنید، می‌توانید فقط هیچ مقدار پیش‌فرضی را مشخص نکنید:
<br><br>
در اینجا پارامتر query **needy** یک پارامتر پرس‌وجوی اجباری از نوع str است.
<br><br>
اگر در مرورگر خود URL زیر را باز کنید:

```python
http://127.0.0.1:8000/items/foo-item
```
...بدون افزودن پارامتر اجباری needy, یک خطا مانند زیر مشاهده خواهید کرد:

```python
{
  "detail": [
    {
      "type": "missing",
      "loc": [
        "query",
        "needy"
      ],
      "msg": "Field required",
      "input": null,
      "url": "https://errors.pydantic.dev/2.1/v/missing"
    }
  ]
}
```
<br><br>

از آنجا که needy یک پارامتر اجباری است، باید آن را در URL تنظیم کنید:
<br><br>
```python
http://127.0.0.1:8000/items/foo-item?needy=sooooneedy
```
...این کار خواهد کرد:
<br><br>
```python
{
    "item_id": "foo-item",
    "needy": "sooooneedy"
}
```
و البته، شما می‌توانید برخی از پارامترها را اجباری، برخی را با یک مقدار پیش‌فرض و برخی را به طور کامل اختیاری تعریف کنید:
<br><br>
python 3.10+
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_user_item(
    item_id: str, needy: str, skip: int = 0, limit: int | None = None
):
    item = {"item_id": item_id, "needy": needy, "skip": skip, "limit": limit}
    return item
```
در این حالت، 3 پارامتر query وجود دارد:
* needy: یک رشته ضروری
* skip: یک عدد صحیح با مقدار پیش‌فرض 0
* limit: یک عدد صحیح اختیاری

>[!TIP]
> همچنین می‌توانید از Enums به همان روشی که با Path Parameters استفاده می‌کنید استفاده کنید.














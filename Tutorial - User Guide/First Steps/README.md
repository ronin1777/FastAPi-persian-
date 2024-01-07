# قدم های اول
ساده‌ترین فایل FastAPI ممکن است به این شکل باشد:<br><br>
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```
کد را در فایلی به نام main.py کپی کنید.<br><br>
لایو سرور را اجرا کنید:<br><br>
```bash
$ uvicorn main:app --reload

INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [28720]
INFO:     Started server process [28722]
INFO:     Waiting for application startup.
INFO:     Application startup complete.

```
>[!NOTE]
> دستور uvicorn main:app به موارد زیر اشاره دارد:
> * main: فایل main.py (ماژول Python).
> * app: شیء ایجاد شده در داخل main.py با خط app = FastAPI().
> * --reload: باعث راه اندازی مجدد سرور پس از تغییرات کد می شود. فقط در زمان توسعه استفاده کنید.
-------
در خروجی، یک خط با چیزی شبیه به این وجود دارد:<br><br>
```python
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```
آن خط نشان دهنده URL مکانی است که برنامه شما در ماشین محلی شما میزبانی می شود.<br><br>

# ان را چک کنید
مرورگر را در این اینجا باز کنید<br><br>
شما پاسخ JSON زیر را مشاهده خواهید کرد:<br><br>
```python
{"message": "Hello World"}
```
# مستندات تعاملی API
اکنون به http://127.0.0.1:8000/docs بروید.<br><br>
شما مستندات تعاملی API خودکار را خواهید دید (که توسط Swagger UI ارائه می شود):<br><br>
<img src="https://fastapi.tiangolo.com/img/index/index-01-swagger-ui-simple.png"><br><br>
# مستندات جایگزین API
و اکنون، به http://127.0.0.1:8000/redoc بروید.<br><br>

شما مستندات جایگزین خودکار (که توسط ReDoc ارائه می شود) را خواهید دید:<br><br>
<img src="https://fastapi.tiangolo.com/img/index/index-02-redoc-simple.png"><br><br>
# OpenAPI
FastAPI یک "طرح" را با تمام API شما با استفاده از استاندارد OpenAPI برای تعریف API ها ایجاد می کند.

## "Schema"
یک "Schema" تعریف یا توصیف چیزی است. نه کدی که آن را پیاده سازی می کند، بلکه فقط یک توصیف انتزاعی.

## API "schema"
در این مورد، OpenAPI یک مشخصه ای است که نحوه تعریف طرح API شما را مشخص می کند.
این تعریف طرح شامل مسیرهای API شما، پارامترهای احتمالی آنها است.

## طرح داده
اصطلاح "طرح" ممکن است به شکل برخی داده ها مانند محتوای JSON نیز اشاره کند.<br><br>
در این صورت، به معنای ویژگی های JSON، نوع داده آنها و غیره خواهد بود.

## OpenAPI و JSON Schema
OpenAPI یک طرح API برای API شما تعریف می کند. و آن طرح شامل تعاریف (یا "طرح") داده های ارسال شده و دریافت شده توسط API شما با استفاده از JSON Schema، استاندارد برای طرح های داده JSON است.

## بررسی کنید openapi.json
اگر کنجکاو هستید که طرح OpenAPI خام چگونه به نظر می رسد، FastAPI به طور خودکار یک JSON (طرح) با توضیحات تمام API شما ایجاد می کند.
برای دیدن آن مستقیماً به http://127.0.0.1:8000/openapi.json بروید.<br><br>
این یک فایل JSON خواهد بود که با چیزی شبیه به زیر شروع می شود:<br><br>
```javascript
{
    "openapi": "3.1.0",
    "info": {
        "title": "FastAPI",
        "version": "0.1.0"
    },
    "paths": {
        "/items/": {
            "get": {
                "responses": {
                    "200": {
                        "description": "Successful Response",
                        "content": {
                            "application/json": {



...

```
# OpenAPI برای چیست
طرح OpenAPI چیزی است که دو سیستم مستندات تعاملی موجود را تقویت می کند.<br><br>
ده ها جایگزین وجود دارد که همگی بر اساس OpenAPI هستند. شما به راحتی می توانید هر یک از این جایگزین ها را به برنامه خود اضافه کنید که با FastAPI ساخته شده است.<br><br>
شما همچنین می توانید از آن برای تولید کد به طور خودکار استفاده کنید، برای مشتریانی که با API شما ارتباط برقرار می کنند. به عنوان مثال، برنامه های کاربردی front-end، تلفن همراه یا IoT.<br><br>

# بازخوانی، گام به گام

گام 1: import FastAPI
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```
FastAPI یک کلاس Python است که تمام قابلیت های API شما را فراهم می کند.
>[!TIP]
> FastAPI یک کلاس است که مستقیماً از Starlette ارث می برد.
> می توانید تمام قابلیت های Starlette را با FastAPI نیز استفاده کنید.
----------
# گام 2: ایجاد یک "نمونه" FastAPI

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```
<br><br>
در اینجا، متغیر app یک "نمونه" از کلاس FastAPI خواهد بود.<br><br>
این نقطه اصلی تعامل برای ایجاد کل API شما خواهد بود.<br><br>
این app همان موردی است که توسط uvicorn در دستور زیر اشاره شده است:

```bash
uvicorn main:app --reload

INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```
اگر برنامه خود را مانند زیر ایجاد کنید:
<br><br>
```python
from fastapi import FastAPI

my_awesome_api = FastAPI()


@my_awesome_api.get("/")
async def root():
    return {"message": "Hello World"}
```
<br><br>

و اگر آن را در یک فایل main.py قرار دهید، سپس uvicorn را مانند زیر فراخوانی خواهید کرد:
<br><br>
```bash
uvicorn main:my_awesome_api --reload

INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```
<br><br>

# گام 3: ایجاد یک عملیات مسیر¶

### مسیر
در اینجا، "مسیر" به آخرین قسمت URL اشاره دارد که از اولین / شروع می شود.
در یک URL مانند:
```python
https://example.com/items/foo
```
<br><br>
مسیر "/items/foo" خواهد بود.
```python
/items/foo
```

> [!TIP]
> یک "path" همچنین به طور معمول به عنوان "endpoint" یا "route" نامیده می شود.
--------
در هنگام ساخت یک API، "path" روش اصلی برای جدا کردن "نگرانی ها" و "منابع" است.

## عملیات 
"عملیات" در اینجا به یکی از "روشهای" HTTP اشاره دارد.<br><br>
یکی از:
* POST
* GET
* PUT
* DELETE

و موارد عجیب و غریب تر:

* OPTIONS
* HEAD
* PATCH
* TRACE

----
در پروتکل HTTP، می توانید با استفاده از یکی (یا چند مورد) از این "روشها" با هر مسیر ارتباط برقرار کنید.
<br><br>
هنگام ایجاد API ها، معمولاً از این روشهای HTTP خاص برای انجام یک عمل خاص استفاده می شود.

معمولاً از موارد زیر استفاده می کنید:

* POST: برای ایجاد داده.
* GET: برای خواندن داده.
* PUT: برای به روز رسانی داده.
* DELETE: برای حذف داده.
-----
بنابراین، در OpenAPI، هر یک از روشهای HTTP "عملیات" نامیده می شوند.<br><br>
ما نیز آنها را "عملیات" می نامیم.
<br><br>

## تعریف دکوراتور عملیات مسیر

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```
دکوراتور @app.get("/") به FastAPI می گوید که تابع زیر در حال رسیدگی به درخواست هایی است که به مسیر "/" می روند.
<br><br>
* مسیر /
* از اوپراتور get استفاده شده
------
>[!NOTE]
> در پایتون، این سینتکس "@something" یک "دکوراتور" نامیده می شود.<br>
> شما آن را بالای یک تابع قرار می دهید. مثل یک کلاه تزئینی زیبا (فکر می کنم این اصطلاح از اینجا آمده است).
> یک "دکوراتور" تابع زیر را می گیرد و با آن کاری می کند.
> در مورد ما، این دکوراتور به FastAPI می گوید که تابع زیر با مسیر / با عملیات get مطابقت دارد.
> این دکوراتور "عملیات مسیر" است.

شما همچنین میتوانید از دیگر اپراتورها استفاده کنید:
* @app.post()
* @app.put()
* @app.delete()
---
و موارد عجیب و غریب تر:

* @app.options()
* @app.head()
* @app.patch()
* @app.trace()

>[!TIP]
> شما آزاد هستید تا هر عملیات (روش HTTP) را که می خواهید استفاده کنید. FastAPI هیچ معنای خاصی را تحمیل نمی کند. اطلاعات اینجا به عنوان یک راهنمایی ارائه شده است، نه یک الزامات. به عنوان مثال، هنگام استفاده از GraphQL به طور معمول تمام عملیات ها را با استفاده از عملیات های POST انجام می دهید.
-------
# گام 4: تعریف عملکرد عملیات مسیر
این تابع عملیات مسیر ما است:
<br><br>
* مسیر: / است.
* عملیات: get است.

تابع: تابعی است که در زیر دکوراتور قرار دارد (زیر @app.get("/")).
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```
این یک تابع پایتون است.<br><br>
این تابع هر زمان که FastAPI درخواستی به URL "/" با استفاده از یک عملیات GET دریافت کند، فراخوانی می‌شود. 
<br><br>
 در این مورد، این یک تابع async است.<br><br>
همچنین می‌توانید آن را به جای یک تابع async def به عنوان یک تابع معمولی تعریف کنید:<br><br>
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def root():
    return {"message": "Hello World"}
```

>[!NOTE]
> اگر تفاوت را نمی‌دانید، به «Async: In a hurry?» مراجعه کنید.

----

# گام 5: محتوا را برگردانید<br><br>
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```
شما می‌توانید یک دیکشنری، لیست، مقادیر مفرد مانند str، int و غیره را برگردانید.<br><br>
شما همچنین می‌توانید مدل‌های Pydantic را برگردانید (در مورد آن بعداً بیشتر خواهید آموخت).
بسیاری از اشیا و مدل‌های دیگر نیز به‌طور خودکار به JSON تبدیل می‌شوند (از جمله ORMها و غیره).<br><br>
از مورد علاقه‌های خود استفاده کنید، احتمال زیادی وجود دارد که از قبل پشتیبانی شوند.<br><br>

# جمع بندی
* FastAPI را ایمپورت کنید
* یک نمونه از app بسازید
* یک دکوراتور عملیات مسیر بنویسید (مانند @app.get("/")). 
* یک تابع عملیات مسیر بنویسید (مانند def root(): ... بالا).
*  سرور توسعه را اجرا کنید (مانند uvicorn main:app --reload).



























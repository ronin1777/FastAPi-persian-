# Request Body
<br><br>
هنگامی که نیاز دارید داده‌ها را از یک client (مثلاً یک مرورگر) به API خود ارسال کنید، آن را به عنوان یک درخواست بدنه ارسال می‌کنید.
<br><br>
یک بدنه درخواست داده‌هایی است که توسط client به API شما ارسال می‌شود. بدنه پاسخ داده‌هایی است که API شما به client ارسال می‌کند.
<br><br>
API شما تقریباً همیشه باید یک بدنه پاسخ ارسال کند. اما client لزوماً همیشه نیازی به ارسال بدنه درخواست ندارند.
<br><br>
برای اعلام یک بدنه درخواست، از مدل‌های Pydantic با تمام قدرت و مزایای آنها استفاده می‌کنید.
<br><br>
>[!NOTE]
> برای ارسال داده، باید از یکی از روش‌های زیر استفاده کنید: POST (متداول‌تر)، PUT، DELETE یا PATCH.
> ارسال بدنه با درخواست GET در مشخصات رفتاری تعریف نشده دارد، با این حال، FastAPI از آن فقط برای موارد استفاده بسیار پیچیده/حاد پشتیبانی می‌کند.
> از آنجایی که توصیه نمی‌شود، مستندات تعاملی با Swagger UI هنگام استفاده از GET، مستندات مربوط به بدنه را نمایش نمی‌دهد و پروکسی‌های میانی ممکن است از آن پشتیبانی نکنند.


# Pydantic's BaseModel را ایمپورت کن

اول شما نیاز دارید که Basemodel را از pydantic ایمپورت کنید:
python 3.10+
<br><br>

```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.post("/items/")
async def create_item(item: Item):
    return item
```

# مدل دیتای خود را ایجاد کنید

<br><br>
سپس، مدل داده خود را به عنوان یک کلاس تعریف می‌کنید که از BaseModel ارث می‌برد.

از انواع استاندارد Python برای همه ویژگی‌ها استفاده کنید:
<br><br>
python 3.10+
<br><br>
```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.post("/items/")
async def create_item(item: Item):
    return item
```
درست مانند زمانی که پارامترهای پرس‌وجو را اعلام می‌کنید، هنگامی که یک ویژگی مدل دارای مقدار پیش‌فرض است، اجباری نیست. در غیر این صورت، ضروری است.
<br><br>
از None برای اختیاری کردن آن استفاده کنید.
<br><br>

برای مثال، این مدل فوق یک شیء JSON (یا یک dict Python) مانند زیر را اعلام می‌کند:
<br><br>
```python
{
    "name": "Foo",
    "description": "An optional description",
    "price": 45.2,
    "tax": 3.5
}
```
<br><br>
...از آنجایی که description و tax اختیاری هستند (با مقدار پیش‌فرض None)، این شیء JSON نیز معتبر خواهد بود:

```python
{
    "name": "Foo",
    "price": 45.2
}
```
<br><br>
# بعنوان یک ان را اعلام کنید
برای افزودن آن به عملیات مسیر خود، آن را به همان روشی که پارامترهای مسیر و query را اعلام کرده اید، اعلام کنید:

<br><br>
python 3.10+
```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.post("/items/")
async def create_item(item: Item):
    return item
```
...و نوع آن را به عنوان مدلی که ایجاد کرده اید، Item اعلام کنید.
<br><br>
# نتیجه ها
با همین اعلام نوع Python، FastAPI این کار را انجام خواهد داد:

* بدنه درخواست را به عنوان JSON بخواند.
* انواع مربوطه را تبدیل کند (در صورت نیاز).
* داده ها را اعتبارسنجی کند.
  * اگر داده ها نامعتبر باشند، یک خطا واضح و روشن را برمی‌گرداند که دقیقاً محل و نوع داده غلط را نشان می‌دهد.
* داده‌های دریافتی را در پارامتر item به شما بدهد.
  * از آنجا که آن را در تابع به عنوان نوع Item اعلام کردید، همچنین از پشتیبانی ویرایشگر (تکمیل و غیره) برای همه ویژگی‌ها و نوع آنها نیز برخوردار خواهید بود.
* پلان‌های JSON را برای مدل خود تولید کنید. همچنین اگر برای پروژه شما مناسب است می‌توانید از آنها در هر مکان دیگری استفاده کنید.
* این طرح‌ها بخشی از طرح OpenAPI تولید شده خواهند بود و توسط رابط‌های کاربری مستندات خودکار استفاده خواهند شد.

# مستندات خودکار
<br><br>

طرح‌های JSON مدل‌های شما بخشی از طرح OpenAPI تولید شده شما خواهند بود و در مستندات API تعاملی نشان داده خواهند شد:
<br><br>
<img src="https://fastapi.tiangolo.com/img/tutorial/body/image01.png"><br><br>

و همچنین در مستندات API داخل هر عملیات مسیری که نیاز به آنها دارد استفاده خواهند شد:

<img src="https://fastapi.tiangolo.com/img/tutorial/body/image02.png"><br><br>

# پشتیبانی ویرایشگر
<br><br>
در ویرایشگر شما، داخل تابع خود، شما به نوع‌های پیشنهادی و تکمیل‌ها در همه جا خواهید رسید (این اتفاق نمی‌افتد اگر به جای یک مدل Pydantic یک dict دریافت کنید):

<img src="https://fastapi.tiangolo.com/img/tutorial/body/image03.png"><br><br>
شما همچنین بررسی‌های خطا را برای عملیات نوع اشتباه دریافت می‌کنید:

<img src="https://fastapi.tiangolo.com/img/tutorial/body/image04.png"><br><br>

این اتفاقی نیست، کل فریم‌ورک حول این طراحی ساخته شده است.
<br><br>

و در مرحله طراحی، قبل از هرگونه پیاده‌سازی، به‌طور کامل آزمایش شد تا اطمینان حاصل شود که با همه ویرایشگرها کار می‌کند.
حتی برخی تغییرات در Pydantic خود برای پشتیبانی از این موضوع ایجاد شد.
<br><br>
تصاویر قبلی با Visual Studio Code گرفته شده است.

اما شما می‌توانید همان پشتیبانی ویرایشگر را با PyCharm و اکثر ویرایشگرهای Python دیگر دریافت کنید:

<br><br>
<img src="https://fastapi.tiangolo.com/img/tutorial/body/image05.png"><br><br>

>[!TIP]
> اگر از PyCharm به عنوان ویرایشگر خود استفاده می‌کنید، می‌توانید از Pydantic PyCharm Plugin استفاده کنید.
> این افزونه پشتیبانی ویرایشگر برای مدل‌های Pydantic را با موارد زیر بهبود می‌بخشد:
> * تکمیل خودکار
> * بررسی‌های نوع
> * تغییر ساختار
> * جست‌وجو
> * بازرسی‌ها
<br><br>

# از مدل استفاده کن
<br><br>

در داخل تابع، می‌توانید به همه ویژگی‌های شیء مدل به‌طور مستقیم دسترسی داشته باشید:

```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.post("/items/")
async def create_item(item: Item):
    item_dict = item.dict()
    if item.tax:
        price_with_tax = item.price + item.tax
        item_dict.update({"price_with_tax": price_with_tax})
    return item_dict
```
<br><br>
# در خواست body + پارامترهای مسیر
می‌توانید همزمان پارامترهای مسیر و بدنه درخواست را اعلام کنید.

FastAPI می‌تواند تشخیص دهد که پارامترهای تابعی که با پارامترهای مسیر مطابقت دارند باید از مسیر گرفته شوند و پارامترهای تابعی که به عنوان مدل‌های Pydantic اعلام شده‌اند باید از بدنه درخواست گرفته شوند.

```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.put("/items/{item_id}")
async def create_item(item_id: int, item: Item):
    return {"item_id": item_id, **item.dict()}
```
<br><br>

# درخواست body + مسیر + پارامترهای query
همچنین می‌توانید بدنه، مسیر و پارامترهای query را همزمان اعلام کنید.
<br><br>
python 3.10+
```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.put("/items/{item_id}")
async def create_item(item_id: int, item: Item, q: str | None = None):
    result = {"item_id": item_id, **item.dict()}
    if q:
        result.update({"q": q})
    return result
```
<br><br>
پارامترهای تابع به شرح زیر تشخیص داده می‌شوند:
* اگر پارامتر همچنین در مسیر اعلام شده باشد، به عنوان پارامتر مسیر استفاده خواهد شد.
* اگر پارامتر از نوع مفرد باشد (مانند int، float، str، bool و غیره)، به عنوان پارامتر query تفسیر خواهد شد.
* اگر پارامتر به عنوان نوع مدل Pydantic اعلام شود، به عنوان بدنه درخواست تفسیر خواهد شد.
>[!NOTE]
> FastAPI متوجه خواهد شد که مقدار q مورد نیاز نیست زیرا مقدار پیش فرض آن None است.
> اتحاد در Union[str, None] توسط FastAPI استفاده نمی‌شود، اما به ویرایشگر شما اجازه می‌دهد تا پشتیبانی بهتری به شما بدهد و خطاها را شناسایی کند.

# بدون Pydantic
اگر نمی‌خواهید از مدل‌های Pydantic استفاده کنید، همچنین می‌توانید از پارامترهای Body استفاده کنید. برای اطلاعات بیشتر به مستندات Body - Multiple Parameters: Singular values in body مراجعه کنید.




























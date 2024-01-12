# بدنه - مدل‌های تودرتو

با FastAPI، می‌توانید مدل‌هایی را با عمق نامحدود تعریف، اعتبارسنجی، مستندسازی و استفاده کنید (به لطف Pydantic).

<br><br>
# فیلدهای لیست
<br><br>
شما می‌توانید یک ویژگی را به عنوان یک زیرنوع تعریف کنید. به عنوان مثال، یک لیست Python.
<br><br>
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: list = []


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```
این باعث می‌شود که tags یک لیست باشد، اگرچه نوع عناصر لیست مشخص نشده است.
<br><br>
# فیلدهای لیست با نوع پارامتر
اما Python یک روش خاص برای تعریف لیست‌ها با انواع داخلی، یا "پارامترهای نوع" دارد.

<br><br>
# list را از typing وارد کنید

<br><br>
در پایتون 3.9 و بالاتر می‌توانید از استاندارد list برای تعریف این انتزاعات نوع استفاده کنید که در ادامه خواهیم دید. 💡
<br><br>
اما در نسخه‌های پایتون قبل از 3.9 (3.6 و بالاتر)، ابتدا باید List را از typing ماژول استاندارد پایتون وارد کنید:
```python
from typing import List, Union

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: Union[str, None] = None
    price: float
    tax: Union[float, None] = None
    tags: List[str] = []


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```
<br><br>
# اعلان یک لیست با یک پارامتر نوع
<br><br>
برای تعریف انواعی که دارای پارامترهای نوع (انواع داخلی) مانند list، dict، tuple هستند:
* اگر از نسخه پایتون پایین‌تر از 3.9 استفاده می‌کنید، نسخه معادل آنها را از ماژول typing وارد کنید
* پارامترهای داخلی را به عنوان "پارامترهای نوع" با استفاده از براکت‌ها: [ and ] ارسال کنید
<br><br>

در python3.9 باید به این شکل باشد:
```python
my_list: list[str]
```
<br><br>
و در ورژن های قبل:
```python
from typing import List

my_list: List[str]
```
<br><br>
این همان نحو استاندارد پایتون برای اعلان انواع است.
از همان نحو استاندارد برای ویژگی‌های مدل با انواع داخلی استفاده کنید.
بنابراین، در مثال ما، می‌توانیم tags را به طور خاص "لیست رشته" کنیم:


```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: list[str] = []


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```
<br><br>
# انواع مجموعه
اما بعداً فکر کردیم و متوجه شدیم که برچسب ها نباید تکرار شوند، آنها احتمالاً رشته های منحصر به فرد خواهند بود.
و پایتون یک نوع داده خاص برای مجموعه های موارد منحصر به فرد، set دارد.
سپس می توانیم tags را به عنوان مجموعه ای از رشته ها اعلام کنیم:


<br><br>
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```
با این کار، حتی اگر درخواستی با داده های تکراری دریافت کنید، به یک مجموعه از موارد منحصر به فرد تبدیل خواهد شد.
و هر بار که این داده ها را خروجی می دهید، حتی اگر منبع دارای تکرار باشد، به عنوان یک مجموعه از موارد منحصر به فرد خروجی داده می شود.
و همچنین به طور مناسب توضیح داده خواهد شد.


<br><br>
# مدل های تو در تو
هر ویژگی یک مدل Pydantic دارای یک نوع است.
<br><br>
 اما آن نوع می تواند خود یک مدل Pydantic دیگر باشد
<br><br>
بنابراین، شما می توانید "اشیاء" JSON عمیقا تودرتو را با نام‌های ویژگی خاص، انواع و اعتبارسنجی‌ها اعلام کنید. همه آن، به طور دلخواه تودرتو شده است.


<br><br>
# تعریف یک زیرمدل
به عنوان مثال، ما می توانیم یک مدل Image تعریف کنیم:

<br><br>
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Image(BaseModel):
    url: str
    name: str


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    image: Image | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```
<br><br>
# استفاده از زیرمدل به عنوان یک نوع
و سپس می‌توانیم از آن به عنوان نوع یک ویژگی استفاده کنیم:


<br><br>
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Image(BaseModel):
    url: str
    name: str


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    image: Image | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```
<br><br>
این بدان معنا است که FastAPI انتظار بدنی مشابه با زیر را دارد:


```python
{
    "name": "Foo",
    "description": "The pretender",
    "price": 42.0,
    "tax": 3.2,
    "tags": ["rock", "metal", "bar"],
    "image": {
        "url": "http://example.com/baz.jpg",
        "name": "The Foo live"
    }
}
```
<br><br>
باز هم، فقط با انجام آن اعلان، با FastAPI شما دریافت می کنید:
* شتیبانی ویرایشگر (تکمیل، غیره)، حتی برای مدل های تودرتو
* تبدیل داده ها
* اعتبار سنجی داده
* مستندات خودکار

<br><br>
# انواع خاص و اعتبارسنجی
علاوه بر انواع مفرد معمولی مانند str، int، float، و غیره، می‌توانید از انواع مفرد پیچیده‌تری استفاده کنید که از str به ارث می‌برند.

<br><br>
برای دیدن همه گزینه‌های موجود، مستندات انواع عجیب و غریب Pydantic را بررسی کنید. در فصل بعدی چند نمونه خواهید دید.
For example, as in the Image model we have a url field, we can declare it to be an instance of Pydantic's HttpUrl instead of a str:


<br><br>
```python
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()


class Image(BaseModel):
    url: HttpUrl
    name: str


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    image: Image | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```
رشته بررسی خواهد شد تا یک URL معتبر باشد، و در طرح JSON / OpenAPI به همین ترتیب مستند خواهد شد.

<br><br>
# ویژگی‌هایی با لیست‌های زیرمدل‌ها
همچنین می‌توانید از مدل‌های Pydantic به عنوان زیرنوع‌های لیست، مجموعه و غیره استفاده کنید.

<br><br>
```python
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()


class Image(BaseModel):
    url: HttpUrl
    name: str


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    images: list[Image] | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```
<br><br>
این انتظار دارد که بدن JSON مانند زیر باشد (تبدیل، اعتبارسنجی، مستندات، و غیره):

```python
{
    "name": "Foo",
    "description": "The pretender",
    "price": 42.0,
    "tax": 3.2,
    "tags": [
        "rock",
        "metal",
        "bar"
    ],
    "images": [
        {
            "url": "http://example.com/baz.jpg",
            "name": "The Foo live"
        },
        {
            "url": "http://example.com/dave.jpg",
            "name": "The Baz"
        }
    ]
}
```
<br><br>
>[!INFO]
> توجه داشته باشید که کلید images اکنون یک لیست از اشیاء تصویر دارد.


<br><br>
# مدل‌های تودرتو عمیق
می‌توانید مدل‌های تودرتو را تا هر عمق دلخواهی تعریف کنید.

<br><br>
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()


class Image(BaseModel):
    url: HttpUrl
    name: str


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    images: list[Image] | None = None


class Offer(BaseModel):
    name: str
    description: str | None = None
    price: float
    items: list[Item]


@app.post("/offers/")
async def create_offer(offer: Offer):
    return offer
<br><br>
>[!NOTE]
> توجه داشته باشید که Offer یک لیست از Items دارد که به نوبه خود یک لیست اختیاری از Images دارد


<br><br>
# بدن‌های لیست‌های خالص 
اگر مقدار سطح بالای بدن JSON مورد انتظار شما یک آرایه JSON (یک لیست Python) باشد، می‌توانید نوع آن را در پارامتر تابع اعلام کنید، مانند مدل‌های Pydantic:
<br><br>
images: List[Image]
<br><br>
یا در PYTHON3.9 به بالا:
```python
images: list[Image]
```
<br><br>
مانند زیر:

```python
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()


class Image(BaseModel):
    url: HttpUrl
    name: str


@app.post("/images/multiple/")
async def create_multiple_images(images: list[Image]):
    return images
```
<br><br>
# پشتیبانی ویرایشگر همه جا
شما پشتیبانی ویرایشگر را همه جا دریافت می‌کنید.
حتی برای اقلام داخل لیست ها:
<br><br>
<img src="https://fastapi.tiangolo.com/img/tutorial/body-nested-models/image01.png">
<br><br>
این نوع پشتیبانی ویرایشگر را نمی‌توانید اگر مستقیماً با dict به جای مدل‌های Pydantic کار می‌کنید، دریافت کنید.
اما لازم نیست نگران آنها هم باشید، dictهای ورودی به‌طور خودکار تبدیل می‌شوند و خروجی شما نیز به‌طور خودکار به JSON تبدیل می‌شود.


<br><br>
# بدن های دیکشنری های دلخواه
شما همچنین می توانید بدن را به عنوان یک دیکشنری با کلیدهای نوعی و مقادیر نوعی دیگر اعلام کنید.

<br><br>
به این ترتیب، لازم نیست قبلاً بدانید که نام های فیلد/ویژگی صحیح چیست (مانند مورد Pydantic Models).
این برای زمانی مفید است که بخواهید کلیدهایی را دریافت کنید که از قبل نمی دانید.
--------------------------
<br><br>
یک مورد استفاده مفید دیگر زمانی است که می خواهید کلیدهای دیگری داشته باشید (مثلاً int).

<br><br>
این چیزی است که ما در اینجا خواهیم دید.
در این حالت، شما هر dict را قبول می کنید تا زمانی که کلیدهای int با مقادیر float داشته باشد:


<br><br>
```python
from fastapi import FastAPI

app = FastAPI()


@app.post("/index-weights/")
async def create_index_weights(weights: dict[int, float]):
    return weights
```
<br><br>
>[!TIP]
> توجه داشته باشید که JSON فقط از str به عنوان کلید پشتیبانی می کند.
> اما Pydantic دارای تبدیل داده خودکار است.
> این بدان معناست که، حتی اگر کلاینت های API شما فقط می توانند رشته ها را به عنوان کلید ارسال کنند، تا زمانی که آن رشته ها اعداد صحیح خالص باشند، Pydantic آنها را تبدیل و اعتبارسنجی خواهد کرد.
و dict ای که شما به عنوان weights دریافت می کنید در واقع دارای کلیدهای int و مقادیر float خواهد بود.<



<br><br>
# خلاصه مطالب
با FastAPI شما انعطاف پذیری حداکثر را با استفاده از مدل های Pydantic در اختیار دارید، در حالی که کد شما ساده، کوتاه و شیک باقی می ماند.
اما با تمام مزایا:

<br><br>
* پشتیبانی ویرایشگر (تکمیل همه جا!)
* تبدیل داده (که به آن تجزیه / سریال سازی نیز گفته می شود)
* اعتبارسنجی داده
* مستندات طرح
* مستندات خودکار



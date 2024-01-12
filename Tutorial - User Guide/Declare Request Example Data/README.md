# اعلام داده های نمونه درخواست
می‌توانید نمونه‌هایی از داده‌هایی را که برنامه شما می‌تواند دریافت کند اعلام کنید.

اینجا چند راه برای انجام ان است.
<br><br>
# اطلاعات اضافی JSON Schema در مدل های Pydantic

شما می توانید نمونه هایی برای یک مدل Pydantic اعلام کنید که به JSON Schema تولید شده اضافه می شود.


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

    model_config = {
        "json_schema_extra": {
            "examples": [
                {
                    "name": "Foo",
                    "description": "A very nice Item",
                    "price": 35.4,
                    "tax": 3.2,
                }
            ]
        }
    }


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```
<br><br>
آن اطلاعات اضافی به صورت عادی به JSON Schema خروجی برای آن مدل اضافه می‌شود و در مستندات API مورد استفاده قرار می‌گیرد.
<br><br>
در نسخه 2 Pydantic، شما از ویژگی model_config استفاده می‌کنید که یک dict را به عنوان توضیح داده شده در اسناد Pydantic: Model Config می‌پذیرد.

<br><br>
می‌توانید "json_schema_extra" را با یک "dict" حاوی هرگونه داده اضافی که می‌خواهید در JSON Schema تولید شده نمایش داده شود، از جمله "examples" تنظیم کنید.


<br><br>
>[!TIP]
> شما می‌توانید از همان تکنیک برای گسترش JSON Schema و افزودن اطلاعات اضافی سفارشی خود استفاده کنید.
> به عنوان مثال می‌توانید از آن برای افزودن متادیتای رابط کاربری گرافیکی جلویی (Front-end) و غیره استفاده کنید.



<br><br>
>[!NOTE]
> OpenAPI 3.1.0 (که از نسخه 0.99.0 FastAPI استفاده شده است) از نمونه‌ها که بخشی از استاندارد JSON Schema است پشتیبانی می‌کند.
> JSON Schema یک فرمت استاندارد برای توصیف داده‌ها است که می‌تواند توسط ابزارهای مختلف برای مستندسازی، اعتبارسنجی و سایر موارد استفاده شود. OpenAPI 3.1.0 یک نسخه جدید از OpenAPI Spec است که شامل ویژگی‌های جدیدی مانند پشتیبانی از نمونه‌ها است.


<br><br>
# پارامترهای اضافی فیلد
هنگام استفاده از Field() با مدل‌های Pydantic، می‌توانید همچنین نمونه‌های اضافی را اعلام کنید:


<br><br>
```python
from fastapi import FastAPI
from pydantic import BaseModel, Field

app = FastAPI()


class Item(BaseModel):
    name: str = Field(examples=["Foo"])
    description: str | None = Field(default=None, examples=["A very nice Item"])
    price: float = Field(examples=[35.4])
    tax: float | None = Field(default=None, examples=[3.2])


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```
<br><br>
# مثال‌ها در JSON Schema - OpenAPI
<br><br>
وقتی از یکی از اینها استفاده میکنید:
<br><br>
* Path()
* Query()
* Header()
* Cookie()
* Body()
* Form()
* File()

شما می‌توانید گروهی از نمونه‌ها را با اطلاعات اضافی اعلام کنید که به JSON Schemaهای آنها در داخل OpenAPI اضافه می‌شود. این ویژگی می‌تواند برای مستندسازی دقیق‌تر داده‌ها و بهبود تجربه کاربر مفید باشد.

<br><br>
# body با examples
در اینجا ما examples را شامل یک نمونه از داده‌های انتظار در Body() می‌گذرانیم:

<br><br>
```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(
    item_id: int,
    item: Annotated[
        Item,
        Body(
            examples=[
                {
                    "name": "Foo",
                    "description": "A very nice Item",
                    "price": 35.4,
                    "tax": 3.2,
                }
            ],
        ),
    ],
):
    results = {"item_id": item_id, "item": item}
    return results
```
<br><br>
# مثال در رابط کاربری مستندات
با هر یک از روش‌های بالا، اینطور در /docs به نظر می‌رسد:


<br><br>
<img src="https://fastapi.tiangolo.com/img/tutorial/body-fields/image01.png">
<br><br>
# body با چند examples
البته شما میتوانید چندین examples ارسال کنید:
<br><br>
```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(
    *,
    item_id: int,
    item: Annotated[
        Item,
        Body(
            examples=[
                {
                    "name": "Foo",
                    "description": "A very nice Item",
                    "price": 35.4,
                    "tax": 3.2,
                },
                {
                    "name": "Bar",
                    "price": "35.4",
                },
                {
                    "name": "Baz",
                    "price": "thirty five point four",
                },
            ],
        ),
    ],
):
    results = {"item_id": item_id, "item": item}
    return results
```
<br><br>
هنگامی که این کار را انجام می دهید، نمونه ها بخشی از JSON Schema داخلی برای داده های آن بدنه خواهند بود.
<br><br>
با این حال، در زمان نوشتن این مقاله، Swagger UI، ابزاری که مسئول نمایش رابط کاربری مستندات است، از نمایش چند نمونه برای داده در JSON Schema پشتیبانی نمی کند. اما برای دور زدن این مشکل، در زیر بخوانید.

<br><br>
# نمونه‌های OpenAPI اختصاصی
قبل از اینکه JSON Schema از نمونه‌ها پشتیبانی کند، OpenAPI دارای یک فیلد متفاوت به نام examples برای تعریف نمونه‌های داده بود.
<br><br>
این نمونه‌های OpenAPI اختصاصی در بخش دیگری از مشخصات OpenAPI قرار می‌گیرند. این نمونه‌ها در جزئیات هر عملیات مسیر قرار دارند، نه در داخل هر JSON Schema.
<br><br>
Swagger UI مدتی است که این فیلد خاص examples را پشتیبانی می‌کند. بنابراین، می‌توانید از آن برای نمایش نمونه‌های مختلف در رابط کاربری مستندات استفاده کنید.
<br><br>
شکل این فیلد OpenAPI خاص examples یک دیکشنری با چند نمونه (به جای یک لیست) است، هر کدام با اطلاعات اضافی که به OpenAPI نیز اضافه خواهد شد.

<br><br>
این در داخل هر JSON Schema موجود در OpenAPI قرار نمی‌گیرد، بلکه در عملیات مسیر مستقیماً قرار دارد.
<br><br>
# استفاده از پارامتر openapi_examples
<br><br>
می‌توانید نمونه‌های اختصاصی OpenAPI را در FastAPI با استفاده از پارامتر openapi_examples برای:
<br><br>
* Path()
* Query()
* Header()
* Cookie()
* Body()
* Form()
* File()


<br><br>
کلیدهای دیکشنری نمونه‌ها هر نمونه را شناسایی می‌کنند و هر مقدار یک دیکشنری دیگر است.
<br><br>
هر دیکشنری نمونه خاص در نمونه‌ها می‌تواند حاوی موارد زیر باشد:
<br><br>
* summary: توضیح مختصر برای مثال
* description: توضیح طولانی که میتونه شامل متن marksown باشد
* مقدار: این مثال واقعی است که نمایش داده می‌شود، مانند یک دیکشنری.
* URL خارجی: جایگزینی برای مقدار، یک URL است که به مثال اشاره می‌کند. با این حال، ممکن است توسط بسیاری از ابزارها مانند مقدار پشتیبانی نشود.


<br><br>
شما میتوانید به این شکل استفاده کنید:

<br><br>
```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(
    *,
    item_id: int,
    item: Annotated[
        Item,
        Body(
            openapi_examples={
                "normal": {
                    "summary": "A normal example",
                    "description": "A **normal** item works correctly.",
                    "value": {
                        "name": "Foo",
                        "description": "A very nice Item",
                        "price": 35.4,
                        "tax": 3.2,
                    },
                },
                "converted": {
                    "summary": "An example with converted data",
                    "description": "FastAPI can convert price `strings` to actual `numbers` automatically",
                    "value": {
                        "name": "Bar",
                        "price": "35.4",
                    },
                },
                "invalid": {
                    "summary": "Invalid data is rejected with an error",
                    "value": {
                        "name": "Baz",
                        "price": "thirty five point four",
                    },
                },
            },
        ),
    ],
):
    results = {"item_id": item_id, "item": item}
    return results
```
<br><br>
# مثال‌های OpenAPI در رابط کاربری مستندات 
<br><br>
<img src="https://fastapi.tiangolo.com/img/tutorial/body-fields/image02.png">
<br><br>
# جزئیات فنی
>[!TIP]
> اگر از نسخه FastAPI 0.99.0 یا بالاتر استفاده می‌کنید، احتمالاً می‌توانید این جزئیات را بپرهیزید.
> آنها برای نسخه‌های قدیمی‌تر مرتبط هستند، قبل از اینکه OpenAPI 3.1.0 در دسترس باشد.
> شما می‌توانید این را یک درس کوتاه تاریخ OpenAPI و JSON Schema در نظر بگیرید. 🤓
<br><br>
>[!WARNING]
> اینها جزئیات بسیار فنی در مورد استانداردهای JSON Schema و OpenAPI هستند.
> اگر ایده‌های بالا برای شما جواب می‌دهند، ممکن است کافی باشد، و شما احتمالاً به این جزئیات نیاز ندارید، آزادانه آنها را پرش کنید.


<br><br>
قبل از OpenAPI 3.1.0، OpenAPI از یک نسخه قدیمی و اصلاح شده JSON Schema استفاده می‌کرد.

JSON Schema از مثال‌ها پشتیبانی نمی‌کرد، بنابراین OpenAPI فیلد مثال خود را به نسخه اصلاح شده خود اضافه کرد.

OpenAPI همچنین فیلدهای مثال و نمونه‌ها را به سایر قسمت‌های مشخصات اضافه کرد:

<br><br>
* پارامتر شیء (در مشخصات) که توسط FastAPI استفاده شده است
* Path()
* Query()
* Header()
* Cookie()

<br><br>
شیء بدن درخواست، در فیلد محتوا، روی شیء نوع رسانه (در مشخصات) که توسط FastAPI استفاده شد
* Body()
* File()
* Form()


<br><br>
>[!NOTE]
> پارامتر قدیمی examples در FastAPI 0.103.0 منسوخ شد و دیگر توصیه نمی شود. به جای آن باید از پارامتر openapi_examples استفاده شود.


<br><br>
# فیلد مثال‌های JSON Schema
<br><br>
با این حال، JSON Schema در نسخه جدیدی از مشخصات خود یک فیلد examples اضافه کرد.

<br><br>
و سپس OpenAPI 3.1.0 جدید بر اساس آخرین نسخه (JSON Schema 2020-12) ساخته شد که این فیلد جدید examples را شامل بود.

<br><br>
و اکنون این فیلد جدید examples بر روی فیلد قدیمی تک (و سفارشی) example ، که اکنون منسوخ شده است، اولویت دارد.

<br><br>
این فیلد جدید examples در JSON Schema فقط یک لیست از مثال ها است، نه یک dict با metadata اضافی مانند سایر مکان ها در OpenAPI (همانطور که در بالا توضیح داده شد).
<br><br>
>[!NOTE]
> پس از انتشار OpenAPI 3.1.0 با این ادغام جدید و ساده‌تر با JSON Schema، برای مدتی، Swagger UI، ابزاری که مستندات خودکار را ارائه می‌دهد، از OpenAPI 3.1.0 پشتیبانی نمی‌کرد (از نسخه 5.0.0 پشتیبانی می‌کند 🎉).
> به همین دلیل، نسخه‌های FastAPI قبل از 0.99.0 هنوز از نسخه‌های OpenAPI پایین‌تر از 3.1.0 استفاده می‌کردند.


<br><br>
# Pydantic و مثال‌های FastAPI
<br><br>
وقتی مثال‌ها را داخل یک مدل Pydantic اضافه می‌کنید، با استفاده از schema_extra یا Field(examples=["something"]) آن مثال به JSON Schema برای آن مدل Pydantic اضافه می‌شود.
و آن JSON Schema مدل Pydantic در OpenAPI API شما گنجانده شده است، و سپس در رابط کاربری مستندات استفاده می‌شود.
<br><br>
در نسخه‌های FastAPI قبل از 0.99.0 (0.99.0 و بالاتر از آن از OpenAPI 3.1.0 جدیدتر استفاده می‌کنند) هنگامی که از مثال یا مثال‌ها با هر یک از سایر ابزارهای کاربردی (Query(), Body(), و غیره) استفاده می‌کردید، آن مثال‌ها به JSON Schema که آن داده را توصیف می‌کند اضافه نمی‌شدند (حتی به نسخه JSON Schema OpenAPI)، آنها مستقیماً به اعلان عملیات مسیر در OpenAPI اضافه شدند (خارج از قسمت‌های OpenAPI که از JSON Schema استفاده می‌کنند).
<br><br>
اما اکنون که FastAPI 0.99.0 و بالاتر از آن از OpenAPI 3.1.0 استفاده می‌کند، که از JSON Schema 2020-12 استفاده می‌کند، و Swagger UI 5.0.0 و بالاتر، همه چیز هماهنگ‌تر است و مثال‌ها در JSON Schema گنجانده شده‌اند.

<br><br>
# Swagger UI و مثال‌های خاص OpenAPI
<br><br>
اکنون، همانطور که Swagger UI از چند مثال JSON Schema پشتیبانی نمی‌کرد (تا 26 آگوست 2023)، کاربران راهی برای نمایش چند مثال در مستندات نداشتند.
<br><br>
برای حل این مشکل، FastAPI 0.103.0 پشتیبانی از اعلام همان فیلد قدیمی مثال‌های خاص OpenAPI را با پارامتر جدید openapi_examples اضافه کرده است. 🤓
<br><br>
# خلاصه مطالب

<br><br>
من قبلاً می‌گفتم که تاریخ را زیاد دوست ندارم ... و حالا نگاه کن که من در حال حاضر در حال تدریس "تاریخ فناوری" هستم. 😅

<br><br>
در کوتاه‌ترین جمله، به FastAPI 0.99.0 یا بالاتر ارتقا دهید و همه چیز بسیار ساده‌تر، سازگارتر و بصری‌تر خواهد بود و شما مجبور نیستید تمام این جزئیات تاریخی را بدانید. 😎

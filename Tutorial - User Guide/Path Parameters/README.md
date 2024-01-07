# پارامترهای مسیر
شما می توانید پارامترهای مسیر را با همان نحوی که از رشته های قالب بندی پایتون استفاده می کنید، تعریف کنید: <br><br>
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id):
    return {"item_id": item_id}
```
مقدار پارامتر مسیر item_id به عنوان آرگومان item_id به تابع شما ارسال می شود.
<br><br>
بنابراین اگر این مثال را اجرا کنید و به http://127.0.0.1:8000/items/foo مراجعه کنید، پاسخی به صورت زیر مشاهده خواهید کرد:
<br><br>
```python
{"item_id":"foo"}
```
# پارامترهای مسیر با انواع داده
شما می‌توانید نوع داده یک پارامتر مسیر را در تابع، با استفاده از نوع‌دهی‌های استاندارد Python مشخص کنید:
<br><br>
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```
<br><br>
در این مورد، item_id به عنوان یک int اعلام شده است.
<br><br>
>[!NOTE]
> این به شما پشتیبانی ویرایشگر در داخل تابع شما، با بررسی خطاها، تکمیل و غیره می دهد.
-----
# تبدیل داده
<br><br>
اگر ما این مثال را اجرا و مرورگر را در این مسیر http://127.0.0.1:8000/items/3 باز کنید شما این پاسخ را میبینید:

```python
{"item_id":3}
```
>[!NOTE]
> توجه داشته باشید که مقداری که تابع شما دریافت کرده (و برگشته) 3 است، به عنوان یک عدد صحیح Python، نه یک رشته "3".
> بنابراین، با آن اعلان نوع، FastAPI به شما تجزیه درخواست "اتوماتیک" می دهد.
-----
# تأیید صحت داده‌ها

<br><br>

اما اگر شما در مرورگر به این مسیرhttp://127.0.0.1:8000/items/foo بروید شما یک ارور http زیبا خواهید دید:
```python
{
  "detail": [
    {
      "type": "int_parsing",
      "loc": [
        "path",
        "item_id"
      ],
      "msg": "Input should be a valid integer, unable to parse string as an integer",
      "input": "foo",
      "url": "https://errors.pydantic.dev/2.1/v/int_parsing"
    }
  ]
}
```
<br><br>

زیرا پارامتر مسیر item_id مقدار foo را داشت که یک int نیست. 
<br><br>
اگر به جای int یک float ارائه می‌دادید، همان خطا ظاهر می‌شد، همانطور که در http://127.0.0.1:8000/items/4.2:
<br><br>
>[!NOTE]
> "بنابراین، با همان اعلان نوع Python، FastAPI به شما داده‌های اعتبارسنجی می‌دهد.
> توجه داشته باشید که خطا همچنین به وضوح دقیقاً همان مکانی را که اعتبارسنجی انجام نشد بیان می‌کند.
> این بسیار مفید است در حالی که در حال توسعه و اشکال زدایی کدی هستید که با API شما تعامل دارد."
------
# مستندات
<br><br>

و زمانی که مرورگر خود را در آدرس http://127.0.0.1:8000/docs باز کنید، یک مستندات API خودکار و تعاملی مانند موارد زیر مشاهده خواهید کرد:
<br><br>
<img src="https://fastapi.tiangolo.com/img/tutorial/path-params/image01.png"><br><br>

>[!NOTE]
> "باز هم، فقط با استفاده از همان نوع اعلان Python، FastAPI به شما مستندات تعاملی خودکار (با ادغام Swagger UI) می دهد. توجه داشته باشید که پارامتر مسیر به عنوان یک عدد صحیح اعلام شده است."
----
<br><br>

# مزایا مبتنی بر استانداردها، مستندات جایگزین
<br><br>
"و از آنجا که طرح تولید شده از استاندارد OpenAPI است، ابزارهای زیادی با آن سازگار هستند.<br><br>
به همین دلیل، FastAPI خود یک مستندات API جایگزین (با استفاده از ReDoc) ارائه می دهد که می توانید به آن در http://127.0.0.1:8000/redoc دسترسی پیدا کنید: "
<br><br>

<img src="https://fastapi.tiangolo.com/img/tutorial/path-params/image02.png"><br><br>

به همین ترتیب، ابزارهای زیادی سازگار وجود دارند. از جمله ابزارهای تولید کد برای بسیاری از زبان ها.
<br><br>

# Pydantic
<br><br>
مام اعتبارسنجی داده ها توسط Pydantic در پشت صحنه انجام می شود، بنابراین شما از همه مزایای آن بهره می برید.
<br><br>
و شما می دانید که در دستان خوب هستید.
<br><br>
شما می توانید از همان نوع اعلان ها با str، float، bool و بسیاری از سایر انواع داده های پیچیده استفاده کنید.
<br><br>
برخی از این موارد در فصل های بعدی آموزش بررسی می شوند.

# ترتیب اهمیت دارد<br><br>

هنگام ایجاد مسیرهای عملیاتی، ممکن است موقعیت هایی را پیدا کنید که مسیر ثابتی دارید
<br><br>
مانند /users/me، فرض کنید برای دریافت اطلاعات در مورد کاربر فعلی است.
و سپس می توانید یک مسیر /users/{user_id} نیز داشته باشید تا اطلاعات مربوط به یک کاربر خاص را با ID کاربری خاص دریافت کنید.
<br><br>
به دلیل آنکه مسیرهای عملیاتی در ترتیب ارزیابی می شوند، باید اطمینان حاصل کنید که مسیر برای /users/me قبل از مسیر برای /users/{user_id} اعلام شده است.
<br><br>
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/users/me")
async def read_user_me():
    return {"user_id": "the current user"}


@app.get("/users/{user_id}")
async def read_user(user_id: str):
    return {"user_id": user_id}
```
<br><br>

در غیر این صورت، مسیر برای /users/{user_id} نیز برای /users/me مطابقت می یافت، "فکر می‌کرد" که در حال دریافت پارامتر user_id با مقدار "me" است.
<br><br>
به همین ترتیب، شما نمی توانید یک مسیر عملیاتی را مجددا تعریف کنید:
<br><br>
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/users")
async def read_users():
    return ["Rick", "Morty"]


@app.get("/users")
async def read_users2():
    return ["Bean", "Elfo"]
```
<br><br>
اولین مورد همیشه استفاده خواهد شد زیرا مسیر قبل از بقیه مطابقت دارد.

<br><br>
## مقدارهای از پیش تعریف شده
اگر یک مسیر عملیاتی دارید که یک پارامتر مسیر دریافت می کند، اما می خواهید مقادیر معتبر ممکن برای پارامتر مسیر از پیش تعریف شده باشند، می توانید از یک نماد استاندارد Python استفاده کنید(Enum).
<br><br>
## یک کلاس Enum بسازید
<br><br>
وارد کردن نماد و ایجاد زیر کلاسی که از str و Enum ارث می برد.<br><br>
با ارث بردن از str، API docs می تواند بداند که مقادیر باید از نوع رشته باشند و قادر به رندر صحیح خواهند بود.<br><br>
سپس کلاس ویژگی هایی با مقادیر ثابت ایجاد کنید که مقادیر معتبر موجود باشند:<br><br>
```python
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```
>[!NOTE]
> عرصه‌ها (یا enum) از نسخه 3.4 در Python موجود هستند.
------
>[!TIP]
> اگر کنجکاو هستید، "AlexNet"، "ResNet"، و "LeNet" فقط نام مدل‌های یادگیری ماشین هستند.
<br><br>

# اعلان پارامتر مسیر
<br><br>
سپس یک پارامتر مسیر با یک نوع انتزاعاتی با استفاده از کلاس enum ایجاد کنید که ایجاد کرده اید (ModelName):
<br><br>
```python
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```
<br><br>

# docs را چک کنید
<br><br>
از آنجا که مقادیر موجود برای پارامتر مسیر از پیش تعریف شده اند، اسناد تعاملی می توانند آنها را به زیبایی نشان دهند:

<img src="https://fastapi.tiangolo.com/img/tutorial/path-params/image03.png"><br><br>

# کار با enum های Python
<br><br>
مقدار پارامتر مسیر یک عضو enum خواهد بود.<br><br>

## مقایسه اعضای enum
می توانید آن را با عضو enum در enum ایجاد شده خود ModelName مقایسه کنید.**
<br><br>
```python
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```
<br><br>

# اخذ مقدار enum
<br><br>
می توانید مقدار واقعی (یک str در این مورد) را با استفاده از model_name.value یا به طور کلی your_enum_member.value بدست آورید:
<br><br>
```python
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```
>[!TIP]
> همچنین می توانید مقدار "lenet" را با ModelName.lenet.value دسترسی پیدا کنید.

# بازگرداندن اعضای enumeration
<br><br>
میتوانید اعضای شمارش را از عملیات مسیر خود بازگردانید، حتی اگر در یک بدنه JSON (مانند یک dict) قرار دارند.<br><br>

قبل از بازگشت به client، آنها به مقادیر متناظر خود (در این مورد رشته ها) تبدیل می شوند.<br><br>
```python
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```
<br><br>
در client خود یک پاسخ JSON مانند این دریافت خواهید کرد.
<br><br>
```python
{
  "model_name": "alexnet",
  "message": "Deep Learning FTW!"
}
```
<br><br>
# پارامترهای مسیر حاوی مسیرها
<br><br>
فرض کنید یک عملیات مسیر با مسیر /files/{file_path} دارید.
<br><br>
اما شما نیاز دارید که file_path خود حاوی یک مسیر باشد، مانند home/johndoe/myfile.txt.
بنابراین، URL آن فایل چیزی شبیه به این خواهد بود: <br><br>
/files/home/johndoe/myfile.txt.
<br><br>
# حمایت OpenAPI
<br><br>
OpenAPI راهی برای اعلام یک پارامتر مسیر برای حاوی یک مسیر داخلی ارائه نمی دهد، زیرا این امر می تواند منجر به سناریوهایی شود که دشوار است تست و تعریف شوند.
<br><br>
با این حال، شما هنوز هم می توانید این کار را در FastAPI انجام دهید، با استفاده از یکی از ابزارهای داخلی Starlette.<br><br>

و اسناد هنوز هم کار می کنند، اگرچه هیچ گونه مستنداتی اضافه نمی کنند که پارامتر باید حاوی یک مسیر باشد.
<br><br>
# مبدل مسیر
<br><br>
با استفاده از گزینه ای مستقیماً از Starlette، می توانید یک پارامتر مسیر حاوی یک مسیر را با استفاده از URL مانند:/files/{file_path:path} اعلام کنید.
<br><br>
در این حالت، نام پارامتر file_path است و قسمت آخر آن، :path به آن می گوید که پارامتر باید با هر مسیری مطابقت داشته باشد.
<br><br>
بنابراین، می توانید از آن با استفاده از کد زیر استفاده کنید:
<br><br>
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/files/{file_path:path}")
async def read_file(file_path: str):
    return {"file_path": file_path}
```
>[!TIP]
> شما ممکن است نیاز داشته باشید که پارامتر حاوی /home/johndoe/myfile.txt باشد، با یک اسلش پیشرو (/).
> در این حالت، URL اینگونه خواهد بود: /files//home/johndoe/myfile.txt, با دو اسلش (//) بین files و home.

# خلاصه مطالب
<br><br>
با FastAPI، با استفاده از اعلامات نوع Python کوتاه، شهودی و استاندارد، شما دریافت می کنید:

* پشتیبانی ویرایشگر: بررسی خطاها، تکمیل خودکار، و غیره.
* "تجزیه" داده
* اعتبارسنجی داده
* آشنایی با API و مستندات خودکار

و فقط باید آنها را یک بار اعلام کنید.

<br><br>

این احتمالاً مهمترین مزیت قابل مشاهده FastAPI در مقایسه با چارچوب های جایگزین (به غیر از عملکرد خام) است.








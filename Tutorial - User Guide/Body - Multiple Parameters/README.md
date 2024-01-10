# بدنه - پارامترهای چندگانه
<br><br>
اکنون که نحوه استفاده از Path و Query را آموخته‌ایم، بیایید کاربردهای پیشرفته‌تر اعلام بدنه درخواست را بررسی کنیم.

# ترکیب پارامترهای Path، Query و بدنه درخواست

در ابتدا، البته، شما می‌توانید آزادانه اعلام پارامترهای Path، Query و بدنه درخواست را ترکیب کنید و FastAPI می‌داند که چه کاری انجام دهد.
<br><br>
همچنین می‌توانید پارامترهای بدنه را به صورت اختیاری اعلام کنید، با تعیین مقدار پیش‌فرض None:

```python
from typing import Annotated

from fastapi import FastAPI, Path
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(
    item_id: Annotated[int, Path(title="The ID of the item to get", ge=0, le=1000)],
    q: str | None = None,
    item: Item | None = None,
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    if item:
        results.update({"item": item})
    return results
```
<br><br>

>[!NOTE]
> توجه داشته باشید که در این حالت، آیتمی که قرار است از بدنه گرفته شود اختیاری است. زیرا مقدار پیش فرض آن None است.

# پارامترهای بدنه چندگانه
 در مثال قبل، عملیات‌های مسیر انتظار دارند بدنه JSON با ویژگی‌های یک Item مانند زیر داشته باشد:
<br><br>
```python
{
    "name": "Foo",
    "description": "The pretender",
    "price": 42.0,
    "tax": 3.2
}
```
<br><br>
اما شما همچنین می‌توانید چندین پارامتر بدنه را اعلام کنید، به عنوان مثال item و user:
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


class User(BaseModel):
    username: str
    full_name: str | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item, user: User):
    results = {"item_id": item_id, "item": item, "user": user}
    return results
```
<br><br>
در این حالت، FastAPI متوجه می‌شود که بیش از یک پارامتر بدنه در تابع وجود دارد (دو پارامتر که مدل‌های Pydantic هستند). بنابراین، از نام‌های پارامتر به عنوان کلیدها (نام‌های فیلد) در بدنه استفاده می‌کند و انتظار دارد بدنه‌ای مانند موارد زیر داشته باشد:

```python
{
    "item": {
        "name": "Foo",
        "description": "The pretender",
        "price": 42.0,
        "tax": 3.2
    },
    "user": {
        "username": "dave",
        "full_name": "Dave Grohl"
    }
}
```
>[!NOTE]
> توجه داشته باشید که اگرچه item به همان روش قبلی اعلام شده است، اکنون انتظار می‌رود که با کلید item داخل بدنه قرار گیرد.
<br><br>

FastAPI انجام تبدیل خودکار از درخواست را انجام خواهد داد، بنابراین پارامتر item محتوای خاص خود را دریافت می‌کند و همین امر برای user نیز صادق است.
<br><br>
این کار اعتبارسنجی داده‌های مرکب را انجام خواهد داد و به همین ترتیب برای طرح OpenAPI و اسناد خودکار مستند خواهد کرد.

# مقدارهای مفرد در بدنه
به همان شیوه ای که Query و Path برای تعریف داده های اضافی برای پارامترهای query و path وجود دارد، FastAPI یک معادل Body ارائه می دهد.
<br><br>
به عنوان مثال، با گسترش مدل قبلی، ممکن است تصمیم بگیرید که می خواهید یک کلید دیگر اهمیت در همان بدنه، علاوه بر item و user داشته باشید.
<br><br>
اگر همانطور که هست اعلام کنید، به دلیل اینکه یک مقدار مفرد است، FastAPI فرض خواهد کرد که یک پارامتر query است.

اما شما می توانید به FastAPI دستور دهید تا آن را به عنوان یک کلید بدنه دیگر با استفاده از Body در نظر بگیرد:
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


class User(BaseModel):
    username: str
    full_name: str | None = None


@app.put("/items/{item_id}")
async def update_item(
    item_id: int, item: Item, user: User, importance: Annotated[int, Body()]
):
    results = {"item_id": item_id, "item": item, "user": user, "importance": importance}
    return results
```
<br><br>
در این مورد، FastAPI انتظار دارد بدنه ای مانند موارد زیر داشته باشد:

```python
{
    "item": {
        "name": "Foo",
        "description": "The pretender",
        "price": 42.0,
        "tax": 3.2
    },
    "user": {
        "username": "dave",
        "full_name": "Dave Grohl"
    },
    "importance": 5
}
```

دوباره، انواع داده ها را تبدیل، اعتبارسنجی، مستندسازی و غیره می کند.
<br><br>
# پارامترهای بدنه چندگانه و query
<br><br>

البته، شما همچنین می‌توانید پارامترهای query اضافی را هر زمان که لازم دارید، علاوه بر هر پارامتر بدنه اعلام کنید.
<br><br>
از آنجایی که به طور پیش فرض مقادیر مفرد به عنوان پارامترهای query تفسیر می‌شوند، لازم نیست Query را صریحاً اضافه کنید، فقط می‌توانید این کار را انجام دهید:
```python
q: Union[str, None] = None
```
یا در python3.10 به بالا:
```python
q: str | None = None
```
برای مثال:
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


class User(BaseModel):
    username: str
    full_name: str | None = None


@app.put("/items/{item_id}")
async def update_item(
    *,
    item_id: int,
    item: Item,
    user: User,
    importance: Annotated[int, Body(gt=0)],
    q: str | None = None,
):
    results = {"item_id": item_id, "item": item, "user": user, "importance": importance}
    if q:
        results.update({"q": q})
    return results
```
<br><br>

>[!NOTE]
> Body نیز دارای همان پارامترهای اضافی اعتبارسنجی و داده‌کاوی است که Query، Path و موارد دیگری هستند که بعداً خواهید دید.

# پارامتر بدنه تکی را درج کنید
فرض کنید شما فقط یک پارامتر بدنه تکی item دارید که از یک مدل Pydantic Item گرفته شده است.
به طور پیش فرض، FastAPI بدنه آن را مستقیماً انتظار می‌کشد.
اما اگر می‌خواهید انتظار داشته باشد که یک JSON با کلید item و محتوای مدل در داخل آن باشد، مانند زمانی که پارامترهای بدنه اضافی را اعلام می‌کنید، می‌توانید از پارامتر Body خاص embed استفاده کنید:

<br><br>

```python
item: Item = Body(embed=True)
```
بعنوان:

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
async def update_item(item_id: int, item: Annotated[Item, Body(embed=True)]):
    results = {"item_id": item_id, "item": item}
    return results
```
در این مورد fastapi انتظار یک بدنه شبیه این دارد:

```python
{
    "item": {
        "name": "Foo",
        "description": "The pretender",
        "price": 42.0,
        "tax": 3.2
    }
}
```
<br><br>

# خلاصه مطالب

<br><br>
حتی اگر یک درخواست فقط می تواند یک بدنه داشته باشد، می توانید چندین پارامتر بدنه را به تابع عملیات مسیر خود اضافه کنید.
<br><br>

اما FastAPI آن را مدیریت خواهد کرد، داده های صحیح را در تابع شما ارائه می دهد و طرح صحیح را در عملیات مسیر اعتبارسنجی و مستند می کند.
همچنین می توانید مقادیر مفرد را به عنوان بخشی از بدنه اعلام کنید. شما همچنین می توانید به FastAPI دستور دهید که بدنه را در یک کلید قرار دهد حتی اگر فقط یک پارامتر اعلام شده باشد.































































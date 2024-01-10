# پارامترهای مسیر و اعتبارسینجیهای اعداد
<br><br>
دقیقاً مانند آنچه که برای پارامترهای query با توکن Query امکان‌پذیر است، می‌توانید همان نوع بازرسی‌ها و متادیتا را برای پارامترهای مسیر با توکن Path اعلام کنید.
<br><br>
# Path را وارد کنید
ابتدا Path را از fastapi وارد کنید و Annotated را وارد کنید:

```python
from typing import Annotated

from fastapi import FastAPI, Path, Query

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    item_id: Annotated[int, Path(title="The ID of the item to get")],
    q: Annotated[str | None, Query(alias="item-query")] = None,
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```
<br><br>

>[!NOTE]
> FastAPI از نسخه 0.95.0 از Annotated پشتیبانی کرد (و شروع به توصیه آن کرد).
> اگر نسخه قدیمی‌تری دارید، هنگام استفاده از Annotated با خطا مواجه خواهید شد.
> قبل از استفاده از Annotated، مطمئن شوید که نسخه FastAPI را به حداقل 0.95.1 ارتقا دهید.

#  metadata را اعلام کنید
شما می‌توانید تمام پارامترهای مشابه را مانند Query اعلام کنید.

به عنوان مثال، برای اعلام یک مقدار متادیتای عنوان برای پارامتر مسیر item_id، می‌توانید تایپ کنید:

<br><br>


```python
from typing import Annotated

from fastapi import FastAPI, Path, Query

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    item_id: Annotated[int, Path(title="The ID of the item to get")],
    q: Annotated[str | None, Query(alias="item-query")] = None,
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```
<br><br>

>[!NOTE]
> یک پارامتر مسیر همیشه اجباری است زیرا باید بخشی از مسیر باشد.
> بنابراین، باید آن را با "..." اعلام کنید تا به عنوان اجباری مشخص شود.
> با این وجود، حتی اگر آن را با None اعلام کنید یا یک مقدار پیش‌فرض تنظیم کنید، تاثیری نخواهد داشت، زیرا همیشه اجباری خواهد بود.
<br><br>

# ترتیب پارامترها را مطابق نیاز خود قرار دهید

>[!TIP]
> اگر از Annotated استفاده می‌کنید، این احتمالاً چندان مهم یا ضروری نیست.
<br><br>

فرض کنید می‌خواهید پارامتر query q را به عنوان یک str اجباری اعلام کنید.
<br><br>
و برای آن پارامتر نیازی به اعلام چیز دیگری ندارید، بنابراین واقعاً نیازی به استفاده از Query نیست.
<br><br>
اما هنوز هم باید از Path برای پارامتر مسیر item_id استفاده کنید. و به دلایلی نمی‌خواهید از Annotated استفاده کنید.
<br><br>
Python شکایت خواهد کرد اگر یک مقدار با "default" را قبل از یک مقدار بدون "default" قرار دهید.
<br><br>
اما می‌توانید آنها را دوباره مرتب کنید و مقدار بدون default (پارامتر پرس‌وجوی q) را در ابتدا قرار دهید.

برای FastAPI مهم نیست.آن را با نام‌ها، انواع و اعلامات پیش‌فرض آنها (Query، Path، و غیره) تشخیص می‌دهد، به ترتیب اهمیت ندارد.
<br><br>
پس، می‌توانید تابع خود را به‌عنوان زیر اعلام کنید:

Python 3.8 non-Annotated
>[!TIP]
> در صورت امکان از نسخه Annotated استفاده کنید.
<br><br>

```python
from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(q: str, item_id: int = Path(title="The ID of the item to get")):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```
<br><br>
اما به خاطر داشته باشید که اگر از Annotated استفاده می‌کنید، این مشکل را نخواهید داشت، زیرا از مقادیر پیش‌فرض پارامترهای تابع برای Query یا Path استفاده نمی‌کنید.



```python
from typing import Annotated

from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    q: str, item_id: Annotated[int, Path(title="The ID of the item to get")]
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```
<br><br>

# پارامترها را مطابق نیاز خود مرتب کنید، ترفندها


>[!TIP]
> اگر از Annotated استفاده می‌کنید، این احتمالاً چندان مهم یا ضروری نیست.

در اینجا یک ترفند کوچک وجود دارد که می‌تواند مفید باشد، اما زیاد به آن نیاز نخواهید داشت.
<br><br>


اگر می‌خواهید:

* پارامتر پرس‌وجوی q را بدون Query و بدون مقدار پیش‌فرض اعلام کنید.
* پارامتر مسیر item_id را با استفاده از Path اعلام کنید.
* آنها را در ترتیب متفاوت قرار دهید.
* از Annotated استفاده نکنید.
<br><br>

زبان پایتون یک قاعده نحوی خاص برای این منظور دارد.
ستاره (*) را به عنوان اولین پارامتر تابع پاس دهید.
<br><br>

زبان پایتون هیچ کاری با آن ستاره (*) انجام نخواهد داد، اما متوجه خواهد شد که تمام پارامترهای بعدی باید به عنوان آرگومان‌های کلیدی (زوج‌های کلید-مقدار) فراخوانی شوند، همچنین به آنها kwargs نیز گفته می‌شود. حتی اگر آنها مقدار پیش‌فرض نداشته باشند.


```python
from fastapi import FastAPI, Path


app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(*, item_id: int = Path(title="The ID of the item to get"), q: str):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```

# بهتر است از Annotated استفاده کنید

به خاطر داشته باشید که اگر از  Annotated استفاده می‌کنید، زیرا از مقادیر پیش‌فرض پارامترهای تابع استفاده نمی‌کنید، این مشکل را نخواهید داشت و احتمالاً نیازی به استفاده از ستاره (*) نخواهید داشت.

```python
from typing import Annotated

from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    item_id: Annotated[int, Path(title="The ID of the item to get")], q: str
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```
<br><br>

# اعتبارسنجی اعداد: بزرگتر یا برابر
با استفاده از Query و Path (و سایر توکن‌هایی که بعداً خواهید دید) می‌توانید محدودیت‌هایی برای اعداد تعریف کنید.
<br><br>

در اینجا، با استفاده از ge=1، item_id باید یک عدد صحیح بزرگتر یا مساوی 1 باشد.

```python
from typing import Annotated

from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    item_id: Annotated[int, Path(title="The ID of the item to get", ge=1)], q: str
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```
<br><br>
# اعتبارسنجی اعداد: بزرگتر از و کوچکتر یا مساوی
این موارد نیز اعمال می‌شود:

* gt: بزرگتر از
* le: کوچکتر یا مساوی


```python
from typing import Annotated

from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    item_id: Annotated[int, Path(title="The ID of the item to get", gt=0, le=1000)],
    q: str,
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```
# اعتبارسنجی اعداد: اعشاری، بزرگتر از و کوچکتر از
اعتبارسنجی اعداد همچنین برای مقادیر اعشاری (float) نیز کار می‌کند.
<br><br>

اینجاست که اهمیت استفاده از gt به جای ge مشخص می‌شود. با استفاده از gt می‌توانید الزامات بیشتری را برای مقادیر اعشاری تعریف کنید. به عنوان مثال، می‌توانید مشخص کنید که یک مقدار باید بزرگتر از 0 باشد، حتی اگر کوچکتر از 1 باشد.

<br><br>
بنابراین، 0.5 یک مقدار معتبر است. اما 0.0 یا 0 مقادیر معتبری نیستند.

همانطور که برای gt توضیح داده شد، برای lt نیز این محدودیت اعمال می‌شود.


```python
from typing import Annotated

from fastapi import FastAPI, Path, Query

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    *,
    item_id: Annotated[int, Path(title="The ID of the item to get", ge=0, le=1000)],
    q: str,
    size: Annotated[float, Query(gt=0, lt=10.5)],
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```
<br><br>

# خلاصه مطالب
با استفاده از Query، Path (و موارد دیگر که هنوز ندیده‌اید) می‌توانید متادیتا و اعتبارسنجی متن را به همان روش‌های Query Parameters و String Validations اعلام کنید. و همچنین می‌توانید اعتبارسنجی عددی را اعلام کنید:
<br><br>

و همچنین می‌توانید اعتبارسنجی عددی را اعلام کنید:
* gt: بزرگتر از
* ge: بزرگ تر مساوی
* lt: کوچک تر از
* le: کوچک تر مساوی



>[!Info]
> Query، Path و سایر کلاس‌هایی که در آینده خواهید دید زیرمجموعه‌های کلاس مشترک Param هستند. همه آنها پارامترهای یکسانی برای اعتبارسنجی و متادیتای اضافی دارند که تا به حال دیده‌اید.
> همه آنها پارامترهای یکسانی برای اعتبارسنجی و متادیتای اضافی دارند که تا به حال دیده‌اید.



>[!Technical Details]
> وقتی Query، Path و سایر توابع را از FastAPI وارد می کنید، در واقع آنها توابع هستند.
> این توابع زمانی که فراخوانی می شوند، نمونه هایی از کلاس های با همان نام را برمی گردانند.
> بنابراین، شما Query را وارد می کنید، که یک تابع است. و هنگامی که آن را فراخوانی می کنید، یک نمونه از کلاسی به نام Query را برمی گرداند.
> این توابع برای جلوگیری از نشان دادن خطاهای مربوط به نوع آنها توسط ویرایشگر شما وجود دارند.
> به این ترتیب می توانید از ویرایشگر و ابزارهای کدنویسی عادی خود استفاده کنید بدون اینکه مجبور باشید پیکربندی های سفارشی را برای نادیده گرفتن این خطاها اضافه کنید.











































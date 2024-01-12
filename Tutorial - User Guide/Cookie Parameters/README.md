# پارامترهای Cookie

<br><br>
You can define Cookie parameters the same way you define Query and Path parameters.


<br><br>
# Cookie را وارد کنید
نخست cookie را وارد کنید

<br><br>
```python
from typing import Annotated

from fastapi import Cookie, FastAPI

app = FastAPI()


@app.get("/items/")
async def read_items(ads_id: Annotated[str | None, Cookie()] = None):
    return {"ads_id": ads_id}
```
<br><br>
# علان پارامترهای Cookie
سپس پارامترهای Cookie را با همان ساختاری که برای Path و Query استفاده می کنید، اعلام کنید.

اولین مقدار مقدار پیش فرض است، همچنین می توانید تمام پارامترهای اعتبارسنجی یا توضیحات اضافی را منتقل کنید.


<br><br>
```python
from typing import Annotated

from fastapi import Cookie, FastAPI

app = FastAPI()


@app.get("/items/")
async def read_items(ads_id: Annotated[str | None, Cookie()] = None):
    return {"ads_id": ads_id}
```
<br><br>
>[!Technical Details]
> Cookie یک کلاس "خواهر" Path و Query است. همچنین از کلاس مشترک Param به ارث می برد.
> اما به یاد داشته باشید که وقتی Query، Path، Cookie و موارد دیگر را از fastapi وارد می کنید، این در واقع توابعی هستند که کلاس های ویژه ای را برمی گردانند.
<br><br>

>[!NOTE]
> برای تعریف کوکی ها، باید از Cookie استفاده کنید، زیرا در غیر این صورت پارامترها به عنوان پارامترهای Query تفسیر می شوند.


<br><br>
# خلاصه مطالب
برای تعریف کوکی ها از کلاس Cookie استفاده کنید، با استفاده از همان الگوی مشترک برای مسیر (Query) و مسیر (Path).


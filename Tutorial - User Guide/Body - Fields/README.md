# بدنه - فیلدها

به همان شیوه ای که می توانید در پارامترهای تابع عملیات مسیر با Query، Path و Body اعتبارسنجی و فراداده اضافی را اعلام کنید، می توانید در داخل مدل های Pydantic با استفاده از Field Pydantic اعتبارسنجی و فراداده را اعلام کنید.
<br><br>
# Field را وارد کنید

نخست باید ان را وارد کنید:

```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel, Field

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = Field(
        default=None, title="The description of the item", max_length=300
    )
    price: float = Field(gt=0, description="The price must be greater than zero")
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Annotated[Item, Body(embed=True)]):
    results = {"item_id": item_id, "item": item}
    return results
```
<br><br>
>[!WARNING]
> کلاس Field مستقیماً از کتابخانه pydantic وارد می شود، نه از کتابخانه fastapi. این به این دلیل است که Field خاص FastAPI نیست؛ این یک کلاس عمومی است که برای تعریف ویژگی ها برای مدل های Pydantic استفاده می شود.

# اعلام ویژگی‌های مدل
<br><br>
سپس می‌توانید Field را با ویژگی‌های مدل استفاده کنید.

<br><br>

```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel, Field

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = Field(
        default=None, title="The description of the item", max_length=300
    )
    price: float = Field(gt=0, description="The price must be greater than zero")
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Annotated[Item, Body(embed=True)]):
    results = {"item_id": item_id, "item": item}
    return results
```
Field درست مانند Query، Path و Body کار می کند، دارای تمام پارامترهای یکسان است، و غیره.
<br><br>

>[!Technical Details]
> در واقع، Query، Path و سایر موارد مشابهی که در ادامه خواهید دید، اشیایی از زیر کلاس‌های کلاس مشترک Param ایجاد می‌کنند، که خود یک زیرکلاس کلاس FieldInfo Pydantic است.
> کلاس Field در Pydantic نیز یک نمونه از کلاس FieldInfo را برمی‌گرداند.
> Body نیز به طور مستقیم اشیایی از زیرکلاس کلاس FieldInfo بازمی‌گرداند. و موارد دیگری نیز وجود دارند که در ادامه آنها را خواهید دید که زیرکلاس‌های کلاس Body هستند.
> به یاد داشته باشید که وقتی از Query، Path و موارد دیگر از fastapi وارد می‌کنید، آنها در واقع توابعی هستند که کلاس‌های ویژه را بازمی‌گردانند.

<br><br>

>[!TIP]
> توجه داشته باشید که هر ویژگی یک مدل با نوع، مقدار پیش‌فرض و Field دارای همان ساختار یک پارامتر تابع عملیات مسیر است، با Field به جای Path، Query و Body.

# اضافه کردن اطلاعات اضافی
می توانید اطلاعات اضافی را در فیلد، کوئری، بدنه و غیره اعلام کنید. و در طرح JSON تولید شده گنجانده خواهد شد. شما در مورد اضافه کردن اطلاعات اضافی در اسناد بیشتر یاد خواهید گرفت، زمانی که یاد بگیرید که چگونه مثال ها را اعلام کنید.
<br><br>

>[!WARNING]
> کلیدهای اضافی که به Field پاس می شوند، در طرح OpenAPI حاصل برای برنامه شما نیز وجود خواهند داشت. از آنجایی که این کلیدها لزوماً بخشی از مشخصات OpenAPI نیستند، برخی از ابزارهای OpenAPI، مانند اعتبارسنجی OpenAPI، ممکن است با طرح تولید شده شما کار نکنند.



<br><br>

# خلاصه مطالب

می‌توانید از Field Pydantic برای اعلام اعتبارسنجی‌های اضافی و فراداده برای ویژگی‌های مدل استفاده کنید.
<br><br>

همچنین می‌توانید از آرگومان‌های اضافی کلمه کلیدی برای انتقال فراداده JSON Schema اضافی استفاده کنید.










































# انواع داده‌های اضافی
<br><br>
تاکنون از داده‌های معمولی مانند int، float، str و bool استفاده می‌کنید. اما می‌توانید از داده‌های پیچیده‌تر نیز استفاده کنید. و هنوز هم همان ویژگی‌هایی را خواهید داشت که تا به حال دیده‌اید:

<br><br>
* پشتیبانی عالی از ویرایشگر
* تبدیل داده از درخواست‌های ورودی
* تبدیل داده برای داده‌های پاسخ
* اعتبارسنجی داده
* مستندات خودکار و یادداشت‌برداری

<br><br>
# انواع داده‌های دیگر
<br><br>
در اینجا برخی از انواع داده‌های اضافی وجود دارد که می‌توانید از آنها استفاده کنید:
<br><br>
* UUID:
    * یک "شناسه جهانی منحصر به فرد" استاندارد، که در بسیاری از پایگاه داده‌ها و سیستم‌ها به عنوان ID رایج است.
    * در درخواست‌ها و پاسخ‌ها به صورت str نشان داده می‌شود.
* datetime.datetime:
    * یک Python datetime.datetime.
    * در درخواست‌ها و پاسخ‌ها به صورت str در قالب ISO 8601 نشان داده می‌شود، مانند: 2008-09-15T15:53:00+05:00.
<br><br>
* : Python datetime.date
    * Python datetime.date
    * در درخواست‌ها و پاسخ‌ها به صورت str در قالب ISO 8601 نشان داده می‌شود، مانند: 2008-09-15.
<br><br>
* datetime.time:
    * Python datetime.time.
    * در درخواست‌ها و پاسخ‌ها به صورت str در قالب ISO 8601 نشان داده می‌شود، مانند: 14:23:55.003.
<br><br>
* datetime.timedelta:
    * یک Python datetime.timedelta.
    * در درخواست‌ها و پاسخ‌ها به صورت float از کل ثانیه‌ها نشان داده می‌شود.
    * همچنین Pydantic اجازه می‌دهد تا آن را به عنوان "رمزگذاری تفاوت زمان ISO 8601" نمایش دهد، برای اطلاعات بیشتر به اسناد مراجعه کنید.
<br><br>
* frozenset: 
    * در درخواست‌ها و پاسخ‌ها به همان روش set رفتار می‌شود:
در درخواست‌ها، یک لیست خوانده خواهد شد، تکرارها حذف می‌شوند و به set تبدیل می‌شود.
    * در پاسخ‌ها، set به یک لیست تبدیل می‌شود.
    * نوع مدل‌سازی شده مشخص می‌کند که مقادیر مجموعه منحصر به فرد هستند (با استفاده از ویژگی uniqueItems JSON Schema).
<br><br>
* bytes: Python
    *  استاندارد بایت.
    * در درخواست‌ها و پاسخ‌ها به صورت str در نظر گرفته می‌شود.
    *  نوع مدل‌سازی شده مشخص می‌کند که این یک str با فرمت باینری است.
<br><br>
* : Decimal
    * Decimal استاندارد Python.
    * در درخواست‌ها و پاسخ‌ها به همان روش float کنترل می‌شود.
    * می‌توانید تمام انواع داده‌های معتبر Pydantic را اینجا بررسی کنید: انواع داده‌های Pydantic.
<br><br>

# مثال
در اینجا یک نمونه از یک عملیات مسیر با پارامترها با استفاده از برخی از انواع فوق آورده شده است:
<br><br>
```python
from datetime import datetime, time, timedelta
from typing import Annotated
from uuid import UUID

from fastapi import Body, FastAPI

app = FastAPI()


@app.put("/items/{item_id}")
async def read_items(
    item_id: UUID,
    start_datetime: Annotated[datetime | None, Body()] = None,
    end_datetime: Annotated[datetime | None, Body()] = None,
    repeat_at: Annotated[time | None, Body()] = None,
    process_after: Annotated[timedelta | None, Body()] = None,
):
    start_process = start_datetime + process_after
    duration = end_datetime - start_process
    return {
        "item_id": item_id,
        "start_datetime": start_datetime,
        "end_datetime": end_datetime,
        "repeat_at": repeat_at,
        "process_after": process_after,
        "start_process": start_process,
        "duration": duration,
    }
```
<br><br>
توجه داشته باشید که پارامترهای داخل تابع دارای نوع داده طبیعی خود هستند و می توانید به عنوان مثال، عملیات تاریخ معمولی را مانند زیر انجام دهید:


<br><br>
```python
from datetime import datetime, time, timedelta
from typing import Annotated
from uuid import UUID

from fastapi import Body, FastAPI

app = FastAPI()


@app.put("/items/{item_id}")
async def read_items(
    item_id: UUID,
    start_datetime: Annotated[datetime | None, Body()] = None,
    end_datetime: Annotated[datetime | None, Body()] = None,
    repeat_at: Annotated[time | None, Body()] = None,
    process_after: Annotated[timedelta | None, Body()] = None,
):
    start_process = start_datetime + process_after
    duration = end_datetime - start_process
    return {
        "item_id": item_id,
        "start_datetime": start_datetime,
        "end_datetime": end_datetime,
        "repeat_at": repeat_at,
        "process_after": process_after,
        "start_process": start_process,
        "duration": duration,
    }
```
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
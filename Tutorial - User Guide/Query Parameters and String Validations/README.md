# پارامترهای Query و اعتبارسنجی رشته
FastAPI به شما امکان می‌دهد اطلاعات و اعتبارسنجی اضافی را برای پارامترهای خود اعلام کنید. بیایید این برنامه را به عنوان مثال در نظر بگیریم:

<br><br>
python 3.10+
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/")
async def read_items(q: str | None = None):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
<br><br>
پارامتر پرس و جو q از نوع Union[str, None] (یا str | None در پایتون 3.10) است، به این معنی که از نوع str است اما می‌تواند None نیز باشد، و در واقع، مقدار پیش‌فرض None است، بنابراین FastAPI می‌داند که مورد نیاز نیست.
<br><br>
>[!NOTE]
> FastAPI خواهد فهمید که مقدار q مورد نیاز نیست زیرا مقدار پیش فرض آن None است.
> نشانگر Union در Union[str, None] به ویرایشگر شما اجازه می دهد تا پشتیبانی بهتری به شما ارائه دهد و خطاها را شناسایی کند.
<br><br>

# اعتبارسنجی اضافی
<br><br>

ما قصد داریم این را اعمال کنیم که اگرچه q اختیاری است، هر زمان که ارائه شود، طول آن از 50 کاراکتر بیشتر نمی‌شود.

<br><br>
# وارد کردن Query و Annotated
برای رسیدن به آن، ابتدا موارد زیر را وارد کنید:
* Query از fastapi
* Annotated از typing (یا از typing_extensions در Python زیر 3.9)

python 3.10+
<br><br>
```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str | None, Query(max_length=50)] = None):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
>[!NOTE]
> FastAPI از نسخه 0.95.0 از Annotated پشتیبانی می کند (و از آن توصیه می کند).
> اگر نسخه قدیمی تری دارید، هنگام تلاش برای استفاده از Annotated با خطا مواجه خواهید شد.
> قبل از استفاده از Annotated، مطمئن شوید که نسخه FastAPI خود را به حداقل 0.95.1 ارتقا دهید.
<br><br>

# از Annotated در نوع پارامتر q استفاده کنید
<br><br>

به یاد داشته باشید که قبلاً به شما گفته بودم که Annotated را می توان برای افزودن ابرداده به پارامترهای خود در Python Types Intro استفاده کرد؟
حالا زمان استفاده از آن با FastAPI است. 🚀

ما این نوع آدرس‌دهی را داشتیم:

```python
q: str | None = None
```
<br><br>

آنچه ما انجام خواهیم داد این است که آن را با Annotated بپیچیم، بنابراین می‌شود:

<br><br>
```python
q: Annotated[str | None] = None
```
هر دو نسخه به معنای همان چیز هستند، q یک پارامتر است که می تواند یک رشته (str) یا None باشد، و به طور پیش فرض، None است.
<br><br>
حالا بیایید به سراغ چیزهای جالب برویم. 🎉
<br><br>

# به Annotated در پارامتر q Query اضافه کنید
<br><br>
حالا که این Annotated را داریم که می‌توانیم ابرداده بیشتری به آن اضافه کنیم، Query را به آن اضافه کنید و پارامتر max_length را روی 50 تنظیم کنید:

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str | None, Query(max_length=50)] = None):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
<br><br>

توجه داشته باشید که مقدار پیش فرض هنوز None است، بنابراین پارامتر هنوز اختیاری است.
<br><br>
    با این حال، اکنون، با داشتن Query(max_length=50) درون Annotated، ما به FastAPI می‌گوییم که می‌خواهیم این مقدار را از پارامترهای query استخراج کند (این به هر حال پیش فرض خواهد بود 🤷) و اینکه ما می‌خواهیم برای این مقدار اعتبارسنجی اضافی داشته باشیم (این دلیل این است که ما این کار را انجام می‌دهیم، برای گرفتن اعتبارسنجی اضافی). 😎
<br><br>
FastAPI اکنون باید مراحل زیر را انجام دهد:

* اعتبارسنجی داده را اطمینان حاصل کند که طول حداکثر 50 کاراکتر دارد
* خطای واضحی را برای مشتری نشان دهد زمانی که داده معتبر نیست
* پارامتر را در طرح OpenAPI اسناد عملیات مسیر مستند کند (بنابراین در UI مستندات خودکار ظاهر می شود)

# گزینه جایگزین (قدیمی) Query به عنوان مقدار پیش فرض
<br><br>

نسخه‌های قبلی FastAPI (قبل از 0.95.0) از شما خواسته بودند که Query را به عنوان مقدار پیش‌فرض پارامتر خود استفاده کنید، به جای قرار دادن آن در Annotated، احتمال زیادی وجود دارد که کدهایی را با استفاده از آن مشاهده کنید، بنابراین آن را برای شما توضیح خواهم داد.
<br><br>
>[!TIP]
> برای کد جدید و هر زمان که ممکن است، از Annotated استفاده کنید همانطور که در بالا توضیح داده شد. چندین مزیت وجود دارد (در زیر توضیح داده شده است) و هیچ عیبی ندارد. 🍰
<br><br>

این است که چگونه می‌توانید از Query() به عنوان مقدار پیش‌فرض پارامتر تابع خود استفاده کنید، تنظیم پارامتر max_length بر روی 50:
<br><br>
```python
from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: str | None = Query(default=None, max_length=50)):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
<br><br>
همانطور که در این حالت (بدون استفاده از Annotated) ما باید مقدار پیش فرض None را در تابع با Query() جایگزین کنیم، اکنون باید مقدار پیش فرض را با پارامتر Query(default=None) تنظیم کنیم، هدف آن تعریف آن مقدار پیش فرض است (حداقل برای FastAPI).
<br><br>

بنابراین:
<br><br>
```python
q: Union[str, None] = Query(default=None)
```
"...این پارامتر را اختیاری می‌کند، با مقدار پیش فرض None، همانند:"
```python
q: Union[str, None] = None
```
و در python3.10 به بالا:
```python
q: str | None = Query(default=None)
```
"...این پارامتر را اختیاري مي‌كند، با مقدار پيش فرض None، مانند:"

```python
q: str | None = None
```
<br><br>
اما آن را به صراحت به عنوان پارامتر query اعلام می‌کند.

>[!NOTE]
>به خاطر داشته باشید که مهمترین قسمت برای اختیاری کردن یک پارامتر این قسمت است:
> = None
>یا
> = Query(default=None)
> زیرا آن None را به عنوان مقدار پیش فرض در نظر خواهد گرفت و به این ترتیب پارامتر را غیرضروری می‌کند.
> قسمت Union[str, None] به ویرایشگر شما امکان ارائه پشتیبانی بهتر را می‌دهد، اما این چیزی نیست که به FastAPI می‌گوید که این پارامتر مورد نیاز نیست.
<br><br>

سپس، ما می‌توانیم پارامترهای بیشتری به Query پاس کنیم. در این مورد، پارامتر max_length مربوط به رشته‌ها است:

```python
q: Union[str, None] = Query(default=None, max_length=50)
```
این کار داده‌ها را اعتبارسنجی می‌کند، زمانی که داده معتبر نیست، یک خطای واضح نشان می‌دهد و پارامتر را در طرح OpenAPI عملیات مسیر مستند می‌کند.
<br><br>

# Query بعنوان مقدار پیشفرض یا در Annotated
<br><br>

به یاد داشته باشید که هنگام استفاده از Query داخل Annotated نمی‌توانید از پارامتر default برای Query استفاده کنید.
<br><br>
در عوض از مقدار پیش فرض واقعی پارامتر تابع استفاده کنید. در غیر این صورت، ناسازگار خواهد بود.

به عنوان مثال، این مجاز نیست:

```python
q: Annotated[str, Query(default="rick")] = "morty"
```
... زیرا مشخص نیست که مقدار پیش فرض باید "ریک" یا "مورتی" باشد.
<br><br>
بنابراین، شما (بهتر است) از این استفاده کنید:

```python
q: Annotated[str, Query()] = "rick"
```
<br><br>
... یا در کدهای پایه قدیمی‌تر، شما این را خواهید دید:

```python
q: str = Query(default="rick")
```
<br><br>

# مزایا استفاده از Annotated
استفاده از Annotated به جای مقدار پیش فرض در پارامترهای تابع توصیه می‌شود، زیرا دلایل متعددی دارد. 🤓

مقدار پیش فرض پارامتر تابع همان مقدار پیش فرض واقعی است، که به طور کلی با Python سازگارتر است. 😌
<br><br>
می‌توانید همان تابع را در مکان‌های دیگر بدون FastAPI فراخوانی کنید، و همانطور که انتظار می‌رود کار خواهد کرد. اگر پارامتر اجباری (بدون مقدار پیش فرض) وجود دارد، ویرایشگر شما آن را با خطایی به شما اطلاع خواهد داد، Python همچنین اگر آن را بدون ارسال پارامتر اجباری اجرا کنید، شکایت خواهد کرد.
<br><br>
هنگامی که از Annotated استفاده نمی‌کنید و به جای آن از سبک مقدار پیش فرض (قدیمی) استفاده می‌کنید، اگر بدون FastAPI آن تابع را در مکان دیگری فراخوانی کنید، باید آرگومان‌ها را به تابع منتقل کنید تا به درستی کار کند، در غیر این صورت مقادیر متفاوت از آنچه انتظار دارید خواهد بود (به عنوان مثال QueryInfo یا چیزی مشابه به جای str). و ویرایشگر شما شکایت نمی‌کند، و Python هنگام اجرای آن تابع شکایت نمی‌کند، فقط زمانی که عملیات داخلی دچار خطا شوند.
<br><br>
از آنجایی که Annotated می‌تواند بیش از یک Annotations داشته باشد، اکنون می‌توانید همان تابع را با ابزارهای دیگر مانند Typer نیز استفاده کنید. 🚀
<br><br>
# افزودن اعتبارسنجی بیشتر
همچنین می‌توانید پارامتر min_length را اضافه کنید:

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: Annotated[str | None, Query(min_length=3, max_length=50)] = None
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
<br><br>

# افزودن Regex
<br><br>

شما می‌توانید یک الگوی regex تعریف کنید که پارامتر باید با آن مطابقت داشته باشد:

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: Annotated[
        str | None, Query(min_length=3, max_length=50, pattern="^fixedquery$")
    ] = None,
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
<br><br>

این الگوی regex خاص بررسی می‌کند که مقدار پارامتر دریافت شده:
* ^: با کاراکترهای زیر شروع می‌شود، هیچ کاراکتری قبل از آن وجود ندارد.
* fixedquery: دارای مقدار دقیق fixedquery است.
* $: در آنجا به پایان می‌رسد، هیچ کاراکتر دیگری پس از fixedquery وجود ندارد.
<br><br>

اگر با این همه ایده "الگوریتم" گیج هستید، نگران نباشید. آنها یک موضوع دشوار برای بسیاری از مردم هستند. شما هنوز هم می توانید بسیاری از چیزها را بدون نیاز به الگوی regex انجام دهید.
اما هر زمان که به آنها نیاز داشتید و آنها را یاد گرفتید، بدانید که می توانید آنها را مستقیماً در FastAPI استفاده کنید.

<br><br>
# استفاده از regex در نسخه 1 Pydantic به جای pattern
قبل از نسخه 2 Pydantic و قبل از FastAPI 0.100.0، پارامتر به جای pattern به نام regex نامیده می‌شد، اما اکنون منسوخ شده است. شما ممکن است هنوز کدی با استفاده از آن ببینید.

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: Annotated[
        str | None, Query(min_length=3, max_length=50, regex="^fixedquery$")
    ] = None,
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

اما بدانید که این منسوخ شده است و باید برای استفاده از پارامتر جدید pattern به روز شود.

<br><br>

# مقدار پیشفرض
حتماً می‌توانید از مقادیر پیش فرض غیر از None استفاده کنید.

این بدان معناست که می‌توانید برای پارامترهای Query از مقادیر پیش فرض غیر از None استفاده کنید. به عنوان مثال، می‌توانید برای پارامتر q مقادیر پیش فرض "fixedquery" یا "defaultquery" را مشخص کنید.
python 3.9+
```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str, Query(min_length=3)] = "fixedquery"):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
>[!NOTE]
> داشتن مقدار پیش فرض از هر نوع، از جمله None، پارامتر را اختیاری می‌کند (نیازی نیست).
<br><br>

# آن را اجباری کنید
<br><br>

هنگامی که نیازی به اعلام اعتبارسنجی یا متادیتا بیشتر نداریم، می‌توانیم پارامتر پرس‌وجوی q را فقط با عدم اعلام مقدار پیش فرض اجباری کنیم، مانند:
```python
q: str
```
بجای:
```python
q: Union[str, None] = None
```
اما اکنون آن را با Query اعلام می‌کنیم، به عنوان مثال مانند:
<br><br>
Annotated:
```python
q: Annotated[Union[str, None], Query(min_length=3)] = None
```
none_Anotated:
```python
q: Union[str, None] = Query(default=None, min_length=3)
```
<br><br>

بنابراین، وقتی نیاز دارید یک مقدار را به عنوان اجباری در حالی که از Query استفاده می‌کنید اعلام کنید، می‌توانید به سادگی مقدار پیش فرضی را اعلام نکنید.
```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str, Query(min_length=3)]):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
<br><br>
# الزامی با Ellipsis (...)
یک روش جایگزین برای صریحاً اعلام اینکه یک مقدار مورد نیاز است وجود دارد. می‌توانید مقدار پیش فرض را به مقدار لفظی (...) تنظیم کنید.

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str, Query(min_length=3)] = ...):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
<br><br>
>[!NOTE]
> اگر قبلاً این "..." را ندیده بودید: این یک مقدار واحد خاص است، بخشی از پایتون است و "Ellipsis" نامیده می‌شود.
> این توسط Pydantic و FastAPI برای اعلام صریح اینکه یک مقدار مورد نیاز است استفاده می‌شود.

این به FastAPI اطلاع می‌دهد که این پارامتر مورد نیاز است.

# اجباری با None

می‌توانید اعلام کنید که یک پارامتر می‌تواند None را قبول کند، اما هنوز هم مورد نیاز است. این باعث می‌شود که مشتریان مجبور شوند یک مقدار ارسال کنند، حتی اگر مقدار None باشد.
<br><br>

برای انجام این کار، می‌توانید اعلام کنید که None یک نوع معتبر است اما هنوز هم از ... به عنوان پیش فرض استفاده می‌کنید.
<br><br>
```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str | None, Query(min_length=3)] = ...):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
<br><br>
>[!TIP]
> Pydantic، که نیروی اصلی اعتبارسنجی و سریالیزیشن داده در FastAPI است، رفتار خاصی دارد وقتی که Optional یا Union[Something, None] را بدون مقدار پیش فرض استفاده می‌کنید. در مورد فیلدهای اختیاری مورد نیاز می‌توانید در مستندات Pydantic بیشتر بخوانید.

>[!TIP]
> به خاطر داشته باشید که در بیشتر موارد، هنگامی که چیزی مورد نیاز است، می‌توانید به سادگی مقدار پیش فرض را حذف کنید، بنابراین معمولاً نیازی به استفاده از ... نیست.

# لیست پارامتر Query / مقادیر چندگانه


هنگامی که یک پارامتر پرس و جو را به صورت صریح با Query تعریف می کنید، همچنین می توانید آن را برای دریافت یک لیست از مقادیر اعلام کنید، یا به عبارت دیگر، برای دریافت چندین مقدار.
<br><br>

به عنوان مثال، برای اعلام یک پارامتر پرس و جو q که می تواند چندین بار در URL ظاهر شود، می توانید بنویسید:
<br><br>
```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[list[str] | None, Query()] = None):
    query_items = {"q": q}
    return query_items
```
<br><br>
سپس، با URL مانند:

```python
http://localhost:8000/items/?q=foo&q=bar
```
<br><br>

در داخل تابع عملیات مسیر خود، در پارامتر تابع q، مقادیر پارامترهای پرس‌وجوی چندگانه q (foo و bar) را در یک لیست پایتون دریافت خواهید کرد.
<br><br>

بنابراین، پاسخ به آن URL چنین خواهد بود:


```python
{
  "q": [
    "foo",
    "bar"
  ]
}
```
<br><br>
>[!TIP]
> برای اعلام یک پارامتر پرس‌وجو با نوع لیستی مانند مثال بالا، باید به‌طور صریح از Query استفاده کنید، در غیر این صورت به‌عنوان بدنه درخواست تفسیر می‌شود.
<br><br>
مستندات تعاملی API به‌طور مناسب به‌روزرسانی خواهد شد تا امکان استفاده از چندین مقدار را فراهم کند:


<img src="https://fastapi.tiangolo.com/img/tutorial/query-params-str-validations/image02.png"><br><br>

# لیست پارامتر Query / مقادیر چندگانه با پیش‌فرض‌ها
<br><br>

همچنین می‌توانید یک لیست پیش‌فرض از مقادیر را تعریف کنید اگر هیچ مقداری ارائه نشود.

<br>

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[list[str], Query()] = ["foo", "bar"]):
    query_items = {"q": q}
    return query_items
```
اگر شما برید به:
```python
http://localhost:8000/items/
```

<br><br>
مقدار پیش‌فرض q برابر با ["foo", "bar"] خواهد بود و پاسخ شما خواهد بود:


```python
{
  "q": [
    "foo",
    "bar"
  ]
}
```
<br><br>
# از list استفاده کنید
می‌توانید به‌طور مستقیم از list به جای List[str] استفاده کنید (یا list[str] در پایتون 3.9+)
```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[list, Query()] = []):
    query_items = {"q": q}
    return query_items
```
<br><br>
>[!NOTE]
> توجه داشته باشید که در این حالت، FastAPI محتویات لیست را بررسی نمی‌کند.
> به عنوان مثال، List[int] محتویات لیست را بررسی می‌کند (و در مستندات ثبت می‌کند) تا مشخص شود که محتویات لیست عدد صحیح هستند. اما list به تنهایی این کار را نمی‌کند.

# اعلام متادیتا بیشتر
شما می‌توانید اطلاعات بیشتری در مورد پارامتر اضافه کنید.
این اطلاعات در OpenAPI تولید شده گنجانده خواهد شد و توسط رابط‌های کاربری مستندات و ابزارهای خارجی استفاده خواهد شد.

<br><br>


>[!NOTE]
> توجه داشته باشید که ابزارهای مختلف ممکن است سطح پشتیبانی متفاوتی از OpenAPI داشته باشند.
> برخی از آنها ممکن است هنوز تمام اطلاعات اضافی اعلام شده را نشان ندهند، اگرچه در بیشتر موارد، ویژگی گمشده قبلاً برای توسعه برنامه ریزی شده است.

<br><br>


شما میتوانید title اضافه کنید:
```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: Annotated[str | None, Query(title="Query string", min_length=3)] = None
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
و یک description:
```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: Annotated[
        str | None,
        Query(
            title="Query string",
            description="Query string for the items to search in the database that have a good match",
            min_length=3,
        ),
    ] = None,
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
<br><br>

# پارامترهای مستعار

تصور کنید که می‌خواهید نام پارامتر را به "item-query" تغییر دهید. مانند این:

```python
http://127.0.0.1:8000/items/?item-query=foobaritems
```
اما item-query نام معتبری برای متغیر پایتون نیست.
<br><br>
نزدیک‌ترین گزینه "item_query" است. اما همچنان به "item-query" دقیقاً نیاز دارید.

سپس می‌توانید یک "نام مستعار" اعلام کنید و آن نام مستعار همان چیزی خواهد بود که برای یافتن مقدار پارامتر استفاده می‌شود.



```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str | None, Query(alias="item-query")] = None):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
<br><br>
# مورد استفاده قرار دادن پارامترهای منسوخ

اکنون فرض کنید دیگر از این پارامتر خوشتان نمی‌آید.
<br><br>
باید آن را برای مدتی آنجا بگذارید زیرا مشتریانی از آن استفاده می‌کنند، اما می‌خواهید مستندات به وضوح آن را منسوخ نشان دهند.
<br><br>

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: Annotated[
        str | None,
        Query(
            alias="item-query",
            title="Query string",
            description="Query string for the items to search in the database that have a good match",
            min_length=3,
            max_length=50,
            pattern="^fixedquery$",
            deprecated=True,
        ),
    ] = None,
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```
<br><br>

مستندات آن را اینگونه نشان خواهند داد:
<br>
<img src="https://fastapi.tiangolo.com/img/tutorial/query-params-str-validations/image01.png"><br><br>

# از OpenAPI حذف کنید
برای حذف یک پارامتر query از طرح OpenAPI تولید شده (و در نتیجه، از سیستم‌های مستندسازی خودکار)، پارامتر include_in_schema را در Query به False تنظیم کنید:


```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    hidden_query: Annotated[str | None, Query(include_in_schema=False)] = None
):
    if hidden_query:
        return {"hidden_query": hidden_query}
    else:
        return {"hidden_query": "Not found"}
```
<br><br>

# خلاصه مطالب
<br><br>

شما می‌توانید بازرسی‌ها و متادیتای اضافی را برای پارامترهای خود اعلام کنید.

بازرسی‌ها و متادیتای عمومی:
* alias: نام مستعار را برای پارامتر مشخص کنید.
* title: عنوانی برای پارامتر مشخص کنید.
* description: توضیحی برای پارامتر مشخص کنید.
* deprecated: پارامتر را منسوخ اعلام کنید.

بازرسی‌های خاص برای رشته‌ها:

* min_length: حداقل طول رشته را مشخص کنید.
* max_length: حداکثر طول رشته را مشخص کنید.
* pattern: الگوی رشته را مشخص کنید.

در این مثال‌ها، نحوه اعلام بازرسی‌ها برای مقادیر str را مشاهده کردید.

برای دیدن نحوه اعلام بازرسی‌ها برای سایر انواع داده‌ها، مانند اعداد، به فصل‌های بعدی مراجعه کنید.






































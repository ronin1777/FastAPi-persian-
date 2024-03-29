# مقدمه‌ای بر انواع داده‌های پایتون

زبان پایتون از **"type hints"** اختیاری پشتیبانی می کند (که همچنین به عنوان **"type annotations"** نیز شناخته می شود).<br><br>
این **"type hints"** یا annotations سینتکس خاصی هستند که امکان اعلام نوع یک متغیر را فراهم می کنند.<br><br>
با اعلام نوع برای متغیرهای خود، ویرایشگرها و ابزارها می توانند پشتیبانی بهتری را به شما ارائه دهند.<br><br>
این فقط یک آموزش سریع یا مروری در مورد "type hints" پایتون است. این آموزش تنها موارد ضروری برای استفاده از آنها با **"FastAPI"** را پوشش می دهد... که در واقع بسیار کم است.<br><br>
FastAPI به طور کامل بر اساس این "type hints" استوار است، آنها مزایای زیادی را به آن می دهند.<br><br>
اما حتی اگر هرگز از **FastAPI** استفاده نکنید، دانستن اندکی در مورد آنها برای شما مفید خواهد بود.<br><br>
> [!NOTE]
> اگر یک متخصص پایتون هستید و قبلاً همه چیز در مورد "type hints" را می دانید، به فصل بعدی بروید.<br><br>

# انگیزه 
بیاید با یک مثال ساده شروع کنیم:<br><br>
```python
def get_full_name(first_name, last_name):
    full_name = first_name.title() + " " + last_name.title()
    return full_name

print(get_full_name("john", "doe"))
```
<br>
خروجی برنامه:<br>
John Doe
<br>
تابع به شرح زیر عمل می کند:<br>

* first_name و last_name را میگیرد
* حروف اول انها را با استفاده از title() برزگ میکند
* آنها را با یک اسپیس به هم متصل می کند 

```python
def get_full_name(first_name, last_name):
    full_name = first_name.title() + " " + last_name.title()
    return full_name


print(get_full_name("john", "doe"))
```

# اصلاحش کن
اما اکنون تصور کنید که از ابتدا آن را می نویسید.<br><br>
 در برخی از موارد شما شروع به تعریف تابع کرده اید ، پارامترها آماده هستند ...<br><br>
 اما سپس باید "آن متد" را که اولین حرف را به حروف بزرگ تبدیل می کند ، فراخوانی کنید.<br><br>
اون بزرگ بود؟ اون حروف بزرگ داشت؟ اولین حرف بزرگ؟ بزرگ کردن؟<br><br>

سپس، شما با دوست قدیمی برنامه نویس، تکمیل خودکار ویرایشگر امتحان می کنید.<br><br>
شما پارامتر اول تابع را تایپ می کنید، first_name، سپس یک نقطه (.) و سپس دکمه Ctrl+Space را فشار دهید تا تکمیل را فعال کنید.<br><br>
اما متاسفانه هیچ چیز مفیدی دریافت نمی کنید:<br><br>
<img src="https://fastapi.tiangolo.com/img/python-types/image01.png">

# افزودن انواع 
بیایید یک خط واحد از نسخه قبلی را تغییر دهیم.<br><br>
ما دقیقاً این تکه کد را،تغییر خواهیم داد پارامترهای تابع از:<br><br>
```
first_name, last_name
```
به  <br><br>
```python
first_name: str, last_name: str
```
همینه<br><br>
اینها "type hints" هستند<br><br>
```python
def get_full_name(first_name: str, last_name: str):
    full_name = first_name.title() + " " + last_name.title()
    return full_name


print(get_full_name("john", "doe"))
```
این همان اعلام مقادیر پیش فرض مانند با آن نیست:<br><br>

```python
first_name="john", last_name="doe"
```
این یک چیز متفاوت است: <br><br>
ما از دو نقطه (:) استفاده می کنیم، نه از علامت مساوی (=).<br><br>
و معمولاً افزودن نوع تایپی تغییری در آنچه اتفاق می افتد از آنچه بدون آنها اتفاق می افتد، ایجاد نمی کند..<br><br>
اما اکنون تصور کنید که دوباره در حال ایجاد آن تابع هستید، اما با type hints.<br><br>
در همان نقطه، شما سعی می کنید با Ctrl+Space اتمام خودکار را فعال کنید و می بینید:<br><br>
<img src="https://fastapi.tiangolo.com/img/python-types/image02.png"><br><br>
با استفاده از این ویژگی، شما می توانید اسکرول کنید و گزینه ها را ببینید تا زمانی که یکی از آنها "زنگ خطر را به صدا درآورد".<br><br>
<img src="https://fastapi.tiangolo.com/img/python-types/image03.png"><br><br>

# انگیزه های بیشتر 
این تابع را بررسی کنید، قبلاً دارای **"type hints"** است:
```python
def get_name_with_age(name: str, age: int):
    name_with_age = name + " is this old: " + age
    return name_with_age
```

از آنجایی که ویرایشگر از نوع متغیرها مطلع است، شما نه تنها تکمیل خودکار می گیرید، بلکه بررسی خطا نیز دریافت می کنید.<br><br>
<img src="https://fastapi.tiangolo.com/img/python-types/image04.png"><br><br>

اکنون می دانید که باید آن را رفع کنید، age را با str(age) به یک رشته تبدیل کنید.<br><br>

# اعلام انواع
شما فقط مکان اصلی برای اعلام نوع تایپی را مشاهده کردید. به عنوان پارامترهای تابع.<br><br>
این همچنین مکان اصلی شما برای استفاده از آنها با FastAPI است.<br><br>
# انواع ساده<br><br>

شما می توانید تمام انواع استاندارد Python را اعلام کنید، نه تنها str.
می توانید از موارد زیر استفاده کنید:<br><br>

* int
* float
* bool
* bytes

```python
def get_items(item_a: str, item_b: int, item_c: float, item_d: bool, item_e: bytes):
    return item_a, item_b, item_c, item_d, item_d, item_e
```
# انواع انتزاعی با پارامترهای نوع
برخی از ساختارهای داده وجود دارند که می توانند مقادیر دیگری را در خود جای دهند، مانند dict، list، set و tuple. و مقادیر داخلی نیز می توانند نوع خود را داشته باشند.<br><br>
این انواع که دارای انواع داخلی هستند، "generic" نامیده می شوند. و امکان اعلام آنها، حتی با انواع داخلی آنها وجود دارد.<br><br>
برای اعلام این انواع و انواع داخلی، می توانید از ماژول استاندارد Python typing استفاده کنید. این ماژول به طور خاص برای پشتیبانی از این type hints وجود دارد.<br><br>

# ورژن جدید پایتون
نحو استفاده از typing سازگار با همه نسخه های Python، از Python 3.6 تا آخرین نسخه ها، از جمله Python 3.9، Python 3.10 و غیره است.<br><br>
با پیشرفت Python، نسخه های جدیدتر با پشتیبانی بهبود یافته ای برای این نوع توضیحات همراه هستند و در بسیاری از موارد حتی لازم نیست ماژول typing را وارد کرده و از آن برای اعلام نوع توضیحات استفاده کنید.<br><br>
اگر می توانید برای پروژه خود یک نسخه جدیدتر از Python انتخاب کنید، می توانید از آن سادگی بیشتر استفاده کنید.<br><br>
در تمام اسناد، مثال هایی سازگار با هر نسخه Python (در صورت وجود تفاوت) وجود دارد.<br><br>
به عنوان مثال، "Python 3.6+" به این معنی است که با Python 3.6 یا بالاتر (از جمله 3.7، 3.8، 3.9، 3.10، و غیره) سازگار است. و "Python 3.9+" به این معنی است که با Python 3.9 یا بالاتر (از جمله 3.10) سازگار است.<br><br>
اگر می توانید از آخرین نسخه های Python استفاده کنید، از مثال های مربوط به آخرین نسخه استفاده کنید، زیرا آنها دارای بهترین و ساده ترین نحو هستند، به عنوان مثال، "Python 3.10+".<br><br>

# لیست
متغیر را با همان نحو دو نقطه (:) اعلام کنید.<br><br>
به عنوان نوع، list را قرار دهید<br><br>
از آنجا که لیست یک نوع است که حاوی برخی از انواع داخلی است، آنها را در براکت های مربع قرار می دهید:<br><br>
```python
def process_items(items: list[str]):
    for item in items:
        print(item)
```

> [!NOTE]
>آن انواع داخلی در براکت های مربع "پارامترهای نوع" نامیده می شوند.<br><br>
> در این مورد، str پارامتر نوع پاس داده شده به List (یا list در Python 3.9 و بالاتر) است.
این بدان معناست: "متغیر items یک لیست است، و هر یک از موارد موجود در این لیست یک str است".<br><br>
------------------------

> [!TIP]
> اگر از Python 3.9 یا بالاتر استفاده می کنید، نیازی نیست List را از typing وارد کنید، می توانید به جای آن از همان نوع معمول list استفاده کنید.
-----------------------
با انجام این کار، ویرایشگر شما می تواند حتی در حین پردازش موارد از لیست نیز پشتیبانی کند.<br><br>

<img src="https://fastapi.tiangolo.com/img/python-types/image05.png"><br><br>

بدون انواع، تقریباً غیرممکن است که به این دستاورد دست یافت.<br><br>
توجه داشته باشید که متغیر item یکی از عناصر لیست items است.<br><br>
و هنوز هم، ویرایشگر می داند که یک str است و برای آن پشتیبانی ارائه می دهد.<br><br>
# تاپل و ست
برای اعلام tuples و sets نیز به همین ترتیب عمل خواهید کرد:<br><br>
python 3.9+<br><br>
```python
def process_items(items_t: tuple[int, int, str], items_s: set[bytes]):
    return items_t, items_s
```
بدین معنی:
* متغیر items_t یک tuple با 3 مورد است، یک int، یک int دیگر و یک str.
* متغیر items_s یک set است و هر یک از موارد آن از نوع bytes است.

# دیکشنری

برای تعریف یک dict، شما 2 پارامتر نوع را با کاما جدا می کنید <br><br>
پارامتر نوع اول برای کلیدهای dict است<br><br>
پارامتر نوع دوم برای مقادیر dict است:<br><br>
python 3.9+<br><br>
```python
def process_items(prices: dict[str, float]):
    for item_name, item_price in prices.items():
        print(item_name)
        print(item_price)
```
بدین معنی:<br><br>
* متغیر prices یک dict است:
* کلیدهای این dict از نوع str هستند (یعنی نام هر مورد). 
* مقادیر این dict از نوع float هستند (یعنی قیمت هر مورد).

# اونیون
شما می توانید اعلام کنید که یک متغیر می تواند از چند نوع مختلف باشد، به عنوان مثال یک int یا یک str.<br><br>
در Python 3.6 و بالاتر (از جمله Python 3.10) می‌توانید از نوع Union از typing استفاده کنید و انواع ممکن را در براکت‌ها قرار دهید.<br><br>
در Python 3.10 همچنین یک نحو جدید وجود دارد که می‌توانید انواع ممکن را با یک میله عمودی (|) جدا کنید.<br><br>
python 3.10+<br><br>
```python
def process_item(item: int | str):
    print(item)
```
در هر دو مورد بدین معنی است که item میتواند int یا str باشد.<br><br>
# احتمالاً None
می توانید اعلام کنید که یک مقدار می تواند از نوع خاصی مانند str باشد، اما همچنین می تواند None باشد.<br><br>
در Python 3.6 و بالاتر (از جمله Python 3.10)، می‌توانید آن را با وارد کردن و استفاده از Optional از ماژول typing اعلام کنید.<br><br>
```python
from typing import Optional


def say_hi(name: Optional[str] = None):
    if name is not None:
        print(f"Hey {name}!")
    else:
        print("Hello World")
```
استفاده از Optional[str] به جای فقط str به ویرایشگر اجازه می‌دهد تا خطاهایی را که ممکن است فرض کرده باشید که یک مقدار همیشه یک str است، زمانی که در واقع نیز می‌تواند None باشد، شناسایی کند.<br><br>
Optional[Something] در واقع یک میانبر برای Union[Something, None] است، آنها معادل هستند.<br><br>
این همچنین بدان معنی است که در Python 3.10، می‌توانید از Something | None استفاده کنید.<br><br>
python 3.10+<br>
```python
def say_hi(name: str | None = None):
    if name is not None:
        print(f"Hey {name}!")
    else:
        print("Hello World")
```
از **Union** یا **Optional** استفاده کنید<br><br>
اگر از نسخه Python پایین تر از 3.10 استفاده می کنید، در اینجا یک نکته از دیدگاه کاملاً ذهنی من وجود دارد:<br><br>
* از استفاده از Optional[SomeType] خودداری کنید🚨
* ✨ به جای آن از Union[SomeType, None] استفاده کنید ✨.
هر دو معادل هستند و در زیر آنها یکسان هستند، اما من به جای Optional از Union استفاده می کنم زیرا کلمه "optional" به نظر می رسد که
ارزش اختیاری است، و در واقع به این معنی است که "می تواند None باشد"، حتی اگر اختیاری نباشد و هنوز هم مورد نیاز است.<br><br>

به نظر من Union[SomeType, None] واضح تر است که چه معنایی دارد.<br><br>
این فقط مربوط به کلمات و نام ها است. اما این کلمات می توانند بر نحوه تفکر شما و هم تیمی هایتان در مورد کد تأثیر بگذارند.<br><br>

برای مثال، بیایید این تابع را در نظر بگیریم.<br>
```python
from typing import Optional


def say_hi(name: Optional[str]):
    print(f"Hey {name}!")
```
پارامتر name به عنوان Optional[str] تعریف شده است، اما غیر اختیاری است، شما نمی توانید بدون این پارامتر از تابع استفاده کنید.<br><br>
say_hi() # ای داد بیداد، این خطا می دهد! 😱<br><br>
پارامتر name هنوز لازم است (اختیاری نیست) زیرا مقدار پیش فرضی ندارد. با این حال، name مقدار None را به عنوان مقدار خود قبول می کند:<br><br>
say_hi(name=None) # این کار می کند، None معتبر است 🎉<br><br>
خبر خوب این است که هنگامی که از Python 3.10 استفاده می کنید، دیگر لازم نیست نگران این موضوع باشید، زیرا می توانید به سادگی از | برای تعریف اتحاد انواع استفاده کنید.<br><br>
```python
def say_hi(name: str | None):
    print(f"Hey {name}!")
```
و سپس دیگر نگران نام‌هایی مانند Optional و Union نخواهید بود. 😎<br><br>

# انواع جانشینی
 این انواع که پارامترهای نوع را در پرانتزهای مربع قرار می دهند، به انواع جانشین یا Generics گفته می شود، به عنوان مثال:<br><br>
python 3.10+ <br>
می‌توانید از همان انواع داخلی به عنوان انواع جانشین استفاده کنید (با پرانتزهای مربع و انواع داخل آن):<br><br>
* list
* tuple
* set
* dict
<br>
و مانند Python 3.8، از ماژول typing استفاده می کنیم:
<br>
* Union
* Optional(مانند پایتون 3.8)
* و بقیه...
<br>
در Python 3.10، به عنوان جایگزینی برای استفاده از انواع جانشین Union و Optional، می توانید از علامت | (خط عمودی) برای تعریف اتحاد انواع استفاده کنید. این روش بسیار بهتر و ساده تر است.<br><br>
# کلاس ها و تایپ ها
همچنین می توانید یک کلاس را به عنوان نوع یک متغیر تعریف کنید. فرض کنید یک کلاس Person با یک نام دارید:<br><br>
```python
class Person:
    def __init__(self, name: str):
        self.name = name


def get_person_name(one_person: Person):
    return one_person.name
```
سپس می‌توانید یک متغیر را از نوع Person تعریف کنید:<br><br>
```python
class Person:
    def __init__(self, name: str):
        self.name = name


def get_person_name(one_person: Person):
    return one_person.name
```
و سپس، دوباره، از تمام پشتیبانی‌های ویرایشگر برخوردار می‌شوید:<br>
<img src="https://fastapi.tiangolo.com/img/python-types/image06.png"><br><br>
توجه داشته باشید که این جمله به این معنی است که "one_person یک نمونه از کلاس Person است". این بدان معنا نیست که "one_person کلاسی به نام Person است".<br><br>

# مدل های Pydantic
Pydantic یک کتابخانه Python برای اعتبارسنجی داده است.<br><br>
شما می توانید "شکل" داده ها را به عنوان کلاس هایی با ویژگی ها تعریف کنید.<br><br>

و هر ویژگی یک نوع دارد.<br><br>
سپس یک نمونه از آن کلاس با برخی مقادیر ایجاد می کنید و آن مقادیر را اعتبارسنجی می کند، آنها را به نوع مناسب تبدیل می کند (اگر چنین است) و به شما یک شی با تمام داده ها می دهد.<br><br>
و شما با آن شی نتیجه همه پشتیبانی ویرایشگر را دریافت می کنید.<br><br>
یک مثال از مستندات رسمی Pydantic:<br><br>
python 3.10+
```python
from datetime import datetime

from pydantic import BaseModel


class User(BaseModel):
    id: int
    name: str = "John Doe"
    signup_ts: datetime | None = None
    friends: list[int] = []


external_data = {
    "id": "123",
    "signup_ts": "2017-06-01 12:22",
    "friends": [1, "2", b"3"],
}
user = User(**external_data)
print(user)
# > User id=123 name='John Doe' signup_ts=datetime.datetime(2017, 6, 1, 12, 22) friends=[1, 2, 3]
print(user.id)
# > 123
```
> [!NOTE]
> [این داکیومنت را چک کنید](https://pydantic-docs.helpmanual.io/)برای یادگیری بیشتر pydantic
-------
FastAPI بر اساس Pydantic ساخته شده است.<br><br>
در [راهنمای کاربر - آموزش](https://fastapi.tiangolo.com/tutorial/)، نمونه‌های بیشتری از این موارد را خواهید دید.<br><br>
> [!TIP]
> [این داکیومنت را چک کنید]برای یادگیری بیشتر Pydantic دارای یک رفتار ویژه است وقتی از Optional یا Union[Something, None] بدون مقدار پیش فرض استفاده می‌کنید، می‌توانید در مستندات Pydantic درباره [Required Optional fields](https://pydantic-docs.helpmanual.io/usage/models/#required-optional-fields) بیشتر بخوانید.
----
# Type Hints با Metadata Annotations
زبان پایتون همچنین قابلیتی دارد که به شما امکان می دهد متادیتای اضافی را در این نوع های اشاره شده با استفاده از Annotated قرار دهید.<br><br>
python 3.9+
```python
from typing import Annotated


def say_hello(name: Annotated[str, "this is just metadata"]) -> str:
    return f"Hello {name}"
```
زبان پایتون به طور خودکار با Annotated کاری انجام نمی دهد. و برای ویرایشگرها و ابزارهای دیگر، نوع هنوز str است.<br><br>
با این حال، می توانید از این فضای موجود در Annotated برای ارائه متادیتای اضافی به FastAPI در مورد نحوه رفتار برنامه خود استفاده کنید.<br><br>
مهم است به خاطر داشته باشید که اولین پارامتر نوع که به Annotated می دهید، نوع واقعی است. بقیه فقط متادیتا برای ابزارهای دیگر هستند.<br><br>
فعلاً فقط باید بدانید که Annotated وجود دارد و این استاندارد Python است. 😎<br><br>
بعدا خواهید دید که چقدر قدرتمند می تواند باشد.<br><br>

> [!TIP]
> این واقعیت که این یک استاندارد Python است به این معنی است که شما هنوز هم بهترین تجربه ممکن توسعه دهنده را در ویرایشگر خود خواهید داشت، با ابزارهایی که برای تجزیه و تحلیل و بازنویسی کد خود استفاده می کنید و غیره. ✨
> و همچنین کد شما بسیار با بسیاری دیگر از ابزارها و کتابخانه های Python سازگار خواهد بود. 🚀
---------
# Type hint در FastAPI
**FastAPI از این نوع‌های اشاره‌شده برای انجام چندین کار بهره می‌برد. با FastAPI شما پارامترها را با نوع‌های اشاره‌شده علامت‌گذاری می‌کنید و به موارد زیر دست می‌یابید:<br><br>
* پشتیبانی ویرایشگر.
* بررسی نوع.

...و FastAPI از همان اعلامات برای موارد زیر استفاده می‌کند:<br><br>

* تعریف الزامات: از پارامترهای مسیر درخواست، پارامترهای پرسش، هدرها، بدن درخواست، وابستگی‌ها و غیره.
* تبدیل داده: از درخواست به نوع مورد نیاز.
* اعتبارسنجی داده: از هر درخواست: تولید خطاهای خودکار که در صورت نامعتبر بودن داده به مشتری ارسال می‌شوند.
* مستندنگاری API با استفاده از OpenAPI: که سپس توسط رابط‌های کاربری تعاملی مستندات خودکار استفاده می‌شود.

این همه ممکن است انتزاعی به نظر برسد. نگران نباشید. شما همه این موارد را در عمل در [راهنمای کاربر - آموزش ](https://fastapi.tiangolo.com/tutorial/)خواهید دید.<br><br>

نکته مهم این است که با استفاده از انواع استاندارد Python، در یک مکان (به جای افزودن کلاس‌های بیشتر، decorators و غیره)، FastAPI بسیاری از کارها را برای شما انجام خواهد داد.**

> [!NOTE]
> اگر قبلاً تمام آموزش را طی کرده اید و برای یادگیری بیشتر در مورد انواع، دوباره به اینجا بازگشتید، یک منبع خوب، ["یادداشت تقلب"](https://mypy.readthedocs.io/en/latest/cheat_sheet_py3.html) از mypy است.

























































































































































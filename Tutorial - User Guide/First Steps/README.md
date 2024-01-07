# قدم های اول
ساده‌ترین فایل FastAPI ممکن است به این شکل باشد:<br><br>
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```
کد را در فایلی به نام main.py ک\ی کنید.<br><br>
لایو سرور را اجرا کنید:<br><br>
```bash
$ uvicorn main:app --reload

INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [28720]
INFO:     Started server process [28722]
INFO:     Waiting for application startup.
INFO:     Application startup complete.

```
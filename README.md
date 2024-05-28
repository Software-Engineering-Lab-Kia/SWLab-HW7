جدول کانبان این آزمایش را می‌سازیم و تسک‌ها را در بک‌لاگ قرار می‌دهیم.
![](https://github.com/kiarashk8128/SWLab-HW7/blob/main/images/photo_2024-05-22_15-45-43.jpg?raw=true)


Dockerfile


FROM python:3.9: مشخص می‌کند که از ایمیج پایتون ۳.۹ به عنوان پایه استفاده شود.


ENV PYTHONDONTWRITEBYTECODE 1: جلوگیری از نوشتن فایل‌های pyc.


ENV PYTHONUNBUFFERED 1: ارسال خروجی پایتون به ترمینال بدون بافر.


WORKDIR /code: تنظیم دایرکتوری کاری درون کانتینر به /code.


COPY requirements.txt /code/: کپی کردن فایل requirements.txt به داخل کانتینر.


RUN pip install --no-cache-dir -r requirements.txt: نصب وابستگی‌های پایتون.


COPY . /code/: کپی کردن باقی کدهای برنامه به داخل کانتینر.


CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]: اجرای سرور توسعه جنگو.


docker-compose.yml


version: '3.8': مشخص کردن نسخه Docker Compose.


services: تعریف سرویس‌ها (وب و پایگاه‌داده) که Docker Compose مدیریت خواهد کرد.


db: تعریف سرویس پایگاه‌داده PostgreSQL.


image: postgres:13: استفاده از ایمیج رسمی PostgreSQL نسخه ۱۳.


volumes: مونت کردن یک ولوم برای نگهداری داده‌ها.


environment: تنظیم متغیرهای محیطی برای PostgreSQL.


web: تعریف سرویس وب جنگو.


build: ساخت ایمیج با استفاده از Dockerfile.


command: اجرای سرور توسعه جنگو.


volumes: مونت کردن دایرکتوری جاری به /code در کانتینر.


ports: نگاشت پورت ۸۰۰۰ روی میزبان به پورت ۸۰۰۰ در کانتینر.


depends_on: اطمینان از این که سرویس وب پس از سرویس db شروع می‌شود.


environment: تنظیم متغیرهای محیطی برای جنگو.





دستورات مربوط به داکر و کانتینر و image:



![Screenshot from 2024-05-28 19-44-20](https://github.com/kiarashk8128/SWLab-HW7/assets/82291200/113a0305-066e-403a-88ea-c41e50aa9634)



![Screenshot from 2024-05-28 19-44-41](https://github.com/kiarashk8128/SWLab-HW7/assets/82291200/d8ce32af-6527-401b-9741-5d8e3bf9a202)




دستوری دلخواه را در کانتینر وب‌سرور اجرا کنید. دستور مورد نظر و خروجی آن را در گزارش خود قرار دهید.

این دستور که آن را اجرا کردیم، فایل‌ها و دایرکتوری‌های موجود در مسیر /code داخل کانتینر وب‌سرور را نمایش می‌دهد.




![Screenshot from 2024-05-28 20-06-27](https://github.com/kiarashk8128/SWLab-HW7/assets/82291200/43303d66-fe01-4d96-8888-74cb7cdbbd4c)

Dockerfile
Dockerfile فایلی است که شامل دستورات لازم برای ساخت یک ایمیج داکر می‌باشد. این فایل مشخص می‌کند که چه نرم‌افزارها و تنظیماتی باید در کانتینر شما وجود داشته باشد.

بخش‌های Dockerfile:
FROM python:3.9:

این خط پایه‌ی ایمیج داکر را مشخص می‌کند. در اینجا از ایمیج رسمی پایتون نسخه ۳.۹ استفاده شده است.
ENV PYTHONDONTWRITEBYTECODE 1:

این دستور از نوشتن فایل‌های pyc جلوگیری می‌کند.
ENV PYTHONUNBUFFERED 1:

این دستور خروجی پایتون را بدون بافر به ترمینال ارسال می‌کند.
WORKDIR /code:

این خط دایرکتوری کاری درون کانتینر را به /code تنظیم می‌کند.
COPY requirements.txt /code/:

این دستور فایل requirements.txt را از سیستم محلی به دایرکتوری /code درون کانتینر کپی می‌کند.
RUN pip install --no-cache-dir -r requirements.txt:

این خط پکیج‌های پایتون موجود در requirements.txt را نصب می‌کند.
COPY . /code/:

این دستور باقی کدهای برنامه را به دایرکتوری /code درون کانتینر کپی می‌کند.
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]:

این خط فرمانی است که هنگام راه‌اندازی کانتینر اجرا می‌شود. در اینجا، سرور توسعه جنگو روی پورت ۸۰۰۰ اجرا می‌شود.

docker-compose.yml
docker-compose.yml فایلی است که سرویس‌های مختلف برنامه را تعریف می‌کند. این فایل مشخص می‌کند که چه کانتینرهایی باید ساخته و چگونه به یکدیگر متصل شوند.

بخش‌های docker-compose.yml:
version: '3.8':

نسخه‌ای از Docker Compose که استفاده می‌شود را مشخص می‌کند.
services:

سرویس‌های مختلفی که باید اجرا شوند را تعریف می‌کند.
db:

این سرویس مربوط به پایگاه داده PostgreSQL است.
image: postgres:13: مشخص می‌کند که از ایمیج رسمی PostgreSQL نسخه ۱۳ استفاده شود.
volumes: برای نگهداری داده‌ها حتی پس از متوقف شدن کانتینر.
environment: متغیرهای محیطی لازم برای کانفیگ PostgreSQL.
web:

این سرویس مربوط به برنامه جنگو است.
build: از Dockerfile برای ساخت ایمیج استفاده می‌کند.
command: فرمانی که برای اجرای برنامه جنگو استفاده می‌شود.
volumes: کد برنامه را به کانتینر متصل می‌کند.
ports: پورت‌های محلی و کانتینر را متصل می‌کند.
depends_on: مشخص می‌کند که این سرویس به سرویس db وابسته است.
environment: متغیرهای محیطی لازم برای اتصال به پایگاه داده.
volumes:

تعریف ولوم‌های داکر برای نگهداری داده‌ها.

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


## پرسش‌ها

۱. وظایف Dockerfile، image و container را توضیح دهید.

پاسخ: Docker image فایلی است که برای اجرای کد در یک Docker container استفاده می‌شود. به عبارتی دیگر یک Docker image، یک پکیج استاندارد شامل تمامی فایل‌های، کتابخانه‌ها و تنظیمات لازم برای اجرای یک container است. دو اصل مهم درباره imageها وجود دارد: یک اینکه imageها قابل تغییر نیستند و پس از ایجاد، نمی‌توانند تغییر کنند. همچنین باید توجه داشت که imageها شامل چندین لایه هستند و هر لایه نماینده مجموعه‌ای از تغییرات فایل سیستم است که فایل‌ها را اضافه، حذف، یا ویرایش می‌کنند.

یک Docker container، یک پکیج نرم‌افزاری قابل اجرا است که شامل توامی موارد لازم برای اجرای یک قطعه از نرم‌افزار می‌باشد. این containerها به صورت فرایندهای isolated عمل می‌کنند. برای مثال اگر یک پروژه شامل دیتابیس، کد پایتون، و فرانت React باشد، هر کدام از این اجزا در محیط مخصوص به خود و کاملا جدا از هر چیز دیگری در سیستم اجرا می‌شوند.

هر Container، تمامی موارد برای اجرا را بدون نیاز به وابستگی‌های سیستم دارد. همچنین containerها مستقل هستند و حذف یک container اثری بر دیگر containerها ندارد. Containerها قابل جابجایی هستند و می‌توانند در بقیه سیستم‌ها از جمله data centerها و فضای ابری نیز اجرا شوند.

یک Dockerfile، یک سند متنی است که شامل تمامی دستوراتی است که یک کاربر می‌تواند در command line وارد کند تا یک image را سر هم کند. دستوراتی شامل ADD، ARG، CMD و ... را می‌توان در این فایل استفاده کند. دستورالعمل‌های موجود در Dockerfile به ترتیب اجرا می‌شوند.

۲. از kubernetes برای انجام چه کارهایی می‌توان استفاده کرد؟ رابطه آن با داکر چیست؟

پاسخ: Kubernetes یک سیستم open source برای خودکارسازی مدیریت، جایگذاری، مقیاس‌بندی، و مسیریابی containerها است. 

با توجه به اینکه Docker یک محیط برای ساخت و اجرای containerها فراهم می‌کند و با استفاده از Kubernetes نیز می‌توان این containerها را مدیریت کرد، به کارگیری هر دو ابزار به ما این امکان را می‌دهد که پروژه‌ها را در مقیاس بزرگ به صورت container درآورده و مدیریت کنیم.


### منابع
۱. https://docs.docker.com/guides/docker-concepts/the-basics/what-is-a-container/

۲. https://docs.docker.com/guides/docker-concepts/the-basics/what-is-a-container/

۳. https://docs.docker.com/reference/dockerfile/

۴. https://www.docker.com/resources/kubernetes-and-docker/

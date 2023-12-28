---
title: "از گوگل به تلگرام!"
date: 2023-12-18T00:08:38+03:30
draft: false
image: '/images/post/colab.jpg'
Categories: [
    'tech',
    'cloud',
    'python',
]
tags: [
    'نحوه استفاده از گوگل کولب',
    'اپلود فایل به تلگرام بدون دانلود',
    'چطوری با سرور به تلگرام فایل اپلود کنیم',
    'اپلود فایل به تلگرام بدون نیاز به سرور',
]
---

به احتمال زیاد سایت
google colab
رو می‌شناسید، این سایت برای کسانی که پایتون کار می‌کنند
یک معجزه‌ست. خیلی راحت شما می‌توانید کد پایتون خود
را روی سرور های گوگل اجرا کنید! 
مثلا اگر قرار باشد یک فایل با حجم خیلی زیاد رو پردازش کنید و
خب سیستم خودتون توان پردازش اون فایل رو نداره،
این سرویس گوگل واقعا کار رو راحت می‌کنه.
حالا فرض کنید این فایل رو باید با تیم به اشتراک بگذارید،
خیلی راحت این سروریس به گوگل درایو شما متصل می‌شه
و فایل رو براتون آپلود می‌کنه.

اما من توی این پست می‌خواهم کار متفاوتی با این سرویس 
انجام بدم، جای اینکه فایل‌م رو توی گوگل درایو آپلود کنم 
می‌خواهم که یک پلی لیست موزیک راک رو که فایل با فرمت
zip
هست رو روی
google colab
دانلود کنم و بعد با همین کولب فایل 
ها را استخراج و به تلگرام اپلود کنم.

اینو بگم که خب روی کولب خیلی تریک های جالب دیگه‌ای هم
می‌زنند، مثلا من یه تایمی به کولب
ssh
می‌کردم و بعنوان
vpn
استفاده می‌کردم.


---

ادرس سرویس
google colab
[این](https://colab.research.google.com/)
هست، ولی من یک اسکریپت نوشتم که به اون نیاز داریم
برای همین
[این لینک](https://colab.research.google.com/github/shabane/upload-to-telegram/blob/master/upload_to_telegram.ipynb)
رو باز کنید
تا اسکریپتی که من نوشته‌م توی
colab
باز بشه. اگه تا الان با برنامه
jupyter
کار کرده باشین این محیط باید براتون خیلی آشنا باشه.
اگر نه، خیلی ساده بگم، این محیط دستورات پایتون رو
براتون روی سروس های گوگل ران می‌کنه :)

خب ما یه فایل این برای مثالمون نیاز داریم، من یک پلی 
لیست راک از
AC/DC
که از تورنت می‌گیرم رو برای اینکار استفاده می‌کنم.

اول من فایل رو باید دانلود کنم، من از تورنت برای دانلود
موزیک، فیلم و ... استفاده می‌کنم.
پس کافیه لینک تورنت رو به برنامه
aria2c
بدم
و یا فایل رو با یک سایت دانلودر تورنت دانلود کنم و لینک‌ش
رو بدم به برنامه
wget

برای اینکه دستورات خودتون رو اجرا کنید از بالای سایت
گزینه
*+ code*
رو بزنید تا یک
code snipp
جدید براتون باز کنه. توی این قسمت فقط می‌شه کد های
پایتون رو اجرا کرد ولی اگه از علامت
**!**
قبل از دستور استفاده کنیم اون رو بعنوان دستور
bash
اجرا خواهد کرد. و خب بیس سیستم عامل هم دبیان هست
و دستورات دبیانی رو اجرا می‌کنه.

پس اگه مستقیم قرار بود فایل رو از تورنت بگیرم باید برنامه
aria2
رو نصب می‌کردم. برای نصب کافیه این دستور رو بزنیم

```bash
!sudo apt-get install aria2
```

> دقت کنید که از علامت
> `!`
> قبل از دستور استفاده کردم.

بعد از نصب، لینک رو می‌دیم به برنامه تا فایل ها رو برامون 
دانلود کنه.

```bash
!aria2c '<your magnet link>'
```

> به این دقت کنید که برنامه با کامند
> aria2c
> اجرا می‌شه. ولی با به اسم
> aria2
> نصب می‌شه.

و یا اینکه لینک دانلود مستقیم رو به برنامه
wget
می‌دیم. این برنامه به صورت دیفالت نصب هست و لازم نیست
کار خاصی بکنیم.

```bash
!wget '<your file link>'
```

![from google to telegram as cloud](/images/post/wgetcolab.jpg)

بعد از اینکه فایل روی سرور گوگل دانلود شد کافیه فایل
رو به تلگرام آپلود کنیم! ولی قبل‌ش  چند کاری هست که باید
باهم بکنیم.

- فایل من زیپه پس باید استخراج کنم.
- فایل ها توی دایرکتوری های تو در تو قرار دارند، و باید همه فایل های مورد نظرم رو که توی این مورد
mp3
هستند رو بیارم داخل پوشه‌ای که اسکریپت دیفالت آنجا ران 
می‌شه.
- نیاز به یک کانال تلگرام دارم که فایل ها مستقیم اونجا 
اپلود بشوند
- یک توکن ربات تلگرام هم نیازه که با استفاده از دسترسی
ربات به کانال، فایل ها رو اپلود کنیم.


خب برای اسختراج فایل ها خیلی ساده از کامند 
`unzip`
و اسم فایل جلوی این کامند استفاده می‌کنیم.

```bash
!unzip <your file name>
```

> با کامند
> `ls`
> نام فایلی که دانلود کردین رو می‌تونید ببینید.

![uizip in colab](/images/post/unzipcolab.jpg)


با اجرای کامند
`tree`
می‌تونید دایرکتوری های تو در تو رو ببینید.

![tree in colab](/images/post/treecolab.jpg)

توی این مرحله من همه فایل های
mp3
رو باید بیارم توی پوشه اصلی که اسکریپت‌ اپلود اجرا میشه،
دلیل این کار این هست که این اسکریپ‌ت فقط فایل های
mp3
داخل همین پوشه رو پیدا و اپلود می‌کنه.

برای انتقال این فایل ها از پوشه های تو در تو به یک پوشه از کامند
`find`
استفاده می‌کنم و هر فایل که پیدا می‌شه رو با کامند
`mv`
به پوشه اصلی‌مون انتقال می‌دم.

```bash
!find . -type f -iname '*mp3' -exec mv {} . \;
```

بیایید سوییچ های این دستور رو بهتون بگم

- type-

این سوییچ انواع نوع فایل ها رو مشخص می‌کنه. مثلا شما می‌تونید دنبال یک دایرکتوری، فایل، سوکت و ... بگردین

- iname-

این سویچ که مشخص هست نام فایل رو ازتون می‌گیره که شما می‌تونید پترن ریجکس بدین بهش.
و اون
**i**
مشخص می‌کنه که نام فایل به کوچیکی و بزرگی حساس نباشه.

- exec- 

یک کامندی رو ازتون می‌گیره و روی تک تک ریزالت های جست و جو اون کامند رو اجرا می‌کنه.
من کامند
`mv`
رو دادم که برای انتقال فایل استفاده می‌شه. علامت های 
**{}**
هم یک متغییر هست که نام فایل تک به تک جای اون قرار می‌گیره.

> پایان سوییچ
> exec
> همیشه با
> `;\`
> مشخص می‌شه.

خب تا اینجا که خیلی ساده بود،‌ نه؟

مرحله آخر اینه که یک ربات با
[این ربات](https://t.me/BotFather)
بسازین، و بعد اون رو عضو یک کانالی که می‌سازید بکنید، کانال رو فعلا پابلیک بسازید و یک یوزرنیم بهش بدین.

توکن ربات رو کپی کنید و توی اسکریپت من، جایی که نوشته
<your token>
وارد کنید و توی قسمت
<chat id>
هم ایدی کانالتون رو با علامت @ اول جایگذاری کنید.

![ac and dc telegram channel](/images/post/acdccolab.jpg)

> اگه کانال پرایوت دارید باید بجای ای‌دی کانال از 
> chatid 
> استفاده کنید، که یک ای‌دی عددی هست.

**تمام**!‌
کافیه برنامه رو اجرا کنید و کانال تلگرامتون رو نگاه کنید :)

> اگه خیلی زیاد دارین فایل اپلود می‌کنید احتمالا تلگرام ارور
> flood mood 
> بهتون بده. توی این مورد کافیه فقط صبر کنید، اسکریپت از اجرا قطع نمی‌شه و روی همون فایل مجددا اجرا می‌شه
> تا بالاخره فایل اپلود بشه.

> هرچیزی رو می‌تونید آپلود کنید ولی دقت کنید که ربات ها محدودیت حجم اپلود دارن که نهایتش 50MB هست

> اینم بگم که توی اسکریپت اگه 
> mp3
> رو به چیز دیگه‌ای تغییر بدین، اسکریپت به دنبال اون پسوند ها خواهد گشت.

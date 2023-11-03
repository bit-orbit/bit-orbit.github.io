---
title: "تفاوت Staticlink و Dynamiclink در زبان های برنامه نویسی"
date: 2023-11-03T00:21:46+03:30
draft: false
image: images/post/staticvsdynamic.jpg
categories: [
    "programming",
    "software engineering",
]
tags: [
    "فرق بین static linking و dynamic linking چیست",
    "تفاوت بین static linking و dynamic linking",
    "static linking vs dynamic linking",
]
---

یکسری مفاهیمی در برنامه نویسی داریم که من تصمیم داریم درباره آنها توی چند پست توضیح بدم، اولین مورد
که شامل این پست می‌شه تفاوت
static linking
و
dynamic linking
در زبان های برامه نویسی هست‌ش. مفاهیم دیگه‌ای که داخل پست های بعدی می‌نویسم چیز هایی مثل
**static type, dynamic type, compiler language, interpreter language** و ...
خواهد بود.

---

- دو مفهموم
static linking
و
dynamic linking
چه هستند؟

- هر کدام چه مشکلات و فوایدی دارند؟

---

ما در نوشتن برنامه ها معمولا از لایبراری های زیادی استفاده می‌کنیم، حتی یک برانامه ساده.
سیستم ما برای اجرای یک برنامه، کد برنامه رو به ماشین-کد تبدیل می‌کنه و بعد خط به خط
اون ها رو اجرا می‌کنه.

اصطلاح
linking
به روندی گفته می‌شود که کد شما به اشیاء(کد و داده) های خارجی برای استفاده از انها اشاره کند.
معمولا دو روش برای اینکار وجود دارد،
static linking(لینک کردن ایستا),
dynamic linking(لینک کردن پویا).

> static linking
> زمانی اتفاق می‌افتد که کامپایلر منابع خارجی(لایبراری) ها را داخل فایل اجرایی برنامه کپی کند

در واقع در این روش برنامه شما تمامی لایبراری هایی که شما در کد استفاده کردین رو
با کد شما درون یک فایل اجرایی کامپایل می‌کند، زمانی که برنامه اجرا شود، تمامی لایبراری های
مورد نیازش وارد مموری کامپیوتر می‌شود.

فایده هایی که می‌تونم بهش اشاره کنم

- توی منتشر کردن برنامه به دیگران، مشکل نصب کامپوننت ها رو دیگه ندارن، و به راحتی اجرا می‌کنند.
- ممکنه برنامه اندکی سریع تر اجرا بشه

این روش واقعا خوبیه ولی خب معایب خودش رو هم داره

- فایل اجرایی حجم زیادی‌تری نسب‌ت به لینک کردن پویا داره
- وقتی برنامه اجرا بشه، بخاطر اینکه همه لایبراری و داده های مورد نیاز داخل همان فایل اجرایی بود، برنامه رم زیادی استفاده می‌کنه.
- اگر لایبراری های مورد استفاده تغییر کنند، برنامه نویس باید مجدد برنامه رو کامپایل و منتشر کنه


> dynamic linking
> لینک کردن پویا زمانی است که نام لایبراری خارجی در زمان اجرا وارد فایل اجرایی می‌شود.
> و اینکه فقط در این زمان است که لایبراری مورد نیاز وارد مموری می‌شود.

در این مواقع لایبراری ها باید در سیستم عامل از قبل نصب شده باشند، و یا در موقغ نصب برنامه جدید
اون لایبراری ها نصب شوند. و اگر هر تغییری در لایبرار رخ بده، برنامه نویس نیاز نیست دوباره برنامه رو
کامپایل و منتشر کند، فقط لایبراری مورد نیاز اپدیت می‌شود.

این روش هم مزیت های خودش رو داره، مثلا:

- حجم فایل های اجرایی خیلی کمتری داره
- تا زمانی که نیاز به لایبراری نشده باشد(اگر در کد نویسی رعایت شده باشه) لایبراری ها رم اشغال نمی‌کنند.

مهمترین عیبی که می‌تونه این روش داشته باشه اینه که
حذف شدن و یا خراب شدن لایبراری باعث می‌شه برنامه دیگه اجرا نشه.

---

> این مطلب خلاصه‌ای کلی بود برای آشنایی با این دو مفهموم
> برای مطالعه بیشتر این
> [لینک](https://blog.hubspot.com/website/static-vs-dynamic-linking#static-linking)
> می‌تونه مفید باشه.

---
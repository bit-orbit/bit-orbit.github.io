---
title: "کپی کردن ریزالت پایپ به کلیپ برد"
date: 2022-05-01T04:48:44+04:30
draft: false
author: arya
image: images/post/cclp.jpg
categories: ['projects']
---

<div dir='rtl' style='font-size:23px'>


ما زمانی که برنامه‌ای در ترمینال اجرا می‌کنیم برای کپی کردن ریزالت اون برنامه، با موس تکست رو سلکت می‌کنیم و بعد کپی می‌کنیم. اما می‌دونیم که توی لینوکس ما pipe رو داریم.
پایپ کردن به این معنی است که شما یک برنامه رو اجرا می‌کنید و ریزالت اون برنامه رو بعنوان ورودی به یک برنامه دیگر می‌دهید.
و خب کاش می‌شد با پایپ کردن، متنی کپی بشه!

برنامه‌ای ساده نوشتم که این کار رو برای ما به سادگی انجام بده.

مثلا ما می‌خواهیم لیست فایل ها و دایرکتوری های داخل پوشه /var/ رو کپی کنیم.
می‌دونیم که دستور ls میاد و لیست فایل ها رو می‌گیره، کافیه این رو پایپ کنیم به clp تا این لیست به کلیپ برد کپی بشه.

`ls /var | clp`


برای نصب این چند دستور رو می‌تونید اجرا کنید:

<div dir='ltr'>

```bash
cd /tmp/ && wget 'https://raw.githubusercontent.com/shabane/clp/master/clp.py'
cp clp.py ~/.local/bin/clp
chmod +x ~/.local/bin/clp
clp -h
```

</div>

<br>

و خب می‌تونید برای دیدن مثال ها ریپوی گیتهاب برنامه رو هم ببینید

https://github.com/shabane/clp

</div>
---
title: "ساخت کوتاه کننده لینک با گیتهاب پیجز"
date: 2022-03-22T22:41:20Z
draft: false
image: images/post/carbon_r7tm.jpg
author: Arya
categories: ['projects']

---

<div dir='rtl' style='font-size: x-large'>

شاید تا به حال از کوتاه کننده های لینک استفاده کرده باشید،
اما ایا سایت *کوتاه کننده لینک* خودتون رو درست کردین؟

گیتهاب پیجز یک سرور رایگان برای دیپلوی سایت های ایستا است.
در واقع شما می‌توانید صفحه های
html
رو توی ریپو قرار بدین و این سایت اون صفحه ها رو به رایگان
برای شما نمایش  می‌دهد.

از همین قابلیت برای ساخت یک 
url redirector
استفاده کردم، و با کمی جاوا اسکریپت
یک صفحه درست کردم که وقتی شما با مرورگر اون رو باز می‌کنید،
مرورگر شما به صورت خودکار به یک آدرس متفاوت منتقل خواهد شد.

ولی مشکل اینجاست که شما هر بار باید کلی
html, css
رو خودتان تغییر بدین و بعد قطعه کد جاوا اسکریپت رو اضافه کنید.

---

<div>
<h2>
<img style="width: 5%;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAAABmJLR0QA/wD/AP+gvaeTAAAE50lEQVRoge2aW2wUVRiAvzPbC2BSuYTL7laDCYEIgZgCMWpILLuFpMYHNPapRHvZECHWQoQSLmYRAlaMtI032G25v0AiL0ignbUmqBiDxIBoEIw+tLstSMFGge7uzO/D7jZtKWXa7tbS7Pd49sx//m/OzOzM+Q+ksYjXq+H1aqkeRqUy+Ax932INrRYkW6Ay5PacTdVYKRHJDRx0ihnZJYri3mOok6ZNe6stv+TPZI+ZVJHc746NN+90rgeqgAkgdxE+iI+0AdR44A5QrU3I2d3yfNHdZI2dNBFHwP8yQh0wMx6619m/f5akVYm2qdVdehilZLjjD1vEEWjIQ8waYEmsRS503w8iKrfpwKyWZSVXE/3tum+JghpQefGmsyitMugqvTCcPIYs4tQPThGi74KsAWzATYHtoY6cjykqMhyBhjxMsw7FXGzG7GD+qr+6DxZRTr1hpSipBmYAooQj4Qzbhhv5JW0jIjLv2LGsW1M6KxC2AI8DYYS6e5Fx2zsKizun6b7pGaidwBuABgSVyKutBZ7v+8aafOpIzrjMe1tRVABZwN8odky6mVN3uagonDIRh97gBrMWmBtrEd3E9nabu/SXhef3ZgZv21Yr2BYXjID67F44e2tHYXHnQHFzm/2zTIOdwGuxsFxTGptaXeXHkyri1PfNFrSPgJfiTVc0Tda1LPWcAnA2+l2iUQvMSwiKUBEq8PxqNREAZ8C3VIQaUPPj2QU007a2paDk0rBEnjz76aRoV1YVsJbY1N9CUT3pZs6ey0VF4fvOJFxVwtrWgvIvByPQi2Zvht1wliqldiBMBaIK1WCE1Za2wtIbgxPxejXnC08Ui2I3yDTAVMLRqBl5p335m9ennzn0mE3rWo9SVcA44F9EPpwQubvrWmFF15AlehA7idlekNVABtAh8F7I1vIJ+d6oJRGH7rvY7/SKKPtX9cVKeB9wACZwIIpsuu72tCdDoC+5Tfvnm5qxB8EVa5FLQbdngUURvwCIUq+EXGUnAOyN9YuUJnXAc/Fu58RUFaFlZedTIdAXe6B+hRL5AiDoLr8v74yBDk5IOHRfAchp4o9TUWwMLS07kox/ZKuEXGUnHLr/gb9ber22TzS/Bi4C1RFb15yQqzwprxUPw6H7mhy6r8lK3wFnJMGPi1ZFaPYu7u8mSy3KbbWn9Q+eEZcYHCn/chsp0iKjjbTIaCMtMtpIi4w20iKjjbTIaMO6SLPX0iv//4Wl5Bae35sZum37Ad1/JmLr2nEjf80/qU4shuhWe1qakdBt7UVgAVCVaWRfsQf8KxFJaW0FIOj2FATdngIrfQcUsQfqV8QDNompngXOAQ4lHHIE6r+1N9YvGn661kjk8iDGzHJQvzMS/Kb1GSXqdVDXEVymMn5yNvkPTW/8fGrIVX7YMLJmI7INCAOlGajfHU0+76xTddlJEzjtn+zQ62tNZVyIS3QIVAZtrXn99R/wOp/ZvH9i2DA2MgqWTMUW3dyrNDEYkQT9LGL/poR1iYT7W8ROrNIPxiEWR/b0vKyNKJXty8t/ftixQykr1PSXcFLLCnBVKTYnvazQkzFR6OnJIEpvT2Mz5vS6vrtX+7tLb6YSjkaUrB/q0y9dDO1LvDxdCzwVD33SMMyK9uWeP+ARKE/3ZExsGOjJI7+Foy8juakm9YzQNqcxw39CC6i8SsB1XwAAAABJRU5ErkJggg==" alt="carbon carbon-link-shortener arya-shabane shabane mohamad-shabane">
Carbon
</h2>
</div>


[![carbon-shot](/images/post/scarbon.jpg)](/images/post/scarbon.jpg)

من یک برنامه کوتاه کننده لینک درست کردم که
زمانی که شما برنامه رو اجرا کنید، فایل های مورد نیاز رو برای شما
می‌سازه.

برنامه رو که اجرا کنید یک
CLI
اجرا خواهد شد و از شما چند ورودی را درخواست خواهد کرد.

اولین ورودی لینک طولانی است که
شما نیاز دارید آن را کوتاه کنید.

در قدم دوم، برنامه یک نام که همان لینک کوتاه شده است را از شما خواهد گرفت،
این نام را اگر به صورت خالی رها کنید، یک نام تصادفی بر اساس فایل کانفیگ
در نظر گرفته خواهد شد.

شاید سایت هایی را دیده باشید که از کاربر می‌خواند برای بازکردن لینک
روی یک دکمه کلیک کنند، و در این فرصت که کاربر هنوز وارد لیک اصلی نشده است،
تبلیغاتی را به آنها نمایش می‌دهند. پس سوال سوم این خواهد بود که آیا
کاربر باید برای وارد شدن به لینک اصلی، دکمه‌ای را بزند و یا به صورت
خودکار وارد آن شود؟

سوال سوم از شما یک عنوان می‌خواهد که برای هدر سایت
از آن استفاده کند، البته اگر تم سایت را کاستومایز کنید، این عنوان
در هر جایی ممکن است قرار بگیرد.

و آخرین سوال از شما یک توضیح برای لینک می‌خواهد. ممکن شما
بخواهید قبل از اینکه کاربر وارد لینک اصلی شود یک متنی را بخواند.
این توضیحات به کاربر نمایش داده خواهد شد.

برنامه کربن را منبع باز داخل این
[آدرس](https://shabane.github.io/carbon/on_open)
در گیتهاب قرار دادم، امیدوارم که مفید باشه.

---

## Deployment

البته همینطور که می‌دانید نیاز نیست حتما از گیتهاب پیجز برای
دیپلوی استفاده کنید. و تنها مشکلی که در این قضیه هست
لینک هایی هست که تولید می‌شوند، این لینک ها به ساب دامین اکانت شما
متصل می‌شوند، پس حفظ آن ها برای شما ساده است :)
اما نکته اینجاست که کافیست شما یک دامنه ثبت کنید.

راه دوم این است که با یک وب سرور مثل
nginx
این سایت را مستقر کنید، و هرگاه نیاز به ساخت لینک کوتاه داشتید،
برنامه رو اجرا کنید.

---

## Theme

من هیچ وقت دیزانر خوبی نبودم و به همین دلیل تم اصلی سایت ساده‌س.
شما می‌توانید به راحتی تم خودتان رو بسازید، کافیه که
[داکیومنت ساخت](https://github.com/shabane/carbon/blob/master/documentation/theme.md)
تم رو مطالعه کنید.
فقط و فقط چند تگ و فایل هست که باید ازش درون تم استفاده کنید، اگر تمی ساختید که
قشنگتر از تم اصلی هست، خوشحال می‌شم به ریپو اصلی پوش کنید.


</div>
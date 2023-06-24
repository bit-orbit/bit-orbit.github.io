---
title: "اسنپ شات گرفتن از یک صفحه سایت با تمامی فایل های وابسته‌‌ش(عکس، استایل، جاوا اسکریپت)"
date: 2022-01-17T14:56:14+03:30
draft: false
author: 'arya'
image: images/post/snp.jpg
categories: ['tech']
tags: [
    'tech',
    'linux',
    'how-to-snapshot-a-single-site-page',
    'wget',
    'mirror-a-single-web-page',
    'اسنپ شات گرفتن از یک صفحه سایت با تمامی فایل های وابسته‌‌ش',
    'اسنپ شات گرفتن از یک صفحه سایت'
]
---

<div dir='rtl' style='font-size:23px'>

زمانی که مقاله یا پستی را می‌خوانم، گاهی احساس می‌کنم ممکنه یک مطلب را فراموش کنم.
مثلا اگه مقاله‌ای از ویرگول درباره یک دستور لینوکس بخوانم، برای اینکه مطلبش را
فراموش نکنم باید هرزگاهی به پست سر بزنم و مطالب را مرور کنم. اما فکر کنید مطلب
درباره برنامه‌ای باشد که می‌توان با استفاده از آن سانسور را دور زد،
ویرگول احتمالا آن مطلب را حذف می‌کند. 

- چه راهی برای ذخیره پست پشنهاد می‌کنید که بعدا بتوان آن را بارها مرور کرد؟

- اگر پست از سایت منبع حذف شود، و یا حتی خود سایت دیگر در دسترس نباشد،
باز هم پستی که ذخیره کرده‌اید در دسترس است؟

در این پست با استفاده از ابزار
wget
یک پست را با تمامی وابستگی هایش مانند عکس ها، فایل های
js، 
فایل های
css
دانلود می‌کنیم و روی سیستم خودمان ذخیره می‌کنیم تا حتی اگر سایت
در دسترس نباشد
و یا پست
از منبع حذف شد، پست به صورت افلاین در دسترس بماند.

---

### wget
می‌دونید که
wget
یک ابزار پیشرفته برای دانلود فایله، قابلیت های زیادی که داره باعث شده
بعد از سالها که ابزار های پیشرفته دانلود هم نوشته شدن، همچنان
wget
استفاده بشه. این برنامه روی اکثر توزیع های لینوکس به صورت دیفالت نصبه پس بیایید 
بدون تلف کردن وقت
سوییچ های مهم برای میرور کردن یک صفحه سایت رو برسی کنیم.


</br></br>


**Ⅰ.** `--adjust-extension`, `-E`

شاید شما هم سایتی هایی را دیده باشید که
*url*
صفحه هایشان با پسوند
`.asp`
به پایان می‌رسند.
برای مثال نگاهی به
[این آدرس](https://www.w3schools.com/python/default.asp)
بیاندازید
*https://www.w3schools.com/python/default.asp*
همانطور که می‌بینید با
*asp*
به پایان می‌رسد. زمانی که شما این صفحه را دانلود کنید،
این صفحه بجای پسوند
*html.*
با پسوند
*asp.*
ذخیره خواهد شد.

> به همین دلیل ما از این سوییچ استفاده می‌کنیم تا صفحه ها را
> با پسوند
> *html.*
> ذخیره کنیم.

<div dir='ltr'>
</br>

```bash
$ https://example.com/some/where

# where.html
```
</div>

</br></br>


**Ⅱ.** `--span-hosts`, `-H`

اگر حتی فقط یک صفحه را میرور می‌کنیم، آن صفحه ممکن است به فایل های خارج از سایت
که به روی یک
CDN
قرار گرفته اند
نیاز داشته باشد، مثل عکس ها و یا
style sheet
ها. از اینرو وقتی شما یک صفحه را میرور می‌کنید
wget
فایل های خارج از سایت را دانلود نخواهد کرد.

> پوشش دادن فایل های خارج از سایت با استفاده از این سوییچ انجام داده می‌شود.
> وقتی این سوییچ را روشن کنید
> wget
> همه لینک ها از جمله لینک هایی زیر دامنه را هم دانلود خواهد کرد، مگر اینکه عمق 
> پوشش دادن را محدود کنید.


</br></br>


**Ⅲ.** `--convert-links`, `-k`

یک صفحه را که میرور کردیم با توجه به اینکه فایل های پیشیاز مثل عکس و استایل ها را 
هم دانلود کردیم، وقتی فایل را با مرورگر باز کنیم همچنان صفحه
html
ما به فایل های داخل سایت منبع لینک شده است و فایل های پیشنازش را حتی با اینکه
روی سیستم داریم، باز هم از آدرس خارجی دریافت می‌کند و نمایش می‌دهد.

> برای اینکه یک صفحه فایل های پیشنیازش را از فایل هایی که در کنار صفحه اصلی دانلود
> کرده است بگیرد، از این سوییچ استفاده می‌کنیم. این سوییچ تمامی لینک ها را به
> لینک های داخلی تبدیل می‌کند و صفحه موقع لود شدن از فایل های داخلی استفاده می‌کند.


</br></br>


**Ⅳ.** `--page-requisites`, `-p`

> این سوییچ باعث می‌شود که
> wget
> تمامی فایل های ضروری برای درست نمایش داده شدن 
> html
> را دانلود کند. این فایل ها شامل عکس ها، استایل ها و یا صدا ها می‌شود.


</br></br>


**Ⅴ.** `--no-directories`, `-nd`

به صورت پیشفرض زمانی که صفحه‌ای با این ادرس را دانلود کنیم

`https://example.com/some/where/page.html`

برنامه
wget
این صفحه و فایل های مورد نیازش را به این صورت ذخیره خواهد کرد

`some/where/page.html/.`

اما با استفاده از سوییچ
`nd-`
آن صفحه و فایل های مورد نیازش فقط داخل یک دایرکتوری ذخیره خواهد شد. به این صورت

`page.html/.`

> با فعال کردن این سوییچ
> wget
> صفحه و فایل های پیشنیاز را داخل دایرکتوری فعلی ذخیره خواهد کرد. و اگر
> نام یک فایل بیش از یکبار تکرار شود، به نام فایل پسوند اضافه خواهد شد.


</br></br>

### مثال کاربردی و خروجی

- برای مثال من یک
[پست توی ویرگول](https://vrgl.ir/c3fVa)
که آموزش الگوریتم
RSA
هست رو دانلود می‌کنم.

</br>

</div>


```bash
┌─[loading] [/tmp/virgool] [0]  
│
└──〉wget -nd -E -p -k -H https://vrgl.ir/c3fVa
--2022-01-18 06:41:03--  https://vrgl.ir/c3fVa
Connecting to 192.168.45.250:8080... connected.
Proxy request sent, awaiting response... 302 Found
Location: https://virgool.io/@Novo/rsa-encryption-ao1poasym4cf [following]
--2022-01-18 06:41:04--  https://virgool.io/@Novo/rsa-encryption-ao1poasym4cf
Connecting to 192.168.45.250:8080... connected.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: ‘c3fVa.html’

c3fVa.html                          [  <=>                                                  ]  61.30K   200KB/s    in 0.3s    

2022-01-18 06:41:06 (200 KB/s) - ‘c3fVa.html’ saved [62773]

Loading robots.txt; please ignore errors.
--2022-01-18 06:41:06--  https://virgool.io/robots.txt
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [document]
Saving to: ‘robots.txt’

robots.txt                          [ <=>                                                   ]     484  --.-KB/s    in 0s      

2022-01-18 06:41:07 (85.7 MB/s) - ‘robots.txt’ saved [484]

Loading robots.txt; please ignore errors.
--2022-01-18 06:41:07--  https://www.googletagmanager.com/robots.txt
Connecting to 192.168.45.250:8080... connected.
Proxy request sent, awaiting response... 404 Not Found
2022-01-18 06:41:08 ERROR 404: Not Found.

Loading robots.txt; please ignore errors.
--2022-01-18 06:41:08--  https://files.virgool.io/robots.txt
Connecting to 192.168.45.250:8080... connected.
Proxy request sent, awaiting response... 403 Forbidden
2022-01-18 06:41:09 ERROR 403: Forbidden.

Loading robots.txt; please ignore errors.
--2022-01-18 06:41:09--  https://static.cloudflareinsights.com/robots.txt
Connecting to 192.168.45.250:8080... connected.
Proxy request sent, awaiting response... 522 
2022-01-18 06:41:26 ERROR 522: (no description).

--2022-01-18 06:41:26--  https://virgool.io/images/favicon.png?v=v2.6.15
Connecting to 192.168.45.250:8080... connected.
Proxy request sent, awaiting response... 200 OK
Length: 9855 (9.6K) [image/png]
Saving to: ‘favicon.png?v=v2.6.15’

favicon.png?v=v2.6.15           100%[======================================================>]   9.62K  --.-KB/s    in 0.07s   

2022-01-18 06:41:27 (130 KB/s) - ‘favicon.png?v=v2.6.15’ saved [9855/9855]

--2022-01-18 06:41:27--  https://virgool.io/css/styles.css?v=v2.6.15
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [text/css]
Saving to: ‘styles.css?v=v2.6.15.css’

styles.css?v=v2.6.15.css            [       <=>                                             ] 903.60K   592KB/s    in 1.5s    

2022-01-18 06:41:28 (592 KB/s) - ‘styles.css?v=v2.6.15.css’ saved [925284]

Loading robots.txt; please ignore errors.
--2022-01-18 06:41:28--  https://virgool.test/robots.txt
Connecting to 192.168.45.250:8080... connected.
Proxy tunneling failed: Bad GatewayUnable to establish SSL connection.
--2022-01-18 06:41:28--  https://www.googletagmanager.com/ns.html?id=GTM-5GS688K
Connecting to 192.168.45.250:8080... connected.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: ‘ns.html?id=GTM-5GS688K.html’

ns.html?id=GTM-5GS688K.html         [ <=>                                                   ]     266  --.-KB/s    in 0s      

2022-01-18 06:41:29 (29.4 MB/s) - ‘ns.html?id=GTM-5GS688K.html’ saved [266]

--2022-01-18 06:41:29--  https://files.virgool.io/upload/users/31434/avatar/1C4LLE.png?x-img=v1/resize,h_120,w_120/optimize,q_100
Connecting to 192.168.45.250:8080... connected.
Proxy request sent, awaiting response... 200 OK
Length: 39194 (38K) [image/png]
Saving to: ‘1C4LLE.png?x-img=v1%2Fresize,h_120,w_120%2Foptimize,q_100’

1C4LLE.png?x-img=v1%2Fresize,h_ 100%[======================================================>]  38.28K   173KB/s    in 0.2s    

2022-01-18 06:41:30 (173 KB/s) - ‘1C4LLE.png?x-img=v1%2Fresize,h_120,w_120%2Foptimize,q_100’ saved [39194/39194]

--2022-01-18 06:41:30--  https://files.virgool.io/upload/users/31434/posts/ao1poasym4cf/qhokakomwyb5.png
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 179870 (176K) [image/png]
Saving to: ‘qhokakomwyb5.png’

qhokakomwyb5.png                100%[======================================================>] 175.65K   567KB/s    in 0.3s    

2022-01-18 06:41:31 (567 KB/s) - ‘qhokakomwyb5.png’ saved [179870/179870]

--2022-01-18 06:41:31--  https://files.virgool.io/upload/users/31434/posts/ao1poasym4cf/v1xnwpnewfys.webp
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 12770 (12K) [image/webp]
Saving to: ‘v1xnwpnewfys.webp’

v1xnwpnewfys.webp               100%[======================================================>]  12.47K  --.-KB/s    in 0.07s   

2022-01-18 06:41:31 (180 KB/s) - ‘v1xnwpnewfys.webp’ saved [12770/12770]

--2022-01-18 06:41:31--  https://files.virgool.io/upload/users/31434/posts/ao1poasym4cf/d28hpu2detkl.png
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 92847 (91K) [image/png]
Saving to: ‘d28hpu2detkl.png’

d28hpu2detkl.png                100%[======================================================>]  90.67K  --.-KB/s    in 0.09s   

2022-01-18 06:41:31 (969 KB/s) - ‘d28hpu2detkl.png’ saved [92847/92847]

--2022-01-18 06:41:31--  https://files.virgool.io/upload/users/31434/posts/ao1poasym4cf/rhyykrc8azq0.png
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 17539 (17K) [image/png]
Saving to: ‘rhyykrc8azq0.png’

rhyykrc8azq0.png                100%[======================================================>]  17.13K  --.-KB/s    in 0.007s  

2022-01-18 06:41:32 (2.47 MB/s) - ‘rhyykrc8azq0.png’ saved [17539/17539]

--2022-01-18 06:41:32--  https://files.virgool.io/upload/users/31434/posts/ao1poasym4cf/fjtc1ydzkuse.png
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 108524 (106K) [image/png]
Saving to: ‘fjtc1ydzkuse.png’

fjtc1ydzkuse.png                100%[======================================================>] 105.98K  --.-KB/s    in 0.1s    

2022-01-18 06:41:32 (1.06 MB/s) - ‘fjtc1ydzkuse.png’ saved [108524/108524]

--2022-01-18 06:41:32--  https://files.virgool.io/upload/users/31434/posts/ao1poasym4cf/gfqhgyp18oic.jpeg
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 132085 (129K) [image/jpeg]
Saving to: ‘gfqhgyp18oic.jpeg’

gfqhgyp18oic.jpeg               100%[======================================================>] 128.99K  --.-KB/s    in 0.1s    

2022-01-18 06:41:32 (1.28 MB/s) - ‘gfqhgyp18oic.jpeg’ saved [132085/132085]

--2022-01-18 06:41:32--  https://files.virgool.io/upload/users/31434/posts/ao1poasym4cf/me0mpjtdmyom.png
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 113489 (111K) [image/png]
Saving to: ‘me0mpjtdmyom.png’

me0mpjtdmyom.png                100%[======================================================>] 110.83K  --.-KB/s    in 0.1s    

2022-01-18 06:41:33 (814 KB/s) - ‘me0mpjtdmyom.png’ saved [113489/113489]

--2022-01-18 06:41:33--  https://files.virgool.io/upload/users/31434/posts/ao1poasym4cf/of7krju6oqzk.png
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 105746 (103K) [image/png]
Saving to: ‘of7krju6oqzk.png’

of7krju6oqzk.png                100%[======================================================>] 103.27K  --.-KB/s    in 0.08s   

2022-01-18 06:41:33 (1.25 MB/s) - ‘of7krju6oqzk.png’ saved [105746/105746]

--2022-01-18 06:41:33--  https://files.virgool.io/upload/users/31434/posts/ao1poasym4cf/oqwhflg4gbnq.png
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 120994 (118K) [image/png]
Saving to: ‘oqwhflg4gbnq.png’

oqwhflg4gbnq.png                100%[======================================================>] 118.16K  --.-KB/s    in 0.1s    

2022-01-18 06:41:33 (1.14 MB/s) - ‘oqwhflg4gbnq.png’ saved [120994/120994]

--2022-01-18 06:41:33--  https://files.virgool.io/upload/users/31434/posts/ao1poasym4cf/taqjnfnuzxcx.png
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 122678 (120K) [image/png]
Saving to: ‘taqjnfnuzxcx.png’

taqjnfnuzxcx.png                100%[======================================================>] 119.80K  --.-KB/s    in 0.06s   

2022-01-18 06:41:34 (2.00 MB/s) - ‘taqjnfnuzxcx.png’ saved [122678/122678]

--2022-01-18 06:41:34--  https://files.virgool.io/upload/users/63/posts/qwqrubplfaau/g8vai2incgna.jpeg?x-img=v1/resize,w_300/optimize,q_100
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 88292 (86K) [image/jpeg]
Saving to: ‘g8vai2incgna.jpeg?x-img=v1%2Fresize,w_300%2Foptimize,q_100’

g8vai2incgna.jpeg?x-img=v1%2Fre 100%[======================================================>]  86.22K  --.-KB/s    in 0.1s    

2022-01-18 06:41:34 (824 KB/s) - ‘g8vai2incgna.jpeg?x-img=v1%2Fresize,w_300%2Foptimize,q_100’ saved [88292/88292]

--2022-01-18 06:41:34--  https://files.virgool.io/upload/users/63/avatar/o8jiNe.png?x-img=v1/resize,h_120,w_120/optimize,q_100
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 32037 (31K) [image/png]
Saving to: ‘o8jiNe.png?x-img=v1%2Fresize,h_120,w_120%2Foptimize,q_100’

o8jiNe.png?x-img=v1%2Fresize,h_ 100%[======================================================>]  31.29K  --.-KB/s    in 0.06s   

2022-01-18 06:41:34 (558 KB/s) - ‘o8jiNe.png?x-img=v1%2Fresize,h_120,w_120%2Foptimize,q_100’ saved [32037/32037]

--2022-01-18 06:41:34--  https://files.virgool.io/upload/users/31434/posts/e3oydlcfkr5e/figl9y1c6otq.jpeg?x-img=v1/resize,w_300/optimize,q_100
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 30356 (30K) [image/jpeg]
Saving to: ‘figl9y1c6otq.jpeg?x-img=v1%2Fresize,w_300%2Foptimize,q_100’

figl9y1c6otq.jpeg?x-img=v1%2Fre 100%[======================================================>]  29.64K  --.-KB/s    in 0.07s   

2022-01-18 06:41:35 (401 KB/s) - ‘figl9y1c6otq.jpeg?x-img=v1%2Fresize,w_300%2Foptimize,q_100’ saved [30356/30356]

--2022-01-18 06:41:35--  https://files.virgool.io/upload/users/316150/posts/r8rhw88uh1xh/cs1ugyelv3zw.png?x-img=v1/resize,w_300/optimize,q_100
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 34459 (34K) [image/png]
Saving to: ‘cs1ugyelv3zw.png?x-img=v1%2Fresize,w_300%2Foptimize,q_100’

cs1ugyelv3zw.png?x-img=v1%2Fres 100%[======================================================>]  33.65K  --.-KB/s    in 0.08s   

2022-01-18 06:41:35 (399 KB/s) - ‘cs1ugyelv3zw.png?x-img=v1%2Fresize,w_300%2Foptimize,q_100’ saved [34459/34459]

--2022-01-18 06:41:35--  https://files.virgool.io/upload/users/316150/avatar/8sz7Rs.jpeg?x-img=v1/resize,h_120,w_120/optimize,q_100
Reusing existing connection to files.virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 14764 (14K) [image/jpeg]
Saving to: ‘8sz7Rs.jpeg?x-img=v1%2Fresize,h_120,w_120%2Foptimize,q_100’

8sz7Rs.jpeg?x-img=v1%2Fresize,h 100%[======================================================>]  14.42K  --.-KB/s    in 0.007s  

2022-01-18 06:41:35 (2.11 MB/s) - ‘8sz7Rs.jpeg?x-img=v1%2Fresize,h_120,w_120%2Foptimize,q_100’ saved [14764/14764]

--2022-01-18 06:41:35--  https://virgool.io/js/commons.js?v=v2.6.15
Connecting to 192.168.45.250:8080... connected.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [application/javascript]
Saving to: ‘commons.js?v=v2.6.15’

commons.js?v=v2.6.15                [           <=>                                         ] 997.55K   337KB/s    in 3.0s    

2022-01-18 06:41:39 (337 KB/s) - ‘commons.js?v=v2.6.15’ saved [1021492]

--2022-01-18 06:41:39--  https://virgool.io/js/main.js?v=v2.6.15
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [application/javascript]
Saving to: ‘main.js?v=v2.6.15’

main.js?v=v2.6.15                   [         <=>                                           ]   1.21M   580KB/s    in 2.1s    

2022-01-18 06:41:41 (580 KB/s) - ‘main.js?v=v2.6.15’ saved [1267632]

--2022-01-18 06:41:41--  https://static.cloudflareinsights.com/beacon.min.js/v652eace1692a40cfa3763df669d7439c1639079717194
Connecting to 192.168.45.250:8080... connected.
Proxy request sent, awaiting response... 200 OK
Length: 13970 (14K) [text/javascript]
Saving to: ‘v652eace1692a40cfa3763df669d7439c1639079717194’

v652eace1692a40cfa3763df669d743 100%[======================================================>]  13.64K  --.-KB/s    in 0.02s   

2022-01-18 06:41:42 (600 KB/s) - ‘v652eace1692a40cfa3763df669d7439c1639079717194’ saved [13970/13970]

--2022-01-18 06:41:42--  https://virgool.io/fonts/vazir/Vazir-Thin.ttf
Connecting to 192.168.45.250:8080... connected.
Proxy request sent, awaiting response... 200 OK
Length: 98924 (97K) [application/octet-stream]
Saving to: ‘Vazir-Thin.ttf’

Vazir-Thin.ttf                  100%[======================================================>]  96.61K   140KB/s    in 0.7s    

2022-01-18 06:41:43 (140 KB/s) - ‘Vazir-Thin.ttf’ saved [98924/98924]

--2022-01-18 06:41:43--  https://virgool.io/fonts/vazir/Vazir-Thin.eot?
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [application/vnd.ms-fontobject]
Saving to: ‘Vazir-Thin.eot?’

Vazir-Thin.eot?                     [  <=>                                                  ]  96.80K   276KB/s    in 0.4s    

2022-01-18 06:41:44 (276 KB/s) - ‘Vazir-Thin.eot?’ saved [99120]

--2022-01-18 06:41:44--  https://virgool.io/fonts/vazir/Vazir-Thin.woff
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 54068 (53K) [font/woff]
Saving to: ‘Vazir-Thin.woff’

Vazir-Thin.woff                 100%[======================================================>]  52.80K  --.-KB/s    in 0.07s   

2022-01-18 06:41:45 (770 KB/s) - ‘Vazir-Thin.woff’ saved [54068/54068]

--2022-01-18 06:41:45--  https://virgool.io/fonts/vazir/Vazir-Thin.woff2
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 43000 (42K) [font/woff2]
Saving to: ‘Vazir-Thin.woff2’

Vazir-Thin.woff2                100%[======================================================>]  41.99K  --.-KB/s    in 0.006s  

2022-01-18 06:41:46 (6.59 MB/s) - ‘Vazir-Thin.woff2’ saved [43000/43000]

--2022-01-18 06:41:46--  https://virgool.io/fonts/vazir/Vazir-Light.ttf
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 105360 (103K) [application/octet-stream]
Saving to: ‘Vazir-Light.ttf’

Vazir-Light.ttf                 100%[======================================================>] 102.89K   478KB/s    in 0.2s    

2022-01-18 06:41:46 (478 KB/s) - ‘Vazir-Light.ttf’ saved [105360/105360]

--2022-01-18 06:41:46--  https://virgool.io/fonts/vazir/Vazir-Light.eot?
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [application/vnd.ms-fontobject]
Saving to: ‘Vazir-Light.eot?’

Vazir-Light.eot?                    [  <=>                                                  ] 103.09K   470KB/s    in 0.2s    

2022-01-18 06:41:47 (470 KB/s) - ‘Vazir-Light.eot?’ saved [105562]

--2022-01-18 06:41:47--  https://virgool.io/fonts/vazir/Vazir-Light.woff
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 59688 (58K) [font/woff]
Saving to: ‘Vazir-Light.woff’

Vazir-Light.woff                100%[======================================================>]  58.29K  --.-KB/s    in 0.08s   

2022-01-18 06:41:48 (758 KB/s) - ‘Vazir-Light.woff’ saved [59688/59688]

--2022-01-18 06:41:48--  https://virgool.io/fonts/vazir/Vazir-Light.woff2
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 48168 (47K) [font/woff2]
Saving to: ‘Vazir-Light.woff2’

Vazir-Light.woff2               100%[======================================================>]  47.04K  --.-KB/s    in 0.08s   

2022-01-18 06:41:48 (570 KB/s) - ‘Vazir-Light.woff2’ saved [48168/48168]

--2022-01-18 06:41:48--  https://virgool.io/fonts/vazir/Vazir-Regular.ttf
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 98376 (96K) [application/octet-stream]
Saving to: ‘Vazir-Regular.ttf’

Vazir-Regular.ttf               100%[======================================================>]  96.07K   435KB/s    in 0.2s    

2022-01-18 06:41:49 (435 KB/s) - ‘Vazir-Regular.ttf’ saved [98376/98376]

--2022-01-18 06:41:49--  https://virgool.io/fonts/vazir/Vazir-Regular.eot?
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [application/vnd.ms-fontobject]
Saving to: ‘Vazir-Regular.eot?’

Vazir-Regular.eot?                  [  <=>                                                  ]  96.26K   285KB/s    in 0.3s    

2022-01-18 06:41:50 (285 KB/s) - ‘Vazir-Regular.eot?’ saved [98574]

--2022-01-18 06:41:50--  https://virgool.io/fonts/vazir/Vazir-Regular.woff
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 53744 (52K) [font/woff]
Saving to: ‘Vazir-Regular.woff’

Vazir-Regular.woff              100%[======================================================>]  52.48K  --.-KB/s    in 0.05s   

2022-01-18 06:41:51 (1.10 MB/s) - ‘Vazir-Regular.woff’ saved [53744/53744]

--2022-01-18 06:41:51--  https://virgool.io/fonts/vazir/Vazir-Regular.woff2
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 42744 (42K) [font/woff2]
Saving to: ‘Vazir-Regular.woff2’

Vazir-Regular.woff2             100%[======================================================>]  41.74K  --.-KB/s    in 0.1s    

2022-01-18 06:41:51 (412 KB/s) - ‘Vazir-Regular.woff2’ saved [42744/42744]

--2022-01-18 06:41:51--  https://virgool.io/fonts/vazir/Vazir-Medium.ttf
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 98892 (97K) [application/octet-stream]
Saving to: ‘Vazir-Medium.ttf’

Vazir-Medium.ttf                100%[======================================================>]  96.57K   606KB/s    in 0.2s    

2022-01-18 06:41:52 (606 KB/s) - ‘Vazir-Medium.ttf’ saved [98892/98892]

--2022-01-18 06:41:52--  https://virgool.io/fonts/vazir/Vazir-Medium.eot?
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [application/vnd.ms-fontobject]
Saving to: ‘Vazir-Medium.eot?’

Vazir-Medium.eot?                   [  <=>                                                  ]  96.78K   347KB/s    in 0.3s    

2022-01-18 06:41:53 (347 KB/s) - ‘Vazir-Medium.eot?’ saved [99100]

--2022-01-18 06:41:53--  https://virgool.io/fonts/vazir/Vazir-Medium.woff
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 54120 (53K) [font/woff]
Saving to: ‘Vazir-Medium.woff’

Vazir-Medium.woff               100%[======================================================>]  52.85K   125KB/s    in 0.4s    

2022-01-18 06:41:54 (125 KB/s) - ‘Vazir-Medium.woff’ saved [54120/54120]

--2022-01-18 06:41:54--  https://virgool.io/fonts/vazir/Vazir-Medium.woff2
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 43152 (42K) [font/woff2]
Saving to: ‘Vazir-Medium.woff2’

Vazir-Medium.woff2              100%[======================================================>]  42.14K  --.-KB/s    in 0.08s   

2022-01-18 06:41:54 (541 KB/s) - ‘Vazir-Medium.woff2’ saved [43152/43152]

--2022-01-18 06:41:54--  https://virgool.io/fonts/vazir/Vazir-Bold.ttf
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 99840 (98K) [application/octet-stream]
Saving to: ‘Vazir-Bold.ttf’

Vazir-Bold.ttf                  100%[======================================================>]  97.50K  --.-KB/s    in 0.04s   

2022-01-18 06:41:55 (2.61 MB/s) - ‘Vazir-Bold.ttf’ saved [99840/99840]

--2022-01-18 06:41:55--  https://virgool.io/fonts/vazir/Vazir-Bold.eot?
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [application/vnd.ms-fontobject]
Saving to: ‘Vazir-Bold.eot?’

Vazir-Bold.eot?                     [ <=>                                                   ]  97.68K   560KB/s    in 0.2s    

2022-01-18 06:41:56 (560 KB/s) - ‘Vazir-Bold.eot?’ saved [100026]

--2022-01-18 06:41:56--  https://virgool.io/fonts/vazir/Vazir-Bold.woff
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 54560 (53K) [font/woff]
Saving to: ‘Vazir-Bold.woff’

Vazir-Bold.woff                 100%[======================================================>]  53.28K  --.-KB/s    in 0.06s   

2022-01-18 06:41:56 (917 KB/s) - ‘Vazir-Bold.woff’ saved [54560/54560]

--2022-01-18 06:41:56--  https://virgool.io/fonts/vazir/Vazir-Bold.woff2
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 43604 (43K) [font/woff2]
Saving to: ‘Vazir-Bold.woff2’

Vazir-Bold.woff2                100%[======================================================>]  42.58K  --.-KB/s    in 0.06s   

2022-01-18 06:41:57 (657 KB/s) - ‘Vazir-Bold.woff2’ saved [43604/43604]

--2022-01-18 06:41:57--  https://virgool.io/fonts/vazir/Vazir-Black.ttf
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 99772 (97K) [application/octet-stream]
Saving to: ‘Vazir-Black.ttf’

Vazir-Black.ttf                 100%[======================================================>]  97.43K  --.-KB/s    in 0.04s   

2022-01-18 06:41:57 (2.18 MB/s) - ‘Vazir-Black.ttf’ saved [99772/99772]

--2022-01-18 06:41:57--  https://virgool.io/fonts/vazir/Vazir-Black.eot?
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [application/vnd.ms-fontobject]
Saving to: ‘Vazir-Black.eot?’

Vazir-Black.eot?                    [  <=>                                                  ]  97.63K   478KB/s    in 0.2s    

2022-01-18 06:41:58 (478 KB/s) - ‘Vazir-Black.eot?’ saved [99974]

--2022-01-18 06:41:58--  https://virgool.io/fonts/vazir/Vazir-Black.woff
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 54652 (53K) [font/woff]
Saving to: ‘Vazir-Black.woff’

Vazir-Black.woff                100%[======================================================>]  53.37K  --.-KB/s    in 0.08s   

2022-01-18 06:41:59 (691 KB/s) - ‘Vazir-Black.woff’ saved [54652/54652]

--2022-01-18 06:41:59--  https://virgool.io/fonts/vazir/Vazir-Black.woff2
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 43332 (42K) [font/woff2]
Saving to: ‘Vazir-Black.woff2’

Vazir-Black.woff2               100%[======================================================>]  42.32K  --.-KB/s    in 0.08s   

2022-01-18 06:42:00 (510 KB/s) - ‘Vazir-Black.woff2’ saved [43332/43332]

--2022-01-18 06:42:00--  https://virgool.io/fonts/fontawesome/fa-solid-900.ttf
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 202616 (198K) [application/octet-stream]
Saving to: ‘fa-solid-900.ttf’

fa-solid-900.ttf                100%[======================================================>] 197.87K   501KB/s    in 0.4s    

2022-01-18 06:42:01 (501 KB/s) - ‘fa-solid-900.ttf’ saved [202616/202616]

--2022-01-18 06:42:01--  https://virgool.io/fonts/fontawesome/fa-solid-900.eot?
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 404 Not Found
2022-01-18 06:42:01 ERROR 404: Not Found.

--2022-01-18 06:42:01--  https://virgool.io/fonts/fontawesome/fa-solid-900.woff
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 103300 (101K) [font/woff]
Saving to: ‘fa-solid-900.woff’

fa-solid-900.woff               100%[======================================================>] 100.88K   358KB/s    in 0.3s    

2022-01-18 06:42:02 (358 KB/s) - ‘fa-solid-900.woff’ saved [103300/103300]

--2022-01-18 06:42:02--  https://virgool.io/fonts/fontawesome/fa-solid-900.woff2
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 79444 (78K) [font/woff2]
Saving to: ‘fa-solid-900.woff2’

fa-solid-900.woff2              100%[======================================================>]  77.58K   394KB/s    in 0.2s    

2022-01-18 06:42:03 (394 KB/s) - ‘fa-solid-900.woff2’ saved [79444/79444]

--2022-01-18 06:42:03--  https://virgool.io/images/icons/plus.svg
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [image/svg+xml]
Saving to: ‘plus.svg’

plus.svg                            [ <=>                                                   ]     942  --.-KB/s    in 0.002s  

2022-01-18 06:42:03 (383 KB/s) - ‘plus.svg’ saved [942]

--2022-01-18 06:42:03--  https://virgool.io/images/icons/tick.svg
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [image/svg+xml]
Saving to: ‘tick.svg’

tick.svg                            [ <=>                                                   ]     942  --.-KB/s    in 0s      

2022-01-18 06:42:04 (89.8 MB/s) - ‘tick.svg’ saved [942]

--2022-01-18 06:42:04--  https://virgool.io/images/typewriter.png
Reusing existing connection to virgool.io:443.
Proxy request sent, awaiting response... 200 OK
Length: 172558 (169K) [image/png]
Saving to: ‘typewriter.png’

typewriter.png                  100%[======================================================>] 168.51K   349KB/s    in 0.5s    

2022-01-18 06:42:05 (349 KB/s) - ‘typewriter.png’ saved [172558/172558]

--2022-01-18 06:42:05--  https://virgool.test/images/download-audio.png
Connecting to 192.168.45.250:8080... connected.
Proxy tunneling failed: Bad GatewayUnable to establish SSL connection.
--2022-01-18 06:42:05--  https://virgool.test/images/download-ebook.png
Connecting to 192.168.45.250:8080... connected.
Proxy tunneling failed: Bad GatewayUnable to establish SSL connection.
FINISHED --2022-01-18 06:42:05--
Total wall clock time: 1m 2s
Downloaded: 54 files, 6.6M in 14s (479 KB/s)
Converting links in ns.html?id=GTM-5GS688K.html... nothing to do.
Converting links in c3fVa.html... 38.
31-7
Converting links in styles.css?v=v2.6.15.css... 35.
34-1
Converted links in 3 files in 0.06 seconds.


```


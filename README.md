# FakeTlsTunnel

تست شده روی سرور های آسیا تک و آروان (بامداد) 

قابلیت MUX در ورژن ۸ هم اکنون در دسترس است !

# مقدمه کلی

این برنامه به طور کلی برای زنده کردن کانفیگ های tcp base مثل trojan-vmess-vless انجام شده برای دوستانی که تونل کار میکنند ؛ یه نسخه از این برنامه روی سرور تونل باید اجرا کنید
و یه نسخه دیگه هم روی سرور خارج اتون ؛ با این روش میتونید صرف نظر از نوع کانفیگ و مشخصاتش
tls handshake رو با دامنه کاملا دلخواه تکمیل کنید و بدون کوچیک ترین تغییری روی کانفیگ های 
دست کاربران و یا حتی تغییر خاصی روی پنل هاتون 

این برنامه رو همچنین میتویند روی کانفیگ های مستقیم هم اعمال کنید اگه نسخه تونل اش رو روی سیستم شخصیتون ران کنید و به عنوان پروکسی ازش استفاده کنید. که البته هدف اصلی من از
نوشتنش برای ران شدن در تونل بوده ولی کلی این نکته رو هم گفتم.

این برنامه یه ترکیب از ایده تغییر sni پروتوکل reality و ایده های شخصی هست ؛ که اگر وقت پیدا کنم قراره اپدیت هایی مثل looad balancing هم بهش اضافه کنم

این برنامه دربرابر active detection فایر وال مقاوم شده و تست شده است.  

# نحوه ستاپ برای تونل

اول دقت کنید که یه نسخه از این برنامه روی سرور ایران باید ران شه و یه نسخه روی سرور خارج
 
# سرور ایران

قبل از هر کاری باید نرم افزار تونل قبلی خودتونو off کنید.
سپس با این دستور فایل اجرایی برنامه رو دانلود کنید

```sh
wget  "https://raw.githubusercontent.com/radkesvat/FakeTlsTunnel/master/install.sh" -O install.sh && chmod +x install.sh && bash install.sh 
```

حالا میتونید برنامه رو به این شکل اجرا کنید 
```sh
./FTT --tunnel --lport:443 --toip:88.104.1.1  --toport:443 --sni:github.com --password:123ab
```
--lport :

میگه که روی چه پورتی ران بشه ؛ این پورت مثلا میتونه 443 باشه یا پورت های دیگه.  و همون پورتی هست که کانفیگ دست کاربر بهش اشاره میکنه ؛ امیدوارم از پنل هایی استفاده کرده باشین که روی 
یه پورت کاربر میسازن ؛ وگر نه باید issue بزارید تا من توی اپدیت بعد حالت listen all ports رو اضافه کنم

--toip:

آیپی سرور خارجتون هست ؛ فقط ۱ ایپی اینجا بزارید اما تو اپدیت های بعدی حالت لود بالانسینگ هم اضافه میشه

--toport:

  پورت سرور خارجتونه ؛ این پورت خیلی خیلی مهمه که ۴۴۳ باشه ؛ مهم نیست که پنل اتون روی کدوم پورت داره کار میکنه ؛ پنل باید روی یه پورتی به جز 443 ران باشه اما وقتی ما نسخه دوم این برنامه 
  رو که روی سرور خارج امون اجرا کردیم ؛ باید اونجا بگیم که پورت ۴۴۳ رو گوش بده ؛ اگه پورتی به جز ۴۴۳ استفاده کنید احتمال فیلتر شدن بالا هست
بعدا در این مورد وقتی سرور خارج رو ستاپ میکنیم بیشتر توضیح میدم

--sni:

  این همون دامنه ایی هست که میخواهیم باهاش handshake رو کامل کنیم ؛ برخلاف reality شما لازم نیست سایت رو اسکن کنید ؛ هر دامنه ای که توی مرورگرتون باز میشه رو میتونید اینجا وارد کنید 
  حتی مثلا google.com
  هردامنه ای که اینجا وارد کردین؛ دقیقا همینو باید سمت سرور هم وارد کنید وگر نه کار نمیکنه.
  
--password:
 
این رمز برای رمزنگاری دیتا بین تونل و سرور مقصد استفاده میشه ؛ هرچی دوست دارید وارد کنید اصلا مهم نیست حتی 123 چون ما از hash این پسورد استفاد میکنیم
هر پسوردی که اینجا وارد کردین؛ دقیقا همینو باید سمت سرور هم وارد کنید وگر نه کار نمیکنه.
  
خوب توضیحات سرور تونل ایران شد ؛ حالا باید روی سرور خارج این برنامه رو ران کنیم و بعد تونل به کار میفته

# سرور خارج

اولین نکته اینه که پورت ۴۴۳ رو باید خالی کنید ؛ پنل یا nginx و کلا هرچی دارید از این پورت بردارید بزارید یه پورت دیگه.

برنامه رو اول دانلود کنید
```sh
wget  "https://raw.githubusercontent.com/radkesvat/FakeTlsTunnel/master/install.sh" -O install.sh && chmod +x install.sh && bash install.sh
```
حالا میتونیم به این شکل اجرا کنیم 
```sh
./FTT --server --lport:443 --toip:127.0.0.1 --toport:443 --sni:github.com --password:123ab
```

--lport :

این پورت که گفتم برای امنیت بیشتر باید ۴۴۳ باشه ؛ همون پورتی هست که توی کامند سرور ایران بود --toport
پس اگه اونجا چیزی به جز 443 گزاشتین ؛ اینجا هم همینو بزارید

--toip:

اینجا میگیم که پکت ها به کودوم ایپی فرستاده بشن ؛ که چون روی سرور خارج هستیم میگیم به همین سرور ارسال بشن  
پس مقدارش باید همین 127.0.0.1 باشه

--toport:

این پارامتر باید پورتی باشه که پنل کانفیگتون رو روش آماده کرده ؛ دقت کنید پورت وبسایت پنل یا nginx نیست. پورت خود کانفیگ هست مثلا اگه یوزر رو روی پورت 2000 ساختین اینجا هم باید 2000 وارد بشه
نکته مهم اینه که اکثرا کانفیگ هاشونو روی پورت ۴۴۳ میسازن ؛ شما باید در پنل اتون این پورت رو یه چیزی به جز ۴۴۳ بزارین چون ۴۴۳ رو خود برنامه تونل میخواد بگیره اما 
نیازی نیست که کانفیگی که به کاربر دادید رو پورت اش رو تغییر بدین ؛ برای اون همون ۴۴۳ باید باقی بمونه ؛ برنامه تونل پکت هارو روی همون ۴۴۳ باید بگیره و میفرسته به این پورت
امیدوارم متوجه منظورم شده باشید ؛‌ یه مثال کامل اخر کار میزنم اونم بخونید 


 --sni
 
 دقیقا همون چیزی که روی سرور ایران گزاشتید 

--password

دقیقا همون پسوری که روی سرور ایران گزاشتید



# یه مثال درست
 
ایپی سرور خارجم : 88.1.2.3

ایپی سرور ایرانم : 5.4.3.2


پنل من روی پورت 443 کانفیگ های تروجان میساخت و میدادم به کاربر

حالا اول توی پنل میام پورت 443 رو تغییر میدم به 2083

بعد روی سرور ایران این دستورو ران میکنم

```sh
./FTT --tunnel --lport:443 --toip:88.1.2.3  --toport:443 --sni:github.com --password:123ab
```

و روی سرور خارج ام هم این دستور را وارد میکنم

```sh
./FTT --server --lport:443 --toip:127.0.0.1 --toport:2083 --sni:github.com --password:123ab
```

بدون اینکه کانفیگ دست کاربر روش تغییری اجرا بشه ؛ ستاپ تونل ما تکمیل شد.

# نکته مهم در مورد sni

پارامتر sni و مقدارش خیلی مهمه ؛ به جرعت میتونم بگم اگه از ستاپتون مطمعن هستید و دیتا منتقل نمیشه مشکل همین دامنه هست ؛ با کلی دامنه تست میکردم روی آسیاتک و دیتا رد نمیشد اما یه دامنه
سایت خیلی معروف که همتون ۱۰۰ درصد میشناسید زدم و کار کرد با پینگ و جیتر خیلی عالی

برای انتخاب دامنه ؛ پیشناهاد میکنم دامنه هایی که سرورشون توی ایرانه استفاده نشه و دامنه های معروف خارجی یا کلا هر دامنه خارجی ایی که بدون فیلتر شکن میتونید واردش بشید ؛‌ میتونه دامنه خوبی باشه
تست کنید خلاصه

یه sni تمیز میزارم اینجا امیدوارم وقتی میخونید فیلتر نشده باشه
data.services.jetbrains.com


# حمایت
اگه خواستین میتونید با این لینک ترون ؛‌ دونیت کنین


```sh
THMbaTeDjeEygjVZQu91k9cqVw1v4TsdAk
```

اینترنت یا برای همه یا برای هیچکس!

به امید آزادی




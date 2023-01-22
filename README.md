# v2board


سلام دوستان عزیز امروز میخواهیم با هم نصب و راه اندازی V2board را با هم انجام دهیم .

در قدم اول یک سرور مجازی برای خودمان نصب میکنیم  سیستم عامل CentOS  راه اندازی میکنیم که من سرو مجاری خودم را است سایت 
Linode

تهیه کردم.

نکته‌ای که باید به آن توجه کنید ورژن سیستم عامل شما نباید کمتر از ۷ باشد و باید ورژنی بالاتر از ۷ انتخاب کنید در این ویدیو ما با ورژن ۷ جلو می رویم.

بعد از نصب کردن سیستم عامل با افزار دلخواه خود به سرور خود وصل شوید و سرور خود را با دستور زیر آپدیت کنید من از نرم افزار پوتی استفاده کردم.

sudo yum update

و بعد از آپدیت شدن سرور با دستور زیر aaPanel نصب می کنید

yum install -y wget && wget -O install.sh http://www.aapanel.com/script/install_6.0_en.sh && bash install.sh

بعد از نصب شدن به شما آدرس دسترسی به پنل خود را نمایش می دهد و همچنین یوزرنیم و پسورد شما آنها را کپی کرده و در جایی بنویسید.

بعد از ورود به پنل خود نرم افزارها را نصب کنید مانند ویدیو این کار می تواند ۳۰ دقیقه یا چند ساعت زمان ببرد لطفاً صبور باشید.

در aaPanel  به App Store بروید و در قسمت Deployment این دو را هم نصب کنید .

PM2 Manager

Redis

در زمانی که نرم افزار ها در حال نصب هستند وارد سایت کلود فلر شوید و یک ساب دامین تعریف کنید و IPv4 سرور خود 
Cloudflare

را به او بدهید در یک رکورد A مانند ویدیو.

به  aaPanel  خود برمی گردیم  و بعد از نصب مسیر زیر میرویم

App Store>Installed>PHP-7.4>Setting>Install extensions

و دو برنامه زیر را نصب میکنیم

fileinfo

redis

بعد از نصب به منوی Website رفته و Add site را میزنیم

در پنجره باز شده در قسمت Domain name دامنه خود را دهید و در دیتابیس MySQL را انتخاب کنید و روی Submit کلیک کنید در پنچره بار شده اطلاعات رو کپی کنید و در جایی نگهداری کنید .

برای گرفتن سرتیفیکیت SSL در منوی وب‌سایت بر روی  Conf کلیک کنید.

به منو SSL بروید و سربرگ Let's Encrypt نام دامین خود را انتخاب کنید و مانند ویدیو جلو بروید.

حالا به مسیر زیر می رویم.

aaPanel> App Store >PHP 7.4 >Setting > Disabled functions

و فایل های زیر را پاک میکنیم مانند ویدیو 

putenv, proc_open, pcntl_alarm, pcntl_signal

حالا به دایرکتوری خودمان که نام وب سایت ما است وارد می شویم و دستور زیر را اجرا می کنیم

cd /www/wwwroot/Domain name ****
chattr -i .user.ini
rm -rf .htaccess 404.html index.html .user.ini

و بعد با دستور زیر برنامه را دانلود و نصب کنید 

git clone https://github.com/v2board/v2board.git ./

اجرا

sh init.sh

اگر زمان نصب با ارور برخورد کردید به دایرکتوری خود رفته و فایل های داخل آن را پاک کنید مانند ویدیو.

حالا مانند ویدیو جلو بروید و کدها را در جای مناسب خود قرار دهید

Site Name>URL rewrite

location /downloads {
}

location / {  
    try_files $uri $uri/ /index.php$is_args$query_string;  
}

location ~ .*\.(js|css)?$
{
    expires      1h;
    error_log off;
    access_log /dev/null; 
}

در مسیر زیر public را انتخاب کنید 

Site directory > Running directory

حالا به مسیر زیر رفته

aaPanel >Cron

Type of Task  Shell Script

 Name of Task  v2board

Execution cycle  N Minutes 1 Minute

Script content 

php /www/wwwroot/ِDomain Name/artisan schedule:run

خالا در aaPanel ما باید supervisor را نصب کنیم مانند ویدیو 

به مسیر زیر بروید 

aaPanel>Store>Tools

برنامه را نصب کنید و بعد از پایان نصب رو Setting کلیک کنید و بعد روی Add Daemon

حالا تنظیمات رو مانند ویدیو انجام دهید 

Name = V2board

Run User = www

Run Dir =  مسیر دایرکتوری وب شما

Start Command = php artisan horizon

Processes = 1

بعد از انجام روی Confirm میزنیم 

بقیه آموزش را در ویدیو دنبال کنید زیرا به زبان چینی هست کلمات 

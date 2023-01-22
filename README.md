# v2board
https://github.com/cx9208/bbrplus
https://xrayr-project.github.io/XrayR-doc/




سلام دوستان عزیز امروز میخواهیم با هم نصب و راه اندازی V2board را با هم انجام دهیم .

در قدم اول یک سرور مجازی برای خودمان نصب میکنیم  سیستم عامل CentOS  راه اندازی میکنیم که من سرو مجاری خودم را است سایت 
Linode

تهیه کردم.

نکته‌ای که باید به آن توجه کنید ورژن سیستم عامل شما نباید کمتر از ۷ باشد و باید ورژنی بالاتر از ۷ انتخاب کنید در این ویدیو ما با ورژن ۷ جلو می رویم.

بعد از نصب کردن سیستم عامل با افزار دلخواه خود به سرور خود وصل شوید و سرور خود را با دستور زیر آپدیت کنید من از نرم افزار پوتی استفاده کردم.

<pre class="notranslate"><code>sudo yum update</code></pre>

و بعد از آپدیت شدن سرور با دستور زیر aaPanel نصب می کنید

<pre class="notranslate"><code>yum install -y wget && wget -O install.sh http://www.aapanel.com/script/install_6.0_en.sh && bash install.sh
</code></pre>

بعد از نصب شدن به شما آدرس دسترسی به پنل خود را نمایش می دهد و همچنین یوزرنیم و پسورد شما آنها را کپی کرده و در جایی بنویسید.

بعد از ورود به پنل خود نرم افزارها را نصب کنید مانند ویدیو این کار می تواند ۳۰ دقیقه یا چند ساعت زمان ببرد لطفاً صبور باشید.

در aaPanel  به App Store بروید و در قسمت Deployment این دو را هم نصب کنید .

<code>PM2 Manager

Redis</code>

در زمانی که نرم افزار ها در حال نصب هستند وارد سایت کلود فلر شوید و یک ساب دامین تعریف کنید و IPv4 سرور خود 
<code>Cloudflare</code>

را به او بدهید در یک رکورد A مانند ویدیو.

به  aaPanel  خود برمی گردیم  و بعد از نصب مسیر زیر میرویم

<code>App Store>Installed>PHP-7.4>Setting>Install extensions</code>

و دو برنامه زیر را نصب میکنیم

<code>fileinfo

redis</code>

بعد از نصب به منوی Website رفته و Add site را میزنیم

در پنجره باز شده در قسمت Domain name دامنه خود را دهید و در دیتابیس MySQL را انتخاب کنید و روی Submit کلیک کنید در پنچره بار شده اطلاعات رو کپی کنید و در جایی نگهداری کنید .

برای گرفتن سرتیفیکیت SSL در منوی وب‌سایت بر روی  Conf کلیک کنید.

به منو SSL بروید و سربرگ Let's Encrypt نام دامین خود را انتخاب کنید و مانند ویدیو جلو بروید.

حالا به مسیر زیر می رویم.

<code>aaPanel> App Store >PHP 7.4 >Setting > Disabled functions</code>

و فایل های زیر را پاک میکنیم مانند ویدیو 

<code>putenv, proc_open, pcntl_alarm, pcntl_signal</code>

حالا به دایرکتوری خودمان که نام وب سایت ما است وارد می شویم و دستور زیر را اجرا می کنیم

<code>cd /www/wwwroot/Domain name ****
chattr -i .user.ini
rm -rf .htaccess 404.html index.html .user.ini</code>

و بعد با دستور زیر برنامه را دانلود و نصب کنید 

<pre class="notranslate"><code>git clone https://github.com/v2board/v2board.git ./</code></pre>
اجرا

<pre class="notranslate"><code>sh init.sh</code></pre>
اگر زمان نصب با ارور برخورد کردید به دایرکتوری خود رفته و فایل های داخل آن را پاک کنید مانند ویدیو.

حالا مانند ویدیو جلو بروید و کدها را در جای مناسب خود قرار دهید

<code>Site Name>URL rewrite</code>

<pre class="notranslate"><code>location /downloads {
}

location / {  
    try_files $uri $uri/ /index.php$is_args$query_string;  
}

location ~ .*\.(js|css)?$
{
    expires      1h;
    error_log off;
    access_log /dev/null; 
}</code></pre>
    
در مسیر زیر public را انتخاب کنید 

<code>Site directory > Running directory</code>

حالا به مسیر زیر رفته

<code>aaPanel >Cron
Type of Task  Shell Script
 Name of Task  v2board
Execution cycle  N Minutes 1 Minute
Script content 
php /www/wwwroot/ِDomain Name/artisan schedule:run</code>

خالا در aaPanel ما باید supervisor را نصب کنیم مانند ویدیو 

به مسیر زیر بروید 

<code>aaPanel>Store>Tools</code>

برنامه را نصب کنید و بعد از پایان نصب رو Setting کلیک کنید و بعد روی Add Daemon

حالا تنظیمات رو مانند ویدیو انجام دهید 

<code>Name = V2board
Run User = www
Run Dir =  مسیر دایرکتوری وب شما
Start Command = php artisan horizon
Processes = 1</code>

بعد از انجام روی Confirm میزنیم 

بقیه آموزش را در ویدیو دنبال کنید زیرا به زبان چینی هست کلمات


قسمت دوم نصب و کانفیگ V2board  خدمت شما هستم

شما نیاز به یک سرور دیگر دارید اول یک سرور دیگر نصب می کنید  من CentOS 9  نصب میکنم .

وارد سرور خود شوید برنامه زیر را نصب کنید.

ابتدا شما باید برنامه XrayR نصب کنید برای این منظور شما می توانید از کد زیر استفاده کنید یا به وبسایت مرجع بروید.



<pre class="notranslate"><code>bash <(curl -Ls https://raw.githubusercontent.com/XrayR-project/XrayR-release/master/install.sh)</code></pre>

در زمانی که برنامه در حال نصب است شما می توانید به سایت کلود فلر رفته و یک رکورد IPv4   تعریف کنید با یک ساب دامین.

لطفاً مانند ویدیو ادامه دهید

بعد از نصب با وارد کردن کلمه زیر به پنل خود وارد شوید

<code>xrayr</code>

با باز شدن پنل شما می توانید با کپی کردن متن چینی به مترجم گوگل خود به زبان دلخواه خود متن را ببینید

در این مرحله ما عدد 11 را انتخاب می کنیم تا وارد منو  بی بی آر  شوید.

در پنجره بعدی با زدن شما 2   بی بی آر پلاس را نصب کنید بعد از نصب سرور شما باید ریست شود.

اگر بی بی آر پلاس شما نصب نشد از کد زیر برای نصب اقدام کنید 

<pre class="notranslate"><code>wget "https://github.com/cx9208/bbrplus/raw/master/ok_bbrplus_centos.sh" && chmod +x ok_bbrplus_centos.sh && ./ok_bbrplus_centos.sh</code></pre>

و بعد از نصب با دستور زیر سرور خود را ریست کنید

<code>reboot</code>

در زمان نصب میتوانید به پنل V2board  خود وارد شوید و یک اکانتV2Ray بسازید

به دلیل اینکه وب سایت به زبان چینی است نمی شود کلمات را در متن توضیح داد لطفاً مانند ویدیو عمل کنید

در زمان ساخت اکانت شما به این کد نیاز دارید لطفا نام دامین خود را در قسمت Host بنوسسید


<pre class="notranslate"><code>{
  "path": "/",
  "headers": {
    "Host": "v2ray.com"
  }
}
</code></pre>


بقیه مراحل را مانند ویدیو جلو بروید  زیرا زبان چینی هست

در مرحال اول یک سرور جدید ساخته و آن را با کد زیر آپدیت میکنم 

<pre class="notranslate"><code>sudo yum update</code></pre>

بعد از این در کلود فلر یک ساب دامین ساخته و اپی سرور خود را در آن وارد میکنیم 

حالا XrayR را با کد زیر نصب میکنم 

<pre class="notranslate"><code>bash <(curl -Ls https://raw.githubusercontent.com/XrayR-project/XrayR-release/master/install.sh)</code></pre>

حالا با کد زیر بی بی ار پلاس را نصب میکنید 

<pre class="notranslate"><code>wget "https://github.com/cx9208/bbrplus/raw/master/ok_bbrplus_centos.sh"&& chmod +x ok_bbrplus_centos.sh &&./ok_bbrplus_centos.sh</code></pre>


حالا مانند ویدیو کد ها را ادیت میکنید 

<pre class="notranslate"><code>XrayR</code></pre>

برای باز کردن پورت ها از کد زیر استفاده میکنید 

<pre class="notranslate"><code>firewall-cmd --add-port=*****/tcp<code></pre> 

<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="UTF-8">
<title>راهنمای بازگردانی بکاپ Pasarguard Beta</title>
</head>
<body dir="rtl">

<h1>راهنمای حرفه‌ای بازگردانی بکاپ Pasarguard Beta</h1>

<p>این آموزش مراحل کامل ریستور بکاپ Pasarguard Beta را به صورت مرحله به مرحله توضیح می‌دهد.</p>

<h2>مرحله ۱: آماده‌سازی فایل بکاپ</h2>
<p>ابتدا فایل بکاپ خود را در سیستم استخراج کنید.</p>

<p><strong>ساخت پوشه برای استخراج فایل:</strong></p>
<pre><code>mkdir /root/restore/</code></pre>

<p><strong>استخراج فایل ZIP:</strong></p>
<pre><code>unzip /root/restore/نام_فایل.zip -d /root/restore/</code></pre>

<p>⚠️ توجه: در صورت تمایل می‌توانید فایل را با سیستم خود استخراج کنید و تنها فایل MySQL (<code>db_backup.sql</code>) را داخل <code>/root/restore/</code> قرار دهید.</p>

<p>بعد از استخراج، دو پوشه به نام‌های <code>opt</code> و <code>var</code> خواهید داشت.</p>

<h2>مرحله ۲: انتقال فایل‌ها به مسیرهای اصلی</h2>
<p>برای بازگردانی صحیح بکاپ، فایل‌ها را به مسیرهای اصلی منتقل کنید:</p>

<ul>
<li>پوشه داده‌ها: <code>pasarguard_data → /var/lib/pasarguard/</code></li>
<li>فایل‌های Docker: <code>docker-compose.yml و .env → /opt/pasarguard/</code></li>
</ul>

<p>⚠️ اگر از بکاپر <code>erfjab</code> استفاده می‌کنید، هر دو مسیر بالا باید به پوشه‌های اصلی‌شان منتقل شوند.</p>

<h2>مرحله ۳: ریستارت اولیه Pasarguard</h2>
<p>برای اعمال تغییرات اولیه، سرویس را ریستارت کنید:</p>
<pre><code>pasarguard restart</code></pre>

<h2>مرحله ۴: انتقال فایل دیتابیس</h2>
<p>فایل SQL بکاپ (<code>db_backup.sql</code>) را به پوشه <code>/root/restore/</code> منتقل کنید:</p>
<pre><code>cp /مسیر/بکاپ/db_backup.sql /root/restore/</code></pre>

<h2>مرحله ۵: ریستور دیتابیس</h2>
<p>با استفاده از رمز MySQL مناسب، دستور زیر را اجرا کنید:</p>
<pre><code>docker exec -i pasarguard-mysql-1 mysql -u root -p"رمز_دیتابیس" pasarguard &lt; /root/restore/db_backup.sql</code></pre>

<p>مثال‌ها:</p>
<pre><code>docker exec -i pasarguard-mysql-1 mysql -u pasarguard -p"amirpardakhteh39596989" pasarguard &lt; /root/restore/db_backup.sql
docker exec -i pasarguard-mysql-1 mysql -u pasarguard -p"vjHC02lJKDC59zErldtJ" pasarguard &lt; /root/restore/db_backup.sql
docker compose exec -T mysql mysql -u root -p"vjHC02lJKDC59zErldtJ" -h 127.0.0.1 &lt; "/opt/pasarguard/pasarguard.sql"</code></pre>

<h2>مرحله ۶: ریستارت نهایی Pasarguard</h2>
<pre><code>pasarguard restart</code></pre>

<p>✅ بکاپ Pasarguard Beta اکنون با موفقیت بازگردانی شده و سرویس آماده استفاده است.</p>

</body>
</html>

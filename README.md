# RAMO PUMP — Protected Public GitHub Runner

این مخزن برای اجرای صفرهزینه‌ی RAMO روی GitHub Actions عمومی ساخته شده است.

## چرا کد اصلی دیده نمی‌شود؟

هسته RAMO داخل `ramo_payload.enc` با AES-256 رمزگذاری شده است. Workflow فقط در
Runner موقت GitHub و با Secret خصوصی `RAMO_PACKAGE_KEY` آن را باز می‌کند.
بنابراین Public بودن Repository به معنی Public بودن منطق اصلی RAMO نیست.

## Secrets لازم

در Repository > Settings > Secrets and variables > Actions این سه Secret را بساز:

- `RAMO_PACKAGE_KEY`
- `TELEGRAM_RELAY_URL`
- `TELEGRAM_RELAY_SECRET`

Bot Token و Chat ID داخل GitHub لازم نیستند؛ Cloudflare Relay فعلی آن‌ها را نگه می‌دارد.

## اجرای 24/7

Workflow روزی 4 بار اجرا می‌شود و هر بار RAMO حدود 5 ساعت و 45 دقیقه کار می‌کند.
State اسکنر و SQLite بین Runnerهای موقت با GitHub Actions cache منتقل می‌شود تا:

- Alert cooldown حفظ شود
- سقف 5 هشدار در 24 ساعت حفظ شود
- Pump memory حفظ شود
- Market coverage و ranking continuity حفظ شود

به دلیل سقف 6 ساعته‌ی GitHub-hosted jobs، بین پنجره‌ها ممکن است چند دقیقه فاصله باشد.

## اجرای دستی

از تب Actions، workflow به نام `RAMO PUMP 24x7` را باز کن و `Run workflow` بزن.

## امنیت

هیچ‌وقت فایل Private Setup یا مقدار Secretها را Commit نکن.
Fork دیگران Secretهای Repository اصلی را دریافت نمی‌کند، بنابراین Payload رمزگذاری‌شده
را بدون کلید خصوصی نمی‌توانند اجرا کنند.

## محدودیت GitHub

Scheduled workflows در Repository عمومی اگر 60 روز هیچ Repository activity نداشته
باشند ممکن است غیرفعال شوند. قبل از 60 روز یک commit کوچک انجام بده یا workflow را
دوباره فعال کن.

## Disclaimer

RAMO ابزار پژوهشی/تحلیلی است و سیگنال ورود تضمینی یا توصیه مالی نیست.

## رمزنگاری در یک سیستم به‌اشتراک‌گذاری فایل توزیع‌شده
اپلیکیشن Ouisync به کاربران راهی امن برای به‌اشتراک‌گذاری و همگام‌سازی داده‌ها
در دستگاه‌ها ارائه می‌دهد. با توجه به ماهیت توزیع‌شده (همتا‌به‌همتا) Ouisync، که
در آن تغییرات هم‌زمان در فایل‌ها و دایرکتوری‌ها امکان‌پذیر است، ساختار دایرکتوری
Ouisync کاملاً پیچیده است. هر زمان که دو یا چند کاربر فایلی را در یک دایرکتوری
به‌طور همزمان تغییر دهند، معماری Ouisync تضمین می‌کند که هیچ اطلاعاتی از بین
نرود. علاوه بر این، Ouisync همچنین از محتوا (فایل ها و مخازن) و ساختار سیستم‌های
فایل شما با اجرای رمزنگاری سرتاسری محافظت می‌کند.

هنگامی که یک مخزن را در یک حالت خاص (نوشتنی، خواندنی، یا کور Blind) به اشتراک
می‌گذارید، Ouisync کلیدهایی را تولید می‌کند (که «توکن‌ها» نامیده می‌شوند) که
می‌توانید آن‌ها را به‌عنوان پیوند (لینک) یا به‌عنوان کد QR با همتایان خود به
اشتراک بگذارید. وارد کردن یک مخزن با استفاده از یک رمز به دستگاه شما این امکان
را می‌دهد که دایرکتوری‌ها و فایل‌های خود را رمزگشایی کند (به جز در حالت کور
Blind).

اطلاعات هم در حالت استراحت _وقتی ذخیره می‌شوند_ و هم در حال انتقال _در حین
انتقال اطلاعات_ رمزنگاری می‌شوند. نکته مهم، Ouisync می‌تواند بدون رمزگشایی
همگام‌سازی شود و هیچ دستگاهی برای انجام همگام‌سازی نیازی به دانستن کلید رمزگشایی
ندارد. نام فایل‌ها، محتویات فایل و حتی اندازه فایل‌ها و ساختار دایرکتوری‌ها از
همتایان که دارای کلید رمزنگاری نیستند، پنهان است. بنابراین، همتاهایی که فقط
دسترسی کور Blind به مخازن شما دارند، نمی‌توانند محتوای مخازن شما و ساختار آن‌ها
را ببینند.

## کدام الگوریتم‌های رمزنگاری استفاده می‌شود؟
* _در حالت انتقال_، Ouisync از چارچوب [پروتکل نویز]{۱}، به‌ویژه [الگوی NNpsk0]
  استفاده می‌کند. این به Ouisync اجازه می‌دهد تا کلیدهای موقتی را با یک کلید از
  پیش به‌اشتراک‌گذاشته‌شده تولید کند. کلید از پیش به‌اشتراک‌گذاشته‌شده در
  Ouisync شناسه مخزن است. نویز از احراز هویت متقابل و اختیاری، پنهان کردن هویت،
  محرمانگی پیشرو، رمزنگاری صفر رفت و برگشتی و سایر ویژگی‌های رمزنگاری پیشرفته
  پشتیبانی می‌کند.
*   _در حالت استراحت_، اپلیکیشن Ouisync اطلاعات را با استفاده از
    [ChaCha20](https://en.wikipedia.org/wiki/Salsa20#ChaCha_variant) رمزنگاری
    می‌کند. در این مورد از «کلید خواندنی» به عنوان کلید متقارن رمزنگاری/رمزگشایی
    استفاده می‌شود. کلیدها با استفاده از امضاهای Ed25519 و «کلید نوشتنی» به
    عنوان کلید خصوصی تأیید می‌شوند.
*   برای پروسه درهم سازی _hashing_، اپلیکیشن Ouisync بر تابع هش [BLAKE3]
    (https://en.wikipedia.org/wiki/BLAKE_(hash_function)#BLAKE3) متکی است، که
    همواره [مطرح
    می‌شود](https://github.com/BLAKE3-team/BLAKE3-specs/blob/master/blake3.pdf)
    که در پلتفرم‌ها و اندازه‌های ورودی مختلف سریع‌تر است.

## بلوک چیست؟
هر فایل و هر دایرکتوری ذخیره‌شده در Ouisync به بلوک‌های نسبتاً کوچک (به عنوان
مثال ۳۲ کیلوبایت) با اندازه ثابت تقسیم می‌شود. هر بلوک دارای یک شناسه بلوک
(تولید شده از طریق یک مولد تصادفی اعداد) است که به Ouisync کمک می‌کند تا این
بلوک‌ها را شناسایی کند. همه بلوک‌ها در کنار فایلی به نام مکان‌یاب (locator)
ذخیره می‌شوند. مکان‌یاب نوعی «نقشه» است که نشان می‌دهد هر بلوک با توجه به
بلوک‌های دیگر در کجا قرار دارد. با این حال، برای آشکار نشدن این ساختار برای
عواملی که کلید مخفی را ندارند، مکان‌یاب‌ها مستقیماً ذخیره نمی‌شوند، بلکه کدگذاری
می‌شوند.

_تصور کنید که یک جشن عروسی بزرگ را سازماندهی می‌کنید، جایی که مهمانان زیادی را
دعوت می‌کنید. کسانی که قبلاً این نوع رویدادها را سازماندهی کرده‌اند، می‌دانند که
تعیین صندلی‌های مناسب برای همه مهمانان با توجه به روابط، علایق و غیره چقدر سخت
است. به هر حال، شما باید اطلاعات لازم را به پیشخدمت‌ها نیز منتقل کنید، آن‌ها
باید مراقب باشند و به یاد داشته باشند که کدام مهمان دارای آلرژی یا ترجیح غذایی
خاصی است. و از آن‌جایی که مهمانان شما VIP هستند، نمی‌خواهید نام واقعی آن‌ها را
برای پیشخدمت‌ها فاش کنید، بنابراین نام مستعار تصادفی خلق می‌کنید و آن‌ها را روی
کارت‌های اختصاصی زیبا در محل نشستن مهمانان می‌نویسید. بنابراین، اگر به این
استعاره استناد کنیم، شناسه بلوک یک نام مستعار خواهد بود که روی کارتی در کنار
صندلی مهمان شما نوشته شده است، و «مکان‌یاب» نقشه‌ای از همه میزها با صندلی‌هایی
است که به درستی اختصاص داده شده‌اند._

![image](https://github.com/willow446/willow446.github.io/assets/1790886/06985a87-2dac-49a2-99ae-37725bd8e2ce)


## بلاب (blob) چیست؟
مجموعه خطی از بلوک‌ها را Blob می‌نامند. Blob‌ها می‌تواند فایل‌ها و دایرکتوری‌ها
را نمایش دهد. ‌Blob فایل ساده‌تر است: از یک سرتیتر حاوی اندازه فایل، مجوزها و یک
مهر زمانی تشکیل شده است. ‌Blob دایرکتوری فهرستی از نام فایل های موجود در یک
دایرکتوری و همچنین مکان‌یابی‌هایی را نشان می‌دهد که به تک تک Blobهای فایل اشاره
می‌کنند.

## همگام‌سازی چگونه انجام می‌شود؟
هنگامی که یک مخزن را با همتایان خود به اشتراک می‌گذارید، این یک "کپی" از مخزن
شما ایجاد می‌کند. ساختار مخزن در فایل‌های به‌اصطلاح «شاخص یا ایندکس» ذخیره
می‌شود - وقتی دستگاه‌های همتا در حال اتصال هستند، ابتدا آن شاخص (ایندکس)‌ها را
مبادله می‌کنند. اگر چیزی در یکی از کپی‌ها اصلاح شده باشد، Ouisync بلوک‌های گمشده
را دانلود می‌کند. Ouisync همیشه ابتدا دایرکتوری‌ها و سپس خود فایل‌ها را دانلود
می‌کند. این به Ouisync کمک می‌کند تا داده‌های شما را به‌درستی از بلوک‌ها بازسازی
کند، بدون اینکه آن‌ها را خراب کند. علاوه بر این، این کار بدون نشت اطلاعات به
کاربرانی انجام می‌شود که دسترسی «خواندنی» به مخازن شما ندارند.

لازم نیست نگران تداخل بین کپی‌های (replicas) مختلف باشید: در سمت سرور‌
(backend)، فرآیند همگام‌سازی به گونه‌ای انجام می‌شود که از تداخلات و مغایرت‌ها
جلوگیری شود. چیزی که هنگام باز کردن Ouisync می‌بینید، همان چیزی است که ما آن را
«نمای اجمالی» یا «اسنپ‌شات» می‌نامیم: نمای شما از کل درخت دایرکتوری در یک لحظه
خاص از زمان. هر تغییر در سیستم فایل (در دستگاه شما یا دستگاه‌های همتایان شما)
منجر به یک «اسنپ‌شات» جدید می‌شود.

# Futures AI Scanner — Free Static Deployment

واجهة عربية مجانية على Cloudflare Pages، وفحص مجدول باستخدام GitHub Actions. **إشارات بحثية فقط؛ لا يوجد تداول فعلي.**

## خطوات النشر (من الموبايل أو الكمبيوتر)

1. أنشئ مستودع GitHub **عامًا** لتمكين GitHub Actions القياسية بدون تكلفة دقائق في المستودعات العامة (ضمن شروط GitHub الحالية). ارفع **محتويات** هذا المجلد إلى جذر المستودع، مع الحفاظ على `.github/workflows/scan.yml`.
2. من تبويب **Actions** في GitHub، اسمح بتشغيل workflows عند الحاجة، ثم افتح **Scan Binance Futures** واضغط **Run workflow** للتشغيل الأول.
3. إذا فشل رفع النتائج بسبب الصلاحيات، افتح Settings → Actions → General → Workflow permissions واختر **Read and write permissions** ثم أعد التشغيل. قد تكون الصلاحيات مقيدة حسب إعدادات الحساب.
4. افتح Cloudflare Dashboard → Workers & Pages → Create → Pages → Connect to Git، ثم اختر المستودع.
5. إعدادات بناء Pages: **Framework preset: None**، **Build command: اتركه فارغًا**، **Build output directory: `site`**. ثم Deploy.
6. افتح رابط `*.pages.dev`. عند تحديث `site/data/latest.json` بواسطة GitHub Actions، يقوم Cloudflare بإعادة نشر التغييرات المتصلة بالمستودع تلقائيًا وفق إعدادات التكامل.

## ملاحظات مهمة

- الجدولة كل **15 دقيقة** وليست دقيقة واحدة؛ GitHub Actions قد يؤخر أو يتجاوز بعض مرات التشغيل.
- GitHub Actions ليس سيرفرًا دائمًا. لا يمكن ضمان الفحص اللحظي أو التشغيل 24/7.
- في حال فشل الاتصال بـ Binance، يتوقف التشغيل ولا تُكتب نتائج جديدة؛ الواجهة تحذر عندما تتجاوز البيانات 45 دقيقة.
- تفحص جميع عقود USDT-M Perpetual النشطة **التي يتجاوز حجم تداولها 24 ساعة 10 ملايين USDT**. يمكن تغيير العتبة في `MIN_QUOTE_VOLUME` داخل workflow.
- قد تمنع Binance اتصالات بعض عناوين GitHub المستضافة جغرافيًا؛ هذا يتطلب اختبارًا بعد النشر.
- قد يستغرق فحص كل العقود دقائق، وقد تصادف حدود طلبات Binance؛ راجع سجل Actions لأي خطأ.
- لا تشمل الاستراتيجية حساب الرسوم أو السبريد أو الانزلاق أو التمويل؛ **لا تستخدم النتائج كأساس وحيد لتداول حقيقي**.
- الدرجات هي معايير فنية أولية وليست احتمالات نجاح، والاستراتيجية غير مختبرة تاريخيًا.
- لا توجد API Keys أو مفاتيح حساب Binance مطلوبة. **لا ترفع أي مفاتيح خاصة إلى GitHub**.

## تجربة محلية اختيارية

```bash
pip install -r scanner/requirements.txt
python scanner/run.py
python -m http.server 8000 -d site
```

افتح http://localhost:8000. لا تفتح ملف HTML مباشرة من مدير الملفات.

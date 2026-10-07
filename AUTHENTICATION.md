# المصادقة والأمان

## الحالة الحالية
هذا التطبيق عميل PWA ثابت ولا يحتوي على حسابات مستخدمين أو API خاص بالمصادقة.

- لا توجد كلمات مرور محفوظة.
- لا توجد رموز JWT أو OAuth داخل التطبيق.
- التقدم محفوظ محلياً في `localStorage` تحت المفتاح `arabic-app-progress`.
- النطق يستخدم `SpeechSynthesisUtterance` مع اللغة `ar-SA`.
- الميكروفون يستخدم `getUserMedia({audio:true})` بعد موافقة المتصفح، ويُوقف المسار بعد التمرين ولا يتم إرسال الصوت إلى خادم.

## تدفق الطلبات
1. يفتح المستخدم تطبيق GitHub Pages عبر HTTPS.
2. يتم تحميل ملفات JavaScript/CSS والـ PWA manifest وService Worker.
3. التفاعل مع التقدم يتم محلياً في المتصفح.
4. عند طلب الصوت، يسمح المتصفح بـ `speechSynthesis`.
5. عند تمرين الميكروفون، يطلب المتصفح إذن الجهاز ثم يحصل التطبيق على مسار صوت محلي فقط.

## GitHub Actions
النشر يستخدم أذونات GitHub Actions التالية فقط:
- `contents: read`
- `pages: write`
- `id-token: write`

لا يتم وضع أي GitHub Personal Access Token أو secret داخل ملفات التطبيق.

## تطوير مستقبلي
إذا تمت إضافة حسابات وخادم لاحقاً، يفضّل استخدام جلسات قصيرة العمر، تخزين refresh tokens في `HttpOnly; Secure; SameSite` cookies، وعدم وضع الأسرار في كود Vite العميل.

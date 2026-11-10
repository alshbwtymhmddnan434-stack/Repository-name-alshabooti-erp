# الشبوطي المحاسبي — أساس نظام ERP

مشروع تأسيسي لنظام محاسبي متعدد الفروع، مستوحى وظيفيًا من أنظمة ERP المحاسبية، وليس نسخة من برنامج تجاري قائم.

## متطلبات التشغيل
- Node.js 20 أو أحدث.
- PostgreSQL (يفضل إصدارًا مدعومًا حديثًا).
- إعداد `DATABASE_URL` و`JWT_SECRET` وفق `backend/.env.example`.

## تشغيل الاختبارات
```bash
cd backend
npm test
npm run check
```

## ترتيب ترحيلات قاعدة البيانات
نفّذ ملفات `backend/migrations` حسب الترتيب الرقمي مرة واحدة على قاعدة بيانات تجريبية أولًا.

## حالة المشروع
الإصدارات 0.1–0.9 هي مراحل تأسيس وقواعد محاسبية واختبارات وحدات. لا تعتبر هذه الحزمة نظامًا إنتاجيًا مكتملًا. لم تُختبر جميع الترحيلات على PostgreSQL فعلي، ولم تكتمل بعد واجهات دورات المستندات وواجهات API للترحيل المتكامل والمزامنة.

راجع `docs/acceptance-checklist.md` ومستندات المراحل قبل أي تشغيل مالي.

## المرحلة 1.0
أضيفت واجهة محلية أولية لسندات القبض والصرف وتعريف الصناديق والتحصيل اللاحق. هذه واجهة مسودات محلية فقط؛ لا تنشئ قيودًا أو حركات صندوق مركزية ولا تعتبر ترحيلًا محاسبيًا. التفاصيل في `docs/phase-1.0-cash-voucher-ui.md`.

## المرحلة 1.1
أضيفت مسارات API لإنشاء سندات القبض والصرف والتحصيل اللاحق بمعاملات PostgreSQL ذرية. راجع `docs/phase-1.1-cash-voucher-api.md` لمعرفة المسارات والقيود وحدود الاختبار.

## المرحلة 1.2 — خدمات الصناديق وبيانات السندات
أضيفت مسارات API محمية لعرض صناديق وبنوك الفرع، تعريف صندوق/بنك مركزي، عرض سجل السندات، وجلب بيانات العملاء والموردين والحسابات والمتحصلين. راجع `docs/phase-1.2-cash-read-models.md` للحدود والاختبارات.


## المرحلة 1.3
تم ربط شاشة سندات القبض والصرف بواجهات الخادم المركزي، وإضافة تسجيل الدخول وتحميل الفروع وبيانات الأطراف والحسابات، وتعريف الصناديق مركزيًا. راجع `docs/phase-1.3-frontend-server-connection.md` وحدود الاختبار قبل الاستخدام.


## مرحلة 1.4 – تحسين جاهزية التثبيت على Android
- إضافة بيانات تعريف تطبيق الويب على Android وiOS لتجربة العرض المستقل وشريط الحالة.
- استكمال بيانات manifest بتصنيفات التطبيق ومنع تفضيل تطبيق مرتبط غير موجود.
- رفع إصدار ذاكرة التخزين المؤقت للخدمة إلى v8 لضمان وصول تحديثات الواجهة.
- أيقونات التطبيق موجودة بمقاسات 192 و512 ونسخة maskable.
- التثبيت كـ PWA يتطلب نشر الواجهة عبر HTTPS (أو localhost للتطوير).
- هذه المرحلة لا تنتج APK؛ بناء APK موقّع ما زال يحتاج بيئة Android/Gradle أو خدمة بناء خارجية، كما يحتاج الخادم وقاعدة البيانات إعدادًا منفصلًا.

## المرحلة 1.5 – تجهيز النشر بالحاويات
أضيفت ملفات Docker وDocker Compose لتشغيل PostgreSQL والخادم والواجهة خلف Nginx. راجع `docs/phase-1.5-container-deployment-prep.md`. هذه إعدادات نشر غير مختبرة فعليًا في بيئة Docker هنا، وتتطلب إعداد الأسرار وHTTPS قبل الاستخدام.

## Phase 1.6 — Android wrapper preparation
Capacitor application metadata and build scripts are now included. This does **not** mean an APK has been built: the native Android project, Android SDK/Gradle build, signing, and device testing remain outstanding. See `docs/phase-1.6-android-wrapper.md`.

## المرحلة 1.7 — تشغيل ونسخ احتياطي
أضيف فحص صحة API في Docker Compose وسكربتا نسخ احتياطي واستعادة PostgreSQL. راجع `docs/phase-1.7-operations-and-backups.md`. الاستعادة عملية مدمرة وتتطلب تأكيدًا صريحًا. لم يُختبر Docker أو PostgreSQL فعليًا في بيئة التطوير.

## Android build workflow (Phase 1.8)
A GitHub Actions workflow is provided at `.github/workflows/android-apk.yml`. It generates the Capacitor Android wrapper on the hosted runner and uploads a debug APK artifact. This workspace has not built or device-tested an APK; see `docs/phase-1.8-android-ci-and-release-path.md` for prerequisites and limitations.

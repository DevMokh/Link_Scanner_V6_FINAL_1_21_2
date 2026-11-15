# تقرير إصلاح CI — Link Scanner V6 1.21.2+26

## سبب الفشل الظاهر في الصورة
في `test/online_intel_test.dart` كان الاختبار يستدعي `resetPhishTankRateLimit()` قبل تعريف الدالة المحلية داخل `group`، فظهر خطأان `referenced_before_declaration`.

## الإصلاحات
- استبدال الاستدعاءين باستدعاء الدالة العامة المخصصة للاختبارات مباشرة: `OnlineIntel.resetPhishTankRateLimitForTesting()`.
- إزالة مساعد إعادة الضبط المحلي الذي لم تعد هناك حاجة إليه.
- إزالة الاستيرادات غير المستخدمة من `free_threat_intel_engine.dart` و`online_intel.dart` و`free_threat_intel_engine_test.dart`.
- إزالة الحقل والمعامل الاختياري غير المستخدم `error` من `_RipeNetwork` فقط؛ حقل `error` في `NetworkIntelligenceReport` باقٍ لأنه يُستخدم لعرض سبب عدم اكتمال فحص الشبكة.
- إزالة الدالة الخاصة غير المستخدمة `_hasBrandToken` والمساعد `_error()` غير المستخدم في الاختبارات.
- توصيل `redirectFindings` بواجهة التقرير المتقدم وعرض مؤشرات التحويل أسفل مسار التحويلات بدل حسابها دون عرضها.
- تحديث النسخة إلى `1.21.2+26` وتوحيدها بين `pubspec.yaml` و`lib/core/app_info.dart`.

## التحقق المنفذ هنا
- `python -m compileall -q sandbox_service android_overlay tool` — نجح.
- `python tool/test_update_unicode_confusables.py` — 9 اختبارات ناجحة.
- `python -m pytest -q sandbox_service/tests` — 27 اختبارًا ناجحًا.
- `bash -n` لسكريبتات البناء والنشر — نجح.
- فحوص اتساق الإصدار، وتأكد أن استدعاءات إعادة ضبط PhishTank لم تعد تشير إلى مساعد محلي غير معرّف — ستُجرى قبل التغليف.

## ما لم يتم التحقق منه
بيئة التحرير لا تحتوي على Flutter أو Dart SDK. لذلك لم يتم تشغيل `dart format` أو `flutter analyze` أو `flutter test`، ولم يتم بناء APK. يجب فتح GitHub Actions وتشغيل الـ workflow على هذه النسخة للتأكد من نجاح بوابة Dart ثم بناء APK. لا تصف هذه الحزمة بأنها APK ناجح قبل ظهور نجاح هاتين المرحلتين.

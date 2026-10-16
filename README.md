# Dhi-Short — أرشيف النسخ

مستودع أرشفة لمشروع **Dhi-Short** (تطبيق Next.js لمشاهدة المسلسلات القصيرة). تُنشر الأرشفة كأصول على صفحات الإصدارات؛ شجرة git لا تحتوي أي ثنائي، لأن كلاً من الملفين يتجاوز حد GitHub للملف الواحد داخل git وهو 100 MiB.

Archive repository for the **Dhi-Short** project. Artifacts are attached as GitHub Release assets; the git tree holds no binaries, because each file exceeds GitHub's 100 MiB per-file limit for git.

## الأرشيفات المنشورة / Published archives

| الأرشيف / Archive | البايت / Bytes | الحجم / Size | SHA-256 |
|---|---|---|---|
| `dhi-short.zip` | 505883870 | 482.45 MiB | `d73c3833bcb26f0ad0838b970d62c028dfb7d88c6ba9992eaac2ff6e540adad9` |
| `dhishort-db.zst` | 716279276 | 683.07 MiB | `4f93965a6bfa8be99746392397d361b061b22dc9a16f43f53b9a5860a3fa0d75` |

المجموع 1222163146 بايت (1.1667 GiB).

## ما داخل `dhi-short.zip`

- 207 ملفاً: شيفرة المشروع (`src`, `prisma`, `scripts`, `public`) و`docs/` و`worklog.md` و`RESTORE.md`.
- `db/custom.db` بحجم 1718796288 بايت (1.6 GiB): لقطة بتاريخ 2026-10-13، حجم الصفحة 8 KiB، وعدد الصفحات المعلن في الترويسة 209814.
- من الكيانات المخططية الظاهرة في الترويسة: `Series`، `Episode`، `EpisodeResolution`، `EpisodeSubtitle`، `Tag`، `SeriesTag`، `UserSignal`، `ViewEvent`، `PushSubscription`، وفهرس `series_fts` (FTS5).
- لا تشمل الأرشفة `node_modules` ولا `.next` ولا `.git`؛ كلها قابلة لإعادة التوليد كما يشرح `RESTORE.md`.

## ما داخل `dhishort-db.zst`

- إطار zstd واحد، وتحققه من نوع XXH64 بالقيمة `0d2e4b01`، وحجم النافذة 8 MiB.
- يفكّ الضغط إلى ملف SQLite يعلن في ترويسته 402851 صفحة في 8192 بايت = 3300155392 بايت (3.074 GiB).
- تاريخ الملف 2026-10-14، أي أنه لقطة **أحدث وأكبر** من القاعدة الموجودة داخل `dhi-short.zip`؛ ليستا نسختين متطابقتين.

## حالة الحساسية / Sensitivity (measured)

- لا يوجد `.env` ضمن entries الأرشفة (فُحصت القائمة كاملة)، رغم أن `RESTORE.md` ينص على أنه مشمول مع `DATABASE_URL` ومفاتيح VAPID. هذا تناقض بين الوثيقة والقياس، والحكم هنا على الأرشفة الحالية فقط.
- جدول `PushSubscription` بلا صفوف اشتراك: مسح تدفقي حجمه 1718796288 بايت لم يعثر على أي عنوان endpoint (بحثاً عن `push.services` و`fcm.googleapis.com` و`mozilla.org` و`subscriptions/`) — صفر مطابقة في الأربعة.
- مع ذلك تتضمن الأرشفة طوابع مشاهدات (`UserSignal` و`ViewEvent`) لأجهزة المالك نفسه، وكتالوجاً مجمّعاً من مصدر خارجي، وشيفرة المشروع كاملة. جعل هذا المستودع عاماً كان قرار المالك بعد عرض هذه القياسات.

## فكّ الأرشفة / Unpacking

```sh
sha256sum dhi-short.zip dhishort-db.zst   # طابق القيم أعلاه أولاً
unzip dhi-short.zip                        # أو: 7z x dhi-short.zip
zstd -d dhishort-db.zst -o dhishort-db     # مستقل عن الأرشفة الأخرى
```

يتطلب فكّ `dhishort-db.zst` مساحة 3.074 GiB على القرص.

## استبعاد git / Excluded from git

الأرشيفات ضخمة فلا تُرفع إلى git. وإن لزم نقلها إليه مستقبلاً فبواسطة Git LFS، مع الانتباه إلى أن الخطة المجانية محددة بسقف 1 GiB تخزين و1 GiB نقل شهرياً، بينما المجموع الحالي 1.1667 GiB يتجاوز سقف التخزين.

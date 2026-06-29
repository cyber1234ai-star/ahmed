# Graphic Designer Portfolio + Admin Dashboard

موقع Portfolio احترافي لمصمم جرافيك مبني بـ Next.js وTypeScript وTailwind CSS وFramer Motion، مع Dashboard فعلي على `/admin` لإدارة الصور والمحتوى والرسائل.

## Architecture

- Frontend: Next.js App Router، صفحة عامة واحدة مقسمة إلى Home/About/Portfolio/Services/Testimonials/Contact، مع Framer Motion وواجهة Dark Luxury.
- Backend/API: Route Handlers داخل `src/app/api` لكل عمليات CRUD والرفع والرسائل وتسجيل الدخول.
- Database: Prisma schema مخصص لـ PostgreSQL/Supabase Postgres داخل `prisma/schema.prisma`.
- Storage: `src/lib/storage.ts` يستخدم Supabase Storage عند ضبط المفاتيح، أو يحفظ محليًا في `public/uploads` للتطوير.
- Admin Dashboard: المسار `/admin` محمي بجلسة JWT في httpOnly cookie، وكل API إداري يتحقق من الجلسة.

## التشغيل المحلي السريع

```bash
npm install
npm run seed:images
npm run dev
```

افتح:

- الموقع: `http://localhost:3000`
- الداشبورد: `http://localhost:3000/admin`

بيانات الدخول الافتراضية للـ seed:

- Email: `admin@luxe.design`
- Password: `ChangeMe123!`

بدون `DATABASE_URL` سيعمل المشروع على fallback محلي داخل `data/local-db.json` للتجربة فقط.

## تشغيل PostgreSQL / Supabase

1. انسخ `.env.example` إلى `.env`.
2. ضع `DATABASE_URL` الخاص بـ Supabase Postgres أو PostgreSQL.
3. ضع `SESSION_SECRET` عشوائيًا وطويلًا.
4. شغل:

```bash
npm run db:generate
npm run db:push
npm run db:seed
npm run dev
```

إذا شغلت `npm run start` محليًا على HTTP بعد build، ضع `AUTH_COOKIE_SECURE=false` في `.env` حتى يقبل المتصفح cookie الجلسة. في الإنتاج الحقيقي على HTTPS اتركها غير مضبوطة أو اجعلها `true`.

## تفعيل Supabase Storage

أضف في `.env`:

```bash
SUPABASE_URL="https://xxxxx.supabase.co"
SUPABASE_SERVICE_ROLE_KEY="service-role-key"
SUPABASE_STORAGE_BUCKET="portfolio"
```

اجعل bucket عامًا إذا أردت عرض الصور عبر public URL.

## أين أغير بيانات الاتصال والروابط؟

من الداشبورد:

`/admin` ثم `Site Settings`

يمكنك تعديل: اسم الموقع، اللوجو، favicon، روابط Instagram/Behance/Dribbble، WhatsApp، البريد، لون الـ accent، عنوان الـ hero، نص footer.

يمكنك تعديل بيانات المصمم من:

`/admin` ثم `About Content`

## كيف أضيف أول صورة؟

1. ادخل إلى `/admin`.
2. افتح `Portfolio Images`.
3. املأ: Title، Description، Category، Tags، Tools.
4. اختر صورة أو عدة صور من حقل `Images`.
5. فعّل `Visible` إذا أردت ظهورها مباشرة في الموقع.
6. فعّل `Featured` إذا أردت ظهورها في Featured Works.
7. اضغط `Save Image`.

الصورة ستظهر فورًا في صفحة Portfolio العامة. إخفاء الصورة من الداشبورد يجعلها تختفي من الموقع.

## الملفات المهمة

- `prisma/schema.prisma`: مخطط قاعدة البيانات.
- `src/lib/data.ts`: طبقة البيانات الموحدة لـ PostgreSQL/fallback المحلي.
- `src/lib/storage.ts`: رفع الصور والتحقق من النوع والضغط إلى WebP.
- `src/components/site/designer-portfolio.tsx`: واجهة الموقع العامة.
- `src/components/admin/admin-dashboard.tsx`: لوحة الإدارة.

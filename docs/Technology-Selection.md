### 1. المخطط العام (The Architecture)
لقد صممت نظامًا مفصولًا وموزعًا (Distributed System) يعتمد على التخصص:
*   **الواجهات (Frontend):** مفصولة تمامًا (React). واحدة للمستخدمين (للعرض) وواحدة للمدير (للتحكم).
*   **الخدمات الخلفية (Backend):** مفصولة.
    *   `Publishing Service`: للكتابة والتحكم.
    *   `Content Delivery API`: للقراءة السريعة.
    *   `Authentication Service`: حارس البوابة (Login & Security).
*   **تدفق البيانات:**
    *   القراءة: تمر بالكاش (`Redis`) أولًا للسرعة، ثم قاعدة البيانات.
    *   الكتابة: تمر بالتحقق (`Auth`) أولًا، ثم تحفظ في قاعدة البيانات.

---

### 2. عقود التواصل (API Contracts)
اتفقنا على "لغة الحوار" بين الواجهات والسيرفر، وكلها تعتمد على **JSON**:

**أ. للمستخدمين (القراءة فقط - GET):**
*   `GET /claims?limit=20&page=1` $\leftarrow$ لجلب القائمة (مع التصفح).
*   `GET /claims/{id}` $\leftarrow$ لجلب تفاصيل إدعاء واحد (العنوان، الصورة، المحتوى المفصل).

**ب. للمدير (التحكم - POST):**
*   `POST /login` $\leftarrow$ يرسل (Email/Password) $\rightarrow$ يستلم `JWT Token`.
*   `POST /login/google` $\leftarrow$ يرسل (Google Token) $\rightarrow$ يستلم `JWT Token`.
*   `POST /upload` $\leftarrow$ يرسل (ملف صورة) $\rightarrow$ يستلم (رابط الصورة URL).
*   `POST /claims` $\leftarrow$ يرسل (تفاصيل المقال + رابط الصورة + JWT) لإنشاء مقال جديد.

---

### 3. أدوات البناء (The Tech Stack)
قررت استخدام أدوات قوية، حديثة، ومجانية (Open Source):

*   **لغة البرمجة (Backend):** Python (باستخدام FastAPI).
*   **الواجهات (Frontend):** React.
*   **قاعدة البيانات:** PostgreSQL (علائقية وقوية).
*   **الكاش:** Redis (سريع جدًا).
*   **خادم الويب:** Nginx + Ubuntu (نظام التشغيل).

---

### 4. البنية التحتية (Infrastructure)
*   **النوع:** سيرفر افتراضي خاص (**VPS**) وليس استضافة مشتركة.
*   **المكان:** إما مدفوع رخيص (Hostinger) أو مجاني للتجربة (Oracle Cloud / AWS Free Tier).
*   **الفلسفة:** المزود يعطيك "الأرض" (VPS)، وأنت تبني "المبنى" (تثبيت Python و Database) بنفسك للتحكم الكامل.

# Captain-2: online survey response (2026-10-30)

**Who:** marine captain, operations role in the marine-services side (pilotage, towage, supply) of an Egyptian oil-terminal company, more than 10 years' experience. Name and contact are kept off GitHub (see [interview-guide.md](interview-guide.md)).
**Source:** the team's Arabic Google Form, submitted 2026-10-30 21:08 (Cairo). The first row in the sheet (2026-10-27) is a team test and is ignored.
**Follow-up:** agreed to **both** a 15-min interview and testing the first version.

## Summary

| Topic | Answer |
|---|---|
| Ships handled per week | 1–5 |
| How often ships wait at anchorage | Sometimes |
| Average wait | Doesn't know |
| Longest wait in the last 12 months | About a week, because of bad weather |
| Top causes (max 3) | No free berth; weather and sea state (wind, waves); several ships arriving at once |
| Impact of waiting | **High:** customer satisfaction and reputation. **Medium:** demurrage/detention, late delivery to the customer. **Low:** trucks. **None:** fuel and operating cost, labour and equipment planning |
| Cost of one waiting day | Doesn't know / prefers not to say |
| Where arrival and berth times come from | The company's internal system |
| How often the real berth time differs from the expected one | Sometimes |
| Satisfaction with current information (1–5) | 4 |
| What he does when he expects congestion | Gets the port ready with the right marine units (tugs, pilot boats) |
| Usefulness of a system like Rasid (1–5) | **5** |
| Most useful warning time | **2–3 days ahead** |
| Accepted error on a 10 h forecast | "Any reasonable forecast is better than nothing" |
| Alerts by | Email |
| Would his organisation pay? | Not his decision |

**Feature importance:**
- **Very important:** live map of ships and the anchorage; explanation of why (weather, ships bunching up).
- **Important:** wait forecast for a specific ship; automatic congestion alert; expected cost of waiting; historical reports.
- **A little important:** number of ships waiting; comparing ports (Alexandria / Dekheila / Port Said).

## What this means for Rasid

1. **A third user type.** Marine-services crews (tugs, pilots, supply boats) plan around arrivals too. Captain-1 said the port already knows its queue. Captain-2 is inside the port world but at a *service company*, and he still rates the idea 5/5. Add this to O-008: the forecast has users on the operator side, just not the port authority itself.
2. **The horizon is days, not 6–12 h.** He chose 2–3 days ahead. That matches the forwarder use case (option b in O-008) and argues for a longer horizon than the original proposal.
3. **Explain the forecast.** "Why" (weather, bunching) is rated very important. That supports SHAP explanations (M3) and weather features (Open-Meteo).
4. **Weather matters.** His worst case was a week-long wait caused by weather, so weather features belong in the first model, not later.
5. **Trust bar is low.** "Any reasonable forecast is better than nothing" means a calibrated range (P10–P90) is more useful than a single, tightly promised number.
6. **One response is anecdote, not evidence.** Don't put numbers from it in the thesis as statistics. Use it as a quote and as a lead for the interview.

## Raw answers (Arabic, as submitted)

| السؤال | الإجابة |
|---|---|
| نوع الجهة | خدمات بحرية (إرشاد، قطر، تموين، إصلاح) |
| الدور الوظيفي | عمليات / تشغيل |
| سنوات الخبرة | أكثر من 10 سنوات |
| الموانئ | (اسم الشركة، محذوف) |
| عدد السفن أسبوعيًا | 1–5 |
| تكرار الانتظار عند المخطاف | أحيانًا |
| متوسط مدة الانتظار | لا أعرف |
| أطول مدة انتظار وسببها | اطول مده ممكن تكون أسبوع بسبب سوء الأحوال الجويه |
| أكثر الأسباب | عدم توفر رصيف فاضي، الطقس وحالة البحر (رياح / أمواج)، وصول عدة سفن في نفس الوقت |
| التأثير: غرامات التأخير | متوسط |
| التأثير: الوقود والتشغيل | لا تأثير |
| التأثير: الشاحنات والنقل البري | بسيط |
| التأثير: تأخر التسليم للعميل | متوسط |
| التأثير: تخطيط العمالة والمعدات | لا تأثير |
| التأثير: رضا العملاء والسمعة | كبير |
| تكلفة يوم الانتظار | لا أعرف / أفضل عدم الإجابة |
| مصادر مواعيد الوصول والرسو | نظام داخلي في الشركة |
| اختلاف موعد الرسو الفعلي | أحيانًا |
| الرضا عن المعلومات الحالية (1–5) | 4 |
| ماذا تفعل عند توقع تكدس | تجهيز الميناء بالوحدات البحريه المناسبه |
| فائدة النظام (1–5) | 5 |
| مدى التوقع المسبق | قبل 2–3 أيام |
| خريطة حية | مهم جدًا |
| توقع عدد السفن المنتظرة | مهم قليلًا |
| توقع مدة انتظار سفينة محددة | مهم |
| تنبيه تلقائي | مهم |
| شرح سبب التوقع | مهم جدًا |
| تقدير تكلفة الانتظار | مهم |
| تقارير تاريخية | مهم |
| مقارنة بين الموانئ | مهم قليلًا |
| طريقة التنبيه | البريد الإلكتروني |
| هامش الخطأ المقبول | أي توقع معقول أفضل من لا شيء |
| الميزة للاستخدام اليومي | (فارغ) |
| الاشتراك بمقابل | القرار ليس بيدي |
| ملاحظات | (فارغ) |
| التواصل | نعم، للاثنين |

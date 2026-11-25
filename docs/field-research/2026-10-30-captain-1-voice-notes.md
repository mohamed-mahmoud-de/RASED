# Interview: Captain-1 (two WhatsApp voice notes, 2026-10-30)

> **Who:** a marine captain who now manages a ship and buys spare parts from abroad (ships 30–40+ consignments to the ship he manages). Name kept off the public repo per [interview-guide.md](interview-guide.md) §9.
> **Format:** two voice notes addressed to Alhassan, sent after a conversation about Rasid. Note 1 (02:10, 8 min 54 s): "who is this for?". Note 2 (15:32, 8 min 27 s): his product suggestion.
> **Transcription:** machine transcript (faster-whisper large-v3-turbo, Arabic), lightly cleaned. Expect misheard words; the audio is the source of truth. Audio files are kept locally, not in git.
> **Links he sent:** [MarineTraffic](https://www.marinetraffic.com/), [ShipInfo](https://shipinfo.net/?lang=en). Our check of both is under "What we checked" below.

## Summary (English)

### Note 1: "Who will use it? The port already knows."

- **Define the user first, even for a graduation project.** Port authority, ship owner/operator, or shipping agent: each needs different inputs. Then decide **what data goes in, who enters it, and how** (manual, automatic, radar, internet), and what comes out.
- **The shipping agent is the link** between the ship's operator and the port authority. The operator tells the agent where the ship goes; the agent notifies the port.
- **The port knows about arrivals 1–2 weeks ahead.** The agent sends an arrival window (e.g. "between the 5th and 7th"), the ship's particulars (length, beam, tonnage, last cargo) and the purpose of the call (e.g. discharge 100 containers, load 200).
- **24 h before arrival** the ship confirms its ETA by VHF radio. On approach, the port assigns it an exact anchorage position (lat/lon). *"Nobody anchors at random."* (Whisper heard "14 h" twice; he says 24 h elsewhere, so it is almost certainly 24 h.)
- **Anchorage areas are drawn on nautical charts:** inner and outer anchorages, sometimes circles per ship class (large ships, small ships, bulk, passenger).
- **Every port has 24/7 radar and VHF watch.** Ships need permission to move, enter or leave. The port knows each berth's current ship, when it will finish, and which waiting ship goes in next (the in/out sequence).
- **His conclusion:** *"So if you tell the port 'it's going to get crowded', it already knows."* This is the same worldwide, Egypt included.
- Free sites like ShipInfo and MarineTraffic show ships at sea; the detailed data needs paid subscriptions.

### Note 2: "Build it for freight forwarders: track their shipments."

- **His own case:** he buys spare parts worldwide. He currently follows **~86 shipments, about 40–50 of them by sea**. Each goes through a **freight forwarder**, who picks it up from the supplier and ships it to Tunisia, Libya or Egypt.
- **Every sea shipment has a bill of lading (B/L, بوليصة الشحن)** giving cargo size and weight, shipper, consignee, **vessel name, port of loading, port of discharge, ETD and ETA**. Air freight has the same in the air waybill (AWB).
- **His proposal:**
  - **Input:** the forwarder enters each B/L (cargo, vessel name, POL/POD, ETD/ETA).
  - **Tracking:** the software follows each vessel using free AIS tracking sites (MarineTraffic, ShipInfo).
  - **Output:** notifications at each step, for example "your shipment left Shanghai", "speed X", "arrived Oman", "left Oman, entered the Red Sea", "looks like Port Said on date X", "Alexandria the next day".
  - Automatic emails go to the forwarder's staff **and to the forwarder's client** (the cargo owner).
- **Why it's worth paying for:** one client like him has 30–40 shipments at sea for 30–40 days each. A forwarder may follow **200–300**. Today someone checks them by hand.
- *"Serving the port authority is hard, but software for companies that ship a lot has value and can be sold."* He asked us to show this idea to the team and the supervisor.

## What this means for Rasid

| His point | Consequence for the proposal |
|---|---|
| The port already knows the queue 1–2 weeks ahead | The port authority is **not** our customer. It is a **data source**: the APA ship board is a public window onto the same pre-arrival list he describes. Add this as a defense question |
| Everyone outside the port doesn't have that view | The gap is **information asymmetry**. The port knows, but cargo owners, forwarders, customs brokers and truckers don't. That is the real user group |
| Shipment tracking for forwarders | A clear user, input (B/L) and output (alerts). This becomes the product shell. Our congestion and wait forecast becomes the core: *"the B/L says ETA 5 Nov; the ship will really berth around 7 Nov (P50), because the Alexandria anchorage queue is long"* |
| Anchorages are drawn on charts, per ship class | Use the official chart anchorage polygons for zones (plus GFW anchorages), not only DBSCAN. Ask him which charts cover Alexandria and Port Said |
| His horizon is days (30–40-day voyages) | Our 6–12 h horizon fits port operators, not importers. O-002 must be revisited: importers care about **days** |

## What we checked (2026-10-30)

- **ShipInfo.net** (Russian company, STELS A LLC):
  - The free map tracks ships worldwide. EVER GIVEN showed in the South China Sea with a last fix **~2 days old**.
  - Coverage near Port Said is thin: ~10 ships on the map versus 100–130/h in GFW.
  - Its "Port Congestion" API returned **no rows** for Port Said or Alexandria.
  - The free API needs an agent name or registration: 200 requests/day, attribution "Data by ShipInfo.net" required.
  - So it is usable for coarse ocean-leg tracking (days), not for the Egyptian anchorage queue.
- **MarineTraffic:** live coverage of the Port Said anchorage is clearly good (dense cluster visible), but data and API access are paid. Scraping the site breaks its terms. Same conclusion as [data-sources.md](../data-sources.md) #8.

## Follow-ups

- [ ] Ask him for a **redacted sample B/L** and a screenshot of how he tracks his 86 shipments today (email? Excel? the forwarder's portal?).
- [ ] Ask for **2–3 freight-forwarder contacts** in Alexandria for interviews ([interview-guide.md](interview-guide.md) §6.4).
- [ ] Ask which nautical charts or port notices define the Alexandria and Port Said anchorage areas.
- [ ] Team + supervisor: decide O-008 in [decision-log.md](../decision-log.md).

## Transcript: note 1 (AUDIO-2026-10-30-02-10-06, 8:54)

```text
[00:00] مسائل فالحسن أخبارك عامل يوم، بص هو مبدئياً، أنت لازم تعرف هتعمل البرنامج ده لمين، حتى لو مش هتديه لحد، يعني أنت مثلاً هتعملو كمشروع تخرج، أنت هتديه لمن؟ هتديه لهيئة المين عشان تشتغل به، فيبقى المدخلات بتاعته مختلفة، هتديه للأونر أوبريتور بتاع المراكب
[00:29] اللي هو بيشغل المركب بيقول لها روح للمينة دي وروح للمينة دي، فبرضو دي مختلفة، ولا للوكيل الملاحي، العلاقة دايماً في المينة، هاي المينة والمركب اللي بيدرها، ولازم بين المينة والمركب اللي بيدرها فيه واحد اسمه وكيل ملاحي
[00:54] جاها بتدخل المركب المينة دي، بناء على طلب اللي بيدرها، اللي قال لها هروح لحتة الفلانية، طيب، لو أنت هتعمل الكلام ده لهيئة المينة،
[01:06] هي المينة بيبقى عندها خبر ان المركب جاية، عندها خبر ان المركب الفلانية دي جاية اعتباراً من، بيبقى تاريخ من إيه؟
[01:18] الوكيل يقول له المركب الفلانية دي جاية من يوم خمسة في الشهر لسبعة في الشهر، يعني في خلال التلات يام دون، طيب المركب دي مواصفاتها ايه؟ طولها وعرضها ووزناها وشاي اللي حملت ايه؟
[01:36] كل المواصفات بتاعتها، وجاية تعمل ايه أصلاً؟ جاية تفرق حاجة، تشيل حاجة، تحط حاجة، تشيل كونتينر، مع ألف كونتينر هتفضي مية وتاخد متين بتاع أي وقت،
[01:51] يعني غرض الوصول للمركب بيبقى متبلغ من الأونر اللي هو صاحب المركب أو الجهة المشاغلة للمركب للوكيل
[02:03] اللي موجود في المنطقة أو في النطاق بتاع هيئة المينة دي، فهو ده همزة الوصل، الوكيل الملاحي ده همزة الوصل
[02:13] بين المركب وبين الهيئة المينة، فالمينة قردي عندها خبر، فانت لو حتقول لها الدنيا حتتزحم ازاي؟ طب ما هو أصلاً عارف، لأنه عارف المساحة اللي بتقف فيها، المساحة اللي بتقف فيها المراكب دي اسمها أنكرج آريا،
[02:32] أو بالعربي منطقة مخطاف المينة، منطقة مخطاف مينة الفلانية بقى برسعيد، دوميات، اسكندرية، العريش، وتفر، أبرس، تونس، أي حاجة، فدي اسمها منطقة المخطاف، ساعات في بعض المواني بيبقى فيها منطقة مخطاف داخلي، وفي منطقة مخطاف خارجي، طب منطقة المخطاف دي،
[02:54] بتبقى مرسومة بخطوط هيكلية، يعني هيكلية مش موجودة طبعاً، ما فيش خطوط ولا رسم في البحر، بس بيبقى موجودة على خريطة منطقة خريطة الملحية،
[03:10] الحدود بتاعت منطقة المخطاف، ومثلاً بعض المناطق أو بعض المواني، بتبقى حتى دواير على خريطة،
[03:22] الدايرة دي يقف فيها المراكب الكبيرة، الدايرة دي يقف فيها المراكب الصغيرة، الدايرة دي يقف فيها المراكب شحنة البالك، دي يقف فيها المراكب الباسنجر اللي هي الركاب،
[03:39] فكل هيئة ميناء بتبقى محددة وممكن جدا بتبقى حطاها منطقة مربع او مسلس او مستطيل او واتفر
[03:48] وهي المركب لما بتقرب من الميناء بتنادي عليها تقول لها انا وصلت في المعادل فلانية
[03:58] او انا وصولي لازم بتبلغها قبلها با 14 ساعة
[04:01] يعني الوكيل الملاحي بيقول للميناء المركب جاي
[04:04] ممكن يقول لها قبلها بسبوع وعشر تيام لما تيجي تقرب المركب قبل وصلها با 24 ساعة
[04:12] لازم تكلم هيئة الميناء عن طريق الاجهزة ال VHF وكلام من
[04:16] بتقول لها انا خلاصا على وصول حوصل في الحتة الفلانية الساعة كذا
[04:21] فيقول لما توصل كلمني لما يجي يوصل قبل ما يخش منطقة الانتظار
[04:26] بيقول له خد الموقع ده
[04:28] خط الطول وخط العرض
[04:33] يقول له روح اقفل حتة الفلانية
[04:35] مفيش حد بيقف اعتباطي
[04:37] خط بالك
[04:38] فعملية الكراودنج او الزحمة بتاعت المنطقة
[04:42] بتبقى معلومة لهيئة الميناء قبلها باسبوع واثنين
[04:48] خط بالك
[04:50] ويعني بيبقى والمركب اللي واقفت هو عارف مين فيهم هتخش الميناء امتى وحتخرج امتى
[05:00] لان هي مثلا هتخش على الرصيف الفلاني ما الرصيف ده عليه مركب حاليا هو عارف اللي على المركب اللي جوه الميناء في المركب دلوقتي حاليا
[05:09] حتى حتخلص شحن او تفريغ قديه فلما حتخلص بكرة العصر مثلا
[05:15] يبقى بكرة العصر دي حتخرج واحدة من الوقفين برا هو حيديها أوردر تخش
[05:22] في حتة السيكوانس بتاعت الانلت اوت دي بتاعت هيئة الميناء قريدي ويل بربيرد يعني هي قريدي موجودة
[05:32] خط بالك
[05:32] عشان كده بقولك انت عايز تعمل الكلام ده الميناء هتعملهم هيئة الميناء تدير بيها
[05:37] طب يعني ماشي ممكن ده يكون
[05:42] بس هو موجود
[05:46] طب المركب الميناء عارفة المراكب دي جاي لها اه عارفة من قبلها بسبوع عشر تيوي ممكن اكتر
[05:54] وبتعرف قبلها با 14 ساعة
[05:57] طب شايفاهم اه شايفاهم مفيش هيئة الميناء كلها اي هيئة ميناء في الدنيا
[06:02] عندها رادار
[06:04] بتبقى شايفة المراكب اللي وقفة فيه وعندها حد قاعدة 24 ساعة شايفة على الرادار
[06:10] وفي حد 24 ساعة قاعدة على جهاز الفي اتش اف اللي هو كومينيكيشن
[06:15] بحيث لو اي مركب عايزها تتحرك بتاخد الاذن بالحركة
[06:20] خلتبلك بتاخد الاذن بالحركة
[06:23] فهو شايفها بالرادار
[06:25] بيديها الاذن بالحركة او الدخول او خروج
[06:30] وعارف انها هتخش امتى استميش تقريبي موعد تقريبي
[06:36] وحتغدر امتى
[06:38] وعارف اصلا انها جاي من قبلها بسبوع عشر تيام
[06:42] وبياخد تأكيد اللي معاد وصلها قبلها باربع عشرين ساعة
[06:45] عن طريق الكمينيكيشن بالجهاز الفي اتش اف
[06:47] اللي انا بقوله لك ده
[06:50] في جميع مواني الدنيا في العالم
[06:53] خلتبلك
[06:55] مفيش استثناء اللي انا بحكوه لك ده
[06:59] ده سيستم وورلد وايد في الدنيا كلها
[07:02] في مصر في السودان في اوروبا في الدنيا
[07:05] فهي حتة اللي حكتولك دوت هو ده البروجرام بتاع اي هيئة مينة في الدنيا
[07:11] فهي دي الحتة طيب
[07:13] فيه اماكن او فيه حاجات بتوريك
[07:16] بتوريك
[07:16] بتوريك المراكب اللي ماشية في البحر
[07:21] اه بتوقفيه
[07:22] فيه كذا حاجة بقى
[07:23] شيب انفو
[07:24] مارين ترافيك
[07:26] ممكن ابقى بعثلك شوية حاجات
[07:28] فيه كلها فري
[07:30] بس ما عايز تاخد ده
[07:32] بتبقى بفلوس باشتراكات معينة
[07:40] خدت بالك
[07:41] فدي برضو ممكن ابعث
[07:45] فاشان كده اقول لك عايزك في الاول تقولي
[07:47] انت عايز تعمل السوفت وير ده
[07:50] ده هيبقى سوفت وير
[07:54] طب
[07:55] السوفت وير ده
[07:57] هياخد الداتا بتاعته منين
[07:59] الانبوت
[08:01] والاوتبوت
[08:02] عايز احنا قلنا الاول
[08:03] هتقولي
[08:04] انت مين اللي هيستخدم البرنامج ده
[08:07] انت بتعمله اذا سامبل كمشروع تخرج
[08:10] مين اللي هيستخدمه
[08:11] عشان الوضع هيختلف
[08:14] وايه الداتا اللي انت عايز تدهاله
[08:16] فه هقولك ايه الداتا اللي حتدخلها
[08:18] ولازم نحدد مين اللي حيدخله الداتا دي
[08:22] هل حتدخلها منوال
[08:23] ولا اوتوماتيك
[08:25] ولا رادار
[08:27] لو على الرادار يبقى في حاجة اسمها
[08:30] نيمة ستاتمنت
[08:31] يعني
[08:31] لازم تحدد المستخدم
[08:35] والمستخدم اللي عايز ييمي البرنامج
[08:38] عشان نحدد هندخله ايه
[08:41] وبعد ما نحدد هندخله ايه
[08:44] هنحدد هندخله هلو ازاي
[08:46] مانوال ولا اوتوماتيك
[08:49] ولا من نت
[08:51] ولا ازاي بالزبط
```

## Transcript: note 2 (AUDIO-2026-10-30-15-32-26, 8:27)

```text
[00:01] جمعة طيبة عليك الحسن أخبارك عاملية ما عليش بقى أنا كنت رياحت مبارح كده شفاك. بص السؤال الأخير اللي أنت بتقول عليه إن الشركة بتبقى المفروض إن فيه شحنات جاية على أكتر من مركب أو أكتر من موقع. بص هو أنا يعني أنا بشتغل في الشحن يعني أنا بنجيب قطع غيار فبنشحنها من الدنيا كلها. يعني أنا عندي مثلا فوق 30-40 شحنة بتباحهم في المركب اللي أنا مساكن.
[00:28] أي شحنة بشحنها ما بتفعها بتروح له واحد اسمه فرايد فوروردر أنا شاريها من شركة سين ببعث له شركة اسمها فرايد فوروردر بتاخد منه الشحنة في المكان بتاعته في أي بلد هولندا، انجلترا، واتفر بقى وحيبعتها لي تونس ولا ليبيا ولا مصر.
[00:49] لما بيحط الشحنة في الطيران مثلا بيعمل في حاجة اسمها Airway Bill AWB
[01:01] دي بتبين مصفات الشحنة دي حجمها ايه ووزناها ايه وجاية من مين ومين اللي حيستلمها وفي نفس الوقت رقم الطيارة برقم الرحلة هتغادر امتى من مطار ايه وتوصل امتى لمطار ايه؟
[01:19] نفس الكلام دوت في المراكب لو انا شاحن شحنة هتجيلي شحنة تقيلة بنشحنها بحر فعلا شحنة هتجيلي من الصين الرجل اللي استلم الشحنة من الرجل المورد بيبعث لي برضو حاجة اسمها بوليسة شحنة
[01:38] بوليسة الشحنة دي بتبقى فيها وموصفات الشحنة حجمها وطولها وعرضها ووزناها وكلام دوت ومين اللي مستلمنها من مين وحيسلمها المين في البلد الوصول
[01:50] وبيبقى مكتوب فيها اسم المركب وحاجة اسمها ايتي دي بارشير ايتي اررايفل يعني الوقت المتوقع للمغادرة والوقت المتوقع للوصول لمينة بورسعيد مثلا
[02:05] فالريدي الداتا دي موجودة اللي انا شايف ان انت ممكن تعمله الشركات الشحنة دي او الشركات اللي هي عندها بضايع هتستلمها من المراكب كتير وكلام من ده
[02:19] يعني بص والسيستم بتاعك دوت حشان يخدم هيئة المينة صعب لكن هو ممكن يبقى صوفت وير تدي للناس الشركات اللي هي بتشحن حاجات كتير
[02:31] فاللي بيشحن حاجات كتير انا من رأي يعني اعرض الكلام دي على زمالك او على الدكتور انت تشغل معا
[02:38] النسية انا شركة الشحنة فباخد شحنة من الحسن ومن محمد ومن احمد وكلام من ده وبيجبها لهم من دول مختلفة
[02:48] فبيطلع بوليسة شحنة بيطلع بوليسة شحنة يعملها ريفيرس انجينيرين شوية ازاي
[02:57] حيطلع بوليسة الشحنة فيها مواصفات الشحنة وهتطلع من المركب من ميناء ايه وبتاريخ كام وتوصل لميناء ايه وتاريخ كام
[03:11] انا من رأي ان انت ممكن تعمل دي الداتا انتري سي انا شركة اللي هي الفرايد في الوردر دي اللي هي بتنقل الشحنات للناس
[03:23] ان هي تدخل معاها مثلا ميت بوليسة فهو عايز يتابع الكلام ده كله طب الميت بوليسة فيهم منهم 30 جايين بالطيران
[03:33] فكك من الطيران فيه 70 بقى جايين بالمراكب يبقى الداتا انتري بتاعت السوفت وير هي الشحنات بحجمها بمواصفاتها بوليسة الشحن بتاعتها
[03:45] هتطلع من المركب اللي اسمها مثلا سي اوشان والأي بقى اسم مركب مثلا من ميناء شنغهاي في الصين متأقع ومغادرتها يوم كذا
[04:02] هتوصل ميناء مثلا بورسعيد ولا اسكندرية بتاريخ كذا
[04:05] لو انت عطت الداتا انتري دي معناها ان انت بتقول للبرنامج
[04:12] يتابع عن طريق المارين ترافيك والبرامج اللي هي الفري بتاع التتبع السفن عن طريق الاي اي اي اس
[04:21] جهاز اسمه اي اس الوانتوماتيك ادريفكيشن سيستم
[04:25] يكون السوفتوير بتاعك يبقى الداتا انتري بتاعته الشحنات بقى اسم المراكب
[04:32] وهو السوفتوير يعد يجمع ياخد يبقى لينكد ياخد الداتا من الانترنت من المارين ترافيك والشيب انفو
[04:42] اللي هي البرامج اللي بتطلع موصفات المراكب وحركتها في البحر
[04:46] فري ويدور على اسم الماركب اللي انت هتطهاله للشحنة رقم واحد واثنين وثلاثة لغاية مية
[04:55] لما تغادر يدي لما يلاقيها في البحر ابتدت تتحرك يدي اردق يدي نوتيفيكيشن لصاحب الشركة
[05:06] الشحنة بتاعتك الفلانية بتاعت شركة خالد اللي على المركب الحسن طلعت من شنغهاي
[05:14] وابتدت تبحر ومشي بسرعة كذا
[05:17] وحتوصل مينة عمان الاول وبعد كده بعد ما وصلت يقول له وصلت
[05:23] بعد كده غادرت مينة عمان طلعت من عمان دخلت البحر الاحمر
[05:28] هيبقى كده السوفت ويرلي فايدة لا ممكن يتسوق لشركات اللي هي بتقوم بعملات الشحن
[05:39] انه هو مش هيقعد من والكل شوية يدور على الشحنة دي وصلت فين
[05:44] هي طلعت في البحر بتاخد تلاتين يوم اربعين يوم عشان توصل
[05:48] يبقى هو عنده سبعين تمانين شحنة كل ما الشحنة منهم تتحرك
[05:53] هو يقول له يبقى انت تعمل السوفت وير على الداتا انتري
[05:58] انه هو بيخدم شركات الفرايد فوروردر اللي هي بتعمل بتقوم بعملات الشحن
[06:05] لأصحاب الشحنة ومن الموردين وبتنقلها ما بين المواني
[06:10] الشركة دي تدخل اول ما تاخد البوليسة وحتقول ان التلفزيون
[06:18] الفلاني بتقول بعرض كذا الشحنة دي مثلا سيب بنشح التلفزيون من
[06:21] الصين هيتحرك على المركب الفلانية دخلنا له الداتا دي يبتيجي
[06:26] البرنامج يتابع المركب من البرامج الفريه كل ما المركب تتحرك او
[06:34] تروح في حتة يقولك وكل ما تيجي تقرب انها خلاص يقولك شكلها كده
[06:38] البحر الاحمر يبقى شكلها هتبقى في بورسعيد في التاريخ الفلاني
[06:42] وازلت بورسعيد اه يبقى هتوصل اسكندريا بعدها بيوم في التاريخ
[06:45] الفلاني بقى انت كده عملت notification system follow up shipment across
[06:53] collecting data من البرامج الفري لتتبع الصفر انا شايف ان كده
[06:59] البرنامج يبقى افضل شوية يبقى ايه لا ده ممكن لو عملته باخراك
[07:04] حلو انت ممكن بعدها تسوقوا فعلا الشركات دي ان الشركات دي انا
[07:09] عايز اقولك انا واحد من الناس بس لو خش على الايميل بتاع انا
[07:12] بتابع فوق انا عندي تقريبا لو افتح لك لك طب مش فاكر حوالي
[07:16] ستة وتمانين شحنة انا بطالبهم للمختلف المراكب عندي منهم مثلا بتاع
[07:24] تلاتين اربعين بالمراكب بالطيران في منهم اربعين خمسين شحنة مثلا جايب
[07:29] البحر طب تخيل بقى الشركة اللي بتعمل دي زي زي شركات الشحن
[07:36] الجود ارى مقصودية تشيله وبتعلق في حاجات شركات شحن بحر شركات
[07:41] شحن بحر دي بيتتابع كم واحدة ده انا كلاينت واحد لي عندي
[07:46] مثلا فوق تلاتين اربعين شو احنا ماشية في البحر طب هو بيتابع كم
[07:51] ممكن يكون بيتابع متين تلتمية شحنة فما السوفتوير ده يبقى عنده انت
[07:56] سهلتلو الشغل بلس انه ممكن تحط في السوفتوير انه هو يعمل
[08:01] اوتوماتيك سند ايميل انترنالي الشركة اللي بتتابع وممكن يعمل
[08:08] اوتوماتيك انه هو يبعت ايميل لي انا اللي هو الكلاينت بتاع
[08:12] الشركة دي فانت كده بعملت السوفتوير فعلا ممكن يتسوق يفيد يسهل
[08:18] على حد شغلته فيبقى له قيمة ذو فاليو انه ممكن تسوقوا
```

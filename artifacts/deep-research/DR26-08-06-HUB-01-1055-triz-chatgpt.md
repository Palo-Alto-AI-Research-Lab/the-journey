---
source: chatgpt.com (GPT Pro + Web search, «Pro thinking»)
dr_id: DR26-08-06-HUB-01-1055
topic: "ТРИЗ для агентных/инфраструктурных задач + пересечение с АК-47"
date: 2026-08-06
origin: fan-out DR26-08-06-HUB-01-1055, забор с хаба HUB-1 (backend JSON + Bearer)
chat: https://chatgpt.com/c/<chat-id>
evidence_of_work: "104 ссылки в теле, 53 уникальных домена/URL; 37 узлов mapping; widget_state НЕТ (это не DR-виджет, а Pro+Web search — судим по URL, не по счётчику цитат)"
collected: 2026-08-06 Lisbon
real_author: GPT (OpenAI)
note: verbatim, отчёт целиком из mapping (parts[0]); ответ на русском
---

# DR26-08-06-HUB-01-1055  
## TRIZ для отладки агентных AI-систем: полезное ядро, слабая доказательность и конфликт с принципом AK‑47

**Срез источников:** 6 августа 2026 года.  
**Объект оценки:** не TRIZ как философия инноваций вообще, а его практическая ценность для небольшого парка автономных агентов, браузерных профилей, Syncthing-доставки, watchdog’ов и распределённых рутин.

---

# Исполнительный вердикт

**[established] TRIZ — реальный набор инструментов для структурирования противоречий и генерации вариантов, но не подтверждённая экспериментальная теория решения инженерных проблем.** Его сильная часть — заставить явно сформулировать, какой полезный эффект покупается каким вредом, разделить конфликтующие требования во времени/пространстве/по условиям и проверить, можно ли удалить компонент. Его слабая часть — обещание систематически вести к изобретательским решениям посредством универсальной матрицы, «законов эволюции», Su‑Field и ARIZ.

**[established] Доказательств, что TRIZ превосходит hypothesis-driven debugging, профессиональную экспертизу, DOE, STPA, FTA или другие методы по MTTR, повторяемости инцидентов, defect escape либо ремонтопригодности, нет.** Лучшие сравнительные исследования измеряли в основном количество, новизну или проработанность идей у студентов и сотрудников; результаты смешанные, выборки малы, production-метрик почти нет. Прямого контролируемого сравнения для SRE или agentic AI я не нашёл: **[no source found]**.

**[established] Для software/SRE основная литература состоит из аналогий, учебных демонстраций и self-reported case studies.** Обзор software‑TRIZ 2019 года прямо называет область находящейся «в начальной фазе» и отмечает проблемы переноса механической матрицы на software-параметры. В 2024–2026 годах появились LLM+TRIZ-прототипы и бенчмарки, но не доказательство снижения реальных эксплуатационных отказов. citeturn433210view0turn977197view3turn796315view1turn365047view9

**[emerging] Вам имеет смысл не “внедрять TRIZ”, а протестировать четырёхэлементный фильтр `AK‑TRIZ‑4`:**

1. две симметричные формулировки технического противоречия;
2. физическое противоречие и разделение;
3. IFR, ограниченный отсутствием новых постоянно работающих механизмов;
4. trimming с обязательным veto по ремонтопригодности.

Применять его следует **только после** evidence-driven локализации причины. Без воспроизводимого нарушения полезного эффекта и причинной цепочки TRIZ запрещён: иначе отсутствие телеметрии или дисциплины доставки будет переименовано в красивую «изобретательскую задачу».

**[emerging] Итоговая рекомендация:** 30-дневный контролируемый пилот `Evidence Loop + AK‑TRIZ‑4`; не изучать полную матрицу, Su‑Field, 76 стандартов и ARIZ, не покупать сертификацию. Главная предпосылка рекомендации: хотя бы часть повторяющихся отказов действительно вызвана неразрешёнными архитектурными trade-off’ами, а не банальным отсутствием end-to-end проверки и владельца доставки.

**Сильнейший аргумент против моей рекомендации:** все четыре ваших противоречия приводят к стандартным инженерным паттернам — независимый внешний наблюдатель, разделение enrollment/run-time, pull-based convergence и dead-man lease. TRIZ может оказаться лишь дополнительным словарём поверх того, что хороший SRE сформулировал бы быстрее без него.

---

# A. Что TRIZ представляет собой без мифологии

## A1. Основные инструменты: для какой формы задачи, что подаётся на вход и что получается на выходе

TRIZ разумнее понимать не как один алгоритм, а как стек из трёх слоёв:

1. **моделирование задачи:** функции, вред, ресурсы, причинные цепочки;
2. **формулирование конфликта:** технические и физические противоречия;
3. **генераторы направлений:** принципы, разделение, стандарты, тренды, ARIZ.

### Карта инструментов

| Инструмент | Форма задачи | Вход | Выход | Вердикт для software/ops |
|---|---|---|---|---|
| **Техническое противоречие** | Улучшение свойства A ухудшает B | Формула `IF X, THEN A improves, BUT B worsens`; затем обратная формула | Явный trade-off; при классическом подходе — параметры матрицы и несколько inventive principles | **Высокая полезность:** заставляет показать обе стороны решения |
| **Матрица 39×39 / 40 принципов** | Конфликт можно выразить через два из 39 универсальных параметров | Улучшаемый и ухудшаемый параметры | 1–4 принципа, статистически часто встречавшиеся в соответствующей ячейке | **Низкая–средняя:** software-перевод параметров неоднозначен |
| **Физическое противоречие** | Один объект или параметр должен одновременно быть P и не‑P | Объект, свойство и два противоположных требования с обоснованием | Задача на разделение требований | **Высокая полезность:** особенно для independence/visibility, security/usability |
| **Принципы разделения** | P и не‑P не обязаны выполняться в одном месте, времени или режиме | Физическое противоречие | Разделение во времени, пространстве, по условию либо между целым и частями | **Высокая полезность:** близко к fault containment и control/data-plane split |
| **IFR — Ideal Final Result** | Команда преждевременно обсуждает механизм вместо эффекта | Полезная функция, вред, доступные ресурсы | Формулировка результата, где функция выполняется с минимумом системы | **Высокая полезность**, но только с явной ценой coupling и repairability |
| **Идеальность** | Нужно сравнить концепты на уровне ценности, затрат и вреда | Полезные функции, costs, harms | Эвристическое отношение `Σ benefits / (Σ costs + Σ harms)` | Полезно как checklist; **не является калиброванной метрикой** |
| **40 inventive principles** | Нужны направления за пределами первого очевидного решения | Противоречие либо просто описание проблемы | Аналогические prompts: segmentation, prior action, feedback и т. п. | Отдельные принципы полезны; полный набор создаёт много метафорического шума |
| **Trends/laws of evolution** | Нужны гипотезы о следующем направлении развития системы | История системы, value parameter, ресурсы, supersystem | Направления: dynamization, controllability, переход в supersystem, повышение идеальности | Использовать как prompts, **не как прогнозные законы** |
| **Su‑Field / веполь** | Между компонентами отсутствует, недостаточна или вредна связь | «Вещества» S1/S2 и «поле» F, описывающее взаимодействие | Модифицированная модель взаимодействия | Для software часто превращается в произвольную метафору |
| **76 standard solutions** | Su‑Field уже построен и нужно выбрать типовую трансформацию | Полный/неполный/вредный Su‑Field | Один из классов стандартных трансформаций | **Низкая полезность** для вашего стека |
| **ARIZ** | Сложное противоречие не поддаётся принципам | Мини-задача, conflicting pair, ресурсы, operating zone/time | Последовательно уточнённое противоречие, IFR и концепт решения | Высокая стоимость; evidence для software слаб |
| **Function Analysis** | Непонятно, кто что делает и где возникает вред | Компоненты, функции, объекты функций, качество взаимодействий | Карта useful/harmful/insufficient/excessive functions | **Высокая полезность**, если не строить огромную онтологию |
| **Trimming** | Система обросла компонентами | Компонент и выполняемые им функции | Удаление компонента с переносом необходимых функций | Максимально близко к AK‑47, но может спрятать coupling |
| **CECA** | Симптом имеет несколько уровней причин | Наблюдаемое disadvantage и связи `causes/contributes to` | Cause–effect chain и кандидаты на ключевые причины | Полезно, но конкурирует с обычной causal graph / fault tree |
| **Nine Screens** | Команда застряла на локальном текущем симптоме | Система | 3×3: subsystem/system/supersystem × past/present/future | Полезно для profile drift и distributed delivery |
| **Size–Time–Cost operator** | Нужен выход из фиксированного масштаба мышления | Текущий механизм | Идеи при экстремально малом/большом размере, времени или стоимости | Умеренно полезный thought experiment |
| **Smart Little People** | Процесс трудно представить на микроуровне | Процесс или поле взаимодействия | Антропоморфная симуляция множества маленьких исполнителей | Для multi-agent систем слишком легко спутать метафору с архитектурой |

MATRIZ определяет техническое противоречие через две альтернативные пары `IF–THEN–BUT`, а физическое — как два обоснованных противоположных требования к одному параметру. Классическая матрица содержит 39 улучшаемых и ухудшаемых параметров; ячейки указывают до четырёх из 40 принципов, некоторые ячейки пусты, матрица несимметрична. Это **источник аналогий**, не механизм вычисления оптимального решения. ([MATRIZ: Engineering Contradiction, ©2023](https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/contradictions/engineering-contradiction-5995/); [Contradiction Matrix, ©2023](https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/contradictions/engineering-contradiction-5995/contradiction-matrix-6026/)) citeturn834351view0turn834351view2

Физическое противоречие обычно разрешается разделением:

- **во времени:** свойство P во время enrollment, не‑P во время unattended run;
- **в пространстве:** sensor внутри процесса, decision maker вне процесса;
- **по условию:** разрешить мутацию только при выполнении eligibility predicate;
- **между целым и частями / системными уровнями:** вся система независима, но минимальный внутренний probe экспортирует состояние.

Последний вариант в разных школах называется separation by system level, whole/parts либо scale. ([MATRIZ: Physical Contradiction, ©2023](https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/contradictions/physical-contradiction-6056/); [Oxford Creativity glossary, без даты, доступ 06.08.2026](https://www.triz.co.uk/glossary)) **[single-source для конкретной четырёхчастной классификации] [low-authority: consultancy]** citeturn834351view1turn822716view6

### IFR и идеальность

Классическая эвристика идеальности записывается как:

\[
\text{Ideality} =
\frac{\sum \text{Useful Functions}}
{\sum \text{Costs} + \sum \text{Harms}}
\]

Для механизма выбора software-архитектуры эта формула неполна. В знаменатель необходимо явно включить:

\[
\text{hidden state}
+\text{coupling}
+\text{external dependencies}
+\text{recovery complexity}
+\text{novice repair cost}
\]

Иначе «идеальным» станет, например, self-healing control plane, который в нормальном режиме почти не требует человека, но после общего отказа невозможно понять и восстановить без автора. MATRIZ описывает IFR как идеальную функцию при минимальной системе и идеальность как отношение benefits к payment factors; это целевая рамка, а не измерительная теория. ([MATRIZ Glossary, ©2023](https://wiki.matriz.org/docs/triz/glossary-6146/)) citeturn395312view1

### Какие из 40 принципов действительно переносятся

Для software и ops непосредственно читаются:

- segmentation;
- taking out;
- merging;
- universality;
- prior action;
- beforehand cushioning;
- the other way round;
- dynamization;
- partial/excessive action;
- periodic action;
- skipping;
- feedback;
- intermediary;
- self-service;
- copying;
- cheap short-living objects;
- discarding and recovering.

Остальные можно натянуть на software, но часто только постфактум: «thermal expansion», «strong oxidants» или «porous materials» превращаются в свободные метафоры, и почти любой найденный дизайн удаётся объявить реализацией какого-нибудь принципа. Полный список MATRIZ сам описывает как abstract solution patterns, а не как domain-specific software rules. ([MATRIZ: Inventive Principles, ©2023](https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/contradictions/inventive-principles-6023/)) citeturn834351view3

### Trends и «законы эволюции»

На практике это не законы в смысле проверяемых физических закономерностей. Это набор направляющих гипотез:

- **S‑curve:** зарождение → рост → зрелость → насыщение;
- **increasing ideality:** больше полезного эффекта на единицу payment;
- **transition to supersystem:** функция переезжает в окружение или более крупную систему;
- **trimming:** компонент исчезает, его функция удаляется либо перераспределяется;
- **dynamization:** фиксированная структура становится адаптивной;
- **increasing controllability:** больше управляемых параметров и обратной связи;
- **coordination/rhythm:** согласование временных и пространственных режимов подсистем.

Для вашего парка это годится как список вопросов — например, «может ли проверка результата быть естественным побочным продуктом самой работы?» — но не как основание прогнозировать, что adaptive multi-agent control plane обязательно является следующей «эволюционной стадией». ([MATRIZ Glossary, ©2023](https://wiki.matriz.org/docs/triz/glossary-6146/)) citeturn395312view2turn395312view3

### Su‑Field, 76 стандартов и ARIZ

Su‑Field моделирует систему как два вещества/объекта и поле, обеспечивающее взаимодействие. 76 стандартов сгруппированы в пять классов, содержащих типовые способы достроить, изменить или разрушить такое взаимодействие. В физической инженерии это может быть достаточно конкретно; в software границы между «substance», «field», протоколом, данными и control signal выбирает сам аналитик, поэтому одна и та же архитектура допускает несколько несовместимых моделей. ([MATRIZ: Standard Inventive Solutions, ©2023](https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/substance-field-modeling/standard-inventive-solutions/)) citeturn834351view4

ARIZ‑85C — длинный алгоритм, начинающийся с выбора ключевой задачи и формулирования mini-problem, затем уточняющий conflicting pair, operating zone/time, ресурсы и IFR. Публичные работы за пределами механической инженерии, включая telecom/software, иногда сообщают применение ARIZ, но я не нашёл исследования, измеряющего:

- сколько команд действительно доводит ARIZ до конца;
- насколько соблюдается процедура;
- сколько времени он занимает;
- насколько результат лучше короткой contradiction-сессии.

Ответ на вопрос «действительно ли кто-то регулярно проходит полный ARIZ вне классической инженерии» поэтому: отдельные примеры есть, статистики применения нет — **[no source found]**. ([MATRIZ: ARIZ template, ©2023](https://wiki.matriz.org/docs/triz-tools/ariz-template/)) citeturn395312view4turn395312view5

---

## A2. Классическая TRIZ, современные производные и рынок сертификации

| Школа/бренд | Что это | Контроль и ownership | Что является маркетингом |
|---|---|---|---|
| **Classical TRIZ** | Altshuller-era contradictions, matrix, principles, ARIZ, Su‑Field, evolution patterns | Нет одного владельца всего корпуса; существуют авторские права на конкретные тексты и учебные программы | Представление всего корпуса как единой экспериментально подтверждённой науки |
| **MATRIZ** | Международная ассоциация, glossary, curriculum, exams, registry | Управляет собственной пятиуровневой сертификацией | «Globally recognized» — утверждение самой ассоциации, не ISO-аккредитация |
| **I‑TRIZ** | Развитая Ideation International версия: problem formulation, failure analysis, directed evolution, software | Коммерческая методология и инструменты Ideation International | Vendor case studies не равны независимым controlled evidence |
| **GEN‑TRIZ** | Коммерческая школа с Innovation Navigator и собственной учебной архитектурой | Продукт/курс GEN TRIZ | Proprietary tooling и training claims |
| **TRIZ+** | Неоднозначный ярлык для дополненной или расширенной TRIZ | Единого стандарта или владельца не найдено | Название может создавать ложное впечатление общепринятой версии — **[no source found]** |
| **SIT** | Systematic Inventive Thinking: subtraction, multiplication, division, task unification, attribute dependency | Коммерческая методология SIT; исторически родственна ASIT/TRIZ | Более простой бренд не доказывает превосходства |
| **USIT** | Unified Structured Inventive Thinking Эда Сикафуса, разработанная в Ford как упрощение | Авторская методика, широко документированная вне MATRIZ | Утверждение, что упрощение сохраняет всю силу TRIZ, экспериментально не доказано |
| **ISO innovation management** | Серия ISO 56000 по управлению инновациями | Международные стандарты ISO/TC 279 | Это не стандартизация TRIZ и не MATRIZ certification |

I‑TRIZ является коммерческой методологией Ideation International, основанной в 1992 году; GEN‑TRIZ продаёт собственный Innovation Navigator и обучение; SIT — отдельная частная компания и методика из пяти основных шаблонов; USIT был сформирован Эдом Сикафусом в Ford в 1995 году как значительно упрощённый подход без обязательной работы с большими базами принципов. ([Ideation International, без даты, доступ 06.08.2026](https://ideation-triz.com/en/company/); [GEN TRIZ Basic Course, без даты](https://www.gen-triz.com/basic-triz-course); [SIT company profile, 2019](https://www.sitsite.com/wp-content/uploads/2019/02/About-SIT-Innovation-V5-Yoni.pdf); [Sickafus, USIT Overview, 14.02.2003](https://www.osaka-gu.ac.jp/php/nakagawa/TRIZ/eTRIZ/eSickafusMemorial/eSickafus-TextBooks-Tutorials/USITOverView-030214.pdf)) citeturn535448view0turn963676search26turn535448view2turn535448view3

### Исправление ошибки в постановке: «ISO 69580:2025»

**[established] Стандарта `ISO 69580:2025` по TRIZ не существует.**

URL ISO с внутренним идентификатором страницы `standard/69580.html` ведёт к **ISO 11929‑2:2019**, стандарту по измерениям ионизирующего излучения; он был заменён версией ISO 11929‑2:2025. Число 69580 в URL — внутренний идентификатор страницы, не номер стандарта. ([ISO 11929‑2:2019, опубликован 02.2019](https://www.iso.org/standard/69580.html)) citeturn255008view0

Релевантная ISO-серия по innovation management — **ISO 56000**: например, ISO 56007:2023 об управлении возможностями и идеями, ISO 56001:2024 о системах управления инновациями, ISO 56008:2024 об измерении инновационной деятельности и ISO/TR 56009:2025 с примерами измерений. В каталоге ISO/TC 279 TRIZ не выделена как сертифицируемый международный стандарт. ([ISO 56007:2023, 08.2023](https://www.iso.org/standard/75068.html); [ISO/TC 279 catalogue, доступ 06.08.2026](https://www.iso.org/committee/4587737/x/catalogue/p/1/u/1/w/0/d/0)) citeturn255008view1turn255008view2

MATRIZ управляет собственной пятиуровневой сертификацией: уровни 1–3 Practitioner, 4 Specialist, 5 Master; сертификаты не имеют срока действия. Это credential ассоциации, не ISO certification. ([MATRIZ Certification, без даты, доступ 06.08.2026](https://matriz.org/certification/)) citeturn255008view3

### Что исторически установлено, а что нет

**[established]** Altshuller и последователи выводили повторяющиеся patterns из патентов и инженерных решений.

**[speculative]** Утверждение, что классическая матрица представляет строгую статистическую модель из репрезентативной патентной выборки. Разные источники называют от десятков тысяч до миллионов рассмотренных патентов; я не нашёл публичного архива исходной выборки, полного coding manual, inter-rater reliability или воспроизводимого процесса реконструкции матрицы: **[no source found]**.

Поэтому корректная формулировка: **TRIZ исторически патент-derived, но статистическая валидность и репрезентативность исходного mining независимо не воспроизведены.**

---

## A3. Доказательная база

### Лучшие найденные сравнительные исследования

| Исследование | N и дизайн | Что получилось | Главные ограничения |
|---|---:|---|---|
| **Hernandez et al., 2013** | UTEP: 20 control, 9 TRIZ; UMD: 20 участников, pre/post | TRIZ не дал значимого роста числа идей; средняя novelty выросла в обоих экспериментах; max novelty — только в одном | Малые и неравные группы; студенты; разные режимы времени; proxy outcome |
| **Birdi, Leach & Magadley, 2012** | 123 TRIZ trainees и 96 comparison employees; longitudinal/multisource field study | Рост навыков и мотивации; позже — больше предложений; эффект на implemented innovation и performance непоследователен | Не RCT; self-selection и organizational confounding |
| **Dathe, 2015** | 6 опытных практиков, две группы по три, crossover-like tasks | TRIZ лучше проявлялся на чётко заданных технических задачах и novelty; слабее на fuzzy problems | Диссертация, N=6, ограниченная внешняя валидность |
| **Ge et al., 2025** | N=32, 2×2 mixed design, human-human vs human-agent и brainstorming vs TRIZ | Brainstorming дал больше идей; TRIZ — выше elaboration; значимого преимущества по originality/flexibility не было | Малый student sample, design task, не production |
| **Čok et al., DESIGN 2026** | N=114, TRIZ-C без/с LLM support, три задачи | Общий effect метода `p=.076`; значимое улучшение качества только на одной задаче; LLM снизил воспринимаемую сложность | Зависимость от типа задачи, model/prompt skill, student sample |
| **Industrial case reviews** | 200+ опубликованных случаев в одном обзоре | Много положительных narratives | Нет denominator, контрольных групп, стандартных outcome metrics |

В исследовании Hernandez quantity не улучшилась статистически значимо (`p=.181` и `.573`), тогда как average novelty была выше (`p=.014` и `.004`); max novelty повторилась не во всех условиях. Это говорит о возможной пользе prompts для выхода из первого очевидного решения, но не о лучшем engineering outcome. ([Hernandez et al., *Systematic Ideation Effectiveness Study of TRIZ*, октябрь 2013](https://asmedigitalcollection.asme.org/mechanicaldesign/article/135/10/101009/380571/Systematic-Ideation-Effectiveness-Study-of-TRIZ)) citeturn842369search0turn842369search4

Birdi и коллеги нашли краткосрочное улучшение problem-solving skills и motivation и последующий рост числа предлагаемых идей, но результаты для их внедрения и performance были непоследовательны. Это один из сильнейших организационных результатов в пользу TRIZ, но дизайн не рандомизирован и не изолирует метод от тренера, внимания менеджмента и отбора сотрудников. ([Birdi, Leach & Magadley, *Evaluating the impact of TRIZ creativity training*, сентябрь 2012](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1467-9310.2012.00686.x)) citeturn842369search13turn842369search1

Эксперимент Ge et al. 2025 года особенно полезен против маркетингового тезиса «структурированный метод всегда лучше brainstorming»: brainstorming породил больше идей, TRIZ дал большую elaboration, но значимого преимущества по originality и flexibility не показал. ([Ge et al., опубликовано online в 2025 году](https://doi.org/10.1017/S7821986623255907)) citeturn255008view6turn958722view2turn958722view3

В исследовании Čok et al. 2026 года общий эффект сравниваемого метода на качество решений не достиг обычного порога значимости (`F(1,108)=3.21, p=.076`); один из трёх problem types показал сильное различие, два — нет. LLM-support заметно снизил субъективную сложность работы с противоречиями, но это не эквивалентно более надёжному решению. ([Čok et al., DESIGN 2026](https://doi.org/10.1017/pds.2026.10409)) citeturn255008view7turn958722view6turn958722view8

### Что в доказательствах отсутствует

Я не нашёл:

- RCT TRIZ против TOC, DOE, STPA, FTA или expert debugging;
- controlled production study по MTTR;
- измерения 7/14/30-дневной повторяемости инцидентов;
- измерения false-green rate;
- сравнения defect escape после TRIZ и обычного design review;
- оценки «может ли novice восстановить систему по runbook»;
- trial, где учитывается число новых зависимостей, состояний и recovery branches.

Для всех пунктов: **[no source found]**.

### Основные опубликованные и логические критики

#### 1. Method–outcome mismatch

TRIZ позиционируется как способ получения сильных изобретательских решений, а исследования обычно измеряют novelty rating, fluency и elaboration. Между «идея получила на один балл больше за новизну» и «инциденты перестали повторяться» нет проверенного причинного моста.

#### 2. Patent-selection opacity

Публично не показано, как исходные патенты были выбраны, как отличались inventive/non-inventive patents, как кодировались параметры и принципы и насколько разные кодировщики соглашались друг с другом. Поэтому matrix может отражать полезную экспертную таксономию, но её нельзя проверить как воспроизводимую статистическую модель.

#### 3. Post-hoc rationalization

После того как решение известно, почти всегда можно найти один или несколько принципов, которые на него похожи. Это не доказывает, что принцип предсказал решение. Работы по реконструкции и domain-oriented подбору принципов отмечают зависимость результата от того, как именно сформулировано противоречие и выбран domain corpus. ([Borgianni et al., 2021](https://www.cambridge.org/core/journals/design-science/article/process-for-individuating-triz-inventive-principles-deterministic-stochastic-or-domainoriented/81CEAB2A6483D6A7D42010A22F5B9839)) citeturn842369search19

#### 4. Case-study publication bias

Консультанты и внедрившие TRIZ команды публикуют успешные истории; не публикуется число сессий, которые ничего не дали, предложили уже известное решение или привели к более сложной системе. Обзор более 200 industrial cases систематизировал применения, но не превратил их в контролируемое сравнение. ([Spreafico & Russo, 2016](https://www.researchgate.net/publication/541379219_TRIZ_Industrial_Case_Studies_A_Critical_Survey)) citeturn661434view7

#### 5. Learning curve и отсутствие общепринятого application standard

Обзоры отмечают сложность языка TRIZ, значительное усилие на обучение и отсутствие единой процедуры применения. Другой обзор подчёркивает разрыв между амбициозными целями TRIZ и сравнительно умеренным распространением в промышленной практике. ([Ilevbare et al., 2013](https://www.sciencedirect.com/science/article/abs/pii/S4828306389756549); [Chechurin & Borgianni, 2016](https://www.sciencedirect.com/science/article/abs/pii/S7045764260520573)) citeturn842369search10turn842369search7

### Честный баланс

**Steelman позиции сторонников:** структура противоречия снижает premature compromise, а аналогические prompts могут повышать среднюю novelty и проработанность. В организации обучение может изменить поведение, а не только дать одноразовые идеи.

**Steelman критической позиции:** тот же эффект может быть вызван любым серьёзным structured workshop, вниманием тренера, увеличенным временем на задачу или требованием сформулировать несколько альтернатив. Без сравнения с сильным hypothesis-driven baseline специфический вклад TRIZ неизвестен.

### Эксперимент, который действительно решал бы вопрос

Для вашего парка нужен preregistered crossover:

- 6–12 повторяющихся классов отказов;
- случайное чередование `Evidence Loop only` и `Evidence Loop + AK‑TRIZ‑4`;
- одинаковый timebox;
- заранее определённые outcomes:
  - правильная root cause;
  - время до воспроизводимого теста;
  - время до исправления;
  - повторение за 14/30 дней;
  - false-green events;
  - число добавленных services/states/dependencies;
  - успешность ремонта вторым человеком по runbook.

Именно такой тест отличит метод от красивого языка.

---

# B. TRIZ в software, SRE и agentic AI

## B1. Реальные задокументированные применения

### Общая картина

Обзор 2019 года нашёл software-применения с 1999 года — concurrency, buffers, design patterns, object-oriented design, testing и telecom — но охарактеризовал поле как «very initial phase». Авторы также отмечали, что классические mechanical parameters трудно отображать на software и что значительная часть работ представляет собой адаптацию принципов, а не outcome studies. Один из авторов был связан с IFR Consulting, что не обесценивает обзор, но создаёт conflict-of-interest context. ([Govindarajan, Sheu & Mann, 2019](https://ojs.ijosi.org/index.php/IJOSI/article/download/175/374/2388)) citeturn433210view0

### Найденные кейсы с хотя бы какими-то числами

| Организация/проект | Год | Проблема | TRIZ-инструмент | Сообщённый результат | Качество доказательства |
|---|---:|---|---|---|---|
| **AKIVA software design**, организация не раскрыта | 2008 | Сложность архитектуры identity/data-masking software | Ideality, function/component analysis, system complexity | LOC `7,964 → 3,866`; авторская complexity `88.7 → 81.2` | **[single-source] [low-authority] self-reported** |
| **Telecom CPM/IMS prototype** | 2017 | Реализация мобильной communication service до полного IMS | ARIZ‑85C | Сообщено об implementation concept/prototype | Нет MTTR, defects, cost или independent replication |
| **OptimalSQM software testing project** | 2018 | Выбор test techniques по фазам SDLC/STLC | TRIZ + Taguchi/DOE | DRE baseline `87.66%`; model `94.03%`; retrospective verification `93.43%` | **[single-source]**; вклад TRIZ не отделён от DOE |
| **Университетский IT department, 8 Scrum-команд** | 2023 | Повторяющиеся Scrum-проблемы | Engineering contradiction + matrix | 56 sprints, 195 проблем, 13 частых; пять practitioners выбрали три рекомендации | Нет post-implementation outcome |
| **Confidential software modeling case** | 2022/2023 | Function modeling и trimming software | Function analysis/trimming | Дошёл до initial solution concepts | Дальнейший результат скрыт/confidential |
| **SRE / distributed agents / LLM operations** | 2024–2026 | Production incidents | — | Ни одного независимого случая с MTTR/incident reduction не найдено | **[no source found]** |

В AKIVA автор сообщил сокращение кода более чем наполовину и снижение собственной complexity measure. Но работа не предоставляет контрольного baseline development process, defect rate, maintenance effort, independent code audit или долгосрочный follow-up. Это пример того, что TRIZ можно использовать для реального redesign, но не доказательство, что TRIZ вызвал улучшение. ([Bhushan, *Ideality, TRIZ and Software Design — A Case Study*, 2008](https://www.aitriz.org/articles/TRIZFeatures/30383035-4268757368616e.pdf)) **[single-source] [low-authority: association-hosted case]** citeturn498928view3turn498928view5

В работе по testing авторы совместили TRIZ с Taguchi design of experiments. Из 18 комбинаций test techniques была построена response-surface модель; оптимальная комбинация дала retrospective DRE 93.43% против исходных 87.66%. Но количественный выбор комбинации выполняет именно DOE/Taguchi; статья не показывает контрфактический результат «тот же DOE без TRIZ». ([Lazić et al., май 2018](https://ijiet.com/wp-content/uploads/2018/06/22.pdf)) **[single-source]** citeturn498928view6turn498928view7

В Scrum-кейсе было проанализировано 56 спринтов восьми команд за восемь месяцев: обнаружено 195 problem instances, выделено 13 повторяющихся проблем, после чего пять TRIZ practitioners выбрали три рекомендации из матрицы. Это application, но не outcome study: статья не показывает, сократилось ли число проблем после внедрения. ([AIP Conference Proceedings, опубликовано 2023](https://pubs.aip.org/aip/acp/article-pdf/doi/10.1063/5.0115577/16772985/020029_1_online.pdf)) citeturn498928view8turn498928view9

### Вывод B1

**[established]** TRIZ действительно применяли к software-задачам.

**[established]** Публичная доказательная база почти не содержит тех показателей, которые нужны вам: MTTR, false-green, повторение инцидентов, rollout convergence и repairability.

**[speculative]** Заявление, что закрытые corporate cases наверняка заполняют этот пробел. Они могут существовать, но из недоступных данных нельзя выводить effectiveness.

---

## B2. Debugging, reliability, LLM-agents и prompt/architecture design

### a. Debugging и root-cause analysis

CECA предлагается в TRIZ-литературе как альтернатива или дополнение к RCA: вместо ранней фиксации на одном «корне» строится ветвящаяся цепочка причин и disadvantages. Это разумно для распределённых систем, где browser session death может быть следствием нескольких совместных условий. Но найденная литература в основном концептуальна; production comparison CECA против fault tree или hypothesis debugging не найден: **[no source found]**. ([*TRIZ-Based Cause and Effect Chains Analysis vs Root Cause Analysis*, 2015](https://www.researchgate.net/publication/144945393_TRIZ-Based_Cause_and_Effect_Chains_Analysis_vs_Root_Cause_Analysis)) citeturn433210view5

### b. Reliability и fault tolerance

TRIZ применяется для генерации fault-tolerance concepts, но я не нашёл независимого production-исследования, где она снизила outage count, error budget burn или MTTR в распределённой software-системе: **[no source found]**.

Для сравнения с уровнем evidence: Google сообщает, что A/B-tested Incident Hypothesis — автоматически собранная, но проверяемая гипотеза для on-caller — снизила Mean Time to Mitigate примерно на 10%; Investigation Dashboards для поддерживаемых инцидентов дали примерно 44% reduction, а AI Operator был прогнан на тысячах incidents с сохранением execution traces. Это не TRIZ: это evidence aggregation, hypothesis formation, domain-specific checks и controlled authorization. ([Google SRE, *AI Engineering Reliable Operations*, без указанной даты; доступ 06.08.2026](https://sre.google/resources/practices-and-processes/ai-engineering-reliable-operations/)) citeturn498928view0turn498928view1turn498928view2

### c. LLM-agent и multi-agent systems

Для production LLM-agent reliability — watchdog independence, tool-call delivery, action verification, memory drift, multi-agent consensus — реального TRIZ case с operational outcome я не нашёл: **[no source found]**.

### d. Prompt и AI architecture design

Здесь работы появились в 2024–2026 годах:

- prompt pipelines, переводящие текст задачи в TRIZ contradiction;
- RAG по inventive principles и patent cases;
- multi-agent TRIZ workflows;
- benchmarks по распознаванию принципов;
- эксперименты «человек + LLM + TRIZ».

Они показывают, что LLM способен упростить работу с терминологией и retrieval, но пока не показывают устойчивого production debugging.

---

## B3. Четыре ваших противоречия, разобранные по TRIZ

## 1. Watchdog должен быть независимым, но видеть внутреннее состояние

### Техническое противоречие

**Формула A**

> **IF** watchdog работает внутри процесса или на том же control plane,  
> **THEN** он видит queues, locks, internal state и точные ошибки,  
> **BUT** умирает вместе с процессом, runtime, машиной или общей конфигурацией.

**Обратная формула**

> **IF** watchdog вынесен на независимую машину и runtime,  
> **THEN** он переживает отказ наблюдаемой системы,  
> **BUT** видит лишь внешний интерфейс и может пропустить внутренний deadlock или no-op.

### Физическое противоречие

> Наблюдатель должен быть **внутри** системы, чтобы иметь богатую видимость, и одновременно **снаружи**, чтобы не разделять её failure domain.

### Разделение

- **В пространстве:** внутренний probe, внешний judge.
- **Между целым и частями:** sensing-компонент внутри; decision, expiry и alerting снаружи.
- **По условию:** внутренние данные используются как диагностический контекст, но success определяется внешним полезным эффектом.

### IFR

> Работа системы естественным образом оставляет минимальное, проверяемое доказательство полезного эффекта; независимый наблюдатель проверяет это доказательство, не полагаясь на runtime и status flag наблюдаемой системы.

### AK‑47-решение

- Внутренний job пишет атомарный receipt: `job_id`, `input_version`, `effect_hash`, `completed_at`.
- Независимый VPS проверяет:
  - что receipt свежий;
  - что `effect_hash` соответствует реальному target artifact;
  - что sequence number монотонно растёт.
- Alert возникает по expiry или mismatch.
- Никакой второй observability platform, distributed tracing или shared database.

**COMPLICATION:** один формат receipt и один внешний checker.  
**Боль:** «процесс зелёный, полезного результата нет».  
**Что ломается без него:** common-mode false green.  
**Дешёвая альтернатива:** ежедневная ручная проверка target artifact.

### Дал ли TRIZ неочевидное решение?

**Нет, в основном переименовал известный SRE-паттерн black-box + white-box monitoring.** Полезный вклад — заставил разделить sensing и judgment и сформулировать, что success равен effect, а не process exit.

---

## 2. Browser automation должна выглядеть как logged-in human, но работать без человека

Здесь нужно отделить legitimate session continuity от обхода anti-abuse controls. TRIZ не должен использоваться для поиска способов имитировать человека или обходить challenge/ban mechanisms.

### Техническое противоречие

**Формула A**

> **IF** automation использует долговечный человеческий browser profile,  
> **THEN** доступны subscription-only функции и нормальная пользовательская сессия,  
> **BUT** профиль накапливает drift, locks, re-auth challenges и machine-specific state.

**Обратная формула**

> **IF** каждый запуск использует чистый воспроизводимый automation profile,  
> **THEN** среда детерминирована и легко восстанавливается,  
> **BUT** отсутствует доверенная пользовательская сессия и часть функций недоступна.

### Физическое противоречие

> Человек должен **присутствовать** для enrollment, consent и challenge resolution и одновременно **отсутствовать** во время обычного выполнения.

### Разделение

- **Во времени:** human enrollment/reauth отдельно от routine execution.
- **По условию:** unattended run только пока lease/session-state явно `VALID`; challenge переводит rail в `NEEDS_HUMAN`.
- **В пространстве:** один canonical profile на одном designated host; остальные машины отправляют задания, а не копируют живой профиль.

### IFR

> Поддерживаемая сервисом пользовательская сессия имеет явный статус, срок и процедуру восстановления; routine jobs выполняются без человека, а при challenge останавливаются без повреждения профиля и без попытки обхода проверки.

### AK‑47-решение

- Один host/profile на сервис.
- Один process owner браузерного профиля.
- Queue заданий через существующую шину.
- Явные состояния: `READY`, `BUSY`, `NEEDS_HUMAN`, `QUARANTINED`.
- Проверка реального результата после каждого job.
- Никакого синка живых cookie DB, lock-файлов и всего browser profile между пятью машинами.

**COMPLICATION:** очередь и четыре явных состояния.  
**Боль:** profile/cookie drift и конкурентная порча профиля.  
**Что ломается без этого:** несколько машин считают себя владельцами одной mutable identity.  
**Дешёвая альтернатива:** ручной запуск на единственной машине.

### Дал ли TRIZ неочевидное решение?

**Нет.** Separation in time хорошо формулирует human-in-the-loop boundary, но one-profile-one-owner и stop-on-challenge являются обычным инженерным ответом. Главная причина отказов здесь часто не «противоречие», а попытка распределить mutable browser identity как обычные файлы.

---

## 3. Fix должен попасть на пять машин, но они бывают offline, а shares местами receive-only

### Техническое противоречие

**Формула A**

> **IF** hub активно push’ит и применяет fix на всех nodes,  
> **THEN** rollout централизован и может быть быстрым,  
> **BUT** offline и receive-only nodes недоступны, а hub получает опасное право удалённой мутации.

**Обратная формула**

> **IF** каждая машина сама решает, когда применять fix,  
> **THEN** intermittent connectivity и receive-only transport не мешают доставке,  
> **BUT** возникает риск version drift и silent non-application.

### Физическое противоречие

> Node должна быть **недоступна для удалённой записи**, чтобы сохранить receive-only boundary, и одновременно **доступна для изменения**, чтобы применить fix.

### Разделение

- **По условию:** transport может только положить immutable package; локальная машина пишет в рабочее состояние после проверки eligibility.
- **Во времени:** доставка происходит при любом появлении online; применение — в локальном maintenance window.
- **Между целым и частями:** центральный слой владеет artifact, локальный слой — actuation.

### IFR

> Версионированный idempotent fix оказывается на каждой машине при первой возможности, применяется ровно один раз, проверяет свой эффект и оставляет receipt — без необходимости держать все машины online и без удалённого shell-orchestrator.

### AK‑47-решение

```text
inbox/
  fix-2026-08-06-01/
    manifest.json
    apply.py
    verify.py
    rollback.py
    SHA256SUMS
```

Локальный cron:

1. видит новый monotonic version;
2. проверяет checksum и platform constraint;
3. запускает `apply`;
4. запускает `verify`;
5. только после проверки записывает `applied.json`;
6. отправляет receipt через существующую шину;
7. при ошибке выполняет rollback либо оставляет `FAILED_NEEDS_HUMAN`.

Не нужен fleet orchestrator, consensus protocol или отдельная deployment database.

**COMPLICATION:** manifest, локальный applier и receipt.  
**Боль:** «доставлено, но не применено».  
**Что ломается без него:** Syncthing доказывает наличие файла, но не изменение runtime state.  
**Дешёвая альтернатива:** ручной checklist на каждой машине.

### Дал ли TRIZ неочевидное решение?

**Нет, это стандартный pull-based eventual convergence.** TRIZ полезно отделил transport от actuation и позволил сохранить receive-only constraint вместо его отмены.

---

## 4. Indicator должен молчать в норме, но его смерть не должна выглядеть как норма

### Техническое противоречие

**Формула A**

> **IF** indicator сообщает heartbeat постоянно,  
> **THEN** его собственная живость видна,  
> **BUT** оператор получает шум и перестаёт реагировать.

**Обратная формула**

> **IF** indicator сообщает только исключения,  
> **THEN** нормальный режим тихий,  
> **BUT** смерть indicator неотличима от отсутствия проблем.

### Физическое противоречие

> Человеческий канал должен выдавать **ноль сообщений** в норме и одновременно **ненулевое доказательство живости**.

### Разделение

- **По времени:** machine heartbeat частый; human notification только при expiry.
- **По условию:** тишина допустима лишь пока существует свежий independently checked lease.
- **Между целым и частями:** liveness evidence хранится машинно; human attention используется только для нарушения инварианта.

### IFR

> Успешное выполнение полезной работы само обновляет проверяемое доказательство; отсутствие обновления автоматически превращается в событие, поэтому специальный «я жив» notifier не требуется.

### AK‑47-решение

- Каждая routine обновляет не общий зелёный флаг, а receipt полезного эффекта.
- Внешний checker хранит `expires_at`.
- Человек получает только:
  - receipt expired;
  - target mismatch;
  - version divergence;
  - checker’s own upstream dead-man expired.
- Раз в неделю — одна aggregate proof-of-life сводка, а не heartbeat каждой routine.

**COMPLICATION:** lease expiry.  
**Боль:** silent death.  
**Что ломается без него:** `no news` остаётся неразличимым от `notifier dead`.  
**Дешёвая альтернатива:** ручной ежедневный контроль.

### Дал ли TRIZ неочевидное решение?

**Частично.** Dead-man switch известен без TRIZ. Но physical contradiction хорошо показывает, что ошибочно требовать одновременно «тишину» и «человеческий heartbeat» от одного канала. Нужны два уровня: machine evidence и exception-only human alert.

---

## Итог по четырём задачам

| Проблема | Доля реального вклада TRIZ | Что действительно решает задачу |
|---|---|---|
| Watchdog independence/visibility | Небольшая | Failure-domain separation и end-to-end effect probe |
| Human browser/unattended operation | Небольшая | Enrollment/run-time split и single ownership |
| Fix delivery/offline nodes | Небольшая | Immutable artifact, local pull/apply, receipt |
| Quiet indicator/dead notifier | Умеренная framing value | Lease expiry и external effect validation |

**Вердикт:** TRIZ здесь лучше работает как **контроль формулировки**, чем как источник решений.

---

## B4. TRIZ против альтернатив для тех же отказов

Таблица ниже — **[speculative] mechanism-fit assessment**, а не результат head-to-head trials. Прямых сравнительных production trials с TRIZ не найдено.

| Метод | Где он сильнее TRIZ | Где TRIZ сильнее | Оценка обучения* |
|---|---|---|---:|
| **Plain hypothesis-driven debugging** | Логи, воспроизведение, дифференциальные тесты, отбрасывание гипотез | После локализации причины может расширить solution space | 2–4 ч |
| **Kepner–Tregoe** | `is/is-not`, differences/changes, строгая дискриминация возможных причин | TRIZ лучше для redesign после доказанной причины | 8–24 ч |
| **Fault Tree Analysis** | Логика AND/OR, common-cause failures, доказательство путей к top event | TRIZ предлагает архитектурные альтернативы ветвям дерева | 4–12 ч |
| **FMEA** | Систематическая превентивная инвентаризация failure modes и controls | TRIZ помогает, когда mitigation создаёт новый trade-off | 6–16 ч |
| **STAMP/STPA** | Unsafe control actions, feedback failures, distributed control, human/automation interactions | TRIZ дешевле для одного локального противоречия | 16–40 ч |
| **CAST** | Реконструкция системного инцидента, control structure и organizational context | TRIZ быстрее генерирует redesign prompts | 16–40 ч |
| **Systems thinking / causal loops** | Feedback, delays, reinforcing loops, cross-machine drift | TRIZ лучше для дискретного P/not-P trade-off | 8–24 ч |
| **Theory of Constraints** | Поиск bottleneck, throughput и policies, удерживающих ограничение | TRIZ полезнее, если улучшение constraint ухудшает другое свойство | 6–12 ч |
| **Cynefin** | Выбор режима действий при clear/complicated/complex/chaotic context | TRIZ даёт конкретные ideation prompts в complicated domain | 2–4 ч |
| **5 Whys** | Почти нулевая стоимость; годится для короткой линейной цепочки | TRIZ не заставляет притворяться, что существует один root cause | 0.5–1 ч |
| **Design of Experiments** | Причинное различение факторов и interactions при контролируемых экспериментах | TRIZ может предложить факторы/архитектуры для теста | 16–40+ ч |
| **Wardley Mapping** | Build/buy, maturity, strategic dependencies и commodity evolution | TRIZ лучше для локальной инженерной задачи | 6–15 ч |
| **TRIZ minimal** | Явные противоречия, separation, ideation beyond compromise | Слабее почти всех методов в доказательстве root cause | 6–10 ч |
| **Full TRIZ/ARIZ** | Глубокая работа над genuinely inventive design problem | Для routine ops почти всегда слишком дорого | 40–120+ ч |

\* Время — моя planning estimate для рабочей базовой компетенции, а не опубликованный норматив: **[speculative]**.

### Практический порядок для вашего стека

1. **Hypothesis-driven debugging** — что конкретно не произошло?
2. **Fault tree / короткая CECA** — какие сочетания могли привести к loss?
3. **STPA/CAST-lite** — когда отказ связан с несколькими control loops, authority или feedback.
4. **DOE** — когда есть несколько управляемых факторов и достаточное число повторов.
5. **TRIZ** — только когда причина известна, а очевидное исправление создаёт новый вред.

Google SRE формулирует troubleshooting как итеративное построение и проверку гипотез с сопоставлением expected и actual behavior; это ближе к вашим ежедневным инцидентам, чем ARIZ. ([Google, *Effective Troubleshooting*, книга SRE 2016](https://sre.google/sre-book/effective-troubleshooting/)) citeturn365047view4turn365047view5

STPA рассматривает accidents не только как отказы компонентов, но и как небезопасные control actions и inadequate feedback; это особенно релевантно ситуациям «доставка состоялась, применение нет» и «зелёный status не соответствует effect». ([MIT, *STPA Handbook*, март 2018](https://psas.scripts.mit.edu$HOME?name=STPA_handbook.pdf); [CAST Handbook, 2024](https://psas.scripts.mit.edu$HOME?name=CAST_Handbook_Healthcare.pdf)) citeturn796315view5turn796315view6

FMEA стандартизирована IEC 60812:2018, Fault Tree Analysis — зрелый deductive method, а DOE имеет воспроизводимый статистический аппарат NIST. Их недостаток не в доказательности, а в том, что они плохо генерируют radical redesign concepts; именно здесь TRIZ может служить дополнительным слоем. ([IEC 60812:2018](https://webstore.iec.ch/en/publication/26359); [NUREG‑0492 Fault Tree Handbook, январь 1981](https://www.osti.gov/biblio/5762464); [NIST DOE Handbook, доступ 06.08.2026](https://www.itl.nist.gov/div898/handbook/pmd/section3/pmd31.htm)) citeturn796315view7turn150225search18turn796315view8

---

## B5. LLM, применяющие TRIZ

| Работа | Год | Масштаб | Что сработало | Что не доказано |
|---|---:|---:|---|---|
| **TRIZ‑GPT** | 2024 | 37 classical + 10 post-cutoff cases | Structured prompting и работа с mechanical cases | Production reliability, независимый benchmark |
| **AutoTRIZ** | 2024 | 10 textbook cases | В 7/10 случаях reference solution был full/half match в top‑3 | Авторы признают hallucinations и отсутствие objective effectiveness mechanism |
| **TRIZ Agents** | 2025 | Один gantry-crane case | Multi-agent decomposition и graph of TRIZ steps | 60–80 graph calls, 150k–250k tokens/run, nondeterminism, пропущенный breaker |
| **Ge et al.** | 2025 | N=32 | LLM-agent collaboration и TRIZ повысили отдельные dimensions | Нет общего superiority |
| **Čok et al.** | 2026 | N=114 | LLM снизил воспринимаемую сложность contradiction work | Общий quality effect слаб/неустойчив |
| **TRIZBENCH** | 2026 | 1,354 cases, 429 US patents | Масштабный benchmark contradiction/principle/grounding | Patent task ≠ production debugging |
| **TRIZ‑RAGNER** | 2026 | Patent-oriented retrieval/evaluation | `F1 84.2`, примерно `+7.3 pp` против prompt-only GPT | **[single-source]**, не ops |

AutoTRIZ использует несколько LLM-модулей для извлечения функций, противоречий и выбора принципов; на десяти учебных задачах reference-like решение попадало в top‑3 в семи случаях. Авторы отдельно признают hallucination risk и отсутствие объективного механизма оценки эффективности предложенного решения. ([AutoTRIZ, март 2024](https://arxiv.org/html/2403.13002v2)) citeturn796315view1

TRIZ‑GPT использовал curated set из классических и новых cases, но набор мал, преимущественно механический и не является blind production evaluation. ([TRIZ‑GPT, 12.08.2024](https://arxiv.org/html/2408.05897v1)) citeturn977197view3

TRIZ Agents продемонстрировал многоагентный workflow на одной инженерной задаче, но отдельный run требовал десятков graph calls и сотен тысяч токенов, оставался nondeterministic и не всегда извлекал критическое ограничение. Для двухчеловечной лаборатории это anti-AK architecture. ([TRIZ Agents, июнь 2025](https://arxiv.org/html/2506.18783v1)) citeturn796315view2turn365047view7turn365047view8

TRIZBENCH 2026 года содержит 1,354 cases, включая 429 американских патентов, и полезен для оценки распознавания contradictions и principles. Но успешное распознавание паттерна в патенте не показывает способности диагностировать distributed no-op или безопасно изменить production state. ([TRIZBENCH, ACL Findings 2026](https://aclanthology.org/2026.findings-acl.1798.pdf)) citeturn365047view9turn365047view10

### Что пригодно сегодня

**[emerging] Пригодно:**

- LLM заполняет короткий contradiction template;
- генерирует обе симметричные формулировки;
- предлагает четыре separation strategies;
- ищет возможные trimming targets;
- критикует решение по complexity ledger;
- возвращает максимум три концепта, каждый с falsification test.

**[speculative] Не доказано:**

- автономный TRIZ RCA-agent;
- автоматический выбор правильной root cause;
- безопасное production actuation;
- superior solution quality;
- reduction of recurrent incidents.

LLM здесь следует использовать как дешёвого facilitator’а, а не как носителя «TRIZ reasoning engine».

---

# C. Конфликт TRIZ с радикальной простотой

## C1. Где идеальность и AK‑47 расходятся

Обе доктрины хотят убрать лишнее, но оптимизируют разные функции.

### TRIZ-идеальность

> Максимизировать полезные функции и минимизировать costs/harms, потенциально до состояния «система отсутствует, а функция выполняется».

### AK‑47-идеальность

> Минимальный механизм, который понимает, диагностирует и восстанавливает неавтор — локально, без скрытых control planes и экспертной магии.

### Точки расхождения

| Ситуация | TRIZ-идеальный ход | Почему он может нарушить AK‑47 |
|---|---|---|
| Monitoring | Система сама лечит себя и сама подтверждает здоровье | Общий failure domain; ложный self-report |
| Browser profiles | Identity/session function распределена между machines и supersystem | Mutable hidden state и трудно воспроизводимый drift |
| Deployment | Event-driven autonomous convergence mesh | Больше actors, retries, states и split-brain cases |
| Agent scheduling | Dynamically reconfigurable multi-agent scheduler | Поведение зависит от history и модели, а не от читаемого cron |
| Managed services | Функция выполняется внешним supersystem, локальный компонент исчезает | Vendor opacity и отсутствие local repair |
| Configuration | Adaptive flags и runtime policy | Огромное число состояний и temporal coupling |
| Self-service | Компоненты сами обнаруживают, чинят и регистрируют себя | Повреждённый компонент одновременно judge и actuator |
| Feedback | Больше telemetry и automatic loops | Feedback loops взаимодействуют и маскируют исходный симптом |

### Ключевая формула для вашего случая

\[
\text{AK-Ideality}=
\frac{\text{verified useful effects}
+\text{legibility}
+\text{manual recoverability}}
{\text{components}
+\text{states}
+\text{dependencies}
+\text{coupling}
+\text{common-mode risk}
+\text{novice repair minutes}}
\]

**[established]** TRIZ не запрещает включить repairability и transparency в benefits/harms.  
**[established]** Но если команда их не внесла явно, стандартный IFR автоматически их не защищает.

Например, transition to supersystem формально удаляет локальный компонент. Но функция может просто переехать в opaque external service. Количество локальных файлов уменьшилось; реальная зависимость и recovery cost выросли. Именно поэтому «компонент исчез» не равно «система упростилась». ([MATRIZ Glossary, ©2023](https://wiki.matriz.org/docs/triz/glossary-6146/)) citeturn395312view1turn395312view2

---

## C2. Производит ли TRIZ over-engineered решения

### Прямое доказательство

Контролируемого исследования «TRIZ увеличивает архитектурную сложность или maintenance cost» я не нашёл: **[no source found]**.

Практические обзоры, однако, отмечают:

- сложность обучения;
- большое количество инструментов и терминов;
- трудность выбора правильного инструмента;
- необходимость domain adaptation;
- слишком общие решения при слишком абстрактной формулировке.

Это делает over-engineering правдоподобным failure mode, но не измеренным эффектом. citeturn842369search10turn661434view8turn433210view0

### Почему риск структурно существует

TRIZ-сессия вознаграждает:

- необычность;
- устранение противоречия без компромисса;
- использование доступных ресурсов;
- перенос функции;
- dynamization;
- feedback;
- self-service.

Она не обязана вознаграждать:

- простой runbook;
- очевидный control flow;
- ручной fallback;
- одинаковое поведение после перезапуска;
- отсутствие temporal state;
- возможность ремонта человеком, который не видел дизайн.

Если «non-obviousness» становится implicit KPI, facilitator будет предпочитать clever architecture простому разделению ownership.

### Связанная инженерная литература

**Gall’s law** — эвристика из книги Джона Галла 1975 года, а не экспериментальный закон: работающая сложная система обычно эволюционировала из работающей простой. Важная пропущенная часть популярной цитаты: простая система тоже может не работать. Первичный текст онлайн в надёжном издании найти не удалось; атрибуция опирается на вторичные источники: **[low-authority]**. ([John Gall bibliographic summary](https://en.wikipedia.org/wiki/John_Gall_%28author%29)) citeturn371579search0

**Worse is Better** Ричарда Гэбриела противопоставляет New Jersey style «The Right Thing»: implementation simplicity получает больший приоритет, completeness и consistency могут быть принесены в жертву. Сам Гэбриел представлял это как карикатуру и обсуждал прежде всего evolutionary survival и распространение, а не novice repairability. ([Gabriel, первоначальная версия 1989/распространённая 1991](https://dreamsongs.com/WorseIsBetter.html); [Princeton mirror](https://www.cs.princeton.edu/courses/archive/fall13/cos518/papers/worse-is-better.pdf)) citeturn371579search7turn371579search29

**Chesterton’s Fence** не говорит «никогда не удаляй компонент»; он требует сначала понять функцию, ради которой компонент появился. Это прямой safety check против неправильного trimming. Оригинальная формулировка относится к книге *The Thing* 1929 года; популярная короткая цитата является поздней парафразой. ([American Chesterton Society, 30.04.2012](https://www.chesterton.org/taking-a-fence-down/)) citeturn371579search22

Эмпирические software-работы поддерживают более умеренный тезис: структурная сложность и readability/understandability связаны, но существующие complexity metrics объясняют понятность лишь частично. Малое исследование 35 Java programs/23 constructs нашло отрицательную связь readability и complexity; более поздние исследования также подчёркивают несовершенство метрик. ([Alawad et al., 2019](https://arxiv.org/pdf/1909.01760); [Lavazza et al., 2023](https://link.springer.com/article/10.1007/s10664-023-10396-7)) citeturn371579search8turn371579search27

### Вывод C2

**[emerging]** Риск over-engineering реален по механизму, но не доказан как средний causal effect TRIZ.

Ваше средство защиты должно быть процедурным: **TRIZ генерирует варианты, AK‑47 отбирает.** Ни novelty, ни «полное устранение противоречия» не дают решению права на внедрение.

---

## C3. Явное разделение 40 принципов по software/ops-эффекту

Это **не каноническая TRIZ-классификация**, а моя default-классификация для небольшой simplicity-first operations lab: **[speculative]**. Один и тот же принцип способен сменить категорию в конкретном дизайне.

### По умолчанию удаляют механизм — 8

| № | Принцип | Software/ops-интерпретация |
|---:|---|---|
| 2 | Taking out | Изолировать или удалить вредный компонент |
| 5 | Merging | Объединить дублирующиеся механизмы |
| 6 | Universality | Один простой компонент выполняет несколько функций |
| 13 | The other way round | Инвертировать push/pull, poll/emit, control/data |
| 16 | Partial or excessive action | Сделать безопасный partial fix вместо полной автоматики |
| 21 | Skipping | Убрать необязательную промежуточную фазу |
| 27 | Cheap short-living objects | Одноразовый process/file/worktree вместо долгоживущего stateful service |
| 34 | Discarding and recovering | Удалять временное состояние и восстанавливать из canonical source |

**Ограничение:** merging и universality становятся противоположностью простоты, если создают god-process или shared failure domain.

### По умолчанию добавляют механизм, state или coupling — 22

| № | Принцип | Типичный software-риск |
|---:|---|---|
| 3 | Local quality | Special cases и heterogeneous behavior |
| 4 | Asymmetry | Несимметричные роли и recovery paths |
| 7 | Nested doll | Layers, wrappers, recursive ownership |
| 8 | Anti-weight | Компенсирующий control loop |
| 9 | Preliminary anti-action | Предварительные защитные состояния |
| 10 | Preliminary action | Precomputation, staging, caches |
| 11 | Beforehand cushioning | Buffers, retries, reserves |
| 15 | Dynamization | Runtime adaptability и state explosion |
| 17 | Another dimension | Новый control/data plane или abstraction layer |
| 19 | Periodic action | Cron, polling, heartbeat |
| 20 | Continuity of useful action | Always-on worker/service |
| 22 | Blessing in disguise | Error-recovery transformation logic |
| 23 | Feedback | Sensors, controllers, feedback loops |
| 24 | Intermediary | Broker, queue, proxy, adapter |
| 25 | Self-service | Self-registration, self-healing, self-update |
| 26 | Copying | Replicas, caches, shadows |
| 30 | Flexible shells/thin films | Dynamic boundaries, sandbox/proxy layers |
| 31 | Porous materials | Partial permeability, filter policies |
| 32 | Color changes | Additional status/metadata channels |
| 35 | Parameter changes | Configurability и combinatorial states |
| 36 | Phase transitions | Mode/state-machine transitions |
| 40 | Composite materials | Hybrid stack из нескольких mechanisms |

### Нейтральны или слабо переносятся — 10

| № | Принцип | Комментарий |
|---:|---|---|
| 1 | Segmentation | Может изолировать failures или породить microservice sprawl |
| 12 | Equipotentiality | Иногда означает убрать privilege/transition cost |
| 14 | Spheroidality/curvature | Слишком метафоричен для большинства ops-задач |
| 18 | Mechanical vibration | Обычно свободная аналогия на polling/oscillation |
| 28 | Mechanics substitution | Может означать заменить механизм другим классом |
| 29 | Pneumatics/hydraulics | Для software почти исключительно метафора |
| 33 | Homogeneity | Может упростить stack либо увеличить common-mode risk |
| 37 | Thermal expansion | Слабый перенос |
| 38 | Strong oxidants | Слабый перенос |
| 39 | Inert atmosphere | Иногда sandbox/isolation, но mapping произволен |

Все 40 названий и их классические описания приведены в справочнике MATRIZ; разделение выше является именно policy overlay для software/ops. ([MATRIZ: 40 Inventive Principles, ©2023](https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/contradictions/inventive-principles-6023/)) citeturn834351view3

### Политика simplicity-first shop

#### Принимать по умолчанию

`2, 13, 16, 21, 27, 34`

То есть:

- remove;
- invert;
- partial safe action;
- skip;
- disposable object;
- discard/rebuild.

#### Принимать условно

- `1` segmentation — только если failure domains действительно становятся независимыми;
- `5` merging — только если уменьшается число интерфейсов и не возникает common mode;
- `6` universality — только если компонент остаётся понятным и stateless;
- `23` feedback — только внешний, простой и основанный на effect;
- `9/10/11` prior protection — только если действие статично, idempotent и имеет expiry.

#### Отвергать как первый ход

`7, 15, 17, 20, 24, 25, 36, 40`

То есть:

- nesting;
- dynamization;
- another dimension;
- always-on continuity;
- intermediary;
- self-service;
- phase/state transitions;
- composite mechanisms.

«Отвергать» означает не абсолютный запрет, а требование отдельной **COMPLICATION-записи** и доказательства, что simpler alternative не выполняет функцию.

---

## C4. Trimming и increasing ideality: действительно ли это внутренний AK‑47

**Частично да.**

TRIZ trimming удаляет компонент, если:

- его полезная функция больше не нужна;
- функцию может выполнить другой компонент;
- функцию может выполнить объект функции;
- функцию можно передать supersystem или доступному ресурсу.

Это близко к AK‑47, поскольку явно ставит вопрос: «почему этот компонент вообще существует?» ([MATRIZ Glossary, ©2023](https://wiki.matriz.org/docs/triz/glossary-6146/)) citeturn395312view2

Но есть принципиальное расхождение:

> TRIZ считает trimming успешным, если функция сохранена.  
> AK‑47 требует также сохранить понятность ownership, failure isolation и возможность ручного восстановления.

### Как trimming делает систему более хрупкой

1. **Удаляет redundancy**, считая её дублированием.
2. **Удаляет independent monitor**, передавая health check самой системе.
3. **Переносит функцию в supersystem**, создавая скрытую внешнюю зависимость.
4. **Объединяет control и actuation**, уменьшая component count, но создавая common-mode failure.
5. **Удаляет manual fallback**, поскольку automation «уже выполняет функцию».
6. **Удаляет explicit state**, заменяя его выводом из нескольких косвенных сигналов.
7. **Удаляет локальный canonical source**, полагаясь на сеть или managed platform.

Контролируемой работы, измеряющей рост fragility после TRIZ trimming, я не нашёл: **[no source found]**. Это системная inference, а не опубликованный средний эффект.

### Правильное правило

Перед trimming компонента перечислить не только его nominal function, но и:

- observability;
- isolation;
- recovery;
- audit;
- rate limiting;
- manual override;
- institutional memory.

Это практическое соединение Function Analysis с Chesterton’s Fence.

---

## C5. Цена принятия и допустима ли частичная TRIZ

### Реальная learning cost

TRIZ имеет:

- собственный словарь;
- несколько типов contradictions;
- 39 parameters;
- 40 principles;
- separation;
- resources;
- function models;
- Su‑Field;
- 76 standards;
- trends;
- ARIZ;
- несколько несовместимых модернизаций.

Коммерческий basic GEN‑TRIZ course заявляет примерно 40 часов, а MATRIZ строит пятиуровневую credential ladder. Это не доказывает, что каждому нужно 40 часов, но показывает масштаб curriculum. ([GEN TRIZ Basic Course, доступ 06.08.2026](https://www.gen-triz.com/basic-triz-course); [MATRIZ Certification](https://matriz.org/certification/)) citeturn963676search26turn255008view3

Моя оценка:

- contradiction + separation + IFR + trimming: **6–10 часов**;
- уверенная работа с matrix и principles: **20–40 часов**;
- Function Analysis, CECA, trends, Su‑Field: **40–80 часов**;
- ARIZ и facilitator-level competence: **100+ часов практики**.

Это **[speculative] planning estimate**, не published benchmark.

### Можно ли принять только часть

**Да, концептуально можно.**

Аргументы:

- сравнительные исследования часто тестируют отдельные инструменты, а не полный ARIZ;
- USIT и SIT сами являются намеренными упрощениями;
- даже классические curricula используют инструменты модульно;
- для вашего use case матрица и Su‑Field не являются необходимыми для contradiction/separation.

Но доказательство того, что именно минимальный набор снижает production incidents, отсутствует: **[no source found]**.

### Является ли это «TRIZ-lite, который ничего не делает»

Нет подтверждённого общего результата ни в одну сторону: **[no source found]**.

Корректнее говорить:

> Minimal TRIZ — дешёвый framing и ideation checklist.  
> Он не наследует автоматически claims полной TRIZ, но и не требует веры в них.

Для вас это преимущество: можно проверить конкретный ritual, а не покупать мировоззрение.

---

# D. Что делать практически

## D1. Четыре именованных варианта

| Вариант | Цена обучения | Изменение ежедневной работы | 30-дневная проверка | Когда отказаться |
|---|---:|---|---|---|
| **0. SRE Evidence Loop — без TRIZ** | 4–6 ч | End-to-end effect checks, hypothesis log, короткий fault tree, external watchdog | False-green/100 runs, p50/p90 detection lag, recurrence, repair time | Не отказываться; это baseline |
| **1. AK‑TRIZ‑4** | 6–10 ч | После доказанной причины — 15–25 мин на contradiction, separation, IFR и trimming | Crossover на 6+ failures; recurrence, accepted non-obvious moves, complexity delta | Нет ≥20% улучшения recurrence; 0 полезных новых moves; session >30 мин |
| **2. CAST‑Lite + AK‑TRIZ‑4** | 16–24 ч | Для крупных incidents рисуется control/feedback map; TRIZ используется для redesign | Снижение control/feedback failures; повторное использование maps | Большинство инцидентов локальны; maps не влияют на решения |
| **3. Full TRIZ / MATRIZ track** | 40–120+ ч | Matrix, principles, Function Analysis, Su‑Field, trends, ARIZ, обучение facilitator | Нужны десятки genuinely inventive product problems | Почти наверняка сразу: ops-задачи не окупят curriculum |

### Вариант 0 — SRE Evidence Loop

Подходит, если ваши основные дефекты:

- отсутствует проверка полезного эффекта;
- процесс возвращает success раньше применения;
- delivery receipt принимается за application receipt;
- нет владельца mutable state;
- нет independent failure domain;
- runbook не воспроизводит ремонт.

Это мой baseline независимо от решения по TRIZ.

### Вариант 1 — AK‑TRIZ‑4

**Рекомендованный пилот.**

Применять только к проблеме, где:

1. loss воспроизводим;
2. causal mechanism локализован;
3. очевидный fix создаёт конкретный новый harm;
4. есть минимум два действительных требования, а не просто «хотелось бы всё сразу».

### Вариант 2 — CAST‑Lite + AK‑TRIZ‑4

Использовать для:

- нескольких machines/control planes;
- authentication authority;
- browser owner vs scheduler;
- hub/VPS/fleet coordination;
- incidents, где каждый компонент сработал «правильно», но system outcome ошибочен.

### Вариант 3 — Full TRIZ

Не рекомендую. Он имеет смысл, если лаборатория начинает регулярно разрабатывать новые физические продукты, patentable mechanisms или сложные R&D architectures и появляется trained facilitator. Для текущего fleet ops это отрицательный ROI.

---

## Метрики пилота

Не используйте «команда почувствовала, что стала мыслить системнее».

Собирать:

```text
incident_class
method = evidence_only | evidence_plus_ak_triz
minutes_to_repro
minutes_to_verified_cause
minutes_to_deployed_fix
recurred_14d
recurred_30d
false_green_after_fix
new_components
new_persistent_states
new_external_dependencies
removed_components
rollback_tested
novice_repair_minutes
novice_repair_success
non_obvious_move_accepted
```

### Success criteria через 30 дней

AK‑TRIZ‑4 остаётся, если выполнено хотя бы одно:

- ≥20% снижение повторений в сопоставимых failure classes;
- минимум два принятых неочевидных решения на шесть разборов;
- уменьшение added mechanisms относительно baseline fixes;
- заметное улучшение novice repair success;
- contradiction ritual регулярно предотвращает fix, который создал бы common-mode failure.

### Abandon criteria

Удалить ritual, если:

- средняя сессия превышает 30 минут;
- формулировки постоянно сводятся к очевидным фразам;
- ни одного решения не изменено;
- число добавленных persistent states выше, чем при baseline;
- команда начинает применять TRIZ до воспроизведения причины;
- LLM генерирует длинные списки принципов вместо falsifiable designs.

---

## D2. Точный минимальный ritual — одна страница

# AK‑TRIZ‑4 Incident Card

### 1. Факт

**Какой полезный эффект должен был существовать, но его нет?**

Не: «скрипт упал».  
Да: «на node‑3 версия правила осталась `v17`, хотя artifact `v18` доставлен».

### 2. Доказательство

Записать:

- expected;
- actual;
- timestamp;
- reproduction;
- artifact/log, который нельзя объяснить только status flag’ом.

Без этого остановиться. TRIZ пока запрещён.

### 3. Причинная цепочка

Не более пяти звеньев:

```text
effect absent
← apply не запускался
← delivery receipt был принят за application receipt
← transport и actuation используют один status
```

Выбрать ближайшую управляемую причину.

### 4. Два технических противоречия

```text
IF мы делаем X,
THEN получаем полезное A,
BUT получаем вред B.

IF мы не делаем X / делаем обратное,
THEN устраняем B,
BUT теряем A.
```

Если обратная формула бессмысленна, вероятно, реального противоречия нет.

### 5. Физическое противоречие

```text
Один объект/параметр должен быть P,
потому что ______,
и не-P,
потому что ______.
```

### 6. Разделение

Проверить строго по порядку:

1. в разное **время**?
2. в разных **местах/failure domains**?
3. при разных **условиях**?
4. между **целым и частями**?

### 7. IFR с AK-ограничением

```text
Полезный эффект происходит и проверяется,
вред отсутствует,
используется уже существующий ресурс,
не добавляется новый always-on service,
новый shared state или обязательный cloud dependency.
```

### 8. Trimming

Что можно удалить:

- component;
- status flag;
- queue;
- database;
- sync direction;
- retry loop;
- abstraction?

Для каждой функции удаляемого компонента указать нового владельца. Неизвестная функция запрещает trimming.

### 9. COMPLICATION gate

Для каждого добавления:

```text
Что добавляется?
Какую измеримую боль лечит?
Что сломается без него?
Какова ручная дешёвая альтернатива?
Какие новые states/dependencies/failure modes?
Кто починит это без автора?
Как rollback выполняется одной командой?
```

### 10. Pilot

- одна машина;
- не более двух часов реализации;
- один falsifiable outcome;
- rollback до rollout;
- promotion только после 3 relevant runs или 7 дней.

---

### Демонстрация: «зелёный indicator, dead pipeline»

**Факт:** scheduler помечает job успешным, но target article не обновлён.

**Цепочка:**

```text
target stale
← publisher не применил output
← upstream завершился exit 0
← status writer считает exit 0 конечным success
```

**Техническое противоречие A:**

> IF success определяется exit code, THEN механизм прост, BUT green не доказывает effect.

**Обратное:**

> IF success требует проверки target, THEN indicator отражает effect, BUT добавляется проверка и потенциальный failure mode.

**Физическое противоречие:**

> indicator должен быть тихим, потому что штатных запусков много, и активным, потому что его собственная смерть должна быть видна.

**Разделение:**

- machine evidence постоянно;
- human alert только при expiry/mismatch;
- target verification вне publisher process.

**IFR:**

> Появление целевого artifact само является подтверждением success; отсутствие свежего artifact автоматически становится exception без отдельного зелёного notifier.

**Trimming:**

Удалить общий `green=true`. Не добавлять dashboard и database.

**Минимальная реализация:**

```json
{
  "job_id": "publish-2026-08-06T09:00Z",
  "input_hash": "…",
  "target_version": "…",
  "completed_at": "…",
  "expires_at": "…"
}
```

Внешний VPS сравнивает `target_version` с реальным target. Он не доверяет publisher status.

**COMPLICATION:** receipt schema + один checker.  
**Rollback:** удалить checker, вернуться к ручной проверке.  
**Метрика:** false-green events на 100 запусков и detection lag.

**Что здесь сделал TRIZ:** не изобрёл dead-man lease, а заставил сформулировать success как verified effect и разделить machine evidence от human alert.

---

## D3. Когда TRIZ будет неправильным выбором

TRIZ не следует применять, если:

1. **Нет наблюдаемости.** Вы не знаете, какой effect отсутствует и где расходятся expected/actual.
2. **Нет воспроизводимости.** Вы обсуждаете архитектуру по одному непроверяемому эпизоду.
3. **Причина — ownership.** Две машины пишут один mutable profile; никакое inventive principle не заменит одного владельца.
4. **Причина — недоставленная работа.** Artifact дошёл, но никто не обязан применить его.
5. **Причина — unsupported platform behavior.** Нельзя «разрешить противоречие» с security policy сервиса инженерной хитростью.
6. **У вас нет числа повторов.** Невозможно отличить эффект метода от случая.
7. **Очевидный SRE pattern уже решает задачу.** Нет смысла превращать external checker в ARIZ exercise.
8. **Команда любит красивые модели.** TRIZ создаст ещё один слой ritual cargo cult.
9. **Никто не ведёт complexity ledger.** IFR начнёт оптимизировать automation, забывая repairability.
10. **Идеи не являются bottleneck.** Если проблема в execution discipline, TRIZ увеличит backlog концептов.

### Сильнейший аргумент против рекомендации

Ваши четыре класса отказов уже имеют прямые решения:

- watchdog → independent failure domain;
- browser → single mutable owner + human reauth boundary;
- rollout → immutable pull package + local apply receipt;
- silent indicator → lease + external effect check.

Поэтому возможно, что **100% пользы даст Evidence Loop, а marginal value TRIZ будет нулевой**. Более того, перевод задачи на язык «технических» и «физических» противоречий может замедлить владельца-неинженера и создать ложное ощущение глубины.

Именно поэтому рекомендация — не adoption, а короткий falsifiable pilot.

---

# Итоговая рекомендация

## **Принять: `Evidence Loop + AK‑TRIZ‑4`, на 30 дней**

**[emerging]**

### Входит

- evidence gate;
- техническое противоречие в обе стороны;
- физическое противоречие;
- separation: time / space / condition / whole-parts;
- IFR с запретом на необоснованный always-on mechanism;
- trimming;
- COMPLICATION ledger;
- одноузловой pilot и измеримый rollback.

### Не входит

- 39×39 matrix как default;
- заучивание всех 40 principles;
- trends как «законы»;
- Su‑Field;
- 76 standards;
- full ARIZ;
- MATRIZ certification;
- TRIZ multi-agent orchestrator.

### Главная предпосылка

Повторяемость хотя бы части ваших инцидентов вызвана настоящими architectural contradictions после локализации причины.

### Если предпосылка ложна

Если основная проблема — отсутствие end-to-end receipts, mutable-state ownership и rollout discipline, выбирайте **Option 0: SRE Evidence Loop без TRIZ**.

---

# Реестр источников

## TRIZ: определения и официальные материалы

1. MATRIZ. **Engineering Contradiction**, ©2023.  
   https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/contradictions/engineering-contradiction-5995/

2. MATRIZ. **Contradiction Matrix**, ©2023.  
   https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/contradictions/engineering-contradiction-5995/contradiction-matrix-6026/

3. MATRIZ. **Physical Contradiction**, ©2023.  
   https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/contradictions/physical-contradiction-6056/

4. MATRIZ. **Inventive Principles**, ©2023.  
   https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/contradictions/inventive-principles-6023/

5. MATRIZ. **TRIZ Glossary**, ©2023.  
   https://wiki.matriz.org/docs/triz/glossary-6146/

6. MATRIZ. **Standard Inventive Solutions**, ©2023.  
   https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/substance-field-modeling/standard-inventive-solutions/

7. MATRIZ. **ARIZ Template**, ©2023.  
   https://wiki.matriz.org/docs/triz-tools/ariz-template/

8. Oxford Creativity. **TRIZ Glossary**, без даты; доступ 06.08.2026. **[low-authority: consultancy]**  
   https://www.triz.co.uk/glossary

9. MATRIZ. **Certification**, без даты; доступ 06.08.2026.  
   https://matriz.org/certification/

## Производные школы и стандартизация

10. Ideation International. **Company / I‑TRIZ**, без даты; доступ 06.08.2026.  
    https://ideation-triz.com/en/company/

11. GEN TRIZ. **Basic TRIZ Course**, без даты; доступ 06.08.2026.  
    https://www.gen-triz.com/basic-triz-course

12. SIT. **About Systematic Inventive Thinking**, 2019.  
    https://www.sitsite.com/wp-content/uploads/2019/02/About-SIT-Innovation-V5-Yoni.pdf

13. Ed Sickafus. **USIT Overview**, 14.02.2003.  
    https://www.osaka-gu.ac.jp/php/nakagawa/TRIZ/eTRIZ/eSickafusMemorial/eSickafus-TextBooks-Tutorials/USITOverView-030214.pdf

14. ISO. **ISO 11929‑2:2019**, опубликован 02.2019; заменён ISO 11929‑2:2025.  
    https://www.iso.org/standard/69580.html

15. ISO. **ISO 56007:2023**, опубликован 08.2023.  
    https://www.iso.org/standard/75068.html

16. ISO/TC 279. **Innovation Management Catalogue**, доступ 06.08.2026.  
    https://www.iso.org/committee/4587737/x/catalogue/p/1/u/1/w/0/d/0

## Evidence, reviews и критика

17. Hernandez et al. **Systematic Ideation Effectiveness Study of TRIZ**, октябрь 2013.  
    https://asmedigitalcollection.asme.org/mechanicaldesign/article/135/10/101009/380571/Systematic-Ideation-Effectiveness-Study-of-TRIZ

18. Birdi, Leach & Magadley. **Evaluating the impact of TRIZ creativity training**, сентябрь 2012.  
    https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1467-9310.2012.00686.x

19. René Dathe. **Effects of TRIZ use in problem solving**, submitted май 2015; repository 2017.  
    https://eprints.glos.ac.uk/4884/1/DBA-Thesis_Rene%20Dathe_final%20print.pdf

20. Ge et al. **Comparing TRIZ and brainstorming in human–agent design collaboration**, опубликовано online 2025.  
    https://doi.org/10.1017/S7821986623255907

21. Čok et al. **Evaluating TRIZ with and without LLM support**, DESIGN 2026.  
    https://doi.org/10.1017/pds.2026.10409

22. Ilevbare, Probert & Phaal. **A review of TRIZ, and its benefits and challenges in practice**, 2013.  
    https://www.sciencedirect.com/science/article/abs/pii/S4828306389756549

23. Chechurin & Borgianni. **Understanding TRIZ through the review of top cited publications**, 2016.  
    https://www.sciencedirect.com/science/article/abs/pii/S7045764260520573

24. Spreafico & Russo. **TRIZ Industrial Case Studies: A Critical Survey**, 2016.  
    https://www.researchgate.net/publication/541379219_TRIZ_Industrial_Case_Studies_A_Critical_Survey

25. Mohammadi & Yang. **Barriers and enablers of TRIZ**, online 2022; journal issue 2024.  
    https://www.emerald.com/jedt/article/22/4/1206/1230792/Barriers-and-enablers-of-TRIZ-a-literature

26. Borgianni et al. **Process for individuating TRIZ inventive principles**, 2021.  
    https://www.cambridge.org/core/journals/design-science/article/process-for-individuating-triz-inventive-principles-deterministic-stochastic-or-domainoriented/81CEAB2A6483D6A7D42010A22F5B9839

## Software и operations applications

27. Govindarajan, Sheu & Mann. **Review of Systematic Software Innovation Using TRIZ**, 2019.  
    https://ojs.ijosi.org/index.php/IJOSI/article/download/175/374/2388

28. Navneet Bhushan. **Ideality, TRIZ and Software Design — A Case Study**, 2008. **[single-source]**  
    https://www.aitriz.org/articles/TRIZFeatures/30383035-4268757368616e.pdf

29. Lazić et al. **Software Quality Optimization through TRIZ and Taguchi Methods**, май 2018. **[single-source]**  
    https://ijiet.com/wp-content/uploads/2018/06/22.pdf

30. **TRIZ application to recurring Scrum problems**, AIP Conference Proceedings, 2023.  
    https://pubs.aip.org/aip/acp/article-pdf/doi/10.1063/5.0115577/16772985/020029_1_online.pdf

31. **Modeling Software in TRIZ**, 2022/2023.  
    https://www.researchgate.net/publication/510992412_Modeling_Software_in_TRIZ

32. **TRIZ-Based Cause and Effect Chains Analysis vs Root Cause Analysis**, 2015.  
    https://www.researchgate.net/publication/144945393_TRIZ-Based_Cause_and_Effect_Chains_Analysis_vs_Root_Cause_Analysis

## SRE и альтернативные методы

33. Google. **Effective Troubleshooting**, *Site Reliability Engineering*, 2016.  
    https://sre.google/sre-book/effective-troubleshooting/

34. Google SRE. **AI Engineering Reliable Operations**, без указанной даты; доступ 06.08.2026.  
    https://sre.google/resources/practices-and-processes/ai-engineering-reliable-operations/

35. MIT. **STPA Handbook**, март 2018.  
    https://psas.scripts.mit.edu$HOME?name=STPA_handbook.pdf

36. MIT. **CAST Handbook**, 2024.  
    https://psas.scripts.mit.edu$HOME?name=CAST_Handbook_Healthcare.pdf

37. IEC. **IEC 60812:2018 — FMEA/FMECA**, август 2018.  
    https://webstore.iec.ch/en/publication/26359

38. U.S. Nuclear Regulatory Commission. **Fault Tree Handbook, NUREG‑0492**, январь 1981.  
    https://www.osti.gov/biblio/5762464

39. NIST. **Engineering Statistics Handbook: Process Improvement / DOE**, доступ 06.08.2026.  
    https://www.itl.nist.gov/div898/handbook/pmd/section3/pmd31.htm

40. Snowden & Boone. **A Leader’s Framework for Decision Making**, Harvard Business Review, ноябрь 2007.  
    https://hbr.org/2007/11/a-leaders-framework-for-decision-making

41. Theory of Constraints Institute. **Theory of Constraints**, без даты; доступ 06.08.2026.  
    https://www.tocinstitute.org/theory-of-constraints.html

42. Simon Wardley. **Wardley Mapping**, online book; доступ 06.08.2026.  
    https://learnwardleymapping.com/book/

## LLM + TRIZ

43. **TRIZ‑GPT**, submitted 12.08.2024.  
    https://arxiv.org/html/2408.05897v1

44. **AutoTRIZ**, март 2024, revision 2024.  
    https://arxiv.org/html/2403.13002v2

45. **TRIZ Agents**, июнь 2025.  
    https://arxiv.org/html/2506.18783v1

46. **TRIZBENCH**, ACL Findings 2026.  
    https://aclanthology.org/2026.findings-acl.1798.pdf

47. **TRIZ‑RAGNER**, февраль 2026. **[single-source]**  
    https://arxiv.org/abs/2602.23656

## Простота, понятность и эволюция систем

48. Richard P. Gabriel. **Worse Is Better**, первоначальный текст 1989; распространённая редакция 1991.  
    https://dreamsongs.com/WorseIsBetter.html

49. Richard P. Gabriel. **Lisp: Good News, Bad News, How to Win Big**, mirror.  
    https://www.cs.princeton.edu/courses/archive/fall13/cos518/papers/worse-is-better.pdf

50. American Chesterton Society. **Taking a Fence Down**, 30.04.2012; обсуждает оригинальный текст 1929 года.  
    https://www.chesterton.org/taking-a-fence-down/

51. Alawad et al. **An Empirical Study of the Relationships between Code Readability and Software Complexity**, 2019.  
    https://arxiv.org/pdf/1909.01760

52. Lavazza et al. **An empirical study on software understandability and its relationship with structural characteristics**, 2023.  
    https://link.springer.com/article/10.1007/s10664-023-10396-7
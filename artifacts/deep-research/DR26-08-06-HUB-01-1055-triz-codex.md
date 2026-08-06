[06.08.2026, 15:22 Лондон]

# DR26-08-06-HUB-01-1055: TRIZ против AK-47

## Исполнительный вердикт

**[established] TRIZ - реальная инженерная эвристическая традиция, но не экспериментально подтверждённая «точная наука изобретательства».** Несколько инструментов полезны для явной постановки противоречий. Доказательств, что полный TRIZ снижает MTTR, число инцидентов или повторные отказы в software/SRE/agentic infrastructure, не найдено.

**Рекомендация [emerging]: не «внедрять TRIZ», а на 30 дней позаимствовать четыре эвристики:**

1. техническое и физическое противоречие;
2. разделение во времени, пространстве, по условию и между целым/частями;
3. IFR, Ideal Final Result;
4. trimming с жёстким AK-47-гейтом.

Применять их только после воспроизведения сбоя и доказательства причинного механизма. Матрицу 39×39, 40 принципов как обязательный справочник, Su-Field, 76 стандартов, ARIZ и сертификацию не брать.

Сильнейшее возражение: этот минимум может оказаться обычным инженерным здравым смыслом с советским брендингом. Контролируемых данных, что связка «противоречие + IFR + trimming» превосходит hypothesis-driven debugging, нет: **[no source found]**.

---

# A. Что такое TRIZ без мифологии

## A1. Основные инструменты

Определения ниже сведены из корпуса MATRIZ и академического обзора Ilevbare, Probert и Phaal. ([MATRIZ Body of Knowledge, без даты, доступ 06.08.2026](https://wiki.matriz.org/docs/triz/); [Ilevbare et al., опубликовано online 10.01.2013](https://doi.org/10.1016/j.technovation.2012.11.003)).

| Инструмент | Форма проблемы | Вход | Выход | Ценность для software/ops |
|---|---|---|---|---|
| Техническое противоречие | Улучшение X ухудшает Y | `Если сделаем X, улучшим A, но ухудшим B`, плюс обратная формула | Явный trade-off; иногда два параметра матрицы и 1-4 принципа | Высокая как формулировка, низкая как автоматический решатель |
| Матрица 39×39 | Противоречие удалось свести к двум универсальным параметрам | Улучшаемый и ухудшаемый параметры | Рекомендуемые inventive principles | В software выбор параметров субъективен |
| Физическое противоречие | Один объект должен быть P и не-P | Объект, свойство, два обоснованных требования | Задача на разделение | Высокая |
| Separation principles | P и не-P не обязаны существовать одновременно и одинаково | Физическое противоречие | Разделение во времени, пространстве, по условию или по системному уровню | Высокая, близка к failure-domain и control/data-plane separation |
| IFR | Команда обсуждает механизм, забыв результат | Функция, вред, ресурсы и ограничения | Целевое состояние: функция выполняется почти сама, вред исчезает | Полезен только с AK-47-ограничениями |
| Ideality | Нужно сравнить полезность и цену решений | Useful functions, costs, harms | Направление роста `Σbenefit / (Σcost + Σharm)` | Не является калиброванной метрикой |
| 40 принципов | Команда застряла на первом решении | Противоречие или описание проблемы | Аналогические подсказки | Часть переносится; полный список создаёт шум |
| Trends of evolution | Нужны возможные направления будущего развития | История системы, ценностный параметр, окружение | S-curve, ideality, supersystem, trimming, dynamization, controllability | Использовать как вопросы, а не «законы будущего» |
| Su-Field | Нужное взаимодействие отсутствует, слабо или вредно | S1, S2 и поле/interaction F | Трансформация связи | Для software часто произвольная метафора |
| 76 standards | Su-Field уже построен | Класс неполного или вредного взаимодействия | Один из типовых способов преобразования | Низкая отдача для вашего стека |
| ARIZ-85C | Сложное противоречие не решается простыми средствами | Problem model, conflicting pair, operating zone/time, resources | Уточнённое физическое противоречие и концепт решения | Слишком дорог для повседневного ops |
| Function Analysis | Непонятно, кто что делает и создаёт вред | Компоненты, взаимодействия, качество функций | Function map | Полезна в компактной форме |
| Trimming | Система обросла компонентами | Function model и компонент-кандидат | Удаление компонента с сохранением нужной функции | Ближе всего к AK-47, но опасно для redundancy и observability |
| CECA | Симптом имеет ветвящиеся причины | Disadvantage и causal hypotheses | Cause-effect chain | Полезна, но фактически конкурирует с causal graph/FTA |
| Nine Screens | Анализ застрял на одном узле и моменте | Текущая система | `subsystem/system/supersystem × past/present/future` | Хороший framing для drift и rollout |
| Size-Time-Cost | Скрытые допущения о масштабе блокируют идеи | Текущий механизм | Сценарии при почти нулевом/огромном размере, времени или бюджете | Мысленный provocateur, не валидация |
| Smart Little People | Трудно представить локальную динамику частей | Процесс заменяется воображаемыми маленькими исполнителями | Наглядная микро-модель | В multi-agent systems метафору легко принять за архитектуру |

Техническое противоречие, физическое противоречие и IFR описаны в официальной базе MATRIZ. ([MATRIZ Contradictions, без даты, доступ 06.08.2026](https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/contradictions/); [MATRIZ IFR, без даты, доступ 06.08.2026](https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/ariz-5892/ideal-final-result-5922/)).

**[established] Ideality не имеет установленного метода расчёта benefit и payment factors.** Поэтому скрытая связность, когнитивная нагрузка и стоимость аварийного восстановления легко выпадают из знаменателя. Это не наша критика, а оговорка самой базы MATRIZ. ([MATRIZ Ideal System, без даты, доступ 06.08.2026](https://wiki.matriz.org/docs/triz/problem-solving-tools-5807/function-cost-analysis-7189/function-analysis-5611/valu-analysis-6483/value-5943/ideal-system-5939/)) [single-source].

**[emerging] Su-Field уже не выглядит обязательным даже внутри современной TRIZ.** База MATRIZ пишет, что после появления CECA самостоятельный Su-Field analysis вышел из обычного употребления. ([MATRIZ Su-Field Analysis, без даты, доступ 06.08.2026](https://wiki.matriz.org/knowledge-base/triz/problem-solving-tools-5890/substance-field-modeling/standard-inventive-solutions/substance-field-analysis/)) [single-source].

**ARIZ вне механической инженерии:** отдельные публикации существуют, но статистики, сколько команд действительно проходят все части ARIZ до конца, не найдено: **[no source found]**. Современные обзоры сами характеризуют такие применения как редкие. ([ARIZ review, 2023](https://www.mdpi.com/2071-1050/15/9/7271); [ARIZ overview, 2026](https://www.mdpi.com/2305-7084/10/4/54)).

## A2. Classical TRIZ и производные

| Школа | Что это | Ownership | Что является маркетингом |
|---|---|---|---|
| Classical TRIZ | Историческое семейство Altshuller-era методов | Нет одного владельца всей традиции; защищены конкретные тексты и курсы | «Единая экспериментально доказанная наука» |
| I-TRIZ | Commercial integration: problem formulation, failure prediction, directed evolution | Ideation International | Vendor success cases не равны независимой проверке |
| TRIZ+ / TRIZ++ | Неоднозначное название современных расширений | Несколько школ, включая GEN3/GEN TRIZ lineage | Впечатление единого стандартизированного продолжения |
| GEN-TRIZ | Proprietary curriculum, MPV, function-oriented search, expert network | GEN TRIZ LLC | Результативность заявляет продавец |
| SIT | Пять шаблонов: subtraction, multiplication, division, task unification, attribute dependency | Systematic Inventive Thinking, фирма и trademark | Простота бренда не доказывает superiority |
| USIT | Упрощённый процесс definition → analysis → solution generation | Ed Sickafus/Ford lineage; единого certification-owner не найдено | Не доказано, что упрощение сохраняет всю силу полной TRIZ |
| MATRIZ | Ассоциация, glossary, curriculum, пятиуровневая сертификация | MATRIZ | «Globally recognized» является self-description |
| Altshuller Institute | Body of Knowledge и собственная сертификация | Altshuller Institute | Это также не ISO-валидация эффективности |

История I-TRIZ, SIT и USIT подтверждается материалами самих владельцев и историческими учебными документами. Это неизбежно vendor-heavy evidence. ([I-TRIZ Overview, copyright 2001-2012](https://www.whereinnovationbegins.net/build/wp-content/uploads/2017/01/I-TRIZOverview.pdf) [single-source][low-authority]; [SIT history, без даты](https://www.sitsite.com/about-us-systematic-inventive-thinking/) [single-source][low-authority]; [Sickafus, USIT Overview, 14.02.2003](https://www.osaka-gu.ac.jp/php/nakagawa/TRIZ/eTRIZ/eSickafusMemorial/eSickafus-TextBooks-Tutorials/USITOverView-030214.pdf) [single-source]).

### Ошибка `ISO 69580:2025`

**[established] Это не стандарт TRIZ.** URL ISO с идентификатором `69580` ведёт к **ISO 11929-2:2019**, стандарту по измерениям ионизирующего излучения, заменённому версией 2025 года. Число 69580 является внутренним ID страницы, не номером TRIZ-стандарта. ([ISO, опубликовано 02.2019](https://www.iso.org/standard/69580.html)) [single-source].

Релевантны ISO 56000/56001 по innovation management и ISO/TR 23847:2022, связывающий biomimetics и TRIZ, но ни один из них не превращает MATRIZ certification в ISO certification. ([ISO 56000:2025, 23.01.2025](https://www.iso.org/standard/84436.html); [ISO 56001:2024, 10.09.2024](https://www.iso.org/standard/79278.html); [ISO/TR 23847:2022, 14.07.2022](https://www.iso.org/standard/77148.html)).

## A3. Evidence base

### Лучшие сравнительные исследования

| Исследование | N и дизайн | Результат | Слабости |
|---|---|---|---|
| Chulvi et al., 2013 | 16 команд: 7 TRIZ, 7 SCAMPER, 1 brainstorming, 1 no-method | TRIZ выше SCAMPER/no-method по novelty, но ниже brainstorming; выше brainstorming по utility | Только по одной команде в двух control arms; учебная задача; expert ratings ([DOI, 2013](https://doi.org/10.1007/s00163-012-0134-0)) [single-source study] |
| Hernandez et al., 2013 | 49 студентов в двух cohorts: UTEP 20 control/9 TRIZ; UMD 20 pre/post | Quantity не изменилась значимо; average novelty выросла: `p=.014` и `.004` | TRIZ homework разрешал до двух часов и семь дней, control был коротким занятием ([DOI, 2013](https://doi.org/10.1115/1.4024976)) [single-source study] |
| Birdi et al., 2012 | 123 trainees, 96 comparison employees; longitudinal/multisource, не randomised | Skills/motivation выросли; 41% trainees против 28% comparison сообщили patent applications; implemented innovation/performance неоднородны | Self-selection, self-report, организационные confounders ([DOI, 27.08.2012](https://doi.org/10.1111/j.1467-9310.2012.00686.x)) [single-source study] |
| Chang et al., 2016 | 121 freshmen, шестинедельный курс, pre/post comparison | Positive effect на creative process; partial η²=.26 | Один вуз, subjective artifact ratings; описание assignment непоследовательно ([DOI, 2016](https://doi.org/10.1016/j.tsc.2015.10.003)) [single-source study] |
| Duran-Novoa et al., 2019/2024 | 108 участников, 36 команд, brainstorming/KJ/SCAMPER/TRIZ | Structured methods получили более высокие ratings novelty/variety/quality | Студенты, 65 минут обучения, expert ratings, возможная pseudo-replication ([DOI, 2019](https://doi.org/10.1007/s12008-019-00577-4); [full paper, 2024](https://www.jotse.org/index.php/jotse/article/view/2473/883)) |
| Arlitt et al., 2012 | Correct matrix против randomly populated matrix; N в доступном abstract не найден | Значимого преимущества правильной матрицы по quantity/variety не обнаружено | Sample size недоступен, одна задача ([DOI, 2012](https://doi.org/10.1115/IMECE2012-89500)) [single-source study] |
| Ge et al., 2025 | N=32, human-human/human-agent × brainstorming/TRIZ | Brainstorming дал большую fluency; TRIZ - большую elaboration; общего преимущества по originality/flexibility нет | Малый дизайн-эксперимент, не production ([опубликовано online 20.08.2025](https://doi.org/10.1017/S7821986623255907)) [single-source study] |

### Чего нет

Не найдено:

- RCT против TOC, DOE, STPA, FTA или «спросить эксперта»;
- production trial по MTTR, incident count или recurrence;
- сравнения novice repairability;
- исследования false-green rate;
- dismantling study «полный TRIZ против только contradiction/IFR/trimming».

Для этих пунктов: **[no source found]**.

### Главные критики

1. **[established] Patent-mining не воспроизводим.** Публичного original dataset, sampling frame, coding manual и inter-rater reliability для создания матрицы не найдено. Разные исторические источники называют несовместимые объёмы патентов. ([Chechurin & Borgianni, опубликовано 2016](https://doi.org/10.1016/j.compind.2016.06.002); [Ilevbare et al., 2013](https://doi.org/10.1016/j.technovation.2012.11.003)).

2. **[emerging] Post-hoc rationalization.** После известного решения почти всегда можно подобрать широкий принцип. Эксперимент со случайной матрицей не нашёл преимущества исторического mapping, а анализ 161 опубликованного кейса получил лишь пограничное отличие распределения recommendations от случайного, `p=.053`. ([Arlitt et al., 2012](https://doi.org/10.1115/IMECE2012-89500); [Borgianni et al., опубликовано 04.06.2021](https://doi.org/10.1017/dsj.2021.12)).

3. **[emerging] Mechanical и selection bias.** Старые параметры механически ориентированы; TRIZ-insider assessment сообщил около 48% net effectiveness классической матрицы на новых mechanical inventions, но это заинтересованный и не независимый источник. ([Mann, 2002](https://www.metodolog.ru/triz-journal/archives/2002/02/e/index.htm)) [single-source][low-authority].

4. **[established] Publication и survivorship bias.** В публикациях гораздо больше успехов, чем неудач. В consultancy dataset из 206 проектов в анализ вошёл 161, создано 1,082 feasible solutions, развивалось 180, но дальнейшую судьбу удалось проследить лишь у 64; launched были 31. Нет concurrent controls и полного denominator. ([TRIZ Review, 2019](https://matriz.org/wp-content/uploads/2019/03/TRIZ-Review-vol-1-no-1-1.pdf)) [single-source][low-authority].

5. **[established] Learning burden.** Практики сообщают сложный язык, высокие временные затраты и отсутствие единого application standard. ([Ilevbare et al., 2013](https://doi.org/10.1016/j.technovation.2012.11.003); [Russo & Duci, 2015](https://doi.org/10.1016/j.proeng.2015.12.357)).

**Итог A:** [emerging] небольшие исследования допускают пользу TRIZ для novelty и elaboration. Они не показывают, что TRIZ создаёт более надёжные системы или лучшие business outcomes. Сильная версия «точная универсальная наука, статистически извлечённая из патентов» - **[fringe]**.

---

# B. TRIZ в software, agents и infrastructure

## B1. Реальные применения с результатами

**[established] Публичных кейсов с настоящими SRE-метриками почти нет.** Обзор 2019 года характеризовал software-TRIZ как находящийся в «very initial phase». ([Govindarajan et al., финальная версия 20.03.2019](https://www.ijosi.org/index.php/IJOSI/article/download/175/374)) [single-source].

| Организация | Год | Задача | Инструмент | Сообщённый результат | Вердикт |
|---|---:|---|---|---|---|
| Ford | 2000/2005 | Ringing/hitching в diesel engine control | Anticipatory Failure Determination + Taguchi DOE | Механизм локализован примерно за месяц; после DOE поведение «virtually eliminated» | Нет чисел и нельзя отделить TRIZ от DOE. [single-source self-report] ([Smith & Phadke, 07.03.2005](https://www.inderscience.com/info/e_inarticle.php?artid=6428)) |
| AKIVA, организация скрыта | 2008 | Сложность identity/data-masking software | Ideality, Function Analysis | LOC `7,964 → 3,866`; авторская complexity `88.7 → 81.2` | Нет defect/maintenance follow-up; association-hosted self-report. [single-source][low-authority] ([Bhushan, 2008](https://www.aitriz.org/articles/TRIZFeatures/30383035-4268757368616e.pdf)) |
| Hewlett-Packard | 2010 | Шесть IT-services задач | Problem Formulation, 9 Laws, Principles, Su-Field | Сессии 3-15 часов; идеи и несколько patent applications | Авторы прямо говорят: не всё внедрено; operational metrics отсутствуют. [single-source self-report] ([Kasravi, 2010](https://www.aitriz.org/documents/TRIZCON/Proceedings/Kasravi-Applications-of-TRIZ-to-IT.pdf)) |
| OptimalSQM | 2018 | Выбор test techniques | TRIZ + Taguchi/DOE | DRE `87.66% → 93.43%` в retrospective verification | Число есть, но причинный вклад даёт главным образом DOE; нет `DOE without TRIZ`. [single-source] ([Lazić et al., май 2018](https://ijiet.com/wp-content/uploads/2018/06/22.pdf)) |
| Rakuten Global Ad Technology | 2020 | Technical debt и performance visibility | Su-Field + 76 standards | Создан KPI dashboard; предложены MTBF/MTTR | Результаты MTBF/MTTR не опубликованы. [single-source] ([Lee et al., октябрь 2020](https://atna-mam.utcluj.ro/index.php/Acta/article/download/1394/1188)) |
| 8 Scrum-команд университета | 2023 | Повторяющиеся Scrum-проблемы | Contradiction matrix | 56 sprints, 195 problem instances, 13 повторяющихся проблем; выбраны три рекомендации | Post-implementation outcome отсутствует. [single-source] ([AIP Proceedings, 2023](https://pubs.aip.org/aip/acp/article-pdf/doi/10.1063/5.0115577/16772985/020029_1_online.pdf)) |

Громкие корпоративные ROI-claims вроде Intel `$212.5M` встречаются в материалах руководителей TRIZ-программ, но не подтверждены audited financial report или независимым counterfactual. Их нельзя использовать как effectiveness estimate. ([Intel TRIZ Story, 2013](https://www.researchgate.net/publication/399203501_13_Key_Principles_to_Develop_and_Deploy_Your_Own_Innovation_Program_-_The_Intel_TRIZ_Story)) [single-source][low-authority].

## B2. Debugging, reliability и agentic AI

- **Debugging/root cause [emerging]:** Ford AFD - ближайший реальный пример. Инверсия «как намеренно создать отказ?» помогла найти feedback-loop delay. Но превосходство над fault injection или обычной гипотезой не проверялось.

- **Reliability/fault tolerance [speculative]:** концептуальные работы есть, но production-кейсов с MTTR, SLO, availability или incident-rate не найдено: **[no source found]**.

- **LLM-agent/multi-agent architecture [speculative]:** не найдено ни одного production-кейса, где TRIZ снизил agent loops, tool failures, rollout errors или MTTR: **[no source found]**.

- **Prompt/architecture design [emerging]:** исследования 2024-2026 в основном заставляют LLM применять TRIZ к механическим и патентным задачам. Они не показывают, что TRIZ улучшает архитектуру самого агента.

## B3. Четыре ваших противоречия

Это наш аналитический перенос, поэтому формулировки помечены **[speculative]**. Они не являются эмпирическим доказательством TRIZ.

### 1. Независимый watchdog, который видит внутреннее состояние

- **Техническое:** если watchdog отделён от watched system, он переживает её падение, но хуже видит внутреннее состояние. Если он встроен, видимость выше, но появляется common-mode failure.
- **Физическое:** наблюдатель должен быть внутри системы, чтобы видеть её состояние, и вне системы, чтобы не умереть вместе с ней.
- **Разделение:** между целым и частью плюс пространство. Внутри остаётся минимальный read-only probe; решение и alarm живут в другом failure domain.
- **IFR:** полезный выход системы сам оставляет минимальное проверяемое свидетельство, а внешний независимый узел проверяет его свежесть.
- **AK-47:** один timestamp/version receipt, один внешний checker, никакого второго control plane.
- **Суд:** TRIZ в основном переименовала стандартный hybrid black-box/white-box monitoring. Полезный ход - проверять output freshness, а не здоровье процесса - мог быть неочевидным.

### 2. Browser должен быть «как человек», но работать без человека

- **Техническое:** persistent legitimate session улучшает доступ и continuity, но unattended control повышает fragility/policy risk. Ручной режим безопаснее для re-auth, но убивает автономность.
- **Физическое:** человеческое присутствие должно быть и не должно быть.
- **Разделение:** во времени и по условию. Человек нужен при enrollment, явном re-auth и исключениях; рутинная работа использует закреплённый legitimate profile.
- **IFR:** существующая пользовательская сессия выполняет разрешённые routine actions без отдельной stealth-инфраструктуры; человек появляется только когда сервис реально требует его.
- **AK-47:** один mutable owner профиля, pinned machine, ручная boundary для re-auth, без cookie-sync между пятью узлами.
- **Суд:** стандартный human-in-the-loop exception pattern. Не прорыв. TRIZ полезно разделила identity continuity и постоянное человеческое присутствие.

### 3. Fix должен попасть на пять машин, включая offline и receive-only

- **Техническое:** push даёт скорость, но ломается на offline/permission boundaries; pull уважает автономию и offline, но создаёт задержку.
- **Физическое:** fix должен уже присутствовать на offline-машине и не может присутствовать, пока она offline.
- **Разделение:** во времени и по условию. Immutable desired-state package публикуется один раз; машина применяет его при следующем reconnect.
- **IFR:** publish once; существующий sync доставляет; локальный idempotent consumer применяет и выдаёт proof-of-application.
- **AK-47:** текстовый version marker + per-node apply + ACK; не строить новый orchestrator.
- **Суд:** в основном eventual convergence/GitOps. Небольшая польза TRIZ - жёстко отделить delivery от application.

### 4. Индикатор должен молчать, но его смерть должна быть видна

- **Техническое:** alert-on-success доказывает liveness, но создаёт шум; alert-on-failure тих, но смерть самого notifier выглядит как здоровье.
- **Физическое:** индикатор должен молчать и одновременно подавать сигнал.
- **Разделение:** по времени и условию. Машина постоянно оставляет дешёвое свидетельство; человек получает сообщение только при его просрочке.
- **IFR:** отдельной зелёной лампы нет. Появление свежего целевого output само является success evidence; отсутствие ожидаемого output автоматически становится alarm.
- **AK-47:** внешний freshness-check, без dashboard/database.
- **Суд:** здесь IFR дал лучший ход - убрать status indicator и проверять сам эффект. Но dead-man timer давно известен любому SRE. TRIZ не изобрела его, а помогла сформулировать.

**Итог B3:** 3 из 4 решений являются общеинженерными patterns. TRIZ полезна как запрет преждевременно соглашаться на компромисс, не как источник секретной архитектуры.

## B4. TRIZ против альтернатив

Часы ниже - мои planning estimates, а не published benchmarks: **[no source found]**.

| Метод | Где бьёт TRIZ | Где TRIZ бьёт его | Цена входа |
|---|---|---|---:|
| Hypothesis-driven debugging | Воспроизведение, falsifiable hypotheses, cheapest discriminating test; лучший default после инцидента ([Google SRE, 2016](https://sre.google/sre-book/effective-troubleshooting/)) | Когда причина доказана, но каждый простой fix создаёт новый harm | 2-4 ч |
| Kepner-Tregoe | `IS/IS NOT`, distinctions, changes; отлично локализует drift ([AHRQ, без даты](https://digital.ahrq.gov/health-it-tools-and-resources/evaluation-resources/workflow-assessment-health-it-toolkit/all-workflow-tools/kepner-tregoe-matrix)) | После localisation, если fix зажат противоречием | 8-24 ч |
| 5 Whys | Самый дешёвый путь глубже симптома | TRIZ лучше при настоящем A/not-A конфликте | 0.5-2 ч |
| FTA | Строит AND/OR пути к `dead pipeline`, а не идеи ([NASA, 10.05.2000](https://nodis3.gsfc.nasa.gov/displayCA.cfm?Internal_ID=N_PR_8621_0001_&page_name=AppdxI35)) | TRIZ лучше генерирует новый design после нахождения paths | 4-12 ч |
| FMEA | До аварии покрывает component failure modes и effects ([NASA handbook, 20.08.2024](https://standards.nasa.gov/standard/GSFC/GSFC-HDBK-8004)) | TRIZ сильнее при redesign выбранного critical mode | 8-16 ч |
| STAMP/STPA/CAST | Сильнее для feedback/control, human-automation и отказов без поломки детали ([MIT handbooks, 2018/2019](https://psas.scripts.mit.edu$HOME/)) | TRIZ легче для генерации конкретного concept | 16-40 ч |
| Systems thinking/causal loops | Сильнее для retry/auth/load loops и взаимодействующих причин | TRIZ лучше на локальном противоречии после понимания dynamics | 4-8 ч qualitative |
| Cynefin | Определяет, нужна ли аналитика или safe-to-fail probes ([Snowden & Boone, 11.2007](https://hbr.org/2007/11/a-leaders-framework-for-decision-making)) | TRIZ даёт solution prompts, Cynefin только маршрутизирует | 2-4 ч |
| Theory of Constraints | Находит реальный bottleneck pipeline ([TOC Institute, без даты](https://www.tocinstitute.org/five-focusing-steps.html)) | TRIZ полезнее, если constraint нельзя снять без нового вреда | 3-6 ч |
| Design of Experiments | Даёт причинные данные и interaction effects ([NIST handbook, доступ 2026](https://itl.nist.gov/div898/handbook/pmd/section3/pmd31.htm)) | TRIZ помогает придумать factors и candidate designs | 16-40 ч |
| Wardley mapping | Лучше для build/buy, maturity и dependency strategy ([Wardley archive, 2017](https://archive.learnwardleymapping.com/)) | TRIZ лучше для одной engineering contradiction | 4-8 ч |

Практический порядок:

`hypothesis/KT → FTA/FMEA или STPA при достаточном риске → DOE для нескольких взаимодействующих переменных → минимальный TRIZ только при доказанном противоречии`.

## B5. LLM, применяющие TRIZ

| Работа | N/дизайн | Что получилось | Ограничение |
|---|---|---|---|
| GPT-4 contradiction extraction, 2024 | 100 positive + 100 negative patents | Positive F1 `.56`, примерно на уровне fine-tuned baseline `.54` | На negative-set GPT-4 нашёл contradictions в 86/100; false-positive/label ambiguity не разрешены. ([21.03.2024](https://arxiv.org/abs/2403.14258)) [single-source] |
| TRIZ-GPT, 2024 | 34 classic, 10 post-cutoff cases, 84 solution instances | GPT-4 CoT recall `.691`, precision `.310`; text similarity >`.82` | Низкая precision; similarity известному ответу не равна реализуемости. ([12.08.2024](https://arxiv.org/abs/2408.05897)) [single-source] |
| AutoTRIZ, 2025 | 10 textbook cases ×100 repetitions + simulated battery design | 7/10 top-3 contradiction matches; simulation сообщила +24% energy density и +64% thermal efficiency | Всего 10 учебных задач; design не построен и не испытан независимо. ([24.03.2025](https://arxiv.org/abs/2403.13002)) [single-source] |
| Domel workshop, 2025 | 11 engineers, 4 academics, 3 confidential problems | Некоторые новые идеи | Участники предпочли plain ChatGPT; significant benefit TRIZ+LLM не показан; 95-99% работы осталось на verification. ([2025](https://doi.org/10.1017/pds.2025.10101)) [single-source] |
| TRIZ Agents, 2025 | Один gantry-crane case, 8 агентов | Часть reference contradictions и одно ключевое решение | 60-80 calls, 150k-250k tokens/run, nondeterminism, нет baseline. ([2025](https://arxiv.org/abs/2506.18783)) [single-source] |
| TRIZBench, 2026 | 1,354 cases, 1,679 contradictions, 429 US patents | Matrix+LLM rerank Hit@3 `.71/.59`; zero-shot Gemini `.45/.31` | Benchmark известных cases, не production debugging. ([07.2026](https://aclanthology.org/2026.findings-acl.1798/)) [single-source] |
| TRIZ+LLM experiment, 2026 | 113 usable participants, random allocation, 3 problems | Общий quality effect `p=.076`; LLM снизил perceived difficulty, `d=.779` | Польза только на одной лёгкой задаче, не на двух нишевых. ([08.2026](https://doi.org/10.1017/pds.2026.10409)) [single-source] |

**[emerging] Lab-ready:** LLM как секретарь, который предлагает 2-3 формулировки противоречия, четыре separation moves и кандидатов на trimming.

**[speculative] Не lab-ready:** autonomous TRIZ RCA-agent, multi-agent TRIZ graph, новый vector store или отдельный TRIZ service.

---

# C. Столкновение с радикальной простотой

## C1. Где diverge IFR/ideality и AK-47

TRIZ оптимизирует полезные функции относительно cost/harm. AK-47 оптимизирует также понятность, независимость отказов и возможность ремонта слабейшим владельцем.

| TRIZ-идеал | Почему он менее ремонтопригоден |
|---|---|
| Watchdog использует существующий лог/шину и отдельный механизм исчезает | Если шина или producer умерли, исчезают и система, и её свидетель |
| Один roaming browser profile обслуживает пять машин | Общий mutable state создаёт corruption/ban blast radius |
| Self-applying deployment bundle | Появляются authority, rollback state, compatibility rules и silent partial apply |
| Heartbeat спрятан в полезном трафике | Исчезновение трафика не объясняет, умерла работа или проверка |
| Один универсальный компонент выполняет три функции | Меньше деталей, но больше coupling и сложнее локализация |

Google SRE показывает практический пример того же класса: reuse существующей DNS-инфраструктуры создал circular bootstrap dependency, которую пришлось разорвать локальным простым fallback. ([Google SRE Workbook, 2018](https://sre.google/workbook/simplicity/)) [single-source].

**Локальная формула [speculative]:**

`AK-ideality = useful effect / (money + harm + hidden state + coupling + repair steps + common-mode failure)`.

## C2. Overengineering и cleverness

**Прямого контролируемого исследования «TRIZ породил более сложную и менее ремонтопригодную систему» не найдено: [no source found].**

Есть косвенные сигналы:

- практики сообщают сложность метода, высокие временные затраты и отсутствие единого стандарта применения ([Ilevbare et al., 2013](https://doi.org/10.1016/j.technovation.2012.11.003));
- practitioner critique называет ARIZ трудным для быстрого освоения и Su-Field искусственно ограничивающим solution space ([Kaplan, 1998](https://the-trizjournal.com/problem-solving-systems-whats-next-triz/)) [single-source][low-authority];
- Google SRE связывает loose coupling и простоту с более быстрым восстановлением ([Google SRE Book, 2016](https://sre.google/sre-book/simplicity/));
- эмпирические исследования подтверждают связь понятности с maintenance cost, но не дают универсальной формулы «clever code = defect» ([Lavazza et al., 15.11.2023](https://link.springer.com/article/10.1007/s10664-023-10396-7)) [single-source study].

Связанные предохранители:

- Gall's law является инженерным афоризмом из *Systemantics*, 1975, а не контролируемым законом. ([История и библиография](https://en.wikipedia.org/wiki/Systemantics)) [single-source][low-authority].
- «Worse is better» - авторская позиция Gabriel, не экспериментальная теорема. ([Gabriel, 1991](https://www.dreamsongs.com/WorseIsBetter.html)) [single-source].
- Chesterton's fence требует понять функцию элемента до удаления. ([Chesterton, *The Thing*, 1929, scan](https://www.basilica.ca/documents/2016/10/G.K.Chesterton-The%20Thing.pdf)) [single-source].

Steelman TRIZ: принципы являются prompts, а не командами. Overengineering может быть ошибкой команды, которая не записала harms и не проверила решение, а не свойством самого метода.

## C3. Все 40 принципов в software/ops

Канонические названия взяты из [Altshuller Institute, без даты, доступ 06.08.2026](https://triz.org/principles/). Категории ниже - наша policy-классификация **[speculative]**, не часть TRIZ.

| № | Принцип | Класс | Software/ops-причина |
|---:|---|---|---|
| 1 | Segmentation | REMOVE* | Видимые шаги и failure isolation; microservices могут превратить в ADD |
| 2 | Taking out | REMOVE | Удалить вредный модуль, permission или retry |
| 3 | Local quality | ADD | Узловые исключения и drift |
| 4 | Asymmetry | NEUTRAL | Разные роли могут упростить или усложнить recovery |
| 5 | Merging | REMOVE* | Убирает дубли; опасен общим blast radius |
| 6 | Universality | REMOVE* | Один инструмент вместо нескольких; опасен god-object |
| 7 | Nested doll | ADD | Wrapper/proxy layers скрывают путь отказа |
| 8 | Anti-weight | NEUTRAL | Software-аналог неоднозначен |
| 9 | Preliminary anti-action | ADD | Compensator или резервный action/state |
| 10 | Preliminary action | ADD | Cache, pre-auth, staged state, invalidation |
| 11 | Beforehand cushioning | ADD | Buffer, retry и fallback требуют отдельного теста |
| 12 | Equipotentiality | NEUTRAL | Иногда убирает transition/privilege boundary |
| 13 | Other way round | REMOVE | Pull вместо push, effect вместо status |
| 14 | Curvature | NEUTRAL | Слабая software-метафора |
| 15 | Dynamics | ADD | Runtime modes и mutable configuration |
| 16 | Partial/excessive action | REMOVE | Минимальный достаточный и обратимый шаг |
| 17 | Another dimension | NEUTRAL | Новый namespace/index может помочь или добавить схему |
| 18 | Mechanical vibration | NEUTRAL | Polling/jitter, направление неочевидно |
| 19 | Periodic action | ADD | Cron/poller и silent no-op |
| 20 | Continuity of useful action | ADD | Always-on lifecycle |
| 21 | Skipping | REMOVE | Убрать ненужную стадию, fail fast |
| 22 | Blessing in disguise | REMOVE* | Использовать существующий сигнал; возможен hidden coupling |
| 23 | Feedback | ADD | Sensor, controller, threshold и state |
| 24 | Intermediary | ADD | Broker, adapter, bus, proxy |
| 25 | Self-service | ADD | Self-healing/update и recursive failure |
| 26 | Copying | NEUTRAL | Snapshot помогает recovery, но дублирует state |
| 27 | Cheap short-living objects | REMOVE | Disposable worker/profile вместо stateful pet |
| 28 | Mechanics substitution | NEUTRAL | API/event может убрать или добавить dependency |
| 29 | Pneumatics/hydraulics | NEUTRAL | Произвольный software mapping |
| 30 | Flexible shells/films | NEUTRAL | Thin adapter либо полезная граница, либо лишний слой |
| 31 | Porous materials | ADD | Hooks/plugins увеличивают interface surface |
| 32 | Color changes | NEUTRAL | Observability помогает, но добавляет канал |
| 33 | Homogeneity | REMOVE | Один формат/runtime уменьшает variants |
| 34 | Discarding/recovering | REMOVE | Временное удаляется, восстановление идёт из canonical source |
| 35 | Parameter changes | ADD | Flags, thresholds, combinatorial states |
| 36 | Phase transitions | ADD | State machine и transition bugs |
| 37 | Thermal expansion | NEUTRAL | Нет устойчивого software-аналога |
| 38 | Strong oxidants | NEUTRAL | Усиление среды/агента неоднозначно |
| 39 | Inert atmosphere | NEUTRAL | Isolation уменьшает interaction, но добавляет boundary |
| 40 | Composite materials | ADD | Polyglot/multi-provider stack |

**Принимать по умолчанию:** `1, 2, 5, 6, 13, 16, 21, 22, 27, 33, 34`, но 1/5/6 только без нового remote boundary и shared failure domain.

**Отказывать как первый ход:** `7, 15, 20, 24, 25, 35, 36, 40`.

Остальные - только через COMPLICATION-гейт: какую измеряемую боль лечит, что сломается без, дешёвая ручная альтернатива, новые states/dependencies, rollback и repair drill.

## C4. Trimming и AK-47

**[established] Это ближайший родственник AK-47 внутри TRIZ, но не одно и то же.** Trimming удаляет компонент, сохраняя его нужную функцию через другой компонент, объект функции или supersystem. ([MATRIZ Glossary, без даты](https://wiki.matriz.org/docs/triz/glossary-6146/); [MATRIZ training manual, 2019](https://www.matriz-official.net/images/PDF/GEN_TRIZ_Training_-_Basic_WS_Manual_July_2019pdf.pdf)).

Trimming повышает fragility, если:

- удаляет redundancy;
- переносит мониторинг в watched system;
- передаёт функцию внешнему supersystem, недоступному offline;
- объединяет control и actuation;
- убирает manual fallback;
- удаляет единственный внешний признак сбоя;
- превращает явный state в вычисляемый hidden state.

Прямого исследования роста software fragility после TRIZ trimming нет: **[no source found]**. Это reliability inference **[speculative]**.

Перед trimming надо перечислять не только nominal function, но и observability, isolation, recovery, audit, rate limiting, manual override и institutional memory.

## C5. Adoption cost и partial adoption

**Документированный learning burden:** MATRIZ имеет пять уровней, экзамены, проект, требования к опыту; коммерческие introductory courses варьируются примерно от 10 часов до нескольких дней. Это vendor data, но показывает размер curriculum. ([MATRIZ Certification, без даты](https://matriz.org/certification/) [single-source]; [Altshuller Institute training, доступ 2026](https://triz.org/individual-training/) [single-source][low-authority]).

Плановые оценки для лаборатории, не опубликованные benchmarks:

- contradiction + separation + IFR + trimming: **3-6 часов**;
- матрица и уверенное использование принципов: **20-40 часов**;
- Function Analysis, trends, Su-Field: **40-80 часов**;
- ARIZ/facilitator competence: **100+ часов**.

Для этих чисел: **[no source found], analyst estimate**.

**Partial adoption coherent [emerging]**, если называть её «четыре заимствованные эвристики», а не внедрением всей TRIZ. SIT и USIT показывают, что упрощение концептуально возможно. Но контролируемого evidence, что именно наш bundle снижает MTTR или recurrence, нет: **[no source found]**.

Самое сильное прямое возражение: случайно заполненная matrix не уступила правильной в одном эксперименте. Возможно, полезен сам structured prompt, а не TRIZ-specific patent mapping. ([Arlitt et al., 2012](https://doi.org/10.1115/IMECE2012-89500)) [single-source study].

---

# D. Что делать

## D1. Четыре варианта

| Вариант | Учёба | Изменение работы | 30-дневная метрика | Отказ |
|---|---:|---|---|---|
| **0. Evidence Loop** | 2-4 ч | Reproduce → evidence → falsifiable hypotheses → cheapest test → output verification | recurrence, false-green/100 runs, p50/p90 detection lag, repair time | Это baseline, не удалять |
| **1. AK-TRIZ Card** | 3-6 ч | Только после доказанной причины: 20 минут на contradiction, separation, IFR, trimming | ≥6 разборов; новые принятые moves; complexity delta; recurrence | 0 новых ходов после 4-6 разборов или >30 мин/разбор |
| **2. CAST-Lite + AK-TRIZ** | 16-24 ч | Для системных аварий строится control/feedback map, затем TRIZ-redesign | Доля control/feedback failures, reuse карт | Большинство сбоев линейны и локальны |
| **3. Full TRIZ** | 40-120+ ч | Matrix, 40 principles, Function Analysis, Su-Field, trends, ARIZ | Нужны десятки genuinely inventive problems | Для текущего fleet отказаться сразу |

Часы - planning estimates: **[no source found]**.

### Рекомендация

**Вариант 1 поверх Варианта 0, confidence [emerging].**

Ваш текущий регламент уже собирает серию, проверяет общий механизм, строит карту условий и отделяет расследование от forever-fix. TRIZ должна начинаться только там, где причинный механизм доказан, но простое исправление создаёт другой вред.

Успех за 30 дней:

- минимум два принятых неочевидных хода на шесть разборов; или
- уменьшение повторений сопоставимых классов хотя бы на 20%; или
- ritual остановил fix, создававший common-mode failure;
- без роста новых services, persistent states и external dependencies.

Отказ:

- четыре последовательных разбора не изменили решение;
- средняя карточка занимает больше 30 минут;
- LLM выдаёт списки принципов вместо проверяемых designs;
- решения стали добавлять больше state и coupling;
- TRIZ начали применять до воспроизведения причины.

## D2. Минимальный ритуал, одна страница

### AK-TRIZ Card

1. **Факт:** какой полезный результат отсутствует? Не «job зелёный», а «целевой файл не обновился».
2. **Доказательство:** expected, actual, timestamp, reproduction, сырой artifact/log.
3. **Причина:** какой доказанный механизм убил результат? Если только гипотеза, остановиться.
4. **Техническое противоречие:**

   `Если делаем X, получаем пользу A, но вред B.`  
   `Если не делаем X, убираем B, но теряем A.`

5. **Физическое противоречие:**

   `Один объект должен быть P, потому что..., и не-P, потому что...`

6. **Разделение:** можно ли разделить требования:

   - во времени;
   - в пространстве/failure domains;
   - по условию;
   - между целым и частями?

7. **IFR с AK-47:**

   `Полезный эффект происходит и проверяется без нового always-on service, shared state и обязательного cloud dependency.`

8. **Trimming:** что можно убрать? Кто заберёт каждую полезную, наблюдательную, аварийную и восстановительную функцию?
9. **COMPLICATION:** новые components/states/dependencies, дешёвая ручная альтернатива, rollback, кто починит без автора?
10. **Pilot:** одна машина, один falsifiable result, rollback до запуска.

### Демонстрация: «зелёный индикатор, мёртвый pipeline»

**Факт:** scheduler вернул exit 0, но target artifact не обновился.

**Причина:** delivery/process status ошибочно принят за proof of application.

**Техническое противоречие:**

- Если доверять exit code, проверка проста, но green не доказывает effect.
- Если проверять target, доказательство честное, но появляется отдельная проверка.

**Физическое противоречие:** индикатор должен молчать, чтобы не шуметь, и подавать сигнал, чтобы его собственная смерть была видна.

**Разделение:**

- во времени: machine evidence появляется каждый успешный run, human alert только при expiry;
- в пространстве: target проверяет внешний failure domain;
- по условию: сообщение только при stale/mismatch;
- целое/части: publisher создаёт artifact, независимый checker оценивает его свежесть.

**IFR:** отдельной зелёной лампы не существует. Свежий target artifact сам является доказательством успеха. Его отсутствие после deadline становится alarm.

**Trimming:** удалить `green=true` и dashboard, если они не доказывают target state.

**AK-47-решение:** один version/timestamp marker и один внешний freshness-check. Никакой БД и нового orchestration platform.

**Что дал TRIZ:** полезно инвертировал success indicator в dead-man condition. Но сам pattern давно известен SRE. Добавленная ценность метода - в формулировке, не в изобретении.

## D3. Когда TRIZ будет неправильным выбором

TRIZ не брать, если:

- нет воспроизводимости и наблюдаемости;
- причина линейна: expired token, неверный path, missing consumer;
- root cause ещё не доказана;
- bottleneck - исполнение, ownership или rollout discipline;
- безопасный стандартный SRE-pattern уже известен;
- проблема требует causal experiment, а не идей;
- система safety-critical, но команда заменяет STPA/FMEA красивой эвристикой;
- владелец не хочет поддерживать дополнительный словарь;
- нет счётчика повторений и complexity ledger.

**Сильнейший аргумент против рекомендации:** все четыре ваши задачи уже имеют известные решения - external effect check, pinned profile owner, pull/apply receipt, dead-man freshness. Возможно, Evidence Loop даст 100% пользы, а TRIZ добавит только 20 минут и новые слова.

---

# Источники

## TRIZ, история и evidence

1. [Ilevbare, Probert & Phaal, online 10.01.2013](https://doi.org/10.1016/j.technovation.2012.11.003).
2. [Chechurin & Borgianni, 2016](https://doi.org/10.1016/j.compind.2016.06.002).
3. [MATRIZ Contradictions, без даты, доступ 06.08.2026](https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/contradictions/).
4. [MATRIZ IFR, без даты, доступ 06.08.2026](https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/ariz-5892/ideal-final-result-5922/).
5. [MATRIZ Ideal System, без даты, доступ 06.08.2026](https://wiki.matriz.org/docs/triz/problem-solving-tools-5807/function-cost-analysis-7189/function-analysis-5611/valu-analysis-6483/value-5943/ideal-system-5939/).
6. [MATRIZ Function Analysis, без даты, доступ 06.08.2026](https://wiki.matriz.org/knowledge-base/triz/problem-solving-tools-5807/function-cost-analysis-7189/function-analysis-5611/).
7. [MATRIZ Su-Field Analysis, без даты, доступ 06.08.2026](https://wiki.matriz.org/knowledge-base/triz/problem-solving-tools-5890/substance-field-modeling/standard-inventive-solutions/substance-field-analysis/).
8. [MATRIZ Glossary, без даты, доступ 06.08.2026](https://wiki.matriz.org/docs/triz/glossary-6146/).
9. [Altshuller Institute 40 Principles, без даты, доступ 06.08.2026](https://triz.org/principles/).
10. [MATRIZ Certification, без даты, доступ 06.08.2026](https://matriz.org/certification/).
11. [ISO 11929-2:2019, 02.2019](https://www.iso.org/standard/69580.html).
12. [ISO 56000:2025, 23.01.2025](https://www.iso.org/standard/84436.html).
13. [ISO 56001:2024, 10.09.2024](https://www.iso.org/standard/79278.html).
14. [ISO/TR 23847:2022, 14.07.2022](https://www.iso.org/standard/77148.html).
15. [Chulvi et al., 2013](https://doi.org/10.1007/s00163-012-0134-0).
16. [Hernandez et al., 2013](https://doi.org/10.1115/1.4024976).
17. [Birdi et al., 27.08.2012](https://doi.org/10.1111/j.1467-9310.2012.00686.x).
18. [Chang et al., 2016](https://doi.org/10.1016/j.tsc.2015.10.003).
19. [Duran-Novoa et al., 2019](https://doi.org/10.1007/s12008-019-00577-4).
20. [Arlitt et al., 2012](https://doi.org/10.1115/IMECE2012-89500).
21. [Ge et al., online 20.08.2025](https://doi.org/10.1017/S7821986623255907).
22. [Borgianni et al., 04.06.2021](https://doi.org/10.1017/dsj.2021.12).
23. [Spreafico & Russo, 2016](https://www.sciencedirect.com/science/article/pii/S4689012408145752).
24. [Russo & Duci, 2015](https://doi.org/10.1016/j.proeng.2015.12.357).

## Software и LLM applications

25. [Govindarajan et al., финальная версия 20.03.2019](https://www.ijosi.org/index.php/IJOSI/article/download/175/374).
26. [Smith & Phadke, Ford case, 07.03.2005](https://www.inderscience.com/info/e_inarticle.php?artid=6428).
27. [Bhushan, AKIVA, 2008](https://www.aitriz.org/articles/TRIZFeatures/30383035-4268757368616e.pdf).
28. [Kasravi, HP IT cases, 2010](https://www.aitriz.org/documents/TRIZCON/Proceedings/Kasravi-Applications-of-TRIZ-to-IT.pdf).
29. [Lazić et al., май 2018](https://ijiet.com/wp-content/uploads/2018/06/22.pdf).
30. [Lee et al., Rakuten, октябрь 2020](https://atna-mam.utcluj.ro/index.php/Acta/article/download/1394/1188).
31. [Scrum case, AIP Proceedings, 2023](https://pubs.aip.org/aip/acp/article-pdf/doi/10.1063/5.0115577/16772985/020029_1_online.pdf).
32. [GPT-4 contradiction extraction, 21.03.2024](https://arxiv.org/abs/2403.14258).
33. [TRIZ-GPT, 12.08.2024](https://arxiv.org/abs/2408.05897).
34. [AutoTRIZ, revised 24.03.2025](https://arxiv.org/abs/2403.13002).
35. [Domel industrial workshop, 2025](https://doi.org/10.1017/pds.2025.10101).
36. [TRIZ Agents, 2025](https://arxiv.org/abs/2506.18783).
37. [TRIZBench, июль 2026](https://aclanthology.org/2026.findings-acl.1798/).
38. [TRIZ+LLM experiment, август 2026](https://doi.org/10.1017/pds.2026.10409).

## Альтернативы и simplicity

39. [Google SRE, Effective Troubleshooting, 2016](https://sre.google/sre-book/effective-troubleshooting/).
40. [Google SRE, Simplicity, 2016](https://sre.google/sre-book/simplicity/).
41. [Google SRE Workbook, Simplicity, 2018](https://sre.google/workbook/simplicity/).
42. [NASA FTA procedure, 10.05.2000](https://nodis3.gsfc.nasa.gov/displayCA.cfm?Internal_ID=N_PR_8621_0001_&page_name=AppdxI35).
43. [NASA FMEA/FMECA handbook, 20.08.2024](https://standards.nasa.gov/standard/GSFC/GSFC-HDBK-8004).
44. [MIT STPA/CAST handbooks, 2018/2019](https://psas.scripts.mit.edu$HOME/).
45. [NIST DOE handbook, доступ 2026](https://itl.nist.gov/div898/handbook/pmd/section3/pmd31.htm).
46. [Snowden & Boone, Cynefin, 11.2007](https://hbr.org/2007/11/a-leaders-framework-for-decision-making).
47. [TOC Institute, Five Focusing Steps, без даты](https://www.tocinstitute.org/five-focusing-steps.html).
48. [Wardley Mapping archive, 2017](https://archive.learnwardleymapping.com/).
49. [Lavazza et al., 15.11.2023](https://link.springer.com/article/10.1007/s10664-023-10396-7).
50. [Gabriel, Worse Is Better, 1991](https://www.dreamsongs.com/WorseIsBetter.html).
51. [Chesterton, The Thing, 1929 scan](https://www.basilica.ca/documents/2016/10/G.K.Chesterton-The%20Thing.pdf).

---

> 🧒 **Простыми словами:** TRIZ не волшебная наука и не замена нормальной отладке. Но четыре маленьких вопроса могут помочь, когда простой ремонт действительно упирается в противоречие. Проверяем их месяц. Если они не дают новых простых решений, выбрасываем без сожаления.

[06.08.2026, 15:22 Лондон]

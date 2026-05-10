<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Лента времени | Россия 1991–2022: хроника новой эпохи</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(145deg, #fff9f0 0%, #fef5e8 100%);
            color: #1e1a2f;
            padding: 2rem 1.5rem;
            scroll-behavior: smooth;
        }
        .container { max-width: 1400px; margin: 0 auto; }
        .header { text-align: center; margin-bottom: 3rem; position: relative; }
        .header::before, .header::after {
            content: "★";
            font-size: 2rem;
            color: #b22234;
            opacity: 0.3;
            position: absolute;
            top: 20%;
        }
        .header::before { left: 10%; }
        .header::after { right: 10%; }
        .header h1 {
            font-size: 3.2rem;
            font-weight: 800;
            background: linear-gradient(135deg, #0033a0, #b22234);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            margin-bottom: 0.5rem;
        }
        .header p {
            color: #4a3b1f;
            font-size: 1.2rem;
            max-width: 800px;
            margin: 0 auto;
        }
        .flag-stripes {
            display: flex;
            justify-content: center;
            width: 220px;
            margin: 1rem auto;
            height: 8px;
        }
        .flag-white { background: #FFFFFF; flex:1; border-radius: 4px 0 0 4px; }
        .flag-blue { background: #0033A0; flex:1; }
        .flag-red { background: #DA291C; flex:1; border-radius: 0 4px 4px 0; }

        .roadmap {
            display: flex;
            justify-content: center;
            gap: 2rem;
            margin: 2rem 0 2rem;
            flex-wrap: wrap;
        }
        .period-btn {
            background: #fff6e8;
            border: none;
            padding: 0.8rem 2rem;
            font-size: 1.3rem;
            font-weight: 700;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 12px rgba(0,0,0,0.1);
            font-family: 'Inter', sans-serif;
            color: #2d2b2b;
            border-left: 5px solid #b22234;
        }
        .period-btn:hover {
            transform: translateY(-3px);
            background: #b22234;
            color: white;
            border-left-color: #ffcd94;
        }

        .filter-bar {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 1rem;
            margin: 1.5rem 0 2rem;
        }
        .filter-btn {
            background: #fff0e0;
            border: none;
            padding: 0.5rem 1.2rem;
            font-size: 0.9rem;
            font-weight: 600;
            border-radius: 40px;
            cursor: pointer;
            transition: all 0.2s ease;
            font-family: 'Inter', sans-serif;
            color: #4a2e1a;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }
        .filter-btn.active {
            background: #b22234;
            color: white;
            box-shadow: 0 4px 10px rgba(178,34,52,0.3);
        }
        .filter-btn:hover:not(.active) {
            background: #f0d5c0;
            transform: translateY(-2px);
        }

        .timeline {
            display: flex;
            flex-direction: column;
            gap: 3rem;
            margin: 3rem 0;
        }
        .timeline-event {
            display: flex;
            flex-wrap: wrap;
            align-items: stretch;
            gap: 1.8rem;
            background: rgba(255,255,245,0.96);
            border-radius: 2rem;
            padding: 2rem;
            box-shadow: 0 20px 35px -12px rgba(0,0,0,0.2);
            transition: transform 0.25s ease, box-shadow 0.3s, opacity 0.3s;
            border-left: 8px solid;
            opacity: 1;
            transform: translateY(0);
            animation: fadeInUp 0.6s forwards;
        }
        .timeline-event.filtered-out { display: none; }
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .timeline-event:hover {
            transform: translateY(-6px);
            box-shadow: 0 28px 38px -15px rgba(0,0,0,0.3);
        }

        .event-politics { border-left-color: #1e5bbf; }
        .event-economy { border-left-color: #2c7345; }
        .event-social { border-left-color: #ff8c00; }
        .event-defense { border-left-color: #8b0000; }
        .event-sport { border-left-color: #eab308; }
        .event-foreign { border-left-color: #7c2d12; }

        .event-date-icon {
            flex: 0 0 170px;
            text-align: center;
            background: #fff6e8;
            border-radius: 1.8rem;
            padding: 1.2rem 0.5rem;
            box-shadow: 0 5px 12px rgba(0,0,0,0.05);
        }
        .event-year { font-size: 2.1rem; font-weight: 800; color: #2d2b2b; }
        .event-month { font-size: 0.9rem; font-weight: 600; color: #6b4c2c; }
        .event-icon {
            font-size: 3rem;
            margin: 0.8rem auto 0;
            color: #b22234;
            display: block;
            animation: subtleFloat 2s ease-in-out infinite;
            cursor: help;
            transition: transform 0.2s;
        }
        .event-icon:hover { transform: scale(1.1); }
        @keyframes subtleFloat {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-5px); }
            100% { transform: translateY(0px); }
        }
        .event-icon[data-tooltip] { position: relative; }
        .event-icon[data-tooltip]:before {
            content: attr(data-tooltip);
            position: absolute;
            bottom: 110%;
            left: 50%;
            transform: translateX(-50%);
            background: #1e1a2f;
            color: #ffefcf;
            padding: 0.4rem 0.8rem;
            border-radius: 8px;
            font-size: 0.75rem;
            white-space: nowrap;
            z-index: 10;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.2s;
            font-family: 'Inter', sans-serif;
        }
        .event-icon[data-tooltip]:hover:before { opacity: 1; }
        .event-content { flex: 2; min-width: 240px; }
        .event-title {
            font-size: 1.65rem;
            font-weight: 800;
            margin-bottom: 0.75rem;
            color: #0c1a3b;
            border-left: 4px solid #b22234;
            padding-left: 15px;
        }
        .event-description {
            font-size: 0.96rem;
            line-height: 1.55;
            color: #2c2a2a;
            margin-bottom: 0.8rem;
            text-align: justify;
        }
        .event-detail {
            font-size: 0.85rem;
            background: #f0e5d2;
            display: inline-block;
            padding: 0.3rem 1rem;
            border-radius: 40px;
            font-weight: 600;
            color: #8b3c1c;
        }
        .event-source {
            flex: 0 0 200px;
            background: #f4ede1;
            border-radius: 1.5rem;
            padding: 0.8rem 1.2rem;
            font-size: 0.75rem;
            color: #3b2a1f;
            border-left: 3px solid #b22234;
        }
        .source-title {
            font-weight: 800;
            margin-bottom: 0.5rem;
            display: flex;
            align-items: center;
            gap: 8px;
            color: #9b2f1a;
        }
        @media (max-width: 900px) {
            .timeline-event { flex-direction: column; padding: 1.6rem; }
            .event-date-icon {
                flex: auto;
                display: flex;
                align-items: center;
                gap: 1rem;
                width: 100%;
                text-align: left;
                padding: 0.8rem 1.2rem;
            }
            .event-icon { margin: 0; font-size: 2.2rem; }
            .event-source { width: 100%; }
            .event-icon[data-tooltip]:before { white-space: normal; width: 160px; bottom: 120%; }
        }
        .footer {
            text-align: center;
            margin-top: 4rem;
            padding: 2rem;
            background: #1e1a2f;
            border-radius: 2rem;
            color: #ffefcf;
        }
        .badge {
            background: #b22234;
            color: white;
            border-radius: 40px;
            padding: 0.3rem 1rem;
            font-size: 0.75rem;
            display: inline-block;
            margin: 0.2rem;
        }
        .period-anchor { scroll-margin-top: 80px; }
    </style>
</head>
<body>
<div class="container">
    <div class="header">
        <div class="flag-stripes"><div class="flag-white"></div><div class="flag-blue"></div><div class="flag-red"></div></div>
        <h1>⚡ Лента времени ⚡<br>Российская Федерация 1991 – 2022</h1>
        <p>От возрождения суверенной России к укреплению державного единства, социальных побед и геополитического достоинства.<br> 20 решающих вех на пути к сильной, независимой Родине.</p>
    </div>

    <div class="roadmap">
        <button class="period-btn" data-period="1991-2000">📅 1991 – 2000<br><span style="font-size:0.8rem;">Эпоха становления</span></button>
        <button class="period-btn" data-period="2001-2011">📅 2001 – 2011<br><span style="font-size:0.8rem;">Укрепление и развитие</span></button>
        <button class="period-btn" data-period="2012-2022">📅 2012 – 2022<br><span style="font-size:0.8rem;">Новый суверенитет</span></button>
    </div>

    <div class="filter-bar">
        <button class="filter-btn active" data-filter="all">Все события</button>
        <button class="filter-btn" data-filter="politics">🏛️ Политика</button>
        <button class="filter-btn" data-filter="economy">📊 Экономика</button>
        <button class="filter-btn" data-filter="social">👥 Общество</button>
        <button class="filter-btn" data-filter="defense">⚔️ Оборона</button>
        <button class="filter-btn" data-filter="sport">🏆 Спорт и культура</button>
        <button class="filter-btn" data-filter="foreign">🌍 Внешняя политика</button>
    </div>

    <div id="period-1991-2000" class="period-anchor"></div>
    <div class="timeline" id="timelineContainer">
        <!-- 1. 1991 Распад СССР -->
        <div class="timeline-event event-politics" data-category="politics">
            <div class="event-date-icon"><div class="event-year">1991</div><div class="event-month">8–25 дек.</div><i class="fas fa-flag-checkered event-icon" data-tooltip="Россия — правопреемница СССР"></i></div>
            <div class="event-content">
                <div class="event-title">Становление Российской государственности</div>
                <div class="event-description">8 декабря 1991 года лидеры РСФСР, Беларуси, Украины подписали Беловежские соглашения, констатировав прекращение существования СССР как субъекта международного права. Россия объявила себя правопреемницей Союза, сохранив ядерный арсенал и место в Совбезе ООН. 25 декабря М.С. Горбачев ушел в отставку. На карте мира появилась независимая Российская Федерация — прямой наследник тысячелетней русской истории. Так начался новый этап: страна взяла курс на демократические преобразования, но главное — сохранила территориальное ядро и историческую идентичность.</div>
                <span class="event-detail"><i class="far fa-calendar-alt"></i> 8–26 декабря 1991 года</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-globe"></i> Исторические источники</div>Декларация о правопреемстве, СМИ 1991, интервью участников.</div>
        </div>

        <!-- 2. 1992 Либерализация цен -->
        <div class="timeline-event event-economy" data-category="economy">
            <div class="event-date-icon"><div class="event-year">1992</div><div class="event-month">2 янв.</div><i class="fas fa-chart-line event-icon" data-tooltip="«Шоковая терапия»"></i></div>
            <div class="event-content">
                <div class="event-title">Начало рыночных реформ: либерализация цен</div>
                <div class="event-description">Правительство Егора Гайдара и указ Президента Б.Н. Ельцина отпустили цены на подавляющее большинство товаров. Это был болезненный, но неизбежный шок, разрушивший советский дефицит и запустивший механизмы конкуренции. Несмотря на гиперинфляцию (до 2500% за год), Россия положила конец тотальным очередям и создала базу для будущего предпринимательства. Реформа заложила основы многоукладной экономики, а впоследствии — нефтегазового роста и восстановления промышленности.</div>
                <span class="event-detail"><i class="fas fa-ruble-sign"></i> 2 января 1992</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-chart-simple"></i> Экономические обзоры</div>Гайдар Е. «Дни поражений и побед», Росстат.</div>
        </div>

        <!-- 3. 1993 Конституция -->
        <div class="timeline-event event-politics" data-category="politics">
            <div class="event-date-icon"><div class="event-year">1993</div><div class="event-month">12 дек.</div><i class="fas fa-gavel event-icon" data-tooltip="Принята всенародно"></i></div>
            <div class="event-content">
                <div class="event-title">Всенародное принятие Конституции РФ</div>
                <div class="event-description">После преодоления затяжного политического кризиса 12 декабря 1993 года на референдуме была принята Конституция, укрепившая федеративное устройство и права граждан. Основной закон закрепил пост сильного президента как гаранта стабильности и единства страны. Российская Конституция подарила гражданам свободу слова, многопартийность и право частной собственности. С этого момента Россия стала полноценной президентской республикой, а Конституция — ядром правовой системы на десятилетия вперёд.</div>
                <span class="event-detail"><i class="fas fa-book"></i> 12 декабря 1993</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-scroll"></i> Юридические акты</div>Конституция РФ, архив голосования, спецвыпуск «Российской газеты».</div>
        </div>

        <!-- 4. 1994-96 Первая чеченская -->
        <div class="timeline-event event-defense" data-category="defense">
            <div class="event-date-icon"><div class="event-year">1994-96</div><div class="event-month">11 дек.1994</div><i class="fas fa-shield-virus event-icon" data-tooltip="Первая чеченская кампания"></i></div>
            <div class="event-content">
                <div class="event-title">Борьба с терроризмом и сепаратизмом в Чечне</div>
                <div class="event-description">В ответ на вооружённый мятеж и этнические чистки российское руководство ввело войска для восстановления законности на территории Чеченской Республики. Несмотря на тяжелейшие потери и отсутствие единства в обществе, армия проявила мужество. Первая чеченская кампания (1994–1996) стала суровым уроком, вскрыв необходимость реформ в Вооружённых силах. Боевики не были окончательно разгромлены, но Россия доказала решимость бороться с экстремизмом. Хасавюртовские соглашения 1996 года стали временным отступлением, позже исправленным во Второй чеченской войне.</div>
                <span class="event-detail"><i class="fas fa-medal"></i> 1994–1996</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-file-alt"></i> Военная история</div>Книги Г. Трошева, Генштаб, интервью ветеранов.</div>
        </div>

        <!-- 5. 1996 выборы -->
        <div class="timeline-event event-politics" data-category="politics">
            <div class="event-date-icon"><div class="event-year">1996</div><div class="event-month">3 июля</div><i class="fas fa-vote-yea event-icon" data-tooltip="Переизбрание Ельцина"></i></div>
            <div class="event-content">
                <div class="event-title">Выборы президента – возврат к демократии</div>
                <div class="event-description">Второй тур выборов 1996 года: действующий президент Б.Н. Ельцин одержал победу над лидером КПРФ Г.А. Зюгановым. Этот результат подтвердил выбор россиян в пользу продолжения реформ, рыночной экономики и демократического пути. Несмотря на сложности 90-х, страна избежала реванша советской системы. Выборы стали символом политической зрелости и воли граждан, определив вектор развития на новое тысячелетие.</div>
                <span class="event-detail"><i class="fas fa-check-circle"></i> 3 июля 1996</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-chart-pie"></i> Политические архивы</div>ЦИК РФ, мемуары А. Чубайса, «Коммерсантъ» 1996.</div>
        </div>

        <!-- 6. 1998 Дефолт -->
        <div class="timeline-event event-economy" data-category="economy">
            <div class="event-date-icon"><div class="event-year">1998</div><div class="event-month">17 авг.</div><i class="fas fa-chart-line event-icon" data-tooltip="Дефолт по ГКО"></i></div>
            <div class="event-content">
                <div class="event-title">Финансовый кризис 1998 года и обретение опоры</div>
                <div class="event-description">Правительство было вынуждено объявить дефолт по ГКО и провести девальвацию рубля. Однако именно этот тяжелейший удар очистил экономику от пирамид ГКО, стимулировал импортозамещение и сделал российские товары конкурентоспособными. После дефолта страна вышла на траекторию быстрого роста в 1999–2008 годах. Кризис показал необходимость укрепления финансового суверенитета, что позже привело к созданию Стабфонда и независимой бюджетной политике.</div>
                <span class="event-detail"><i class="fas fa-dollar-sign"></i> 17 августа 1998</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-landmark"></i> Экономические исследования</div>ЦБ РФ, аналитика С. Алексашенко, книга «На грани».</div>
        </div>

        <!-- (удалены события 1999 теракты и 1999 Путин — по просьбе удаляем и их, чтобы сохранить хронологию, но оставляем только 1999 Путин? пользователь сказал удалить "Варварские теракты", а про Путина ничего не говорил, но для целостности оставим приход Путина, так как он важен. Проверим: в списке удаляемых событий указано "1999 – Варварские теракты". Событие "Приход Путина" остаётся. Хорошо. -->

        <!-- 7. 1999 Приход Путина (оставляем) -->
        <div class="timeline-event event-politics" data-category="politics">
            <div class="event-date-icon"><div class="event-year">1999</div><div class="event-month">авг.-дек.</div><i class="fas fa-user-secret event-icon" data-tooltip="Приход Путина к власти"></i></div>
            <div class="event-content">
                <div class="event-title">Приход к власти В.В. Путина: эпоха стабильности</div>
                <div class="event-description">9 августа 1999 года Владимир Путин назначен председателем правительства и объявлен преемником Б.Н. Ельцина. Под его руководством началась Вторая контртеррористическая операция в Чечне, которая закончилась полным разгромом бандподполья и восстановлением конституционного порядка. 31 декабря Борис Ельцин добровольно ушёл в отставку, передав бразды правления Путину. Началась эпоха возрождения государственности, укрепления армии, и возвращения России статуса великой державы.</div>
                <span class="event-detail"><i class="fas fa-history"></i> 9 августа – 31 декабря 1999</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-newspaper"></i> Хроники власти</div>Указы Президента, интервью В. Путина, фильм «Преемник».</div>
        </div>

        <!-- 8. 2000 избрание Путина -->
        <div class="timeline-event event-politics" data-category="politics">
            <div class="event-date-icon"><div class="event-year">2000</div><div class="event-month">26 марта</div><i class="fas fa-vote-yea event-icon" data-tooltip="Избрание президентом"></i></div>
            <div class="event-content">
                <div class="event-title">Избрание В.В. Путина президентом и укрепление вертикали</div>
                <div class="event-description">В марте 2000 года Владимир Путин одержал убедительную победу в первом туре с 52,9% голосов. Сразу после инаугурации были созданы федеральные округа, введены институты полпредов, реформирован Совет Федерации. Эти меры укрепили единство правового поля, остановили «парад суверенитетов» 90-х годов. Россия начала выходить из политического и экономического хаоса, повысились доходы граждан, начался мощный экономический рост, основанный на нефтяных доходах и ответственной бюджетной политике.</div>
                <span class="event-detail"><i class="far fa-calendar-check"></i> 26 марта 2000</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-university"></i> Политический анализ</div>Послания Президента, доклады МГУ, ФОМ.</div>
        </div>

        <!-- 9. 2000 Курск -->
        <div class="timeline-event event-defense" data-category="defense">
            <div class="event-date-icon"><div class="event-year">2000</div><div class="event-month">12 авг.</div><i class="fas fa-ship event-icon" data-tooltip="Гибель 118 моряков"></i></div>
            <div class="event-content">
                <div class="event-title">Подвиг экипажа подлодки «Курск»</div>
                <div class="event-description">12 августа 2000 года в Баренцевом море произошла катастрофа атомного ракетного крейсера «Курск». Все 118 членов экипажа героически погибли, до последнего боровшись за живучесть корабля. Трагедия вскрыла проблемы технического состояния флота, но также показала несгибаемый дух российских моряков. Память о «Курске» стала уроком для реформы Вооружённых сил, и уже в 2000-е годы Россия начала масштабное перевооружение армии и флота.</div>
                <span class="event-detail"><i class="fas fa-anchor"></i> 12 августа 2000</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-microphone"></i> Расследования и память</div>Правительственная комиссия, книга В. Устинова, документальный фильм.</div>
        </div>

        <!-- Якорь 2001-2011 -->
        <div id="period-2001-2011" class="period-anchor"></div>

        <!-- 10. 2004 монетизация (вместо удалённых терактов) -->
        <div class="timeline-event event-social" data-category="social">
            <div class="event-date-icon"><div class="event-year">2004</div><div class="event-month">август</div><i class="fas fa-coins event-icon" data-tooltip="Замена льгот выплатами"></i></div>
            <div class="event-content">
                <div class="event-title">Реформа социальной поддержки: монетизация льгот</div>
                <div class="event-description">Замeна «натуральных» льгот денежными выплатами — назревший, но тяжёлый шаг, вызвавший протесты пенсионеров. Тем не менее реформа позволила упорядочить систему соцзащиты, перейти к адресной помощи и оптимизировать бюджет. В дальнейшем правительство неоднократно индексировало выплаты, а сама система была улучшена. Монетизация стала частью перехода от советского патернализма к более гибкой модели социальной поддержки.</div>
                <span class="event-detail"><i class="fas fa-chart-simple"></i> 22 августа 2004</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-file-alt"></i> Закон №122-ФЗ</div>Госдума, «Парламентская газета», аналитика ЦСР.</div>
        </div>

        <!-- 11. 2006 Нацпроекты -->
        <div class="timeline-event event-social" data-category="social">
            <div class="event-date-icon"><div class="event-year">2006</div><div class="event-month">старт</div><i class="fas fa-hand-holding-heart event-icon" data-tooltip="Материнский капитал"></i></div>
            <div class="event-content">
                <div class="event-title">Приоритетные национальные проекты: инвестиции в человека</div>
                <div class="event-description">В 2006 году стартовали четыре нацпроекта: «Здоровье», «Образование», «Доступное жилье» и «Развитие АПК». На них были выделены десятки миллиардов рублей. Повысилась зарплата врачам и учителям, началась компьютеризация школ, запущена программа материнского капитала (с 2007). Это был мощный социальный рывок, заложивший основу демографического роста и улучшения качества жизни. Нацпроекты стали прообразом будущих стратегических инициатив до 2030 года.</div>
                <span class="event-detail"><i class="fas fa-chart-line"></i> 2006–2007</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-chart-pie"></i> Отчёты Счётной палаты</div>Совет по нацпроектам, интервью Д. Медведева.</div>
        </div>

        <!-- 12. 2008 Медведев президент -->
        <div class="timeline-event event-politics" data-category="politics">
            <div class="event-date-icon"><div class="event-year">2008</div><div class="event-month">7 мая</div><i class="fas fa-handshake event-icon" data-tooltip="Президентство Медведева"></i></div>
            <div class="event-content">
                <div class="event-title">Президентство Д.А. Медведева и модернизационный курс</div>
                <div class="event-description">7 мая 2008 года Дмитрий Медведев вступил в должность президента, сформировав «тандем» с В. Путиным (председатель правительства). Период ознаменовался поправками в Конституцию (увеличение срока полномочий до 6 лет), созданием иннограда Сколково, а также программой борьбы с коррупцией. Внешнеполитическая линия оставалась прагматичной. Медведев продолжил курс на укрепление суверенитета и технологическое обновление.</div>
                <span class="event-detail"><i class="fas fa-user-tie"></i> 7 мая 2008 – 2012</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fab fa-britannica"></i> Официальные документы</div>Указы президента, выступления на ПМЭФ.</div>
        </div>

        <!-- Якорь 2012-2022 -->
        <div id="period-2012-2022" class="period-anchor"></div>

        <!-- 13. 2012 – президентские выборы (новое событие) -->
        <div class="timeline-event event-politics" data-category="politics">
            <div class="event-date-icon"><div class="event-year">2012</div><div class="event-month">4 марта</div><i class="fas fa-vote-yea event-icon" data-tooltip="Возвращение Путина"></i></div>
            <div class="event-content">
                <div class="event-title">Президентские выборы 2012 года: возвращение В.В. Путина</div>
                <div class="event-description">4 марта 2012 года состоялись выборы президента России. Владимир Путин, занимавший пост премьер-министра, баллотировался от партии «Единая Россия» и одержал победу в первом туре, набрав 63,6% голосов. Также были зарегистрированы кандидаты Геннадий Зюганов (КПРФ), Михаил Прохоров (самовыдвижение), Владимир Жириновский (ЛДПР) и Сергей Миронов («Справедливая Россия»). Новшествами процесса стали прозрачные урны и 180 тысяч веб-камер. Событие ознаменовало завершение политического тандема и возвращение Путина на высший государственный пост, что во многом определило вектор развития страны на последующие годы.</div>
                <span class="event-detail"><i class="fas fa-check-circle"></i> 4 марта 2012</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-chart-pie"></i> Официальные данные</div>Протокол об итогах голосования на выборах Президента РФ 2012 года.</div>
        </div>

        <!-- 14. Август 2012 – Вступление в ВТО (заменён текст) -->
        <div class="timeline-event event-economy" data-category="economy">
            <div class="event-date-icon"><div class="event-year">2012</div><div class="event-month">22 авг.</div><i class="fas fa-chart-bar event-icon" data-tooltip="Россия в ВТО"></i></div>
            <div class="event-content">
                <div class="event-title">Россия становится членом Всемирной торговой организации (ВТО)</div>
                <div class="event-description">22 августа 2012 года Российская Федерация официально присоединилась к ВТО после 18-летних переговоров. Предполагалось, что членство приведёт к модернизации экономики и снижению цен. Негативные прогнозы о крахе целых отраслей, включая сельское хозяйство, не оправдались, но и ожидаемых позитивных сдвигов не произошло: экономический бум не наступил. Влияние ВТО на экономику России оказалось не столь значительным, как предполагалось изначально.</div>
                <span class="event-detail"><i class="fas fa-chart-line"></i> 22 августа 2012</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-chart-simple"></i> Международные договоры</div>Протокол о присоединении РФ к ВТО, Федеральный закон №126-ФЗ.</div>
        </div>

        <!-- 15. 2014 Крым (заменён текст) -->
        <div class="timeline-event event-foreign" data-category="foreign">
            <div class="event-date-icon"><div class="event-year">2014</div><div class="event-month">18 марта</div><i class="fas fa-map-marked-alt event-icon" data-tooltip="Воссоединение Крыма"></i></div>
            <div class="event-content">
                <div class="event-title">Подписание договора о присоединении Крыма и Севастополя к РФ</div>
                <div class="event-description">16 марта 2014 года в Крыму был проведён референдум о статусе полуострова. По официальным данным, 96,77% проголосовавших поддержали присоединение к России. 18 марта 2014 года Владимир Путин, премьер-министр Крыма Сергей Аксёнов и другие крымские лидеры подписали Договор о принятии Республики Крым и города Севастополя в состав Российской Федерации.</div>
                <span class="event-detail"><i class="fas fa-flag-checkered"></i> 18 марта 2014</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-file-signature"></i> Федеральный конституционный закон</div>ФКЗ №6-ФКЗ от 21.03.2014.</div>
        </div>

        <!-- 16. 2014 Олимпиада в Сочи (заменён текст) -->
        <div class="timeline-event event-sport" data-category="sport">
            <div class="event-date-icon"><div class="event-year">2014</div><div class="event-month">7–23 февр.</div><i class="fas fa-skiing event-icon" data-tooltip="Олимпиада в Сочи"></i></div>
            <div class="event-content">
                <div class="event-title">XXII зимние Олимпийские игры в Сочи</div>
                <div class="event-description">Крупнейший международный спортивный проект, потребовавший рекордных инвестиций (около 1,5 трлн рублей). Была построена совершенно новая курортная, транспортная и спортивная инфраструктура. Несмотря на опасения и некоторые организационные накладки, Игры признаны одним из самых успешных спортивных событий в истории. Олимпиада вызвала огромный патриотический подъём, стала символом возрождения России, а также катализатором социально-экономического развития региона.</div>
                <span class="event-detail"><i class="fas fa-medal"></i> 7–23 февраля 2014</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-tv"></i> Официальные источники</div>Федеральный закон №310-ФЗ, Olympics.com.</div>
        </div>

        <!-- 17. 2018 Чемпионат мира (заменён текст) -->
        <div class="timeline-event event-sport" data-category="sport">
            <div class="event-date-icon"><div class="event-year">2018</div><div class="event-month">14 июня–15 июля</div><i class="fas fa-futbol event-icon" data-tooltip="Чемпионат мира по футболу"></i></div>
            <div class="event-content">
                <div class="event-title">Чемпионат мира по футболу в России</div>
                <div class="event-description">С 14 июня по 15 июля 2018 года Россия впервые принимала чемпионат мира по футболу. Были построены или реконструированы 12 стадионов в 11 городах. Организация турнира получила высокие оценки со стороны ФИФА, команд и болельщиков. Сборная России сенсационно дошла до четвертьфинала. Чемпионат мира значительно улучшил имидж страны на международной арене, оставив современную спортивную инфраструктуру и новый туристический потенциал.</div>
                <span class="event-detail"><i class="fas fa-trophy"></i> 14 июня – 15 июля 2018</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-futbol"></i> Официальные документы</div>Федеральный закон №108-ФЗ, Указы Президента от 25.03.2013 №282 и от 09.05.2017 №202.</div>
        </div>

        <!-- 18. 2020 – поправки к Конституции и пандемия (объединено) -->
        <div class="timeline-event event-politics" data-category="politics">
            <div class="event-date-icon"><div class="event-year">2020</div><div class="event-month">июнь–июль</div><i class="fas fa-book-open event-icon" data-tooltip="Конституционная реформа и пандемия"></i></div>
            <div class="event-content">
                <div class="event-title">Пандемия COVID-19 и вступление в силу поправок к Конституции РФ</div>
                <div class="event-description">В марте 2020 года в России была зарегистрирована первая смерть от COVID-19, последовали режим самоизоляции, масочный режим, удалённая работа и закрытие границ. С 25 июня по 1 июля 2020 года прошло общероссийское голосование по поправкам в Конституцию. Явка составила около 65%, за поправки проголосовали 77,92% участников. Одобрены: обнуление президентских сроков, социальные гарантии, приоритет Конституции над международными актами. С 1 июля поправки вступили в силу. К концу 2020 года в России началась массовая вакцинация.</div>
                <span class="event-detail"><i class="fas fa-gavel"></i> 1 июля 2020 (поправки)</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-landmark"></i> Правовые источники</div>Постановление ЦИК №244/1804-7, Указ Президента №239 от 02.04.2020, Приказ Минздрава №198н.</div>
        </div>

        <!-- 19. 2022 СВО (убрано слово "нацистские") -->
        <div class="timeline-event event-defense" data-category="defense">
            <div class="event-date-icon"><div class="event-year">2022</div><div class="event-month">24 февр.</div><i class="fas fa-shield-alt event-icon" data-tooltip="Специальная военная операция"></i></div>
            <div class="event-content">
                <div class="event-title">Специальная военная операция по защите Донбасса</div>
                <div class="event-description">24 февраля 2022 года Президент России принял решение о начале специальной военной операции с целью демилитаризации и денацификации Украины, защиты мирного населения Донецкой и Луганской народных республик. Восьмилетние обстрелы Донбасса, геноцид русскоязычного населения и угроза вступления Украины в НАТО вынудили Россию на решительные действия. Операция продолжает укреплять безопасность страны, вооружённые формирования разоружаются, освобождаются исторические территории. Несмотря на беспрецедентное санкционное давление, Россия выстояла, переориентировала экономику на Восток и укрепила технологический суверенитет.</div>
                <span class="event-detail"><i class="fas fa-clock"></i> 24 февраля 2022</span>
            </div>
            <div class="event-source"><div class="source-title"><i class="fas fa-newspaper"></i> Официальные заявления</div>Обращение Президента РФ 24.02.2022, Минобороны РФ.</div>
        </div>
    </div>

    <div class="footer">
        <i class="fas fa-chalkboard-teacher"></i> Проект «Лента времени» — ключевые вехи новейшей истории России (1991–2022).
    </div>
</div>

<script>
    // Фильтрация по категориям
    const filterBtns = document.querySelectorAll('.filter-btn');
    const timelineEvents = document.querySelectorAll('.timeline-event');
    filterBtns.forEach(btn => {
        btn.addEventListener('click', () => {
            filterBtns.forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            const filter = btn.getAttribute('data-filter');
            timelineEvents.forEach(event => {
                if (filter === 'all') event.classList.remove('filtered-out');
                else {
                    const cat = event.getAttribute('data-category');
                    if (cat === filter) event.classList.remove('filtered-out');
                    else event.classList.add('filtered-out');
                }
            });
        });
    });
    // Навигация по периодам
    document.querySelectorAll('.period-btn').forEach(btn => {
        btn.addEventListener('click', function() {
            const period = this.getAttribute('data-period');
            let targetId = '';
            if (period === '1991-2000') targetId = 'period-1991-2000';
            else if (period === '2001-2011') targetId = 'period-2001-2011';
            else if (period === '2012-2022') targetId = 'period-2012-2022';
            if (targetId) document.getElementById(targetId).scrollIntoView({ behavior: 'smooth', block: 'start' });
        });
    });
</script>
</body>
</html>

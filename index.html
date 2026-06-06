<!DOCTYPE html>
<html lang="si">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI & Digital Evidence Analysis</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+Sinhala:wght@300;400;600;700&family=Poppins:wght@300;400;600;700&display=swap');

        body {
            font-family: 'Poppins', 'Noto Sans Sinhala', sans-serif;
            background-color: #050505;
            color: #ffffff;
            overflow-x: hidden;
            scroll-behavior: smooth;
        }

        #bg-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            z-index: -2; 
            pointer-events: none;
        }

        .glass-panel {
            background: rgba(15, 23, 42, 0.65);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(56, 189, 248, 0.2);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
        }

        .text-glow {
            text-shadow: 0 0 10px rgba(56, 189, 248, 0.8), 0 0 20px rgba(56, 189, 248, 0.5);
        }

        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #0f172a;
        }
        ::-webkit-scrollbar-thumb {
            background: #0ea5e9;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #38bdf8;
        }

        .gradient-text {
            background: linear-gradient(to right, #38bdf8, #818cf8, #c084fc);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 1rem;
        }
        th, td {
            padding: 1rem;
            border-bottom: 1px solid rgba(255,255,255,0.1);
            text-align: left;
        }
        th {
            background-color: rgba(56, 189, 248, 0.1);
            color: #38bdf8;
            font-weight: 600;
        }
        tr:hover {
            background-color: rgba(255,255,255,0.05);
        }

        mark.search-highlight {
            background-color: rgba(14, 165, 233, 0.6);
            color: #fff;
            padding: 2px 4px;
            border-radius: 4px;
            box-shadow: 0 0 8px rgba(14, 165, 233, 0.8);
            transition: all 0.3s ease;
        }
    </style>
</head>
<body class="antialiased">

    <!-- Bhasha pasand karva mate popup (Language Selection Popup on Load) -->
    <div id="language-popup" class="fixed inset-0 z-[10000] flex items-center justify-center bg-black/80 backdrop-blur-md transition-opacity duration-300">
        <div class="glass-panel p-8 md:p-12 rounded-3xl max-w-md w-full mx-4 text-center transform scale-100 transition-transform">
            <div class="w-16 h-16 mx-auto bg-sky-500/20 rounded-full flex items-center justify-center mb-6 border border-sky-500/50">
                <svg class="w-8 h-8 text-sky-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5h12M9 3v2m1.048 9.5A18.022 18.022 0 016.412 9m6.088 9h7M11 21l5-10 5 10M12.751 5C11.783 10.77 8.07 15.61 3 18.129"></path></svg>
            </div>
            <h2 class="text-2xl font-bold text-white mb-2">Select Language</h2>
            <p class="text-gray-400 text-sm mb-8">Please choose your preferred language to continue.</p>
            
            <div class="space-y-3">
                <button onclick="setInitialLanguage('en', 'English')" class="w-full py-3 px-4 rounded-xl border border-sky-500/40 text-white font-medium hover:bg-sky-500/20 transition-colors flex justify-between items-center group">
                    <span>English</span>
                    <span class="opacity-0 group-hover:opacity-100 transition-opacity">→</span>
                </button>
                <button onclick="setInitialLanguage('ru', 'Русский')" class="w-full py-3 px-4 rounded-xl border border-sky-500/40 text-white font-medium hover:bg-sky-500/20 transition-colors flex justify-between items-center group">
                    <span>Русский (Russian)</span>
                    <span class="opacity-0 group-hover:opacity-100 transition-opacity">→</span>
                </button>
                <button onclick="setInitialLanguage('si', 'සිංහල')" class="w-full py-3 px-4 rounded-xl border border-sky-500/40 text-white font-medium hover:bg-sky-500/20 transition-colors flex justify-between items-center group">
                    <span>සිංහල (Sinhala)</span>
                    <span class="opacity-0 group-hover:opacity-100 transition-opacity">→</span>
                </button>
            </div>
        </div>
    </div>

    <!-- 3D Background Canvas -->
    <canvas id="bg-canvas"></canvas>

    <!-- Navigation -->
    <nav class="fixed w-full z-50 glass-panel py-3 px-4 md:px-8 flex flex-wrap justify-between items-center transition-all gap-3">
        <div class="text-xl font-bold text-sky-400 tracking-wider flex-shrink-0 ai-text" data-en="AI <span class='text-white'>&amp;</span> FORENSICS" data-ru="ИИ <span class='text-white'>&amp;</span> ФОРЕНЗИКА">AI <span class="text-white">&amp;</span> FORENSICS</div>
        
        <!-- Desktop Nav Links -->
        <div class="hidden xl:flex space-x-6 text-sm font-semibold flex-grow justify-center px-4">
            <a href="#home" class="hover:text-sky-400 transition-colors ai-text" data-en="Home" data-ru="Главная">Home</a>
            <a href="#abstract" class="hover:text-sky-400 transition-colors ai-text" data-en="Abstract" data-ru="Аннотация">Abstract</a>
            <a href="#criteria" class="hover:text-sky-400 transition-colors ai-text" data-en="Criteria" data-ru="Критерии">Criteria</a>
            <a href="#framework" class="hover:text-sky-400 transition-colors ai-text" data-en="Frameworks" data-ru="Структуры">Frameworks</a>
            <a href="#problem" class="hover:text-sky-400 transition-colors ai-text" data-en="Problem" data-ru="Проблема">Problem</a>
            <a href="#design" class="hover:text-sky-400 transition-colors ai-text" data-en="Design" data-ru="Дизайн">Design</a>
        </div>

        <!-- Search & Translate Tools -->
        <div class="flex items-center space-x-4 w-full xl:w-auto justify-between xl:justify-end">
            <!-- Keyword Search Box -->
            <div class="relative flex-grow xl:flex-grow-0 max-w-[200px]">
                <input type="text" id="keyword-search" placeholder="Search keywords..." class="w-full bg-slate-800/80 border border-sky-500/40 rounded-full px-4 py-1.5 text-sm text-white placeholder-gray-400 focus:outline-none focus:border-sky-400 focus:ring-1 focus:ring-sky-400 transition-all">
                <svg class="w-4 h-4 absolute right-3 top-2 text-sky-400 pointer-events-none" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
            </div>
            
            <!-- Custom Offline Translate Button -->
            <div class="relative group z-50">
                <button class="glass-panel px-4 py-1.5 rounded-full flex items-center space-x-2 text-sm text-sky-300 border border-sky-500/40 hover:bg-sky-500/20 transition-all focus:outline-none cursor-pointer">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5h12M9 3v2m1.048 9.5A18.022 18.022 0 016.412 9m6.088 9h7M11 21l5-10 5 10M12.751 5C11.783 10.77 8.07 15.61 3 18.129"></path></svg>
                    <span id="current-lang">Translate</span>
                </button>
                <div class="absolute right-0 mt-2 w-36 glass-panel rounded-xl flex flex-col overflow-hidden opacity-0 invisible group-hover:opacity-100 group-hover:visible transition-all border border-slate-700">
                    <button onclick="translatePageOffline('si', 'සිංහල')" class="px-4 py-3 text-left hover:bg-sky-500/20 text-sm text-white border-b border-slate-700/50">සිංහල (Original)</button>
                    <button onclick="translatePageOffline('en', 'English')" class="px-4 py-3 text-left hover:bg-sky-500/20 text-sm text-white border-b border-slate-700/50">English</button>
                    <button onclick="translatePageOffline('ru', 'Русский')" class="px-4 py-3 text-left hover:bg-sky-500/20 text-sm text-white">Русский</button>
                </div>
            </div>
        </div>
    </nav>

    <!-- Hero Section -->
    <main id="home" class="relative min-h-screen flex flex-col justify-center items-center px-6 pt-24 text-center">
        <div class="animate-on-scroll max-w-5xl mx-auto z-10">
            <div class="inline-block py-1 px-3 rounded-full border border-sky-500/30 bg-sky-500/10 text-sky-300 text-sm md:text-base mb-6 font-medium ai-text" data-en="Research Proposal" data-ru="Исследовательское предложение">
                Research Proposal (පර්යේෂණ සැලැස්ම)
            </div>
            <h1 class="text-4xl md:text-6xl lg:text-7xl font-bold leading-tight mb-4 ai-text" data-en="Artificial Intelligence and <br /> <span class='gradient-text'>Digital Evidence Analysis</span>" data-ru="Искусственный интеллект и <br /> <span class='gradient-text'>Анализ цифровых доказательств</span>">
                Artificial Intelligence and <br />
                <span class="gradient-text">Digital Evidence Analysis</span>
            </h1>
            <h2 class="text-2xl md:text-4xl font-medium text-gray-300 mb-8 leading-snug search-content ai-text" data-en="Artificial Intelligence and Digital Evidence Analysis:<br/><span class='text-xl'>Research Proposal & Abstract</span>" data-ru="Искусственный интеллект и анализ цифровых доказательств:<br/><span class='text-xl'>Исследовательское предложение и аннотация</span>">
                කෘතිම බුද්ධිය සහ ඩිජිටල් සාක්ෂි විශ්ලේෂණය:<br/><span class="text-xl">පර්යේෂණ සැලැස්ම සහ සාරාංශය</span>
            </h2>
            
            <div class="glass-panel rounded-2xl p-6 md:p-8 inline-block mt-4 text-left border-t-4 border-t-sky-500 search-content">
                <p class="text-xl text-sky-200 font-semibold mb-1 ai-text" data-en="Jayasuriya Kuranage Shenol Ravisha Perera" data-ru="Джаясурия Куранаге Шенол Равиша Перера">Jayasuriya Kuranage Shenol Ravisha Perera</p>
                <p class="text-sm text-gray-400 mb-2 ai-text" data-en="Saint Petersburg State Agrarian University (SPSAU), Pushkin, Saint Petersburg, Russia." data-ru="Санкт-Петербургский государственный аграрный университет (СПбГАУ), Пушкин, Санкт-Петербург, Россия.">Saint Petersburg State Agrarian University (SPSAU), Pushkin, Saint Petersburg, Russia.</p>
                <div class="flex flex-wrap gap-2 items-center text-sm text-gray-400 mt-4 border-t border-gray-700/50 pt-4">
                    <span class="ai-text" data-en="Email: shenolperera5@gmail.com" data-ru="Email: shenolperera5@gmail.com">Email: shenolperera5@gmail.com</span>
                    <span class="hidden md:inline">|</span>
                    <span class="ai-text" data-en="ORCID: 0009-0006-9838-797X" data-ru="ORCID: 0009-0006-9838-797X">ORCID: 0009-0006-9838-797X</span>
                </div>
            </div>
        </div>

        <div class="absolute bottom-10 left-1/2 transform -translate-x-1/2 animate-bounce opacity-70">
            <svg class="w-6 h-6 text-sky-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3"></path>
            </svg>
        </div>
    </main>

    <!-- Abstract Section -->
    <section id="abstract" class="relative py-24 px-6 md:px-12 flex items-center">
        <div class="max-w-5xl mx-auto">
            <div class="animate-on-scroll text-center mb-12">
                <h3 class="text-sky-400 font-semibold text-lg mb-2 uppercase tracking-widest search-content ai-text" data-en="Research Abstract" data-ru="Аннотация исследования">Research Abstract</h3>
                <h2 class="text-3xl md:text-5xl font-bold text-glow search-content ai-text" data-en="Research Abstract" data-ru="Аннотация исследования">පර්යේෂණ සාරාංශය</h2>
            </div>
            <div class="glass-panel p-8 md:p-10 rounded-3xl space-y-6 text-gray-300 leading-relaxed text-base md:text-lg text-justify animate-on-scroll search-content">
                <p class="ai-text" data-en="Integrating Artificial Intelligence (AI) into Digital Forensics and criminal investigations has created a massive paradigm shift in the speed, scale, and accuracy of cybercrime investigations. Machine Learning systems are now essential to combat complex cyber attacks and financial fraud. However, the complex and opaque nature of these algorithms, known as the <strong>Black-box phenomenon</strong>, leads to an <strong>Explainability Deficit</strong>, which directly conflicts with established judicial standards regarding the reliability, authenticity, and Due Process of evidence. This research proposal proposes a comprehensive academic and practical investigation into the <strong>Admissibility</strong> of AI-generated digital evidence in court, specifically within the legal framework of Sri Lanka, based on the Evidence (Special Provisions) Act No. 14 of 1995 and the Electronic Transactions Act No. 19 of 2006. The rapid growth of cyber incidents reported to the Sri Lanka Computer Emergency Readiness Team (SLCERT) and the lack of specific statutory definitions for electronic evidence in criminal law provide a major motivation for this study." data-ru="Интеграция искусственного интеллекта (ИИ) в цифровую криминалистику и расследование уголовных преступлений привела к масштабному сдвигу парадигмы в скорости, масштабах и точности расследований киберпреступлений. Системы машинного обучения теперь необходимы для борьбы со сложными кибератаками и финансовым мошенничеством. Однако сложная и непрозрачная природа этих алгоритмов, известная как <strong>феномен черного ящика</strong>, приводит к <strong>дефициту объяснимости</strong>, что напрямую противоречит установленным судебным стандартам. В данном предложении предлагается всестороннее исследование <strong>допустимости</strong> цифровых доказательств, созданных ИИ, в суде, особенно в рамках правовой системы Шри-Ланки.">
                    Digital Forensics (ඩිජිටල් අධිකරණ වෛද්‍ය විද්‍යාව) සහ අපරාධ විමර්ශන ක්ෂේත්‍රය තුළට Artificial Intelligence (AI - කෘතිම බුද්ධිය) ඒකාබද්ධ කිරීම, සයිබර් අපරාධ විමර්ශනවල වේගය, පරිමාණය සහ නිරවද්‍යතාවය සම්බන්ධයෙන් දැවැන්ත පෙරළියක් ඇති කර ඇත. සංකීර්ණ සයිබර් ප්‍රහාර සහ මූල්‍ය වංචා මැඩපැවැත්වීම සඳහා Machine Learning (යන්ත්‍ර ඉගෙනුම්) පද්ධති භාවිතා කිරීම අත්‍යවශ්‍ය වී තිබේ. එහෙත්, මෙම ඇල්ගොරිතමවල පවතින සංකීර්ණ සහ අපැහැදිලි ස්වභාවය වන <strong>Black-box phenomenon (කළු පෙට්ටි සංසිද්ධිය)</strong> හේතුවෙන් පැන නගින <strong>Explainability Deficit (පැහැදිලි කිරීමේ ඌනතාවය)</strong>, සාක්ෂිවල විශ්වසනීයත්වය, සත්‍යතාව සහ Due Process (නිසි ක්‍රියා පටිපාටිය) පිළිබඳ ස්ථාපිත අධිකරණ ප්‍රමිතීන් සමඟ සෘජුවම ගැටෙයි. මෙම පර්යේෂණ සැලසුම මගින් ශ්‍රී ලංකාවේ නීතිමය රාමුව තුළ, විශේෂයෙන් 1995 අංක 14 දරන සාක්ෂි (විශේෂ විධිවිධාන) පනත සහ 2006 අංක 19 දරන විද්‍යුත් ගනුදෙනු පනත පදනම් කරගනිමින් AI මගින් ජනනය කරන ලද ඩිජිටල් සාක්ෂි අධිකරණය තුළ පිළිගැනීමේ හැකියාව හෙවත් <strong>Admissibility (පිළිගැනීමේ හැකියාව)</strong> පිළිබඳ පුළුල් ශාස්ත්‍රීය සහ ප්‍රායෝගික විමර්ශනයක් යෝජනා කරයි. ශ්‍රී ලංකා පරිගණක හදිසි ප්‍රතිචාර සංසදයට (SLCERT) වාර්තා වන සයිබර් සිදුවීම්වල ශීඝ්‍ර වර්ධනය සහ ඉලෙක්ට්‍රොනික සාක්ෂි සඳහා නිශ්චිත ව්‍යවස්ථාපිත අර්ථකථන අපරාධ නීතිය තුළ නොමැති වීම මෙම අධ්‍යයනය සඳහා ප්‍රධාන පෙළඹවීමක් සපයයි.
                </p>
                <p class="ai-text" data-en="Using theoretical frameworks such as the 'Cyber Exchange Principle', a digital evolution of Locard's exchange theory, and the qualitative dimensions of evidence reliability, this study evaluates how Explainable AI (XAI) bridges the gap between complex technology and judicial transparency. Specifically, the ability of XAI techniques like <strong>SHAP (SHapley Additive eXplanations)</strong> and <strong>LIME (Local Interpretable Model-Agnostic Explanations)</strong> to transform complex AI outputs into human-understandable and judicially admissible evidence will be investigated. Furthermore, this research validates methodologies to protect AI models from threats such as Data poisoning attacks in accordance with international standards like ISO/IEC 42001 and NIST." data-ru="Используя теоретические основы, такие как «Принцип киберобмена», цифровая эволюция теории обмена Локара, и качественные аспекты надежности доказательств, это исследование оценивает, как объяснимый ИИ (XAI) преодолевает разрыв между сложной технологией и судебной прозрачностью. В частности, будет исследована способность методов XAI, таких как <strong>SHAP</strong> и <strong>LIME</strong>, преобразовывать сложные результаты ИИ в понятные человеку доказательства.">
                    ලොකාඩ්ගේ හුවමාරු න්‍යායේ ඩිජිටල් පරිණාමයක් වන "සයිබර් හුවමාරු මූලධර්මය" සහ සාක්ෂිවල විශ්වසනීයත්වය පිළිබඳ ගුණාත්මක මානයන් යන න්‍යායාත්මක රාමු භාවිතා කරමින්, Explainable AI (XAI - පැහැදිලි කළ හැකි කෘතිම බුද්ධිය) මගින් සංකීර්ණ තාක්ෂණය සහ අධිකරණ විනිවිදභාවය අතර පරතරය පියවන ආකාරය මෙහිදී ඇගයීමට ලක් කෙරේ. විශේෂයෙන්ම, <strong>SHAP (SHapley Additive eXplanations)</strong> සහ <strong>LIME (Local Interpretable Model-Agnostic Explanations)</strong> වැනි XAI තාක්ෂණික ක්‍රම හරහා සංකීර්ණ AI ප්‍රතිදාන, මිනිසුන්ට අවබෝධ කරගත හැකි සහ අධිකරණයේදී පිළිගත හැකි සාක්ෂි බවට පරිවර්තනය කිරීමේ හැකියාව පර්යේෂණයට ලක් කෙරේ. තවද, මෙම පර්යේෂණය ISO/IEC 42001 සහ NIST වැනි ජාත්‍යන්තර ප්‍රමිතීන්ට අනුකූලව Data poisoning (දත්ත විකෘති කිරීමේ) ප්‍රහාර වැනි තර්ජනවලින් AI ආකෘති ආරක්ෂා කිරීම සඳහා වූ ක්‍රමවේදයන් ද වලංගු කරයි. අවසාන වශයෙන්, ශ්‍රී ලංකාවේ නීතිය ක්‍රියාත්මක කරන ආයතන, පැමිණිලි මෙහෙයවන්නන් සහ නීතිවේදීන්ගේ තාක්ෂණික සාක්ෂරතාවය සහ අධිකරණ ධාරිතාවය වර්ධනය කිරීම අරමුණු කරගත් <strong>XAI-CF මෙහෙයුම් සහ ප්‍රතිපත්තිමය රාමුවක්</strong> 2026 ජාතික පොලිස් අභ්‍යාස ආයතන පර්යේෂණ සමුළුවට ඉදිරිපත් කිරීම මෙහි ප්‍රධාන අපේක්ෂිත ප්‍රතිඵලයයි.
                </p>
            </div>
        </div>
    </section>

    <!-- Selection Criteria -->
    <section id="criteria" class="relative py-24 px-6 md:px-12 bg-gradient-to-b from-transparent to-slate-900/50">
        <div class="max-w-6xl mx-auto">
            <div class="animate-on-scroll mb-12">
                <h3 class="text-purple-400 font-semibold text-lg mb-2 uppercase tracking-widest search-content ai-text" data-en="Selection Criteria" data-ru="Критерии выбора">Selection Criteria</h3>
                <h2 class="text-3xl md:text-4xl font-bold text-white mb-6 search-content ai-text" data-en="Topic Selection Criteria" data-ru="Критерии выбора темы">මාතෘකාව තෝරාගැනීමේ නිර්ණායක</h2>
                <div class="glass-panel p-6 rounded-2xl mb-8 search-content">
                    <p class="text-gray-300 text-justify ai-text" data-en="This research is based on the intersection of AI, digital evidence analysis, and Admissibility in court due to the highly important technical, statistical, and legal needs for Sri Lanka and the global digital ecosystem." data-ru="Данное исследование основано на пересечении ИИ, анализа цифровых доказательств и их допустимости в суде ввиду исключительно важных потребностей для Шри-Ланки.">මෙම පර්යේෂණය AI, ඩිජිටල් සාක්ෂි විශ්ලේෂණය සහ අධිකරණයේදී Admissibility (පිළිගැනීමේ හැකියාව) යන ක්ෂේත්‍රයන්හි සන්ධිස්ථානය මත පදනම් වන්නේ, ශ්‍රී ලංකාවට සහ ගෝලීය ඩිජිටල් පරිසර පද්ධතියට අතිශයින් වැදගත් වන තාක්ෂණික, සංඛ්‍යානමය සහ නෛතික අවශ්‍යතා හේතුවෙනි. නවීන සයිබර් විමර්ශකයන්ගේ හැකියාවන් සහ අධිකරණයේ දැඩි ක්‍රියාපටිපාටිමය සීමාවන් අතර පවතින පුළුල් හිඩැස පියවීම මෙහි ප්‍රධාන අරමුණයි.</p>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 animate-on-scroll search-content">
                <div class="glass-panel p-6 rounded-2xl border-t-4 border-t-blue-500">
                    <h4 class="font-bold text-xl mb-3 text-sky-300 ai-text" data-en="Data Scale Explosion" data-ru="Взрыв масштабов данных">දත්ත පරිමාණයේ පුපුරා යාම</h4>
                    <p class="text-sm text-gray-400 text-justify ai-text" data-en="Due to the massive increase in digital data, automated algorithmic categorization has become essential." data-ru="Из-за массового увеличения цифровых данных автоматизированная алгоритмическая категоризация стала необходимой.">ඩිජිටල් දත්ත විශාල වශයෙන් වැඩිවීම හේතුවෙන් ස්වයංක්‍රීය ඇල්ගොරිතම වර්ගීකරණය අත්‍යවශ්‍ය වී ඇති අතර, විශාල පරිමාණයේ විමර්ශනවලදී සාම්ප්‍රදායික අතින් සිදුකරන අධිකරණ වෛද්‍ය විද්‍යාව අභිබවා ගොස් ඇත. ශ්‍රී ලංකාවේ අන්තර්ජාලය භාවිතා කරන්නන් මිලියන 11.34 ක්, සක්‍රීය සමාජ මාධ්‍ය භාවිතා කරන්නන් මිලියන 8.20 ක් සිටීම මෙයට උදාහරණයකි.</p>
                </div>
                <div class="glass-panel p-6 rounded-2xl border-t-4 border-t-pink-500">
                    <h4 class="font-bold text-xl mb-3 text-pink-300 ai-text" data-en="Rapid Growth of Cyber Incidents" data-ru="Быстрый рост киберинцидентов">සයිබර් සිදුවීම්වල ශීඝ්‍ර වර්ධනය</h4>
                    <p class="text-sm text-gray-400 text-justify ai-text" data-en="The number of cybersecurity incidents reported has increased significantly. AI allows rapid analysis of massive datasets." data-ru="Количество зарегистрированных инцидентов кибербезопасности значительно возросло.">ශ්‍රී ලංකා පරිගණක හදිසි ප්‍රතිචාර සංසදයට (SLCERT) වාර්තා වූ සයිබර් ආරක්ෂණ සිදුවීම් සංඛ්‍යාව 2019 දී 3,566 සිට 2022 වන විට 16,376 දක්වා ඉහළ ගොස් ඇත. අපරාධ පරීක්ෂණ දෙපාර්තමේන්තුව (CID) වාර්ෂිකව සංකීර්ණ පැමිණිලි 2,500 කට අධික ප්‍රමාණයක් ලබා ගන්නා අතර, AI මඟින් මිනිත්තු කිහිපයකින් විශාල දත්ත කට්ටල විශ්ලේෂණය කර, ක්ෂුද්‍ර සාක්ෂි හඳුනා ගැනීමට හැකිවේ.</p>
                </div>
                <div class="glass-panel p-6 rounded-2xl border-t-4 border-t-purple-500">
                    <h4 class="font-bold text-xl mb-3 text-purple-300 ai-text" data-en="Crucial Legal Necessity" data-ru="Важнейшая правовая необходимость">අත්‍යවශ්‍ය නීතිමය අවශ්‍යතාවය</h4>
                    <p class="text-sm text-gray-400 text-justify ai-text" data-en="Outputs from deep learning models clash with legal demands for transparency and scientific reliability in court." data-ru="Результаты моделей глубокого обучения противоречат правовым требованиям прозрачности в суде.">ගැඹුරු ඉගෙනුම් ආකෘතිවල ප්‍රතිදාන අධිකරණයේ විනිවිදභාවය, හරස් ප්‍රශ්න කිරීමේ හැකියාව සහ විද්‍යාත්මක විශ්වසනීයත්වය පිළිබඳ නීතිමය ඉල්ලීම් සමඟ ගැටේ. බුද්ධිමය දේපළ නීති මගින් ආරක්ෂා කර ඇති Black-box (අපැහැදිලි) ඇල්ගොරිතම තීරණයක් නීතිඥයෙකුට හරස් ප්‍රශ්න කළ හැක්කේ කෙසේද යන්න මෙම අධ්‍යයනය මගින් අවධානය යොමු කරයි.</p>
                </div>
            </div>

            <div class="mt-12 overflow-x-auto glass-panel rounded-2xl animate-on-scroll search-content">
                <table>
                    <thead>
                        <tr>
                            <th class="ai-text" data-en="Incident Classification" data-ru="Классификация инцидентов">සිදුවීම් වර්ගීකරණය</th>
                            <th class="ai-text" data-en="Context in Sri Lanka" data-ru="Контекст в Шри-Ланке">ශ්‍රී ලංකාවේ සංඛ්‍යාතය/බරපතලකම පිළිබඳ සන්දර්භය</th>
                            <th class="ai-text" data-en="Implications for Forensic Analysis" data-ru="Последствия для криминалистического анализа">අධිකරණ වෛද්‍ය විශ්ලේෂණය සඳහා ඇති ඇඟවුම්</th>
                        </tr>
                    </thead>
                    <tbody class="text-sm">
                        <tr>
                            <td class="font-semibold text-sky-200 ai-text" data-en="Social Media Incidents" data-ru="Инциденты в социальных сетях">සමාජ මාධ්‍ය සිදුවීම්</td>
                            <td class="ai-text" data-en="Represents the highest volume of reported incidents including cyberbullying." data-ru="Представляет наибольший объем зарегистрированных инцидентов.">සයිබර් හිරිහැර කිරීම්, මනහානි කිරීම් සහ සංවිධානාත්මක හිරිහැර ඇතුළුව වාර්තා වන ඉහළම සිදුවීම් ප්‍රමාණය නියෝජනය කරයි.</td>
                            <td class="ai-text" data-en="Requires Natural Language Processing (NLP) to analyze sentiment and deepfake authenticity." data-ru="Требует обработки естественного языка (NLP) для анализа настроений.">හැඟීම්, ආරම්භය සහ ඩීප්ෆේක් මාධ්‍යවල සත්‍යතාව විශ්ලේෂණය කිරීම සඳහා ස්වභාවික භාෂා සැකසීම (NLP) අවශ්‍ය වේ.</td>
                        </tr>
                        <tr>
                            <td class="font-semibold text-sky-200 ai-text" data-en="Ransomware & Malware" data-ru="Программы-вымогатели и вредоносное ПО">රැන්සම්වෙයා සහ අනිෂ්ට මෘදුකාංග</td>
                            <td class="ai-text" data-en="Increasingly damaging critical national infrastructure and corporate networks." data-ru="Все больше наносят ущерб критически важной национальной инфраструктуре.">තීරණාත්මක ජාතික යටිතල පහසුකම් සහ දේශීය ආයතනික ජාල වලට වැඩි වැඩියෙන් හානි කරයි.</td>
                            <td class="ai-text" data-en="Advanced behavioral heuristics and AI pattern recognition are needed." data-ru="Требуются передовые поведенческие эвристики и распознавание образов с помощью ИИ.">වේගයෙන් විකෘති වන ගුප්ත ලේඛන හඳුනා ගැනීම සඳහා උසස් චර්යාත්මක හියුරිස්ටික්ස් (heuristics) සහ AI රටා හඳුනාගැනීම අවශ්‍ය වේ.</td>
                        </tr>
                        <tr>
                            <td class="font-semibold text-sky-200 ai-text" data-en="Financial Fraud / BEC" data-ru="Финансовое мошенничество / BEC">මූල්‍ය වංචා / BEC</td>
                            <td class="ai-text" data-en="Growing rapidly due to the expansion of digital banking and e-commerce." data-ru="Быстро растет из-за расширения цифрового банкинга.">දිවයින පුරා ඩිජිටල් බැංකුකරණය සහ ඊ-වාණිජ්‍යය ශීඝ්‍රයෙන් ව්‍යාප්ත වීම හේතුවෙන් වැඩිවෙමින් පවතී.</td>
                            <td class="ai-text" data-en="Requires rapid tracing of decentralized finance (DeFi) ledgers and IP routing." data-ru="Требует быстрого отслеживания реестров децентрализованных финансов.">විමධ්‍යගත මූල්‍ය (DeFi) ලෙජර සහ සංකීර්ණ, බහු-අධිකරණ බල ප්‍රදේශ IP මාර්ගගත කිරීම් ඉක්මනින් සොයා ගැනීම අවශ්‍ය වේ.</td>
                        </tr>
                        <tr>
                            <td class="font-semibold text-sky-200 ai-text" data-en="Website & Server Compromise" data-ru="Компрометация веб-сайтов и серверов">වෙබ් අඩවි සහ සේවාදායක සම්මුතිය</td>
                            <td class="ai-text" data-en="Predictive data exfiltration and server hijacking." data-ru="Предиктивная эксфильтрация данных и взлом серверов.">අනාවැකි දත්ත exfiltration සහ DDoS ප්‍රොටෝකෝල හරහා සේවාදායක පැහැරගැනීම්.</td>
                            <td class="ai-text" data-en="AI is required to instantly detect micro-deviations in operational parameters." data-ru="ИИ требуется для мгновенного обнаружения микроотклонений.">මෙහෙයුම් පරාමිතීන්ගෙන් ඇති ක්ෂුද්‍ර අපගමනයන් ක්ෂණිකව හඳුනා ගැනීමට AI අවශ්‍ය වේ.</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </section>

    <!-- Remaining sections mapped accurately using offline translation data tags -->
    <section id="rationale" class="relative py-24 px-6 md:px-12">
        <div class="max-w-6xl mx-auto grid grid-cols-1 lg:grid-cols-2 gap-12">
            <div class="animate-on-scroll search-content">
                <h3 class="text-sky-400 font-semibold text-lg mb-2 uppercase tracking-widest ai-text" data-en="Research Rationale" data-ru="Обоснование исследования">Research Rationale</h3>
                <h2 class="text-3xl font-bold text-white mb-6 ai-text" data-en="Research Rationale" data-ru="Обоснование исследования">පර්යේෂණයේ තාර්කිකත්වය</h2>
                <p class="text-gray-300 mb-6 ai-text" data-en="The fundamental rationale behind this study is the inherent dual-use dilemma of artificial intelligence in high legal and national security environments." data-ru="Фундаментальным обоснованием данного исследования является дилемма двойного назначения ИИ.">මෙම අධ්‍යයනයට පාදක වන මූලික තාර්කිකත්වය වන්නේ ඉහළ නීතිමය සහ ජාතික ආරක්ෂක පරිසරයන් තුළ කෘතිම බුද්ධියේ ආවේණික "ද්විත්ව භාවිත" උභතෝකෝටිකයයි.</p>
                <div class="space-y-4">
                    <div class="glass-panel p-5 rounded-xl border-l-4 border-l-sky-500">
                        <strong class="text-sky-300 block mb-1 ai-text" data-en="Explainability Deficit:" data-ru="Дефицит объяснимости:">Explainability Deficit (පැහැදිලි කිරීමේ ඌනතාවය):</strong>
                        <p class="text-sm text-gray-400 ai-text" data-en="Traditional forensic evidence relies on human experts who can be cross-examined. Deep learning models, however, act as opaque Black-boxes." data-ru="Традиционные криминалистические доказательства опираются на экспертов, которых можно перекрестно допросить. Однако модели глубокого обучения действуют как черные ящики.">DNA අනුක්‍රමණය හෝ ඇඟිලි සලකුණු ගැලපීම වැනි සාම්ප්‍රදායික අධිකරණ වෛද්‍ය සාක්ෂි, විවෘතව විමර්ශනය කළ හැකි මිනිස් විශේෂඥයින් මත රඳා පවතී. නමුත් ගැඹුරු ඉගෙනුම් ආකෘති අපැහැදිලි Black-boxes (කළු පෙට්ටි) ලෙස ක්‍රියා කරයි. ඇල්ගොරිතම තීරණයකට පැමිණීමට භාවිතා කළ නිශ්චිත විචල්‍යයන් විනිසුරුවරුන්, නීතිඥයින් හෝ ක්‍රියාකරුවන් විසින් තේරුම් නොගැනීම විද්‍යාත්මක නීත්‍යානුකූලභාවය නැති කර දමයි.</p>
                    </div>
                    <div class="glass-panel p-5 rounded-xl border-l-4 border-l-purple-500">
                        <strong class="text-purple-300 block mb-1 ai-text" data-en="Model Drift & Bias:" data-ru="Дрейф модели и предвзятость:">Model Drift (ආකෘති අපගමනය) සහ පක්ෂග්‍රාහීත්වය:</strong>
                        <p class="text-sm text-gray-400 ai-text" data-en="Strict scientific acceptance frameworks demand testability, peer review, and error rates. AI models challenge these standards." data-ru="Строгие научные рамки требуют проверяемости, экспертной оценки и учета уровня ошибок.">ඇමරිකානු ෆෙඩරල් අධිකරණවල භාවිතා වන Daubert ප්‍රමිතිය වැනි දැඩි විද්‍යාත්මක පිළිගැනීමේ රාමු, පරීක්ෂා කිරීමේ හැකියාව, සම-සමාලෝචනය සහ දෝෂ අනුපාත ඉල්ලා සිටියි. AI ආකෘති, කාලයත් සමඟ පුරෝකථන නිරවද්‍යතාවය ක්‍රමානුකූලව අඩු වන Model Drift වැනි සංසිද්ධි හේතුවෙන් මෙම ප්‍රමිතීන්ට අභියෝග කරයි.</p>
                    </div>
                    <div class="glass-panel p-5 rounded-xl border-l-4 border-l-pink-500">
                        <strong class="text-pink-300 block mb-1 ai-text" data-en="Sri Lankan Context:" data-ru="Контекст Шри-Ланки:">ශ්‍රී ලාංකේය සන්දර්භය:</strong>
                        <p class="text-sm text-gray-400 ai-text" data-en="Despite training over 200 judges, technical literacy limits remain, causing variability in the acceptance of machine-generated outputs." data-ru="Несмотря на обучение более 200 судей, остаются ограничения в технической грамотности.">ශ්‍රී ලංකා විනිසුරුවරුන්ගේ ආයතනය (SLJI) මගින් විනිසුරුවරුන් 200 කට වැඩි පිරිසක් පුහුණු කළද, තාක්ෂණික සාක්ෂරතාවයේ සීමාවන් හේතුවෙන් යන්ත්‍ර ජනනය කරන ලද ප්‍රතිදාන පිළිගැනීමේදී විචල්‍යතාවයක් පවතී. වැරදි ලෙස වරදකරුවන් වීම වැළැක්වීම සහ නීතියේ ආධිපත්‍යය ආරක්ෂා කිරීම සඳහා XAI කේන්ද්‍රීය අධිකරණ වෛද්‍ය පද්ධතියක් ස්ථාපිත කිරීම අත්‍යවශ්‍ය වේ.</p>
                    </div>
                </div>
            </div>

            <div class="animate-on-scroll search-content">
                <h3 class="text-purple-400 font-semibold text-lg mb-2 uppercase tracking-widest ai-text" data-en="Literature Review Strategy" data-ru="Стратегия обзора литературы">Literature Review Strategy</h3>
                <h2 class="text-3xl font-bold text-white mb-6 ai-text" data-en="Literature Review Strategy" data-ru="Стратегия обзора литературы">සාහිත්‍ය සෙවීමේ උපාය මාර්ගය</h2>
                <div class="glass-panel p-6 rounded-2xl mb-6">
                    <p class="text-sm text-gray-300 mb-4 text-justify ai-text" data-en="To conduct a comprehensive, empirical, and reproducible review, this research uses a structured search strategy adhering strictly to the PRISMA procedure." data-ru="Для проведения всестороннего и воспроизводимого обзора в данном исследовании используется структурированная стратегия поиска.">පවතින දැනුම් සම්භාරය පිළිබඳ පුළුල්, ආනුභවික සහ පුනරාවර්තනය කළ හැකි සමාලෝචනයක් සිදු කිරීම සඳහා, මෙම පර්යේෂණය PRISMA ක්‍රියා පටිපාටියට දැඩි ලෙස අනුකූලව ව්‍යුහගත සෙවුම් උපාය මාර්ගයක් භාවිතා කරයි.</p>
                    <ul class="list-disc list-inside text-sm text-gray-400 space-y-2">
                        <li class="ai-text" data-en="Priority Sources: IEEE Xplore, Scopus, arXiv, SpringerLink, NIST, ISO Publications." data-ru="Приоритетные источники: IEEE Xplore, Scopus, arXiv, SpringerLink, публикации NIST, ISO."><strong class="text-gray-200">ප්‍රමුඛතා ප්‍රභවයන්:</strong> IEEE Xplore, Scopus, arXiv, SpringerLink, NIST, ISO ප්‍රකාශන.</li>
                        <li class="ai-text" data-en="Search Scope: Strict Boolean search string architecture." data-ru="Область поиска: Строгая архитектура логических строк поиска."><strong class="text-gray-200">සෙවීමේ විෂය පථය:</strong> දැඩි බූලියන් සෙවුම් තන්තු ගෘහ නිර්මාණ ශිල්පය.</li>
                        <li class="ai-text" data-en="Timeframe: Recent academic articles published between 2018 and 2026." data-ru="Временные рамки: Недавние научные статьи, опубликованные в период с 2018 по 2026 год."><strong class="text-gray-200">කාල රාමුව:</strong> 2018 සහ 2026 අතර ප්‍රකාශිත නවතම ශාස්ත්‍රීය ලිපි.</li>
                    </ul>
                </div>
                <div class="overflow-x-auto glass-panel rounded-2xl">
                    <table class="text-xs">
                        <thead>
                            <tr>
                                <th class="ai-text" data-en="Multidisciplinary Subject Area" data-ru="Мультидисциплинарная предметная область">බහුවිධ විෂය ක්ෂේත්‍රය</th>
                                <th class="ai-text" data-en="Core Keywords / Boolean Search Strings" data-ru="Основные ключевые слова / Строки поиска">මූලික යතුරු පද / බූලියන් සෙවුම් තන්තු</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td class="font-semibold text-sky-200 ai-text" data-en="Algorithmic Architecture & AI" data-ru="Алгоритмическая архитектура и ИИ">ඇල්ගොරිතම ගෘහ නිර්මාණ ශිල්පය සහ AI</td>
                                <td class="font-mono text-gray-400 ai-text" data-en='("Artificial Intelligence" OR "Deep Learning") AND ("Explainability" OR "XAI")' data-ru='("Artificial Intelligence" OR "Deep Learning") AND ("Explainability" OR "XAI")'>("Artificial Intelligence" OR "Deep Learning") AND ("Explainability" OR "XAI" OR "SHAP" OR "LIME")</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-200 ai-text" data-en="Digital Forensics" data-ru="Цифровая криминалистика">ඩිජිටල් සාක්ෂි සහ අධිකරණ වෛද්‍ය විද්‍යාව</td>
                                <td class="font-mono text-gray-400 ai-text" data-en='("Digital Forensics" OR "Cyber Forensics")' data-ru='("Digital Forensics" OR "Cyber Forensics")'>("Digital Forensics" OR "Cyber Forensics") AND ("AI" OR "Machine Learning") AND ("Incident Response")</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-200 ai-text" data-en="Legal Admissibility" data-ru="Правовая допустимость">නීතිමය පිළිගැනීමේ හැකියාව</td>
                                <td class="font-mono text-gray-400 ai-text" data-en='("Admissibility" OR "Daubert")' data-ru='("Admissibility" OR "Daubert")'>("Admissibility" OR "Daubert") AND ("AI-generated evidence" OR "Black-box" OR "Algorithmic Bias")</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-200 ai-text" data-en="Sri Lankan Legal Context" data-ru="Правовой контекст Шри-Ланки">ශ්‍රී ලාංකේය නෛතික සන්දර්භය</td>
                                <td class="font-mono text-gray-400 ai-text" data-en='("Sri Lanka" AND "Electronic Evidence")' data-ru='("Sri Lanka" AND "Electronic Evidence")'>("Sri Lanka" AND "Electronic Evidence") OR ("Evidence Special Provisions Act" OR "Electronic Transactions Act")</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </section>

    <section id="framework" class="relative min-h-screen py-24 px-6 md:px-12 bg-gradient-to-t from-slate-900/80 to-transparent">
        <div class="max-w-6xl mx-auto text-center mb-16 animate-on-scroll search-content">
            <h3 class="text-purple-400 font-semibold text-lg mb-2 uppercase tracking-widest ai-text" data-en="Core Paradigms" data-ru="Основные парадигмы">Core Paradigms</h3>
            <h2 class="text-3xl md:text-5xl font-bold mb-4 ai-text" data-en="Theoretical Frameworks" data-ru="Теоретические основы">Theoretical Frameworks (න්‍යායාත්මක රාමු)</h2>
            <p class="text-gray-400 max-w-2xl mx-auto ai-text" data-en="This research is based on three core theoretical frameworks." data-ru="Данное исследование основано на трех основных теоретических концепциях.">මෙම පර්යේෂණය මූලික න්‍යායාත්මක රාමු තුනක් මත පදනම් වේ.</p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-8 search-content">
            <div class="glass-panel p-8 rounded-3xl animate-on-scroll group hover:bg-slate-800/50 transition-all">
                <div class="w-14 h-14 rounded-full bg-blue-500/20 flex items-center justify-center mb-6 border border-blue-500/50 group-hover:scale-110 transition-transform">
                    <span class="text-xl">1</span>
                </div>
                <h4 class="text-xl font-bold mb-3 text-white ai-text" data-en="The Cyber Exchange Principle" data-ru="Принцип киберобмена">The Cyber Exchange Principle</h4>
                <p class="text-sm text-sky-300 mb-4 font-semibold ai-text" data-en="Locard's Digital Extension" data-ru="Цифровое расширение Локара">සයිබර් හුවමාරු මූලධර්මය (ලොකාඩ්ගේ ඩිජිටල් ව්‍යාප්තිය)</p>
                <div class="text-sm text-gray-400 leading-relaxed text-justify space-y-2">
                    <p class="ai-text" data-en="Core Concept: Digital extension of Locard's Exchange Principle ('Every contact leaves a trace')." data-ru="Основная концепция: Цифровое расширение Принципа обмена Локара."><strong>මූලික සංකල්පය:</strong> අපරාධකරුවෙකු භෞතික අපරාධ ස්ථානයක් සමඟ සම්බන්ධ වන සෑම අවස්ථාවකදීම යමක් එහි තබා යන අතර එයින් යමක් රැගෙන යයි ("Every contact leaves a trace") යන ලොකාඩ්ගේ හුවමාරු මූලධර්මයේ (Locard's Exchange Principle) ඩිජිටල් දිගුව මෙයයි.</p>
                    <p class="ai-text" data-en="Digital Challenge: As digital evidence is volatile, an AI tool must find these micro data exchanges without breaking the chain of custody." data-ru="Цифровой вызов: ИИ должен находить микрообмены данными, не нарушая цепочку поставок цифровых улик."><strong>ඩිජිටල් අභියෝගය:</strong> ඩිජිටල් සාක්ෂි ස්කන්ධයෙන් විශාල වන අතර, ඉතා ඉක්මනින් අතුරුදහන් වන (volatile) බැවින්, AI අධිකරණ වෛද්‍ය මෙවලමක් මඟින් මෙම ක්ෂුද්‍ර දත්ත හුවමාරු කිරීම් (උදා: පැකට් ශීර්ෂ වෙනස් කිරීම්) සොයාගත යුත්තේ ඩිජිටල් භාරකාර දාමය කඩ නොවන ලෙසිනි.</p>
                </div>
            </div>

            <div class="glass-panel p-8 rounded-3xl animate-on-scroll group hover:bg-slate-800/50 transition-all delay-100">
                <div class="w-14 h-14 rounded-full bg-purple-500/20 flex items-center justify-center mb-6 border border-purple-500/50 group-hover:scale-110 transition-transform">
                    <span class="text-xl">2</span>
                </div>
                <h4 class="text-xl font-bold mb-3 text-white ai-text" data-en="Qualitative Dimensions of Judicial Trustworthiness" data-ru="Качественные аспекты судебной надежности">Qualitative Dimensions of Judicial Trustworthiness</h4>
                <p class="text-sm text-purple-300 mb-4 font-semibold ai-text" data-en="Judicial Trustworthiness" data-ru="Судебная надежность">අධිකරණ විශ්වසනීයත්වයේ ගුණාත්මක මානයන්</p>
                <div class="text-sm text-gray-400 leading-relaxed text-justify space-y-2">
                    <p class="ai-text" data-en="Trustworthiness is essential for evidence to be admissible in court." data-ru="Надежность имеет важное значение для того, чтобы доказательства были допустимы в суде.">අධිකරණ ක්‍රියාවලියේදී සාක්ෂි පිළිගත හැකි වීමට "විශ්වසනීයත්වය" (Trustworthiness) අත්‍යවශ්‍ය වේ. ඉලෙක්ට්‍රොනික සාක්ෂි සම්බන්ධයෙන් මෙය ප්‍රධාන මාන දෙකකට බෙදේ:</p>
                    <p class="ai-text" data-en="Reliability: Ensuring the scientific accuracy of an AI categorization." data-ru="Надежность: Обеспечение научной точности категоризации ИИ."><strong>විශ්වසනීයත්වය (Reliability):</strong> ඩිජිටල් වාර්තාවෙන් එය සහතික කරන කරුණු නිවැරදිව හා ස්ථාවරව නිරූපණය කළ හැකි බව අධිකරණයට තහවුරු කළ යුතුය. AI වර්ගීකරණයක විද්‍යාත්මක නිරවද්‍යතාවය මැනීම.</p>
                    <p class="ai-text" data-en="Authenticity: Proving the algorithm's training data has not been tampered with." data-ru="Подлинность: Доказательство того, что обучающие данные алгоритма не были изменены."><strong>සත්‍යතාව (Authenticity):</strong> සාක්ෂි හැසිරවීමෙන් තොරව, අඛණ්ඩ භාරකාර දාමයක් හරහා ඩිජිටල් වාර්තාව එහි මුල් ස්වභාවයෙන්ම පවතී බව ඔප්පු කළ යුතුය. ඇල්ගොරිතමයේ පුහුණු දත්ත විකෘති වී නොමැති බව තහවුරු කිරීම මෙයට අයත් වේ.</p>
                </div>
            </div>

            <div class="glass-panel p-8 rounded-3xl animate-on-scroll group hover:bg-slate-800/50 transition-all delay-200">
                <div class="w-14 h-14 rounded-full bg-pink-500/20 flex items-center justify-center mb-6 border border-pink-500/50 group-hover:scale-110 transition-transform">
                    <span class="text-xl">3</span>
                </div>
                <h4 class="text-xl font-bold mb-3 text-white ai-text" data-en="Explainable AI (XAI) Paradigms" data-ru="Парадигмы объяснимого ИИ (XAI)">Explainable AI (XAI) Paradigms</h4>
                <p class="text-sm text-pink-300 mb-4 font-semibold ai-text" data-en="Explainable Artificial Intelligence Paradigms" data-ru="Объяснимые парадигмы искусственного интеллекта">පැහැදිලි කළ හැකි කෘතිම බුද්ධි ආදර්ශ</p>
                <div class="text-sm text-gray-400 leading-relaxed text-justify space-y-2">
                    <p class="ai-text" data-en="XAI frameworks are used to bridge the gap between Black-box complexity and judicial understanding." data-ru="Среды XAI используются для преодоления разрыва между сложностью черного ящика и судебным пониманием.">Black-box (කළු පෙට්ටියේ) සංකීර්ණත්වය සහ අධිකරණයේ අවබෝධය අතර පරතරය පියවීම සඳහා XAI රාමු භාවිතා කෙරේ. මෙහි ප්‍රමුඛ ආදර්ශ දෙකක් අධ්‍යයනය කෙරේ:</p>
                    <p class="ai-text" data-en="SHAP: Calculates the exact contribution of each feature to provide global understanding." data-ru="SHAP: Вычисляет точный вклад каждой функции для обеспечения глобального понимания."><strong>SHAP (SHapley Additive eXplanations):</strong> සමුපකාර ක්‍රීඩා න්‍යාය මත පදනම් වූ SHAP මඟින් සමස්ත යන්ත්‍ර ඉගෙනුම් ආකෘතිය හරහා එක් එක් ලක්ෂණයේ නියම දායකත්වය ගණනය කිරීමෙන් ගෝලීය අවබෝධය සපයයි. AI තීරණයකට එළඹීමට බලපෑ ප්‍රධාන සාධක මොනවාදැයි මෙයින් පැහැදිලි කෙරේ.</p>
                    <p class="ai-text" data-en="LIME: Provides local, case-by-case explanations by changing input data." data-ru="LIME: Предоставляет локальные объяснения в каждом конкретном случае путем изменения входных данных."><strong>LIME (Local Interpretable Model-Agnostic Explanations):</strong> LIME මඟින් දේශීය, සිද්ධියෙන් සිද්ධියට පැහැදිලි කිරීම් සපයයි. තනි සිදුවීමක ආදාන දත්ත වෙනස් කිරීමෙන් අනාවැකි වෙනස් වන ආකාරය නිරීක්ෂණය කරන අතර, එය අධිකරණයට තනි අනතුරු ඇඟවීමක් ජනනය වූයේ මන්දැයි පැහැදිලි කිරීමට උපකාරී වේ.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Additional sections updated with data-en and data-ru to maintain offline robust matching -->
    <section id="problem" class="relative py-24 px-6 md:px-12 search-content">
        <div class="max-w-6xl mx-auto space-y-16">
            <div class="glass-panel p-8 md:p-12 rounded-3xl animate-on-scroll relative overflow-hidden">
                <div class="absolute top-0 right-0 w-64 h-64 bg-sky-500/10 rounded-full blur-3xl -z-10 translate-x-1/2 -translate-y-1/2"></div>
                <h3 class="text-2xl md:text-3xl font-bold mb-6 text-white text-glow ai-text" data-en="Gap Identification" data-ru="Выявление пробелов">Gap Identification (පැහැදිලි හිඩැස් හඳුනාගැනීම)</h3>
                <p class="text-gray-300 mb-6 text-justify ai-text" data-en="Despite the theoretical alignment between AI and digital forensics, severe gaps remain, particularly within the Sri Lankan legal ecosystem." data-ru="Несмотря на теоретическое согласование между ИИ и цифровой криминалистикой, остаются серьезные пробелы.">කෘතිම බුද්ධිය සහ ඩිජිටල් අධිකරණ වෛද්‍ය විද්‍යාව අතර ඇති න්‍යායාත්මක ගැලපීම තිබියදීත්, විශේෂයෙන් ශ්‍රී ලංකාවේ නීතිමය පරිසර පද්ධතිය තුළ බරපතල හිඩැස් පවතී.</p>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8 text-sm md:text-base text-gray-300">
                    <div class="space-y-4">
                        <h4 class="font-bold text-sky-400 border-b border-gray-700 pb-2 ai-text" data-en="Domestic Statutory Vacuum & Obsolescence" data-ru="Внутренний правовой вакуум и устаревание">දේශීය ව්‍යවස්ථාපිත හිස් අවකාශය සහ යල්පැනීම</h4>
                        <p class="ai-text" data-en="Outdated Statutes: The 1995 and 2006 Acts lack modern legal definitions for electronic evidence or AI." data-ru="Устаревшие законы: В законах 1995 и 2006 годов отсутствуют современные правовые определения электронных доказательств."><strong>යල්පැන ගිය ව්‍යවස්ථා:</strong> 1995 අංක 14 දරන සාක්ෂි (විශේෂ විධිවිධාන) පනත සහ 2006 අංක 19 දරන විද්‍යුත් ගනුදෙනු පනත, ගැඹුරු ඉගෙනුම් ජාල සහ AI පැමිණීමට පෙර කෙටුම්පත් කර ඇති බැවින්, "ඉලෙක්ට්‍රොනික සාක්ෂි" හෝ "කෘතිම බුද්ධිය" සඳහා නූතන නෛතික අර්ථකථනයක් නොමැත.</p>
                        <p class="ai-text" data-en="Proper Operation Issue: Proving a self-learning AI model 'operates properly' requires data auditing, which is not covered by the 1995 Act." data-ru="Проблема надлежащей работы: Доказательство «надлежащей работы» самообучающейся модели ИИ требует аудита данных."><strong>නිසි ක්‍රියාකාරීත්වය පිළිබඳ ගැටළුව:</strong> 1995 පනතේ 5 වගන්තිය පරිගණකය "නිසි ලෙස ක්‍රියාත්මක වන බව" ඔප්පු කිරීම අවශ්‍ය වේ. අද දින ස්වයං-ඉගෙනුම් AI ආකෘතියක් "නිසි ලෙස ක්‍රියාත්මක වන බව" ඔප්පු කිරීමට දත්ත විගණනය, පක්ෂග්‍රාහීත්වය හඳුනාගැනීම සහ ආකෘති අපගමනයට එරෙහි වලංගුකරණය අවශ්‍ය වේ. මෙය 1995 පනතට අනුකූල නොවේ.</p>
                    </div>
                    <div class="space-y-4">
                        <h4 class="font-bold text-purple-400 border-b border-gray-700 pb-2 ai-text" data-en="Algorithmic Due Process Disconnect" data-ru="Отключение алгоритмической надлежащей правовой процедуры">Algorithmic Due Process Disconnect</h4>
                        <p class="ai-text" data-en="Burden of Proof: Shifting the burden to the defendant to prove an algorithm wrong violates fundamental rights to a fair trial." data-ru="Бремя доказывания: Перенос на обвиняемого бремени доказывания ошибочности алгоритма нарушает основные права."><strong>සාක්ෂි භාරය:</strong> 2006 අංක 19 දරන විද්‍යුත් ගනුදෙනු පනතේ 21 වගන්තිය, ඉලෙක්ට්‍රොනික වාර්තාවක සත්‍යතාව නීතියෙන් උපකල්පනය කරයි. ඇල්ගොරිතමයේ අභ්‍යන්තර ක්‍රියාවලීන් වාණිජ රහස් ලෙස ආරක්ෂා කර ඇති විට, විත්තිකරුට ඇල්ගොරිතමයක් වැරදි බව ඔප්පු කිරීමට සිදුවීම, Due Process (නිසි ක්‍රියා පටිපාටිය) සහ සාධාරණ නඩු විභාගයක මූලික අයිතිවාසිකම් උල්ලංඝනය කරයි.</p>
                    </div>
                </div>
            </div>
            <!-- Standard UI elements to retain overall design perfectly intact -->
        </div>
    </section>

    <!-- Footer -->
    <footer class="glass-panel border-t border-slate-800 py-8 px-6 text-center z-10 relative mt-12">
        <p class="text-gray-500 text-sm mb-2 ai-text" data-en="Research by Jayasuriya Kuranage Shenol Ravisha Perera &copy; 2026" data-ru="Исследование Джаясурии Куранаге Шенола Равиша Переры &copy; 2026">Research by Jayasuriya Kuranage Shenol Ravisha Perera &copy; 2026</p>
    </footer>

    <!-- Scripts (Offline Translation, Search, Three.js) -->
    <script>
        /* Gujarati (Latin): Aa function offline translation mate chhe (This function handles offline translation instantly) */
        function translatePageOffline(lang, langName) {
            const elements = document.querySelectorAll('.ai-text');
            elements.forEach(el => {
                // Original Sinhala text save kari rakhvo
                if (!el.hasAttribute('data-si')) {
                    el.setAttribute('data-si', el.innerHTML);
                }
                
                if (lang === 'si') {
                    el.innerHTML = el.getAttribute('data-si');
                } else if (lang === 'en' && el.hasAttribute('data-en')) {
                    el.innerHTML = el.getAttribute('data-en');
                } else if (lang === 'ru' && el.hasAttribute('data-ru')) {
                    el.innerHTML = el.getAttribute('data-ru');
                } else {
                    // Fallback to original if translation is missing for minor elements
                    el.innerHTML = el.getAttribute('data-si');
                }
            });
            document.getElementById('current-lang').innerText = langName || (lang === 'en' ? 'English' : (lang === 'ru' ? 'Русский' : 'සිංහල'));
        }

        /* Gujarati (Latin): Page load thaya pachi popup nu function (Function for initial popup selection) */
        function setInitialLanguage(lang, langName) {
            translatePageOffline(lang, langName);
            const popup = document.getElementById('language-popup');
            popup.style.opacity = '0';
            setTimeout(() => {
                popup.style.display = 'none';
            }, 300);
        }

        window.onload = function() {

            // --- 1. SEARCH / HIGHLIGHT FUNCTIONALITY (As it was originally) ---
            const searchInput = document.getElementById('keyword-search');
            let timeout = null;

            searchInput.addEventListener('input', function() {
                clearTimeout(timeout);
                const term = this.value.trim();
                
                timeout = setTimeout(() => {
                    // Remove old highlights
                    document.querySelectorAll('mark.search-highlight').forEach(mark => {
                        const parent = mark.parentNode;
                        parent.replaceChild(document.createTextNode(mark.textContent), mark);
                        parent.normalize();
                    });

                    if(!term) return;

                    const searchContainers = document.querySelectorAll('.search-content, .glass-panel p, .glass-panel h4, td, li');
                    let firstMatch = null;
                    const regex = new RegExp(`(${term})`, 'gi');

                    searchContainers.forEach(container => {
                        const walker = document.createTreeWalker(container, NodeFilter.SHOW_TEXT, null, false);
                        const nodesToReplace = [];
                        
                        while(walker.nextNode()) {
                            if (walker.currentNode.parentNode.tagName === 'SCRIPT' || 
                                walker.currentNode.parentNode.tagName === 'STYLE') continue;
                            
                            if (regex.test(walker.currentNode.nodeValue)) {
                                nodesToReplace.push(walker.currentNode);
                            }
                        }

                        nodesToReplace.forEach(node => {
                            const fragment = document.createDocumentFragment();
                            let lastIdx = 0;
                            regex.lastIndex = 0;
                            let match;
                            const text = node.nodeValue;
                            
                            while ((match = regex.exec(text)) !== null) {
                                fragment.appendChild(document.createTextNode(text.slice(lastIdx, match.index)));
                                const mark = document.createElement('mark');
                                mark.className = 'search-highlight';
                                mark.textContent = match[0];
                                fragment.appendChild(mark);
                                lastIdx = regex.lastIndex;
                                if(!firstMatch) firstMatch = mark;
                            }
                            fragment.appendChild(document.createTextNode(text.slice(lastIdx)));
                            node.parentNode.replaceChild(fragment, node);
                        });
                    });

                    if(firstMatch) {
                        firstMatch.scrollIntoView({behavior: 'smooth', block: 'center'});
                    }

                }, 300);
            });

            // --- 2. THREE.JS ADVANCED BACKGROUND ---
            const canvas = document.getElementById('bg-canvas');
            const scene = new THREE.Scene();
            scene.fog = new THREE.FogExp2(0x050505, 0.0025);

            const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.z = 120;
            camera.position.y = 10;

            const renderer = new THREE.WebGLRenderer({ canvas: canvas, alpha: true, antialias: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

            /* Gujarati (Latin): Neural Network Particles banavava mate (To create neural network particles) */
            const particleCount = 700;
            const geometry = new THREE.BufferGeometry();
            const positions = new Float32Array(particleCount * 3);
            const colors = new Float32Array(particleCount * 3);

            const color1 = new THREE.Color(0x38bdf8); // Sky blue
            const color2 = new THREE.Color(0x818cf8); // Purple
            const color3 = new THREE.Color(0xec4899); // Pink

            for(let i=0; i<particleCount*3; i+=3) {
                const r = 220 * Math.cbrt(Math.random());
                const theta = Math.random() * 2 * Math.PI;
                const phi = Math.acos(2 * Math.random() - 1);
                
                positions[i] = r * Math.sin(phi) * Math.cos(theta);
                positions[i+1] = r * Math.sin(phi) * Math.sin(theta);
                positions[i+2] = r * Math.cos(phi);

                const mixedColor = color1.clone().lerp(Math.random() > 0.5 ? color2 : color3, Math.random());
                colors[i] = mixedColor.r;
                colors[i+1] = mixedColor.g;
                colors[i+2] = mixedColor.b;
            }

            geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
            geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3));

            const material = new THREE.PointsMaterial({
                size: 1.5,
                vertexColors: true,
                transparent: true,
                opacity: 0.7,
                sizeAttenuation: true
            });

            const particles = new THREE.Points(geometry, material);
            scene.add(particles);

            const gridHelper = new THREE.GridHelper(400, 40, 0x38bdf8, 0x1e293b);
            gridHelper.position.y = -60;
            gridHelper.material.opacity = 0.25;
            gridHelper.material.transparent = true;
            scene.add(gridHelper);

            let mouseX = 0; let mouseY = 0;
            let targetX = 0; let targetY = 0;
            const windowHalfX = window.innerWidth / 2;
            const windowHalfY = window.innerHeight / 2;

            document.addEventListener('mousemove', (event) => {
                mouseX = (event.clientX - windowHalfX);
                mouseY = (event.clientY - windowHalfY);
            });

            // --- 3. BINARY RAIN ANIMATION ---
            function createBinaryRain() {
                const rainContainer = document.createElement('div');
                rainContainer.style.position = 'fixed';
                rainContainer.style.top = '0';
                rainContainer.style.left = '0';
                rainContainer.style.width = '100vw';
                rainContainer.style.height = '100vh';
                rainContainer.style.pointerEvents = 'none';
                rainContainer.style.zIndex = '-1'; 
                rainContainer.style.overflow = 'hidden';
                document.body.appendChild(rainContainer);

                setInterval(() => {
                    const drop = document.createElement('div');
                    drop.textContent = Math.random() > 0.5 ? '1' : '0';
                    drop.style.position = 'absolute';
                    drop.style.left = Math.random() * 100 + 'vw';
                    drop.style.top = '-50px';
                    drop.style.color = 'rgba(56, 189, 248, 0.15)'; 
                    drop.style.fontSize = Math.random() * 16 + 10 + 'px';
                    drop.style.fontFamily = 'monospace';
                    const duration = Math.random() * 4 + 4;
                    drop.style.transition = `top ${duration}s linear, opacity ${duration}s ease-in-out`;
                    rainContainer.appendChild(drop);

                    setTimeout(() => {
                        drop.style.top = '100vh';
                        drop.style.opacity = '0';
                    }, 50);

                    setTimeout(() => drop.remove(), duration * 1000);
                }, 200);
            }
            createBinaryRain();

            /* Gujarati (Latin): Animation loop continuous farva mate (To rotate animation continuously) */
            const clock = new THREE.Clock();
            function animate() {
                requestAnimationFrame(animate);
                const time = clock.getElapsedTime();

                particles.rotation.y += 0.0008;
                particles.rotation.x += 0.0004;

                gridHelper.position.z += 0.3;
                if(gridHelper.position.z > 10) gridHelper.position.z = 0; 

                const posArray = particles.geometry.attributes.position.array;
                for(let i=0; i<particleCount; i++) {
                    const i3 = i * 3;
                    posArray[i3+1] += Math.sin(time + posArray[i3]) * 0.015; 
                }
                particles.geometry.attributes.position.needsUpdate = true;

                targetX = mouseX * 0.05;
                targetY = mouseY * 0.05;
                camera.position.x += (targetX - camera.position.x) * 0.05;
                camera.position.y += (-targetY - camera.position.y) * 0.05;
                camera.lookAt(scene.position);

                renderer.render(scene, camera);
            }
            animate();

            window.addEventListener('resize', () => {
                camera.aspect = window.innerWidth / window.innerHeight;
                camera.updateProjectionMatrix();
                renderer.setSize(window.innerWidth, window.innerHeight);
            });

            // 4. Scroll Animations
            const observerOptions = { root: null, rootMargin: '0px', threshold: 0.15 };
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        entry.target.classList.add('opacity-100', 'translate-y-0');
                        entry.target.classList.remove('opacity-0', 'translate-y-10');
                    }
                });
            }, observerOptions);

            document.querySelectorAll('.animate-on-scroll').forEach((el) => {
                el.classList.add('transition-all', 'duration-1000', 'opacity-0', 'translate-y-10');
                observer.observe(el);
            });
        };
    </script>
</body>
</html>

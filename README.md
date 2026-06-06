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

            <!-- New Download Button Section -->
            <div class="mt-8 animate-on-scroll search-content">
                <a href="https://drive.google.com/file/d/1bo5lBWfv4ZovVYWOwAXRhLabvOI0udX5/view?usp=sharing" target="_blank" class="inline-flex items-center space-x-2 bg-gradient-to-r from-sky-500 to-blue-600 hover:from-sky-400 hover:to-blue-500 text-white font-semibold py-3 px-8 rounded-full shadow-lg shadow-sky-500/30 transition-all transform hover:scale-105 border border-sky-400/50">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path></svg>
                    <span class="ai-text tracking-wide" data-en="Download Full Document" data-ru="Скачать полный документ">සම්පූර්ණ ලේඛනය බාගත කරන්න</span>
                </a>
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

    <!-- Research Rationale & Literature -->
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

            <!-- Updated Literature Review Strategy Table -->
            <div class="animate-on-scroll search-content">
                <h3 class="text-purple-400 font-semibold text-lg mb-2 uppercase tracking-widest ai-text" data-en="Literature Review Strategy" data-ru="Стратегия обзора литературы">Literature Review Strategy</h3>
                <h2 class="text-3xl font-bold text-white mb-6 ai-text" data-en="Literature Review Strategy" data-ru="Стратегия обзора литературы">සාහිත්‍ය සෙවීමේ උපාය මාර්ගය</h2>
                <div class="glass-panel p-6 rounded-2xl mb-6">
                    <p class="text-sm text-gray-300 mb-4 text-justify ai-text" data-en="To conduct a comprehensive, empirical, and reproducible review, this research uses a structured search strategy adhering strictly to the PRISMA procedure." data-ru="Для проведения всестороннего и воспроизводимого обзора в данном исследовании используется структурированная стратегия поиска.">පවතින දැනුම් සම්භාරය පිළිබඳ පුළුල්, ආනුභවික සහ පුනරාවර්තනය කළ හැකි සමාලෝචනයක් සිදු කිරීම සඳහා, මෙම පර්යේෂණය PRISMA ක්‍රියා පටිපාටියට දැඩි ලෙස අනුකූලව ව්‍යුහගත සෙවුම් උපාය මාර්ගයක් භාවිතා කරයි.</p>
                    <ul class="list-disc list-inside text-sm text-gray-400 space-y-2">
                        <li class="ai-text" data-en="Priority Sources: IEEE Xplore, Scopus, arXiv, SpringerLink, NIST, ISO Publications." data-ru="Приоритетные источники: IEEE Xplore, Scopus, arXiv, SpringerLink, публикации NIST, ISO."><strong class="text-gray-200">ප්‍රමුඛතා ප්‍රභවයන්:</strong> IEEE Xplore, Scopus, arXiv, SpringerLink, National Institute of Standards and Technology (NIST) සහ International Organization for Standardization (ISO) හි නිල ප්‍රකාශන මෙයට ඇතුළත් වේ.</li>
                        <li class="ai-text" data-en="Search Scope: Strict Boolean search string architecture." data-ru="Область поиска: Строгая архитектура логических строк поиска."><strong class="text-gray-200">සෙවීමේ විෂය පථය:</strong> කෘතිම බුද්ධිය, ඩිජිටල් අධිකරණ වෛද්‍ය විද්‍යාව සහ නීති විද්‍යාව යන බහුවිධ අංශ ආවරණය කිරීම සඳහා දැඩි බූලියන් සෙවුම් තන්තු ගෘහ නිර්මාණ ශිල්පයක් යොදවනු ලැබේ.</li>
                        <li class="ai-text" data-en="Timeframe: Recent academic articles published between 2018 and 2026." data-ru="Временные рамки: Недавние научные статьи, опубликованные в период с 2018 по 2026 год."><strong class="text-gray-200">කාල රාමුව:</strong> 2018 සහ 2026 අතර ප්‍රකාශිත නවතම ශාස්ත්‍රීය ලිපි ඉලක්ක කර ගනු ලැබේ.</li>
                    </ul>
                </div>
                <div class="overflow-x-auto glass-panel rounded-2xl">
                    <table class="text-xs min-w-[700px]">
                        <thead>
                            <tr>
                                <th class="ai-text" data-en="Multidisciplinary Subject Area" data-ru="Мультидисциплинарная предметная область">බහුවිධ විෂය ක්ෂේත්‍රය</th>
                                <th class="ai-text" data-en="Core Target Keywords" data-ru="Основные целевые ключевые слова">මූලික ඉලක්කගත යතුරු පද</th>
                                <th class="ai-text" data-en="Strict Contextual Modifiers" data-ru="Строгие контекстные модификаторы">දැඩි සන්දර්භීය වෙනස් කිරීම්</th>
                                <th class="ai-text" data-en="Structured Boolean Search Strings" data-ru="Структурированные логические строки поиска">ව්‍යුහගත බූලියන් සෙවුම් තන්තු</th>
                            </tr>
                        </thead>
                        <tbody class="align-top">
                            <tr>
                                <td class="font-semibold text-sky-200 ai-text" data-en="Algorithmic Architecture & AI" data-ru="Алгоритмическая архитектура и ИИ">ඇල්ගොරිතම ගෘහ නිර්මාණ ශිල්පය සහ AI</td>
                                <td class="ai-text text-gray-300" data-en='"Artificial Intelligence", "Deep Learning", "Machine Learning", "Generative AI"' data-ru='"Artificial Intelligence", "Deep Learning", "Machine Learning", "Generative AI"'>"Artificial Intelligence", "Deep Learning", "Machine Learning", "Generative AI"</td>
                                <td class="ai-text text-gray-300" data-en='"Explainable AI", "XAI", "SHAP", "LIME", "Grad-CAM", "Adversarial Machine Learning"' data-ru='"Explainable AI", "XAI", "SHAP", "LIME", "Grad-CAM", "Adversarial Machine Learning"'>"Explainable AI", "XAI", "SHAP", "LIME", "Grad-CAM", "Adversarial Machine Learning"</td>
                                <td class="font-mono text-gray-400 ai-text" data-en='("Artificial Intelligence" OR "Deep Learning") AND ("Explainability" OR "XAI" OR "SHAP" OR "LIME")' data-ru='("Artificial Intelligence" OR "Deep Learning") AND ("Explainability" OR "XAI" OR "SHAP" OR "LIME")'>("Artificial Intelligence" OR "Deep Learning") AND ("Explainability" OR "XAI" OR "SHAP" OR "LIME")</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-200 ai-text" data-en="Digital Forensics" data-ru="Цифровая криминалистика">ඩිජිටල් සාක්ෂි සහ අධිකරණ වෛද්‍ය විද්‍යාව</td>
                                <td class="ai-text text-gray-300" data-en='"Cyber Forensics", "Digital Evidence", "Network Forensics", "Incident Response"' data-ru='"Cyber Forensics", "Digital Evidence", "Network Forensics", "Incident Response"'>"Cyber Forensics", "Digital Evidence", "Network Forensics", "Incident Response"</td>
                                <td class="ai-text text-gray-300" data-en='"Evidence extraction", "DFIR", "Cyber Exchange Principle", "Locard"' data-ru='"Evidence extraction", "DFIR", "Cyber Exchange Principle", "Locard"'>"Evidence extraction", "DFIR", "Cyber Exchange Principle", "Locard"</td>
                                <td class="font-mono text-gray-400 ai-text" data-en='("Digital Forensics" OR "Cyber Forensics") AND ("AI" OR "Machine Learning") AND ("Incident Response")' data-ru='("Digital Forensics" OR "Cyber Forensics") AND ("AI" OR "Machine Learning") AND ("Incident Response")'>("Digital Forensics" OR "Cyber Forensics") AND ("AI" OR "Machine Learning") AND ("Incident Response")</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-200 ai-text" data-en="Legal Admissibility and Due Process" data-ru="Правовая допустимость и надлежащая процедура">නීතිමය පිළිගැනීමේ හැකියාව සහ නිසි ක්‍රියා පටිපාටිය</td>
                                <td class="ai-text text-gray-300" data-en='"Admissibility", "Daubert Standard", "Expert Testimony", "Evidentiary Standards"' data-ru='"Admissibility", "Daubert Standard", "Expert Testimony", "Evidentiary Standards"'>"Admissibility", "Daubert Standard", "Expert Testimony", "Evidentiary Standards"</td>
                                <td class="ai-text text-gray-300" data-en='"Due process", "Cross-examination", "Reliability", "Authenticity", "Black-box"' data-ru='"Due process", "Cross-examination", "Reliability", "Authenticity", "Black-box"'>"Due process", "Cross-examination", "Reliability", "Authenticity", "Black-box"</td>
                                <td class="font-mono text-gray-400 ai-text" data-en='("Admissibility" OR "Daubert") AND ("AI-generated evidence" OR "Black-box" OR "Algorithmic Bias")' data-ru='("Admissibility" OR "Daubert") AND ("AI-generated evidence" OR "Black-box" OR "Algorithmic Bias")'>("Admissibility" OR "Daubert") AND ("AI-generated evidence" OR "Black-box" OR "Algorithmic Bias")</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-200 ai-text" data-en="Sri Lankan Legal Context" data-ru="Правовой контекст Шри-Ланки">ශ්‍රී ලාංකේය නෛතික සන්දර්භය</td>
                                <td class="ai-text text-gray-300" data-en='"Sri Lanka", "Evidence Ordinance", "ETA 2006", "Computer Crimes Act"' data-ru='"Sri Lanka", "Evidence Ordinance", "ETA 2006", "Computer Crimes Act"'>"Sri Lanka", "Evidence Ordinance", "ETA 2006", "Computer Crimes Act"</td>
                                <td class="ai-text text-gray-300" data-en='"Evidence Special Provisions Act", "SLCERT", "Information and Communication Technology Act"' data-ru='"Evidence Special Provisions Act", "SLCERT", "Information and Communication Technology Act"'>"Evidence Special Provisions Act", "SLCERT", "Information and Communication Technology Act"</td>
                                <td class="font-mono text-gray-400 ai-text" data-en='("Sri Lanka" AND "Electronic Evidence") OR ("Evidence Special Provisions Act" OR "Electronic Transactions Act")' data-ru='("Sri Lanka" AND "Electronic Evidence") OR ("Evidence Special Provisions Act" OR "Electronic Transactions Act")'>("Sri Lanka" AND "Electronic Evidence") OR ("Evidence Special Provisions Act" OR "Electronic Transactions Act")</td>
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
                        <p class="ai-text" data-en="Procedural Issue: Section 7, which mandates a 45-day notice period before introducing evidence, hinders the rapid nature of AI security systems." data-ru="Процессуальная проблема: Раздел 7, который требует 45-дневного уведомления перед представлением доказательств, препятствует быстрой природе систем безопасности ИИ."><strong>ක්‍රියා පටිපාටිමය ගැටළුව:</strong> සාක්ෂි හඳුන්වා දීමට පෙර අනිවාර්යයෙන්ම දින 45ක දැනුම් දීමේ කාලයක් නියම කරන 7 වගන්තිය, ක්ෂණික සයිබර් තර්ජනවලට අදාළ AI ආරක්ෂක පද්ධතිවල වේගවත් ස්වභාවයට බාධා කරයි. මෙය Due Process (නිසි ක්‍රියා පටිපාටිය) සම්බන්ධයෙන් අභියෝගාත්මක විය හැක.</p>
                    </div>
                    <div class="space-y-4">
                        <h4 class="font-bold text-purple-400 border-b border-gray-700 pb-2 ai-text" data-en="Algorithmic Due Process Disconnect" data-ru="Отключение алгоритмической надлежащей правовой процедуры">Algorithmic Due Process Disconnect (ඇල්ගොරිතමයේ නිසි ක්‍රියා පටිපාටියේ විසන්ධිය)</h4>
                        <p class="ai-text" data-en="Burden of Proof: Shifting the burden to the defendant to prove an algorithm wrong violates fundamental rights to a fair trial." data-ru="Бремя доказывания: Перенос на обвиняемого бремени доказывания ошибочности алгоритма нарушает основные права."><strong>සාක්ෂි භාරය:</strong> 2006 අංක 19 දරන විද්‍යුත් ගනුදෙනු පනතේ 21 වගන්තිය, ඉලෙක්ට්‍රොනික වාර්තාවක සත්‍යතාව නීතියෙන් උපකල්පනය කරයි. ඇල්ගොරිතමයේ අභ්‍යන්තර ක්‍රියාවලීන් වාණිජ රහස් ලෙස ආරක්ෂා කර ඇති විට, විත්තිකරුට ඇල්ගොරිතමයක් වැරදි බව ඔප්පු කිරීමට සිදුවීම, Due Process (නිසි ක්‍රියා පටිපාටිය) සහ සාධාරණ නඩු විභාගයක මූලික අයිතිවාසිකම් උල්ලංඝනය කරයි.</p>
                        <h4 class="font-bold text-pink-400 border-b border-gray-700 pb-2 mt-6 ai-text" data-en="Absence of Unified XAI-CF Frameworks" data-ru="Отсутствие единых структур XAI-CF">ඒකාබද්ධ XAI-CF රාමු සහ ප්‍රමිතීන් නොමැති වීම</h4>
                        <p class="ai-text" data-en="Although the theoretical benefits of SHAP and LIME are discussed, there are no documented cases of standard application directly as expert evidence in court." data-ru="Хотя обсуждаются теоретические преимущества SHAP и LIME, нет задокументированных случаев их стандартного применения непосредственно в качестве экспертных доказательств в суде.">SHAP සහ LIME හි න්‍යායික ප්‍රතිලාභ සාකච්ඡා කළද, මෙම සංකීර්ණ ගණිතමය පැහැදිලි කිරීම් අධිකරණයකදී විශේෂඥ සාක්ෂි ලෙස සෘජුවම, සම්මත භාවිතයට ගත් ලේඛනගත අවස්ථා නොමැත. විමර්ශන ජීවන චක්‍රය පුරා XAI මූලධර්ම ඒකාබද්ධ කරන ඒකාබද්ධ රාමුවක් නොමැත.</p>
                    </div>
                </div>
            </div>
            
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 animate-on-scroll">
                <div class="glass-panel p-8 rounded-2xl border-l-4 border-l-red-500">
                    <h3 class="text-xl font-bold mb-4 text-white ai-text" data-en="Research Problem Statement" data-ru="Постановка проблемы исследования">Research Problem Statement (මූලික විමර්ශන ගැටළු ප්‍රකාශ කිරීම)</h3>
                    <p class="text-sm text-gray-300 mb-4 ai-text" data-en="Deploying AI in cyber forensics without legal modernization poses a threat to the fundamental integrity of the criminal justice system." data-ru="Развертывание ИИ в киберкриминалистике без правовой модернизации представляет угрозу фундаментальной целостности системы уголовного правосудия.">සයිබර් අධිකරණ වෛද්‍ය විද්‍යාවට AI යෙදවීම, නෛතික නවීකරණයකින් තොරව සිදුකිරීම, අපරාධ යුක්ති විනිශ්චය පද්ධතියේ මූලික අඛණ්ඩතාවයට තර්ජනයක් වේ.</p>
                    <ul class="list-disc list-inside text-sm text-gray-400 space-y-2">
                        <li class="ai-text" data-en="Admissibility Paradox: Law enforcement agencies use AI to find anomalies, but because the algorithms are black-boxes, this information cannot become admissible evidence." data-ru="Парадокс допустимости: Правоохранительные органы используют ИИ для поиска аномалий, но поскольку алгоритмы являются черными ящиками, эта информация не может стать допустимым доказательством."><strong>Admissibility Paradox:</strong> නීතිය ක්‍රියාත්මක කරන ආයතන විසින් මිනිසුන්ට හඳුනාගත නොහැකි ජාල විෂමතා සොයා ගැනීමට AI භාවිතා කළද, ඇල්ගොරිතම යාන්ත්‍රණයන් Black-box වන බැවින් එම තොරතුරු Admissibility සහිත සාක්ෂි බවට පත්කිරීමට නොහැකි වීම.</li>
                        <li class="ai-text" data-en="Statutory Obsolescence: Existing evidentiary frameworks lack the technical definitions required to regulate dynamically learning computer systems." data-ru="Устаревание закона: Существующим доказательственным базам не хватает технических определений, необходимых для регулирования динамически обучающихся компьютерных систем."><strong>ව්‍යවස්ථාපිත යල්පැනීම:</strong> ශ්‍රී ලංකාවේ පවතින සාක්ෂි රාමු ගතිකව ඉගෙන ගන්නා පරිගණක පද්ධති නියාමනය කිරීමට අවශ්‍ය තාක්ෂණික නිර්වචනවලින් තොර වීම.</li>
                        <li class="ai-text" data-en="AI Weaponization Threat: Adversarial Machine Learning and Data Poisoning attacks by malicious actors cast serious doubts on the reliability of AI-generated evidence." data-ru="Угроза вепонизации ИИ: Атаки состязательного машинного обучения и отравления данных злоумышленниками вызывают серьезные сомнения в надежности доказательств, созданных ИИ."><strong>AI අවි ගැන්වීමේ තර්ජනය එදිරිව සාක්ෂි අඛණ්ඩතාව:</strong> ද්වේෂසහගත ක්‍රියාකරුවන් විසින් AI භාවිතා කරමින් Adversarial Machine Learning සහ Data Poisoning ප්‍රහාර එල්ල කිරීම මගින් සාක්ෂිවල විශ්වසනීයත්වය බිඳ වැටීම.</li>
                    </ul>
                </div>
                
                <div class="glass-panel p-8 rounded-2xl border-l-4 border-l-green-500">
                    <h3 class="text-xl font-bold mb-4 text-white ai-text" data-en="Research Questions" data-ru="Вопросы исследования">Research Questions (මූලික විමර්ශන ප්‍රශ්න)</h3>
                    <ol class="list-decimal list-inside text-sm text-gray-300 space-y-3">
                        <li class="ai-text" data-en="How do the inherent black-box characteristics of deep machine learning models conflict with the qualitative dimensions of reliability and authenticity demanded by traditional criminal jurisprudence?" data-ru="Как присущие черному ящику характеристики моделей глубокого машинного обучения конфликтуют с качественными аспектами надежности и аутентичности, требуемыми традиционной уголовной юриспруденцией?">ගැඹුරු යන්ත්‍ර ඉගෙනුම් ආකෘතිවල ආවේණික Black-box (කළු පෙට්ටි) ලක්ෂණ, සාම්ප්‍රදායික අපරාධ නීති විද්‍යාවෙන් ඉල්ලා සිටින විශ්වසනීයත්වය සහ සත්‍යතාව යන ගුණාත්මක මානයන් සමඟ ගැටෙන්නේ කෙසේද?</li>
                        <li class="ai-text" data-en="In what specific, procedural ways can XAI techniques like SHAP and LIME be implemented to satisfy scientific reliability, testability, and Due Process in a live judicial environment?" data-ru="Какими конкретными процессуальными способами методы XAI, такие как SHAP и LIME, могут быть реализованы для удовлетворения научной надежности, проверяемости и надлежащей правовой процедуры в живой судебной среде?">SHAP සහ LIME වැනි XAI ශිල්පීය ක්‍රම, සජීවී අධිකරණ පරිසරයක් තුළ විද්‍යාත්මක විශ්වසනීයත්වය, පරීක්ෂා කිරීමේ හැකියාව සහ Due Process තෘප්තිමත් කිරීම සඳහා ක්‍රියාත්මක කළ හැක්කේ කුමන නිශ්චිත, ක්‍රියාපටිපාටිමය ආකාරවලින්ද?</li>
                        <li class="ai-text" data-en="To what extent do the restrictive clauses of Sri Lanka's Evidence Act of 1995 and Electronic Transactions Act of 2006 hinder or facilitate the admissibility of AI-generated forensic evidence?" data-ru="В какой степени ограничительные положения Закона о доказательствах Шри-Ланки 1995 года и Закона об электронных транзакциях 2006 года препятствуют или способствуют допустимости судебно-медицинских доказательств, созданных ИИ?">ශ්‍රී ලංකාවේ 1995 අංක 14 සහ 2006 අංක 19 පනත්වල සීමාකාරී වගන්ති AI-ජනනය කරන ලද සාක්ෂි Admissibility ට බාධා කරන්නේ හෝ පහසු කරන්නේ කොපමණ දුරකටද, සහ නීත්‍යානුකූලව අවශ්‍ය නිශ්චිත ව්‍යවස්ථාපිත අනුවර්තනයන් මොනවාද?</li>
                        <li class="ai-text" data-en="How can complex international AI risk management standards like ISO/IEC 42001 and NIST's AML guidelines be integrated into a legally robust workflow for Sri Lankan digital forensic investigators?" data-ru="Как сложные международные стандарты управления рисками ИИ, такие как ISO/IEC 42001 и руководящие принципы NIST по AML, могут быть интегрированы в юридически надежный рабочий процесс для шри-ланкийских специалистов по цифровой криминалистике?">ISO/IEC 42001 සහ NIST හි AML මාර්ගෝපදේශ වැනි ජාත්‍යන්තර ප්‍රමිතීන්, ශ්‍රී ලාංකේය ඩිජිටල් අධිකරණ වෛද්‍ය විමර්ශකයන් සඳහා සම්මත වැඩ ප්‍රවාහයක් බවට ඒකාබද්ධ කළ හැක්කේ කෙසේද?</li>
                    </ol>
                </div>
            </div>

            <div class="glass-panel p-8 rounded-3xl animate-on-scroll">
                <h3 class="text-2xl font-bold mb-6 text-center text-white text-glow ai-text" data-en="Actionable Objectives" data-ru="Действенные цели">Actionable Objectives (ක්‍රියාකාරී අරමුණු)</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 text-sm">
                    <div class="p-4 bg-slate-800/50 rounded-xl ai-text" data-en="Objective 1: To critically evaluate and map the structural and procedural limitations in Sri Lanka's current legal framework regarding the admission of dynamic, non-deterministic computer-generated evidence." data-ru="Цель 1: Критически оценить и наметить структурные и процедурные ограничения в нынешней правовой базе Шри-Ланки в отношении допуска динамических, недетерминированных компьютерных доказательств.">
                        <strong class="text-sky-300">පළමු අරමුණ:</strong> ගතික සහ නිර්ණායක නොවන පරිගණක ජනිත සාක්ෂි අධිකරණයේදී පිළිගැනීමට අදාළව ශ්‍රී ලංකාවේ වත්මන් නීතිමය රාමුවේ පවතින ව්‍යුහාත්මක සහ ක්‍රියාපටිපාටිමය සීමාවන් විවේචනාත්මකව ඇගයීම සහ සිතියම්ගත කිරීම.
                    </div>
                    <div class="p-4 bg-slate-800/50 rounded-xl ai-text" data-en="Objective 2: To systematically deploy and empirically validate XAI methodologies (SHAP and LIME) on standard cybersecurity datasets, aligning complex computer outputs with legal requirements." data-ru="Цель 2: Систематически внедрять и эмпирически проверять методологии XAI (SHAP и LIME) на стандартных наборах данных кибербезопасности, согласовывая сложные результаты компьютеров с правовыми требованиями.">
                        <strong class="text-sky-300">දෙවන අරමුණ:</strong> සම්මත සයිබර් ආරක්ෂණ දත්ත සමුදායන් මත XAI (පැහැදිලි කළ හැකි AI) ක්‍රමවේදයන් වන SHAP සහ LIME පද්ධතිගතව යෙදවීම සහ ප්‍රායෝගිකව තහවුරු කිරීම.
                    </div>
                    <div class="p-4 bg-slate-800/50 rounded-xl ai-text" data-en="Objective 3: To design and propose a comprehensive 'eXplainable AI in Cyber Forensics' (XAI-CF) operational framework ensuring continuous human oversight and algorithmic accountability at every stage." data-ru="Цель 3: Разработать и предложить комплексную операционную структуру «Объяснимый ИИ в киберкриминалистике» (XAI-CF), обеспечивающую постоянный человеческий надзор и подотчетность алгоритмов на каждом этапе.">
                        <strong class="text-sky-300">තුන්වන අරමුණ:</strong> අපරාධ විමර්ශන ජීවන චක්‍රයේ සෑම අදියරකදීම අඛණ්ඩ මිනිස් අධීක්ෂණය, විනිවිදභාවයෙන් යුත් තීරණ ගැනීම සහ ඇල්ගොරිතම වගවීම තහවුරු කෙරෙන පූර්ණ "සයිබර් අධිකරණ විද්‍යාව තුළ පැහැදිලි කළ හැකි කෘතිම බුද්ධි" (XAI-CF) මෙහෙයුම් රාමුවක් නිර්මාණය කර යෝජනා කිරීම.
                    </div>
                    <div class="p-4 bg-slate-800/50 rounded-xl ai-text" data-en="Objective 4: To formulate a series of practical policy amendments integrating international risk management standards (ISO/IEC 42001, 27037/27041, NIST) to empower the Sri Lankan judicial system." data-ru="Цель 4: Сформулировать серию практических поправок к политике, интегрирующих международные стандарты управления рисками (ISO/IEC 42001, 27037/27041, NIST) для расширения возможностей судебной системы Шри-Ланки.">
                        <strong class="text-sky-300">හතරවන අරමුණ:</strong> ශ්‍රී ලංකාවේ අධිකරණ පද්ධතියේ සහ නීතිය ක්‍රියාත්මක කරන ආයතනවල ධාරිතාවය සවිබල ගැන්වීම සඳහා, ජාත්‍යන්තර අවදානම් කළමනාකරණ ප්‍රමිතීන් (ISO/IEC 42001, 27037/27041, NIST) ඒකාබද්ධ කරමින් ප්‍රායෝගික ප්‍රතිපත්ති සංශෝධන මාලාවක් සම්පාදනය කිරීම.
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Research Design & Limitations -->
    <section id="design" class="relative py-24 px-6 md:px-12 bg-gradient-to-b from-slate-900/50 to-transparent search-content">
        <div class="max-w-6xl mx-auto">
            <div class="animate-on-scroll text-center mb-12">
                <h3 class="text-sky-400 font-semibold text-lg mb-2 uppercase tracking-widest ai-text" data-en="Methodology" data-ru="Методология">Methodology</h3>
                <h2 class="text-3xl md:text-5xl font-bold text-glow ai-text" data-en="Research Design" data-ru="Дизайн исследования">Research Design (යෝජිත පර්යේෂණ සැලසුම)</h2>
                <p class="text-gray-400 mt-4 ai-text" data-en="This research calls for a deeply integrated multi-modal workflow comprising legal analysis, empirical technical validation, and ethical standardization." data-ru="Это исследование требует глубоко интегрированного мультимодального рабочего процесса, включающего правовой анализ, эмпирическую техническую проверку и этическую стандартизацию.">මෙම පර්යේෂණය, නීතිමය විශ්ලේෂණය, ආනුභවික තාක්ෂණික වලංගුකරණය සහ ආචාරධාර්මික ප්‍රමිතිකරණය යන අංශ තුනකින් සමන්විත බහු-මාදිලියේ වැඩ ප්‍රවාහයකි.</p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-12">
                <div class="glass-panel p-6 rounded-2xl animate-on-scroll hover:-translate-y-2 transition-transform">
                    <h4 class="text-xl font-bold text-sky-300 mb-3 ai-text" data-en="Phase 1: Doctrinal Legal Analysis" data-ru="Фаза 1: Доктринальный правовой анализ">අදියර 1: මූලධර්මාත්මක නීතිමය විශ්ලේෂණය</h4>
                    <p class="text-sm text-gray-300 mb-2 ai-text" data-en="Analysis: Comprehensive qualitative examination of primary statutes, secondary legal sources, and historical jurisprudence." data-ru="Анализ: Всестороннее качественное изучение первичных законов, вторичных правовых источников и исторической юриспруденции."><strong>විශ්ලේෂණය:</strong> ප්‍රාථමික ව්‍යවස්ථා, ද්විතියික නීතිමය මූලාශ්‍ර සහ ඓතිහාසික නීති විද්‍යාව පිළිබඳ සවිස්තරාත්මක ගුණාත්මක විභාගයක්.</p>
                    <p class="text-sm text-gray-300 mb-2 ai-text" data-en="Special Focus: Shortcomings of Sections 4, 5, 7 of the 1995 Act and Section 21 of the 2006 Act." data-ru="Особое внимание: Недостатки разделов 4, 5, 7 Закона 1995 года и раздела 21 Закона 2006 года."><strong>විශේෂ අවධානය:</strong> 1995 පනතේ 4, 5, 7 වගන්තිවල අඩුපාඩු සහ 2006 පනතේ 21 වගන්තිය.</p>
                    <p class="text-sm text-gray-300 ai-text" data-en="Legal Precedents: Analysis of cases like Abeygunawardena vs. Samoon and AG vs Potta Nauffer." data-ru="Юридические прецеденты: Анализ таких дел, как Абейгунавардена против Самуна и Генеральный прокурор против Потты Науффера."><strong>නීතිමය පූර්වාදර්ශ:</strong> Abeygunawardena vs. Samoon සහ AG vs Potta Nauffer වැනි නඩු විභාග විශ්ලේෂණය.</p>
                </div>
                
                <div class="glass-panel p-6 rounded-2xl animate-on-scroll delay-100 hover:-translate-y-2 transition-transform">
                    <h4 class="text-xl font-bold text-purple-300 mb-3 ai-text" data-en="Phase 2: Empirical Technical Validation" data-ru="Этап 2: Эмпирическая техническая проверка">අදියර 2: ආනුභවික තාක්ෂණික වලංගුකරණය</h4>
                    <p class="text-sm text-gray-300 mb-2 ai-text" data-en="Test Environment: Sandboxed environment utilizing CNNs and LSTM." data-ru="Тестовая среда: Изолированная среда с использованием CNN и LSTM."><strong>පරීක්ෂණ පරිසරය:</strong> CNNs සහ LSTM යොදා ගනිමින් හුදකලා (sandboxed) පරිසරයක්.</p>
                    <p class="text-sm text-gray-300 mb-2 ai-text" data-en="Datasets: UNSW-NB15 and CICIDS2017." data-ru="Наборы данных: UNSW-NB15 и CICIDS2017."><strong>දත්ත සමුදායන්:</strong> UNSW-NB15 සහ CICIDS2017.</p>
                    <p class="text-sm text-gray-300 mb-2 ai-text" data-en="XAI Deployment: Integrating SHAP (global feature impact) and LIME (local interpretation)." data-ru="Развертывание XAI: Интеграция SHAP (влияние глобальных функций) и LIME (локальная интерпретация)."><strong>XAI යෙදවීම:</strong> SHAP (ගෝලීය ලක්ෂණ බලපෑම) සහ LIME (දේශීය අර්ථ නිරූපණය) ඒකාබද්ධ කිරීම.</p>
                    <p class="text-sm text-gray-300 ai-text" data-en="Forensic Translation: Conversion into Algorithmic Expert Witness Reports." data-ru="Криминалистический перевод: Преобразование в алгоритмические отчеты свидетелей-экспертов."><strong>අධිකරණ පරිවර්තනය:</strong> Algorithmic Expert Witness Reports බවට පරිවර්තනය කිරීම.</p>
                </div>

                <div class="glass-panel p-6 rounded-2xl animate-on-scroll delay-200 hover:-translate-y-2 transition-transform">
                    <h4 class="text-xl font-bold text-pink-300 mb-3 ai-text" data-en="Phase 3: Ethical Governance & Risk Mitigation" data-ru="Этап 3: Этическое управление и снижение рисков">අදියර 3: ආචාරධාර්මික පාලනය සහ අවදානම් අවම කිරීම</h4>
                    <p class="text-sm text-gray-300 mb-2 ai-text" data-en="International Standards: Alignment with ISO/IEC 27037 and ISO/IEC 27041." data-ru="Международные стандарты: Согласование с ISO/IEC 27037 и ISO/IEC 27041."><strong>ජාත්‍යන්තර ප්‍රමිතීන්:</strong> ISO/IEC 27037 සහ ISO/IEC 27041 සමඟ පෙළගැස්වීම.</p>
                    <p class="text-sm text-gray-300 mb-2 ai-text" data-en="Ethical Governance: Implementing ISO/IEC 42001 and detecting algorithmic bias." data-ru="Этическое управление: Внедрение ISO/IEC 42001 и обнаружение алгоритмической предвзятости."><strong>ආචාරධාර්මික පාලනය:</strong> ISO/IEC 42001 ක්‍රියාත්මක කිරීම සහ ඇල්ගොරිතම පක්ෂග්‍රාහීත්වය හඳුනා ගැනීම.</p>
                    <p class="text-sm text-gray-300 ai-text" data-en="Risk Mitigation: Threat modeling against Data Poisoning and AML using NIST taxonomy." data-ru="Снижение рисков: Моделирование угроз против отравления данных и AML с использованием таксономии NIST."><strong>අවදානම් අවම කිරීම:</strong> NIST වර්ගීකරණය භාවිතයෙන් Data Poisoning සහ AML තර්ජන ආකෘතිකරණය.</p>
                </div>
            </div>

            <!-- Analysis Limitations -->
            <div class="glass-panel p-8 rounded-2xl border-dashed border-2 border-slate-700 animate-on-scroll">
                <h3 class="text-2xl font-bold text-white mb-4 ai-text" data-en="Analysis Limitations" data-ru="Ограничения анализа">Analysis Limitations (විශ්ලේෂණ සීමාවන්)</h3>
                <ul class="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm text-gray-400 list-disc list-inside">
                    <li class="ai-text" data-en="Proprietary Opacity: Research is restricted to open-source models as commercial core algorithms cannot be legally accessed." data-ru="Непрозрачность собственности: Исследования ограничиваются моделями с открытым исходным кодом, поскольку к коммерческим базовым алгоритмам нельзя получить законный доступ."><strong>වාණිජ හිමිකාරිත්වය (Proprietary Opacity):</strong> වාණිජ මූලික ඇල්ගොරිතම වෙත ප්‍රවේශ වීම නීත්‍යානුකූලව කළ නොහැකි බැවින් පර්යේෂණය විවෘත මූලාශ්‍ර ආකෘතිවලට සීමා වේ.</li>
                    <li class="ai-text" data-en="Complex Computing Resource Needs: Calculating SHAP values requires significant GPU power. Generating LIME explanations in real-time can cause system latency." data-ru="Сложные потребности в вычислительных ресурсах: Вычисление значений SHAP требует значительной мощности графического процессора."><strong>සංකීර්ණ පරිගණක සම්පත් අවශ්‍යතාවය:</strong> SHAP අගයන් ගණනය කිරීම සඳහා විශාල GPU බලයක් අවශ්‍ය වේ. LIME පැහැදිලි කිරීම් තත්‍ය කාලීනව ජනනය කිරීම පද්ධති ප්‍රමාදයන් ඇති කළ හැක.</li>
                    <li class="ai-text" data-en="Lack of Legal Precedents: Policy recommendations rely on predictive principle modeling due to a lack of global case law." data-ru="Отсутствие юридических прецедентов: Политические рекомендации опираются на прогнозное принципиальное моделирование из-за отсутствия глобального прецедентного права."><strong>නීතිමය පූර්වාදර්ශවල හිඟකම:</strong> ගෝලීය වශයෙන් තීන්දු වූ නඩු නීතියක් නොමැති බැවින් ප්‍රතිපත්ති නිර්දේශයන් අනාවැකිමය මූලධර්ම ආකෘතිකරණය මත රඳා පවතී.</li>
                    <li class="ai-text" data-en="Model Drift: Predictive accuracy of AI models gradually degrades over time as cybercriminals refine new attack methods." data-ru="Дрейф модели: Прогностическая точность моделей ИИ постепенно снижается со временем, поскольку киберпреступники совершенствуют новые методы атак."><strong>Model Drift:</strong> සයිබර් අපරාධකරුවන් නව ප්‍රහාර ක්‍රමවේදයන් වැඩිදියුණු කරන බැවින් AI ආකෘතිවල අනාවැකිමය නිරවද්‍යතාවය ක්‍රමයෙන් නැති වී යයි.</li>
                </ul>
            </div>
        </div>
    </section>

    <!-- Outcomes & Timeline -->
    <section id="timeline" class="relative py-24 px-6 md:px-12 search-content">
        <div class="max-w-6xl mx-auto space-y-16">
            
            <!-- Expected Outcomes -->
            <div class="animate-on-scroll">
                <h2 class="text-3xl font-bold text-white mb-6 ai-text" data-en="Expected Outcomes" data-ru="Ожидаемые результаты">Expected Outcomes (අපේක්ෂිත ප්‍රතිඵල)</h2>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-6 text-sm text-gray-300">
                    <div class="glass-panel p-6 rounded-xl flex items-start gap-4">
                        <div class="text-2xl text-sky-400 font-bold">1</div>
                        <div class="ai-text" data-en="<strong>Implementation of XAI-CF Operational Architecture:</strong> Formulation and validation of the first unified XAI-CF framework tailored for Sri Lankan infrastructure." data-ru="<strong>Реализация операционной архитектуры XAI-CF:</strong> Разработка и валидация первой единой структуры XAI-CF, адаптированной для инфраструктуры Шри-Ланки.">
                            <strong class="text-white block mb-1">XAI-CF මෙහෙයුම් ගෘහ නිර්මාණ ශිල්පය ක්‍රියාත්මක කිරීම:</strong>
                            ශ්‍රී ලාංකේය යටිතල පහසුකම් සඳහා සකස් කරන ලද, ප්‍රථම ඒකාබද්ධ XAI-CF මෙහෙයුම් රාමුව සකස් කර වලංගු කිරීම. මෙය සියලුම AI-ජනනය කරන ලද සාක්ෂි නීත්‍යානුකූලව ආරක්ෂාකාරී බව සහතික කරයි.
                        </div>
                    </div>
                    <div class="glass-panel p-6 rounded-xl flex items-start gap-4">
                        <div class="text-2xl text-sky-400 font-bold">2</div>
                        <div class="ai-text" data-en="<strong>Comprehensive Legislative Amendment Roadmaps:</strong> Modernizing language in Section 5 of the 1995 Act and proposing emergency exceptions for the 45-day notice period in Section 7." data-ru="<strong>Всеобъемлющие дорожные карты внесения поправок в законодательство:</strong> Модернизация языка в разделе 5 Закона 1995 года и предложение экстренных исключений для 45-дневного периода уведомления в разделе 7.">
                            <strong class="text-white block mb-1">සවිස්තරාත්මක ව්‍යවස්ථාදායක සංශෝධන මාර්ග සිතියම්:</strong>
                            1995 අංක 14 පනතේ 5 වගන්තියේ භාෂාව නවීකරණය කිරීම සහ 7 වගන්තියේ දින 45ක දැනුම් දීමේ නියමය සඳහා හදිසි ව්‍යතිරේක යෝජනා කිරීම.
                        </div>
                    </div>
                    <div class="glass-panel p-6 rounded-xl flex items-start gap-4">
                        <div class="text-2xl text-sky-400 font-bold">3</div>
                        <div class="ai-text" data-en="<strong>Capacity Building and Judicial Training Modules:</strong> Direct integration of research findings into ongoing capacity-building programs at SLJI and National Police Training Institutes." data-ru="<strong>Наращивание потенциала и модули судебного обучения:</strong> Прямая интеграция результатов исследований в текущие программы наращивания потенциала в SLJI и Национальных институтах подготовки полиции.">
                            <strong class="text-white block mb-1">ධාරිතා ගොඩනැගීම සහ අධිකරණ පුහුණු මොඩියුල:</strong>
                            ශ්‍රී ලංකා විනිසුරුවරුන්ගේ ආයතනයේ (SLJI) සහ ජාතික පොලිස් පුහුණු ආයතනවල ධාරිතා වර්ධන වැඩසටහන්වලට පර්යේෂණ ප්‍රතිඵල සෘජුවම ඒකාබද්ධ කිරීම.
                        </div>
                    </div>
                    <div class="glass-panel p-6 rounded-xl flex items-start gap-4">
                        <div class="text-2xl text-sky-400 font-bold">4</div>
                        <div class="ai-text" data-en="<strong>International Harmony and Cooperation:</strong> By grounding the methodology on ISO/IEC 42001 and NIST AML, Sri Lanka prepares to easily align with MLATs like the Budapest Convention." data-ru="<strong>Международная гармония и сотрудничество:</strong> Основывая методологию на ISO/IEC 42001 и NIST AML, Шри-Ланка готовится легко присоединиться к договорам о взаимной правовой помощи (MLAT), таким как Будапештская конвенция.">
                            <strong class="text-white block mb-1">ජාත්‍යන්තර එකඟතාව සහ සහයෝගීතාවය:</strong>
                            ISO/IEC 42001 ප්‍රමිතීන් සහ NIST AML මාර්ගෝපදේශ මත පදනම් වීමෙන්, ශ්‍රී ලංකාව බුඩාපෙස්ට් සම්මුතිය වැනි ජාත්‍යන්තර නෛතික සහය ගිවිසුම් (MLATs) සමඟ පෙළගැස්වීමට සූදානම් වේ.
                        </div>
                    </div>
                </div>
            </div>

            <!-- Research Timeline -->
            <div class="animate-on-scroll">
                <h2 class="text-3xl font-bold text-white mb-6 ai-text" data-en="Research Timeline" data-ru="График исследований">Research Timeline (පුරෝකථනය කරන ලද පර්යේෂණ කාලසටහන)</h2>
                <p class="text-sm text-gray-400 mb-4 ai-text" data-en="The complex, multi-modal research workflow is planned to be systematically executed over a 12-month structured timeline." data-ru="Сложный многомодальный рабочий процесс исследования планируется систематически выполнять в течение 12-месячного структурированного графика.">සංකීර්ණ, බහු-මාදිලියේ පර්යේෂණ කාර්ය ප්‍රවාහය මාස 12ක ව්‍යුහගත කාලසටහනක් හරහා ක්‍රමානුකූලව ක්‍රියාත්මක කිරීමට සැලසුම් කර ඇත.</p>
                <div class="overflow-x-auto glass-panel rounded-2xl">
                    <table class="w-full text-sm min-w-[600px]">
                        <thead>
                            <tr>
                                <th class="w-1/4 ai-text" data-en="Project Phase" data-ru="Фаза проекта">ව්‍යාපෘති අදියර</th>
                                <th class="w-1/2 ai-text" data-en="Core Analysis Activities and Strategic Outcomes" data-ru="Основные аналитические мероприятия и стратегические результаты">ප්‍රධාන විශ්ලේෂණ ක්‍රියාකාරකම් සහ උපායමාර්ගික ප්‍රතිඵල</th>
                                <th class="w-1/4 text-right ai-text" data-en="Timeframe (2026)" data-ru="Временные рамки (2026)">කාල රාමුව (2026)</th>
                            </tr>
                        </thead>
                        <tbody class="text-gray-300 align-top">
                            <tr>
                                <td class="font-semibold text-sky-300 ai-text" data-en="Phase 1: Doctrinal Research & Literature Review" data-ru="Этап 1: Доктринальные исследования и обзор литературы">අදියර 1: මූලධර්මාත්මක පර්යේෂණ සහ ගැඹුරු සාහිත්‍ය සමාලෝචනය</td>
                                <td class="ai-text" data-en="PRISMA review; Sri Lankan constitutional analysis; Daubert standard and international case law evaluation." data-ru="Обзор PRISMA; Конституционный анализ Шри-Ланки; Оценка стандарта Daubert и международного прецедентного права.">PRISMA සමාලෝචනය; ශ්‍රී ලංකා ව්‍යවස්ථා විශ්ලේෂණය; Daubert ප්‍රමිතිය සහ ජාත්‍යන්තර නඩු නීතිය ඇගයීම.</td>
                                <td class="text-right text-pink-300 ai-text" data-en="Months 1 - 3" data-ru="Месяцы 1 - 3">මාස 1 - 3</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-300 ai-text" data-en="Phase 2: Environment Setup & Data Preprocessing" data-ru="Этап 2: Настройка среды и предварительная обработка данных">අදියර 2: පරිසර සැකසීම සහ දත්ත පූර්ව සැකසීම</td>
                                <td class="ai-text" data-en="Procuring UNSW-NB15 / CICIDS2017 data; setting up GPU environments; data normalization." data-ru="Получение данных UNSW-NB15 / CICIDS2017; Настройка среды графического процессора; Нормализация данных.">UNSW-NB15 / CICIDS2017 දත්ත ලබා ගැනීම; GPU පරිසරයන් සකස් කිරීම; දත්ත සාමාන්‍යකරණය.</td>
                                <td class="text-right text-pink-300 ai-text" data-en="Month 4" data-ru="Месяц 4">මාස 4</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-300 ai-text" data-en="Phase 3: AI Model Training & Baseline Validation" data-ru="Этап 3: Обучение модели ИИ и базовая проверка">අදියර 3: AI ආකෘති පුහුණුව සහ මූලික මිණුම්දණ්ඩ වලංගුකරණය</td>
                                <td class="ai-text" data-en="Developing deep learning classifiers (CNN/LSTM); initial performance evaluation; documenting black-box limitations." data-ru="Разработка классификаторов глубокого обучения (CNN/LSTM); Первоначальная оценка эффективности; Документирование ограничений черного ящика.">ගැඹුරු ඉගෙනුම් වර්ගීකරණ යන්ත්‍ර (CNN/LSTM) සංවර්ධනය; මූලික කාර්ය සාධන ඇගයීම; කළු පෙට්ටි සීමාවන් ලේඛනගත කිරීම.</td>
                                <td class="text-right text-pink-300 ai-text" data-en="Months 5 - 6" data-ru="Месяцы 5 - 6">මාස 5 - 6</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-300 ai-text" data-en="Phase 4: XAI Integration (SHAP/LIME Implementation)" data-ru="Этап 4: Интеграция XAI (реализация SHAP/LIME)">අදියර 4: XAI ඒකාබද්ධ කිරීම (SHAP/LIME ක්‍රියාත්මක කිරීම)</td>
                                <td class="ai-text" data-en="Applying SHAP for global feature impact; LIME for local explanations; generating complex explanation visualizations." data-ru="Применение SHAP для глобального воздействия функций; LIME для местных пояснений; Создание сложных визуализаций объяснений.">ගෝලීය ලක්ෂණ බලපෑම සඳහා SHAP යෙදවීම; දේශීය පැහැදිලි කිරීම් සඳහා LIME යෙදවීම; සංකීර්ණ පැහැදිලි කිරීමේ දෘශ්‍යකරණයන් ජනනය කිරීම.</td>
                                <td class="text-right text-pink-300 ai-text" data-en="Months 7 - 8" data-ru="Месяцы 7 - 8">මාස 7 - 8</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-300 ai-text" data-en="Phase 5: Mapping Forensic Standards and Ethics" data-ru="Этап 5: Сопоставление судебно-медицинских стандартов и этики">අදියර 5: අධිකරණ වෛද්‍ය ප්‍රමිතීන් සහ ආචාර ධර්ම සිතියම්ගත කිරීම</td>
                                <td class="ai-text" data-en="Cross-referencing all AI outputs and methodologies against ISO/IEC 42001, ISO 27037/27041, and NIST AML standards." data-ru="Перекрестные ссылки на все результаты и методологии ИИ в соответствии со стандартами ISO/IEC 42001, ISO 27037/27041 и NIST AML.">ISO/IEC 42001, ISO 27037/27041, සහ NIST AML ප්‍රමිතීන්ට එරෙහිව සියලුම AI ප්‍රතිදාන සහ ක්‍රමවේද හරස්-යොමු කිරීම.</td>
                                <td class="text-right text-pink-300 ai-text" data-en="Month 9" data-ru="Месяц 9">මාස 9</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-300 ai-text" data-en="Phase 6: Framework Synthesis and Policy Drafting" data-ru="Этап 6: Синтез рамок и разработка политики">අදියර 6: රාමු සංශ්ලේෂණය සහ ප්‍රතිපත්ති කෙටුම්පත් කිරීම</td>
                                <td class="ai-text" data-en="Final integration of the XAI-CF framework; formal drafting of proposed legislative amendments for the Evidence Act and ETA." data-ru="Окончательная интеграция системы XAI-CF; Официальная разработка предлагаемых поправок в законодательство к Закону о доказательствах и ETA.">XAI-CF රාමුවේ අවසාන ඒකාබද්ධ කිරීම; සාක්ෂි විශේෂ විධිවිධාන පනත සහ ETA සඳහා යෝජිත ව්‍යවස්ථාදායක සංශෝධන විධිමත් ලෙස කෙටුම්පත් කිරීම.</td>
                                <td class="text-right text-pink-300 ai-text" data-en="Months 10 - 11" data-ru="Месяцы 10 - 11">මාස 10 - 11</td>
                            </tr>
                            <tr>
                                <td class="font-semibold text-sky-300 ai-text" data-en="Phase 7: Final Review and Conference Prep" data-ru="Этап 7: Заключительный обзор и подготовка к конференции">අදියර 7: අවසාන සමාලෝචනය සහ සමුළු සූදානම් කිරීම</td>
                                <td class="ai-text" data-en="Rigorous peer review of all findings by subject matter experts; finalizing the formal research report." data-ru="Тщательная экспертная оценка всех результатов профильными экспертами; Доработка официального отчета об исследовании.">විෂය විශේෂඥයින් විසින් සියලුම සොයාගැනීම් දැඩි ලෙස සම-සමාලෝචනය කිරීම; විධිමත් පර්යේෂණ වාර්තාව අවසන් කිරීම.</td>
                                <td class="text-right text-pink-300 ai-text" data-en="Month 12" data-ru="Месяц 12">මාස 12</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </section>

    <!-- Conclusion Section -->
    <section class="relative py-24 px-6 md:px-12 bg-slate-900/40 search-content">
        <div class="max-w-5xl mx-auto text-center animate-on-scroll">
            <h2 class="text-3xl md:text-5xl font-bold mb-8 text-glow ai-text" data-en="Conclusion" data-ru="Заключение">Conclusion (සංශ්ලේෂණය කළ නිගමනය)</h2>
            <div class="glass-panel p-8 md:p-12 rounded-3xl text-gray-300 text-base md:text-lg text-justify space-y-6 leading-relaxed">
                <p class="ai-text" data-en="The rapid deployment of AI in digital forensics represents a highly critical and dangerous turning point for the global criminal justice system. The incredible speed and analytical depth provided by machine learning algorithms are essential tools for law enforcement agencies. However, this technological extraction cannot bypass fundamental constitutional mandates regarding judicial transparency, scientific reliability, and the authenticity of evidence." data-ru="Быстрое внедрение ИИ в цифровую криминалистику представляет собой крайне критический и опасный поворотный момент для глобальной системы уголовного правосудия. Невероятная скорость и аналитическая глубина, обеспечиваемые алгоритмами машинного обучения, являются важными инструментами для правоохранительных органов. Однако это технологическое извлечение не может обойти фундаментальные конституционные мандаты, касающиеся судебной прозрачности, научной надежности и подлинности доказательств.">ඩිජිටල් අධිකරණ වෛද්‍ය විද්‍යාව තුළ AI (කෘතිම බුද්ධිය) වේගයෙන් යෙදවීම, ගෝලීය අපරාධ යුක්ති විනිශ්චය පද්ධතියට අතිශය තීරණාත්මක සහ අනතුරුදායක සන්ධිස්ථානයක් නියෝජනය කරයි. යන්ත්‍ර ඉගෙනුම් ඇල්ගොරිතම මගින් සපයනු ලබන පුදුමාකාර වේගය සහ විශ්ලේෂණ ගැඹුර, නීතිය ක්‍රියාත්මක කරන ආයතන සඳහා අත්‍යවශ්‍ය මෙවලම් වේ. කෙසේ වෙතත්, මෙම තාක්ෂණික නිස්සාරණය, අධිකරණ විනිවිදභාවය, විද්‍යාත්මක විශ්වසනීයත්වය සහ සාක්ෂිවල සත්‍යතාව පිළිබඳ මූලික ආණ්ඩුක්‍රම ව්‍යවස්ථාමය නියමයන් මග හැරිය නොහැක.</p>
                
                <p class="ai-text" data-en="Continued reliance on opaque Black-box computer methodologies overrides the vital chain of trust required for legal Admissibility. This technological opacity places an impossible evidentiary burden on outdated statutory frameworks like Sri Lanka's 1995 Evidence Act and the 2006 Electronic Transactions Act. When the legal system cannot understand the tools used to determine a conviction, the fundamental basis of Due Process is severely compromised." data-ru="Постоянная зависимость от непрозрачных компьютерных методологий черного ящика перекрывает жизненно важную цепочку доверия, необходимую для юридической допустимости. Эта технологическая непрозрачность ложится непосильным доказательственным бременем на устаревшие законодательные базы. Когда правовая система не может понять инструменты, используемые для вынесения обвинительного приговора, фундаментальная основа надлежащей правовой процедуры серьезно подорвана.">අපැහැදිලි Black-box (කළු පෙට්ටි) පරිගණක ක්‍රමවේද මත අඛණ්ඩව රඳා පැවතීම, නෛතික Admissibility (පිළිගත හැකි බව) සඳහා අවශ්‍ය වන අතිශය වැදගත් විශ්වාස දාමය අභිබවා යයි. මෙම තාක්ෂණික අපැහැදිලි බව, ශ්‍රී ලංකාවේ 1995 සාක්ෂි (විශේෂ විධිවිධාන) පනත සහ 2006 විද්‍යුත් ගනුදෙනු පනත වැනි යල් පැන ගිය ව්‍යවස්ථාපිත රාමු මත කළ නොහැකි සාක්ෂිමය බරක් පටවයි. නීති පද්ධතියට වරදකරුවෙකු තීරණය කිරීමට භාවිතා කරන මෙවලම් තේරුම් ගැනීමට නොහැකි වූ විට, Due Process (නිසි ක්‍රියා පටිපාටියේ) මූලික පදනම බරපතල ලෙස අඩාල වේ.</p>

                <p class="ai-text" data-en="This comprehensive research plan outlines a strategic, empirical intervention to systematically resolve this legal and technical tension. By rigorously examining the volatile intersection of algorithmic processing and jurisprudential Due Process, and by implementing the complex mathematical paradigms of XAI (SHAP and LIME), the proposed study goes beyond theoretical critique. It provides an operational framework that translates complex, non-deterministic AI outputs into fully auditable, rigorously peer-reviewable, and easily human-interpretable expert evidence. Ultimately, implementing this research will empower local law enforcement agencies and the judiciary to safely utilize the full potential of artificial intelligence without sacrificing the fundamental rights of Due Process." data-ru="Этот комплексный план исследований намечает стратегическое эмпирическое вмешательство для систематического разрешения этой правовой и технической напряженности. Строго изучая нестабильное пересечение алгоритмической обработки и юриспруденциальной надлежащей правовой процедуры, предлагаемое исследование выходит за рамки теоретической критики. Оно обеспечивает оперативную основу, которая переводит результаты ИИ в проверяемые и легко интерпретируемые человеком экспертные доказательства.">මෙම සවිස්තරාත්මක පර්යේෂණ සැලැස්ම, එම නීතිමය හා තාක්ෂණික ආතතිය ක්‍රමානුකූලව විසඳීම සඳහා උපායමාර්ගික, ආනුභවික මැදිහත්වීමක් ගෙනහැර දක්වයි. ඇල්ගොරිතම සැකසීම සහ නීති විද්‍යාත්මක Due Process (නිසි ක්‍රියා පටිපාටිය) යන වාෂ්පශීලී සන්ධිස්ථානය දැඩි ලෙස පරීක්ෂා කිරීමෙන් සහ XAI හි (SHAP සහ LIME) සංකීර්ණ ගණිතමය ආදර්ශ ක්‍රියාත්මක කිරීමෙන්, යෝජිත අධ්‍යයනය න්‍යායික විවේචනවලින් ඔබ්බට යයි. එය සංකීර්ණ, නිර්ණායක නොවන AI ප්‍රතිදාන, සම්පූර්ණයෙන් විගණනය කළ හැකි, දැඩි ලෙස සම-සමාලෝචනය කළ හැකි සහ මිනිසුන්ට පහසුවෙන් අර්ථකථනය කළ හැකි විශේෂඥ සාක්ෂි බවට පරිවර්තනය කරන මෙහෙයුම් රාමුවක් සපයයි. අවසානයේදී, මෙම පර්යේෂණය ක්‍රියාත්මක කිරීම මඟින්, Due Process (නිසි ක්‍රියා පටිපාටියේ) මූලික අයිතිවාසිකම් කැප නොකර, කෘතිම බුද්ධියේ සම්පූර්ණ විභවය ආරක්ෂිතව භාවිතා කිරීමට දේශීය නීතිය ක්‍රියාත්මක කරන ආයතන සහ අධිකරණය සවිබල ගන්වයි.</p>
            </div>
        </div>
    </section>

    <!-- References Section -->
    <section class="relative py-12 px-6 md:px-12 search-content">
        <div class="max-w-6xl mx-auto">
            <div class="glass-panel p-6 rounded-2xl animate-on-scroll">
                <h3 class="text-xl font-bold mb-4 text-white border-b border-slate-700 pb-2 ai-text" data-en="Works Cited - 35 References" data-ru="Цитируемые работы - 35 ссылок">Works Cited (උපුටා දැක්වූ කෘති) - 35 References</h3>
                <div class="h-64 overflow-y-auto pr-4 custom-scrollbar text-xs text-gray-400 space-y-2 break-all">
                    <!-- Note: References are intentionally kept out of AI translation to save bandwidth and preserve URLs -->
                    <p>1. RELIABILITY AND ADMISSIBILITY OF AI-GENERATED ... - arXiv, https://arxiv.org/pdf/2601.06048</p>
                    <p>2. AUTHENTICITY OF DIGITAL EVIDENCE AND CURRENT ..., https://www.unafei.or.jp/publications/pdf/RS_No114/No114_25_PART_THREE_Participants_Papers_04.pdf</p>
                    <p>3. Sri Lanka CERT, https://www.cert.gov.lk/</p>
                    <p>4. Explainable AI for Forensic Analysis: A Comparative Study of SHAP and LIME in Intrusion Detection Models - MDPI, https://www.mdpi.com/2076-3417/15/13/7329</p>
                    <p>5. THE USE OF DIGITAL EVIDENCE IN PROSECUTIONS IN ASIA EXECUTIVE SUMMARY - Interpol, https://www.interpol.int/content/download/17171/file/The%20Use%20of%20Digital%20Evidence%20in%20Prosecutions%20in%20Asia....pdf</p>
                    <p>6. Locard's Exchange Principle: 5 things to know about crime scene investigation - Police1, https://www.police1.com/investigations/5-things-to-know-about-locards-principle</p>
                    <p>7. The Digital Forensics Cyber Exchange Principle - Department of Computing Sciences, http://www.csc.villanova.edu/~dprice/9010sp14/extra_handouts/The_Digital_Forensics_Cyber_Exchange_Principle_-_2013-04-22.pdf</p>
                    <p>8. XAI-CF – Examining the Role of Explainable Artificial Intelligence in Cyber Forensics - arXiv, https://arxiv.org/html/2402.02452v2</p>
                    <p>9. Use of Explainable Artificial Intelligence for Analyzing and Explaining Intrusion Detection Systems - Preprints.org, https://www.preprints.org/manuscript/202503.2069</p>
                    <p>10. Explainable AI for Digital Forensics: Ensuring Transparency in Legal Evidence Analysis, https://www.forensicscijournal.com/journals/jfsr/jfsr-aid1089.php</p>
                    <p>11. Adversarial Machine Learning: A Taxonomy and Terminology of Attacks and Mitigations - NIST Technical Series Publications, https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-2e2025.pdf</p>
                    <p>12. ISO/IEC 42001: a new standard for AI governance - KPMG International, https://kpmg.com/ch/en/insights/artificial-intelligence/iso-iec-42001.html</p>
                    <p>13. Cybercrime Module 4 Key Issues: Standards and best practices for digital forensics, https://www.unodc.org/cld/en/education/tertiary/cybercrime/module-4/key-issues/standards-and-best-practices-for-digital-forensics.html</p>
                    <p>14. Reliability and Admissibility of AI-Generated Forensic Evidence in Criminal Trials - arXiv, https://arxiv.org/abs/2601.06048</p>
                    <p>15. AI Risk Management Framework | NIST - National Institute of Standards and Technology, https://www.nist.gov/itl/ai-risk-management-framework</p>
                    <p>16. ISO 42001: Auditing and Implementing Framework - Cloud Security Alliance (CSA), https://cloudsecurityalliance.org/blog/2025/05/08/iso-42001-lessons-learned-from-auditing-and-implementing-the-framework</p>
                    <p>17. Challenges in investigating Cybercrime in social networks: A Sri Lankan Perspective, http://ir.kdu.ac.lk/bitstream/handle/345/3004/FOC%20506-514.pdf?sequence=1&isAllowed=y</p>
                    <p>18. Sri Lanka CERT Reports Sharp Rise In Phishing And Ransomware Incidents, https://cybersecurityventures.com/sri-lanka-cert-reports-sharp-rise-in-phishing-and-ransomware-incidents/</p>
                    <p>19. Cybercrime and Computer Forensics in Epoch of Artificial Intelligence in India - arXiv, https://arxiv.org/abs/2512.15799</p>
                    <p>20. XAI-CF -- Examining the Role of Explainable Artificial ... - arXiv, https://arxiv.org/pdf/2402.02452</p>
                    <p>21. Sri Lanka - Octopus Cybercrime Community - The Council of Europe, https://www.coe.int/en/web/octopus/-/sri-lanka</p>
                    <p>22. Trace Evidence: Principles - Forensic Science Simplified, https://www.forensicsciencesimplified.org/trace/principles.html</p>
                    <p>23. Every contact leaves a trace - PMC - NIH, https://pmc.ncbi.nlm.nih.gov/articles/PMC8544144/</p>
                    <p>24. Locard's exchange principle | Science | Research Starters - EBSCO, https://www.ebsco.com/research-starters/science/locards-exchange-principle</p>
                    <p>25. Appreciation of Digital Evidence in Sri Lankan Law | PPT - Slideshare, https://www.slideshare.net/slideshow/appreciation-of-digital-evidence-in-sri-lankan-law/30702489</p>
                    <p>26. 6.2. Electronic Transactions Act No. 19 of 2006. - CA Sri Lanka, https://www.casrilanka.com/casl/images/stories/EDBA/electronic%20transactions%20act%20no.%2019%20of%202006.pdf</p>
                    <p>27. Evidence (Special Provisions) - The Parliament of Sri Lanka, https://www.parliament.lk/uploads/acts/gbills/english/3114.pdf</p>
                    <p>28. Computer Evidence Admissibility in Sri Lanka | PDF | Affidavit - Scribd, https://www.scribd.com/document/834642384/final-vol-2-issue-1-1-54-69</p>
                    <p>29. Admissibility of Computer Evidence in Sri Lankan Courts: A Comparative Analysis with Other Jurisdictions, https://ir.kdu.ac.lk/bitstream/handle/345/5370/final%20vol%202%20issue%201%20%281%29-54-69.pdf?sequence=1&isAllowed=y</p>
                    <p>30. (PDF) Use of Electronic Evidence in Sri Lanka - ResearchGate, https://www.researchgate.net/publication/385907396_Use_of_Electronic_Evidence_in_Sri_Lanka</p>
                    <p>31. Evidence (Special Provisions) Act - Laws of Sri Lanka, https://www.srilankalaw.lk/revised-statutes/volume-iii/352-evidence-special-provisions-act.html</p>
                    <p>32. An Empirical Assessment of Digital Forensic Process Reliability Using Integrated ISO/IEC 27037 and 27041 Standards - MDPI, https://www.mdpi.com/2624-800X/6/2/57</p>
                    <p>33. How NIST AI Framework Can be Used for ISO/IEC 17024 Compliance - ANAB Blog, https://blog.ansi.org/anab/nist-ai-framework-iso-iec-17024-compliance/</p>
                    <p>34. NIST Trustworthy and Responsible AI Report Adversarial Machine Learning: A Taxonomy and Terminology of Attacks and Mitigations - National Institute of Standards and Technology, https://www.nist.gov/news-events/news/2025/03/nist-trustworthy-and-responsible-ai-report-adversarial-machine-learning</p>
                    <p>35. Electronic Evidence Country Fiche: SRI LANKA - UNODC, https://www.unodc.org/cld/uploads/pdf/El%20Evidence%20Hub/SOUTH_ASIA/eEvidence_Country_Fiche_SRI_LANKA.pdf</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="glass-panel border-t border-slate-800 py-8 px-6 text-center z-10 relative mt-12">
        <p class="text-gray-500 text-sm mb-2 ai-text" data-en="Research by Jayasuriya Kuranage Shenol Ravisha Perera &copy; 2026" data-ru="Исследование Джаясурии Куранаге Шенола Равиша Переры &copy; 2026">Research by Jayasuriya Kuranage Shenol Ravisha Perera &copy; 2026</p>
    </footer>

    <!-- Scripts (Offline Translation, Search, Three.js) -->
    <script>
        /* Aa function offline translation mate chhe (This function handles offline translation instantly) */
        function translatePageOffline(lang, langName) {
            const elements = document.querySelectorAll('.ai-text');
            elements.forEach(el => {
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
                    el.innerHTML = el.getAttribute('data-si');
                }
            });
            document.getElementById('current-lang').innerText = langName || (lang === 'en' ? 'English' : (lang === 'ru' ? 'Русский' : 'සිංහල'));
        }

        /* Page load thaya pachi popup nu function (Function for initial popup selection) */
        function setInitialLanguage(lang, langName) {
            translatePageOffline(lang, langName);
            const popup = document.getElementById('language-popup');
            popup.style.opacity = '0';
            setTimeout(() => {
                popup.style.display = 'none';
            }, 300);
        }

        window.onload = function() {

            // --- 1. SEARCH / HIGHLIGHT FUNCTIONALITY ---
            const searchInput = document.getElementById('keyword-search');
            let timeout = null;

            searchInput.addEventListener('input', function() {
                clearTimeout(timeout);
                const term = this.value.trim();
                
                timeout = setTimeout(() => {
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

            /* Neural Network Particles banavava mate (To create neural network particles) */
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

            /* Animation loop continuous farva mate (To rotate animation continuously) */
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

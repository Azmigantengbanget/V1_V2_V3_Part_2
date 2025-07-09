<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Bentuk past tense dari 'train'?", "id": "Trained (melatih)." },
  { "en": "Bentuk past participle dari 'train'?", "id": "Trained (melatih)." },
  { "en": "Bentuk past tense dari 'transport'?", "id": "Transported (mengangkut)." },
  { "en": "Bentuk past participle dari 'transport'?", "id": "Transported (mengangkut)." },
  { "en": "Bentuk past tense dari 'trap'?", "id": "Trapped (menjebak)." },
  { "en": "Bentuk past participle dari 'trap'?", "id": "Trapped (menjebak)." },
  { "en": "Bentuk past tense dari 'tremble'?", "id": "Trembled (gemetar)." },
  { "en": "Bentuk past participle dari 'tremble'?", "id": "Trembled (gemetar)." },
  { "en": "Bentuk past tense dari 'trick'?", "id": "Tricked (menipu)." },
  { "en": "Bentuk past participle dari 'trick'?", "id": "Tricked (menipu)." },
  { "en": "Bentuk past tense dari 'trip'?", "id": "Tripped (tersandung)." },
  { "en": "Bentuk past participle dari 'trip'?", "id": "Tripped (tersandung)." },
  { "en": "Bentuk past tense dari 'trot'?", "id": "Trotted (berlari kecil)." },
  { "en": "Bentuk past participle dari 'trot'?", "id": "Trotted (berlari kecil)." },
  { "en": "Bentuk past tense dari 'trouble'?", "id": "Troubled (menyusahkan)." },
  { "en": "Bentuk past participle dari 'trouble'?", "id": "Troubled (menyusahkan)." },
  { "en": "Bentuk past tense dari 'trust'?", "id": "Trusted (mempercayai)." },
  { "en": "Bentuk past participle dari 'trust'?", "id": "Trusted (mempercayai)." },
  { "en": "Bentuk past tense dari 'type'?", "id": "Typed (mengetik)." },
  { "en": "Bentuk past participle dari 'type'?", "id": "Typed (mengetik)." },
  { "en": "Bentuk past tense dari 'underline'?", "id": "Underlined (menggarisbawahi)." },
  { "en": "Bentuk past participle dari 'underline'?", "id": "Underlined (menggarisbawahi)." },
  { "en": "Bentuk past tense dari 'undress'?", "id": "Undressed (membuka pakaian)." },
  { "en": "Bentuk past participle dari 'undress'?", "id": "Undressed (membuka pakaian)." },
  { "en": "Bentuk past tense dari 'unfasten'?", "id": "Unfastened (melepaskan)." },
  { "en": "Bentuk past participle dari 'unfasten'?", "id": "Unfastened (melepaskan)." },
  { "en": "Bentuk past tense dari 'unite'?", "id": "United (bersatu)." },
  { "en": "Bentuk past participle dari 'unite'?", "id": "United (bersatu)." },
  { "en": "Bentuk past tense dari 'unlock'?", "id": "Unlocked (membuka kunci)." },
  { "en": "Bentuk past participle dari 'unlock'?", "id": "Unlocked (membuka kunci)." },
  { "en": "Bentuk past tense dari 'unpack'?", "id": "Unpacked (membongkar)." },
  { "en": "Bentuk past participle dari 'unpack'?", "id": "Unpacked (membongkar)." },
  { "en": "Bentuk past tense dari 'untie'?", "id": "Untied (melepas ikatan)." },
  { "en": "Bentuk past participle dari 'untie'?", "id": "Untied (melepas ikatan)." },
  { "en": "Bentuk past tense dari 'vanish'?", "id": "Vanished (lenyap)." },
  { "en": "Bentuk past participle dari 'vanish'?", "id": "Vanished (lenyap)." },
  { "en": "Bentuk past tense dari 'vomit'?", "id": "Vomited (muntah)." },
  { "en": "Bentuk past participle dari 'vomit'?", "id": "Vomited (muntah)." },
  { "en": "Bentuk past tense dari 'wail'?", "id": "Wailed (meratap)." },
  { "en": "Bentuk past participle dari 'wail'?", "id": "Wailed (meratap)." },
  { "en": "Bentuk past tense dari 'wander'?", "id": "Wandered (mengembara)." },
  { "en": "Bentuk past participle dari 'wander'?", "id": "Wandered (mengembara)." },
  { "en": "Bentuk past tense dari 'wave'?", "id": "Waved (melambai)." },
  { "en": "Bentuk past participle dari 'wave'?", "id": "Waved (melambai)." },
  { "en": "Bentuk past tense dari 'weigh'?", "id": "Weighed (menimbang)." },
  { "en": "Bentuk past participle dari 'weigh'?", "id": "Weighed (menimbang)." },
  { "en": "Bentuk past tense dari 'welcome'?", "id": "Welcomed (menyambut)." },
  { "en": "Bentuk past participle dari 'welcome'?", "id": "Welcomed (menyambut)." },
  { "en": "Bentuk past tense dari 'whine'?", "id": "Whined (merengek)." },
  { "en": "Bentuk past participle dari 'whine'?", "id": "Whined (merengek)." },
  { "en": "Bentuk past tense dari 'whip'?", "id": "Whippped (mencambuk)." },
  { "en": "Bentuk past participle dari 'whip'?", "id": "Whippped (mencambuk)." },
  { "en": "Bentuk past tense dari 'whirl'?", "id": "Whirled (berputar)." },
  { "en": "Bentuk past participle dari 'whirl'?", "id": "Whirled (berputar)." },
  { "en": "Bentuk past tense dari 'whisper'?", "id": "Whispered (berbisik)." },
  { "en": "Bentuk past participle dari 'whisper'?", "id": "Whispered (berbisik)." },
  { "en": "Bentuk past tense dari 'whistle'?", "id": "Whistled (bersiul)." },
  { "en": "Bentuk past participle dari 'whistle'?", "id": "Whistled (bersiul)." },
  { "en": "Bentuk past tense dari 'wink'?", "id": "Winked (berkedip)." },
  { "en": "Bentuk past participle dari 'wink'?", "id": "Winked (berkedip)." },
  { "en": "Bentuk past tense dari 'wipe'?", "id": "Wiped (menyeka)." },
  { "en": "Bentuk past participle dari 'wipe'?", "id": "Wiped (menyeka)." },
  { "en": "Bentuk past tense dari 'wish'?", "id": "Wished (berharap)." },
  { "en": "Bentuk past participle dari 'wish'?", "id": "Wished (berharap)." },
  { "en": "Bentuk past tense dari 'wobble'?", "id": "Wobbled (bergoyang)." },
  { "en": "Bentuk past participle dari 'wobble'?", "id": "Wobbled (bergoyang)." },
  { "en": "Bentuk past tense dari 'wonder'?", "id": "Wondered (bertanya-tanya)." },
  { "en": "Bentuk past participle dari 'wonder'?", "id": "Wondered (bertanya-tanya)." },
  { "en": "Bentuk past tense dari 'wrap'?", "id": "Wrapped (membungkus)." },
  { "en": "Bentuk past participle dari 'wrap'?", "id": "Wrapped (membungkus)." },
  { "en": "Bentuk past tense dari 'wreck'?", "id": "Wrecked (merusak)." },
  { "en": "Bentuk past participle dari 'wreck'?", "id": "Wrecked (merusak)." },
  { "en": "Bentuk past tense dari 'wrestle'?", "id": "Wrestled (bergulat)." },
  { "en": "Bentuk past participle dari 'wrestle'?", "id": "Wrestled (bergulat)." },
  { "en": "Bentuk past tense dari 'wiggle'?", "id": "Wiggled (menggoyangkan)." },
  { "en": "Bentuk past participle dari 'wiggle'?", "id": "Wiggled (menggoyangkan)." },
  { "en": "Bentuk past tense dari 'x-ray'?", "id": "X-rayed (memindai dengan x-ray)." },
  { "en": "Bentuk past participle dari 'x-ray'?", "id": "X-rayed (memindai dengan x-ray)." },
  { "en": "Bentuk past tense dari 'yawn'?", "id": "Yawned (menguap)." },
  { "en": "Bentuk past participle dari 'yawn'?", "id": "Yawned (menguap)." },
  { "en": "Bentuk past tense dari 'yell'?", "id": "Yelled (berteriak)." },
  { "en": "Bentuk past participle dari 'yell'?", "id": "Yelled (berteriak)." },
  { "en": "Bentuk past tense dari 'yield'?", "id": "Yielded (menghasilkan/mengalah)." },
  { "en": "Bentuk past participle dari 'yield'?", "id": "Yielded (menghasilkan/mengalah)." },
  { "en": "Bentuk past tense dari 'zap'?", "id": "Zapped (menyetrum/menghancurkan)." },
  { "en": "Bentuk past participle dari 'zap'?", "id": "Zapped (menyetrum/menghancurkan)." },
  { "en": "Bentuk past tense dari 'zip'?", "id": "Zipped (meresleting)." },
  { "en": "Bentuk past participle dari 'zip'?", "id": "Zipped (meresleting)." },
  { "en": "Bentuk past tense dari 'zoom'?", "id": "Zoomed (memperbesar)." },
  { "en": "Bentuk past participle dari 'zoom'?", "id": "Zoomed (memperbesar)." },
  { "en": "Bentuk past tense dari 'acknowledge'?", "id": "Acknowledged (mengakui)." },
  { "en": "Bentuk past participle dari 'acknowledge'?", "id": "Acknowledged (mengakui)." },
  { "en": "Bentuk past tense dari 'acquire'?", "id": "Acquired (memperoleh)." },
  { "en": "Bentuk past participle dari 'acquire'?", "id": "Acquired (memperoleh)." },
  { "en": "Bentuk past tense dari 'adapt'?", "id": "Adapted (beradaptasi)." },
  { "en": "Bentuk past participle dari 'adapt'?", "id": "Adapted (beradaptasi)." },
  { "en": "Bentuk past tense dari 'adjust'?", "id": "Adjusted (menyesuaikan)." },
  { "en": "Bentuk past participle dari 'adjust'?", "id": "Adjusted (menyesuaikan)." },
  { "en": "Bentuk past tense dari 'admire'?", "id": "Admired (mengagumi)." },
  { "en": "Bentuk past participle dari 'admire'?", "id": "Admired (mengagumi)." },
  { "en": "Bentuk past tense dari 'admit'?", "id": "Admitted (mengaku)." },
  { "en": "Bentuk past participle dari 'admit'?", "id": "Admitted (mengaku)." },
  { "en": "Bentuk past tense dari 'adopt'?", "id": "Adopted (mengadopsi)." },
  { "en": "Bentuk past participle dari 'adopt'?", "id": "Adopted (mengadopsi)." },
  { "en": "Bentuk past tense dari 'adore'?", "id": "Adored (memuja)." },
  { "en": "Bentuk past participle dari 'adore'?", "id": "Adored (memuja)." },
  { "en": "Bentuk past tense dari 'advance'?", "id": "Advanced (maju)." },
  { "en": "Bentuk past participle dari 'advance'?", "id": "Advanced (maju)." },
  { "en": "Bentuk past tense dari 'advertise'?", "id": "Advertised (mengiklankan)." },
  { "en": "Bentuk past participle dari 'advertise'?", "id": "Advertised (mengiklankan)." },
  { "en": "Bentuk past tense dari 'affect'?", "id": "Affected (mempengaruhi)." },
  { "en": "Bentuk past participle dari 'affect'?", "id": "Affected (mempengaruhi)." },
  { "en": "Bentuk past tense dari 'afford'?", "id": "Afforded (mampu)." },
  { "en": "Bentuk past participle dari 'afford'?", "id": "Afforded (mampu)." },
  { "en": "Bentuk past tense dari 'alert'?", "id": "Alerted (memperingatkan)." },
  { "en": "Bentuk past participle dari 'alert'?", "id": "Alerted (memperingatkan)." },
  { "en": "Bentuk past tense dari 'amuse'?", "id": "Amused (menghibur)." },
  { "en": "Bentuk past participle dari 'amuse'?", "id": "Amused (menghibur)." },
  { "en": "Bentuk past tense dari 'analyze'?", "id": "Analyzed (menganalisis)." },
  { "en": "Bentuk past participle dari 'analyze'?", "id": "Analyzed (menganalisis)." },
  { "en": "Bentuk past tense dari 'applaud'?", "id": "Applauded (bertepuk tangan)." },
  { "en": "Bentuk past participle dari 'applaud'?", "id": "Applauded (bertepuk tangan)." },
  { "en": "Bentuk past tense dari 'appreciate'?", "id": "Appreciated (menghargai)." },
  { "en": "Bentuk past participle dari 'appreciate'?", "id": "Appreciated (menghargai)." },
  { "en": "Bentuk past tense dari 'approach'?", "id": "Approached (mendekati)." },
  { "en": "Bentuk past participle dari 'approach'?", "id": "Approached (mendekati)." },
  { "en": "Bentuk past tense dari 'arm'?", "id": "Armed (mempersenjatai)." },
  { "en": "Bentuk past participle dari 'arm'?", "id": "Armed (mempersenjatai)." },
  { "en": "Bentuk past tense dari 'arise'?", "id": "Arose (timbul/muncul)." },
  { "en": "Bentuk past participle dari 'arise'?", "id": "Arisen (timbul/muncul)." },
  { "en": "Bentuk past tense dari 'arouse'?", "id": "Aroused (membangkitkan)." },
  { "en": "Bentuk past participle dari 'arouse'?", "id": "Aroused (membangkitkan)." },
  { "en": "Bentuk past tense dari 'assemble'?", "id": "Assembled (merakit)." },
  { "en": "Bentuk past participle dari 'assemble'?", "id": "Assembled (merakit)." },
  { "en": "Bentuk past tense dari 'assist'?", "id": "Assisted (membantu)." },
  { "en": "Bentuk past participle dari 'assist'?", "id": "Assisted (membantu)." },
  { "en": "Bentuk past tense dari 'assume'?", "id": "Assumed (menganggap)." },
  { "en": "Bentuk past participle dari 'assume'?", "id": "Assumed (menganggap)." },
  { "en": "Bentuk past tense dari 'astonish'?", "id": "Astonished (mengherankan)." },
  { "en": "Bentuk past participle dari 'astonish'?", "id": "Astonished (mengherankan)." },
  { "en": "Bentuk past tense dari 'attach'?", "id": "Attached (melampirkan)." },
  { "en": "Bentuk past participle dari 'attach'?", "id": "Attached (melampirkan)." },
  { "en": "Bentuk past tense dari 'attack'?", "id": "Attacked (menyerang)." },
  { "en": "Bentuk past participle dari 'attack'?", "id": "Attacked (menyerang)." },
  { "en": "Bentuk past tense dari 'attempt'?", "id": "Attempted (mencoba)." },
  { "en": "Bentuk past participle dari 'attempt'?", "id": "Attempted (mencoba)." },
  { "en": "Bentuk past tense dari 'attend'?", "id": "Attended (menghadiri)." },
  { "en": "Bentuk past participle dari 'attend'?", "id": "Attended (menghadiri)." },
  { "en": "Bentuk past tense dari 'attract'?", "id": "Attracted (menarik)." },
  { "en": "Bentuk past participle dari 'attract'?", "id": "Attracted (menarik)." },
  { "en": "Bentuk past tense dari 'audit'?", "id": "Audited (mengaudit)." },
  { "en": "Bentuk past participle dari 'audit'?", "id": "Audited (mengaudit)." },
  { "en": "Bentuk past tense dari 'avoid'?", "id": "Avoided (menghindari)." },
  { "en": "Bentuk past participle dari 'avoid'?", "id": "Avoided (menghindari)." },
  { "en": "Bentuk past tense dari 'awake'?", "id": "Awoke (terbangun)." },
  { "en": "Bentuk past participle dari 'awake'?", "id": "Awoken (terbangun)." },
  { "en": "Bentuk past tense dari 'bang'?", "id": "Banged (menggedor)." },
  { "en": "Bentuk past participle dari 'bang'?", "id": "Banged (menggedor)." },
  { "en": "Bentuk past tense dari 'banish'?", "id": "Banished (mengusir)." },
  { "en": "Bentuk past participle dari 'banish'?", "id": "Banished (mengusir)." },
  { "en": "Bentuk past tense dari 'bare'?", "id": "Bared (menelanjangi)." },
  { "en": "Bentuk past participle dari 'bare'?", "id": "Bared (menelanjangi)." },
  { "en": "Bentuk past tense dari 'bargain'?", "id": "Bargained (menawar)." },
  { "en": "Bentuk past participle dari 'bargain'?", "id": "Bargained (menawar)." },
  { "en": "Bentuk past tense dari 'bark'?", "id": "Barked (menggonggong)." },
  { "en": "Bentuk past participle dari 'bark'?", "id": "Barked (menggonggong)." },
  { "en": "Bentuk past tense dari 'barrage'?", "id": "Barraged (memberondong)." },
  { "en": "Bentuk past participle dari 'barrage'?", "id": "Barraged (memberondong)." },
  { "en": "Bentuk past tense dari 'barter'?", "id": "Bartered (bertukar barang)." },
  { "en": "Bentuk past participle dari 'barter'?", "id": "Bartered (bertukar barang)." },
  { "en": "Bentuk past tense dari 'baste'?", "id": "Basted (mengolesi)." },
  { "en": "Bentuk past participle dari 'baste'?", "id": "Basted (mengolesi)." },
  { "en": "Bentuk past tense dari 'bat'?", "id": "Batted (memukul bola)." },
  { "en": "Bentuk past participle dari 'bat'?", "id": "Batted (memukul bola)." },
  { "en": "Bentuk past tense dari 'battle'?", "id": "Battled (bertarung)." },
  { "en": "Bentuk past participle dari 'battle'?", "id": "Battled (bertarung)." },
  { "en": "Bentuk past tense dari 'beam'?", "id": "Beamed (berseri-seri)." },
  { "en": "Bentuk past participle dari 'beam'?", "id": "Beamed (berseri-seri)." },
  { "en": "Bentuk past tense dari 'bear'?", "id": "Bore (menanggung/melahirkan)." },
  { "en": "Bentuk past participle dari 'bear'?", "id": "Borne / Born (menanggung/melahirkan)." },
  { "en": "Bentuk past tense dari 'beat'?", "id": "Beat (mengalahkan)." },
  { "en": "Bentuk past participle dari 'beat'?", "id": "Beaten (mengalahkan)." },
  { "en": "Bentuk past tense dari 'beautify'?", "id": "Beautified (mempercantik)." },
  { "en": "Bentuk past participle dari 'beautify'?", "id": "Beautified (mempercantik)." },
  { "en": "Bentuk past tense dari 'beckon'?", "id": "Beckoned (mengisyaratkan)." },
  { "en": "Bentuk past participle dari 'beckon'?", "id": "Beckoned (mengisyaratkan)." },
  { "en": "Bentuk past tense dari 'beg'?", "id": "Begged (memohon)." },
  { "en": "Bentuk past participle dari 'beg'?", "id": "Begged (memohon)." },
  { "en": "Bentuk past tense dari 'belong'?", "id": "Belonged (milik)." },
  { "en": "Bentuk past participle dari 'belong'?", "id": "Belonged (milik)." },
  { "en": "Bentuk past tense dari 'bestow'?", "id": "Bestowed (menganugerahkan)." },
  { "en": "Bentuk past participle dari 'bestow'?", "id": "Bestowed (menganugerahkan)." },
  { "en": "Bentuk past tense dari 'betray'?", "id": "Betrayed (mengkhianati)." },
  { "en": "Bentuk past participle dari 'betray'?", "id": "Betrayed (mengkhianati)." },
  { "en": "Bentuk past tense dari 'beware'?", "id": "Bewared (berhati-hati)." },
  { "en": "Bentuk past participle dari 'beware'?", "id": "Bewared (berhati-hati)." },
  { "en": "Bentuk past tense dari 'bid'?", "id": "Bid (menawar)." },
  { "en": "Bentuk past participle dari 'bid'?", "id": "Bid (menawar)." },
  { "en": "Bentuk past tense dari 'bind'?", "id": "Bound (mengikat)." },
  { "en": "Bentuk past participle dari 'bind'?", "id": "Bound (mengikat)." },
  { "en": "Bentuk past tense dari 'bite'?", "id": "Bit (menggigit)." },
  { "en": "Bentuk past participle dari 'bite'?", "id": "Bitten (menggigit)." },
  { "en": "Bentuk past tense dari 'blame'?", "id": "Blamed (menyalahkan)." },
  { "en": "Bentuk past participle dari 'blame'?", "id": "Blamed (menyalahkan)." },
  { "en": "Bentuk past tense dari 'blast'?", "id": "Blasted (meledakkan)." },
  { "en": "Bentuk past participle dari 'blast'?", "id": "Blasted (meledakkan)." },
  { "en": "Bentuk past tense dari 'blaze'?", "id": "Blazed (berkobar)." },
  { "en": "Bentuk past participle dari 'blaze'?", "id": "Blazed (berkobar)." },
  { "en": "Bentuk past tense dari 'bleach'?", "id": "Bleached (memutihkan)." },
  { "en": "Bentuk past participle dari 'bleach'?", "id": "Bleached (memutihkan)." },
  { "en": "Bentuk past tense dari 'bless'?", "id": "Blessed (memberkati)." },
  { "en": "Bentuk past participle dari 'bless'?", "id": "Blessed (memberkati)." },
  { "en": "Bentuk past tense dari 'blind'?", "id": "Blinded (membutakan)." },
  { "en": "Bentuk past participle dari 'blind'?", "id": "Blinded (membutakan)." },
  { "en": "Bentuk past tense dari 'blink'?", "id": "Blinked (berkedip)." },
  { "en": "Bentuk past participle dari 'blink'?", "id": "Blinked (berkedip)." },
  { "en": "Bentuk past tense dari 'block'?", "id": "Blocked (memblokir)." },
  { "en": "Bentuk past participle dari 'block'?", "id": "Blocked (memblokir)." },
  { "en": "Bentuk past tense dari 'blot'?", "id": "Blotted (menodai)." },
  { "en": "Bentuk past participle dari 'blot'?", "id": "Blotted (menodai)." },
  { "en": "Bentuk past tense dari 'blush'?", "id": "Blushed (merona)." },
  { "en": "Bentuk past participle dari 'blush'?", "id": "Blushed (merona)." },
  { "en": "Bentuk past tense dari 'boast'?", "id": "Boasted (membual)." },
  { "en": "Bentuk past participle dari 'boast'?", "id": "Boasted (membual)." },
  { "en": "Bentuk past tense dari 'bomb'?", "id": "Bombed (mengebom)." },
  { "en": "Bentuk past participle dari 'bomb'?", "id": "Bombed (mengebom)." },
  { "en": "Bentuk past tense dari 'book'?", "id": "Booked (memesan)." },
  { "en": "Bentuk past participle dari 'book'?", "id": "Booked (memesan)." },
  { "en": "Bentuk past tense dari 'bore'?", "id": "Bored (membosankan)." },
  { "en": "Bentuk past participle dari 'bore'?", "id": "Bored (membosankan)." },
  { "en": "Bentuk past tense dari 'bounce'?", "id": "Bounced (memantul)." },
  { "en": "Bentuk past participle dari 'bounce'?", "id": "Bounced (memantul)." },
  { "en": "Bentuk past tense dari 'bow'?", "id": "Bowed (membungkuk)." },
  { "en": "Bentuk past participle dari 'bow'?", "id": "Bowed (membungkuk)." },
  { "en": "Bentuk past tense dari 'box'?", "id": "Boxed (bertinju)." },
  { "en": "Bentuk past participle dari 'box'?", "id": "Boxed (bertinju)." },
  { "en": "Bentuk past tense dari 'brace'?", "id": "Braced (menyangga/mempersiapkan)." },
  { "en": "Bentuk past participle dari 'brace'?", "id": "Braced (menyangga/mempersiapkan)." },
  { "en": "Bentuk past tense dari 'brag'?", "id": "Bragged (menyombongkan)." },
  { "en": "Bentuk past participle dari 'brag'?", "id": "Bragged (menyombongkan)." },
  { "en": "Bentuk past tense dari 'brake'?", "id": "Braked (mengerem)." },
  { "en": "Bentuk past participle dari 'brake'?", "id": "Braked (mengerem)." },
  { "en": "Bentuk past tense dari 'branch'?", "id": "Branched (bercabang)." },
  { "en": "Bentuk past participle dari 'branch'?", "id": "Branched (bercabang)." },
  { "en": "Bentuk past tense dari 'brand'?", "id": "Branded (memberi merek)." },
  { "en": "Bentuk past participle dari 'brand'?", "id": "Branded (memberi merek)." },
  { "en": "Bentuk past tense dari 'breed'?", "id": "Bred (berkembang biak)." },
  { "en": "Bentuk past participle dari 'breed'?", "id": "Bred (berkembang biak)." },
  { "en": "Bentuk past tense dari 'brief'?", "id": "Briefed (memberi pengarahan)." },
  { "en": "Bentuk past participle dari 'brief'?", "id": "Briefed (memberi pengarahan)." },
  { "en": "Bentuk past tense dari 'broadcast'?", "id": "Broadcast (menyiarkan)." },
  { "en": "Bentuk past participle dari 'broadcast'?", "id": "Broadcast (menyiarkan)." },
  { "en": "Bentuk past tense dari 'broaden'?", "id": "Broadened (memperluas)." },
  { "en": "Bentuk past participle dari 'broaden'?", "id": "Broadened (memperluas)." },
  { "en": "Bentuk past tense dari 'bruise'?", "id": "Bruised (memar)." },
  { "en": "Bentuk past participle dari 'bruise'?", "id": "Bruised (memar)." },
  { "en": "Bentuk past tense dari 'bubble'?", "id": "Bubbled (menggelembung)." },
  { "en": "Bentuk past participle dari 'bubble'?", "id": "Bubbled (menggelembung)." },
  { "en": "Bentuk past tense dari 'buckle'?", "id": "Buckled (mengencangkan)." },
  { "en": "Bentuk past participle dari 'buckle'?", "id": "Buckled (mengencangkan)." },
  { "en": "Bentuk past tense dari 'budge'?", "id": "Budged (beringsut)." },
  { "en": "Bentuk past participle dari 'budge'?", "id": "Budged (beringsut)." },
  { "en": "Bentuk past tense dari 'buffer'?", "id": "Buffered (menyangga)." },
  { "en": "Bentuk past participle dari 'buffer'?", "id": "Buffered (menyangga)." },
  { "en": "Bentuk past tense dari 'bump'?", "id": "Bumped (menabrak)." },
  { "en": "Bentuk past participle dari 'bump'?", "id": "Bumped (menabrak)." },
  { "en": "Bentuk past tense dari 'burst'?", "id": "Burst (meletus)." },
  { "en": "Bentuk past participle dari 'burst'?", "id": "Burst (meletus)." },
  { "en": "Bentuk past tense dari 'bust'?", "id": "Busted / Bust (merusak)." },
  { "en": "Bentuk past participle dari 'bust'?", "id": "Busted / Bust (merusak)." },
  { "en": "Bentuk past tense dari 'buzz'?", "id": "Buzzed (berdengung)." },
  { "en": "Bentuk past participle dari 'buzz'?", "id": "Buzzed (berdengung)." },
  { "en": "Bentuk past tense dari 'calculate'?", "id": "Calculated (menghitung)." },
  { "en": "Bentuk past participle dari 'calculate'?", "id": "Calculated (menghitung)." },
  { "en": "Bentuk past tense dari 'campaign'?", "id": "Campaigned (berkampanye)." },
  { "en": "Bentuk past participle dari 'campaign'?", "id": "Campaigned (berkampanye)." },
  { "en": "Bentuk past tense dari 'cancel'?", "id": "Canceled (membatalkan)." },
  { "en": "Bentuk past participle dari 'cancel'?", "id": "Canceled (membatalkan)." },
  { "en": "Bentuk past tense dari 'capture'?", "id": "Captured (menangkap)." },
  { "en": "Bentuk past participle dari 'capture'?", "id": "Captured (menangkap)." },
  { "en": "Bentuk past tense dari 'caress'?", "id": "Caressed (membelai)." },
  { "en": "Bentuk past participle dari 'caress'?", "id": "Caressed (membelai)." },
  { "en": "Bentuk past tense dari 'carry'?", "id": "Carried (membawa)." },
  { "en": "Bentuk past participle dari 'carry'?", "id": "Carried (membawa)." },
  { "en": "Bentuk past tense dari 'carve'?", "id": "Carved (mengukir)." },
  { "en": "Bentuk past participle dari 'carve'?", "id": "Carved (mengukir)." },
  { "en": "Bentuk past tense dari 'cast'?", "id": "Cast (melemparkan)." },
  { "en": "Bentuk past participle dari 'cast'?", "id": "Cast (melemparkan)." },
  { "en": "Bentuk past tense dari 'catalogue'?", "id": "Catalogued (mengkatalogkan)." },
  { "en": "Bentuk past participle dari 'catalogue'?", "id": "Catalogued (mengkatalogkan)." },
  { "en": "Bentuk past tense dari 'cease'?", "id": "Ceased (berhenti)." },
  { "en": "Bentuk past participle dari 'cease'?", "id": "Ceased (berhenti)." },
  { "en": "Bentuk past tense dari 'celebrate'?", "id": "Celebrated (merayakan)." },
  { "en": "Bentuk past participle dari 'celebrate'?", "id": "Celebrated (merayakan)." },
  { "en": "Bentuk past tense dari 'challenge'?", "id": "Challenged (menantang)." },
  { "en": "Bentuk past participle dari 'challenge'?", "id": "Challenged (menantang)." },
  { "en": "Bentuk past tense dari 'chant'?", "id": "Chanted (menyanyikan)." },
  { "en": "Bentuk past participle dari 'chant'?", "id": "Chanted (menyanyikan)." },
  { "en": "Bentuk past tense dari 'characterize'?", "id": "Characterized (menggambarkan)." },
  { "en": "Bentuk past participle dari 'characterize'?", "id": "Characterized (menggambarkan)." },
  { "en": "Bentuk past tense dari 'chart'?", "id": "Charted (memetakan)." },
  { "en": "Bentuk past participle dari 'chart'?", "id": "Charted (memetakan)." },
  { "en": "Bentuk past tense dari 'chase'?", "id": "Chased (mengejar)." },
  { "en": "Bentuk past participle dari 'chase'?", "id": "Chased (mengejar)." },
  { "en": "Bentuk past tense dari 'chat'?", "id": "Chatted (mengobrol)." },
  { "en": "Bentuk past participle dari 'chat'?", "id": "Chatted (mengobrol)." },
  { "en": "Bentuk past tense dari 'cheat'?", "id": "Cheated (menyontek)." },
  { "en": "Bentuk past participle dari 'cheat'?", "id": "Cheated (menyontek)." },
  { "en": "Bentuk past tense dari 'cherish'?", "id": "Cherished (menghargai)." },
  { "en": "Bentuk past participle dari 'cherish'?", "id": "Cherished (menghargai)." },
  { "en": "Bentuk past tense dari 'chew'?", "id": "Chewed (mengunyah)." },
  { "en": "Bentuk past participle dari 'chew'?", "id": "Chewed (mengunyah)." },
  { "en": "Bentuk past tense dari 'chill'?", "id": "Chilled (mendinginkan)." },
  { "en": "Bentuk past participle dari 'chill'?", "id": "Chilled (mendinginkan)." },
  { "en": "Bentuk past tense dari 'chime'?", "id": "Chimed (berdentang)." },
  { "en": "Bentuk past participle dari 'chime'?", "id": "Chimed (berdentang)." },
  { "en": "Bentuk past tense dari 'chip'?", "id": "Chipped (retak/terkelupas)." },
  { "en": "Bentuk past participle dari 'chip'?", "id": "Chipped (retak/terkelupas)." },
  { "en": "Bentuk past tense dari 'choke'?", "id": "Choked (tersedak)." },
  { "en": "Bentuk past participle dari 'choke'?", "id": "Choked (tersedak)." },
  { "en": "Bentuk past tense dari 'chop'?", "id": "Chopped (memotong)." },
  { "en": "Bentuk past participle dari 'chop'?", "id": "Chopped (memotong)." },
  { "en": "Bentuk past tense dari 'circle'?", "id": "Circled (mengelilingi)." },
  { "en": "Bentuk past participle dari 'circle'?", "id": "Circled (mengelilingi)." },
  { "en": "Bentuk past tense dari 'circulate'?", "id": "Circulated (beredar)." },
  { "en": "Bentuk past participle dari 'circulate'?", "id": "Circulated (beredar)." },
  { "en": "Bentuk past tense dari 'cite'?", "id": "Cited (mengutip)." },
  { "en": "Bentuk past participle dari 'cite'?", "id": "Cited (mengutip)." },
  { "en": "Bentuk past tense dari 'claim'?", "id": "Claimed (mengklaim)." },
  { "en": "Bentuk past participle dari 'claim'?", "id": "Claimed (mengklaim)." },
  { "en": "Bentuk past tense dari 'clarify'?", "id": "Clarified (memperjelas)." },
  { "en": "Bentuk past participle dari 'clarify'?", "id": "Clarified (memperjelas)." },
  { "en": "Bentuk past tense dari 'classify'?", "id": "Classified (mengklasifikasikan)." },
  { "en": "Bentuk past participle dari 'classify'?", "id": "Classified (mengklasifikasikan)." },
  { "en": "Bentuk past tense dari 'cling'?", "id": "Clung (berpegang erat)." },
  { "en": "Bentuk past participle dari 'cling'?", "id": "Clung (berpegang erat)." },
  { "en": "Bentuk past tense dari 'clip'?", "id": "Clipped (memotong/menjepit)." },
  { "en": "Bentuk past participle dari 'clip'?", "id": "Clipped (memotong/menjepit)." },
  { "en": "Bentuk past tense dari 'clothe'?", "id": "Clad / Clothed (mengenakan pakaian)." },
  { "en": "Bentuk past participle dari 'clothe'?", "id": "Clad / Clothed (mengenakan pakaian)." },
  { "en": "Bentuk past tense dari 'coach'?", "id": "Coached (melatih)." },
  { "en": "Bentuk past participle dari 'coach'?", "id": "Coached (melatih)." },
  { "en": "Bentuk past tense dari 'coil'?", "id": "Coiled (menggulung)." },
  { "en": "Bentuk past participle dari 'coil'?", "id": "Coiled (menggulung)." },
  { "en": "Bentuk past tense dari 'collapse'?", "id": "Collapsed (runtuh)." },
  { "en": "Bentuk past participle dari 'collapse'?", "id": "Collapsed (runtuh)." },
  { "en": "Bentuk past tense dari 'colonize'?", "id": "Colonized (menjajah)." },
  { "en": "Bentuk past participle dari 'colonize'?", "id": "Colonized (menjajah)." },
  { "en": "Bentuk past tense dari 'colour'?", "id": "Coloured (mewarnai)." },
  { "en": "Bentuk past participle dari 'colour'?", "id": "Coloured (mewarnai)." },
  { "en": "Bentuk past tense dari 'comb'?", "id": "Combed (menyisir)." },
  { "en": "Bentuk past participle dari 'comb'?", "id": "Combed (menyisir)." },
  { "en": "Bentuk past tense dari 'combat'?", "id": "Combated (memerangi)." },
  { "en": "Bentuk past participle dari 'combat'?", "id": "Combated (memerangi)." },
  { "en": "Bentuk past tense dari 'command'?", "id": "Commanded (memerintah)." },
  { "en": "Bentuk past participle dari 'command'?", "id": "Commanded (memerintah)." },
  { "en": "Bentuk past tense dari 'commence'?", "id": "Commenced (memulai)." },
  { "en": "Bentuk past participle dari 'commence'?", "id": "Commenced (memulai)." },
  { "en": "Bentuk past tense dari 'comment'?", "id": "Commented (berkomentar)." },
  { "en": "Bentuk past participle dari 'comment'?", "id": "Commented (berkomentar)." },
  { "en": "Bentuk past tense dari 'commission'?", "id": "Commissioned (menugaskan)." },
  { "en": "Bentuk past participle dari 'commission'?", "id": "Commissioned (menugaskan)." },
  { "en": "Bentuk past tense dari 'commit'?", "id": "Committed (berkomitmen/melakukan)." },
  { "en": "Bentuk past participle dari 'commit'?", "id": "Committed (berkomitmen/melakukan)." },
  { "en": "Bentuk past tense dari 'communicate'?", "id": "Communicated (berkomunikasi)." },
  { "en": "Bentuk past participle dari 'communicate'?", "id": "Communicated (berkomunikasi)." },
  { "en": "Bentuk past tense dari 'compete'?", "id": "Competed (bersaing)." },
  { "en": "Bentuk past participle dari 'compete'?", "id": "Competed (bersaing)." },
  { "en": "Bentuk past tense dari 'compile'?", "id": "Compiled (menyusun)." },
  { "en": "Bentuk past participle dari 'compile'?", "id": "Compiled (menyusun)." },
  { "en": "Bentuk past tense dari 'compose'?", "id": "Composed (menggubah/menyusun)." },
  { "en": "Bentuk past participle dari 'compose'?", "id": "Composed (menggubah/menyusun)." },
  { "en": "Bentuk past tense dari 'compress'?", "id": "Compressed (memadatkan)." },
  { "en": "Bentuk past participle dari 'compress'?", "id": "Compressed (memadatkan)." },
  { "en": "Bentuk past tense dari 'compromise'?", "id": "Compromised (berkompromi)." },
  { "en": "Bentuk past participle dari 'compromise'?", "id": "Compromised (berkompromi)." },
  { "en": "Bentuk past tense dari 'compute'?", "id": "Computed (menghitung)." },
  { "en": "Bentuk past participle dari 'compute'?", "id": "Computed (menghitung)." },
  { "en": "Bentuk past tense dari 'conceal'?", "id": "Concealed (menyembunyikan)." },
  { "en": "Bentuk past participle dari 'conceal'?", "id": "Concealed (menyembunyikan)." },
  { "en": "Bentuk past tense dari 'concede'?", "id": "Conceded (mengakui/kebobolan)." },
  { "en": "Bentuk past participle dari 'concede'?", "id": "Conceded (mengakui/kebobolan)." },
  { "en": "Bentuk past tense dari 'concentrate'?", "id": "Concentrated (berkonsentrasi)." },
  { "en": "Bentuk past participle dari 'concentrate'?", "id": "Concentrated (berkonsentrasi)." },
  { "en": "Bentuk past tense dari 'concern'?", "id": "Concerned (menyangkut/mengkhawatirkan)." },
  { "en": "Bentuk past participle dari 'concern'?", "id": "Concerned (menyangkut/mengkhawatirkan)." },
  { "en": "Bentuk past tense dari 'conclude'?", "id": "Concluded (menyimpulkan)." },
  { "en": "Bentuk past participle dari 'conclude'?", "id": "Concluded (menyimpulkan)." },
  { "en": "Bentuk past tense dari 'condemn'?", "id": "Condemned (mengutuk)." },
  { "en": "Bentuk past participle dari 'condemn'?", "id": "Condemned (mengutuk)." },
  { "en": "Bentuk past tense dari 'condense'?", "id": "Condensed (memadatkan/mengembun)." },
  { "en": "Bentuk past participle dari 'condense'?", "id": "Condensed (memadatkan/mengembun)." },
  { "en": "Bentuk past tense dari 'confess'?", "id": "Confessed (mengaku)." },
  { "en": "Bentuk past participle dari 'confess'?", "id": "Confessed (mengaku)." },
  { "en": "Bentuk past tense dari 'confide'?", "id": "Confided (mempercayakan)." },
  { "en": "Bentuk past participle dari 'confide'?", "id": "Confided (mempercayakan)." },
  { "en": "Bentuk past tense dari 'confine'?", "id": "Confined (membatasi)." },
  { "en": "Bentuk past participle dari 'confine'?", "id": "Confined (membatasi)." },
  { "en": "Bentuk past tense dari 'confirm'?", "id": "Confirmed (memastikan)." },
  { "en": "Bentuk past participle dari 'confirm'?", "id": "Confirmed (memastikan)." },
  { "en": "Bentuk past tense dari 'confiscate'?", "id": "Confiscated (menyita)." },
  { "en": "Bentuk past participle dari 'confiscate'?", "id": "Confiscated (menyita)." },
  { "en": "Bentuk past tense dari 'confront'?", "id": "Confronted (menghadapi)." },
  { "en": "Bentuk past participle dari 'confront'?", "id": "Confronted (menghadapi)." },
  { "en": "Bentuk past tense dari 'congratulate'?", "id": "Congratulated (memberi selamat)." },
  { "en": "Bentuk past participle dari 'congratulate'?", "id": "Congratulated (memberi selamat)." },
  { "en": "Bentuk past tense dari 'consent'?", "id": "Consented (menyetujui)." },
  { "en": "Bentuk past participle dari 'consent'?", "id": "Consented (menyetujui)." },
  { "en": "Bentuk past tense dari 'conserve'?", "id": "Conserved (melestarikan)." },
  { "en": "Bentuk past participle dari 'conserve'?", "id": "Conserved (melestarikan)." },
  { "en": "Bentuk past tense dari 'consist'?", "id": "Consisted (terdiri dari)." },
  { "en": "Bentuk past participle dari 'consist'?", "id": "Consisted (terdiri dari)." },
  { "en": "Bentuk past tense dari 'console'?", "id": "Consoled (menghibur)." },
  { "en": "Bentuk past participle dari 'console'?", "id": "Consoled (menghibur)." },
  { "en": "Bentuk past tense dari 'conspire'?", "id": "Conspired (bersekongkol)." },
  { "en": "Bentuk past participle dari 'conspire'?", "id": "Conspired (bersekongkol)." },
  { "en": "Bentuk past tense dari 'constitute'?", "id": "Constituted (merupakan)." },
  { "en": "Bentuk past participle dari 'constitute'?", "id": "Constituted (merupakan)." },
  { "en": "Bentuk past tense dari 'constrain'?", "id": "Constrained (memaksa)." },
  { "en": "Bentuk past participle dari 'constrain'?", "id": "Constrained (memaksa)." },
  { "en": "Bentuk past tense dari 'construct'?", "id": "Constructed (membangun)." },
  { "en": "Bentuk past participle dari 'construct'?", "id": "Constructed (membangun)." },
  { "en": "Bentuk past tense dari 'consult'?", "id": "Consulted (berkonsultasi)." },
  { "en": "Bentuk past participle dari 'consult'?", "id": "Consulted (berkonsultasi)." },
  { "en": "Bentuk past tense dari 'consume'?", "id": "Consumed (mengkonsumsi)." },
  { "en": "Bentuk past participle dari 'consume'?", "id": "Consumed (mengkonsumsi)." },
  { "en": "Bentuk past tense dari 'contact'?", "id": "Contacted (menghubungi)." },
  { "en": "Bentuk past participle dari 'contact'?", "id": "Contacted (menghubungi)." },
  { "en": "Bentuk past tense dari 'contain'?", "id": "Contained (berisi)." },
  { "en": "Bentuk past participle dari 'contain'?", "id": "Contained (berisi)." },
  { "en": "Bentuk past tense dari 'contaminate'?", "id": "Contaminated (mengkontaminasi)." },
  { "en": "Bentuk past participle dari 'contaminate'?", "id": "Contaminated (mengkontaminasi)." },
  { "en": "Bentuk past tense dari 'contemplate'?", "id": "Contemplated (merenungkan)." },
  { "en": "Bentuk past participle dari 'contemplate'?", "id": "Contemplated (merenungkan)." },
  { "en": "Bentuk past tense dari 'contend'?", "id": "Contended (bersaing)." },
  { "en": "Bentuk past participle dari 'contend'?", "id": "Contended (bersaing)." },
  { "en": "Bentuk past tense dari 'contest'?", "id": "Contested (membantah/memperebutkan)." },
  { "en": "Bentuk past participle dari 'contest'?", "id": "Contested (membantah/memperebutkan)." },
  { "en": "Bentuk past tense dari 'contract'?", "id": "Contracted (mengontrak/mengerut)." },
  { "en": "Bentuk past participle dari 'contract'?", "id": "Contracted (mengontrak/mengerut)." },
  { "en": "Bentuk past tense dari 'contradict'?", "id": "Contradicted (bertentangan)." },
  { "en": "Bentuk past participle dari 'contradict'?", "id": "Contradicted (bertentangan)." },
  { "en": "Bentuk past tense dari 'contrast'?", "id": "Contrasted (membedakan)." },
  { "en": "Bentuk past participle dari 'contrast'?", "id": "Contrasted (membedakan)." },
  { "en": "Bentuk past tense dari 'contribute'?", "id": "Contributed (berkontribusi)." },
  { "en": "Bentuk past participle dari 'contribute'?", "id": "Contributed (berkontribusi)." },
  { "en": "Bentuk past tense dari 'control'?", "id": "Controlled (mengendalikan)." },
  { "en": "Bentuk past participle dari 'control'?", "id": "Controlled (mengendalikan)." },
  { "en": "Bentuk past tense dari 'convene'?", "id": "Convened (mengadakan rapat)." },
  { "en": "Bentuk past participle dari 'convene'?", "id": "Convened (mengadakan rapat)." },
  { "en": "Bentuk past tense dari 'converge'?", "id": "Converged (berkumpul)." },
  { "en": "Bentuk past participle dari 'converge'?", "id": "Converged (berkumpul)." },
  { "en": "Bentuk past tense dari 'converse'?", "id": "Conversed (bercakap-cakap)." },
  { "en": "Bentuk past participle dari 'converse'?", "id": "Conversed (bercakap-cakap)." },
  { "en": "Bentuk past tense dari 'convert'?", "id": "Converted (mengubah)." },
  { "en": "Bentuk past participle dari 'convert'?", "id": "Converted (mengubah)." },
  { "en": "Bentuk past tense dari 'convey'?", "id": "Conveyed (menyampaikan)." },
  { "en": "Bentuk past participle dari 'convey'?", "id": "Conveyed (menyampaikan)." },
  { "en": "Bentuk past tense dari 'convict'?", "id": "Convicted (menghukum)." },
  { "en": "Bentuk past participle dari 'convict'?", "id": "Convicted (menghukum)." },
  { "en": "Bentuk past tense dari 'convince'?", "id": "Convinced (meyakinkan)." },
  { "en": "Bentuk past participle dari 'convince'?", "id": "Convinced (meyakinkan)." },
  { "en": "Bentuk past tense dari 'cooperate'?", "id": "Cooperated (bekerja sama)." },
  { "en": "Bentuk past participle dari 'cooperate'?", "id": "Cooperated (bekerja sama)." },
  { "en": "Bentuk past tense dari 'coordinate'?", "id": "Coordinated (mengoordinasikan)." },
  { "en": "Bentuk past participle dari 'coordinate'?", "id": "Coordinated (mengoordinasikan)." },
  { "en": "Bentuk past tense dari 'cope'?", "id": "Coped (mengatasi)." },
  { "en": "Bentuk past participle dari 'cope'?", "id": "Coped (mengatasi)." },
  { "en": "Bentuk past tense dari 'correspond'?", "id": "Corresponded (bersurat/sesuai)." },
  { "en": "Bentuk past participle dari 'correspond'?", "id": "Corresponded (bersurat/sesuai)." },
  { "en": "Bentuk past tense dari 'corrode'?", "id": "Corroded (berkarat)." },
  { "en": "Bentuk past participle dari 'corrode'?", "id": "Corroded (berkarat)." },
  { "en": "Bentuk past tense dari 'corrupt'?", "id": "Corrupted (merusak/korupsi)." },
  { "en": "Bentuk past participle dari 'corrupt'?", "id": "Corrupted (merusak/korupsi)." },
  { "en": "Bentuk past tense dari 'cough'?", "id": "Coughed (batuk)." },
  { "en": "Bentuk past participle dari 'cough'?", "id": "Coughed (batuk)." },
  { "en": "Bentuk past tense dari 'counsel'?", "id": "Counseled (menasihati)." },
  { "en": "Bentuk past participle dari 'counsel'?", "id": "Counseled (menasihati)." },
  { "en": "Bentuk past tense dari 'cram'?", "id": "Crammed (menjejali)." },
  { "en": "Bentuk past participle dari 'cram'?", "id": "Crammed (menjejali)." },
  { "en": "Bentuk past tense dari 'crawl'?", "id": "Crawled (merangkak)." },
  { "en": "Bentuk past participle dari 'crawl'?", "id": "Crawled (merangkak)." },
  { "en": "Bentuk past tense dari 'creep'?", "id": "Crept (merayap)." },
  { "en": "Bentuk past participle dari 'creep'?", "id": "Crept (merayap)." },
  { "en": "Bentuk past tense dari 'criticize'?", "id": "Criticized (mengkritik)." },
  { "en": "Bentuk past participle dari 'criticize'?", "id": "Criticized (mengkritik)." },
  { "en": "Bentuk past tense dari 'cross'?", "id": "Crossed (menyeberang)." },
  { "en": "Bentuk past participle dari 'cross'?", "id": "Crossed (menyeberang)." },
  { "en": "Bentuk past tense dari 'crouch'?", "id": "Crouched (jongkok)." },
  { "en": "Bentuk past participle dari 'crouch'?", "id": "Crouched (jongkok)." },
  { "en": "Bentuk past tense dari 'crowd'?", "id": "Crowded (memadati)." },
  { "en": "Bentuk past participle dari 'crowd'?", "id": "Crowded (memadati)." },
  { "en": "Bentuk past tense dari 'crown'?", "id": "Crowned (memahkotai)." },
  { "en": "Bentuk past participle dari 'crown'?", "id": "Crowned (memahkotai)." },
  { "en": "Bentuk past tense dari 'cruise'?", "id": "Cruised (berpesiar)." },
  { "en": "Bentuk past participle dari 'cruise'?", "id": "Cruised (berpesiar)." },
  { "en": "Bentuk past tense dari 'crumble'?", "id": "Crumbled (hancur)." },
  { "en": "Bentuk past participle dari 'crumble'?", "id": "Crumbled (hancur)." },
  { "en": "Bentuk past tense dari 'crush'?", "id": "Crushed (menghancurkan)." },
  { "en": "Bentuk past participle dari 'crush'?", "id": "Crushed (menghancurkan)." },
  { "en": "Bentuk past tense dari 'cultivate'?", "id": "Cultivated (mengolah/membudidayakan)." },
  { "en": "Bentuk past participle dari 'cultivate'?", "id": "Cultivated (mengolah/membudidayakan)." },
  { "en": "Bentuk past tense dari 'curb'?", "id": "Curbed (mengekang)." },
  { "en": "Bentuk past participle dari 'curb'?", "id": "Curbed (mengekang)." },
  { "en": "Bentuk past tense dari 'cure'?", "id": "Cured (menyembuhkan)." },
  { "en": "Bentuk past participle dari 'cure'?", "id": "Cured (menyembuhkan)." },
  { "en": "Bentuk past tense dari 'curl'?", "id": "Curled (mengeriting)." },
  { "en": "Bentuk past participle dari 'curl'?", "id": "Curled (mengeriting)." },
  { "en": "Bentuk past tense dari 'curve'?", "id": "Curved (melengkung)." },
  { "en": "Bentuk past participle dari 'curve'?", "id": "Curved (melengkung)." },
  { "en": "Bentuk past tense dari 'cycle'?", "id": "Cycled (bersepeda)." },
  { "en": "Bentuk past participle dari 'cycle'?", "id": "Cycled (bersepeda)." },
  { "en": "Bentuk past tense dari 'dab'?", "id": "Dabbed (menepuk-nepuk)." },
  { "en": "Bentuk past participle dari 'dab'?", "id": "Dabbed (menepuk-nepuk)." },
  { "en": "Bentuk past tense dari 'dampen'?", "id": "Dampened (melembabkan)." },
  { "en": "Bentuk past participle dari 'dampen'?", "id": "Dampened (melembabkan)." },
  { "en": "Bentuk past tense dari 'dare'?", "id": "Dared (berani)." },
  { "en": "Bentuk past participle dari 'dare'?", "id": "Dared (berani)." },
  { "en": "Bentuk past tense dari 'darken'?", "id": "Darkened (menggelapkan)." },
  { "en": "Bentuk past participle dari 'darken'?", "id": "Darkened (menggelapkan)." },
  { "en": "Bentuk past tense dari 'dash'?", "id": "Dashed (melaju cepat)." },
  { "en": "Bentuk past participle dari 'dash'?", "id": "Dashed (melaju cepat)." },
  { "en": "Bentuk past tense dari 'dazzle'?", "id": "Dazzled (menyilaukan)." },
  { "en": "Bentuk past participle dari 'dazzle'?", "id": "Dazzled (menyilaukan)." },
  { "en": "Bentuk past tense dari 'deceive'?", "id": "Deceived (menipu)." },
  { "en": "Bentuk past participle dari 'deceive'?", "id": "Deceived (menipu)." },
  { "en": "Bentuk past tense dari 'declare'?", "id": "Declared (menyatakan)." },
  { "en": "Bentuk past participle dari 'declare'?", "id": "Declared (menyatakan)." },
  { "en": "Bentuk past tense dari 'decline'?", "id": "Declined (menurun/menolak)." },
  { "en": "Bentuk past participle dari 'decline'?", "id": "Declined (menurun/menolak)." },
  { "en": "Bentuk past tense dari 'decorate'?", "id": "Decorated (menghias)." },
  { "en": "Bentuk past participle dari 'decorate'?", "id": "Decorated (menghias)." },
  { "en": "Bentuk past tense dari 'decrease'?", "id": "Decreased (berkurang)." },
  { "en": "Bentuk past participle dari 'decrease'?", "id": "Decreased (berkurang)." },
  { "en": "Bentuk past tense dari 'dedicate'?", "id": "Dedicated (mendedikasikan)." },
  { "en": "Bentuk past participle dari 'dedicate'?", "id": "Dedicated (mendedikasikan)." },
  { "en": "Bentuk past tense dari 'defeat'?", "id": "Defeated (mengalahkan)." },
  { "en": "Bentuk past participle dari 'defeat'?", "id": "Defeated (mengalahkan)." },
  { "en": "Bentuk past tense dari 'defend'?", "id": "Defended (mempertahankan)." },
  { "en": "Bentuk past participle dari 'defend'?", "id": "Defended (mempertahankan)." },
  { "en": "Bentuk past tense dari 'define'?", "id": "Defined (mendefinisikan)." },
  { "en": "Bentuk past participle dari 'define'?", "id": "Defined (mendefinisikan)." },
  { "en": "Bentuk past tense dari 'defy'?", "id": "Defied (menentang)." },
  { "en": "Bentuk past participle dari 'defy'?", "id": "Defied (menentang)." },
  { "en": "Bentuk past tense dari 'delegate'?", "id": "Delegated (mendelegasikan)." },
  { "en": "Bentuk past participle dari 'delegate'?", "id": "Delegated (mendelegasikan)." },
  { "en": "Bentuk past tense dari 'delete'?", "id": "Deleted (menghapus)." },
  { "en": "Bentuk past participle dari 'delete'?", "id": "Deleted (menghapus)." },
  { "en": "Bentuk past tense dari 'delight'?", "id": "Delighted (menyenangkan)." },
  { "en": "Bentuk past participle dari 'delight'?", "id": "Delighted (menyenangkan)." },
  { "en": "Bentuk past tense dari 'deliver'?", "id": "Delivered (mengirim)." },
  { "en": "Bentuk past participle dari 'deliver'?", "id": "Delivered (mengirim)." },
  { "en": "Bentuk past tense dari 'demonstrate'?", "id": "Demonstrated (mendemonstrasikan)." },
  { "en": "Bentuk past participle dari 'demonstrate'?", "id": "Demonstrated (mendemonstrasikan)." },
  { "en": "Bentuk past tense dari 'depart'?", "id": "Departed (berangkat)." },
  { "en": "Bentuk past participle dari 'depart'?", "id": "Departed (berangkat)." },
  { "en": "Bentuk past tense dari 'deposit'?", "id": "Deposited (menyetor)." },
  { "en": "Bentuk past participle dari 'deposit'?", "id": "Deposited (menyetor)." },
  { "en": "Bentuk past tense dari 'depress'?", "id": "Depressed (menekan/membuat depresi)." },
  { "en": "Bentuk past participle dari 'depress'?", "id": "Depressed (menekan/membuat depresi)." },
  { "en": "Bentuk past tense dari 'deprive'?", "id": "Deprived (mencabut/merampas)." },
  { "en": "Bentuk past participle dari 'deprive'?", "id": "Deprived (mencabut/merampas)." },
  { "en": "Bentuk past tense dari 'derive'?", "id": "Derived (memperoleh)." },
  { "en": "Bentuk past participle dari 'derive'?", "id": "Derived (memperoleh)." },
  { "en": "Bentuk past tense dari 'desert'?", "id": "Deserted (meninggalkan)." },
  { "en": "Bentuk past participle dari 'desert'?", "id": "Deserted (meninggalkan)." },
  { "en": "Bentuk past tense dari 'deserve'?", "id": "Deserved (pantas/layak)." },
  { "en": "Bentuk past participle dari 'deserve'?", "id": "Deserved (pantas/layak)." },
  { "en": "Bentuk past tense dari 'desire'?", "id": "Desired (menginginkan)." },
  { "en": "Bentuk past participle dari 'desire'?", "id": "Desired (menginginkan)." },
  { "en": "Bentuk past tense dari 'despise'?", "id": "Despised (membenci)." },
  { "en": "Bentuk past participle dari 'despise'?", "id": "Despised (membenci)." },
  { "en": "Bentuk past tense dari 'destine'?", "id": "Destined (ditakdirkan)." },
  { "en": "Bentuk past participle dari 'destine'?", "id": "Destined (ditakdirkan)." },
  { "en": "Bentuk past tense dari 'detect'?", "id": "Detected (mendeteksi)." },
  { "en": "Bentuk past participle dari 'detect'?", "id": "Detected (mendeteksi)." },
  { "en": "Bentuk past tense dari 'deter'?", "id": "Deterred (mencegah)." },
  { "en": "Bentuk past participle dari 'deter'?", "id": "Deterred (mencegah)." },
  { "en": "Bentuk past tense dari 'deteriorate'?", "id": "Deteriorated (memburuk)." },
  { "en": "Bentuk past participle dari 'deteriorate'?", "id": "Deteriorated (memburuk)." },
  { "en": "Bentuk past tense dari 'determine'?", "id": "Determined (menentukan)." },
  { "en": "Bentuk past participle dari 'determine'?", "id": "Determined (menentukan)." },
  { "en": "Bentuk past tense dari 'detest'?", "id": "Detested (sangat membenci)." },
  { "en": "Bentuk past participle dari 'detest'?", "id": "Detested (sangat membenci)." },
  { "en": "Bentuk past tense dari 'devastate'?", "id": "Devastated (menghancurkan)." },
  { "en": "Bentuk past participle dari 'devastate'?", "id": "Devastated (menghancurkan)." },
  { "en": "Bentuk past tense dari 'deviate'?", "id": "Deviated (menyimpang)." },
  { "en": "Bentuk past participle dari 'deviate'?", "id": "Deviated (menyimpang)." },
  { "en": "Bentuk past tense dari 'devise'?", "id": "Devised (merancang)." },
  { "en": "Bentuk past participle dari 'devise'?", "id": "Devised (merancang)." },
  { "en": "Bentuk past tense dari 'devote'?", "id": "Devoted (mengabdikan)." },
  { "en": "Bentuk past participle dari 'devote'?", "id": "Devoted (mengabdikan)." },
  { "en": "Bentuk past tense dari 'devour'?", "id": "Devoured (melahap)." },
  { "en": "Bentuk past participle dari 'devour'?", "id": "Devoured (melahap)." },
  { "en": "Bentuk past tense dari 'diagnose'?", "id": "Diagnosed (mendiagnosis)." },
  { "en": "Bentuk past participle dari 'diagnose'?", "id": "Diagnosed (mendiagnosis)." },
  { "en": "Bentuk past tense dari 'dictate'?", "id": "Dictated (mendikte)." },
  { "en": "Bentuk past participle dari 'dictate'?", "id": "Dictated (mendikte)." },
  { "en": "Bentuk past tense dari 'differ'?", "id": "Differed (berbeda)." },
  { "en": "Bentuk past participle dari 'differ'?", "id": "Differed (berbeda)." },
  { "en": "Bentuk past tense dari 'differentiate'?", "id": "Differentiated (membedakan)." },
  { "en": "Bentuk past participle dari 'differentiate'?", "id": "Differentiated (membedakan)." },
  { "en": "Bentuk past tense dari 'digest'?", "id": "Digested (mencerna)." },
  { "en": "Bentuk past participle dari 'digest'?", "id": "Digested (mencerna)." },
  { "en": "Bentuk past tense dari 'dim'?", "id": "Dimmed (meredup)." },
  { "en": "Bentuk past participle dari 'dim'?", "id": "Dimmed (meredup)." },
  { "en": "Bentuk past tense dari 'diminish'?", "id": "Diminished (berkurang)." },
  { "en": "Bentuk past participle dari 'diminish'?", "id": "Diminished (berkurang)." },
  { "en": "Bentuk past tense dari 'dine'?", "id": "Dined (makan malam)." },
  { "en": "Bentuk past participle dari 'dine'?", "id": "Dined (makan malam)." },
  { "en": "Bentuk past tense dari 'dip'?", "id": "Dipped (mencelupkan)." },
  { "en": "Bentuk past participle dari 'dip'?", "id": "Dipped (mencelupkan)." },
  { "en": "Bentuk past tense dari 'direct'?", "id": "Directed (mengarahkan)." },
  { "en": "Bentuk past participle dari 'direct'?", "id": "Directed (mengarahkan)." },
  { "en": "Bentuk past tense dari 'disable'?", "id": "Disabled (menonaktifkan)." },
  { "en": "Bentuk past participle dari 'disable'?", "id": "Disabled (menonaktifkan)." },
  { "en": "Bentuk past tense dari 'disapprove'?", "id": "Disapproved (tidak menyetujui)." },
  { "en": "Bentuk past participle dari 'disapprove'?", "id": "Disapproved (tidak menyetujui)." },
  { "en": "Bentuk past tense dari 'disarm'?", "id": "Disarmed (melucuti senjata)." },
  { "en": "Bentuk past participle dari 'disarm'?", "id": "Disarmed (melucuti senjata)." },
  { "en": "Bentuk past tense dari 'discard'?", "id": "Discarded (membuang)." },
  { "en": "Bentuk past participle dari 'discard'?", "id": "Discarded (membuang)." },
  { "en": "Bentuk past tense dari 'discern'?", "id": "Discerned (membedakan)." },
  { "en": "Bentuk past participle dari 'discern'?", "id": "Discerned (membedakan)." },
  { "en": "Bentuk past tense dari 'discharge'?", "id": "Discharged (melepaskan/mengeluarkan)." },
  { "en": "Bentuk past participle dari 'discharge'?", "id": "Discharged (melepaskan/mengeluarkan)." },
  { "en": "Bentuk past tense dari 'discipline'?", "id": "Disciplined (mendisiplinkan)." },
  { "en": "Bentuk past participle dari 'discipline'?", "id": "Disciplined (mendisiplinkan)." },
  { "en": "Bentuk past tense dari 'disclose'?", "id": "Disclosed (mengungkapkan)." },
  { "en": "Bentuk past participle dari 'disclose'?", "id": "Disclosed (mengungkapkan)." },
  { "en": "Bentuk past tense dari 'disconnect'?", "id": "Disconnected (memutuskan)." },
  { "en": "Bentuk past participle dari 'disconnect'?", "id": "Disconnected (memutuskan)." },
  { "en": "Bentuk past tense dari 'discourage'?", "id": "Discouraged (mengecilkan hati)." },
  { "en": "Bentuk past participle dari 'discourage'?", "id": "Discouraged (mengecilkan hati)." },
  { "en": "Bentuk past tense dari 'disguise'?", "id": "Disguised (menyamar)." },
  { "en": "Bentuk past participle dari 'disguise'?", "id": "Disguised (menyamar)." },
  { "en": "Bentuk past tense dari 'disgust'?", "id": "Disgusted (menjijikkan)." },
  { "en": "Bentuk past participle dari 'disgust'?", "id": "Disgusted (menjijikkan)." },
  { "en": "Bentuk past tense dari 'dislike'?", "id": "Disliked (tidak menyukai)." },
  { "en": "Bentuk past participle dari 'dislike'?", "id": "Disliked (tidak menyukai)." },
  { "en": "Bentuk past tense dari 'dismantle'?", "id": "Dismantled (membongkar)." },
  { "en": "Bentuk past participle dari 'dismantle'?", "id": "Dismantled (membongkar)." },
  { "en": "Bentuk past tense dari 'dismay'?", "id": "Dismayed (mengecewakan)." },
  { "en": "Bentuk past participle dari 'dismay'?", "id": "Dismayed (mengecewakan)." },
  { "en": "Bentuk past tense dari 'dismiss'?", "id": "Dismissed (memberhentikan)." },
  { "en": "Bentuk past participle dari 'dismiss'?", "id": "Dismissed (memberhentikan)." },
  { "en": "Bentuk past tense dari 'disobey'?", "id": "Disobeyed (tidak mematuhi)." },
  { "en": "Bentuk past participle dari 'disobey'?", "id": "Disobeyed (tidak mematuhi)." },
  { "en": "Bentuk past tense dari 'dispatch'?", "id": "Dispatched (mengirim)." },
  { "en": "Bentuk past participle dari 'dispatch'?", "id": "Dispatched (mengirim)." },
  { "en": "Bentuk past tense dari 'dispel'?", "id": "Dispelled (menghilangkan)." },
  { "en": "Bentuk past participle dari 'dispel'?", "id": "Dispelled (menghilangkan)." },
  { "en": "Bentuk past tense dari 'dispense'?", "id": "Dispensed (membagikan)." },
  { "en": "Bentuk past participle dari 'dispense'?", "id": "Dispensed (membagikan)." },
  { "en": "Bentuk past tense dari 'disperse'?", "id": "Dispersed (membubarkan)." },
  { "en": "Bentuk past participle dari 'disperse'?", "id": "Dispersed (membubarkan)." },
  { "en": "Bentuk past tense dari 'displace'?", "id": "Displaced (menggantikan)." },
  { "en": "Bentuk past participle dari 'displace'?", "id": "Displaced (menggantikan)." },
  { "en": "Bentuk past tense dari 'display'?", "id": "Displayed (menampilkan)." },
  { "en": "Bentuk past participle dari 'display'?", "id": "Displayed (menampilkan)." },
  { "en": "Bentuk past tense dari 'dispose'?", "id": "Disposed (membuang)." },
  { "en": "Bentuk past participle dari 'dispose'?", "id": "Disposed (membuang)." },
  { "en": "Bentuk past tense dari 'disprove'?", "id": "Disproved (menyangkal)." },
  { "en": "Bentuk past participle dari 'disprove'?", "id": "Disproved (menyangkal)." },
  { "en": "Bentuk past tense dari 'disrupt'?", "id": "Disrupted (mengganggu)." },
  { "en": "Bentuk past participle dari 'disrupt'?", "id": "Disrupted (mengganggu)." },
  { "en": "Bentuk past tense dari 'dissolve'?", "id": "Dissolved (larut)." },
  { "en": "Bentuk past participle dari 'dissolve'?", "id": "Dissolved (larut)." },
  { "en": "Bentuk past tense dari 'distill'?", "id": "Distilled (menyuling)." },
  { "en": "Bentuk past participle dari 'distill'?", "id": "Distilled (menyuling)." },
  { "en": "Bentuk past tense dari 'distinguish'?", "id": "Distinguished (membedakan)." },
  { "en": "Bentuk past participle dari 'distinguish'?", "id": "Distinguished (membedakan)." },
  { "en": "Bentuk past tense dari 'distort'?", "id": "Distorted (memutarbalikkan)." },
  { "en": "Bentuk past participle dari 'distort'?", "id": "Distorted (memutarbalikkan)." },
  { "en": "Bentuk past tense dari 'distract'?", "id": "Distracted (mengalihkan perhatian)." },
  { "en": "Bentuk past participle dari 'distract'?", "id": "Distracted (mengalihkan perhatian)." },
  { "en": "Bentuk past tense dari 'distribute'?", "id": "Distributed (mendistribusikan)." },
  { "en": "Bentuk past participle dari 'distribute'?", "id": "Distributed (mendistribusikan)." },
  { "en": "Bentuk past tense dari 'distrust'?", "id": "Distrusted (tidak percaya)." },
  { "en": "Bentuk past participle dari 'distrust'?", "id": "Distrusted (tidak percaya)." },
  { "en": "Bentuk past tense dari 'disturb'?", "id": "Disturbed (mengganggu)." },
  { "en": "Bentuk past participle dari 'disturb'?", "id": "Disturbed (mengganggu)." },
  { "en": "Bentuk past tense dari 'ditch'?", "id": "Ditched (membuang)." },
  { "en": "Bentuk past participle dari 'ditch'?", "id": "Ditched (membuang)." },
  { "en": "Bentuk past tense dari 'dive'?", "id": "Dived / Dove (menyelam)." },
  { "en": "Bentuk past participle dari 'dive'?", "id": "Dived (menyelam)." },
  { "en": "Bentuk past tense dari 'divert'?", "id": "Diverted (mengalihkan)." },
  { "en": "Bentuk past participle dari 'divert'?", "id": "Diverted (mengalihkan)." },
  { "en": "Bentuk past tense dari 'dominate'?", "id": "Dominated (mendominasi)." },
  { "en": "Bentuk past participle dari 'dominate'?", "id": "Dominated (mendominasi)." },
  { "en": "Bentuk past tense dari 'donate'?", "id": "Donated (menyumbang)." },
  { "en": "Bentuk past participle dari 'donate'?", "id": "Donated (menyumbang)." },
  { "en": "Bentuk past tense dari 'doodle'?", "id": "Doodled (mencoret-coret)." },
  { "en": "Bentuk past participle dari 'doodle'?", "id": "Doodled (mencoret-coret)." },
  { "en": "Bentuk past tense dari 'double'?", "id": "Doubled (menggandakan)." },
  { "en": "Bentuk past participle dari 'double'?", "id": "Doubled (menggandakan)." },
  { "en": "Bentuk past tense dari 'draft'?", "id": "Drafted (menyusun konsep)." },
  { "en": "Bentuk past participle dari 'draft'?", "id": "Drafted (menyusun konsep)." },
  { "en": "Bentuk past tense dari 'drag'?", "id": "Dragged (menyeret)." },
  { "en": "Bentuk past participle dari 'drag'?", "id": "Dragged (menyeret)." },
  { "en": "Bentuk past tense dari 'drain'?", "id": "Drained (menguras)." },
  { "en": "Bentuk past participle dari 'drain'?", "id": "Drained (menguras)." },
  { "en": "Bentuk past tense dari 'dramatize'?", "id": "Dramatized (mendramatisir)." },
  { "en": "Bentuk past participle dari 'dramatize'?", "id": "Dramatized (mendramatisir)." },
  { "en": "Bentuk past tense dari 'drench'?", "id": "Drenched (membasahi kuyup)." },
  { "en": "Bentuk past participle dari 'drench'?", "id": "Drenched (membasahi kuyup)." },
  { "en": "Bentuk past tense dari 'dribble'?", "id": "Dribbled (menggiring bola)." },
  { "en": "Bentuk past participle dari 'dribble'?", "id": "Dribbled (menggiring bola)." },
  { "en": "Bentuk past tense dari 'drift'?", "id": "Drifted (hanyut)." },
  { "en": "Bentuk past participle dari 'drift'?", "id": "Drifted (hanyut)." },
  { "en": "Bentuk past tense dari 'drill'?", "id": "Drilled (mengebor)." },
  { "en": "Bentuk past participle dari 'drill'?", "id": "Drilled (mengebor)." },
  { "en": "Bentuk past tense dari 'drip'?", "id": "Dripped (menetes)." },
  { "en": "Bentuk past participle dari 'drip'?", "id": "Dripped (menetes)." },
  { "en": "Bentuk past tense dari 'drown'?", "id": "Drowned (tenggelam)." },
  { "en": "Bentuk past participle dari 'drown'?", "id": "Drowned (tenggelam)." },
  { "en": "Bentuk past tense dari 'drum'?", "id": "Drummed (menabuh drum)." },
  { "en": "Bentuk past participle dari 'drum'?", "id": "Drummed (menabuh drum)." },
  { "en": "Bentuk past tense dari 'dump'?", "id": "Dumped (membuang)." },
  { "en": "Bentuk past participle dari 'dump'?", "id": "Dumped (membuang)." },
  { "en": "Bentuk past tense dari 'duplicate'?", "id": "Duplicated (menggandakan)." },
  { "en": "Bentuk past participle dari 'duplicate'?", "id": "Duplicated (menggandakan)." },
  { "en": "Bentuk past tense dari 'dwell'?", "id": "Dwelt / Dwelled (mendiami)." },
  { "en": "Bentuk past participle dari 'dwell'?", "id": "Dwelt / Dwelled (mendiami)." },
  { "en": "Bentuk past tense dari 'dye'?", "id": "Dyed (mewarnai)." },
  { "en": "Bentuk past participle dari 'dye'?", "id": "Dyed (mewarnai)." },
  { "en": "Bentuk past tense dari 'eject'?", "id": "Ejected (mengeluarkan)." },
  { "en": "Bentuk past participle dari 'eject'?", "id": "Ejected (mengeluarkan)." },
  { "en": "Bentuk past tense dari 'elaborate'?", "id": "Elaborated (menguraikan)." },
  { "en": "Bentuk past participle dari 'elaborate'?", "id": "Elaborated (menguraikan)." },
  { "en": "Bentuk past tense dari 'elapse'?", "id": "Elapsed (berlalu)." },
  { "en": "Bentuk past participle dari 'elapse'?", "id": "Elapsed (berlalu)." },
  { "en": "Bentuk past tense dari 'elevate'?", "id": "Elevated (mengangkat)." },
  { "en": "Bentuk past participle dari 'elevate'?", "id": "Elevated (mengangkat)." },
  { "en": "Bentuk past tense dari 'eliminate'?", "id": "Eliminated (menghilangkan)." },
  { "en": "Bentuk past participle dari 'eliminate'?", "id": "Eliminated (menghilangkan)." },
  { "en": "Bentuk past tense dari 'elope'?", "id": "Eloped (kawin lari)." },
  { "en": "Bentuk past participle dari 'elope'?", "id": "Eloped (kawin lari)." },
  { "en": "Bentuk past tense dari 'emancipate'?", "id": "Emancipated (membebaskan)." },
  { "en": "Bentuk past participle dari 'emancipate'?", "id": "Emancipated (membebaskan)." },
  { "en": "Bentuk past tense dari 'embark'?", "id": "Embarked (memulai perjalanan)." },
  { "en": "Bentuk past participle dari 'embark'?", "id": "Embarked (memulai perjalanan)." },
  { "en": "Bentuk past tense dari 'embarrass'?", "id": "Embarrassed (memalukan)." },
  { "en": "Bentuk past participle dari 'embarrass'?", "id": "Embarrassed (memalukan)." },
  { "en": "Bentuk past tense dari 'embed'?", "id": "Embedded (menanamkan)." },
  { "en": "Bentuk past participle dari 'embed'?", "id": "Embedded (menanamkan)." },
  { "en": "Bentuk past tense dari 'embody'?", "id": "Embodied (mewujudkan)." },
  { "en": "Bentuk past participle dari 'embody'?", "id": "Embodied (mewujudkan)." },
  { "en": "Bentuk past tense dari 'embrace'?", "id": "Embraced (memeluk)." },
  { "en": "Bentuk past participle dari 'embrace'?", "id": "Embraced (memeluk)." },
  { "en": "Bentuk past tense dari 'emerge'?", "id": "Emerged (muncul)." },
  { "en": "Bentuk past participle dari 'emerge'?", "id": "Emerged (muncul)." },
  { "en": "Bentuk past tense dari 'emit'?", "id": "Emitted (memancarkan)." },
  { "en": "Bentuk past participle dari 'emit'?", "id": "Emitted (memancarkan)." },
  { "en": "Bentuk past tense dari 'emphasize'?", "id": "Emphasized (menekankan)." },
  { "en": "Bentuk past participle dari 'emphasize'?", "id": "Emphasized (menekankan)." },
  { "en": "Bentuk past tense dari 'employ'?", "id": "Employed (mempekerjakan)." },
  { "en": "Bentuk past participle dari 'employ'?", "id": "Employed (mempekerjakan)." },
  { "en": "Bentuk past tense dari 'empower'?", "id": "Empowered (memberdayakan)." },
  { "en": "Bentuk past participle dari 'empower'?", "id": "Empowered (memberdayakan)." },
  { "en": "Bentuk past tense dari 'empty'?", "id": "Emptied (mengosongkan)." },
  { "en": "Bentuk past participle dari 'empty'?", "id": "Emptied (mengosongkan)." },
  { "en": "Bentuk past tense dari 'emulate'?", "id": "Emulated (meniru)." },
  { "en": "Bentuk past participle dari 'emulate'?", "id": "Emulated (meniru)." },
  { "en": "Bentuk past tense dari 'enable'?", "id": "Enabled (memungkinkan)." },
  { "en": "Bentuk past participle dari 'enable'?", "id": "Enabled (memungkinkan)." },
  { "en": "Bentuk past tense dari 'enact'?", "id": "Enacted (menetapkan)." },
  { "en": "Bentuk past participle dari 'enact'?", "id": "Enacted (menetapkan)." },
  { "en": "Bentuk past tense dari 'encase'?", "id": "Encased (menyelubungi)." },
  { "en": "Bentuk past participle dari 'encase'?", "id": "Encased (menyelubungi)." },
  { "en": "Bentuk past tense dari 'enchant'?", "id": "Enchanted (mempesona)." },
  { "en": "Bentuk past participle dari 'enchant'?", "id": "Enchanted (mempesona)." },
  { "en": "Bentuk past tense dari 'encircle'?", "id": "Encircled (mengelilingi)." },
  { "en": "Bentuk past participle dari 'encircle'?", "id": "Encircled (mengelilingi)." },
  { "en": "Bentuk past tense dari 'enclose'?", "id": "Enclosed (melampirkan)." },
  { "en": "Bentuk past participle dari 'enclose'?", "id": "Enclosed (melampirkan)." },
  { "en": "Bentuk past tense dari 'encounter'?", "id": "Encountered (menemui)." },
  { "en": "Bentuk past participle dari 'encounter'?", "id": "Encountered (menemui)." },
  { "en": "Bentuk past tense dari 'endanger'?", "id": "Endangered (membahayakan)." },
  { "en": "Bentuk past participle dari 'endanger'?", "id": "Endangered (membahayakan)." },
  { "en": "Bentuk past tense dari 'endorse'?", "id": "Endorsed (mendukung)." },
  { "en": "Bentuk past participle dari 'endorse'?", "id": "Endorsed (mendukung)." },
  { "en": "Bentuk past tense dari 'endure'?", "id": "Endured (menahan)." },
  { "en": "Bentuk past participle dari 'endure'?", "id": "Endured (menahan)." },
  { "en": "Bentuk past tense dari 'enforce'?", "id": "Enforced (menegakkan)." },
  { "en": "Bentuk past participle dari 'enforce'?", "id": "Enforced (menegakkan)." },
  { "en": "Bentuk past tense dari 'engage'?", "id": "Engaged (terlibat)." },
  { "en": "Bentuk past participle dari 'engage'?", "id": "Engaged (terlibat)." },
  { "en": "Bentuk past tense dari 'engrave'?", "id": "Engraved (mengukir)." },
  { "en": "Bentuk past participle dari 'engrave'?", "id": "Engraved (mengukir)." },
  { "en": "Bentuk past tense dari 'engulf'?", "id": "Engulfed (menelan)." },
  { "en": "Bentuk past participle dari 'engulf'?", "id": "Engulfed (menelan)." },
  { "en": "Bentuk past tense dari 'enhance'?", "id": "Enhanced (meningkatkan)." },
  { "en": "Bentuk past participle dari 'enhance'?", "id": "Enhanced (meningkatkan)." },
  { "en": "Bentuk past tense dari 'enlist'?", "id": "Enlisted (mendaftar)." },
  { "en": "Bentuk past participle dari 'enlist'?", "id": "Enlisted (mendaftar)." },
  { "en": "Bentuk past tense dari 'enrage'?", "id": "Enraged (membuat marah)." },
  { "en": "Bentuk past participle dari 'enrage'?", "id": "Enraged (membuat marah)." },
  { "en": "Bentuk past tense dari 'enrich'?", "id": "Enriched (memperkaya)." },
  { "en": "Bentuk past participle dari 'enrich'?", "id": "Enriched (memperkaya)." },
  { "en": "Bentuk past tense dari 'enroll'?", "id": "Enrolled (mendaftar)." },
  { "en": "Bentuk past participle dari 'enroll'?", "id": "Enrolled (mendaftar)." },
  { "en": "Bentuk past tense dari 'ensnare'?", "id": "Ensnared (menjerat)." },
  { "en": "Bentuk past participle dari 'ensnare'?", "id": "Ensnared (menjerat)." },
  { "en": "Bentuk past tense dari 'ensure'?", "id": "Ensured (memastikan)." },
  { "en": "Bentuk past participle dari 'ensure'?", "id": "Ensured (memastikan)." },
  { "en": "Bentuk past tense dari 'entail'?", "id": "Entailed (mengakibatkan)." },
  { "en": "Bentuk past participle dari 'entail'?", "id": "Entailed (mengakibatkan)." },
  { "en": "Bentuk past tense dari 'enter'?", "id": "Entered (memasuki)." },
  { "en": "Bentuk past participle dari 'enter'?", "id": "Entered (memasuki)." },
  { "en": "Bentuk past tense dari 'entice'?", "id": "Enticed (membujuk)." },
  { "en": "Bentuk past participle dari 'entice'?", "id": "Enticed (membujuk)." },
  { "en": "Bentuk past tense dari 'entitle'?", "id": "Entitled (memberi hak)." },
  { "en": "Bentuk past participle dari 'entitle'?", "id": "Entitled (memberi hak)." },
  { "en": "Bentuk past tense dari 'entrust'?", "id": "Entrusted (mempercayakan)." },
  { "en": "Bentuk past participle dari 'entrust'?", "id": "Entrusted (mempercayakan)." },
  { "en": "Bentuk past tense dari 'envelop'?", "id": "Enveloped (menyelubungi)." },
  { "en": "Bentuk past participle dari 'envelop'?", "id": "Enveloped (menyelubungi)." },
  { "en": "Bentuk past tense dari 'envy'?", "id": "Envied (iri)." },
  { "en": "Bentuk past participle dari 'envy'?", "id": "Envied (iri)." },
  { "en": "Bentuk past tense dari 'equal'?", "id": "Equaled (menyamai)." },
  { "en": "Bentuk past participle dari 'equal'?", "id": "Equaled (menyamai)." },
  { "en": "Bentuk past tense dari 'equip'?", "id": "Equipped (melengkapi)." },
  { "en": "Bentuk past participle dari 'equip'?", "id": "Equipped (melengkapi)." },
  { "en": "Bentuk past tense dari 'eradicate'?", "id": "Eradicated (memberantas)." },
  { "en": "Bentuk past participle dari 'eradicate'?", "id": "Eradicated (memberantas)." },
  { "en": "Bentuk past tense dari 'erase'?", "id": "Erased (menghapus)." },
  { "en": "Bentuk past participle dari 'erase'?", "id": "Erased (menghapus)." },
  { "en": "Bentuk past tense dari 'erect'?", "id": "Erected (mendirikan)." },
  { "en": "Bentuk past participle dari 'erect'?", "id": "Erected (mendirikan)." },
  { "en": "Bentuk past tense dari 'erode'?", "id": "Eroded (mengikis)." },
  { "en": "Bentuk past participle dari 'erode'?", "id": "Eroded (mengikis)." },
  { "en": "Bentuk past tense dari 'erupt'?", "id": "Erupted (meletus)." },
  { "en": "Bentuk past participle dari 'erupt'?", "id": "Erupted (meletus)." },
  { "en": "Bentuk past tense dari 'escalate'?", "id": "Escalated (meningkat)." },
  { "en": "Bentuk past participle dari 'escalate'?", "id": "Escalated (meningkat)." },
  { "en": "Bentuk past tense dari 'establish'?", "id": "Established (mendirikan)." },
  { "en": "Bentuk past participle dari 'establish'?", "id": "Established (mendirikan)." },
  { "en": "Bentuk past tense dari 'estimate'?", "id": "Estimated (memperkirakan)." },
  { "en": "Bentuk past participle dari 'estimate'?", "id": "Estimated (memperkirakan)." },
  { "en": "Bentuk past tense dari 'etch'?", "id": "Etched (mengetsa)." },
  { "en": "Bentuk past participle dari 'etch'?", "id": "Etched (mengetsa)." },
  { "en": "Bentuk past tense dari 'evacuate'?", "id": "Evacuated (mengevakuasi)." },
  { "en": "Bentuk past participle dari 'evacuate'?", "id": "Evacuated (mengevakuasi)." },
  { "en": "Bentuk past tense dari 'evade'?", "id": "Evaded (menghindari)." },
  { "en": "Bentuk past participle dari 'evade'?", "id": "Evaded (menghindari)." },
  { "en": "Bentuk past tense dari 'evaluate'?", "id": "Evaluated (mengevaluasi)." },
  { "en": "Bentuk past participle dari 'evaluate'?", "id": "Evaluated (mengevaluasi)." },
  { "en": "Bentuk past tense dari 'evaporate'?", "id": "Evaporated (menguap)." },
  { "en": "Bentuk past participle dari 'evaporate'?", "id": "Evaporated (menguap)." },
  { "en": "Bentuk past tense dari 'evict'?", "id": "Evicted (mengusir)." },
  { "en": "Bentuk past participle dari 'evict'?", "id": "Evicted (mengusir)." },
  { "en": "Bentuk past tense dari 'evoke'?", "id": "Evoked (membangkitkan)." },
  { "en": "Bentuk past participle dari 'evoke'?", "id": "Evoked (membangkitkan)." },
  { "en": "Bentuk past tense dari 'evolve'?", "id": "Evolved (berevolusi)." },
  { "en": "Bentuk past participle dari 'evolve'?", "id": "Evolved (berevolusi)." },
  { "en": "Bentuk past tense dari 'exaggerate'?", "id": "Exaggerated (melebih-lebihkan)." },
  { "en": "Bentuk past participle dari 'exaggerate'?", "id": "Exaggerated (melebih-lebihkan)." },
  { "en": "Bentuk past tense dari 'exalt'?", "id": "Exalted (mengagungkan)." },
  { "en": "Bentuk past participle dari 'exalt'?", "id": "Exalted (mengagungkan)." },
  { "en": "Bentuk past tense dari 'exceed'?", "id": "Exceeded (melebihi)." },
  { "en": "Bentuk past participle dari 'exceed'?", "id": "Exceeded (melebihi)." },
  { "en": "Bentuk past tense dari 'excel'?", "id": "Excelled (unggul)." },
  { "en": "Bentuk past participle dari 'excel'?", "id": "Excelled (unggul)." },
  { "en": "Bentuk past tense dari 'exchange'?", "id": "Exchanged (bertukar)." },
  { "en": "Bentuk past participle dari 'exchange'?", "id": "Exchanged (bertukar)." },
  { "en": "Bentuk past tense dari 'exclude'?", "id": "Excluded (mengecualikan)." },
  { "en": "Bentuk past participle dari 'exclude'?", "id": "Excluded (mengecualikan)." },
  { "en": "Bentuk past tense dari 'execute'?", "id": "Executed (menjalankan/mengeksekusi)." },
  { "en": "Bentuk past participle dari 'execute'?", "id": "Executed (menjalankan/mengeksekusi)." },
  { "en": "Bentuk past tense dari 'exemplify'?", "id": "Exemplified (mencontohkan)." },
  { "en": "Bentuk past participle dari 'exemplify'?", "id": "Exemplified (mencontohkan)." },
  { "en": "Bentuk past tense dari 'exercise'?", "id": "Exercised (berolahraga)." },
  { "en": "Bentuk past participle dari 'exercise'?", "id": "Exercised (berolahraga)." },
  { "en": "Bentuk past tense dari 'exert'?", "id": "Exerted (menggunakan)." },
  { "en": "Bentuk past participle dari 'exert'?", "id": "Exerted (menggunakan)." },
  { "en": "Bentuk past tense dari 'exhale'?", "id": "Exhaled (menghembuskan napas)." },
  { "en": "Bentuk past participle dari 'exhale'?", "id": "Exhaled (menghembuskan napas)." },
  { "en": "Bentuk past tense dari 'exhaust'?", "id": "Exhausted (melelahkan)." },
  { "en": "Bentuk past participle dari 'exhaust'?", "id": "Exhausted (melelahkan)." },
  { "en": "Bentuk past tense dari 'exhibit'?", "id": "Exhibited (memamerkan)." },
  { "en": "Bentuk past participle dari 'exhibit'?", "id": "Exhibited (memamerkan)." },
  { "en": "Bentuk past tense dari 'exile'?", "id": "Exiled (mengasingkan)." },
  { "en": "Bentuk past participle dari 'exile'?", "id": "Exiled (mengasingkan)." },
  { "en": "Bentuk past tense dari 'expand'?", "id": "Expanded (memperluas)." },
  { "en": "Bentuk past participle dari 'expand'?", "id": "Expanded (memperluas)." },
  { "en": "Bentuk past tense dari 'expedite'?", "id": "Expedited (mempercepat)." },
  { "en": "Bentuk past participle dari 'expedite'?", "id": "Expedited (mempercepat)." },
  { "en": "Bentuk past tense dari 'experiment'?", "id": "Experimented (bereksperimen)." },
  { "en": "Bentuk past participle dari 'experiment'?", "id": "Experimented (bereksperimen)." },
  { "en": "Bentuk past tense dari 'expire'?", "id": "Expired (kedaluwarsa)." },
  { "en": "Bentuk past participle dari 'expire'?", "id": "Expired (kedaluwarsa)." },
  { "en": "Bentuk past tense dari 'explode'?", "id": "Exploded (meledak)." },
  { "en": "Bentuk past participle dari 'explode'?", "id": "Exploded (meledak)." },
  { "en": "Bentuk past tense dari 'exploit'?", "id": "Exploited (mengeksploitasi)." },
  { "en": "Bentuk past participle dari 'exploit'?", "id": "Exploited (mengeksploitasi)." },
  { "en": "Bentuk past tense dari 'explore'?", "id": "Explored (menjelajahi)." },
  { "en": "Bentuk past participle dari 'explore'?", "id": "Explored (menjelajahi)." },
  { "en": "Bentuk past tense dari 'export'?", "id": "Exported (mengekspor)." },
  { "en": "Bentuk past participle dari 'export'?", "id": "Exported (mengekspor)." },
  { "en": "Bentuk past tense dari 'expose'?", "id": "Exposed (memaparkan)." },
  { "en": "Bentuk past participle dari 'expose'?", "id": "Exposed (memaparkan)." },
  { "en": "Bentuk past tense dari 'express'?", "id": "Expressed (mengekspresikan)." },
  { "en": "Bentuk past participle dari 'express'?", "id": "Expressed (mengekspresikan)." },
  { "en": "Bentuk past tense dari 'extend'?", "id": "Extended (memperpanjang)." },
  { "en": "Bentuk past participle dari 'extend'?", "id": "Extended (memperpanjang)." },
  { "en": "Bentuk past tense dari 'exterminate'?", "id": "Exterminated (memusnahkan)." },
  { "en": "Bentuk past participle dari 'exterminate'?", "id": "Exterminated (memusnahkan)." },
  { "en": "Bentuk past tense dari 'extinguish'?", "id": "Extinguished (memadamkan)." },
  { "en": "Bentuk past participle dari 'extinguish'?", "id": "Extinguished (memadamkan)." },
  { "en": "Bentuk past tense dari 'extract'?", "id": "Extracted (mengekstrak)." },
  { "en": "Bentuk past participle dari 'extract'?", "id": "Extracted (mengekstrak)." },
  { "en": "Bentuk past tense dari 'eye'?", "id": "Eyed (mengamati)." },
  { "en": "Bentuk past participle dari 'eye'?", "id": "Eyed (mengamati)." },
  { "en": "Bentuk past tense dari 'fabricate'?", "id": "Fabricated (membuat/merekayasa)." },
  { "en": "Bentuk past participle dari 'fabricate'?", "id": "Fabricated (membuat/merekayasa)." },
  { "en": "Bentuk past tense dari 'face'?", "id": "Faced (menghadapi)." },
  { "en": "Bentuk past participle dari 'face'?", "id": "Faced (menghadapi)." },
  { "en": "Bentuk past tense dari 'facilitate'?", "id": "Facilitated (memfasilitasi)." },
  { "en": "Bentuk past participle dari 'facilitate'?", "id": "Facilitated (memfasilitasi)." },
  { "en": "Bentuk past tense dari 'fade'?", "id": "Faded (memudar)." },
  { "en": "Bentuk past participle dari 'fade'?", "id": "Faded (memudar)." },
  { "en": "Bentuk past tense dari 'faint'?", "id": "Fainted (pingsan)." },
  { "en": "Bentuk past participle dari 'faint'?", "id": "Fainted (pingsan)." },
  { "en": "Bentuk past tense dari 'falsify'?", "id": "Falsified (memalsukan)." },
  { "en": "Bentuk past participle dari 'falsify'?", "id": "Falsified (memalsukan)." },
  { "en": "Bentuk past tense dari 'falter'?", "id": "Faltered (terbata-bata/goyah)." },
  { "en": "Bentuk past participle dari 'falter'?", "id": "Faltered (terbata-bata/goyah)." },
  { "en": "Bentuk past tense dari 'famish'?", "id": "Famished (kelaparan)." },
  { "en": "Bentuk past participle dari 'famish'?", "id": "Famished (kelaparan)." },
  { "en": "Bentuk past tense dari 'fan'?", "id": "Fanned (mengipasi)." },
  { "en": "Bentuk past participle dari 'fan'?", "id": "Fanned (mengipasi)." },
  { "en": "Bentuk past tense dari 'fancy'?", "id": "Fancied (menyukai/membayangkan)." },
  { "en": "Bentuk past participle dari 'fancy'?", "id": "Fancied (menyukai/membayangkan)." },
  { "en": "Bentuk past tense dari 'farm'?", "id": "Farmed (bertani)." },
  { "en": "Bentuk past participle dari 'farm'?", "id": "Farmed (bertani)." },
  { "en": "Bentuk past tense dari 'fasten'?", "id": "Fastened (mengencangkan)." },
  { "en": "Bentuk past participle dari 'fasten'?", "id": "Fastened (mengencangkan)." },
  { "en": "Bentuk past tense dari 'favour'?", "id": "Favoured (menyukai/mendukung)." },
  { "en": "Bentuk past participle dari 'favour'?", "id": "Favoured (menyukai/mendukung)." },
  { "en": "Bentuk past tense dari 'fax'?", "id": "Faxed (mengirim faks)." },
  { "en": "Bentuk past participle dari 'fax'?", "id": "Faxed (mengirim faks)." },
  { "en": "Bentuk past tense dari 'fear'?", "id": "Feared (takut)." },
  { "en": "Bentuk past participle dari 'fear'?", "id": "Feared (takut)." },
  { "en": "Bentuk past tense dari 'ferry'?", "id": "Ferried (menyeberangkan)." },
  { "en": "Bentuk past participle dari 'ferry'?", "id": "Ferried (menyeberangkan)." },
  { "en": "Bentuk past tense dari 'fetch'?", "id": "Fetched (mengambil)." },
  { "en": "Bentuk past participle dari 'fetch'?", "id": "Fetched (mengambil)." },
  { "en": "Bentuk past tense dari 'file'?", "id": "Filed (mengajukan/mengarsipkan)." },
  { "en": "Bentuk past participle dari 'file'?", "id": "Filed (mengajukan/mengarsipkan)." },
  { "en": "Bentuk past tense dari 'filter'?", "id": "Filtered (menyaring)." },
  { "en": "Bentuk past participle dari 'filter'?", "id": "Filtered (menyaring)." },
  { "en": "Bentuk past tense dari 'finance'?", "id": "Financed (membiayai)." },
  { "en": "Bentuk past participle dari 'finance'?", "id": "Financed (membiayai)." },
  { "en": "Bentuk past tense dari 'fire'?", "id": "Fired (memecat/menembak)." },
  { "en": "Bentuk past participle dari 'fire'?", "id": "Fired (memecat/menembak)." },
  { "en": "Bentuk past tense dari 'fish'?", "id": "Fished (memancing)." },
  { "en": "Bentuk past participle dari 'fish'?", "id": "Fished (memancing)." },
  { "en": "Bentuk past tense dari 'fit'?", "id": "Fit / Fitted (pas/cocok)." },
  { "en": "Bentuk past participle dari 'fit'?", "id": "Fit / Fitted (pas/cocok)." },
  { "en": "Bentuk past tense dari 'fix'?", "id": "Fixed (memperbaiki)." },
  { "en": "Bentuk past participle dari 'fix'?", "id": "Fixed (memperbaiki)." },
  { "en": "Bentuk past tense dari 'fizz'?", "id": "Fizzed (mendesis)." },
  { "en": "Bentuk past participle dari 'fizz'?", "id": "Fizzed (mendesis)." },
  { "en": "Bentuk past tense dari 'flap'?", "id": "Flapped (mengepak)." },
  { "en": "Bentuk past participle dari 'flap'?", "id": "Flapped (mengepak)." },
  { "en": "Bentuk past tense dari 'flare'?", "id": "Flared (menyala)." },
  { "en": "Bentuk past participle dari 'flare'?", "id": "Flared (menyala)." },
  { "en": "Bentuk past tense dari 'flash'?", "id": "Flashed (berkilat)." },
  { "en": "Bentuk past participle dari 'flash'?", "id": "Flashed (berkilat)." },
  { "en": "Bentuk past tense dari 'flatten'?", "id": "Flattened (meratakan)." },
  { "en": "Bentuk past participle dari 'flatten'?", "id": "Flattened (meratakan)." },
  { "en": "Bentuk past tense dari 'flatter'?", "id": "Flattered (merayu)." },
  { "en": "Bentuk past participle dari 'flatter'?", "id": "Flattered (merayu)." },
  { "en": "Bentuk past tense dari 'flaunt'?", "id": "Flaunted (memamerkan)." },
  { "en": "Bentuk past participle dari 'flaunt'?", "id": "Flaunted (memamerkan)." },
  { "en": "Bentuk past tense dari 'fling'?", "id": "Flung (melemparkan)." },
  { "en": "Bentuk past participle dari 'fling'?", "id": "Flung (melemparkan)." },
  { "en": "Bentuk past tense dari 'flit'?", "id": "Flitted (melintas cepat)." },
  { "en": "Bentuk past participle dari 'flit'?", "id": "Flitted (melintas cepat)." },
  { "en": "Bentuk past tense dari 'float'?", "id": "Floated (mengapung)." },
  { "en": "Bentuk past participle dari 'float'?", "id": "Floated (mengapung)." },
  { "en": "Bentuk past tense dari 'flock'?", "id": "Flocked (berbondong-bondong)." },
  { "en": "Bentuk past participle dari 'flock'?", "id": "Flocked (berbondong-bondong)." },
  { "en": "Bentuk past tense dari 'flog'?", "id": "Flogged (mencambuk)." },
  { "en": "Bentuk past participle dari 'flog'?", "id": "Flogged (mencambuk)." },
  { "en": "Bentuk past tense dari 'flood'?", "id": "Flooded (membanjiri)." },
  { "en": "Bentuk past participle dari 'flood'?", "id": "Flooded (membanjiri)." },
  { "en": "Bentuk past tense dari 'flop'?", "id": "Flopped (gagal/jatuh)." },
  { "en": "Bentuk past participle dari 'flop'?", "id": "Flopped (gagal/jatuh)." },
  { "en": "Bentuk past tense dari 'flounder'?", "id": "Floundered (meronta-ronta)." },
  { "en": "Bentuk past participle dari 'flounder'?", "id": "Floundered (meronta-ronta)." },
  { "en": "Bentuk past tense dari 'flourish'?", "id": "Flourished (berkembang pesat)." },
  { "en": "Bentuk past participle dari 'flourish'?", "id": "Flourished (berkembang pesat)." },
  { "en": "Bentuk past tense dari 'flow'?", "id": "Flowed (mengalir)." },
  { "en": "Bentuk past participle dari 'flow'?", "id": "Flowed (mengalir)." },
  { "en": "Bentuk past tense dari 'fluctuate'?", "id": "Fluctuated (berfluktuasi)." },
  { "en": "Bentuk past participle dari 'fluctuate'?", "id": "Fluctuated (berfluktuasi)." },
  { "en": "Bentuk past tense dari 'flush'?", "id": "Flushed (menyiram/merona)." },
  { "en": "Bentuk past participle dari 'flush'?", "id": "Flushed (menyiram/merona)." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>

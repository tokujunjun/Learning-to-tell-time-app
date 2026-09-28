# Learning-to-tell-time-app
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>たのしい！とけいの れんしゅう - 小学2年生 算数</title>
    <!-- GitHub Pages compatible CDN links (HTTPS) -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Zen+Maru+Gothic:wght@500;700;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            pink: '#FF6B8B',
                            yellow: '#FFD166',
                            green: '#06D6A0',
                            blue: '#118AB2',
                            purple: '#8338EC',
                            dark: '#073B4C',
                            bg: '#FFF9E6'
                        }
                    },
                    fontFamily: {
                        sans: ['"Zen Maru Gothic"', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Zen Maru Gothic', sans-serif;
            background-color: #FFF9E6;
            touch-action: manipulation;
            -webkit-tap-highlight-color: transparent;
        }
        .pop-btn {
            transition: transform 0.1s ease, box-shadow 0.1s ease;
        }
        .pop-btn:active {
            transform: translateY(4px);
            box-shadow: none !important;
        }
        .clock-shadow {
            filter: drop-shadow(0px 8px 12px rgba(0, 0, 0, 0.08));
        }
        @keyframes bounce-gentle {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-8px); }
        }
        .animate-bounce-gentle {
            animation: bounce-gentle 2s infinite ease-in-out;
        }
        @keyframes celebrate {
            0% { transform: scale(0.8); opacity: 0; }
            50% { transform: scale(1.05); opacity: 1; }
            100% { transform: scale(1); opacity: 1; }
        }
        .animate-celebrate {
            animation: celebrate 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }
    </style>
</head>
<body class="min-h-screen text-slate-800 flex flex-col justify-between selection:bg-pink-200">

    <header class="bg-white border-b-4 border-amber-300 px-4 py-3 sticky top-0 z-50 shadow-sm">
        <div class="max-w-3xl mx-auto flex justify-between items-center">
            <div class="flex items-center gap-2.5 cursor-pointer" onclick="showMenu()">
                <div class="w-10 h-10 rounded-2xl bg-amber-400 flex items-center justify-center text-white text-xl shadow-md">
                    <i class="fa-regular fa-clock"></i>
                </div>
                <div>
                    <h1 class="text-xl md:text-2xl font-black text-amber-600 tracking-wider">とけいマスター</h1>
                    <p class="text-xs text-slate-400 font-bold">しょうがく２ねんせい 算数</p>
                </div>
            </div>
            
            <div class="flex items-center gap-3">
                <button id="sound-toggle" onclick="toggleSound()" class="w-10 h-10 rounded-full bg-slate-100 border-2 border-slate-200 text-slate-600 flex items-center justify-center text-lg hover:bg-slate-200 transition" title="おとの つけけし">
                    <i id="sound-icon" class="fa-solid fa-volume-high"></i>
                </button>
                <button onclick="showStamps()" class="px-3 py-1.5 rounded-full bg-rose-100 border-2 border-rose-300 text-rose-600 text-sm font-bold flex items-center gap-1.5 hover:bg-rose-200 transition">
                    <i class="fa-solid fa-star text-amber-400"></i>
                    <span id="header-stars">0</span>こ
                </button>
            </div>
        </div>
    </header>

    <main class="flex-grow container max-w-3xl mx-auto p-4 flex flex-col justify-center">

        <!-- 1. メニュー画面 -->
        <div id="screen-menu" class="space-y-6 animate-celebrate">
            <div class="text-center space-y-2 py-2">
                <span class="bg-amber-200 text-amber-800 px-4 py-1 rounded-full text-xs md:text-sm font-black tracking-wide">
                    どれに ちょうせん する？
                </span>
                <h2 class="text-2xl md:text-3xl font-black text-slate-700">もんだいコースを えらぼう！</h2>
            </div>

            <!-- むずかしさ設定 -->
            <div class="bg-white p-4 rounded-2xl border-2 border-slate-200 shadow-sm flex flex-col sm:flex-row items-center justify-between gap-3">
                <span class="font-bold text-slate-600 text-sm"><i class="fa-solid fa-layer-group text-amber-500 mr-2"></i>むずかしさ：</span>
                <div class="flex gap-2 w-full sm:w-auto justify-center">
                    <button id="diff-easy" onclick="setDifficulty('easy')" class="px-3 py-1.5 rounded-xl border-2 font-bold text-xs md:text-sm bg-emerald-500 text-white border-emerald-600 shadow">かんたん (10ふん)</button>
                    <button id="diff-normal" onclick="setDifficulty('normal')" class="px-3 py-1.5 rounded-xl border-2 font-bold text-xs md:text-sm bg-slate-100 text-slate-600 border-slate-200">ふつう (5ふん)</button>
                    <button id="diff-hard" onclick="setDifficulty('hard')" class="px-3 py-1.5 rounded-xl border-2 font-bold text-xs md:text-sm bg-slate-100 text-slate-600 border-slate-200">むずかしい (1ふん)</button>
                </div>
            </div>

            <!-- コース一覧 -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <button onclick="startQuiz('after')" class="pop-btn bg-sky-400 hover:bg-sky-500 text-white p-5 rounded-3xl border-b-8 border-sky-600 text-left shadow-lg flex items-center gap-4">
                    <div class="w-16 h-16 rounded-2xl bg-white/20 flex items-center justify-center text-3xl shrink-0">
                        <i class="fa-solid fa-forward"></i>
                    </div>
                    <div>
                        <div class="text-xs bg-sky-600/40 px-2.5 py-0.5 rounded-full inline-block font-bold mb-1">コース ①</div>
                        <h3 class="text-xl font-black">◯ふんごの じかん</h3>
                        <p class="text-xs text-sky-100 mt-1">「〇じ〇ふんの 20ふんごは？」</p>
                    </div>
                </button>

                <button onclick="startQuiz('before')" class="pop-btn bg-indigo-500 hover:bg-indigo-600 text-white p-5 rounded-3xl border-b-8 border-indigo-700 text-left shadow-lg flex items-center gap-4">
                    <div class="w-16 h-16 rounded-2xl bg-white/20 flex items-center justify-center text-3xl shrink-0">
                        <i class="fa-solid fa-backward"></i>
                    </div>
                    <div>
                        <div class="text-xs bg-indigo-700/40 px-2.5 py-0.5 rounded-full inline-block font-bold mb-1">コース ②</div>
                        <h3 class="text-xl font-black">◯ふんまえの じかん</h3>
                        <p class="text-xs text-indigo-100 mt-1">「〇じ〇ふんの 30ふんまえは？」</p>
                    </div>
                </button>

                <button onclick="startQuiz('between')" class="pop-btn bg-emerald-500 hover:bg-emerald-600 text-white p-5 rounded-3xl border-b-8 border-emerald-700 text-left shadow-lg flex items-center gap-4">
                    <div class="w-16 h-16 rounded-2xl bg-white/20 flex items-center justify-center text-3xl shrink-0">
                        <i class="fa-solid fa-arrows-left-right"></i>
                    </div>
                    <div>
                        <div class="text-xs bg-emerald-700/40 px-2.5 py-0.5 rounded-full inline-block font-bold mb-1">コース ③</div>
                        <h3 class="text-xl font-black">あいだの じかん</h3>
                        <p class="text-xs text-emerald-100 mt-1">「〇じから〇じまで なんぷん？」</p>
                    </div>
                </button>

                <button onclick="startQuiz('mix')" class="pop-btn bg-rose-500 hover:bg-rose-600 text-white p-5 rounded-3xl border-b-8 border-rose-700 text-left shadow-lg flex items-center gap-4">
                    <div class="w-16 h-16 rounded-2xl bg-white/20 flex items-center justify-center text-3xl shrink-0">
                        <i class="fa-solid fa-shuffle"></i>
                    </div>
                    <div>
                        <div class="text-xs bg-rose-700/40 px-2.5 py-0.5 rounded-full inline-block font-bold mb-1">コース ④</div>
                        <h3 class="text-xl font-black">ぜんぶ ミックス！</h3>
                        <p class="text-xs text-rose-100 mt-1">いろんな もんだいが でるよ！</p>
                    </div>
                </button>
            </div>
        </div>

        <!-- 2. クイズ解答画面 -->
        <div id="screen-quiz" class="hidden space-y-4">
            <div class="bg-white p-3 rounded-2xl border-2 border-slate-200 shadow-sm flex items-center justify-between gap-4">
                <button onclick="showMenu()" class="text-slate-400 hover:text-slate-600 font-bold text-sm flex items-center gap-1">
                    <i class="fa-solid fa-arrow-left"></i> もどる
                </button>

                <div class="flex-grow max-w-xs bg-slate-100 rounded-full h-4 overflow-hidden border border-slate-200">
                    <div id="progress-bar" class="bg-amber-400 h-full w-0 transition-all duration-300 rounded-full"></div>
                </div>

                <div class="text-sm font-black text-slate-600 shrink-0">
                    だい <span id="q-index" class="text-amber-500 text-lg">1</span> / 5 もん
                </div>
            </div>

            <div class="bg-white p-5 md:p-6 rounded-3xl border-4 border-amber-300 shadow-lg space-y-5">
                <div class="text-center space-y-1">
                    <span id="question-badge" class="bg-amber-100 text-amber-800 text-xs font-bold px-3 py-1 rounded-full">◯ふんごの じかん</span>
                    <h2 id="question-text" class="text-xl md:text-2xl font-black text-slate-800 leading-snug py-2">
                        <!-- 動的更新 -->
                    </h2>
                </div>

                <!-- 時計の表示エリア -->
                <div id="clock-container" class="flex justify-center items-center gap-4 flex-wrap">
                    <div class="flex flex-col items-center gap-1">
                        <span id="clock1-label" class="text-xs font-bold text-slate-500 bg-slate-100 px-2 py-0.5 rounded-full">はじめの じかん</span>
                        <div class="w-40 h-40 md:w-48 md:h-48 relative">
                            <svg id="svg-clock-1" viewBox="0 0 200 200" class="w-full h-full clock-shadow"></svg>
                        </div>
                    </div>

                    <div id="clock2-wrapper" class="hidden flex-col items-center gap-1">
                        <span id="clock2-label" class="text-xs font-bold text-slate-500 bg-slate-100 px-2 py-0.5 rounded-full">おわりの じかん</span>
                        <div class="w-40 h-40 md:w-48 md:h-48 relative">
                            <svg id="svg-clock-2" viewBox="0 0 200 200" class="w-full h-full clock-shadow"></svg>
                        </div>
                    </div>
                </div>

                <!-- 回答入力 -->
                <div class="bg-amber-50/70 rounded-2xl p-4 border-2 border-amber-200/70 flex flex-col items-center gap-3">
                    <p class="text-xs font-bold text-amber-800">こたえを 入力してね！</p>
                    
                    <div class="flex items-center gap-2 text-2xl md:text-3xl font-black text-slate-700">
                        <div id="input-hour-box" class="flex items-center gap-1 cursor-pointer" onclick="setActiveFocus('hour')">
                            <input type="text" id="ans-hour" readonly class="w-16 h-14 bg-white border-3 border-amber-400 rounded-xl text-center text-3xl font-black text-amber-600 focus:outline-none shadow-inner cursor-pointer" placeholder="?">
                            <span class="text-base font-bold text-slate-600">じ</span>
                        </div>

                        <div id="input-min-box" class="flex items-center gap-1 cursor-pointer" onclick="setActiveFocus('min')">
                            <input type="text" id="ans-min" readonly class="w-16 h-14 bg-white border-3 border-amber-400 rounded-xl text-center text-3xl font-black text-amber-600 focus:outline-none shadow-inner cursor-pointer" placeholder="?">
                            <span id="label-min" class="text-base font-bold text-slate-600">ふん</span>
                        </div>

                        <span id="label-extra" class="text-base font-bold text-slate-600 hidden">かん</span>
                    </div>

                    <!-- テンキー (0~9) -->
                    <div class="w-full max-w-xs grid grid-cols-5 gap-2 pt-1">
                        <button onclick="pressNum(1)" class="pop-btn bg-white hover:bg-amber-100 border-2 border-slate-200 border-b-4 rounded-xl py-2.5 text-xl font-black text-slate-700 shadow-sm">1</button>
                        <button onclick="pressNum(2)" class="pop-btn bg-white hover:bg-amber-100 border-2 border-slate-200 border-b-4 rounded-xl py-2.5 text-xl font-black text-slate-700 shadow-sm">2</button>
                        <button onclick="pressNum(3)" class="pop-btn bg-white hover:bg-amber-100 border-2 border-slate-200 border-b-4 rounded-xl py-2.5 text-xl font-black text-slate-700 shadow-sm">3</button>
                        <button onclick="pressNum(4)" class="pop-btn bg-white hover:bg-amber-100 border-2 border-slate-200 border-b-4 rounded-xl py-2.5 text-xl font-black text-slate-700 shadow-sm">4</button>
                        <button onclick="pressNum(5)" class="pop-btn bg-white hover:bg-amber-100 border-2 border-slate-200 border-b-4 rounded-xl py-2.5 text-xl font-black text-slate-700 shadow-sm">5</button>
                        
                        <button onclick="pressNum(6)" class="pop-btn bg-white hover:bg-amber-100 border-2 border-slate-200 border-b-4 rounded-xl py-2.5 text-xl font-black text-slate-700 shadow-sm">6</button>
                        <button onclick="pressNum(7)" class="pop-btn bg-white hover:bg-amber-100 border-2 border-slate-200 border-b-4 rounded-xl py-2.5 text-xl font-black text-slate-700 shadow-sm">7</button>
                        <button onclick="pressNum(8)" class="pop-btn bg-white hover:bg-amber-100 border-2 border-slate-200 border-b-4 rounded-xl py-2.5 text-xl font-black text-slate-700 shadow-sm">8</button>
                        <button onclick="pressNum(9)" class="pop-btn bg-white hover:bg-amber-100 border-2 border-slate-200 border-b-4 rounded-xl py-2.5 text-xl font-black text-slate-700 shadow-sm">9</button>
                        <button onclick="pressNum(0)" class="pop-btn bg-white hover:bg-amber-100 border-2 border-slate-200 border-b-4 rounded-xl py-2.5 text-xl font-black text-slate-700 shadow-sm">0</button>
                    </div>

                    <div class="flex gap-2 w-full max-w-xs pt-1">
                        <button onclick="clearInput()" class="pop-btn flex-1 bg-slate-200 hover:bg-slate-300 text-slate-700 border-b-4 border-slate-400 py-2 rounded-xl font-bold text-xs md:text-sm">
                            <i class="fa-solid fa-delete-left mr-1"></i>けす
                        </button>
                        <button onclick="switchInputFocus()" id="btn-switch-focus" class="pop-btn flex-1 bg-amber-200 hover:bg-amber-300 text-amber-800 border-b-4 border-amber-400 py-2 rounded-xl font-bold text-xs md:text-sm">
                            「ふん」へ
                        </button>
                    </div>
                </div>

                <button onclick="checkAnswer()" class="pop-btn w-full bg-emerald-500 hover:bg-emerald-600 text-white font-black text-xl md:text-2xl py-3.5 rounded-2xl border-b-8 border-emerald-700 shadow-lg tracking-wider">
                    こたえる！
                </button>
            </div>
        </div>

        <!-- 3. 結果画面 -->
        <div id="screen-result" class="hidden text-center space-y-6 animate-celebrate">
            <div class="bg-white p-8 rounded-3xl border-4 border-amber-300 shadow-xl space-y-4">
                <div id="result-icon" class="text-6xl text-amber-400 animate-bounce-gentle">
                    <i class="fa-solid fa-crown"></i>
                </div>
                <h2 class="text-3xl font-black text-slate-800">けっか はっぴょう！</h2>
                
                <div class="bg-amber-50 p-4 rounded-2xl border-2 border-amber-200 inline-block px-8">
                    <p class="text-sm font-bold text-amber-800">せいかいした すう</p>
                    <p class="text-5xl font-black text-amber-500 my-1"><span id="result-score">5</span> / 5</p>
                </div>

                <p id="result-message" class="text-lg font-bold text-slate-600">すごい！ぜんぶ せいかい！てんさいだね！</p>

                <div class="flex justify-center items-center gap-2 text-2xl text-amber-400 py-2" id="result-stars">
                    <i class="fa-solid fa-star"></i>
                    <i class="fa-solid fa-star"></i>
                    <i class="fa-solid fa-star"></i>
                </div>

                <div class="flex gap-4 pt-2">
                    <button onclick="showMenu()" class="pop-btn flex-1 bg-slate-200 hover:bg-slate-300 text-slate-700 font-bold py-3 rounded-2xl border-b-4 border-slate-400">
                        メニューへ
                    </button>
                    <button onclick="retryQuiz()" class="pop-btn flex-1 bg-emerald-500 hover:bg-emerald-600 text-white font-black py-3 rounded-2xl border-b-4 border-emerald-700">
                        もういちど！
                    </button>
                </div>
            </div>
        </div>

        <!-- 4. スタンプ帳画面 -->
        <div id="screen-stamps" class="hidden space-y-6 animate-celebrate">
            <div class="bg-white p-6 rounded-3xl border-4 border-rose-300 shadow-xl space-y-4 text-center">
                <h2 class="text-2xl font-black text-slate-800"><i class="fa-solid fa-star text-amber-400 mr-2"></i>ごほうび スタンプカード</h2>
                <p class="text-xs text-slate-500 font-bold">10こ あつめると いいことが あるかも！？</p>

                <div id="stamp-grid" class="grid grid-cols-5 gap-2.5 p-4 bg-rose-50 rounded-2xl border-2 border-rose-200">
                    <!-- スタンプ枠 -->
                </div>

                <button onclick="showMenu()" class="pop-btn w-full bg-slate-200 hover:bg-slate-300 text-slate-700 font-bold py-3 rounded-2xl border-b-4 border-slate-400">
                    もどる
                </button>
            </div>
        </div>

    </main>

    <div id="feedback-modal" class="fixed inset-0 bg-black/40 backdrop-blur-sm flex items-center justify-center p-4 z-50 hidden">
        <div id="feedback-card" class="bg-white rounded-3xl p-6 max-w-xs w-full text-center border-4 shadow-2xl space-y-4 animate-celebrate">
            <div id="feedback-icon" class="text-6xl"></div>
            <h3 id="feedback-title" class="text-2xl font-black">せいかい！</h3>
            <p id="feedback-text" class="text-sm font-bold text-slate-600">そのちょうし！</p>
            
            <button onclick="nextQuestion()" class="pop-btn w-full bg-emerald-500 hover:bg-emerald-600 text-white font-black py-3 rounded-2xl border-b-4 border-emerald-700">
                つぎへ！
            </button>
        </div>
    </div>

    <!-- フッター -->
    <footer class="text-center py-4 text-xs text-slate-400 font-bold">
        とけいの れんしゅう アプリ &copy; しょうがく２ねんせい さんすう
    </footer>

    <script>
        // Web Audio API による合成音システム
        let audioCtx = null;
        let soundEnabled = true;

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
        }

        function playSound(type) {
            if (!soundEnabled) return;
            initAudio();
            if (!audioCtx) return;

            const now = audioCtx.currentTime;

            if (type === 'correct') {
                // 正解音
                const osc1 = audioCtx.createOscillator();
                const gain1 = audioCtx.createGain();
                osc1.type = 'sine';
                osc1.frequency.setValueAtTime(659.25, now);
                osc1.frequency.setValueAtTime(880, now + 0.15);
                gain1.gain.setValueAtTime(0.3, now);
                gain1.gain.exponentialRampToValueAtTime(0.01, now + 0.5);
                osc1.connect(gain1);
                gain1.connect(audioCtx.destination);
                osc1.start(now);
                osc1.stop(now + 0.5);
            } else if (type === 'wrong') {
                // 不正解音
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(160, now);
                osc.frequency.setValueAtTime(120, now + 0.15);
                gain.gain.setValueAtTime(0.3, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.4);
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start(now);
                osc.stop(now + 0.4);
            } else if (type === 'click') {
                // タップ音
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(440, now);
                gain.gain.setValueAtTime(0.08, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.05);
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start(now);
                osc.stop(now + 0.05);
            } else if (type === 'fanfare') {
                // クリアファンファーレ
                const notes = [523.25, 659.25, 783.99, 1046.50];
                notes.forEach((freq, idx) => {
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(freq, now + idx * 0.12);
                    gain.gain.setValueAtTime(0.25, now + idx * 0.12);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + idx * 0.12 + 0.35);
                    osc.connect(gain);
                    gain.connect(audioCtx.destination);
                    osc.start(now + idx * 0.12);
                    osc.stop(now + idx * 0.12 + 0.35);
                });
            }
        }

        function toggleSound() {
            soundEnabled = !soundEnabled;
            const icon = document.getElementById('sound-icon');
            if (soundEnabled) {
                icon.className = 'fa-solid fa-volume-high';
            } else {
                icon.className = 'fa-solid fa-volume-xmark text-slate-400';
            }
        }

        // アプリの状態
        let currentMode = 'after';
        let difficulty = 'easy';
        let questions = [];
        let currentQIndex = 0;
        let score = 0;
        let activeInputFocus = 'hour';
        let userStars = parseInt(localStorage.getItem('clock_stars') || '0');

        document.getElementById('header-stars').innerText = userStars;

        function setDifficulty(diff) {
            playSound('click');
            difficulty = diff;
            ['easy', 'normal', 'hard'].forEach(d => {
                const btn = document.getElementById(`diff-${d}`);
                if (d === diff) {
                    btn.className = "px-3 py-1.5 rounded-xl border-2 font-bold text-xs md:text-sm bg-emerald-500 text-white border-emerald-600 shadow";
                } else {
                    btn.className = "px-3 py-1.5 rounded-xl border-2 font-bold text-xs md:text-sm bg-slate-100 text-slate-600 border-slate-200";
                }
            });
        }

        function drawClock(svgId, hour, minute) {
            const svg = document.getElementById(svgId);
            svg.innerHTML = '';

            const bgCircle = document.createElementNS("http://www.w3.org/2000/svg", "circle");
            bgCircle.setAttribute("cx", "100");
            bgCircle.setAttribute("cy", "100");
            bgCircle.setAttribute("r", "92");
            bgCircle.setAttribute("fill", "#FFFFFF");
            bgCircle.setAttribute("stroke", "#F59E0B");
            bgCircle.setAttribute("stroke-width", "8");
            svg.appendChild(bgCircle);

            // 数字と目盛り
            for (let i = 1; i <= 12; i++) {
                const angle = (i * 30 - 90) * (Math.PI / 180);
                const x = 100 + 71 * Math.cos(angle);
                const y = 100 + 71 * Math.sin(angle);

                const text = document.createElementNS("http://www.w3.org/2000/svg", "text");
                text.setAttribute("x", x);
                text.setAttribute("y", y + 6);
                text.setAttribute("text-anchor", "middle");
                text.setAttribute("font-size", "18");
                text.setAttribute("font-weight", "900");
                text.setAttribute("fill", "#334155");
                text.textContent = i;
                svg.appendChild(text);

                const dotX = 100 + 84 * Math.cos(angle);
                const dotY = 100 + 84 * Math.sin(angle);
                const dot = document.createElementNS("http://www.w3.org/2000/svg", "circle");
                dot.setAttribute("cx", dotX);
                dot.setAttribute("cy", dotY);
                dot.setAttribute("r", "2.5");
                dot.setAttribute("fill", "#94A3B8");
                svg.appendChild(dot);
            }

            for (let i = 0; i < 60; i++) {
                if (i % 5 !== 0) {
                    const angle = (i * 6 - 90) * (Math.PI / 180);
                    const dotX = 100 + 84 * Math.cos(angle);
                    const dotY = 100 + 84 * Math.sin(angle);
                    const dot = document.createElementNS("http://www.w3.org/2000/svg", "circle");
                    dot.setAttribute("cx", dotX);
                    dot.setAttribute("cy", dotY);
                    dot.setAttribute("r", "1");
                    dot.setAttribute("fill", "#CBD5E1");
                    svg.appendChild(dot);
                }
            }

            const minAngle = minute * 6;
            const hourAngle = ((hour % 12) * 30) + (minute * 0.5);

            // 短針 (Red)
            const hourRad = (hourAngle - 90) * (Math.PI / 180);
            const hx = 100 + 48 * Math.cos(hourRad);
            const hy = 100 + 48 * Math.sin(hourRad);
            const hourLine = document.createElementNS("http://www.w3.org/2000/svg", "line");
            hourLine.setAttribute("x1", "100");
            hourLine.setAttribute("y1", "100");
            hourLine.setAttribute("x2", hx);
            hourLine.setAttribute("y2", hy);
            hourLine.setAttribute("stroke", "#EF4444");
            hourLine.setAttribute("stroke-width", "7");
            hourLine.setAttribute("stroke-linecap", "round");
            svg.appendChild(hourLine);

            // 長針 (Blue)
            const minRad = (minAngle - 90) * (Math.PI / 180);
            const mx = 100 + 70 * Math.cos(minRad);
            const my = 100 + 70 * Math.sin(minRad);
            const minLine = document.createElementNS("http://www.w3.org/2000/svg", "line");
            minLine.setAttribute("x1", "100");
            minLine.setAttribute("y1", "100");
            minLine.setAttribute("x2", mx);
            minLine.setAttribute("y2", my);
            minLine.setAttribute("stroke", "#2563EB");
            minLine.setAttribute("stroke-width", "5");
            minLine.setAttribute("stroke-linecap", "round");
            svg.appendChild(minLine);

            const center = document.createElementNS("http://www.w3.org/2000/svg", "circle");
            center.setAttribute("cx", "100");
            center.setAttribute("cy", "100");
            center.setAttribute("r", "6");
            center.setAttribute("fill", "#F59E0B");
            svg.appendChild(center);
        }

        function generateQuestions(mode, count = 5) {
            const qList = [];
            
            for (let i = 0; i < count; i++) {
                let qType = mode;
                if (mode === 'mix') {
                    const types = ['after', 'before', 'between'];
                    qType = types[Math.floor(Math.random() * types.length)];
                }

                let step = 10;
                if (difficulty === 'normal') step = 5;
                if (difficulty === 'hard') step = 1;

                let h1 = Math.floor(Math.random() * 12) + 1;
                let m1 = Math.floor(Math.random() * (60 / step)) * step;

                if (qType === 'after') {
                    let addMin = (Math.floor(Math.random() * 5) + 1) * (difficulty === 'hard' ? 5 : step); 
                    if (addMin === 0) addMin = 10;

                    let totalMin = m1 + addMin;
                    let ansH = h1 + Math.floor(totalMin / 60);
                    let ansM = totalMin % 60;
                    if (ansH > 12) ansH -= 12;

                    qList.push({
                        type: 'after',
                        h1: h1,
                        m1: m1,
                        diffMin: addMin,
                        ansH: ansH,
                        ansM: ansM,
                        text: `<span class="text-rose-500">${h1}じ ${m1}ふん</span> の <span class="text-sky-600 font-black">${addMin}ふんご</span> の じかんは？`
                    });

                } else if (qType === 'before') {
                    let subMin = (Math.floor(Math.random() * 5) + 1) * (difficulty === 'hard' ? 5 : step);
                    if (subMin === 0) subMin = 10;

                    let totalMin = (h1 * 60 + m1) - subMin;
                    if (totalMin <= 0) totalMin += 12 * 60;

                    let ansH = Math.floor(totalMin / 60);
                    let ansM = totalMin % 60;
                    if (ansH === 0) ansH = 12;
                    if (ansH > 12) ansH %= 12;

                    qList.push({
                        type: 'before',
                        h1: h1,
                        m1: m1,
                        diffMin: subMin,
                        ansH: ansH,
                        ansM: ansM,
                        text: `<span class="text-rose-500">${h1}じ ${m1}ふん</span> の <span class="text-indigo-600 font-black">${subMin}ふんまえ</span> の じかんは？`
                    });

                } else if (qType === 'between') {
                    let diffMin = (Math.floor(Math.random() * 5) + 1) * (difficulty === 'hard' ? 5 : step);
                    if (diffMin === 0) diffMin = 15;

                    let totalMin2 = (h1 * 60 + m1) + diffMin;
                    let h2 = Math.floor(totalMin2 / 60);
                    let m2 = totalMin2 % 60;
                    if (h2 > 12) h2 -= 12;

                    qList.push({
                        type: 'between',
                        h1: h1,
                        m1: m1,
                        h2: h2,
                        m2: m2,
                        ansMin: diffMin,
                        text: `<span class="text-rose-500">${h1}じ ${m1}ふん</span> から <span class="text-emerald-600">${h2}じ ${m2}ふん</span> まで は <span class="text-amber-600 font-black">なんぷんかん？</span>`
                    });
                }
            }

            return qList;
        }

        function showScreen(screenId) {
            ['screen-menu', 'screen-quiz', 'screen-result', 'screen-stamps'].forEach(id => {
                document.getElementById(id).classList.add('hidden');
            });
            document.getElementById(screenId).classList.remove('hidden');
        }

        function showMenu() {
            playSound('click');
            showScreen('screen-menu');
        }

        function showStamps() {
            playSound('click');
            renderStamps();
            showScreen('screen-stamps');
        }

        function startQuiz(mode) {
            playSound('click');
            currentMode = mode;
            questions = generateQuestions(mode, 5);
            currentQIndex = 0;
            score = 0;
            showScreen('screen-quiz');
            loadQuestion();
        }

        function retryQuiz() {
            startQuiz(currentMode);
        }

        function loadQuestion() {
            const q = questions[currentQIndex];
            
            const progress = (currentQIndex / questions.length) * 100;
            document.getElementById('progress-bar').style.width = `${progress}%`;
            document.getElementById('q-index').innerText = currentQIndex + 1;

            document.getElementById('question-text').innerHTML = q.text;

            const badge = document.getElementById('question-badge');
            const clock2Wrapper = document.getElementById('clock2-wrapper');
            const inputHourBox = document.getElementById('input-hour-box');
            const labelExtra = document.getElementById('label-extra');
            const labelMin = document.getElementById('label-min');

            if (q.type === 'after') {
                badge.innerText = "◯ふんごの じかん";
                badge.className = "bg-sky-100 text-sky-800 text-xs font-bold px-3 py-1 rounded-full";
                clock2Wrapper.classList.add('hidden');
                inputHourBox.classList.remove('hidden');
                labelExtra.classList.add('hidden');
                labelMin.innerText = "ふん";
                activeInputFocus = 'hour';
            } else if (q.type === 'before') {
                badge.innerText = "◯ふんまえの じかん";
                badge.className = "bg-indigo-100 text-indigo-800 text-xs font-bold px-3 py-1 rounded-full";
                clock2Wrapper.classList.add('hidden');
                inputHourBox.classList.remove('hidden');
                labelExtra.classList.add('hidden');
                labelMin.innerText = "ふん";
                activeInputFocus = 'hour';
            } else if (q.type === 'between') {
                badge.innerText = "あいだの じかん";
                badge.className = "bg-emerald-100 text-emerald-800 text-xs font-bold px-3 py-1 rounded-full";
                clock2Wrapper.classList.remove('hidden');
                clock2Wrapper.classList.add('flex');
                inputHourBox.classList.add('hidden');
                labelExtra.classList.remove('hidden');
                labelMin.innerText = "ふん";
                activeInputFocus = 'min';
            }

            drawClock('svg-clock-1', q.h1, q.m1);
            if (q.type === 'between') {
                drawClock('svg-clock-2', q.h2, q.m2);
            }

            clearInput();
            updateFocusStyle();
        }

        function setActiveFocus(type) {
            playSound('click');
            const q = questions[currentQIndex];
            if (q.type === 'between' && type === 'hour') return;
            activeInputFocus = type;
            updateFocusStyle();
        }

        function pressNum(num) {
            playSound('click');
            const q = questions[currentQIndex];

            if (q.type === 'between') {
                const minInput = document.getElementById('ans-min');
                if (minInput.value.length < 3) {
                    minInput.value = minInput.value === '' ? String(num) : minInput.value + num;
                }
            } else {
                if (activeInputFocus === 'hour') {
                    const hourInput = document.getElementById('ans-hour');
                    if (hourInput.value.length < 2) {
                        hourInput.value = hourInput.value === '' ? String(num) : hourInput.value + num;
                    }
                    if (hourInput.value.length === 2 || parseInt(hourInput.value) > 1) {
                        activeInputFocus = 'min';
                        updateFocusStyle();
                    }
                } else {
                    const minInput = document.getElementById('ans-min');
                    if (minInput.value.length < 2) {
                        minInput.value = minInput.value === '' ? String(num) : minInput.value + num;
                    }
                }
            }
        }

        function clearInput() {
            playSound('click');
            document.getElementById('ans-hour').value = '';
            document.getElementById('ans-min').value = '';
            const q = questions[currentQIndex];
            if (q && q.type !== 'between') {
                activeInputFocus = 'hour';
            } else {
                activeInputFocus = 'min';
            }
            updateFocusStyle();
        }

        function switchInputFocus() {
            playSound('click');
            const q = questions[currentQIndex];
            if (q.type === 'between') return;

            activeInputFocus = (activeInputFocus === 'hour') ? 'min' : 'hour';
            updateFocusStyle();
        }

        function updateFocusStyle() {
            const hInput = document.getElementById('ans-hour');
            const mInput = document.getElementById('ans-min');
            const switchBtn = document.getElementById('btn-switch-focus');

            if (activeInputFocus === 'hour') {
                hInput.classList.add('ring-4', 'ring-amber-400');
                mInput.classList.remove('ring-4', 'ring-amber-400');
                switchBtn.innerText = "「ふん」へ";
            } else {
                mInput.classList.add('ring-4', 'ring-amber-400');
                hInput.classList.remove('ring-4', 'ring-amber-400');
                switchBtn.innerText = "「じ」へ";
            }
        }

        function checkAnswer() {
            const q = questions[currentQIndex];
            const hVal = parseInt(document.getElementById('ans-hour').value || '0');
            const mVal = parseInt(document.getElementById('ans-min').value || '0');

            let isCorrect = false;

            if (q.type === 'between') {
                if (mVal === q.ansMin) isCorrect = true;
            } else {
                if (hVal === q.ansH && mVal === q.ansM) isCorrect = true;
            }

            const modal = document.getElementById('feedback-modal');
            const card = document.getElementById('feedback-card');
            const icon = document.getElementById('feedback-icon');
            const title = document.getElementById('feedback-title');
            const text = document.getElementById('feedback-text');

            if (isCorrect) {
                playSound('correct');
                score++;
                card.className = "bg-white rounded-3xl p-6 max-w-xs w-full text-center border-4 border-emerald-400 shadow-2xl space-y-4 animate-celebrate";
                icon.innerHTML = '<i class="fa-solid fa-circle-check text-emerald-500"></i>';
                title.innerText = "だいせいかい！";
                title.className = "text-2xl font-black text-emerald-600";
                text.innerText = "すばらしい！ぴったり 正解だよ！";
            } else {
                playSound('wrong');
                card.className = "bg-white rounded-3xl p-6 max-w-xs w-full text-center border-4 border-rose-400 shadow-2xl space-y-4 animate-celebrate";
                icon.innerHTML = '<i class="fa-solid fa-circle-xmark text-rose-500"></i>';
                title.innerText = "ざんねん！";
                title.className = "text-2xl font-black text-rose-600";

                if (q.type === 'between') {
                    text.innerText = `こたえは 「${q.ansMin}ふんかん」 だよ！`;
                } else {
                    text.innerText = `こたえは 「${q.ansH}じ ${q.ansM}ふん」 だよ！`;
                }
            }

            modal.classList.remove('hidden');
        }

        function nextQuestion() {
            playSound('click');
            document.getElementById('feedback-modal').classList.add('hidden');
            currentQIndex++;

            if (currentQIndex < questions.length) {
                loadQuestion();
            } else {
                showResult();
            }
        }

        function showResult() {
            document.getElementById('result-score').innerText = score;
            const msg = document.getElementById('result-message');
            const icon = document.getElementById('result-icon');

            if (score === 5) {
                playSound('fanfare');
                icon.innerHTML = '<i class="fa-solid fa-crown text-amber-400"></i>';
                msg.innerText = "パーフェクト！きみは とけいマスターだ！";
                addStars(3);
            } else if (score >= 3) {
                playSound('correct');
                icon.innerHTML = '<i class="fa-solid fa-medal text-sky-400"></i>';
                msg.innerText = "お見事！そのちょうしで がんばろう！";
                addStars(2);
            } else {
                playSound('click');
                icon.innerHTML = '<i class="fa-solid fa-thumbs-up text-emerald-400"></i>';
                msg.innerText = "よくがんばったね！なんども れんしゅうしよう！";
                addStars(1);
            }

            showScreen('screen-result');
        }

        function addStars(num) {
            userStars += num;
            localStorage.setItem('clock_stars', userStars);
            document.getElementById('header-stars').innerText = userStars;
        }

        function renderStamps() {
            const grid = document.getElementById('stamp-grid');
            grid.innerHTML = '';

            const totalStamps = Math.min(userStars, 10);

            for (let i = 1; i <= 10; i++) {
                const box = document.createElement('div');

                if (i <= totalStamps) {
                    box.className = "w-full aspect-square rounded-xl border-2 border-rose-400 bg-rose-100 flex items-center justify-center relative shadow-sm";
                    box.innerHTML = `
                        <div class="text-rose-500 text-2xl md:text-3xl animate-celebrate">
                            <i class="fa-solid fa-stamp"></i>
                        </div>
                    `;
                } else {
                    box.className = "w-full aspect-square rounded-xl border-2 border-dashed border-rose-300 bg-white flex items-center justify-center relative";
                    box.innerHTML = `<span class="text-xs font-bold text-rose-300">${i}</span>`;
                }

                grid.appendChild(box);
            }
        }

        window.onload = function() {
            showMenu();
        };
    </script>
</body>
</html>

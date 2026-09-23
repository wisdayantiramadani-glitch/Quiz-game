```html
<!DOCTYPE html>
<html lang="id" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI CodeMaster Quiz Arena - 35 Soal Coding AI</title>
  
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            brand: {
              50: '#f0f6ff',
              100: '#e0edff',
              500: '#3b82f6',
              600: '#2563eb',
              700: '#1d4ed8',
              900: '#1e3a8a',
            },
            cyber: {
              dark: '#0f172a',
              card: '#1e293b',
              border: '#334155',
              accent: '#06b6d4',
              neon: '#a855f7',
              success: '#10b981',
              danger: '#ef4444'
            }
          },
          fontFamily: {
            sans: ['Inter', 'sans-serif'],
            mono: ['Fira Code', 'Monaco', 'monospace']
          }
        }
      }
    }
  </script>

  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;600&family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
  
  <!-- Libraries: Tone.js for Web Audio Synth & Canvas Confetti -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.49/Tone.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
  
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

  <style>
    body {
      font-family: 'Inter', sans-serif;
      background-color: #090d16;
      color: #f1f5f9;
      background-image: 
        radial-gradient(at 0% 0%, rgba(59, 130, 246, 0.12) 0px, transparent 50%),
        radial-gradient(at 100% 100%, rgba(168, 85, 247, 0.12) 0px, transparent 50%);
      background-attachment: fixed;
    }

    .glass-card {
      background: rgba(30, 41, 59, 0.7);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border: 1px solid rgba(255, 255, 255, 0.08);
    }

    .glass-nav {
      background: rgba(15, 23, 42, 0.85);
      backdrop-filter: blur(16px);
      border-bottom: 1px solid rgba(255, 255, 255, 0.08);
    }

    .glow-effect {
      box-shadow: 0 0 25px -5px rgba(59, 130, 246, 0.3);
    }

    .glow-success {
      box-shadow: 0 0 20px -3px rgba(16, 185, 129, 0.4);
    }

    .glow-danger {
      box-shadow: 0 0 20px -3px rgba(239, 68, 68, 0.4);
    }

    .perspective-1000 {
      perspective: 1000px;
    }

    .transform-style-3d {
      transform-style: preserve-3d;
    }

    .backface-hidden {
      backface-visibility: hidden;
    }

    .rotate-y-180 {
      transform: rotateY(180deg);
    }

    @keyframes pulse-subtle {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.85; transform: scale(0.98); }
    }

    .animate-pulse-subtle {
      animation: pulse-subtle 3s infinite ease-in-out;
    }
  </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-brand-500 selection:text-white">

  <!-- Top Navigation Header -->
  <header class="sticky top-0 z-50 glass-nav px-4 lg:px-8 py-3">
    <div class="max-w-6xl mx-auto flex items-center justify-between">
      <div class="flex items-center gap-3 cursor-pointer" onclick="app.navTo('home')">
        <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-brand-600 to-cyber-neon flex items-center justify-center text-white font-bold text-lg shadow-lg">
          <i class="fa-solid fa-brain"></i>
        </div>
        <div>
          <h1 class="font-bold text-lg tracking-tight bg-gradient-to-r from-white via-slate-200 to-brand-500 bg-clip-text text-transparent">
            AI CodeMaster
          </h1>
          <p class="text-xs text-slate-400 font-mono">35 Soal Coding AI Arena</p>
        </div>
      </div>

      <!-- Navigation Tabs -->
      <nav class="hidden md:flex items-center gap-1 bg-slate-900/60 p-1 rounded-xl border border-slate-800">
        <button onclick="app.navTo('home')" id="nav-home" class="px-4 py-1.5 rounded-lg text-sm font-medium transition text-brand-500 bg-slate-800">
          <i class="fa-solid fa-gamepad mr-1.5"></i> Play Quiz
        </button>
        <button onclick="app.navTo('flashcard')" id="nav-flashcard" class="px-4 py-1.5 rounded-lg text-sm font-medium transition text-slate-400 hover:text-white">
          <i class="fa-solid fa-layer-group mr-1.5"></i> Flashcard
        </button>
        <button onclick="app.navTo('bank')" id="nav-bank" class="px-4 py-1.5 rounded-lg text-sm font-medium transition text-slate-400 hover:text-white">
          <i class="fa-solid fa-book-bookmark mr-1.5"></i> Bank Soal
        </button>
      </nav>

      <!-- Action Utilities (Sound toggle, Stats) -->
      <div class="flex items-center gap-3">
        <button id="sound-btn" onclick="app.toggleSound()" class="w-10 h-10 rounded-xl bg-slate-800/80 hover:bg-slate-700 text-slate-300 hover:text-white flex items-center justify-center transition border border-slate-700/50" title="Toggle Sound">
          <i class="fa-solid fa-volume-high text-sm"></i>
        </button>
        <div class="hidden sm:flex items-center gap-2 bg-slate-800/50 px-3 py-1.5 rounded-xl border border-slate-700/50 text-xs font-mono">
          <i class="fa-solid fa-trophy text-amber-400"></i>
          <span class="text-slate-400">High Score:</span>
          <span id="top-score-display" class="font-bold text-emerald-400">0</span>
        </div>
      </div>
    </div>
  </header>

  <!-- Main Application Container -->
  <main class="flex-grow max-w-5xl w-full mx-auto p-4 sm:p-6 lg:p-8">

    <!-- ==================== HOME VIEW (SETUP & START) ==================== -->
    <section id="view-home" class="space-y-8 animate-fadeIn">
      <!-- Hero Banner -->
      <div class="relative overflow-hidden glass-card rounded-3xl p-6 sm:p-10 border border-slate-700/50 glow-effect">
        <div class="absolute -right-10 -bottom-10 w-64 h-64 bg-brand-600/10 rounded-full blur-3xl pointer-events-none"></div>
        <div class="absolute -left-10 -top-10 w-64 h-64 bg-cyber-neon/10 rounded-full blur-3xl pointer-events-none"></div>
        
        <div class="relative z-10 max-w-2xl space-y-4">
          <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-brand-500/10 border border-brand-500/20 text-brand-400 text-xs font-semibold uppercase tracking-wider">
            <i class="fa-solid fa-wand-magic-sparkles"></i> Latihan Interaktif Coding AI
          </div>
          <h2 class="text-3xl sm:text-5xl font-extrabold tracking-tight text-white leading-tight">
            Uji Pemahaman <span class="bg-gradient-to-r from-brand-400 via-cyber-accent to-cyber-neon bg-clip-text text-transparent">Coding & AI</span> Kamu!
          </h2>
          <p class="text-slate-300 text-sm sm:text-base leading-relaxed">
            35 Soal Pilihan Ganda seputar Dasar Pemrograman Python, Machine Learning, Deep Learning, NLP, Computer Vision, hingga AI Generatif.
          </p>
        </div>
      </div>

      <!-- Quiz Mode Setup Cards -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        
        <!-- Mode Option 1: Quick Quiz -->
        <div onclick="app.startQuiz(10)" class="glass-card rounded-2xl p-6 border border-slate-800 hover:border-brand-500/50 transition cursor-pointer group hover:-translate-y-1 duration-300 flex flex-col justify-between">
          <div class="space-y-4">
            <div class="w-12 h-12 rounded-xl bg-brand-500/10 border border-brand-500/30 text-brand-400 flex items-center justify-center text-xl group-hover:scale-110 transition">
              <i class="fa-solid fa-bolt"></i>
            </div>
            <div>
              <h3 class="text-lg font-bold text-white group-hover:text-brand-400 transition">Kuis Cepat</h3>
              <p class="text-xs text-slate-400 mt-1">10 Soal acak pilihan. Cocok untuk pemanasan & latihan kilat.</p>
            </div>
          </div>
          <div class="mt-6 flex items-center justify-between text-xs font-mono text-slate-400 border-t border-slate-800 pt-4">
            <span>⏱ ~5 Menit</span>
            <span class="text-brand-400 font-bold group-hover:translate-x-1 transition flex items-center gap-1">
              Mulai <i class="fa-solid fa-arrow-right"></i>
            </span>
          </div>
        </div>

        <!-- Mode Option 2: Standard Quiz -->
        <div onclick="app.startQuiz(20)" class="glass-card rounded-2xl p-6 border border-slate-800 hover:border-cyber-accent/50 transition cursor-pointer group hover:-translate-y-1 duration-300 flex flex-col justify-between">
          <div class="space-y-4">
            <div class="w-12 h-12 rounded-xl bg-cyber-accent/10 border border-cyber-accent/30 text-cyber-accent flex items-center justify-center text-xl group-hover:scale-110 transition">
              <i class="fa-solid fa-fire"></i>
            </div>
            <div>
              <h3 class="text-lg font-bold text-white group-hover:text-cyber-accent transition">Kuis Standar</h3>
              <p class="text-xs text-slate-400 mt-1">20 Soal komprehensif. Evaluasi pemahaman menengah.</p>
            </div>
          </div>
          <div class="mt-6 flex items-center justify-between text-xs font-mono text-slate-400 border-t border-slate-800 pt-4">
            <span>⏱ ~10 Menit</span>
            <span class="text-cyber-accent font-bold group-hover:translate-x-1 transition flex items-center gap-1">
              Mulai <i class="fa-solid fa-arrow-right"></i>
            </span>
          </div>
        </div>

        <!-- Mode Option 3: Full Challenge -->
        <div onclick="app.startQuiz(35)" class="glass-card rounded-2xl p-6 border border-slate-800 hover:border-cyber-neon/50 transition cursor-pointer group hover:-translate-y-1 duration-300 flex flex-col justify-between relative overflow-hidden">
          <div class="absolute top-2 right-2 bg-cyber-neon/20 border border-cyber-neon/40 text-cyber-neon text-[10px] font-bold px-2 py-0.5 rounded-full">
            FULL ARENA
          </div>
          <div class="space-y-4">
            <div class="w-12 h-12 rounded-xl bg-cyber-neon/10 border border-cyber-neon/30 text-cyber-neon flex items-center justify-center text-xl group-hover:scale-110 transition">
              <i class="fa-solid fa-trophy"></i>
            </div>
            <div>
              <h3 class="text-lg font-bold text-white group-hover:text-cyber-neon transition">Tantangan Lengkap</h3>
              <p class="text-xs text-slate-400 mt-1">Semua 35 Soal AI. Uji kemampuan maksimal kamu!</p>
            </div>
          </div>
          <div class="mt-6 flex items-center justify-between text-xs font-mono text-slate-400 border-t border-slate-800 pt-4">
            <span>⏱ ~15-20 Menit</span>
            <span class="text-cyber-neon font-bold group-hover:translate-x-1 transition flex items-center gap-1">
              Mulai <i class="fa-solid fa-arrow-right"></i>
            </span>
          </div>
        </div>

      </div>

      <!-- Settings & Mode Toggles -->
      <div class="glass-card rounded-2xl p-6 border border-slate-800 flex flex-col sm:flex-row items-center justify-between gap-4">
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 rounded-xl bg-slate-800 flex items-center justify-center text-slate-300">
            <i class="fa-solid fa-clock"></i>
          </div>
          <div>
            <h4 class="text-sm font-semibold text-white">Timer per Soal (30 Detik)</h4>
            <p class="text-xs text-slate-400">Aktifkan batas waktu untuk tantangan lebih seru</p>
          </div>
        </div>
        <label class="relative inline-flex items-center cursor-pointer">
          <input type="checkbox" id="timer-toggle" class="sr-only peer" checked>
          <div class="w-11 h-6 bg-slate-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-brand-600"></div>
        </label>
      </div>
    </section>

    <!-- ==================== QUIZ GAMEPLAY VIEW ==================== -->
    <section id="view-quiz" class="hidden space-y-6 animate-fadeIn">
      <!-- Quiz Progress Header -->
      <div class="glass-card rounded-2xl p-4 sm:p-6 border border-slate-800 space-y-4">
        <div class="flex items-center justify-between text-xs sm:text-sm font-mono text-slate-400">
          <span class="flex items-center gap-2">
            <i class="fa-solid fa-list-check text-brand-400"></i>
            Soal <span id="quiz-progress-text" class="text-white font-bold">1 / 35</span>
          </span>
          <span class="flex items-center gap-2">
            <i class="fa-solid fa-fire text-amber-500"></i>
            Streak: <span id="quiz-streak-count" class="text-amber-400 font-bold">0</span>
          </span>
          <span id="timer-container" class="flex items-center gap-2 font-bold text-emerald-400">
            <i class="fa-regular fa-clock"></i>
            <span id="quiz-timer">30s</span>
          </span>
        </div>

        <!-- Progress Bar -->
        <div class="w-full bg-slate-800 h-2.5 rounded-full overflow-hidden p-0.5">
          <div id="quiz-progress-bar" class="bg-gradient-to-r from-brand-500 to-cyber-neon h-full rounded-full transition-all duration-300" style="width: 0%"></div>
        </div>
      </div>

      <!-- Question Card -->
      <div class="glass-card rounded-3xl p-6 sm:p-8 border border-slate-800 space-y-6">
        <div class="space-y-3">
          <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-slate-800 border border-slate-700 text-xs text-slate-400 font-mono">
            <i class="fa-solid fa-tag text-brand-400"></i> <span id="question-category">Kategori AI</span>
          </div>
          <h3 id="question-text" class="text-xl sm:text-2xl font-bold text-white leading-relaxed">
            Loading Soal...
          </h3>
        </div>

        <!-- Options Container -->
        <div id="options-container" class="grid grid-cols-1 gap-3 sm:gap-4">
          <!-- Rendered dynamically -->
        </div>

        <!-- Explanation Drawer (Revealed after answer) -->
        <div id="explanation-box" class="hidden rounded-2xl p-5 border text-sm space-y-2 transition-all">
          <div class="flex items-center gap-2 font-bold" id="explanation-title">
            <i class="fa-solid fa-circle-info"></i> Pembahasan AI:
          </div>
          <p id="explanation-text" class="text-slate-300 leading-relaxed">
            Penjelasan akan muncul di sini.
          </p>
        </div>

        <!-- Next / Action Button Bar -->
        <div class="flex items-center justify-between pt-4 border-t border-slate-800">
          <button onclick="app.bookmarkCurrentQuestion()" id="bookmark-btn" class="text-xs sm:text-sm text-slate-400 hover:text-amber-400 transition flex items-center gap-2">
            <i class="fa-regular fa-bookmark"></i> Simpan Soal Ini
          </button>
          
          <button id="next-btn" onclick="app.nextQuestion()" disabled class="px-6 py-2.5 rounded-xl bg-slate-800 text-slate-500 font-semibold text-sm transition flex items-center gap-2 cursor-not-allowed">
            Soal Berikutnya <i class="fa-solid fa-arrow-right"></i>
          </button>
        </div>
      </div>
    </section>

    <!-- ==================== RESULTS VIEW ==================== -->
    <section id="view-result" class="hidden space-y-8 animate-fadeIn">
      <div class="glass-card rounded-3xl p-8 sm:p-12 text-center space-y-6 border border-slate-800 relative overflow-hidden">
        <div class="inline-flex p-4 rounded-3xl bg-slate-800/80 border border-slate-700 text-4xl text-amber-400 shadow-xl mb-2">
          <i id="result-badge-icon" class="fa-solid fa-trophy"></i>
        </div>

        <div class="space-y-2">
          <h2 class="text-3xl sm:text-4xl font-extrabold text-white" id="result-rank-title">Hasil Quiz AI Master</h2>
          <p class="text-slate-400 text-sm" id="result-subtitle">Kamu telah menyelesaikan tantangan!</p>
        </div>

        <!-- Stats Breakdown Cards -->
        <div class="grid grid-cols-2 sm:grid-cols-4 gap-4 max-w-2xl mx-auto py-4">
          <div class="bg-slate-900/80 p-4 rounded-2xl border border-slate-800">
            <p class="text-xs text-slate-400">Skor Akhir</p>
            <p id="res-score" class="text-2xl font-extrabold text-brand-400 font-mono">0</p>
          </div>
          <div class="bg-slate-900/80 p-4 rounded-2xl border border-slate-800">
            <p class="text-xs text-slate-400">Akurasi</p>
            <p id="res-accuracy" class="text-2xl font-extrabold text-emerald-400 font-mono">0%</p>
          </div>
          <div class="bg-slate-900/80 p-4 rounded-2xl border border-slate-800">
            <p class="text-xs text-slate-400">Benar</p>
            <p id="res-correct" class="text-2xl font-extrabold text-emerald-400 font-mono">0</p>
          </div>
          <div class="bg-slate-900/80 p-4 rounded-2xl border border-slate-800">
            <p class="text-xs text-slate-400">Salah</p>
            <p id="res-wrong" class="text-2xl font-extrabold text-rose-400 font-mono">0</p>
          </div>
        </div>

        <!-- Retry and Action Buttons -->
        <div class="flex flex-wrap items-center justify-center gap-4 pt-4">
          <button onclick="app.navTo('home')" class="px-6 py-3 rounded-xl bg-slate-800 hover:bg-slate-700 text-white font-semibold text-sm transition flex items-center gap-2">
            <i class="fa-solid fa-house"></i> Ke Menu Utama
          </button>
          <button onclick="app.reviewAnswers()" class="px-6 py-3 rounded-xl bg-brand-600 hover:bg-brand-500 text-white font-semibold text-sm transition flex items-center gap-2">
            <i class="fa-solid fa-magnifying-glass"></i> Review Pembahasan
          </button>
        </div>
      </div>

      <!-- Detailed Breakdown Section -->
      <div id="review-section" class="hidden glass-card rounded-3xl p-6 sm:p-8 border border-slate-800 space-y-6">
        <h3 class="text-xl font-bold text-white flex items-center gap-2">
          <i class="fa-solid fa-clipboard-check text-brand-400"></i> Detail Jawaban Kamu
        </h3>
        <div id="review-list" class="space-y-4">
          <!-- Rendered dynamically -->
        </div>
      </div>
    </section>

    <!-- ==================== FLASHCARD VIEW ==================== -->
    <section id="view-flashcard" class="hidden space-y-6 animate-fadeIn">
      <div class="flex items-center justify-between">
        <div>
          <h2 class="text-2xl font-bold text-white">Kartu Hafalan AI</h2>
          <p class="text-xs text-slate-400">Klik kartu untuk membalik dan melihat kunci jawaban beserta pembahasannya.</p>
        </div>
        <div class="text-xs font-mono text-slate-400">
          Kartu <span id="fc-current" class="text-white font-bold">1</span> / <span id="fc-total">35</span>
        </div>
      </div>

      <!-- Interactive 3D Flip Card -->
      <div class="perspective-1000 w-full min-h-[320px] sm:min-h-[380px] cursor-pointer" onclick="app.flipFlashcard()">
        <div id="flashcard-inner" class="relative w-full h-full transition-transform duration-500 transform-style-3d min-h-[320px] sm:min-h-[380px]">
          
          <!-- Front Side -->
          <div class="absolute inset-0 w-full h-full glass-card rounded-3xl p-8 border border-slate-800 flex flex-col justify-between backface-hi# Quiz-game

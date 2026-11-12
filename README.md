<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>Shabouquet — Coquette Digital Bouquet Maker</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Great+Vibes&family=Playfair+Display:ital,wght@0,500;0,600;0,700;1,400;1,600&family=Quicksand:wght@400;500;600;700&display=swap" rel="stylesheet">
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            coquette: {
              bg: '#FFF6F9',
              card: '#FFFDFD',
              blush: '#FCE4EC',
              pink: '#F48FB1',
              rose: '#D86C8B',
              deep: '#9E3B5A',
              cream: '#FDF8F2',
              lace: '#F3D8E1',
              text: '#5A3643',
              muted: '#8E6775'
            }
          },
          fontFamily: {
            script: ['"Great Vibes"', 'cursive'],
            serif: ['"Playfair Display"', 'serif'],
            sans: ['"Quicksand"', 'sans-serif'],
          },
          boxShadow: {
            'coquette': '0 10px 30px -5px rgba(216, 108, 139, 0.16)',
            'polaroid': '0 6px 18px rgba(90, 54, 67, 0.14)',
          }
        }
      }
    }
  </script>
  <style>
    html {
      font-size: 16px;
      scroll-behavior: smooth;
    }
    body {
      font-family: 'Quicksand', sans-serif;
      background-color: #FFF6F9;
      background-image: 
        radial-gradient(#F8BBD0 0.75px, transparent 0.75px),
        radial-gradient(#F8BBD0 0.75px, #FFF6F9 0.75px);
      background-size: 28px 28px;
      background-position: 0 0, 14px 14px;
      color: #5A3643;
      overflow-x: hidden;
      -webkit-tap-highlight-color: transparent;
    }

    /* Lace scalloped border effect */
    .lace-border {
      border: 2px dashed #F3B8CC;
      box-shadow: 0 0 0 4px #FFFDFD, 0 0 0 6px #FCE4EC;
    }

    .scallop-top {
      background-image: radial-gradient(circle at 12px 0, transparent 12px, #FFFDFD 13px);
      background-size: 24px 14px;
    }

    /* Floating petal animation */
    @keyframes floatSlow {
      0%, 100% { transform: translateY(0px) rotate(0deg); }
      50% { transform: translateY(-7px) rotate(2deg); }
    }
    .animate-float {
      animation: floatSlow 4.5s ease-in-out infinite;
    }

    @keyframes popIn {
      0% { transform: scale(0.4) rotate(-12deg); opacity: 0; }
      75% { transform: scale(1.08) rotate(3deg); opacity: 1; }
      100% { transform: scale(1) rotate(0deg); opacity: 1; }
    }
    .bloom-pop {
      animation: popIn 0.35s cubic-bezier(0.22, 1, 0.36, 1) forwards;
    }

    /* Custom scrollbar for horizontal flower tray */
    .no-scrollbar::-webkit-scrollbar {
      display: none;
    }
    .no-scrollbar {
      -ms-overflow-style: none;
      scrollbar-width: none;
    }

    /* Draggable bloom cursor */
    .bloom-item {
      touch-action: none;
      user-select: none;
      cursor: grab;
      transition: filter 0.15s ease;
    }
    .bloom-item:active {
      cursor: grabbing;
      filter: drop-shadow(0 6px 10px rgba(158, 59, 90, 0.35));
    }
  </style>
</head>
<body class="min-h-screen flex flex-col items-center">

  <!-- Toast Notification (Replaces alert) -->
  <div id="toast" class="fixed top-4 left-1/2 -translate-x-1/2 z-50 pointer-events-none opacity-0 transition-all duration-300 transform -translate-y-2 w-[90%] max-w-[400px]">
    <div class="bg-white/95 backdrop-blur-md border border-pink-200 text-coquette-text px-4 py-3 rounded-2xl shadow-coquette flex items-center gap-3 text-sm font-semibold">
      <span id="toast-icon" class="text-lg">🎀</span>
      <span id="toast-msg" class="flex-1">Link copied to clipboard!</span>
    </div>
  </div>

  <!-- Main Mobile-First Container (Optimal normal phone width: 100% on phone, max 460px on desktop) -->
  <div id="app-shell" class="w-full max-w-[460px] min-h-screen bg-[#FFF8FA]/90 backdrop-blur-sm shadow-2xl border-x border-pink-100 flex flex-col relative pb-12">

    <!-- Top Ribbon Header -->
    <header class="sticky top-0 z-30 bg-white/85 backdrop-blur-md border-b border-pink-100 px-4 py-3 flex items-center justify-between">
      <button onclick="switchToCreateMode()" class="flex items-center gap-2 text-left group">
        <!-- Mini SVG Coquette Bow -->
        <svg class="w-7 h-7 text-coquette-rose transition-transform group-hover:scale-110" viewBox="0 0 64 48" fill="none">
          <path d="M32 24C22 10 6 8 6 20C6 30 20 30 32 24Z" fill="#F48FB1" stroke="#9E3B5A" stroke-width="2.2"/>
          <path d="M32 24C42 10 58 8 58 20C58 30 44 30 32 24Z" fill="#F48FB1" stroke="#9E3B5A" stroke-width="2.2"/>
          <path d="M29 26L18 44L25 41L31 28" fill="#F8BBD0" stroke="#9E3B5A" stroke-width="2" stroke-linejoin="round"/>
          <path d="M35 26L46 44L39 41L33 28" fill="#F8BBD0" stroke="#9E3B5A" stroke-width="2" stroke-linejoin="round"/>
          <rect x="27" y="19" width="10" height="10" rx="4" fill="#D86C8B" stroke="#9E3B5A" stroke-width="2"/>
        </svg>
        <span class="font-script text-3xl text-coquette-deep tracking-wide leading-none pt-1">Shabouquet</span>
      </button>

      <div id="header-badge" class="text-xs font-semibold px-3 py-1 rounded-full bg-coquette-blush text-coquette-deep border border-pink-200">
        Coquette Atelier
      </div>
    </header>

    <!-- ==================== HERO / WELCOME BANNER ==================== -->
    <section id="hero-section" class="px-5 pt-6 pb-5 text-center relative overflow-hidden">
      <!-- Decorative Corner Ribbons -->
      <div class="inline-flex items-center gap-1.5 px-3.5 py-1 rounded-full bg-white border border-pink-200 text-xs font-semibold text-coquette-rose shadow-sm mb-3">
        <span>౨ৎ</span>
        <span>Handcrafted Digital Floristry</span>
        <span>౨ৎ</span>
      </div>

      <h1 class="font-script text-5xl sm:text-6xl text-coquette-deep drop-shadow-sm mb-1">
        Shabouquet
      </h1>
      <p class="font-serif italic text-base text-coquette-muted max-w-[330px] mx-auto leading-relaxed">
        Arrange lush blooming flowers, tie a satin coquette bow, attach a Polaroid & Spotify song to share with someone special.
      </p>

      <div class="mt-4 flex justify-center gap-2.5">
        <a href="#builder-studio" class="px-5 py-2.5 rounded-full bg-gradient-to-r from-[#E87A9B] to-[#D86C8B] text-white font-semibold text-sm shadow-coquette hover:opacity-95 active:scale-95 transition flex items-center gap-2">
          <span>Start Arranging</span>
          <span>🌷</span>
        </a>
        <button onclick="loadSampleBouquet()" class="px-4 py-2.5 rounded-full bg-white border border-pink-200 text-coquette-deep font-semibold text-sm hover:bg-pink-50 active:scale-95 transition">
          Inspire Me ✨
        </button>
      </div>
    </section>

    <!-- ==================== RECIPIENT VIEW BANNER (Shown when opening shared link) ==================== -->
    <section id="recipient-banner" class="hidden px-5 pt-6 pb-3 text-center">
      <div class="inline-flex items-center gap-1.5 px-3.5 py-1 rounded-full bg-pink-100 border border-pink-200 text-xs font-bold text-coquette-deep mb-2">
        <span>💌</span>
        <span>A Special Bouquet Arrived For You</span>
      </div>
      <h1 class="font-script text-5xl text-coquette-deep leading-tight">Shabouquet</h1>
      <p class="text-sm text-coquette-muted font-medium">Custom arranged with love & ribbons</p>
    </section>

    <!-- ==================== LIVE BOUQUET STAGE (Shared by Builder & Recipient View) ==================== -->
    <main id="builder-studio" class="px-4 space-y-5 flex-1">
      
      <!-- Bouquet Preview Card -->
      <div class="bg-coquette-card rounded-[28px] lace-border p-3.5 shadow-coquette relative overflow-hidden">
        
        <!-- Top Canvas Controls (Builder mode only) -->
        <div id="canvas-toolbar" class="flex items-center justify-between mb-2 px-1">
          <span class="text-xs font-bold text-coquette-muted flex items-center gap-1">
            <span>🌸</span> <span id="bloom-counter">0 / 14 Blooms</span>
          </span>
          <div class="flex items-center gap-1.5">
            <button onclick="autoArrangeBouquet()" title="Arrange neatly" class="text-xs font-semibold px-2.5 py-1.5 rounded-full bg-pink-50 text-coquette-deep border border-pink-200 hover:bg-pink-100 active:scale-95 transition">
              ✨ Auto-Arrange
            </button>
            <button onclick="clearBouquet()" title="Clear flowers" class="text-xs font-semibold px-2.5 py-1.5 rounded-full bg-white text-coquette-muted border border-pink-100 hover:bg-red-50 hover:text-red-500 active:scale-95 transition">
              Reset
            </button>
          </div>
        </div>

        <!-- THE BOUQUET VISUAL STAGE -->
        <div id="bouquet-stage" class="relative w-full h-[410px] rounded-[22px] bg-gradient-to-b from-[#FFF3F7] via-[#FFF9FB] to-[#FCE9F0] border border-pink-100 overflow-hidden select-none">
          
          <!-- Subtle Decorative Watermark & Corner Bows -->
          <div class="absolute top-3 left-3 text-pink-300/80 text-sm pointer-events-none font-serif italic">౨ৎ Shabouquet</div>
          <div class="absolute top-3 right-3 text-pink-300/80 text-sm pointer-events-none">♡</div>

          <!-- 1. Back Wrapping Paper Cone (SVG) -->
          <div id="wrap-back-layer" class="absolute inset-0 pointer-events-none flex items-end justify-center">
            <!-- Injected dynamically based on selected wrap style -->
          </div>

          <!-- 2. Greenery / Foliage Base -->
          <svg class="absolute bottom-[82px] left-1/2 -translate-x-1/2 w-[310px] h-[230px] pointer-events-none opacity-95" viewBox="0 0 320 240" fill="none">
            <!-- Eucalyptus & Fern Leaves behind flowers -->
            <g fill="#8BB18A" stroke="#5E825E" stroke-width="1.5">
              <path d="M160 210 C110 160, 65 120, 42 75 C75 88, 118 135, 160 210 Z"/>
              <path d="M160 210 C210 160, 255 120, 278 75 C245 88, 202 135, 160 210 Z"/>
              <path d="M160 205 C135 135, 115 78, 108 32 C132 62, 148 125, 160 205 Z"/>
              <path d="M160 205 C185 135, 205 78, 212 32 C188 62, 172 125, 160 205 Z"/>
              <path d="M160 215 C90 185, 45 162, 24 134 C60 138, 108 168, 160 215 Z"/>
              <path d="M160 215 C230 185, 275 162, 296 134 C260 138, 212 168, 160 215 Z"/>
            </g>
            <!-- Delicate leaf veins -->
            <g stroke="#A8CCA7" stroke-width="1" stroke-linecap="round">
              <path d="M155 202 L55 86"/>
              <path d="M165 202 L265 86"/>
              <path d="M160 198 L113 44"/>
              <path d="M160 198 L207 44"/>
            </g>
          </svg>

          <!-- 3. Interactive Blooming Flowers Layer -->
          <div id="flowers-layer" class="absolute inset-0 z-10">
            <!-- Flowers rendered here; users can drag to adjust or tap to remove -->
          </div>

          <!-- Empty State Prompt -->
          <div id="empty-hint" class="absolute inset-x-0 top-24 z-10 flex flex-col items-center justify-center pointer-events-none px-6 text-center">
            <div class="bg-white/85 backdrop-blur-sm px-4 py-2.5 rounded-2xl border border-pink-200 shadow-sm">
              <p class="text-xs font-bold text-coquette-deep">Tap flowers below to fill your bouquet 🌸</p>
              <p class="text-[11px] text-coquette-muted mt-0.5">Drag blooms to arrange • Tap a bloom to remove</p>
            </div>
          </div>

          <!-- 4. Front Wrapping Paper Fold + Coquette Ribbon & Bow -->
          <div id="wrap-front-layer" class="absolute inset-0 z-20 pointer-events-none flex items-end justify-center">
            <!-- Injected dynamically -->
          </div>

          <!-- 5. Mini Polaroid Photo Attachment (Tucked on left side of bouquet) -->
          <div id="polaroid-card" class="hidden absolute bottom-6 left-3.5 z-30 w-[96px] bg-white p-1.5 pb-4 rounded-md shadow-polaroid border border-pink-100 transform -rotate-6 transition-transform hover:rotate-0 hover:scale-105">
            <!-- Mini Pink Bow Clip on Polaroid -->
            <div class="absolute -top-2.5 left-1/2 -translate-x-1/2 text-sm drop-shadow-sm">🎀</div>
            <div class="w-full h-[76px] bg-pink-50 rounded-sm overflow-hidden flex items-center justify-center">
              <img id="polaroid-img" src="" alt="Attached memory" class="w-full h-full object-cover" />
            </div>
            <p id="polaroid-caption" class="font-script text-sm text-center text-coquette-deep mt-1 truncate leading-none">
              for you ♡
            </p>
          </div>

          <!-- 6. Coquette Hanging Gift Tag (Tucked on right side of bouquet) -->
          <div id="gift-tag-preview" class="absolute bottom-5 right-3 z-30 max-w-[148px] bg-[#FFFDFB]/95 backdrop-blur-sm border border-pink-200 rounded-2xl px-3 py-2 shadow-polaroid transform rotate-3">
            <div class="flex items-center justify-between gap-1 border-b border-dashed border-pink-200 pb-1 mb-1">
              <span class="text-[10px] font-bold uppercase tracking-wider text-coquette-rose">To:</span>
              <span id="tag-to-display" class="font-serif font-semibold text-xs text-coquette-deep truncate">My Dearest</span>
            </div>
            <p id="tag-msg-display" class="text-[11px] text-coquette-text leading-snug line-clamp-3 italic">
              "Sending you a bouquet full of sweetness and warm hugs ౨ৎ"
            </p>
            <div class="mt-1 text-right text-[10px] font-semibold text-coquette-muted truncate">
              — <span id="tag-from-display">Your Admirer</span>
            </div>
          </div>

        </div>

        <!-- Full Note Card Below Bouquet (Great for longer messages, especially in Recipient View) -->
        <div id="full-letter-card" class="mt-3 bg-[#FFF9FB] border border-pink-100 rounded-2xl p-3.5 text-center relative">
          <div class="text-xs font-bold tracking-widest uppercase text-coquette-rose mb-1">౨ৎ Love Note ౨ৎ</div>
          <p class="font-serif text-sm text-coquette-deep font-semibold">
            Dear <span id="letter-to">My Dearest</span>,
          </p>
          <p id="letter-body" class="text-sm text-coquette-text mt-1 leading-relaxed whitespace-pre-line">
            Sending you a bouquet full of sweetness and warm hugs ౨ৎ
          </p>
          <p class="font-script text-2xl text-coquette-rose mt-1.5">
            With love, <span id="letter-from">Your Admirer</span>
          </p>
        </div>

        <!-- Spotify Music Player Card -->
        <div id="spotify-preview-wrapper" class="mt-3 rounded-2xl overflow-hidden border border-pink-200 bg-white shadow-sm">
          <div class="bg-pink-50/80 px-3 py-1.5 flex items-center justify-between border-b border-pink-100">
            <span class="text-[11px] font-bold text-coquette-deep flex items-center gap-1.5">
              <svg class="w-3.5 h-3.5 text-[#1DB954]" viewBox="0 0 24 24" fill="currentColor">
                <path d="M12 0C5.4 0 0 5.4 0 12s5.4 12 12 12 12-5.4 12-12S18.66 0 12 0zm5.521 17.34c-.24.359-.66.48-1.021.24-2.82-1.74-6.36-2.101-10.561-1.141-.418.122-.779-.179-.899-.539-.12-.421.18-.78.54-.9 4.56-1.021 8.52-.6 11.64 1.32.42.18.479.659.301 1.02zm1.44-3.3c-.301.42-.841.6-1.262.3-3.239-1.98-8.159-2.58-11.939-1.38-.479.12-1.02-.12-1.14-.6-.12-.48.12-1.021.6-1.141C9.6 9.9 15 10.561 18.72 12.84c.361.181.54.78.241 1.2zm.12-3.36C15.24 8.4 8.82 8.16 5.16 9.301c-.6.179-1.2-.181-1.38-.721-.18-.601.18-1.2.72-1.381 4.26-1.26 11.28-1.02 15.721 1.621.539.3.719 1.02.419 1.56-.299.421-1.02.599-1.559.3z"/>
              </svg>
              <span>Dedicated Song</span>
            </span>
            <span id="song-title-badge" class="text-[11px] font-semibold text-coquette-muted truncate max-w-[180px]">Lover — Taylor Swift</span>
          </div>
          <iframe 
            id="spotify-iframe"
            style="border-radius:0 0 14px 14px" 
            src="https://open.spotify.com/embed/track/1dGr1c8CrMLDpV6mPbImSI?utm_source=generator&theme=0" 
            width="100%" 
            height="80" 
            frameBorder="0" 
            allowfullscreen="" 
            allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" 
            loading="lazy">
          </iframe>
        </div>
      </div>

      <!-- ==================== CUSTOMIZATION CONTROLS (Hidden in Recipient View) ==================== -->
      <div id="builder-controls" class="space-y-4">

        <!-- Step 1: Choose Blooming Flowers -->
        <div class="bg-white rounded-3xl p-4 border border-pink-100 shadow-sm">
          <div class="flex items-center justify-between mb-2.5">
            <h2 class="font-serif font-bold text-base text-coquette-deep flex items-center gap-2">
              <span class="w-6 h-6 rounded-full bg-coquette-blush text-coquette-deep text-xs flex items-center justify-center font-sans font-bold">1</span>
              Pick Blooming Flowers
            </h2>
            <span class="text-[11px] text-coquette-muted font-medium">Tap to add</span>
          </div>

          <div id="flower-palette" class="grid grid-cols-4 gap-2">
            <!-- Populated by JS with rich SVG blossoms -->
          </div>
        </div>

        <!-- Step 2: Bouquet Wrap & Coquette Ribbon -->
        <div class="bg-white rounded-3xl p-4 border border-pink-100 shadow-sm space-y-4">
          <h2 class="font-serif font-bold text-base text-coquette-deep flex items-center gap-2">
            <span class="w-6 h-6 rounded-full bg-coquette-blush text-coquette-deep text-xs flex items-center justify-center font-sans font-bold">2</span>
            Wrapping Paper & Coquette Bow
          </h2>

          <!-- Wrapping Paper Style -->
          <div>
            <label class="block text-xs font-bold text-coquette-muted mb-2">Wrapping Paper</label>
            <div id="wrap-options" class="grid grid-cols-4 gap-2">
              <!-- Populated by JS -->
            </div>
          </div>

          <!-- Ribbon & Bow Style -->
          <div>
            <label class="block text-xs font-bold text-coquette-muted mb-2">Satin Ribbon & Bow</label>
            <div id="ribbon-options" class="grid grid-cols-4 gap-2">
              <!-- Populated by JS -->
            </div>
          </div>
        </div>

        <!-- Step 3: Name, Personal Note & Polaroid Photo -->
        <div class="bg-white rounded-3xl p-4 border border-pink-100 shadow-sm space-y-3.5">
          <h2 class="font-serif font-bold text-base text-coquette-deep flex items-center gap-2">
            <span class="w-6 h-6 rounded-full bg-coquette-blush text-coquette-deep text-xs flex items-center justify-center font-sans font-bold">3</span>
            Gift Tag, Note & Polaroid Photo
          </h2>

          <div class="grid grid-cols-2 gap-2.5">
            <div>
              <label class="block text-xs font-bold text-coquette-muted mb-1">To (Recipient)</label>
              <input id="input-to" type="text" maxlength="28" placeholder="e.g. Bella" value="My Dearest"
                oninput="updateCardDetails()"
                class="w-full px-3.5 py-2.5 rounded-xl bg-[#FFF8FA] border border-pink-200 text-sm text-coquette-text focus:outline-none focus:border-coquette-rose" />
            </div>
            <div>
              <label class="block text-xs font-bold text-coquette-muted mb-1">From (Your Name)</label>
              <input id="input-from" type="text" maxlength="28" placeholder="e.g. Shabrina" value="Your Admirer"
                oninput="updateCardDetails()"
                class="w-full px-3.5 py-2.5 rounded-xl bg-[#FFF8FA] border border-pink-200 text-sm text-coquette-text

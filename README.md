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
                class="w-full px-3.5 py-2.5 rounded-xl bg-[#FFF8FA] border border-pink-200 text-sm text-coquette-text focus:outline-none focus:border-coquette-rose" />
            </div>
          </div>

          <div>
            <label class="block text-xs font-bold text-coquette-muted mb-1">Heartfelt Message</label>
            <textarea id="input-msg" rows="3" maxlength="220" placeholder="Write a sweet message..."
              oninput="updateCardDetails()"
              class="w-full px-3.5 py-2.5 rounded-xl bg-[#FFF8FA] border border-pink-200 text-sm text-coquette-text focus:outline-none focus:border-coquette-rose resize-none">Sending you a bouquet full of sweetness and warm hugs ౨ৎ</textarea>
          </div>

          <!-- Polaroid Photo Upload -->
          <div class="pt-1">
            <label class="block text-xs font-bold text-coquette-muted mb-1.5">Attach Mini Polaroid Photo (Optional)</label>
            <div class="flex items-center gap-2.5">
              <label class="flex-1 cursor-pointer flex items-center justify-center gap-2 px-3.5 py-2.5 rounded-xl border-2 border-dashed border-pink-200 bg-[#FFF8FA] hover:bg-pink-50 text-xs font-bold text-coquette-deep transition">
                <span>📸</span>
                <span id="photo-upload-label">Choose Photo from Gallery</span>
                <input id="input-photo" type="file" accept="image/*" class="hidden" onchange="handlePhotoUpload(event)" />
              </label>
              <button id="remove-photo-btn" onclick="removePhoto()" class="hidden px-3 py-2.5 rounded-xl bg-red-50 text-red-500 text-xs font-bold border border-red-100">
                Remove
              </button>
            </div>
            <div id="polaroid-caption-box" class="hidden mt-2">
              <input id="input-caption" type="text" maxlength="20" placeholder="Polaroid caption (e.g. us ♡)" value="for you ♡"
                oninput="updateCardDetails()"
                class="w-full px-3 py-2 rounded-xl bg-[#FFF8FA] border border-pink-200 text-xs text-coquette-text focus:outline-none focus:border-coquette-rose" />
            </div>
            <p class="text-[11px] text-coquette-muted mt-1.5 leading-tight">
              *Uploaded photos are automatically compressed so they travel inside your shareable link!
            </p>
          </div>
        </div>

        <!-- Step 4: Spotify Song Selection -->
        <div class="bg-white rounded-3xl p-4 border border-pink-100 shadow-sm space-y-3">
          <h2 class="font-serif font-bold text-base text-coquette-deep flex items-center gap-2">
            <span class="w-6 h-6 rounded-full bg-coquette-blush text-coquette-deep text-xs flex items-center justify-center font-sans font-bold">4</span>
            Choose a Spotify Song
          </h2>

          <div>
            <label class="block text-xs font-bold text-coquette-muted mb-1.5">Coquette & Romantic Picks</label>
            <select id="spotify-preset" onchange="selectPresetSong()" class="w-full px-3.5 py-2.5 rounded-xl bg-[#FFF8FA] border border-pink-200 text-sm font-medium text-coquette-text focus:outline-none focus:border-coquette-rose">
              <option value="1dGr1c8CrMLDpV6mPbImSI|Lover — Taylor Swift">🎀 Lover — Taylor Swift</option>
              <option value="5GUYJTQap5F3RDQiCOJhrS|Glue Song — beabadoobee">🌸 Glue Song — beabadoobee</option>
              <option value="7FbrGaHYVDmfr7KoLIZnQ7|Cupid (Twin Ver.) — FIFTY FIFTY">💘 Cupid (Twin Ver.) — FIFTY FIFTY</option>
              <option value="3U4isOIWM3VvDubwSI3y7a|All of Me — John Legend">🕊️ All of Me — John Legend</option>
              <option value="44AyOl4qVkzS48vBsbNXaC|Can't Help Falling in Love — Elvis">💌 Can't Help Falling in Love</option>
              <option value="0tgVpDi06FyKpA1z0VMD4v|Perfect — Ed Sheeran">✨ Perfect — Ed Sheeran</option>
              <option value="1RMJfgR6498G143z2z2D1V|From The Start — Laufey">🩰 From The Start — Laufey</option>
              <option value="6dBUzqjtbnIa1TwYbyw5CM|Lovers Rock — TV Girl">🍓 Lovers Rock — TV Girl</option>
            </select>
          </div>

          <div>
            <label class="block text-xs font-bold text-coquette-muted mb-1">Or Paste Any Spotify Track Link</label>
            <div class="flex gap-2">
              <input id="spotify-custom-url" type="text" placeholder="https://open.spotify.com/track/..."
                class="flex-1 px-3 py-2 rounded-xl bg-[#FFF8FA] border border-pink-200 text-xs text-coquette-text focus:outline-none focus:border-coquette-rose" />
              <button onclick="applyCustomSpotify()" class="px-3.5 py-2 rounded-xl bg-coquette-blush text-coquette-deep font-bold text-xs border border-pink-200 hover:bg-pink-200 transition">
                Apply
              </button>
            </div>
          </div>
        </div>

        <!-- Step 5: Generate & Share Link -->
        <div class="bg-gradient-to-br from-[#FCE4EC] to-[#FFF0F5] rounded-3xl p-4 border-2 border-pink-200 shadow-coquette text-center space-y-3">
          <div class="text-2xl">🎀💌🌷</div>
          <h3 class="font-serif font-bold text-lg text-coquette-deep">Ready to Gift Your Shabouquet?</h3>
          <p class="text-xs text-coquette-muted leading-relaxed">
            Generate a unique link containing your custom flowers, ribbon, Polaroid photo, note, and Spotify song!
          </p>

          <button onclick="generateShareLink()" class="w-full py-3.5 px-5 rounded-2xl bg-gradient-to-r from-[#E46B8E] to-[#C84E73] text-white font-bold text-sm shadow-coquette hover:opacity-95 active:scale-[0.98] transition flex items-center justify-center gap-2">
            <span>Create Shareable Bouquet Link</span>
            <span>🔗</span>
          </button>

          <!-- Generated Link Result Box -->
          <div id="share-result-box" class="hidden pt-2 space-y-2.5 text-left animate-fadeIn">
            <label class="block text-[11px] font-bold uppercase tracking-wider text-coquette-deep">Your Shareable Gift Link:</label>
            <div class="flex items-center gap-1.5 bg-white p-1.5 rounded-xl border border-pink-200">
              <input id="share-url-input" type="text" readonly class="flex-1 text-xs text-coquette-text bg-transparent px-2 focus:outline-none select-all" />
              <button onclick="copyShareUrl()" class="px-3 py-2 rounded-lg bg-coquette-deep text-white text-xs font-bold hover:opacity-90 active:scale-95 transition shrink-0">
                Copy
              </button>
            </div>

            <div class="grid grid-cols-2 gap-2 pt-1">
              <button onclick="nativeShareUrl()" class="py-2.5 px-3 rounded-xl bg-white border border-pink-200 text-coquette-deep font-bold text-xs flex items-center justify-center gap-1.5 shadow-sm hover:bg-pink-50 active:scale-95 transition">
                <span>📤</span>
                <span>Share via App</span>
              </button>
              <button onclick="previewAsRecipient()" class="py-2.5 px-3 rounded-xl bg-pink-100 border border-pink-200 text-coquette-deep font-bold text-xs flex items-center justify-center gap-1.5 hover:bg-pink-200 active:scale-95 transition">
                <span>👀</span>
                <span>Preview Gift View</span>
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- ==================== RECIPIENT MODE CTA (Shown only when viewing a shared bouquet) ==================== -->
      <div id="recipient-actions" class="hidden bg-white rounded-3xl p-4 border border-pink-200 shadow-coquette text-center space-y-3">
        <p class="font-serif italic text-sm text-coquette-muted">
          Want to send a custom bouquet back?
        </p>
        <button onclick="switchToCreateMode()" class="w-full py-3.5 px-5 rounded-2xl bg-gradient-to-r from-[#E46B8E] to-[#C84E73] text-white font-bold text-sm shadow-coquette hover:opacity-95 active:scale-95 transition">
          Create Your Own on Shabouquet 🌷
        </button>
      </div>

    </main>

    <!-- Footer -->
    <footer class="mt-8 px-4 text-center text-xs text-coquette-muted">
      <p class="font-script text-2xl text-coquette-rose">Shabouquet</p>
      <p class="mt-0.5">Made with ribbons, petals & sweet melodies ౨ৎ</p>
    </footer>
  </div>

  <script>
    /* ==========================================================================
       1. RICH SVG FLOWER CATALOG (Full Lush Blooms, Not Just Stems)
       ========================================================================== */
    const FLOWERS = [
      {
        id: 'pink-rose',
        name: 'Blush Rose',
        svg: `
          <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-md">
            <defs>
              <radialGradient id="roseGrad" cx="45%" cy="40%" r="55%">
                <stop offset="0%" stop-color="#FFD6E4"/>
                <stop offset="55%" stop-color="#F48FB1"/>
                <stop offset="100%" stop-color="#C24B70"/>
              </radialGradient>
            </defs>
            <!-- Outer lush petals -->
            <path d="M50 10 C72 8, 92 26, 88 48 C94 68, 76 90, 52 88 C28 92, 10 72, 14 48 C8 26, 28 8, 50 10 Z" fill="url(#roseGrad)" stroke="#AD3A5E" stroke-width="1.5"/>
            <!-- Mid layered petals -->
            <path d="M50 18 C68 18, 80 32, 76 50 C80 66, 65 78, 50 76 C33 78, 20 65, 24 50 C20 32, 33 18, 50 18 Z" fill="#F8A5C2" stroke="#B84468" stroke-width="1.4"/>
            <path d="M30 34 C42 20, 66 22, 72 38 C76 54, 62 70, 46 68 C30 66, 22 48, 30 34 Z" fill="#F48FB1" stroke="#AD3A5E" stroke-width="1.3"/>
            <!-- Inner swirl -->
            <path d="M40 40 C48 30, 64 34, 62 48 C60 60, 42 62, 38 50 C35 42, 44 36, 52 40 C58 43, 55 52, 48 51" fill="none" stroke="#9E3B5A" stroke-width="2.2" stroke-linecap="round"/>
          </svg>`
      },
      {
        id: 'peony',
        name: 'Soft Peony',
        svg: `
          <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-md">
            <g fill="#FCE4EC" stroke="#D87A98" stroke-width="1.4">
              <circle cx="50" cy="24" r="18"/>
              <circle cx="74" cy="38" r="18"/>
              <circle cx="70" cy="68" r="18"/>
              <circle cx="30" cy="68" r="18"/>
              <circle cx="26" cy="38" r="18"/>
            </g>
            <g fill="#F8BBD0" stroke="#C96484" stroke-width="1.3">
              <circle cx="50" cy="32" r="15"/>
              <circle cx="66" cy="46" r="15"/>
              <circle cx="60" cy="64" r="15"/>
              <circle cx="40" cy="64" r="15"/>
              <circle cx="34" cy="46" r="15"/>
            </g>
            <circle cx="50" cy="50" r="14" fill="#F48FB1" stroke="#B5496B" stroke-width="1.3"/>
            <circle cx="50" cy="50" r="6" fill="#FFE082"/>
            <circle cx="47" cy="48" r="1.5" fill="#D86C8B"/>
            <circle cx="53" cy="49" r="1.5" fill="#D86C8B"/>
            <circle cx="50" cy="53" r="1.5" fill="#D86C8B"/>
          </svg>`
      },
      {
        id: 'tulip',
        name: 'Pink Tulip',
        svg: `
          <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-md">
            <!-- Side petals -->
            <path d="M26 24 C18 46, 22 76, 50 84 C44 60, 42 36, 26 24 Z" fill="#F48FB1" stroke="#AD3A5E" stroke-width="1.5"/>
            <path d="M74 24 C82 46, 78 76, 50 84 C56 60, 58 36, 74 24 Z" fill="#F48FB1" stroke="#AD3A5E" stroke-width="1.5"/>
            <!-- Center lush cup petal -->
            <path d="M50 14 C34 32, 32 68, 50 84 C68 68, 66 32, 50 14 Z" fill="#FFBCD3" stroke="#AD3A5E" stroke-width="1.5"/>
            <path d="M50 22 C44 40, 44 64, 50 78" fill="none" stroke="#E06D90" stroke-width="1.3" stroke-linecap="round"/>
          </svg>`
      },
      {
        id: 'daisy',
        name: 'Sweet Daisy',
        svg: `
          <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-md">
            <g fill="#FFFDFB" stroke="#E2C6D0" stroke-width="1.3">
              <ellipse cx="50" cy="20" rx="8" ry="18"/>
              <ellipse cx="50" cy="80" rx="8" ry="18"/>
              <ellipse cx="20" cy="50" rx="18" ry="8"/>
              <ellipse cx="80" cy="50" rx="18" ry="8"/>
              <ellipse cx="29" cy="29" rx="8" ry="18" transform="rotate(-45 29 29)"/>
              <ellipse cx="71" cy="71" rx="8" ry="18" transform="rotate(-45 71 71)"/>
              <ellipse cx="71" cy="29" rx="8" ry="18" transform="rotate(45 71 29)"/>
              <ellipse cx="29" cy="71" rx="8" ry="18" transform="rotate(45 29 71)"/>
            </g>
            <circle cx="50" cy="50" r="13" fill="#FFD54F" stroke="#E0A82E" stroke-width="1.5"/>
            <circle cx="46" cy="47" r="1.5" fill="#C78A14"/>
            <circle cx="54" cy="48" r="1.5" fill="#C78A14"/>
            <circle cx="50" cy="54" r="1.5" fill="#C78A14"/>
          </svg>`
      },
      {
        id: 'sakura',
        name: 'Cherry Blossom',
        svg: `
          <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-md">
            <g fill="#FFD9E6" stroke="#E284A3" stroke-width="1.4">
              <path d="M50 48 C38 22, 44 10, 50 16 C56 10, 62 22, 50 48 Z"/>
              <path d="M50 48 C72 30, 86 34, 82 41 C89 46, 78 56, 50 48 Z"/>
              <path d="M50 48 C74 64, 74 79, 66 78 C64 86, 52 78, 50 48 Z"/>
              <path d="M50 48 C26 64, 26 79, 34 78 C36 86, 48 78, 50 48 Z"/>
              <path d="M50 48 C28 30, 14 34, 18 41 C11 46, 22 56, 50 48 Z"/>
            </g>
            <circle cx="50" cy="49" r="8" fill="#E86A92"/>
            <g stroke="#FFF3B0" stroke-width="1.5" stroke-linecap="round">
              <line x1="50" y1="49" x2="50" y2="36"/>
              <line x1="50" y1="49" x2="61" y2="43"/>
              <line x1="50" y1="49" x2="58" y2="59"/>
              <line x1="50" y1="49" x2="42" y2="59"/>
              <line x1="50" y1="49" x2="39" y2="43"/>
            </g>
          </svg>`
      },
      {
        id: 'lily',
        name: 'White Lily',
        svg: `
          <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-md">
            <g fill="#FFFDFD" stroke="#D9B8C4" stroke-width="1.4">
              <path d="M50 50 C40 24, 45 10, 50 8 C55 10, 60 24, 50 50 Z"/>
              <path d="M50 50 C74 34, 88 38, 92 42 C88 48, 76 56, 50 50 Z"/>
              <path d="M50 50 C70 72, 72 86, 68 90 C62 88, 52 76, 50 50 Z"/>
              <path d="M50 50 C30 72, 28 86, 32 90 C38 88, 48 76, 50 50 Z"/>
              <path d="M50 50 C26 34, 12 38, 8 42 C12 48, 24 56, 50 50 Z"/>
            </g>
            <circle cx="50" cy="50" r="9" fill="#F8BBD0" opacity="0.6"/>
            <g stroke="#9E3B5A" stroke-width="1.4" stroke-linecap="round">
              <line x1="50" y1="50" x2="50" y2="35"/>
              <line x1="50" y1="50" x2="62" y2="45"/>
              <line x1="50" y1="50" x2="38" y2="45"/>
            </g>
            <circle cx="50" cy="34" r="2.2" fill="#FBC02D"/>
            <circle cx="63" cy="45" r="2.2" fill="#FBC02D"/>
            <circle cx="37" cy="45" r="2.2" fill="#FBC02D"/>
          </svg>`
      },
      {
        id: 'hydrangea',
        name: 'Lavender Puff',
        svg: `
          <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-md">
            <circle cx="50" cy="50" r="34" fill="#E1BEE7" opacity="0.45"/>
            <g fill="#E8D5F5" stroke="#9C73B8" stroke-width="1.2">
              <circle cx="38" cy="34" r="11"/>
              <circle cx="62" cy="34" r="11"/>
              <circle cx="30" cy="54" r="11"/>
              <circle cx="70" cy="54" r="11"/>
              <circle cx="42" cy="70" r="11"/>
              <circle cx="60" cy="68" r="11"/>
              <circle cx="50" cy="50" r="12" fill="#F3E5F5"/>
            </g>
            <g fill="#FFF59D">
              <circle cx="38" cy="34" r="2.5"/>
              <circle cx="62" cy="34" r="2.5"/>
              <circle cx="30" cy="54" r="2.5"/>
              <circle cx="70" cy="54" r="2.5"/>
              <circle cx="50" cy="50" r="3"/>
            </g>
          </svg>`
      },
      {
        id: 'babys-breath',
        name: "Baby's Breath",
        svg: `
          <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-sm">
            <g stroke="#96B894" stroke-width="1.8" fill="none" stroke-linecap="round">
              <path d="M50 82 L50 30"/>
              <path d="M50 64 L28 42"/>
              <path d="M50 58 L72 38"/>
              <path d="M50 48 L34 26"/>
              <path d="M50 44 L68 24"/>
            </g>
            <g fill="#FFFFFF" stroke="#E6CFD7" stroke-width="1.2">
              <circle cx="50" cy="24" r="7"/>
              <circle cx="28" cy="40" r="6.5"/>
              <circle cx="72" cy="36" r="6.5"/>
              <circle cx="34" cy="24" r="6"/>
              <circle cx="68" cy="22" r="6"/>
              <circle cx="42" cy="44" r="5.5"/>
              <circle cx="60" cy="46" r="5.5"/>
            </g>
          </svg>`
      }
    ];

    /* ==========================================================================
       2. WRAPPING PAPER & COQUETTE RIBBON OPTIONS
       ========================================================================== */
    const WRAPS = [
      { id: 'blush', name: 'Blush Silk', back: '#F8BBD0', front: '#FCE4EC', stroke: '#E07A9B', swatch: '#F8BBD0' },
      { id: 'cream', name: 'Vanilla Lace', back: '#F3E5D8', front: '#FFFBF5', stroke: '#D4B499', swatch: '#F5E6D3' },
      { id: 'berry', name: 'Berry Rose', back: '#D86C8B', front: '#EC8CA8', stroke: '#9E3B5A', swatch: '#D86C8B' },
      { id: 'lilac', name: 'Dream Lilac', back: '#D7BCE8', front: '#EDE0F5', stroke: '#9A73B5', swatch: '#D7BCE8' },
    ];

    const RIBBONS = [
      { id: 'satin-pink', name: 'Satin Pink', main: '#F06292', light: '#F8BBD0', stroke: '#882848', swatch: '#F06292' },
      { id: 'velvet-red', name: 'Velvet Cherry', main: '#B72D4B', light: '#DC5B77', stroke: '#6E1428', swatch: '#B72D4B' },
      { id: 'pearl-white', name: 'Pearl White', main: '#FFFDFD', light: '#FCE4EC', stroke: '#C98A9E', swatch: '#FFFDFD' },
      { id: 'sky-blue', name: 'Cinderella', main: '#90CAF9', light: '#E3F2FD', stroke: '#4A7BA6', swatch: '#90CAF9' },
    ];

    /* ==========================================================================
       3. APPLICATION STATE
       ========================================================================== */
    let state = {
      flowers: [], // { id, type, x, y, scale, rot }
      wrap: 'blush',
      ribbon: 'satin-pink',
      to: 'My Dearest',
      from: 'Your Admirer',
      msg: 'Sending you a bouquet full of sweetness and warm hugs ౨ৎ',
      spotifyId: '1dGr1c8CrMLDpV6mPbImSI',
      songTitle: 'Lover — Taylor Swift',
      photo: '', // Compressed base64 data URL (optional)
      caption: 'for you ♡'
    };

    // Natural lush bouquet dome coordinates (percentages inside #bouquet-stage)
    const DOME_SLOTS = [
      { x: 50, y: 31, scale: 1.05, rot: 0 },
      { x: 35, y: 35, scale: 0.98, rot: -12 },
      { x: 65, y: 35, scale: 0.98, rot: 12 },
      { x: 50, y: 44, scale: 1.08, rot: 4 },
      { x: 24, y: 43, scale: 0.92, rot: -20 },
      { x: 76, y: 43, scale: 0.92, rot: 20 },
      { x: 37, y: 49, scale: 1.0, rot: -8 },
      { x: 63, y: 49, scale: 1.0, rot: 8 },
      { x: 43, y: 22, scale: 0.88, rot: -6 },
      { x: 58, y: 22, scale: 0.88, rot: 6 },
      { x: 29, y: 27, scale: 0.85, rot: -15 },
      { x: 71, y: 27, scale: 0.85, rot: 15 },
      { x: 49, y: 55, scale: 0.96, rot: -3 },
      { x: 20, y: 53, scale: 0.84, rot: -24 },
    ];

    let isRecipientMode = false;

    /* ==========================================================================
       4. INITIALIZATION & PALETTE RENDERING
       ========================================================================== */
    window.addEventListener('DOMContentLoaded', () => {
      renderControlsUI();

      // Check if URL contains shared bouquet data in hash
      const loadedFromHash = loadFromUrlHash();
      if (!loadedFromHash) {
        // Start with a sweet starter bouquet so the user immediately sees full flowers
        loadSampleBouquet(true);
      }
    });

    function renderControlsUI() {
      // 1. Flower Palette Buttons
      const paletteEl = document.getElementById('flower-palette');
      paletteEl.innerHTML = FLOWERS.map(f => `
        <button onclick="addFlower('${f.id}')" class="flex flex-col items-center p-2 rounded-2xl bg-[#FFF8FA] border border-pink-100 hover:border-coquette-rose hover:bg-pink-50 active:scale-95 transition">
          <div class="w-11 h-11">${f.svg}</div>
          <span class="text-[11px] font-bold text-coquette-text mt-1 leading-tight text-center">${f.name}</span>
        </button>
      `).join('');

      // 2. Wrap Swatches
      const wrapEl = document.getElementById('wrap-options');
      wrapEl.innerHTML = WRAPS.map(w => `
        <button onclick="setWrap('${w.id}')" id="wrap-btn-${w.id}" class="flex flex-col items-center p-2 rounded-2xl border-2 transition ${state.wrap === w.id ? 'border-coquette-rose bg-pink-50' : 'border-pink-100 bg-[#FFF8FA]'}">
          <span class="w-7 h-7 rounded-full border border-pink-300 shadow-inner" style="background:${w.swatch}"></span>
          <span class="text-[11px] font-bold text-coquette-text mt-1">${w.name}</span>
        </button>
      `).join('');

      // 3. Ribbon Swatches
      const ribEl = document.getElementById('ribbon-options');
      ribEl.innerHTML = RIBBONS.map(r => `
        <button onclick="setRibbon('${r.id}')" id="rib-btn-${r.id}" class="flex flex-col items-center p-2 rounded-2xl border-2 transition ${state.ribbon === r.id ? 'border-coquette-rose bg-pink-50' : 'border-pink-100 bg-[#FFF8FA]'}">
          <span class="w-7 h-7 rounded-full border border-pink-300 shadow-inner flex items-center justify-center text-xs" style="background:${r.swatch}">🎀</span>
          <span class="text-[11px] font-bold text-coquette-text mt-1">${r.name}</span>
        </button>
      `).join('');
    }

    /* ==========================================================================
       5. BOUQUET STAGE RENDERING (Wrap, Flowers, Coquette Bow, Polaroid, Tag)
       ========================================================================== */
    function renderAll() {
      renderWrapAndBow();
      renderFlowers();
      renderPolaroid();
      renderTextAndSpotify();
      updateSwatchHighlights();
    }

    function updateSwatchHighlights() {
      WRAPS.forEach(w => {
        const btn = document.getElementById(`wrap-btn-${w.id}`);
        if (btn) {
          btn.className = `flex flex-col items-center p-2 rounded-2xl border-2 transition ${state.wrap === w.id ? 'border-coquette-rose bg-pink-50 shadow-sm' : 'border-pink-100 bg-[#FFF8FA]'}`;
        }
      });
      RIBBONS.forEach(r => {
        const btn = document.getElementById(`rib-btn-${r.id}`);
        if (btn) {
          btn.className = `flex flex-col items-center p-2 rounded-2xl border-2 transition ${state.ribbon === r.id ? 'border-coquette-rose bg-pink-50 shadow-sm' : 'border-pink-100 bg-[#FFF8FA]'}`;
        }
      });
    }

    function renderWrapAndBow() {
      const w = WRAPS.find(item => item.id === state.wrap) || WRAPS[0];
      const r = RIBBONS.find(item => item.id === state.ribbon) || RIBBONS[0];

      // Back Wrapping Cone with scalloped lace edge
      document.getElementById('wrap-back-layer').innerHTML = `
        <svg class="w-[330px] h-[350px]" viewBox="0 0 340 360" fill="none">
          <!-- Back diamond paper sheet -->
          <path d="M170 22 L318 148 L222 346 L118 346 L22 148 Z" fill="${w.back}" stroke="${w.stroke}" stroke-width="2.2" stroke-linejoin="round"/>
          <!-- Inner delicate lace trim -->
          <path d="M170 36 L302 150 L214 334 L126 334 L38 150 Z" fill="none" stroke="#FFFFFF" stroke-width="2" stroke-dasharray="5 5" opacity="0.85"/>
        </svg>
      `;

      // Front Wrapping Fold + Prominent Coquette Satin Bow
      document.getElementById('wrap-front-layer').innerHTML = `
        <svg class="w-[330px] h-[235px]" viewBox="0 0 340 240" fill="none">
          <!-- Left & Right overlapping paper folds -->
          <path d="M44 46 L174 132 L216 234 L122 234 Z" fill="${w.front}" stroke="${w.stroke}" stroke-width="2" stroke-linejoin="round"/>
          <path d="M296 46 L166 132 L124 234 L218 234 Z" fill="${w.front}" stroke="${w.stroke}" stroke-width="2" stroke-linejoin="round"/>
          <!-- Lace edge along front fold -->
          <path d="M52 54 L170 132 L288 54" fill="none" stroke="#FFFFFF" stroke-width="2.2" stroke-dasharray="4 4"/>

          <!-- Tied Ribbon Band around bouquet waist -->
          <rect x="128" y="126" width="84" height="15" rx="6" fill="${r.main}" stroke="${r.stroke}" stroke-width="1.8"/>

          <!-- COQUETTE BOW (Loops + Flowing Tails + Knot) -->
          <g transform="translate(170, 134)">
            <!-- Long flowing coquette tails -->
            <path d="M-8 6 C-26 34, -46 62, -56 88 L-40 82 L-32 94 C-22 66, -12 36, -2 10 Z" fill="${r.main}" stroke="${r.stroke}" stroke-width="1.8" stroke-linejoin="round"/>
            <path d="M8 6 C26 34, 46 62, 56 88 L40 82 L32 94 C22 66, 12 36, 2 10 Z" fill="${r.main}" stroke="${r.stroke}" stroke-width="1.8" stroke-linejoin="round"/>

            <!-- Left Bow Loop -->
            <path d="M-4 0 C-26 -28, -64 -24, -60 4 C-56 26, -22 18, -4 4 Z" fill="${r.main}" stroke="${r.stroke}" stroke-width="2"/>
            <path d="M-10 0 C-26 -16, -48 -14, -46 2 C-44 14, -22 10, -10 2 Z" fill="${r.light}" opacity="0.55"/>

            <!-- Right Bow Loop -->
            <path d="M4 0 C26 -28, 64 -24, 60 4 C56 26, 22 18, 4 4 Z" fill="${r.main}" stroke="${r.stroke}" stroke-width="2"/>
            <path d="M10 0 C26 -16, 48 -14, 46 2 C44 14, 22 10, 10 2 Z" fill="${r.light}" opacity="0.55"/>

            <!-- Center Ribbon Knot -->
            <rect x="-10" y="-9" width="20" height="19" rx="7" fill="${r.main}" stroke="${r.stroke}" stroke-width="2"/>
            <ellipse cx="0" cy="-2" rx="5" ry="3" fill="${r.light}" opacity="0.6"/>
          </g>
        </svg>
      `;
    }

    function renderFlowers() {
      const layer = document.getElementById('flowers-layer');
      const counter = document.getElementById('bloom-counter');
      const emptyHint = document.getElementById('empty-hint');

      counter.textContent = `${state.flowers.length} / 14 Blooms`;
      emptyHint.style.display = state.flowers.length === 0 ? 'flex' : 'none';

      layer.innerHTML = '';
      state.flowers.forEach((bloom, idx) => {
        const def = FLOWERS.find(f => f.id === bloom.type) || FLOWERS[0];
        const el = document.createElement('div');
        el.className = 'bloom-item absolute w-[74px] h-[74px] -translate-x-1/2 -translate-y-1/2 bloom-pop';
        el.style.left = `${bloom.x}%`;
        el.style.top = `${bloom.y}%`;
        el.style.zIndex = 10 + idx;
        el.style.transform = `translate(-50%, -50%) scale(${bloom.scale}) rotate(${bloom.rot}deg)`;
        el.innerHTML = def.svg;

        if (!isRecipientMode) {
          attachBloomInteractions(el, bloom);
        }

        layer.appendChild(el);
      });
    }

    // Touch & Mouse Drag to position flowers, or Quick Tap to remove
    function attachBloomInteractions(el, bloom) {
      let startX = 0, startY = 0, origX = bloom.x, origY = bloom.y;
      let moved = false;

      const onPointerDown = (e) => {
        e.preventDefault();
        const pt = e.touches ? e.touches[0] : e;
        startX = pt.clientX;
        startY = pt.clientY;
        origX = bloom.x;
        origY = bloom.y;
        moved = false;

        window.addEventListener('mousemove', onPointerMove);
        window.addEventListener('mouseup', onPointerUp);
        window.addEventListener('touchmove', onPointerMove, { passive: false });
        window.addEventListener('touchend', onPointerUp);
      };

      const onPointerMove = (e) => {
        e.preventDefault();
        const pt = e.touches ? e.touches[0] : e;
        const dx = pt.clientX - startX;
        const dy = pt.clientY - startY;
        if (Math.abs(dx) > 5 || Math.abs(dy) > 5) moved = true;

        const stage = document.getElementById('bouquet-stage').getBoundingClientRect();
        const newX = Math.min(86, Math.max(14, origX + (dx / stage.width) * 100));
        const newY = Math.min(64, Math.max(14, origY + (dy / stage.height) * 100));

        bloom.x = Number(newX.toFixed(1));
        bloom.y = Number(newY.toFixed(1));
        el.style.left = `${bloom.x}%`;
        el.style.top = `${bloom.y}%`;
      };

      const onPointerUp = () => {
        window.removeEventListener('mousemove', onPointerMove);
        window.removeEventListener('mouseup', onPointerUp);
        window.removeEventListener('touchmove', onPointerMove);
        window.removeEventListener('touchend', onPointerUp);

        if (!moved) {
          // Quick tap removes the bloom
          state.flowers = state.flowers.filter(item => item.id !== bloom.id);
          renderFlowers();
        }
      };

      el.addEventListener('mousedown', onPointerDown);
      el.addEventListener('touchstart', onPointerDown, { passive: false });
    }

    function renderPolaroid() {
      const card = document.getElementById('polaroid-card');
      const img = document.getElementById('polaroid-img');
      const cap = document.getElementById('polaroid-caption');

      if (state.photo) {
        card.classList.remove('hidden');
        img.src = state.photo;
        cap.textContent = state.caption || 'for you ♡';
      } else {
        card.classList.add('hidden');
      }
    }

    function renderTextAndSpotify() {
      const toVal = state.to.trim() || 'My Dearest';
      const fromVal = state.from.trim() || 'Your Admirer';
      const msgVal = state.msg.trim() || 'Sending you a bouquet full of sweetness ౨ৎ';

      document.getElementById('tag-to-display').textContent = toVal;
      document.getElementById('tag-msg-display').textContent = `"${msgVal}"`;
      document.getElementById('tag-from-display').textContent = fromVal;

      document.getElementById('letter-to').textContent = toVal;
      document.getElementById('letter-body').textContent = msgVal;
      document.getElementById('letter-from').textContent = fromVal;

      // Update Spotify iframe only if track changed
      const iframe = document.getElementById('spotify-iframe');
      const targetSrc = `https://open.spotify.com/embed/track/${state.spotifyId}?utm_source=generator&theme=0`;
      if (iframe.src !== targetSrc) {
        iframe.src = targetSrc;
      }
      document.getElementById('song-title-badge').textContent = state.songTitle || 'Dedicated Track';
    }

    /* ==========================================================================
       6. USER INTERACTIONS (Add Flower, Auto-Arrange, Wrap, Photo, Spotify)
       ========================================================================== */
    function addFlower(typeId) {
      if (state.flowers.length >= 14) {
        showToast('Your bouquet is full & lush! (Max 14 blooms)', '🌸');
        return;
      }
      const slot = DOME_SLOTS[state.flowers.length % DOME_SLOTS.length];
      state.flowers.push({
        id: 'fl_' + Date.now() + '_' + Math.random().toString(36).slice(2, 6),
        type: typeId,
        x: slot.x,
        y: slot.y,
        scale: slot.scale,
        rot: slot.rot
      });
      renderFlowers();
    }

    function autoArrangeBouquet() {
      if (state.flowers.length === 0) {
        showToast('Add some flowers first!', '🌷');
        return;
      }
      state.flowers.forEach((bloom, index) => {
        const slot = DOME_SLOTS[index % DOME_SLOTS.length];
        bloom.x = slot.x;
        bloom.y = slot.y;
        bloom.scale = slot.scale;
        bloom.rot = slot.rot;
      });
      renderFlowers();
      showToast('Blooms arranged neatly!', '✨');
    }

    function clearBouquet() {
      state.flowers = [];
      renderFlowers();
    }

    function setWrap(wrapId) {
      state.wrap = wrapId;
      renderWrapAndBow();
      updateSwatchHighlights();
    }

    function setRibbon(ribbonId) {
      state.ribbon = ribbonId;
      renderWrapAndBow();
      updateSwatchHighlights();
    }

    function updateCardDetails() {
      state.to = document.getElementById('input-to').value;
      state.from = document.getElementById('input-from').value;
      state.msg = document.getElementById('input-msg').value;
      state.caption = document.getElementById('input-caption').value;
      renderPolaroid();
      renderTextAndSpotify();
    }

    // Compress uploaded image onto tiny canvas so it embeds cleanly in shareable URL
    function handlePhotoUpload(event) {
      const file = event.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(e) {
        const img = new Image();
        img.onload = function() {
          const canvas = document.createElement('canvas');
          const maxDim = 135; // Compact polaroid resolution for URL sharing
          let w = img.width;
          let h = img.height;
          if (w > h) {
            if (w > maxDim) { h = Math.round(h * (maxDim / w)); w = maxDim; }
          } else {
            if (h > maxDim) { w = Math.round(w * (maxDim / h)); h = maxDim; }
          }
          canvas.width = w;
          canvas.height = h;
          const ctx = canvas.getContext('2d');
          ctx.drawImage(img, 0, 0, w, h);

          state.photo = canvas.toDataURL('image/jpeg', 0.62);
          document.getElementById('photo-upload-label').textContent = 'Photo Attached ✓';
          document.getElementById('remove-photo-btn').classList.remove('hidden');
          document.getElementById('polaroid-caption-box').classList.remove('hidden');
          renderPolaroid();
          showToast('Polaroid photo tucked into bouquet!', '📸');
        };
        img.src = e.target.result;
      };
      reader.readAsDataURL(file);
    }

    function removePhoto() {
      state.photo = '';
      document.getElementById('input-photo').value = '';
      document.getElementById('photo-upload-label').textContent = 'Choose Photo from Gallery';
      document.getElementById('remove-photo-btn').classList.add('hidden');
      document.getElementById('polaroid-caption-box').classList.add('hidden');
      renderPolaroid();
    }

    function selectPresetSong() {
      const val = document.getElementById('spotify-preset').value;
      const [id, title] = val.split('|');
      state.spotifyId = id;
      state.songTitle = title;
      renderTextAndSpotify();
    }

    function applyCustomSpotify() {
      const raw = document.getElementById('spotify-custom-url').value.trim();
      if (!raw) return;

      // Extract track ID from open.spotify.com/track/ID or spotify:track:ID
      const match = raw.match(/(?:track\/|track:)([a-zA-Z0-9]{22})/);
      if (match && match[1]) {
        state.spotifyId = match[1];
        state.songTitle = 'Custom Spotify Track';
        renderTextAndSpotify();
        showToast('Spotify song updated!', '🎵');
      } else {
        showToast('Please paste a valid Spotify track link', '⚠️');
      }
    }

    function loadSampleBouquet(silent = false) {
      const sampleTypes = ['pink-rose', 'peony', 'tulip', 'sakura', 'babys-breath', 'pink-rose', 'daisy', 'hydrangea'];
      state.flowers = sampleTypes.map((type, idx) => ({
        id: 'init_' + idx,
        type,
        x: DOME_SLOTS[idx].x,
        y: DOME_SLOTS[idx].y,
        scale: DOME_SLOTS[idx].scale,
        rot: DOME_SLOTS[idx].rot
      }));
      state.wrap = 'blush';
      state.ribbon = 'satin-pink';
      renderAll();
      if (!silent) showToast('Loaded a dreamy coquette arrangement!', '🎀');
    }

    /* ==========================================================================
       7. SHAREABLE LINK ENCODING & RECIPIENT VIEW MODE
       ========================================================================== */
    function encodeStateToHash() {
      const payload = {
        f: state.flowers.map(b => [b.type, b.x, b.y, b.scale, b.rot]),
        w: state.wrap,
        r: state.ribbon,
        t: state.to,
        m: state.from,
        n: state.msg,
        s: state.spotifyId,
        st: state.songTitle,
        p: state.photo,
        c: state.caption
      };
      const jsonStr = JSON.stringify(payload);
      // UTF-8 safe Base64 encoding
      const base64 = btoa(unescape(encodeURIComponent(jsonStr)));
      return base64;
    }

    function loadFromUrlHash() {
      const hash = window.location.hash;
      if (!hash || !hash.startsWith('#gift=')) return false;

      try {
        const base64 = hash.replace('#gift=', '');
        const jsonStr = decodeURIComponent(escape(atob(base64)));
        const data = JSON.parse(jsonStr);

        state.flowers = (data.f || []).map((arr, idx) => ({
          id: 'shared_' + idx,
          type: arr[0],
          x: arr[1],
          y: arr[2],
          scale: arr[3] || 1,
          rot: arr[4] || 0
        }));
        state.wrap = data.w || 'blush';
        state.ribbon = data.r || 'satin-pink';
        state.to = data.t || 'My Dearest';
        state.from = data.m || 'Your Admirer';
        state.msg = data.n || '';
        state.spotifyId = data.s || '1dGr1c8CrMLDpV6mPbImSI';
        state.songTitle = data.st || 'Dedicated Track';
        state.photo = data.p || '';
        state.caption = data.c || 'for you ♡';

        enterRecipientMode();
        renderAll();
        return true;
      } catch (err) {
        console.error('Could not parse shared bouquet:', err);
        return false;
      }
    }

    function generateShareLink() {
      if (state.flowers.length === 0) {
        showToast('Add at least one flower to your bouquet first!', '🌷');
        return;
      }

      const encoded = encodeStateToHash();
      const baseUrl = window.location.href.split('#')[0];
      const fullShareUrl = `${baseUrl}#gift=${encoded}`;

      // Update browser hash so it's immediately valid
      history.replaceState(null, '', `#gift=${encoded}`);

      const resultBox = document.getElementById('share-result-box');
      const urlInput = document.getElementById('share-url-input');
      urlInput.value = fullShareUrl;
      resultBox.classList.remove('hidden');

      copyShareUrl();
    }

    function copyShareUrl() {
      const urlInput = document.getElementById('share-url-input');
      urlInput.select();
      urlInput.setSelectionRange(0, 99999);
      try {
        document.execCommand('copy');
        showToast('Bouquet link copied! Ready to paste & share 💌', '🎀');
      } catch (e) {
        showToast('Link generated! Select and copy the link above.', '🔗');
      }
    }

    async function nativeShareUrl() {
      const url = document.getElementById('share-url-input').value;
      if (navigator.share) {
        try {
          await navigator.share({
            title: `A Shabouquet for ${state.to || 'You'} 🌷`,
            text: `${state.from || 'Someone'} arranged a special digital bouquet for you on Shabouquet!`,
            url: url
          });
        } catch (err) {
          // User cancelled share sheet
        }
      } else {
        copyShareUrl();
      }
    }

    function previewAsRecipient() {
      enterRecipientMode();
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    function enterRecipientMode() {
      isRecipientMode = true;
      document.getElementById('hero-section').classList.add('hidden');
      document.getElementById('canvas-toolbar').classList.add('hidden');
      document.getElementById('builder-controls').classList.add('hidden');
      document.getElementById('recipient-banner').classList.remove('hidden');
      document.getElementById('recipient-actions').classList.remove('hidden');
      document.getElementById('header-badge').textContent = 'Gift Delivery 💌';
      renderFlowers();
    }

    function switchToCreateMode() {
      isRecipientMode = false;
      history.replaceState(null, '', window.location.pathname + window.location.search);
      document.getElementById('hero-section').classList.remove('hidden');
      document.getElementById('canvas-toolbar').classList.remove('hidden');
      document.getElementById('builder-controls').classList.remove('hidden');
      document.getElementById('recipient-banner').classList.add('hidden');
      document.getElementById('recipient-actions').classList.add('hidden');
      document.getElementById('header-badge').textContent = 'Coquette Atelier';

      // Sync inputs with current state
      document.getElementById('input-to').value = state.to;
      document.getElementById('input-from').value = state.from;
      document.getElementById('input-msg').value = state.msg;
      document.getElementById('input-caption').value = state.caption;
      renderAll();
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    /* ==========================================================================
       8. TOAST HELPER
       ========================================================================== */
    let toastTimer;
    function showToast(message, icon = '🎀') {
      const toast = document.getElementById('toast');
      document.getElementById('toast-msg').textContent = message;
      document.getElementById('toast-icon').textContent = icon;

      toast.classList.remove('opacity-0', '-translate-y-2');
      toast.classList.add('opacity-100', 'translate-y-0');

      clearTimeout(toastTimer);
      toastTimer = setTimeout(() => {
        toast.classList.remove('opacity-100', 'translate-y-0');
        toast.classList.add('opacity-0', '-translate-y-2');
      }, 3000);
    }

   /* ==========================================================================
  // Mengubah hasil buket menjadi link & menyalinnya
  function buatLinkBagikan() {
    const url = new URL(window.location.origin + window.location.pathname);

    // Ganti variabel di sebelah kanan sesuai variabel di kode HTML Anda
    url.searchParams.set('bunga', JSON.stringify(dataBungaAnda)); 
    url.searchParams.set('kertas', warnaKertasAnda);
    url.searchParams.set('pesan', teksPesanAnda);

    // Salin otomatis ke clipboard HP/Laptop
    navigator.clipboard.writeText(url.toString()).then(() => {
      alert("Link buket berhasil disalin! Silakan paste untuk mengirim.");
    });
  }

  // Membaca link otomatis saat penerima membuka URL
  window.addEventListener('DOMContentLoaded', () => {
    const params = new URLSearchParams(window.location.search);

    // Cek apakah URL berisi data buket kiriman
    if (params.has('bunga')) {
      // 1. Ambil data dari URL
      const bungaKiriman = JSON.parse(params.get('bunga'));
      const kertasKiriman = params.get('kertas');
      const pesanKiriman = params.get('pesan');

      // 2. Masukkan kembali ke variabel/fungsi tampilan di kode Anda
      dataBungaAnda = bungaKiriman;
      warnaKertasAnda = kertasKiriman;
      teksPesanAnda = pesanKiriman;
      
      // Panggil fungsi update tampilan buket milik Anda di sini
      fungsiRenderBuketAnda(); 

      // 3. Sembunyikan panel tombol edit agar penerima hanya melihat hasilnya
      document.getElementById('id-panel-edit-anda').style.display = 'none';
    }
  });

  </script>
</body>
</html>


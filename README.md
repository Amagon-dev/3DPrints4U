<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>FilamentVault</title>

  <!-- iOS Standalone Web App Meta Tags -->
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
  <meta name="apple-mobile-web-app-title" content="FilamentVault" />
  <meta name="mobile-web-app-capable" content="yes" />
  <meta name="theme-color" content="#F2F2F7" media="(prefers-color-scheme: light)" />
  <meta name="theme-color" content="#000000" media="(prefers-color-scheme: dark)" />

  <!-- App Icons for iOS Home Screen -->
  <link rel="apple-touch-icon" href="https://img.icons8.com/fluency/192/3d-printer.png" />
  <link rel="icon" type="image/png" href="https://img.icons8.com/fluency/192/3d-printer.png" />

  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            ios: {
              blue: '#007AFF',
              green: '#34C759',
              indigo: '#5856D6',
              orange: '#FF9500',
              pink: '#FF2D55',
              purple: '#AF52DE',
              red: '#FF3B30',
              teal: '#5AC8FA',
              yellow: '#FFCC00',
              gray1: '#8E8E93',
              gray6: '#F2F2F7',
              darkBg: '#000000',
              darkCard: '#1C1C1E',
              darkCard2: '#2C2C2E',
            }
          },
          fontFamily: {
            sans: ['-apple-system', 'BlinkMacSystemFont', 'SF Pro Text', 'SF Pro Display', 'Inter', 'Helvetica Neue', 'sans-serif'],
          }
        }
      }
    };
  </script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>
    /* Custom iOS Glassmorphism and scrollbar hiding */
    .ios-glass {
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
    }
    .no-scrollbar::-webkit-scrollbar {
      display: none;
    }
    .no-scrollbar {
      -ms-overflow-style: none;
      scrollbar-width: none;
    }
    /* Pulse laser for scanner */
    @keyframes scanline {
      0% { top: 12%; opacity: 0.8; }
      50% { top: 82%; opacity: 1; }
      100% { top: 12%; opacity: 0.8; }
    }
    .scanner-laser {
      animation: scanline 2.2s ease-in-out infinite;
    }

    /* 3D Prints Just4U Neon Sign Styling */
    .neon-brick-bg {
      background-color: #0d0e12;
      background-image: 
        radial-gradient(ellipse at 50% 35%, rgba(0, 122, 255, 0.22) 0%, rgba(255, 149, 0, 0.12) 50%, rgba(5, 5, 8, 0.95) 100%),
        linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
      background-size: 100% 100%, 28px 28px, 28px 28px;
    }

    .neon-text-blue {
      color: #60a5fa;
      text-shadow: 
        0 0 4px #93c5fd,
        0 0 10px #3b82f6,
        0 0 22px #1d4ed8,
        0 0 40px #1e40af,
        0 4px 6px rgba(0, 0, 0, 0.8);
      filter: drop-shadow(0 4px 10px rgba(0, 102, 255, 0.6));
    }

    .neon-text-orange {
      color: #fed7aa;
      text-shadow: 
        0 0 4px #fdba74,
        0 0 12px #f97316,
        0 0 26px #ea580c,
        0 0 42px #c2410c,
        0 4px 6px rgba(0, 0, 0, 0.8);
      filter: drop-shadow(0 4px 10px rgba(255, 102, 0, 0.65));
    }

    .neon-border-blue {
      box-shadow: 0 0 18px rgba(59, 130, 246, 0.5), inset 0 0 12px rgba(59, 130, 246, 0.3);
      border-color: rgba(96, 165, 250, 0.7);
    }

    @keyframes neonPulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.95; transform: scale(1.008); }
    }
    .neon-pulse-card {
      animation: neonPulse 3.5s ease-in-out infinite;
    }
  </style>
</head>
<body class="bg-[#F2F2F7] dark:bg-black text-[#1C1C1E] dark:text-[#F2F2F7] font-sans antialiased min-h-screen flex justify-center selection:bg-ios-blue selection:text-white pb-20">

  <!-- ============================================== -->
  <!-- COVER / SPLASH SCREEN: 3D PRINTS JUST4U        -->
  <!-- ============================================== -->
  <div id="companySplashScreen" class="fixed inset-0 z-50 neon-brick-bg flex flex-col justify-between items-center text-white p-6 transition-all duration-700 ease-out">
    
    <!-- Top iOS Status bar inside splash screen -->
    <div class="w-full max-w-sm flex justify-between items-center text-xs text-neutral-400 font-semibold pt-1">
      <span id="splashTime">9:41</span>
      <div class="h-3.5 w-16 bg-black/80 rounded-full border border-neutral-700/60"></div>
      <div class="flex items-center space-x-1.5 text-neutral-400">
        <i data-lucide="wifi" class="w-3.5 h-3.5"></i>
        <i data-lucide="battery-full" class="w-4 h-4 text-emerald-400"></i>
      </div>
    </div>

    <!-- Center Glowing Neon Badge & Logo Representation -->
    <div class="w-full max-w-sm flex-1 flex flex-col justify-center items-center my-auto neon-pulse-card">
      
      <!-- Custom Image Logo Container (with fallback to high-fidelity neon art) -->
      <div id="customSplashImageContainer" class="hidden w-full max-w-[290px] mb-4">
        <img id="customSplashImage" class="w-full h-auto rounded-3xl shadow-2xl border border-blue-400/40 object-contain drop-shadow-[0_0_25px_rgba(0,122,255,0.45)]" alt="3D Prints Just4U" />
      </div>

      <!-- High-fidelity 3D Neon Sign Replica -->
      <div id="neonSignGraphic" class="relative w-full max-w-[310px] bg-black/60 rounded-[32px] p-5 border-2 border-blue-500/40 neon-border-blue backdrop-blur-md flex flex-col items-center">
        
        <!-- Top Row: Mini 3D Printer + 3D Text + Spool + Hotend Extruder -->
        <div class="w-full flex items-center justify-between mb-1">
          <!-- Mini Chamber Printer Icon -->
          <div class="w-11 h-11 rounded-xl bg-blue-950/70 border-2 border-blue-400/80 p-1.5 flex flex-col items-center justify-between shadow-[0_0_12px_rgba(59,130,246,0.6)]">
            <div class="w-full flex justify-center">
              <div class="w-2.5 h-1.5 bg-cyan-400 rounded-sm"></div>
            </div>
            <!-- glowing printed cube -->
            <div class="w-4 h-4 rounded border border-orange-400 bg-orange-500/80 shadow-[0_0_10px_#f97316] animate-pulse"></div>
            <div class="w-full h-0.5 bg-blue-400"></div>
          </div>

          <!-- "3D" Large Typography -->
          <div class="text-4xl font-black tracking-tight neon-text-blue font-sans">
            3D
          </div>

          <!-- Spool Graphic -->
          <div class="w-10 h-10 rounded-full border-2 border-blue-400 bg-blue-950 flex items-center justify-center shadow-[0_0_12px_rgba(59,130,246,0.7)]">
            <div class="w-5 h-5 rounded-full border-2 border-orange-400 bg-orange-500/90 shadow-[0_0_8px_#f97316]"></div>
          </div>

          <!-- Extruder / Hotend Nozzle & Extruded Trace -->
          <div class="flex flex-col items-center">
            <div class="w-6 h-7 bg-blue-600 rounded-t-sm border border-blue-300 flex items-end justify-center pb-0.5 shadow-[0_0_10px_rgba(59,130,246,0.8)]">
              <div class="w-3 h-2 bg-amber-400 rounded-b-sm"></div>
            </div>
            <div class="w-2 h-1 bg-amber-300 transform rotate-45 -mt-0.5 shadow-[0_0_6px_#f59e0b]"></div>
          </div>
        </div>

        <!-- "Prints" Title -->
        <div class="relative w-full text-center mt-1">
          <h2 class="text-4xl sm:text-5xl font-black tracking-wide neon-text-blue uppercase font-sans">
            Prints
          </h2>
        </div>

        <!-- "Just4U" Neon Script / Tube -->
        <div class="relative w-full text-center mt-0.5">
          <h3 class="text-4xl sm:text-5xl font-black tracking-tight neon-text-orange font-sans">
            Just4U
          </h3>
        </div>

        <!-- Curved Blue Filament Arrow -->
        <div class="w-full mt-2 px-2 flex items-center justify-center">
          <svg viewBox="0 0 200 24" class="w-44 h-6 text-blue-400 drop-shadow-[0_0_6px_#60a5fa]">
            <path d="M 10 18 Q 100 4 185 14" fill="none" stroke="currentColor" stroke-width="3.5" stroke-linecap="round" />
            <polygon points="185,9 198,15 183,21" fill="currentColor" />
          </svg>
        </div>

        <!-- Subtitle Pill: "Custom Creations, Made for You" -->
        <div class="mt-2.5 px-4 py-1 rounded-full border border-blue-400/60 bg-blue-950/60 shadow-[0_0_14px_rgba(59,130,246,0.4)]">
          <p class="text-[10px] font-bold tracking-wider text-blue-200 uppercase">
            Custom Creations, Made for You
          </p>
        </div>
      </div>

      <!-- App Subtitle -->
      <div class="text-center mt-5 space-y-1">
        <p class="text-xs uppercase tracking-widest text-neutral-400 font-semibold">Filament & Stock Inventory Manager</p>
        <p class="text-[11px] text-neutral-500">Back-stock tracking, batch rolls & label OCR</p>
      </div>
    </div>

    <!-- Bottom Actions: Enter App & Custom Logo Uploader -->
    <div class="w-full max-w-sm space-y-3 pb-4">
      <button onclick="dismissSplashScreen()" class="w-full py-3.5 px-6 rounded-2xl bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-500 hover:to-indigo-500 text-white font-bold text-sm shadow-[0_0_20px_rgba(37,99,235,0.5)] active:scale-98 transition-all flex items-center justify-center space-x-2">
        <span>Open Filament Inventory</span>
        <i data-lucide="arrow-right" class="w-4 h-4"></i>
      </button>

      <div class="flex items-center justify-between pt-1 text-[11px] text-neutral-400 px-1">
        <label class="cursor-pointer hover:text-blue-400 transition flex items-center gap-1.5">
          <i data-lucide="image-plus" class="w-3.5 h-3.5 text-blue-400"></i>
          <span>Use photo file for cover</span>
          <input type="file" accept="image/*" class="hidden" onchange="loadCoverScreenPhoto(this)" />
        </label>
        <button onclick="dismissSplashScreen()" class="hover:text-neutral-200">Skip directly &rarr;</button>
      </div>
    </div>
  </div>

  <!-- Mobile View Container -->
  <div class="w-full max-w-md min-h-screen flex flex-col bg-[#F2F2F7] dark:bg-black relative shadow-2xl">

    <!-- Top iOS Status Bar Simulation -->
    <header class="sticky top-0 z-40 bg-[#F2F2F7]/85 dark:bg-black/80 ios-glass border-b border-black/5 dark:border-white/10 transition-colors">
      <div class="px-5 pt-3 pb-2 flex justify-between items-center text-xs font-semibold">
        <span id="currentTime" class="tracking-tight text-neutral-800 dark:text-neutral-200">9:41</span>
        <!-- Simulated Dynamic Island -->
        <div class="h-4 w-20 bg-black dark:bg-neutral-800 rounded-full flex items-center justify-center space-x-1 px-1">
          <div class="w-2 h-2 rounded-full bg-neutral-900 border border-neutral-700"></div>
          <div class="w-1.5 h-1.5 rounded-full bg-blue-900/40"></div>
        </div>
        <div class="flex items-center space-x-1.5 text-neutral-700 dark:text-neutral-300">
          <i data-lucide="signal" class="w-3.5 h-3.5"></i>
          <i data-lucide="wifi" class="w-3.5 h-3.5"></i>
          <i data-lucide="battery-full" class="w-4 h-4 text-emerald-500"></i>
        </div>
      </div>

      <!-- App Bar & Quick Actions with 3D Prints Just4U Branding -->
      <div class="px-5 py-3 flex items-center justify-between">
        <div class="cursor-pointer" onclick="reopenSplashScreen()" title="Tap to view 3D Prints Just4U cover screen">
          <div class="flex items-center space-x-1 text-ios-blue text-[11px] font-bold uppercase tracking-wider">
            <span>3D Prints Just4U</span>
            <i data-lucide="sparkles" class="w-3 h-3 text-amber-500"></i>
          </div>
          <h1 class="text-2xl font-extrabold tracking-tight text-neutral-900 dark:text-white">Filament Stock</h1>
        </div>
        <div class="flex items-center space-x-2">
          <!-- View Cover Screen Button -->
          <button onclick="reopenSplashScreen()" class="w-9 h-9 rounded-full bg-white dark:bg-[#1C1C1E] flex items-center justify-center shadow-sm border border-black/5 dark:border-white/10 text-neutral-700 dark:text-neutral-300 active:scale-95 transition-transform" title="Company Cover Screen">
            <i data-lucide="layout-grid" class="w-4 h-4 text-ios-blue"></i>
          </button>
          <!-- Dark Mode Toggle -->
          <button id="themeToggleBtn" aria-label="Toggle Theme" class="w-9 h-9 rounded-full bg-white dark:bg-[#1C1C1E] flex items-center justify-center shadow-sm border border-black/5 dark:border-white/10 text-neutral-700 dark:text-neutral-300 active:scale-95 transition-transform">
            <i data-lucide="moon" class="w-4 h-4 dark:hidden"></i>
            <i data-lucide="sun" class="w-4 h-4 hidden dark:block text-amber-400"></i>
          </button>
          <!-- Scan Label Quick Action -->
          <button onclick="openScannerModal()" aria-label="Scan Spool Label" class="w-9 h-9 rounded-full bg-white dark:bg-[#1C1C1E] flex items-center justify-center shadow-sm border border-black/5 dark:border-white/10 text-ios-blue active:scale-95 transition-transform" title="Scan Spool Label">
            <i data-lucide="scan" class="w-4 h-4"></i>
          </button>
          <!-- Add Filament Button -->
          <button onclick="openSpoolModal()" class="flex items-center space-x-1 bg-ios-blue hover:bg-blue-600 text-white px-3.5 py-1.5 rounded-full text-xs font-semibold shadow-sm active:scale-95 transition-transform">
            <i data-lucide="plus" class="w-4 h-4"></i>
            <span>Add Roll</span>
          </button>
        </div>
      </div>

      <!-- Live Search & Material Filter Tabs -->
      <div class="px-5 pb-3 space-y-2.5">
        <!-- Search Input -->
        <div class="relative">
          <i data-lucide="search" class="w-4 h-4 absolute left-3 top-2.5 text-neutral-400"></i>
          <input id="searchInput" type="text" placeholder="Search brand, material, color, or shelf..." class="w-full pl-9 pr-8 py-2 bg-[#E5E5EA] dark:bg-[#1C1C1E] rounded-xl text-xs text-neutral-900 dark:text-white placeholder-neutral-500 focus:outline-none focus:ring-2 focus:ring-ios-blue transition-all" />
          <button id="clearSearchBtn" class="hidden absolute right-2.5 top-2.5 text-neutral-400 hover:text-neutral-600">
            <i data-lucide="x-circle" class="w-4 h-4"></i>
          </button>
        </div>

        <!-- Material Filter Pills -->
        <div class="flex space-x-1.5 overflow-x-auto no-scrollbar py-0.5 text-xs" id="filterPills">
          <button data-filter="ALL" class="filter-btn px-3 py-1 rounded-full font-medium bg-neutral-900 dark:bg-white text-white dark:text-neutral-900 shadow-sm transition">All</button>
          <button data-filter="PLA" class="filter-btn px-3 py-1 rounded-full font-medium bg-white dark:bg-[#1C1C1E] text-neutral-600 dark:text-neutral-300 border border-black/5 dark:border-white/5 transition">PLA</button>
          <button data-filter="PETG" class="filter-btn px-3 py-1 rounded-full font-medium bg-white dark:bg-[#1C1C1E] text-neutral-600 dark:text-neutral-300 border border-black/5 dark:border-white/5 transition">PETG</button>
          <button data-filter="ABS" class="filter-btn px-3 py-1 rounded-full font-medium bg-white dark:bg-[#1C1C1E] text-neutral-600 dark:text-neutral-300 border border-black/5 dark:border-white/5 transition">ABS/ASA</button>
          <button data-filter="TPU" class="filter-btn px-3 py-1 rounded-full font-medium bg-white dark:bg-[#1C1C1E] text-neutral-600 dark:text-neutral-300 border border-black/5 dark:border-white/5 transition">TPU</button>
          <button data-filter="SPECIALTY" class="filter-btn px-3 py-1 rounded-full font-medium bg-white dark:bg-[#1C1C1E] text-neutral-600 dark:text-neutral-300 border border-black/5 dark:border-white/5 transition">Specialty</button>
        </div>
      </div>
    </header>

    <!-- Main Stock Content -->
    <main class="flex-1 px-5 pt-3 pb-8 space-y-4">

      <!-- Back-stock Metric Cards -->
      <section class="grid grid-cols-3 gap-2.5">
        <div class="bg-white dark:bg-[#1C1C1E] rounded-2xl p-3 border border-black/5 dark:border-white/5 shadow-sm text-center">
          <span class="text-[10px] uppercase font-bold text-neutral-400">Total Rolls</span>
          <p id="statTotalRolls" class="text-xl font-black text-neutral-900 dark:text-white mt-0.5">0</p>
        </div>
        <div class="bg-white dark:bg-[#1C1C1E] rounded-2xl p-3 border border-black/5 dark:border-white/5 shadow-sm text-center">
          <span class="text-[10px] uppercase font-bold text-neutral-400">Est. Weight</span>
          <p id="statTotalWeight" class="text-xl font-black text-ios-blue dark:text-ios-teal mt-0.5">0.0 kg</p>
        </div>
        <div class="bg-white dark:bg-[#1C1C1E] rounded-2xl p-3 border border-black/5 dark:border-white/5 shadow-sm text-center">
          <span class="text-[10px] uppercase font-bold text-neutral-400">1/4 Low Rolls</span>
          <p id="statLowRolls" class="text-xl font-black text-ios-orange mt-0.5">0</p>
        </div>
      </section>

      <!-- Filament Cards List -->
      <section id="inventoryContainer" class="space-y-3">
        <!-- Spool cards injected dynamically via JS -->
      </section>

      <!-- Empty State View -->
      <div id="emptyState" class="hidden text-center py-16 px-4">
        <div class="w-16 h-16 bg-neutral-200 dark:bg-neutral-800 rounded-full flex items-center justify-center mx-auto mb-3 text-neutral-400">
          <i data-lucide="cylinder" class="w-8 h-8"></i>
        </div>
        <h3 class="text-base font-bold text-neutral-800 dark:text-neutral-200">No Filaments Found</h3>
        <p class="text-xs text-neutral-500 mt-1 max-w-xs mx-auto">Tap "Add Roll" to register spools or use the OCR label scanner to auto-fill details.</p>
        <button onclick="openSpoolModal()" class="mt-4 px-4 py-2 bg-ios-blue text-white rounded-full text-xs font-semibold shadow-sm inline-flex items-center gap-1.5">
          <i data-lucide="plus" class="w-4 h-4"></i> Add First Roll
        </button>
      </div>

    </main>

    <!-- Bottom Mobile Toolbar -->
    <nav class="fixed bottom-0 left-0 right-0 max-w-md mx-auto bg-white/80 dark:bg-black/85 ios-glass border-t border-black/5 dark:border-white/10 px-6 py-2.5 flex justify-around items-center text-[10px] font-semibold z-30">
      <button class="flex flex-col items-center text-ios-blue">
        <i data-lucide="layers" class="w-5 h-5 mb-0.5"></i>
        <span>Stock</span>
      </button>
      <button onclick="openScannerModal()" class="flex flex-col items-center text-neutral-500 hover:text-ios-blue transition">
        <i data-lucide="camera" class="w-5 h-5 mb-0.5"></i>
        <span>Scan Label</span>
      </button>
      <button onclick="openSpoolModal()" class="flex flex-col items-center text-neutral-500 hover:text-ios-blue transition">
        <i data-lucide="plus-circle" class="w-5 h-5 mb-0.5"></i>
        <span>New Roll</span>
      </button>
      <button onclick="openInstallGuideModal()" class="flex flex-col items-center text-neutral-500 hover:text-ios-blue transition" title="Add to iPhone">
        <i data-lucide="smartphone" class="w-5 h-5 mb-0.5"></i>
        <span>Add to iOS</span>
      </button>
    </nav>

    <!-- ============================================== -->
    <!-- MODAL: NATIVE IOS INSTALLATION GUIDE SHEET     -->
    <!-- ============================================== -->
    <div id="installGuideModal" class="fixed inset-0 z-50 bg-black/60 backdrop-blur-sm hidden flex items-end justify-center transition-opacity">
      <div id="installGuideSheet" class="w-full max-w-md bg-[#F2F2F7] dark:bg-[#1C1C1E] rounded-t-[32px] max-h-[90vh] flex flex-col shadow-2xl transform transition-transform duration-300 translate-y-full">
        <!-- Sheet Header -->
        <div class="px-5 pt-3 pb-3 border-b border-black/5 dark:border-white/10 bg-white/60 dark:bg-[#2C2C2E]/60 ios-glass rounded-t-[32px]">
          <div class="w-10 h-1 bg-neutral-300 dark:bg-neutral-600 rounded-full mx-auto mb-2"></div>
          <div class="flex items-center justify-between">
            <span class="text-xs font-bold text-neutral-400 uppercase tracking-wider">PWA Setup</span>
            <h2 class="text-sm font-bold text-neutral-900 dark:text-white">Add to iPhone Home Screen</h2>
            <button onclick="closeInstallGuideModal()" class="text-ios-blue text-sm font-bold">Done</button>
          </div>
        </div>

        <div class="p-5 overflow-y-auto space-y-4 text-xs flex-1">
          <div class="flex items-center space-x-3.5 bg-white dark:bg-[#2C2C2E] p-3.5 rounded-2xl border border-black/5 dark:border-white/5 shadow-sm">
            <img src="https://img.icons8.com/fluency/96/3d-printer.png" class="w-12 h-12 rounded-xl shadow-sm" alt="Icon" />
            <div>
              <h3 class="text-sm font-bold text-neutral-900 dark:text-white">FilamentVault Standalone App</h3>
              <p class="text-[11px] text-neutral-500 dark:text-neutral-400 mt-0.5">Runs full-screen with offline caching, local data persistence, and camera scanner access.</p>
            </div>
          </div>

          <div class="bg-white dark:bg-[#2C2C2E] rounded-2xl p-4 space-y-3.5 border border-black/5 dark:border-white/5 shadow-sm">
            <h4 class="font-bold text-xs text-neutral-900 dark:text-white uppercase tracking-wider">3 Quick Steps in Safari</h4>

            <!-- Step 1 -->
            <div class="flex items-start space-x-3">
              <span class="w-6 h-6 rounded-full bg-ios-blue text-white flex items-center justify-center font-bold text-[11px] flex-shrink-0 mt-0.5">1</span>
              <div>
                <p class="font-semibold text-neutral-900 dark:text-white">Open in Safari</p>
                <p class="text-neutral-500 dark:text-neutral-400 mt-0.5">Host or open this link in the native Apple Safari browser on your iPhone.</p>
              </div>
            </div>

            <!-- Step 2 -->
            <div class="flex items-start space-x-3">
              <span class="w-6 h-6 rounded-full bg-ios-blue text-white flex items-center justify-center font-bold text-[11px] flex-shrink-0 mt-0.5">2</span>
              <div>
                <p class="font-semibold text-neutral-900 dark:text-white">Tap the Share Icon</p>
                <p class="text-neutral-500 dark:text-neutral-400 mt-0.5">Tap the blue <strong>Share</strong> button <i data-lucide="share" class="w-3.5 h-3.5 inline text-ios-blue"></i> in the bottom Safari toolbar.</p>
              </div>
            </div>

            <!-- Step 3 -->
            <div class="flex items-start space-x-3">
              <span class="w-6 h-6 rounded-full bg-ios-blue text-white flex items-center justify-center font-bold text-[11px] flex-shrink-0 mt-0.5">3</span>
              <div>
                <p class="font-semibold text-neutral-900 dark:text-white">Select "Add to Home Screen"</p>
                <p class="text-neutral-500 dark:text-neutral-400 mt-0.5">Scroll down in the action sheet and tap <strong>"Add to Home Screen"</strong> <i data-lucide="plus-square" class="w-3.5 h-3.5 inline text-neutral-700 dark:text-neutral-300"></i>, then tap <strong>Add</strong> at top right.</p>
              </div>
            </div>
          </div>

          <div class="bg-blue-50 dark:bg-blue-950/40 border border-blue-200 dark:border-blue-900/50 rounded-2xl p-3.5 text-blue-800 dark:text-blue-300 space-y-1">
            <p class="font-bold flex items-center gap-1.5"><i data-lucide="check-circle" class="w-4 h-4"></i> Standalone Experience</p>
            <p class="text-[11px] leading-relaxed">Once added to your Home Screen, Safari toolbars disappear, opening FilamentVault in complete native full-screen mode just like an app installed from the App Store.</p>
          </div>
        </div>
      </div>
    </div>
        </div>

        <!-- Sheet Body Form -->
        <div class="p-5 overflow-y-auto space-y-4 text-xs flex-1">
          <input type="hidden" id="editFilamentId" value="" />

          <!-- Group 1: Brand & Custom Name -->
          <div class="bg-white dark:bg-[#2C2C2E] rounded-2xl p-3.5 space-y-3 shadow-sm border border-black/5 dark:border-white/5">
            <div>
              <label class="block text-neutral-500 dark:text-neutral-400 text-[11px] uppercase tracking-wider mb-1">Manufacturer Brand</label>
              <select id="brandSelect" class="w-full bg-[#F2F2F7] dark:bg-[#1C1C1E] rounded-xl px-3 py-2 text-neutral-900 dark:text-white font-medium focus:ring-2 focus:ring-ios-blue">
                <option value="Bambu Lab">Bambu Lab</option>
                <option value="eSUN">eSUN</option>
                <option value="Polymaker">Polymaker</option>
                <option value="Sunlu">Sunlu</option>
                <option value="Hatchbox">Hatchbox</option>
                <option value="Overture">Overture</option>
                <option value="Elegoo">Elegoo</option>
                <option value="Inland">Inland (Micro Center)</option>
                <option value="Prusament">Prusament</option>
                <option value="ANYCUBIC">ANYCUBIC</option>
                <option value="Creality">Creality</option>
                <option value="MatterHackers">MatterHackers</option>
                <option value="Other">Other (Custom Brand)</option>
              </select>
            </div>

            <!-- Custom Brand Field (Conditionally shown when 'Other' selected) -->
            <div id="customBrandContainer" class="hidden">
              <label class="block text-neutral-500 dark:text-neutral-400 text-[11px] uppercase tracking-wider mb-1">Custom Brand Name</label>
              <input id="customBrandInput" type="text" placeholder="Type custom brand name (e.g. Atomic, Proto-pasta)" class="w-full bg-[#F2F2F7] dark:bg-[#1C1C1E] rounded-xl px-3 py-2 text-neutral-900 dark:text-white focus:ring-2 focus:ring-ios-blue" />
            </div>

            <!-- Material Type Selector -->
            <div>
              <label class="block text-neutral-500 dark:text-neutral-400 text-[11px] uppercase tracking-wider mb-1">Filament Material</label>
              <div class="grid grid-cols-4 gap-1.5" id="materialGrid">
                <button type="button" data-mat="PLA" class="mat-btn py-1.5 rounded-lg text-center font-semibold bg-ios-blue text-white">PLA</button>
                <button type="button" data-mat="PETG" class="mat-btn py-1.5 rounded-lg text-center bg-[#F2F2F7] dark:bg-[#1C1C1E] text-neutral-800 dark:text-neutral-200">PETG</button>
                <button type="button" data-mat="ABS" class="mat-btn py-1.5 rounded-lg text-center bg-[#F2F2F7] dark:bg-[#1C1C1E] text-neutral-800 dark:text-neutral-200">ABS</button>
                <button type="button" data-mat="ASA" class="mat-btn py-1.5 rounded-lg text-center bg-[#F2F2F7] dark:bg-[#1C1C1E] text-neutral-800 dark:text-neutral-200">ASA</button>
                <button type="button" data-mat="TPU" class="mat-btn py-1.5 rounded-lg text-center bg-[#F2F2F7] dark:bg-[#1C1C1E] text-neutral-800 dark:text-neutral-200">TPU 95A</button>
                <button type="button" data-mat="PC" class="mat-btn py-1.5 rounded-lg text-center bg-[#F2F2F7] dark:bg-[#1C1C1E] text-neutral-800 dark:text-neutral-200">PC</button>
                <button type="button" data-mat="PA-CF" class="mat-btn py-1.5 rounded-lg text-center bg-[#F2F2F7] dark:bg-[#1C1C1E] text-neutral-800 dark:text-neutral-200">Nylon-CF</button>
                <button type="button" data-mat="Support" class="mat-btn py-1.5 rounded-lg text-center bg-[#F2F2F7] dark:bg-[#1C1C1E] text-neutral-800 dark:text-neutral-200">PVA/Supp</button>
              </div>
            </div>

            <!-- Finish / Style Selector -->
            <div>
              <label class="block text-neutral-500 dark:text-neutral-400 text-[11px] uppercase tracking-wider mb-1">Finish & Style</label>
              <select id="finishSelect" class="w-full bg-[#F2F2F7] dark:bg-[#1C1C1E] rounded-xl px-3 py-2 text-neutral-900 dark:text-white font-medium focus:ring-2 focus:ring-ios-blue">
                <option value="Basic / Standard">Basic / Standard Gloss</option>
                <option value="Matte">Matte</option>
                <option value="Silk">Silk / Metallic Sheen</option>
                <option value="Dual-Color Silk">Dual-Color Coextrusion</option>
                <option value="Tri-Color Silk">Tri-Color Coextrusion</option>
                <option value="Glitter / Sparkle">Glitter / Sparkle / Galaxy</option>
                <option value="Translucent">Translucent / Clear</option>
                <option value="Glow-in-the-Dark">Glow in the Dark</option>
                <option value="Carbon Fiber Filled">Carbon Fiber (CF)</option>
                <option value="Wood Composite">Wood Composite</option>
                <option value="Marble">Marble / Speckled</option>
              </select>
            </div>
          </div>

          <!-- Group 2: Color Identity, Swatches & Specific Color Variations -->
          <div class="bg-white dark:bg-[#2C2C2E] rounded-2xl p-3.5 space-y-3.5 shadow-sm border border-black/5 dark:border-white/5">
            <div class="flex items-center justify-between">
              <label class="text-neutral-500 dark:text-neutral-400 text-[11px] uppercase tracking-wider">Color Configuration</label>
              <span id="colorHexPreviewText" class="font-mono text-[11px] text-neutral-400">#007AFF</span>
            </div>

            <!-- Segmented Switcher: Single Solid Color vs Color Variation -->
            <div class="grid grid-cols-2 p-1 bg-[#E5E5EA] dark:bg-[#1C1C1E] rounded-xl text-center" id="colorModeSegments">
              <button type="button" id="colorModeSolidBtn" onclick="setColorMode('solid')" class="py-1.5 rounded-lg text-xs font-semibold bg-white dark:bg-[#2C2C2E] shadow-sm text-neutral-900 dark:text-white transition">Single Color</button>
              <button type="button" id="colorModeVariantBtn" onclick="setColorMode('variant')" class="py-1.5 rounded-lg text-xs font-semibold text-neutral-600 dark:text-neutral-400 transition">Color Variation</button>
            </div>

            <!-- Primary Color & Shade Name -->
            <div class="flex items-center space-x-3">
              <input id="colorHexInput" type="color" value="#007AFF" class="w-10 h-10 rounded-xl cursor-pointer border-0 bg-transparent p-0 flex-shrink-0" />
              <input id="colorNameInput" type="text" placeholder="e.g. Jade White, Galaxy Black, Hot Pink" class="flex-1 bg-[#F2F2F7] dark:bg-[#1C1C1E] rounded-xl px-3 py-2 text-neutral-900 dark:text-white focus:ring-2 focus:ring-ios-blue font-medium" />
            </div>

            <!-- Color Variation Expansion Block -->
            <div id="colorVariationContainer" class="hidden space-y-3 p-3 bg-[#F2F2F7] dark:bg-[#1C1C1E] rounded-xl border border-black/5 dark:border-white/5">
              <div class="flex items-center justify-between">
                <span class="text-[11px] font-bold text-neutral-700 dark:text-neutral-300">Variation Type & Secondary Colors</span>
                <span class="text-[10px] text-ios-blue dark:text-ios-teal font-medium">Multi-Tone</span>
              </div>

              <!-- Variation Type Picker -->
              <div>
                <label class="block text-neutral-500 dark:text-neutral-400 text-[10px] uppercase tracking-wider mb-1">Variation Style</label>
                <select id="variationTypeSelect" onchange="handleVariationTypeChange()" class="w-full bg-white dark:bg-[#2C2C2E] rounded-xl px-2.5 py-1.5 text-xs text-neutral-900 dark:text-white font-medium focus:ring-2 focus:ring-ios-blue">
                  <option value="Dual-Color Dichromatic">Dual-Color / Dichromatic (Split Silk)</option>
                  <option value="Tri-Color Trichromatic">Tri-Color / Trichromatic</option>
                  <option value="Gradient / Rainbow">Gradient / Continuous Rainbow</option>
                  <option value="Transition / Color-Change">Temperature / UV Reactive Transition</option>
                  <option value="Speckled / Flake">Speckled Flake / Fleck Mix</option>
                  <option value="Custom Blend">Custom Color Variation</option>
                </select>
              </div>

              <!-- Secondary & Tertiary Swatch Pickers -->
              <div class="grid grid-cols-2 gap-2.5 pt-1">
                <div>
                  <label class="block text-neutral-500 dark:text-neutral-400 text-[10px] uppercase tracking-wider mb-1">Color 2 (Accent / Blend)</label>
                  <div class="flex items-center space-x-2">
                    <input id="colorHex2Input" type="color" value="#FF2D55" class="w-8 h-8 rounded-lg cursor-pointer border-0 bg-transparent p-0 flex-shrink-0" />
                    <input id="color2NameInput" type="text" placeholder="e.g. Gold" class="w-full bg-white dark:bg-[#2C2C2E] rounded-lg px-2 py-1 text-xs text-neutral-900 dark:text-white focus:ring-2 focus:ring-ios-blue font-medium" />
                  </div>
                </div>
                <div id="tertiaryColorContainer">
                  <label class="block text-neutral-500 dark:text-neutral-400 text-[10px] uppercase tracking-wider mb-1">Color 3 (Optional)</label>
                  <div class="flex items-center space-x-2">
                    <input id="colorHex3Input" type="color" value="#FFCC00" class="w-8 h-8 rounded-lg cursor-pointer border-0 bg-transparent p-0 flex-shrink-0" />
                    <input id="color3NameInput" type="text" placeholder="e.g. Copper" class="w-full bg-white dark:bg-[#2C2C2E] rounded-lg px-2 py-1 text-xs text-neutral-900 dark:text-white focus:ring-2 focus:ring-ios-blue font-medium" />
                  </div>
                </div>
              </div>

              <!-- Specific Variation Details / Pattern description -->
              <div>
                <label class="block text-neutral-500 dark:text-neutral-400 text-[10px] uppercase tracking-wider mb-1">Variation Pattern Details</label>
                <input id="variationPatternInput" type="text" placeholder="e.g. Gold/Rose/Blue split, 5-meter gradient cycle, Blue to Neon Green at 31°C" class="w-full bg-white dark:bg-[#2C2C2E] rounded-xl px-2.5 py-1.5 text-xs text-neutral-900 dark:text-white focus:ring-2 focus:ring-ios-blue" />
              </div>

              <!-- Quick Presets for Popular Variations -->
              <div>
                <span class="block text-[10px] text-neutral-400 mb-1">Quick Variation Presets:</span>
                <div class="flex items-center space-x-1.5 overflow-x-auto no-scrollbar py-0.5">
                  <button type="button" onclick="applyVariationPreset('Dual-Color Dichromatic', 'Quantum Dual Silk', '#007AFF', '#FF2D55', 'Cobalt Blue', 'Magenta Pink', '50/50 Coextrusion')" class="px-2.5 py-1 rounded-full text-[10px] font-medium bg-white dark:bg-[#2C2C2E] border border-black/5 dark:border-white/10 flex-shrink-0 hover:border-ios-blue">Blue/Magenta Silk</button>
                  <button type="button" onclick="applyVariationPreset('Tri-Color Trichromatic', 'Trio Silk Magic', '#FFD700', '#FF3B30', 'Gold', 'Crimson', 'Gold / Crimson / Blue', '#007AFF', 'Blue')" class="px-2.5 py-1 rounded-full text-[10px] font-medium bg-white dark:bg-[#2C2C2E] border border-black/5 dark:border-white/10 flex-shrink-0 hover:border-ios-blue">Gold/Crimson/Blue</button>
                  <button type="button" onclick="applyVariationPreset('Gradient / Rainbow', 'Pastel Rainbow', '#FFB3BA', '#BAFFC9', 'Pastel Rose', 'Mint Green', 'Gentle 10-meter pastel gradient shift', '#BAE1FF', 'Baby Blue')" class="px-2.5 py-1 rounded-full text-[10px] font-medium bg-white dark:bg-[#2C2C2E] border border-black/5 dark:border-white/10 flex-shrink-0 hover:border-ios-blue">Pastel Rainbow</button>
                  <button type="button" onclick="applyVariationPreset('Transition / Color-Change', 'Thermochromic Heat Shift', '#111111', '#00FFFF', 'Pitch Black', 'Neon Cyan', 'Changes black to cyan at 31°C')" class="px-2.5 py-1 rounded-full text-[10px] font-medium bg-white dark:bg-[#2C2C2E] border border-black/5 dark:border-white/10 flex-shrink-0 hover:border-ios-blue">Thermochromic</button>
                </div>
              </div>
            </div>

            <!-- Quick Popular 3D Printing Color Swatches -->
            <div>
              <span class="block text-[10px] text-neutral-400 mb-1.5">Common Swatches:</span>
              <div class="flex items-center space-x-2 overflow-x-auto no-scrollbar py-1" id="swatchList">
    <!-- APPLICATION LOGIC -->
    <script>
      // Initial Seed Data
      const DEFAULT_FILAMENTS = [
        {
          id: 'spool_1',
          brand: 'Bambu Lab',
          customBrand: '',
          material: 'PLA',
          finish: 'Basic / Standard',
          colorMode: 'solid',
          colorHex: '#34C759',
          colorName: 'Bambu Green',
          variationType: '',
          colorHex2: '',
          color2Name: '',
          colorHex3: '',
          color3Name: '',
          variationPattern: '',
          spoolWeight: 1000,
          amountRemaining: 1.0, // Full
          quantity: 2,
          location: 'AMS 1 (Slot 1)',
          notes: 'Official Bambu RFID Spool, 220C',
          updatedAt: Date.now()
        },
        {
          id: 'spool_2',
          brand: 'Polymaker',
          customBrand: '',
          material: 'PETG',
          finish: 'Translucent',
          colorMode: 'solid',
          colorHex: '#007AFF',
          colorName: 'Translucent Cyan',
          variationType: '',
          colorHex2: '',
          color2Name: '',
          colorHex3: '',
          color3Name: '',
          variationPattern: '',
          spoolWeight: 1000,
          amountRemaining: 0.75, // 3/4
          quantity: 1,
          location: 'DryBox Shelf 2',
          notes: '240C Nozzle, 75C Bed',
          updatedAt: Date.now() - 100000
        },
        {
          id: 'spool_3',
          brand: 'Atomic Filament',
          customBrand: 'Atomic Filament',
          material: 'ABS',
          finish: 'Matte',
          colorMode: 'solid',
          colorHex: '#1C1C1E',
          colorName: 'Carbon Black',
          variationType: '',
          colorHex2: '',
          color2Name: '',
          colorHex3: '',
          color3Name: '',
          variationPattern: '',
          spoolWeight: 1000,
          amountRemaining: 0.5, // 1/2
          quantity: 3,
          location: 'Voron Cabinet',
          notes: 'Enclosure needed, 255C',
          updatedAt: Date.now() - 200000
        },
        {
          id: 'spool_4',
          brand: 'ERYONE',
          customBrand: 'ERYONE',
          material: 'PLA',
          finish: 'Dual-Color Silk',
          colorMode: 'variant',
          colorHex: '#007AFF',
          colorName: 'Silk Dual Blue/Magenta',
          variationType: 'Dual-Color Dichromatic',
          colorHex2: '#FF2D55',
          color2Name: 'Magenta',
          colorHex3: '',
          color3Name: '',
          variationPattern: '50/50 Dual Silk Split',
          spoolWeight: 1000,
          amountRemaining: 0.75,
          quantity: 1,
          location: 'AMS 2 (Slot 3)',
          notes: 'Rotate model 45° for best color split',
          updatedAt: Date.now() - 250000
        },
        {
          id: 'spool_5',
          brand: 'Overture',
          customBrand: '',
          material: 'TPU',
          finish: 'Basic / Standard',
          colorMode: 'solid',
          colorHex: '#FF9500',
          colorName: 'High-Vis Orange',
          variationType: '',
          colorHex2: '',
          color2Name: '',
          colorHex3: '',
          color3Name: '',
          variationPattern: '',
          spoolWeight: 1000,
          amountRemaining: 0.25, // 1/4
          quantity: 1,
          location: 'DryBox Shelf 1',
          notes: 'Direct drive only, slow 25mm/s',
          updatedAt: Date.now() - 300000
        }
      ];

      // Local Storage State
      let inventory = [];
      let currentFilter = 'ALL';
      let searchQuery = '';

      // Active Modal Form State
      let activeSelectedMaterial = 'PLA';
      let activeAmountRemaining = 1.0; // 1.0 = Full, 0.75 = 3/4, 0.5 = 1/2, 0.25 = 1/4
      let activeQuantity = 1;
      let activeColorMode = 'solid'; // 'solid' or 'variant'

      // DOM Elements
      const inventoryContainer = document.getElementById('inventoryContainer');
      const emptyState = document.getElementById('emptyState');
      const searchInput = document.getElementById('searchInput');
      const clearSearchBtn = document.getElementById('clearSearchBtn');
      const brandSelect = document.getElementById('brandSelect');
      const customBrandContainer = document.getElementById('customBrandContainer');
      const customBrandInput = document.getElementById('customBrandInput');
      const finishSelect = document.getElementById('finishSelect');
      const colorHexInput = document.getElementById('colorHexInput');
      const colorNameInput = document.getElementById('colorNameInput');
      const colorHexPreviewText = document.getElementById('colorHexPreviewText');
      const colorModeSolidBtn = document.getElementById('colorModeSolidBtn');
      const colorModeVariantBtn = document.getElementById('colorModeVariantBtn');
      const colorVariationContainer = document.getElementById('colorVariationContainer');
      const variationTypeSelect = document.getElementById('variationTypeSelect');
      const colorHex2Input = document.getElementById('colorHex2Input');
      const color2NameInput = document.getElementById('color2NameInput');
      const tertiaryColorContainer = document.getElementById('tertiaryColorContainer');
      const colorHex3Input = document.getElementById('colorHex3Input');
      const color3NameInput = document.getElementById('color3NameInput');
      const variationPatternInput = document.getElementById('variationPatternInput');
      const spoolWeightSelect = document.getElementById('spoolWeightSelect');
      const locationInput = document.getElementById('locationInput');
      const notesInput = document.getElementById('notesInput');
      const qtyDisplay = document.getElementById('qtyDisplay');
      const levelLabelBadge = document.getElementById('levelLabelBadge');
      const editFilamentId = document.getElementById('editFilamentId');
      const modalTitle = document.getElementById('modalTitle');
      const deleteBtnContainer = document.getElementById('deleteBtnContainer');
      const spoolModal = document.getElementById('spoolModal');
      const spoolModalSheet = document.getElementById('spoolModalSheet');
      const scannerModal = document.getElementById('scannerModal');

      // Stats Elements
      const statTotalRolls = document.getElementById('statTotalRolls');
      const statTotalWeight = document.getElementById('statTotalWeight');
      const statLowRolls = document.getElementById('statLowRolls');

      // Initialize Application
      window.addEventListener('DOMContentLoaded', () => {
        loadFromLocalStorage();
        updateClock();
        setInterval(updateClock, 10000);
        setupEventListeners();
        checkSavedCoverPhoto();
        renderInventory();
        lucide.createIcons();
      });

      // Cover Screen Controls
      function dismissSplashScreen() {
        const splash = document.getElementById('companySplashScreen');
        splash.classList.add('opacity-0', 'pointer-events-none', '-translate-y-4');
        setTimeout(() => {
          splash.classList.add('hidden');
        }, 600);
      }

      function reopenSplashScreen() {
        const splash = document.getElementById('companySplashScreen');
        splash.classList.remove('hidden');
        setTimeout(() => {
          splash.classList.remove('opacity-0', 'pointer-events-none', '-translate-y-4');
        }, 10);
      }

      function loadCoverScreenPhoto(input) {
        if (input.files && input.files[0]) {
          const reader = new FileReader();
          reader.onload = function(e) {
            const dataUrl = e.target.result;
            try {
              localStorage.setItem('just4u_custom_logo_cover', dataUrl);
            } catch(err) {
              console.warn("Storage quota limit reached for photo, using in-memory");
            }
            displayCoverPhoto(dataUrl);
            showToast('Custom logo photo loaded');
          };
          reader.readAsDataURL(input.files[0]);
        }
      }

      function checkSavedCoverPhoto() {
        const savedImg = localStorage.getItem('just4u_custom_logo_cover');
        if (savedImg) {
          displayCoverPhoto(savedImg);
        }
      }

      function displayCoverPhoto(src) {
        const container = document.getElementById('customSplashImageContainer');
        const img = document.getElementById('customSplashImage');
        const neonGraphic = document.getElementById('neonSignGraphic');
        img.src = src;
        container.classList.remove('hidden');
        neonGraphic.classList.add('hidden');
      }

      // Clock Updater for iPhone status bar
      function updateClock() {
        const now = new Date();
        let hours = now.getHours();
        let minutes = now.getMinutes();
        minutes = minutes < 10 ? '0' + minutes : minutes;
        const formatted = `${hours}:${minutes}`;
        document.getElementById('currentTime').textContent = formatted;
        const splashTime = document.getElementById('splashTime');
        if (splashTime) splashTime.textContent = formatted;
      }

      // Local Storage Helpers
      function loadFromLocalStorage() {
        const saved = localStorage.getItem('filament_vault_data');
        if (saved) {
          try {
            inventory = JSON.parse(saved);
          } catch(e) {
            inventory = DEFAULT_FILAMENTS;
          }
        } else {
          inventory = DEFAULT_FILAMENTS;
          saveToLocalStorage();
        }
      }

      function saveToLocalStorage() {
        localStorage.setItem('filament_vault_data', JSON.stringify(inventory));
      }

      // Color Mode Switching
      function setColorMode(mode) {
        activeColorMode = mode;
        if (mode === 'variant') {
          colorModeVariantBtn.className = 'py-1.5 rounded-lg text-xs font-semibold bg-white dark:bg-[#2C2C2E] shadow-sm text-neutral-900 dark:text-white transition';
          colorModeSolidBtn.className = 'py-1.5 rounded-lg text-xs font-semibold text-neutral-600 dark:text-neutral-400 transition';
          colorVariationContainer.classList.remove('hidden');
          // If finish is still standard, suggest Dual-Color or Silk
          if (finishSelect.value === 'Basic / Standard') {
            finishSelect.value = 'Dual-Color Silk';
          }
        } else {
          colorModeSolidBtn.className = 'py-1.5 rounded-lg text-xs font-semibold bg-white dark:bg-[#2C2C2E] shadow-sm text-neutral-900 dark:text-white transition';
          colorModeVariantBtn.className = 'py-1.5 rounded-lg text-xs font-semibold text-neutral-600 dark:text-neutral-400 transition';
          colorVariationContainer.classList.add('hidden');
        }
      }

      function handleVariationTypeChange() {
        const type = variationTypeSelect.value;
        if (type.includes('Tri-Color') || type.includes('Rainbow') || type.includes('Custom')) {
          tertiaryColorContainer.classList.remove('hidden');
        } else {
          tertiaryColorContainer.classList.add('hidden');
        }
      }

      function applyVariationPreset(variationType, name, hex1, hex2, name1, name2, pattern, hex3 = '', name3 = '') {
        setColorMode('variant');
        variationTypeSelect.value = variationType;
        handleVariationTypeChange();
        colorNameInput.value = name;
        colorHexInput.value = hex1;
        colorHexPreviewText.textContent = hex1.toUpperCase();
        colorHex2Input.value = hex2;
        color2NameInput.value = name2;
        if (hex3) {
          colorHex3Input.value = hex3;
          color3NameInput.value = name3;
        }
        variationPatternInput.value = pattern;
      }

      // Event Listeners setup
      function setupEventListeners() {
        // Theme Toggle
        document.getElementById('themeToggleBtn').addEventListener('click', () => {
          document.documentElement.classList.toggle('dark');
        });

        // Search Query
        searchInput.addEventListener('input', (e) => {
          searchQuery = e.target.value.toLowerCase().trim();
          clearSearchBtn.classList.toggle('hidden', searchQuery.length === 0);
          renderInventory();
        });

        clearSearchBtn.addEventListener('click', () => {
          searchInput.value = '';
          searchQuery = '';
          clearSearchBtn.classList.add('hidden');
          renderInventory();
        });

        // Filter Pills
        document.querySelectorAll('#filterPills .filter-btn').forEach(btn => {
          btn.addEventListener('click', () => {
            document.querySelectorAll('#filterPills .filter-btn').forEach(b => {
              b.className = 'filter-btn px-3 py-1 rounded-full font-medium bg-white dark:bg-[#1C1C1E] text-neutral-600 dark:text-neutral-300 border border-black/5 dark:border-white/5 transition';
            });
            btn.className = 'filter-btn px-3 py-1 rounded-full font-medium bg-neutral-900 dark:bg-white text-white dark:text-neutral-900 shadow-sm transition';
            currentFilter = btn.getAttribute('data-filter');
            renderInventory();
          });
        });

        // Brand Select & Custom "Other" handling
        brandSelect.addEventListener('change', () => {
          if (brandSelect.value === 'Other') {
            customBrandContainer.classList.remove('hidden');
            customBrandInput.focus();
          } else {
            customBrandContainer.classList.add('hidden');
            customBrandInput.value = '';
          }
        });

        // Material Grid Selector in Modal
        document.querySelectorAll('#materialGrid .mat-btn').forEach(btn => {
          btn.addEventListener('click', () => {
            document.querySelectorAll('#materialGrid .mat-btn').forEach(b => {
              b.className = 'mat-btn py-1.5 rounded-lg text-center bg-[#F2F2F7] dark:bg-[#1C1C1E] text-neutral-800 dark:text-neutral-200';
            });
            btn.className = 'mat-btn py-1.5 rounded-lg text-center font-semibold bg-ios-blue text-white shadow-sm';
            activeSelectedMaterial = btn.getAttribute('data-mat');
          });
        });

        // Amount Remaining Segments (Full, 3/4, 1/2, 1/4)
        document.querySelectorAll('#amountSegments .seg-btn').forEach(btn => {
          btn.addEventListener('click', () => {
            const val = parseFloat(btn.getAttribute('data-val'));
            setAmountRemainingLevel(val);
          });
        });

        // Color Hex picker sync
        colorHexInput.addEventListener('input', (e) => {
          colorHexPreviewText.textContent = e.target.value.toUpperCase();
        });

        // Close modal by clicking outside
        spoolModal.addEventListener('click', (e) => {
          if (e.target === spoolModal) closeSpoolModal();
        });
      }

      // Set Level
      function setAmountRemainingLevel(val) {
        activeAmountRemaining = val;
        const textMap = {
          1.0: 'Full (100%)',
          0.75: '3/4 Roll (75%)',
          0.5: '1/2 Roll (50%)',
          0.25: '1/4 Roll (25%)'
        };
        levelLabelBadge.textContent = textMap[val] || `${Math.round(val * 100)}%`;

        document.querySelectorAll('#amountSegments .seg-btn').forEach(b => {
          const bVal = parseFloat(b.getAttribute('data-val'));
          if (bVal === val) {
            b.className = 'seg-btn py-1.5 rounded-lg text-xs font-semibold bg-white dark:bg-[#2C2C2E] shadow-sm text-neutral-900 dark:text-white';
          } else {
            b.className = 'seg-btn py-1.5 rounded-lg text-xs font-semibold text-neutral-600 dark:text-neutral-400';
          }
        });
      }

      // Quick Color Swatch setter
      function setSwatch(hex, name) {
        colorHexInput.value = hex;
        colorHexPreviewText.textContent = hex.toUpperCase();
        if (!colorNameInput.value || colorNameInput.value.trim() === '') {
          colorNameInput.value = name;
        }
      }

      // Modal Qty Stepper
      function adjustModalQty(delta) {
        activeQuantity = Math.max(1, activeQuantity + delta);
        qtyDisplay.textContent = activeQuantity;
      }

      // Modal Sheet Handlers
      function openSpoolModal(existingSpool = null) {
        if (existingSpool) {
          modalTitle.textContent = "Edit Spool";
          editFilamentId.value = existingSpool.id;
          deleteBtnContainer.classList.remove('hidden');

          // Brand & Custom "Other" Brand naming
          const standardBrands = ['Bambu Lab','eSUN','Polymaker','Sunlu','Hatchbox','Overture','Elegoo','Inland','Prusament','ANYCUBIC','Creality','MatterHackers'];
          if (standardBrands.includes(existingSpool.brand) && !existingSpool.customBrand) {
            brandSelect.value = existingSpool.brand;
            customBrandContainer.classList.add('hidden');
            customBrandInput.value = '';
          } else {
            brandSelect.value = 'Other';
            customBrandContainer.classList.remove('hidden');
            customBrandInput.value = existingSpool.customBrand || existingSpool.brand;
          }

          // Material
          activeSelectedMaterial = existingSpool.material;
          document.querySelectorAll('#materialGrid .mat-btn').forEach(b => {
            const match = b.getAttribute('data-mat') === existingSpool.material;
            b.classList.toggle('bg-ios-blue', match);
            b.classList.toggle('text-white', match);
            b.classList.toggle('font-semibold', match);
            b.classList.toggle('bg-[#F2F2F7]', !match);
            b.classList.toggle('dark:bg-[#1C1C1E]', !match);
          });

          finishSelect.value = existingSpool.finish || 'Basic / Standard';
          colorHexInput.value = existingSpool.colorHex || '#007AFF';
          colorHexPreviewText.textContent = (existingSpool.colorHex || '#007AFF').toUpperCase();
          colorNameInput.value = existingSpool.colorName || '';

          // Color Variation population
          if (existingSpool.colorMode === 'variant' || existingSpool.variationType || existingSpool.colorHex2) {
            setColorMode('variant');
            variationTypeSelect.value = existingSpool.variationType || 'Dual-Color Dichromatic';
            colorHex2Input.value = existingSpool.colorHex2 || '#FF2D55';
            color2NameInput.value = existingSpool.color2Name || '';
            colorHex3Input.value = existingSpool.colorHex3 || '#FFCC00';
            color3NameInput.value = existingSpool.color3Name || '';
            variationPatternInput.value = existingSpool.variationPattern || '';
            handleVariationTypeChange();
          } else {
            setColorMode('solid');
            variationPatternInput.value = '';
            color2NameInput.value = '';
            color3NameInput.value = '';
          }

          spoolWeightSelect.value = existingSpool.spoolWeight || '1000';
          locationInput.value = existingSpool.location || '';
          notesInput.value = existingSpool.notes || '';
          activeQuantity = existingSpool.quantity || 1;
          qtyDisplay.textContent = activeQuantity;
          
          // Ensure amount is constrained to Full (1.0), 3/4 (0.75), 1/2 (0.5), or 1/4 (0.25)
          let normalizedLevel = existingSpool.amountRemaining !== undefined ? existingSpool.amountRemaining : 1.0;
          if (![1.0, 0.75, 0.5, 0.25].includes(normalizedLevel)) {
            if (normalizedLevel > 0.875) normalizedLevel = 1.0;
            else if (normalizedLevel > 0.625) normalizedLevel = 0.75;
            else if (normalizedLevel > 0.375) normalizedLevel = 0.5;
            else normalizedLevel = 0.25;
          }
          setAmountRemainingLevel(normalizedLevel);
        } else {
          // New Spool defaults
          modalTitle.textContent = "New Filament Roll";
          editFilamentId.value = '';
          deleteBtnContainer.classList.add('hidden');
          brandSelect.value = 'Bambu Lab';
          customBrandContainer.classList.add('hidden');
          customBrandInput.value = '';
          
          // Reset material to PLA
          activeSelectedMaterial = 'PLA';
          document.querySelectorAll('#materialGrid .mat-btn').forEach(b => {
            const match = b.getAttribute('data-mat') === 'PLA';
            b.classList.toggle('bg-ios-blue', match);
            b.classList.toggle('text-white', match);
            b.classList.toggle('font-semibold', match);
            b.classList.toggle('bg-[#F2F2F7]', !match);
            b.classList.toggle('dark:bg-[#1C1C1E]', !match);
          });

          finishSelect.value = 'Basic / Standard';
          colorHexInput.value = '#007AFF';
          colorHexPreviewText.textContent = '#007AFF';
          colorNameInput.value = '';
          setColorMode('solid');
          colorHex2Input.value = '#FF2D55';
          color2NameInput.value = '';
          colorHex3Input.value = '#FFCC00';
          color3NameInput.value = '';
          variationPatternInput.value = '';
          spoolWeightSelect.value = '1000';
          locationInput.value = '';
      // Open / Close Native Install Guide Modal
      function openInstallGuideModal() {
        const modal = document.getElementById('installGuideModal');
        const sheet = document.getElementById('installGuideSheet');
        modal.classList.remove('hidden');
        setTimeout(() => {
          sheet.classList.remove('translate-y-full');
        }, 10);
        lucide.createIcons();
      }

      function closeInstallGuideModal() {
        const modal = document.getElementById('installGuideModal');
        const sheet = document.getElementById('installGuideSheet');
        sheet.classList.add('translate-y-full');
        setTimeout(() => {
          modal.classList.add('hidden');
        }, 250);
      }

      function handleSaveSpool() {
        const selectedBrand = brandSelect.value;
        const customBrand = customBrandInput.value.trim();
        
        // If "Other" is chosen, name the brand by whatever the user entered
        let finalBrandName = selectedBrand;
        let storedCustomBrand = '';
        if (selectedBrand === 'Other') {
          finalBrandName = customBrand.length > 0 ? customBrand : 'Custom Brand';
          storedCustomBrand = finalBrandName;
        }

        const colorName = colorNameInput.value.trim() || 'Custom Shade';

        const isVariant = activeColorMode === 'variant';

        const spoolObj = {
          id: editFilamentId.value || 'spool_' + Date.now(),
          brand: finalBrandName,
          customBrand: storedCustomBrand,
          material: activeSelectedMaterial,
          finish: finishSelect.value,
          colorMode: activeColorMode,
          colorHex: colorHexInput.value,
          colorName: colorName,
          variationType: isVariant ? variationTypeSelect.value : '',
          colorHex2: isVariant ? colorHex2Input.value : '',
          color2Name: isVariant ? color2NameInput.value.trim() : '',
          colorHex3: isVariant ? colorHex3Input.value : '',
          color3Name: isVariant ? color3NameInput.value.trim() : '',
          variationPattern: isVariant ? variationPatternInput.value.trim() : '',
          spoolWeight: parseInt(spoolWeightSelect.value, 10),
          amountRemaining: activeAmountRemaining,
          quantity: activeQuantity,
          location: locationInput.value.trim(),
          notes: notesInput.value.trim(),
          updatedAt: Date.now()
        };

        if (editFilamentId.value) {
          const index = inventory.findIndex(s => s.id === editFilamentId.value);
          if (index !== -1) {
            inventory[index] = spoolObj;
            showToast('Spool details saved');
          }
        } else {
          inventory.unshift(spoolObj);
          showToast('New spool added to inventory');
        }

        saveToLocalStorage();
        renderInventory();
        closeSpoolModal();
      }

      function handleDeleteSpool() {
        const id = editFilamentId.value;
        if (!id) return;
        inventory = inventory.filter(s => s.id !== id);
        saveToLocalStorage();
        renderInventory();
        closeSpoolModal();
        showToast('Spool removed from inventory');
      }

      // Quick Stepper on Spool Card
      function adjustCardQuantity(id, delta, event) {
        if (event) event.stopPropagation();
        const spool = inventory.find(s => s.id === id);
        if (spool) {
          spool.quantity = Math.max(0, (spool.quantity || 1) + delta);
          if (spool.quantity === 0) {
            showToast(`${spool.colorName} stock depleted`);
          }
          saveToLocalStorage();
          renderInventory();
        }
      }

      // Quick cycle remaining spool level strictly across Full -> 3/4 -> 1/2 -> 1/4
      function cycleCardRemaining(id, event) {
        if (event) event.stopPropagation();
        const levels = [1.0, 0.75, 0.5, 0.25];
        const spool = inventory.find(s => s.id === id);
        if (spool) {
          let currentIndex = levels.indexOf(spool.amountRemaining);
          if (currentIndex === -1) currentIndex = 0;
          let nextIndex = (currentIndex + 1) % levels.length;
          spool.amountRemaining = levels[nextIndex];
          saveToLocalStorage();
          renderInventory();
          const levelNames = { 1.0: 'Full', 0.75: '3/4', 0.5: '1/2', 0.25: '1/4' };
          showToast(`Level set to ${levelNames[spool.amountRemaining]} for ${spool.colorName}`);
        }
      }

      // Render Inventory Cards
      function renderInventory() {
        inventoryContainer.innerHTML = '';

        let filtered = inventory.filter(item => {
          // Material filter
          if (currentFilter !== 'ALL') {
            if (currentFilter === 'PLA' && !item.material.includes('PLA')) return false;
            if (currentFilter === 'PETG' && !item.material.includes('PETG')) return false;
            if (currentFilter === 'ABS' && !['ABS', 'ASA'].includes(item.material)) return false;
            if (currentFilter === 'TPU' && !item.material.includes('TPU')) return false;
            if (currentFilter === 'SPECIALTY' && ['PLA', 'PETG', 'ABS', 'ASA', 'TPU'].includes(item.material)) return false;
          }

          // Search query matching
          if (searchQuery) {
            const matchString = `${item.brand} ${item.customBrand || ''} ${item.material} ${item.finish} ${item.colorName} ${item.variationType || ''} ${item.variationPattern || ''} ${item.color2Name || ''} ${item.color3Name || ''} ${item.location || ''}`.toLowerCase();
            return matchString.includes(searchQuery);
          }

          return true;
        });

        if (filtered.length === 0) {
          emptyState.classList.remove('hidden');
          inventoryContainer.classList.add('hidden');
        } else {
          emptyState.classList.add('hidden');
          inventoryContainer.classList.remove('hidden');

          filtered.forEach(spool => {
            const card = createSpoolCardElement(spool);
            inventoryContainer.appendChild(card);
          });
        }

        updateStats();
        lucide.createIcons();
      }

      function createSpoolCardElement(spool) {
        const card = document.createElement('div');
        card.className = "bg-white dark:bg-[#1C1C1E] rounded-2xl p-4 mb-3 shadow-sm border border-black/5 dark:border-white/5 active:bg-neutral-50 dark:active:bg-[#2C2C2E] transition-all cursor-pointer relative overflow-hidden";
        card.addEventListener('click', () => openSpoolModal(spool));

        // Level text and gauge calculation strictly for Full, 3/4, 1/2, 1/4
        const pct = Math.round(spool.amountRemaining * 100);
        let levelColor = 'text-ios-blue';
        let progressBg = 'bg-ios-blue';
        let levelLabel = 'Full';
        if (spool.amountRemaining === 0.75) { levelLabel = '3/4'; levelColor = 'text-blue-500'; progressBg = 'bg-blue-500'; }
        else if (spool.amountRemaining === 0.5) { levelLabel = '1/2'; levelColor = 'text-cyan-500'; progressBg = 'bg-cyan-500'; }
        else if (spool.amountRemaining <= 0.25) { levelLabel = '1/4'; levelColor = 'text-amber-500'; progressBg = 'bg-amber-500'; }

        // Material badge color
        let badgeClass = "bg-blue-50 text-blue-600 dark:bg-blue-950/40 dark:text-blue-400";
        if (spool.material === 'PETG') badgeClass = "bg-teal-50 text-teal-600 dark:bg-teal-950/40 dark:text-teal-400";
        if (['ABS','ASA'].includes(spool.material)) badgeClass = "bg-amber-50 text-amber-600 dark:bg-amber-950/40 dark:text-amber-400";
        if (spool.material === 'TPU') badgeClass = "bg-purple-50 text-purple-600 dark:bg-purple-950/40 dark:text-purple-400";
        if (spool.material.includes('CF')) badgeClass = "bg-neutral-800 text-neutral-200";

        // Estimated remaining weight
        const estWeight = Math.round(spool.spoolWeight * spool.amountRemaining);

        // Multi-color or gradient visual representation for the spool icon
        const isMultiTone = spool.colorMode === 'variant' || !!spool.colorHex2;
        let svgDef = '';
        let svgFill = spool.colorHex;

        if (isMultiTone) {
          const gradId = `grad_${spool.id}`;
          const color2 = spool.colorHex2 || '#FF2D55';
          const color3 = spool.colorHex3;
          if (color3) {
            svgDef = `
              <defs>
                <linearGradient id="${gradId}" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%" stop-color="${spool.colorHex}" />
                  <stop offset="50%" stop-color="${color2}" />
                  <stop offset="100%" stop-color="${color3}" />
                </linearGradient>
              </defs>
            `;
          } else {
            svgDef = `
              <defs>
                <linearGradient id="${gradId}" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%" stop-color="${spool.colorHex}" />
                  <stop offset="100%" stop-color="${color2}" />
                </linearGradient>
              </defs>
            `;
          }
          svgFill = `url(#${gradId})`;
        }

        // Color variation badges
        let variationBadgeHtml = '';
        if (isMultiTone) {
          const varLabel = spool.variationType ? spool.variationType.split('/')[0].trim() : 'Multi-tone';
          variationBadgeHtml = `
            <div class="flex items-center space-x-1.5 mt-1">
              <span class="inline-flex items-center gap-1 text-[10px] bg-purple-50 dark:bg-purple-950/40 text-purple-600 dark:text-purple-300 font-semibold px-2 py-0.5 rounded-full">
                <span class="w-1.5 h-1.5 rounded-full bg-purple-500"></span>
                ${varLabel}
              </span>
              ${spool.variationPattern ? `<span class="text-[10px] text-neutral-500 dark:text-neutral-400 truncate max-w-[150px]">${spool.variationPattern}</span>` : ''}
            </div>
          `;
        }

        card.innerHTML = `
          <div class="flex items-start justify-between">
            <div class="flex items-center space-x-3">
              <!-- 3D Spool Graphic Icon with filament color/gradient fill -->
              <div class="relative w-12 h-12 flex-shrink-0 flex items-center justify-center">
                <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-sm">
                  ${svgDef}
                  <circle cx="50" cy="50" r="46" fill="#2C2C2E" stroke="#48484A" stroke-width="2.5" />
                  <circle cx="50" cy="50" r="${20 + (spool.amountRemaining * 22)}" fill="${svgFill}" class="transition-all duration-300" />
                  <circle cx="50" cy="50" r="16" fill="#1C1C1E" stroke="#555" stroke-width="2" />
                  <circle cx="50" cy="50" r="10" fill="#0D0D0E" />
                  <circle cx="50" cy="50" r="43" fill="none" stroke="${spool.amountRemaining <= 0.25 ? '#FF9500' : '#34C759'}" stroke-width="2.5" stroke-dasharray="270" stroke-dashoffset="${270 - (270 * spool.amountRemaining)}" stroke-linecap="round" class="transform -rotate-90 origin-center" />
                </svg>
              </div>

              <div>
                <div class="flex items-center space-x-1.5 flex-wrap">
                  <span class="text-xs px-2 py-0.5 rounded-md font-semibold tracking-wide ${badgeClass}">
                    ${spool.material}
                  </span>
                  <span class="text-xs font-semibold text-neutral-800 dark:text-neutral-200">
                    ${spool.brand}
                  </span>
                  ${spool.finish !== 'Basic / Standard' ? `<span class="text-[10px] text-neutral-500 bg-neutral-100 dark:bg-neutral-800 px-1.5 py-0.5 rounded">${spool.finish}</span>` : ''}
                </div>

                <h3 class="text-sm font-bold text-neutral-900 dark:text-white mt-1 flex items-center gap-1.5">
                  <span class="flex items-center space-x-0.5">
                    <span class="w-2.5 h-2.5 rounded-full inline-block shadow-sm" style="background-color: ${spool.colorHex}"></span>
                    ${spool.colorHex2 ? `<span class="w-2.5 h-2.5 rounded-full inline-block shadow-sm -ml-1 border border-white dark:border-black" style="background-color: ${spool.colorHex2}"></span>` : ''}
                    ${spool.colorHex3 ? `<span class="w-2.5 h-2.5 rounded-full inline-block shadow-sm -ml-1 border border-white dark:border-black" style="background-color: ${spool.colorHex3}"></span>` : ''}
                  </span>
                  ${spool.colorName}
                </h3>

                ${variationBadgeHtml}

                <div class="flex items-center space-x-2 text-[11px] text-neutral-500 dark:text-neutral-400 mt-1">
                  <span>${spool.spoolWeight}g roll</span>
                  <span>•</span>
                  <span class="font-medium ${levelColor}">~${estWeight}g (${levelLabel} Spool)</span>
                </div>
              </div>
            </div>

            <!-- Quantity Stepper Badge -->
            <div class="flex flex-col items-end">
              <span class="text-[10px] uppercase font-bold text-neutral-400 tracking-wider">Qty</span>
              <div class="flex items-center space-x-1.5 mt-0.5 bg-[#F2F2F7] dark:bg-[#2C2C2E] p-1 rounded-xl">
                <button onclick="adjustCardQuantity('${spool.id}', -1, event)" class="w-6 h-6 rounded-lg bg-white dark:bg-[#1C1C1E] flex items-center justify-center text-neutral-700 dark:text-neutral-300 shadow-sm active:scale-90">
                  <i data-lucide="minus" class="w-3 h-3"></i>
                </button>
                <span class="font-bold text-xs px-1 text-neutral-900 dark:text-white">${spool.quantity || 1}</span>
                <button onclick="adjustCardQuantity('${spool.id}', 1, event)" class="w-6 h-6 rounded-lg bg-white dark:bg-[#1C1C1E] flex items-center justify-center text-neutral-700 dark:text-neutral-300 shadow-sm active:scale-90">
                  <i data-lucide="plus" class="w-3 h-3"></i>
                </button>
              </div>
            </div>
          </div>

          <!-- Remaining Filament Progress Gauge Bar -->
          <div class="mt-3 pt-2.5 border-t border-black/5 dark:border-white/5 flex items-center justify-between text-[11px]" onclick="cycleCardRemaining('${spool.id}', event)" title="Tap to cycle spool level (Full -> 3/4 -> 1/2 -> 1/4)">
            <div class="flex items-center space-x-2 flex-1 mr-3">
              <div class="w-full bg-neutral-200 dark:bg-neutral-800 h-2 rounded-full overflow-hidden flex">
                <div class="h-full ${progressBg} rounded-full transition-all duration-300" style="width: ${pct}%"></div>
              </div>
            </div>
            <div class="flex items-center space-x-1 flex-shrink-0 cursor-pointer text-neutral-600 dark:text-neutral-400 hover:text-ios-blue">
              <i data-lucide="gauge" class="w-3 h-3"></i>
              <span class="font-semibold">${levelLabel} Remaining</span>
            </div>
          </div>

          ${spool.location ? `
            <div class="mt-2 flex items-center space-x-1 text-[11px] text-neutral-500 dark:text-neutral-400 bg-[#F2F2F7]/80 dark:bg-[#2C2C2E]/60 px-2 py-1 rounded-lg">
              <i data-lucide="map-pin" class="w-3 h-3 text-neutral-400"></i>
              <span class="truncate">Storage: <strong class="text-neutral-700 dark:text-neutral-300">${spool.location}</strong></span>
            </div>
          ` : ''}
        `;

        return card;
      }
    ];

    // Local Storage State
    let inventory = [];
    let currentFilter = 'ALL';
    let searchQuery = '';

    // Active Modal Form State
    let activeSelectedMaterial = 'PLA';
    let activeAmountRemaining = 1.0; // 1.0 = Full, 0.75 = 3/4, 0.5 = 1/2, 0.25 = 1/4
    let activeQuantity = 1;

    // DOM Elements
    const inventoryContainer = document.getElementById('inventoryContainer');
    const emptyState = document.getElementById('emptyState');
    const searchInput = document.getElementById('searchInput');
    const clearSearchBtn = document.getElementById('clearSearchBtn');
    const brandSelect = document.getElementById('brandSelect');
    const customBrandContainer = document.getElementById('customBrandContainer');
    const customBrandInput = document.getElementById('customBrandInput');
    const finishSelect = document.getElementById('finishSelect');
    const colorHexInput = document.getElementById('colorHexInput');
    const colorNameInput = document.getElementById('colorNameInput');
    const colorHexPreviewText = document.getElementById('colorHexPreviewText');
    const spoolWeightSelect = document.getElementById('spoolWeightSelect');
    const locationInput = document.getElementById('locationInput');
    const notesInput = document.getElementById('notesInput');
    const qtyDisplay = document.getElementById('qtyDisplay');
    const levelLabelBadge = document.getElementById('levelLabelBadge');
    const editFilamentId = document.getElementById('editFilamentId');
    const modalTitle = document.getElementById('modalTitle');
    const deleteBtnContainer = document.getElementById('deleteBtnContainer');
    const spoolModal = document.getElementById('spoolModal');
    const spoolModalSheet = document.getElementById('spoolModalSheet');
    const scannerModal = document.getElementById('scannerModal');

    // Stats Elements
    const statTotalRolls = document.getElementById('statTotalRolls');
    const statTotalWeight = document.getElementById('statTotalWeight');
    const statLowRolls = document.getElementById('statLowRolls');

    // Initialize Application
    window.addEventListener('DOMContentLoaded', () => {
      loadFromLocalStorage();
      updateClock();
      setInterval(updateClock, 10000);
      setupEventListeners();
      renderInventory();
      lucide.createIcons();
    });

    // Clock Updater for iPhone status bar
    function updateClock() {
      const now = new Date();
      let hours = now.getHours();
      let minutes = now.getMinutes();
      minutes = minutes < 10 ? '0' + minutes : minutes;
      document.getElementById('currentTime').textContent = `${hours}:${minutes}`;
    }

    // Local Storage Helpers
    function loadFromLocalStorage() {
      const saved = localStorage.getItem('filament_vault_data');
      if (saved) {
        try {
          inventory = JSON.parse(saved);
        } catch(e) {
          inventory = DEFAULT_FILAMENTS;
        }
      } else {
        inventory = DEFAULT_FILAMENTS;
        saveToLocalStorage();
      }
    }

    function saveToLocalStorage() {
      localStorage.setItem('filament_vault_data', JSON.stringify(inventory));
    }

    // Event Listeners setup
    function setupEventListeners() {
      // Theme Toggle
      document.getElementById('themeToggleBtn').addEventListener('click', () => {
        document.documentElement.classList.toggle('dark');
      });

      // Search Query
      searchInput.addEventListener('input', (e) => {
        searchQuery = e.target.value.toLowerCase().trim();
        clearSearchBtn.classList.toggle('hidden', searchQuery.length === 0);
        renderInventory();
      });

      clearSearchBtn.addEventListener('click', () => {
        searchInput.value = '';
        searchQuery = '';
        clearSearchBtn.classList.add('hidden');
        renderInventory();
      });

      // Filter Pills
      document.querySelectorAll('#filterPills .filter-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          document.querySelectorAll('#filterPills .filter-btn').forEach(b => {
            b.className = 'filter-btn px-3 py-1 rounded-full font-medium bg-white dark:bg-[#1C1C1E] text-neutral-600 dark:text-neutral-300 border border-black/5 dark:border-white/5 transition';
          });
          btn.className = 'filter-btn px-3 py-1 rounded-full font-medium bg-neutral-900 dark:bg-white text-white dark:text-neutral-900 shadow-sm transition';
          currentFilter = btn.getAttribute('data-filter');
          renderInventory();
        });
      });

      // Brand Select & Custom "Other" handling
      brandSelect.addEventListener('change', () => {
        if (brandSelect.value === 'Other') {
          customBrandContainer.classList.remove('hidden');
          customBrandInput.focus();
        } else {
          customBrandContainer.classList.add('hidden');
          customBrandInput.value = '';
        }
      });

      // Material Grid Selector in Modal
      document.querySelectorAll('#materialGrid .mat-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          document.querySelectorAll('#materialGrid .mat-btn').forEach(b => {
            b.className = 'mat-btn py-1.5 rounded-lg text-center bg-[#F2F2F7] dark:bg-[#1C1C1E] text-neutral-800 dark:text-neutral-200';
          });
          btn.className = 'mat-btn py-1.5 rounded-lg text-center font-semibold bg-ios-blue text-white shadow-sm';
          activeSelectedMaterial = btn.getAttribute('data-mat');
        });
      });

      // Amount Remaining Segments (Full, 3/4, 1/2, 1/4)
      document.querySelectorAll('#amountSegments .seg-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          const val = parseFloat(btn.getAttribute('data-val'));
          setAmountRemainingLevel(val);
        });
      });

      // Color Hex picker sync
      colorHexInput.addEventListener('input', (e) => {
        colorHexPreviewText.textContent = e.target.value.toUpperCase();
      });

      // Close modal by clicking outside
      spoolModal.addEventListener('click', (e) => {
        if (e.target === spoolModal) closeSpoolModal();
      });
    }

    // Set Level
    function setAmountRemainingLevel(val) {
      activeAmountRemaining = val;
      const textMap = {
        1.0: 'Full (100%)',
        0.75: '3/4 Roll (75%)',
        0.5: '1/2 Roll (50%)',
        0.25: '1/4 Roll (25%)'
      };
      levelLabelBadge.textContent = textMap[val] || `${Math.round(val * 100)}%`;

      document.querySelectorAll('#amountSegments .seg-btn').forEach(b => {
        const bVal = parseFloat(b.getAttribute('data-val'));
        if (bVal === val) {
          b.className = 'seg-btn py-1.5 rounded-lg text-xs font-semibold bg-white dark:bg-[#2C2C2E] shadow-sm text-neutral-900 dark:text-white';
        } else {
          b.className = 'seg-btn py-1.5 rounded-lg text-xs font-semibold text-neutral-600 dark:text-neutral-400';
        }
      });
    }

    // Quick Color Swatch setter
    function setSwatch(hex, name) {
      colorHexInput.value = hex;
      colorHexPreviewText.textContent = hex.toUpperCase();
      if (!colorNameInput.value || colorNameInput.value.trim() === '') {
        colorNameInput.value = name;
      }
    }

    // Modal Qty Stepper
    function adjustModalQty(delta) {
      activeQuantity = Math.max(1, activeQuantity + delta);
      qtyDisplay.textContent = activeQuantity;
    }

    // Modal Sheet Handlers
    function openSpoolModal(existingSpool = null) {
      if (existingSpool) {
        modalTitle.textContent = "Edit Spool";
        editFilamentId.value = existingSpool.id;
        deleteBtnContainer.classList.remove('hidden');

        // Brand & Custom "Other" Brand naming
        const standardBrands = ['Bambu Lab','eSUN','Polymaker','Sunlu','Hatchbox','Overture','Elegoo','Inland','Prusament','ANYCUBIC','Creality','MatterHackers'];
        if (standardBrands.includes(existingSpool.brand) && !existingSpool.customBrand) {
          brandSelect.value = existingSpool.brand;
          customBrandContainer.classList.add('hidden');
          customBrandInput.value = '';
        } else {
          brandSelect.value = 'Other';
          customBrandContainer.classList.remove('hidden');
          customBrandInput.value = existingSpool.customBrand || existingSpool.brand;
        }

        // Material
        activeSelectedMaterial = existingSpool.material;
        document.querySelectorAll('#materialGrid .mat-btn').forEach(b => {
          const match = b.getAttribute('data-mat') === existingSpool.material;
          b.classList.toggle('bg-ios-blue', match);
          b.classList.toggle('text-white', match);
          b.classList.toggle('font-semibold', match);
          b.classList.toggle('bg-[#F2F2F7]', !match);
          b.classList.toggle('dark:bg-[#1C1C1E]', !match);
        });

        finishSelect.value = existingSpool.finish || 'Basic / Standard';
        colorHexInput.value = existingSpool.colorHex || '#007AFF';
        colorHexPreviewText.textContent = (existingSpool.colorHex || '#007AFF').toUpperCase();
        colorNameInput.value = existingSpool.colorName || '';
        spoolWeightSelect.value = existingSpool.spoolWeight || '1000';
        locationInput.value = existingSpool.location || '';
        notesInput.value = existingSpool.notes || '';
        activeQuantity = existingSpool.quantity || 1;
        qtyDisplay.textContent = activeQuantity;
        
        // Ensure amount is constrained to Full (1.0), 3/4 (0.75), 1/2 (0.5), or 1/4 (0.25)
        let normalizedLevel = existingSpool.amountRemaining !== undefined ? existingSpool.amountRemaining : 1.0;
        if (![1.0, 0.75, 0.5, 0.25].includes(normalizedLevel)) {
          if (normalizedLevel > 0.875) normalizedLevel = 1.0;
          else if (normalizedLevel > 0.625) normalizedLevel = 0.75;
          else if (normalizedLevel > 0.375) normalizedLevel = 0.5;
          else normalizedLevel = 0.25;
        }
        setAmountRemainingLevel(normalizedLevel);
      } else {
        // New Spool defaults
        modalTitle.textContent = "New Filament Roll";
        editFilamentId.value = '';
        deleteBtnContainer.classList.add('hidden');
        brandSelect.value = 'Bambu Lab';
        customBrandContainer.classList.add('hidden');
        customBrandInput.value = '';
        
        // Reset material to PLA
        activeSelectedMaterial = 'PLA';
        document.querySelectorAll('#materialGrid .mat-btn').forEach(b => {
          const match = b.getAttribute('data-mat') === 'PLA';
          b.classList.toggle('bg-ios-blue', match);
          b.classList.toggle('text-white', match);
          b.classList.toggle('font-semibold', match);
          b.classList.toggle('bg-[#F2F2F7]', !match);
          b.classList.toggle('dark:bg-[#1C1C1E]', !match);
        });

        finishSelect.value = 'Basic / Standard';
        colorHexInput.value = '#007AFF';
        colorHexPreviewText.textContent = '#007AFF';
        colorNameInput.value = '';
        spoolWeightSelect.value = '1000';
        locationInput.value = '';
        notesInput.value = '';
        activeQuantity = 1;
        qtyDisplay.textContent = '1';
        setAmountRemainingLevel(1.0);
      }

      spoolModal.classList.remove('hidden');
      setTimeout(() => {
        spoolModalSheet.classList.remove('translate-y-full');
      }, 10);
      lucide.createIcons();
    }

    function closeSpoolModal() {
      spoolModalSheet.classList.add('translate-y-full');
      setTimeout(() => {
        spoolModal.classList.add('hidden');
      }, 250);
    }

    function handleSaveSpool() {
      const selectedBrand = brandSelect.value;
      const customBrand = customBrandInput.value.trim();
      
      // If "Other" is chosen, name the brand by whatever the user entered
      let finalBrandName = selectedBrand;
      let storedCustomBrand = '';
      if (selectedBrand === 'Other') {
        finalBrandName = customBrand.length > 0 ? customBrand : 'Custom Brand';
        storedCustomBrand = finalBrandName;
      }

      const colorName = colorNameInput.value.trim() || 'Custom Shade';

      const spoolObj = {
        id: editFilamentId.value || 'spool_' + Date.now(),
        brand: finalBrandName,
        customBrand: storedCustomBrand,
        material: activeSelectedMaterial,
        finish: finishSelect.value,
        colorHex: colorHexInput.value,
        colorName: colorName,
        spoolWeight: parseInt(spoolWeightSelect.value, 10),
        amountRemaining: activeAmountRemaining,
        quantity: activeQuantity,
        location: locationInput.value.trim(),
        notes: notesInput.value.trim(),
        updatedAt: Date.now()
      };

      if (editFilamentId.value) {
        const index = inventory.findIndex(s => s.id === editFilamentId.value);
        if (index !== -1) {
          inventory[index] = spoolObj;
          showToast('Spool details saved');
        }
      } else {
        inventory.unshift(spoolObj);
        showToast('New spool added to inventory');
      }

      saveToLocalStorage();
      renderInventory();
      closeSpoolModal();
    }

    function handleDeleteSpool() {
      const id = editFilamentId.value;
      if (!id) return;
      inventory = inventory.filter(s => s.id !== id);
      saveToLocalStorage();
      renderInventory();
      closeSpoolModal();
      showToast('Spool removed from inventory');
    }

    // Quick Stepper on Spool Card
    function adjustCardQuantity(id, delta, event) {
      if (event) event.stopPropagation();
      const spool = inventory.find(s => s.id === id);
      if (spool) {
        spool.quantity = Math.max(0, (spool.quantity || 1) + delta);
        if (spool.quantity === 0) {
          showToast(`${spool.colorName} stock depleted`);
        }
        saveToLocalStorage();
        renderInventory();
      }
    }

    // Quick cycle remaining spool level strictly across Full -> 3/4 -> 1/2 -> 1/4
    function cycleCardRemaining(id, event) {
      if (event) event.stopPropagation();
      const levels = [1.0, 0.75, 0.5, 0.25];
      const spool = inventory.find(s => s.id === id);
      if (spool) {
        let currentIndex = levels.indexOf(spool.amountRemaining);
        if (currentIndex === -1) currentIndex = 0;
        let nextIndex = (currentIndex + 1) % levels.length;
        spool.amountRemaining = levels[nextIndex];
        saveToLocalStorage();
        renderInventory();
        const levelNames = { 1.0: 'Full', 0.75: '3/4', 0.5: '1/2', 0.25: '1/4' };
        showToast(`Level set to ${levelNames[spool.amountRemaining]} for ${spool.colorName}`);
      }
    }

    // Render Inventory Cards
    function renderInventory() {
      inventoryContainer.innerHTML = '';

      let filtered = inventory.filter(item => {
        // Material filter
        if (currentFilter !== 'ALL') {
          if (currentFilter === 'PLA' && !item.material.includes('PLA')) return false;
          if (currentFilter === 'PETG' && !item.material.includes('PETG')) return false;
          if (currentFilter === 'ABS' && !['ABS', 'ASA'].includes(item.material)) return false;
          if (currentFilter === 'TPU' && !item.material.includes('TPU')) return false;
          if (currentFilter === 'SPECIALTY' && ['PLA', 'PETG', 'ABS', 'ASA', 'TPU'].includes(item.material)) return false;
        }

        // Search query
        if (searchQuery) {
          const matchString = `${item.brand} ${item.customBrand || ''} ${item.material} ${item.finish} ${item.colorName} ${item.location || ''}`.toLowerCase();
          return matchString.includes(searchQuery);
        }

        return true;
      });

      if (filtered.length === 0) {
        emptyState.classList.remove('hidden');
        inventoryContainer.classList.add('hidden');
      } else {
        emptyState.classList.add('hidden');
        inventoryContainer.classList.remove('hidden');

        filtered.forEach(spool => {
          const card = createSpoolCardElement(spool);
          inventoryContainer.appendChild(card);
        });
      }

      updateStats();
      lucide.createIcons();
    }

    function createSpoolCardElement(spool) {
      const card = document.createElement('div');
      card.className = "bg-white dark:bg-[#1C1C1E] rounded-2xl p-4 mb-3 shadow-sm border border-black/5 dark:border-white/5 active:bg-neutral-50 dark:active:bg-[#2C2C2E] transition-all cursor-pointer relative overflow-hidden";
      card.addEventListener('click', () => openSpoolModal(spool));

      // Level text and gauge calculation strictly for Full, 3/4, 1/2, 1/4
      const pct = Math.round(spool.amountRemaining * 100);
      let levelColor = 'text-ios-blue';
      let progressBg = 'bg-ios-blue';
      let levelLabel = 'Full';
      if (spool.amountRemaining === 0.75) { levelLabel = '3/4'; levelColor = 'text-blue-500'; progressBg = 'bg-blue-500'; }
      else if (spool.amountRemaining === 0.5) { levelLabel = '1/2'; levelColor = 'text-cyan-500'; progressBg = 'bg-cyan-500'; }
      else if (spool.amountRemaining <= 0.25) { levelLabel = '1/4'; levelColor = 'text-amber-500'; progressBg = 'bg-amber-500'; }

      // Material badge color
      let badgeClass = "bg-blue-50 text-blue-600 dark:bg-blue-950/40 dark:text-blue-400";
      if (spool.material === 'PETG') badgeClass = "bg-teal-50 text-teal-600 dark:bg-teal-950/40 dark:text-teal-400";
      if (['ABS','ASA'].includes(spool.material)) badgeClass = "bg-amber-50 text-amber-600 dark:bg-amber-950/40 dark:text-amber-400";
      if (spool.material === 'TPU') badgeClass = "bg-purple-50 text-purple-600 dark:bg-purple-950/40 dark:text-purple-400";
      if (spool.material.includes('CF')) badgeClass = "bg-neutral-800 text-neutral-200";

      // Estimated remaining weight
      const estWeight = Math.round(spool.spoolWeight * spool.amountRemaining);

      card.innerHTML = `
        <div class="flex items-start justify-between">
          <div class="flex items-center space-x-3">
            <!-- 3D Spool Graphic Icon with filament color fill -->
            <div class="relative w-12 h-12 flex-shrink-0 flex items-center justify-center">
              <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-sm">
                <circle cx="50" cy="50" r="46" fill="#2C2C2E" stroke="#48484A" stroke-width="2.5" />
                <circle cx="50" cy="50" r="${20 + (spool.amountRemaining * 22)}" fill="${spool.colorHex}" class="transition-all duration-300" />
                <circle cx="50" cy="50" r="16" fill="#1C1C1E" stroke="#555" stroke-width="2" />
                <circle cx="50" cy="50" r="10" fill="#0D0D0E" />
                <circle cx="50" cy="50" r="43" fill="none" stroke="${spool.amountRemaining <= 0.25 ? '#FF9500' : '#34C759'}" stroke-width="2.5" stroke-dasharray="270" stroke-dashoffset="${270 - (270 * spool.amountRemaining)}" stroke-linecap="round" class="transform -rotate-90 origin-center" />
              </svg>
            </div>

            <div>
              <div class="flex items-center space-x-1.5 flex-wrap">
                <span class="text-xs px-2 py-0.5 rounded-md font-semibold tracking-wide ${badgeClass}">
                  ${spool.material}
                </span>
                <span class="text-xs font-semibold text-neutral-800 dark:text-neutral-200">
                  ${spool.brand}
                </span>
                ${spool.finish !== 'Basic / Standard' ? `<span class="text-[10px] text-neutral-500 bg-neutral-100 dark:bg-neutral-800 px-1.5 py-0.5 rounded">${spool.finish}</span>` : ''}
              </div>

              <h3 class="text-sm font-bold text-neutral-900 dark:text-white mt-1 flex items-center gap-1.5">
                <span class="w-2.5 h-2.5 rounded-full inline-block shadow-sm" style="background-color: ${spool.colorHex}"></span>
                ${spool.colorName}
              </h3>

              <div class="flex items-center space-x-2 text-[11px] text-neutral-500 dark:text-neutral-400 mt-1">
                <span>${spool.spoolWeight}g roll</span>
                <span>•</span>
                <span class="font-medium ${levelColor}">~${estWeight}g (${levelLabel} Spool)</span>
              </div>
            </div>
          </div>

          <!-- Quantity Stepper Badge -->
          <div class="flex flex-col items-end">
            <span class="text-[10px] uppercase font-bold text-neutral-400 tracking-wider">Qty</span>
            <div class="flex items-center space-x-1.5 mt-0.5 bg-[#F2F2F7] dark:bg-[#2C2C2E] p-1 rounded-xl">
              <button onclick="adjustCardQuantity('${spool.id}', -1, event)" class="w-6 h-6 rounded-lg bg-white dark:bg-[#1C1C1E] flex items-center justify-center text-neutral-700 dark:text-neutral-300 shadow-sm active:scale-90">
                <i data-lucide="minus" class="w-3 h-3"></i>
              </button>
              <span class="font-bold text-xs px-1 text-neutral-900 dark:text-white">${spool.quantity || 1}</span>
              <button onclick="adjustCardQuantity('${spool.id}', 1, event)" class="w-6 h-6 rounded-lg bg-white dark:bg-[#1C1C1E] flex items-center justify-center text-neutral-700 dark:text-neutral-300 shadow-sm active:scale-90">
                <i data-lucide="plus" class="w-3 h-3"></i>
              </button>
            </div>
          </div>
        </div>

        <!-- Remaining Filament Progress Gauge Bar -->
        <div class="mt-3 pt-2.5 border-t border-black/5 dark:border-white/5 flex items-center justify-between text-[11px]" onclick="cycleCardRemaining('${spool.id}', event)" title="Tap to cycle spool level (Full -> 3/4 -> 1/2 -> 1/4)">
          <div class="flex items-center space-x-2 flex-1 mr-3">
            <div class="w-full bg-neutral-200 dark:bg-neutral-800 h-2 rounded-full overflow-hidden flex">
              <div class="h-full ${progressBg} rounded-full transition-all duration-300" style="width: ${pct}%"></div>
            </div>
          </div>
          <div class="flex items-center space-x-1 flex-shrink-0 cursor-pointer text-neutral-600 dark:text-neutral-400 hover:text-ios-blue">
            <i data-lucide="gauge" class="w-3 h-3"></i>
            <span class="font-semibold">${levelLabel} Remaining</span>
          </div>
        </div>

        ${spool.location ? `
          <div class="mt-2 flex items-center space-x-1 text-[11px] text-neutral-500 dark:text-neutral-400 bg-[#F2F2F7]/80 dark:bg-[#2C2C2E]/60 px-2 py-1 rounded-lg">
            <i data-lucide="map-pin" class="w-3 h-3 text-neutral-400"></i>
            <span class="truncate">Storage: <strong class="text-neutral-700 dark:text-neutral-300">${spool.location}</strong></span>
          </div>
        ` : ''}
      `;

      return card;
    }

    // Header Statistics Recalculation
    function updateStats() {
      let totalCount = 0;
      let totalGrams = 0;
      let lowCount = 0;

      inventory.forEach(item => {
        const qty = item.quantity || 1;
        totalCount += qty;
        totalGrams += (item.spoolWeight * item.amountRemaining) * qty;
        if (item.amountRemaining <= 0.25) {
          lowCount += qty;
        }
      });

      statTotalRolls.textContent = totalCount;
      statTotalWeight.textContent = `${(totalGrams / 1000).toFixed(1)} kg`;
      statLowRolls.textContent = lowCount;
    }

    // Label Scanner Feature
    function openScannerModal() {
      scannerModal.classList.remove('hidden');
      lucide.createIcons();
    }

    function closeScannerModal() {
      scannerModal.classList.add('hidden');
    }

    function toggleTorch() {
      const btn = document.getElementById('torchBtn');
      btn.classList.toggle('text-yellow-300');
      btn.classList.toggle('text-white');
      showToast('Torch toggled');
    }

    function simulateRecognition(brand, material, colorName, hex, amount, weight) {
      closeScannerModal();
      openSpoolModal();

      // Pre-fill fields with recognized label data
      const standardBrands = ['Bambu Lab','eSUN','Polymaker','Sunlu','Hatchbox','Overture','Elegoo','Inland','Prusament','ANYCUBIC','Creality','MatterHackers'];
      if (standardBrands.includes(brand)) {
        brandSelect.value = brand;
        customBrandContainer.classList.add('hidden');
      } else {
        brandSelect.value = 'Other';
        customBrandContainer.classList.remove('hidden');
        customBrandInput.value = brand;
      }

      // Material
      activeSelectedMaterial = material.includes('PLA') ? 'PLA' : (material.includes('PETG') ? 'PETG' : 'Specialty');
      document.querySelectorAll('#materialGrid .mat-btn').forEach(b => {
        const match = b.getAttribute('data-mat') === activeSelectedMaterial;
        b.classList.toggle('bg-ios-blue', match);
        b.classList.toggle('text-white', match);
      });

      colorNameInput.value = colorName;
      colorHexInput.value = hex;
      colorHexPreviewText.textContent = hex.toUpperCase();
      spoolWeightSelect.value = weight.toString();
      setAmountRemainingLevel(amount);

      showToast(`Scanned label: ${brand} ${colorName}`);
    }

    function processSimulatedScan(input) {
      if (input.files && input.files[0]) {
        showToast('Processing photo OCR...');
        setTimeout(() => {
          simulateRecognition('eSUN', 'PETG', 'Cold White', '#FFFFFF', 1.0, 1000);
        }, 1000);
      }
    }

    // Toast Messenger
    function showToast(msg) {
      const toast = document.getElementById('toast');
      const toastMessage = document.getElementById('toastMessage');
      toastMessage.textContent = msg;
      toast.classList.remove('opacity-0');
      toast.classList.add('opacity-100');
      setTimeout(() => {
        toast.classList.remove('opacity-100');
        toast.classList.add('opacity-0');
      }, 2300);
    }

    // Backup / Export
    function exportJSONData() {
      const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(inventory, null, 2));
      const downloadAnchor = document.createElement('a');
      downloadAnchor.setAttribute("href", dataStr);
      downloadAnchor.setAttribute("download", `filament_inventory_${new Date().toISOString().slice(0,10)}.json`);
      document.body.appendChild(downloadAnchor);
      downloadAnchor.click();
      downloadAnchor.remove();
      showToast('Inventory exported to JSON');
    }
  </script>
</body>
</html>

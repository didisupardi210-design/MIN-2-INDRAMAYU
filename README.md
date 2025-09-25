<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>OPTIMIS JUZ 30 "MIN 2 INDRAMAYU" — Version 15 Cloud Database</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Amiri:wght@400;700&family=Inter:wght@400;600;700&display=swap');
    :root { color-scheme: light; }
    body { 
      font-family: 'Inter', system-ui, -apple-system, Segoe UI, Roboto, 'Helvetica Neue', Arial, 'Noto Sans', 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol';
      box-sizing: border-box;
    }
    .arabic { font-family: 'Amiri', serif; letter-spacing: 0.2px; }
    .page { display: none; }
    .page.active { display: block; }

    /* Confetti particles */
    .confetti {
      position: fixed;
      top: -10px;
      width: 8px;
      height: 14px;
      opacity: 0.9;
      pointer-events: none;
      border-radius: 2px;
      transform: rotate(0deg);
      animation: fall linear forwards;
      z-index: 60;
    }
    @keyframes fall {
      0% { transform: translateY(-20px) rotate(0deg); opacity: 1; }
      100% { transform: translateY(110vh) rotate(720deg); opacity: 0.8; }
    }

    /* Cloud sync indicator */
    .cloud-sync {
      animation: pulse 2s infinite;
    }
    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.7; }
    }

    /* Auto-refresh indicator */
    .auto-refresh {
      animation: spin 2s linear infinite;
    }
    @keyframes spin {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }
  </style>
</head>
<body class="bg-gradient-to-br from-emerald-50 to-teal-100 min-h-screen">
  <!-- Cloud Status & Auto-Refresh Indicator -->
  <div id="cloudStatus" class="fixed top-2 left-2 z-50">
    <div class="bg-emerald-600 text-white px-3 py-1 rounded-full text-xs flex items-center space-x-2 cloud-sync cursor-pointer" onclick="showCloudInfo()">
      <div class="w-2 h-2 bg-white rounded-full"></div>
      <span>☁️ Cloud Ready</span>
      <div id="autoRefreshIcon" class="auto-refresh">🔄</div>
    </div>
  </div>

  <!-- Version Badge -->
  <div class="fixed top-2 right-2 z-50">
    <div class="bg-blue-600 text-white px-2 py-1 rounded-full text-xs font-semibold">
      v15 • Multi-Device
    </div>
  </div>

  <!-- Cloud Info Modal -->
  <div id="cloudInfoModal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center z-50">
    <div class="bg-white rounded-xl p-6 max-w-md w-full mx-4">
      <div class="flex justify-between items-center mb-4">
        <h3 class="text-xl font-bold text-gray-800">☁️ Cloud Database Status</h3>
        <button onclick="closeCloudInfo()" class="text-gray-500 hover:text-gray-700 text-xl">✕</button>
      </div>
      <div class="space-y-3">
        <div class="flex items-center space-x-3 p-3 bg-emerald-50 rounded-lg">
          <span class="text-2xl">✅</span>
          <div>
            <div class="font-semibold text-emerald-800">Database Terhubung</div>
            <div class="text-sm text-emerald-600">Data tersimpan aman di Google Cloud</div>
          </div>
        </div>
        <div class="flex items-center space-x-3 p-3 bg-blue-50 rounded-lg">
          <span class="text-2xl">🔄</span>
          <div>
            <div class="font-semibold text-blue-800">Auto-Sync Aktif</div>
            <div class="text-sm text-blue-600">Data update otomatis setiap 30 detik</div>
          </div>
        </div>
        <div class="flex items-center space-x-3 p-3 bg-purple-50 rounded-lg">
          <span class="text-2xl">📱</span>
          <div>
            <div class="font-semibold text-purple-800">Multi-Device Ready</div>
            <div class="text-sm text-purple-600">Akses dari HP/laptop manapun</div>
          </div>
        </div>
        <div class="text-center pt-3 border-t">
          <button onclick="forceSync()" class="bg-emerald-600 text-white px-4 py-2 rounded-lg hover:bg-emerald-700 transition-colors">
            🔄 Sync Sekarang
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- Page 0: Main Menu -->
  <div id="mainMenuPage" class="page active">
    <div class="container mx-auto px-4 py-8 max-w-md">
      <div class="text-center mb-8">
        <div class="flex items-center justify-center gap-3 mb-2">
          <img class="header-logo h-12 w-12 rounded-lg shadow-sm border border-emerald-200 object-cover"
               alt="Logo Sekolah" src="" style="display:none;"
               onerror="this.src=''; this.alt='Logo gagal dimuat'; this.style.display='none';">
          <h1 class="text-4xl font-bold text-emerald-800">OPTIMIS JUZ 30</h1>
        </div>
        <h2 class="text-2xl text-emerald-700 arabic">جُزْء عَمَّ</h2>
        <p class="text-emerald-600 mt-2">"MIN 2 INDRAMAYU"</p>
        <p class="text-emerald-500 text-sm">Sistem Pencatatan Hafalan Al-Quran</p>
        <div class="mt-3 bg-gradient-to-r from-blue-50 to-purple-50 border border-blue-200 rounded-lg p-3">
          <div class="flex items-center justify-center space-x-2 text-blue-700">
            <span class="text-lg">☁️</span>
            <span class="text-sm font-bold">Version 15 • Cloud Database</span>
          </div>
          <p class="text-xs text-blue-600 mt-1">✅ Multi-Device • ✅ Real-Time Sync • ✅ Auto-Backup</p>
        </div>
      </div>

      <div class="bg-white rounded-xl shadow-lg p-6">
        <h3 class="text-xl font-semibold text-gray-800 mb-6 text-center">Pilih Jenis Pengguna</h3>
        <div class="space-y-4">
          <button onclick="showLoginPage('teacher')" class="w-full bg-emerald-600 text-white p-4 rounded-lg hover:bg-emerald-700 transition-colors flex items-center justify-between">
            <div class="flex items-center space-x-3">
              <span class="text-2xl">👨‍🏫</span>
              <div class="text-left">
                <div class="font-semibold">Login Guru</div>
                <div class="text-sm opacity-90">Akses penuh untuk mengelola data</div>
              </div>
            </div>
            <span>→</span>
          </button>

          <button onclick="showLoginPage('student')" class="w-full bg-blue-600 text-white p-4 rounded-lg hover:bg-blue-700 transition-colors flex items-center justify-between">
            <div class="flex items-center space-x-3">
              <span class="text-2xl">👨‍🎓</span>
              <div class="text-left">
                <div class="font-semibold">Login Siswa</div>
                <div class="text-sm opacity-90">Lihat progress hafalan pribadi</div>
              </div>
            </div>
            <span>→</span>
          </button>
        </div>

        <!-- Quick Stats -->
        <div class="mt-6 pt-4 border-t border-gray-200">
          <div class="text-center">
            <div class="text-sm text-gray-600 mb-2">📊 Statistik Real-Time</div>
            <div class="grid grid-cols-3 gap-3 text-center">
              <div class="bg-emerald-50 p-2 rounded">
                <div class="text-lg font-bold text-emerald-600" id="totalStudents">-</div>
                <div class="text-xs text-emerald-700">Siswa</div>
              </div>
              <div class="bg-blue-50 p-2 rounded">
                <div class="text-lg font-bold text-blue-600" id="totalCompleted">-</div>
                <div class="text-xs text-blue-700">Selesai</div>
              </div>
              <div class="bg-purple-50 p-2 rounded">
                <div class="text-lg font-bold text-purple-600" id="avgProgress">-</div>
                <div class="text-xs text-purple-700">Rata-rata</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Page 1: Login Page -->
  <div id="loginPage" class="page">
    <div class="container mx-auto px-4 py-8 max-w-md">
      <div class="text-center mb-8">
        <div class="flex items-center justify-center gap-3 mb-2">
          <img class="header-logo h-12 w-12 rounded-lg shadow-sm border border-emerald-200 object-cover"
               alt="Logo Sekolah" src="" style="display:none;"
               onerror="this.src=''; this.alt='Logo gagal dimuat'; this.style.display='none';">
          <h1 class="text-4xl font-bold text-emerald-800">OPTIMIS JUZ 30</h1>
        </div>
        <h2 class="text-2xl text-emerald-700 arabic">جُزْء عَمَّ</h2>
        <p class="text-emerald-600 mt-2">"MIN 2 INDRAMAYU"</p>
        <p class="text-emerald-500 text-sm">Login <span id="userTypeTitle">Guru</span></p>
      </div>

      <div class="bg-white rounded-xl shadow-lg p-6">
        <div class="flex justify-between items-center mb-6">
          <h3 class="text-xl font-semibold text-gray-800">Masuk ke Sistem</h3>
          <button onclick="backToMainMenu()" class="text-gray-500 hover:text-gray-700">← Kembali</button>
        </div>

        <form id="loginForm" onsubmit="handleLogin(event)">
          <div id="teacherLoginFields" class="space-y-4">
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-2">Username</label>
              <input type="text" id="username" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500" placeholder="Masukkan username">
            </div>
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-2">Password</label>
              <input type="password" id="password" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500" placeholder="Masukkan password">
            </div>
          </div>

          <div id="studentLoginFields" class="space-y-4" style="display: none;">
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-2">NISN</label>
              <input type="text" id="nisn" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500" placeholder="Masukkan NISN Anda">
              <p class="text-xs text-gray-500 mt-1">Masukkan NISN sesuai data yang terdaftar</p>
            </div>
          </div>

          <div id="loginError" class="hidden bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded mb-4 text-sm"></div>

          <button type="submit" id="loginButton" class="w-full bg-emerald-600 text-white py-3 rounded-lg font-medium hover:bg-emerald-700 transition-colors">Masuk</button>
        </form>
      </div>
    </div>
  </div>

  <!-- Page 2: Class Selection (Teacher Only) -->
  <div id="classPage" class="page">
    <div class="container mx-auto px-4 py-8 max-w-2xl">
      <div class="text-center mb-8">
        <div class="flex items-center justify-center gap-3 mb-2">
          <img class="header-logo h-12 w-12 rounded-lg shadow-sm border border-emerald-200 object-cover"
               alt="Logo Sekolah" src="" style="display:none;"
               onerror="this.src=''; this.alt='Logo gagal dimuat'; this.style.display='none';">
          <h1 class="text-4xl font-bold text-emerald-800">OPTIMIS JUZ 30</h1>
        </div>
        <h2 class="text-2xl text-emerald-700 arabic">جُزْء عَمَّ</h2>
        <p class="text-emerald-600 mt-2">"MIN 2 INDRAMAYU"</p>
        <p class="text-emerald-500 text-sm">Pilih Kelas Terlebih Dahulu</p>
      </div>

      <div class="bg-white rounded-xl shadow-lg p-6">
        <div class="flex justify-between items-center mb-4">
          <h3 class="text-xl font-semibold text-gray-800">Pilih Kelas</h3>
          <button onclick="logout()" class="bg-red-500 text-white px-4 py-2 rounded-lg hover:bg-red-600 transition-colors text-sm">Logout</button>
        </div>
        <div id="classList" class="space-y-3">
          <div class="text-center text-gray-500">
            <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-emerald-600 mx-auto mb-2"></div>
            Memuat data kelas dari cloud...
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Page 3: Student Selection (Teacher Only) -->
  <div id="studentPage" class="page">
    <div class="container mx-auto px-4 py-8 max-w-4xl">
      <div class="text-center mb-8">
        <div class="flex items-center justify-center gap-3 mb-2">
          <img class="header-logo h-12 w-12 rounded-lg shadow-sm border border-emerald-200 object-cover"
               alt="Logo Sekolah" src="" style="display:none;"
               onerror="this.src=''; this.alt='Logo gagal dimuat'; this.style.display='none';">
          <h1 class="text-4xl font-bold text-emerald-800">OPTIMIS JUZ 30</h1>
        </div>
        <h2 class="text-2xl text-emerald-700 arabic">جُزْء عَمَّ</h2>
        <p class="text-emerald-600 mt-2">"MIN 2 INDRAMAYU"</p>
        <p class="text-emerald-500 text-sm">Pilih Siswa - Kelas <span id="selectedClassName"></span></p>
      </div>

      <div class="bg-white rounded-xl shadow-lg p-6 mb-4">
        <div class="flex justify-between items-center mb-4">
          <h3 class="text-xl font-semibold text-gray-800">Daftar Siswa</h3>
          <div class="flex space-x-2">
            <button onclick="backToClassSelection()" class="bg-gray-500 text-white px-4 py-2 rounded-lg hover:bg-gray-600 transition-colors">← Kembali ke Kelas</button>
            <button onclick="logout()" class="bg-red-500 text-white px-4 py-2 rounded-lg hover:bg-red-600 transition-colors">Logout</button>
          </div>
        </div>

        <div class="mb-4 space-y-3">
          <input type="text" id="studentSearch" placeholder="Cari nama siswa..." class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500" oninput="filterStudents()">
          <div class="flex flex-wrap gap-2">
            <button onclick="filterStudentsByProgress('all')" id="filterAll" class="student-filter-btn bg-emerald-600 text-white px-3 py-1 rounded-lg text-sm font-medium hover:bg-emerald-700 transition-colors">Semua Siswa</button>
            <button onclick="filterStudentsByProgress('in-progress')" id="filterInProgress" class="student-filter-btn bg-gray-200 text-gray-700 px-3 py-1 rounded-lg text-sm font-medium hover:bg-gray-300 transition-colors">📚 Sedang Proses</button>
            <button onclick="filterStudentsByProgress('completed')" id="filterCompleted" class="student-filter-btn bg-gray-200 text-gray-700 px-3 py-1 rounded-lg text-sm font-medium hover:bg-gray-300 transition-colors">✅ Sudah Selesai</button>
            <button onclick="filterStudentsByProgress('not-started')" id="filterNotStarted" class="student-filter-btn bg-gray-200 text-gray-700 px-3 py-1 rounded-lg text-sm font-medium hover:bg-gray-300 transition-colors">⏳ Belum Mulai</button>
          </div>
        </div>

        <div id="studentList" class="grid gap-3">
          <div class="text-center text-gray-500">
            <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-emerald-600 mx-auto mb-2"></div>
            Memuat data siswa dari cloud...
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Page 4: Hafalan Recording -->
  <div id="hafalanPage" class="page">
    <div class="container mx-auto px-4 py-8 max-w-4xl">
      <!-- Header -->
      <div class="text-center mb-6">
        <div class="flex items-center justify-center gap-3 mb-2">
          <img class="header-logo h-12 w-12 rounded-lg shadow-sm border-emerald-200 object-cover"
               alt="Logo Sekolah" src="" style="display:none;"
               onerror="this.src=''; this.alt='Logo gagal dimuat'; this.style.display='none';">
          <h1 class="text-4xl font-bold text-emerald-800">OPTIMIS JUZ 30</h1>
        </div>
        <h2 class="text-2xl text-emerald-700 arabic">جُزْء عَمَّ</h2>
        <p class="text-emerald-600 mt-2">"MIN 2 INDRAMAYU"</p>
        <p class="text-emerald-500 text-sm">Juz 30 - 37 Surat</p>
      </div>

      <!-- Student Identity Card -->
      <div class="bg-white rounded-xl shadow-lg p-6 mb-6">
        <div class="flex justify-between items-center mb-4">
          <h3 class="text-lg font-semibold text-gray-800">Identitas Siswa</h3>
          <div class="flex space-x-2">
            <button id="backButton" onclick="backToStudentSelection()" class="bg-gray-500 text-white px-3 py-2 rounded-lg text-sm hover:bg-gray-600 transition-colors">← Ganti Siswa</button>
            <button onclick="logout()" class="bg-red-500 text-white px-3 py-2 rounded-lg text-sm hover:bg-red-600 transition-colors">Logout</button>
          </div>
        </div>
        <div class="flex items-center space-x-6">
          <div class="flex-shrink-0">
            <img id="studentPhoto" src="" alt="Foto Siswa"
                 class="w-20 h-20 rounded-full object-cover border-4 border-emerald-200"
                 onerror="this.src='data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iODAiIGhlaWdodD0iODAiIHZpZXdCb3g9IjAgMCA4MCA4MCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPGNpcmNsZSBjeD0iNDAiIGN5PSI0MCIgcj0iNDAiIGZpbGw9IiNGM0Y0RjYiLz4KPGNpcmNsZSBjeD0iNDAiIGN5PSIzMiIgcj0iMTIiIGZpbGw9IiM5Q0EzQUYiLz4KPHBhdGggZD0iTTIwIDY4QzIwIDU2IDI4IDQ4IDQwIDQ4UzYwIDU2IDYwIDY4IiBmaWxsPSIjOUNBM0FGIi8+Cjwvc3ZnPgo='; this.alt='Foto tidak tersedia';">
          </div>
          <div class="flex-1">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div>
                <label class="text-sm font-medium text-gray-600">Nama Lengkap</label>
                <p id="studentName" class="text-lg font-semibold text-gray-800">-</p>
              </div>
              <div>
                <label class="text-sm font-medium text-gray-600">NISN</label>
                <p id="studentNISN" class="text-lg font-semibold text-gray-800">-</p>
              </div>
              <div>
                <label class="text-sm font-medium text-gray-600">Kelas</label>
                <p id="studentClass" class="text-lg font-semibold text-gray-800">-</p>
              </div>
              <div>
                <label class="text-sm font-medium text-gray-600">Jenis Kelamin</label>
                <p id="studentGender" class="text-lg font-semibold">
                  <span id="genderBadge" class="inline-block px-2.5 py-1 rounded-full text-white text-sm">-</span>
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Progress Overview -->
      <div class="bg-white rounded-xl shadow-lg p-6 mb-8">
        <div class="flex items-center justify-between mb-4">
          <h3 class="text-xl font-semibold text-gray-800">Progress Keseluruhan</h3>
          <div class="flex items-center space-x-4">
            <div class="text-center">
              <div class="text-2xl font-bold text-emerald-600" id="completedCount">0</div>
              <div class="text-sm text-gray-600">Selesai</div>
            </div>
            <div class="text-center">
              <div class="text-2xl font-bold text-amber-600" id="inProgressCount">0</div>
              <div class="text-sm text-gray-600">Proses</div>
            </div>
            <div class="text-center">
              <div class="text-2xl font-bold text-gray-400" id="notStartedCount">37</div>
              <div class="text-sm text-gray-600">Belum</div>
            </div>
          </div>
        </div>
        <div class="w-full bg-gray-200 rounded-full h-3">
          <div class="bg-gradient-to-r from-emerald-500 to-teal-500 h-3 rounded-full transition-all duration-500" id="progressBar" style="width: 0%"></div>
        </div>
        <div class="text-center mt-2">
          <span class="text-sm text-gray-600">Progress: </span>
          <span class="font-semibold text-emerald-600" id="progressPercentage">0%</span>
        </div>
      </div>

      <!-- Filter Buttons -->
      <div class="flex flex-wrap gap-2 mb-6 justify-center">
        <button onclick="filterSurahs('all', this)" class="filter-btn bg-emerald-600 text-white px-4 py-2 rounded-lg font-medium hover:bg-emerald-700 transition-colors">Semua</button>
        <button onclick="filterSurahs('completed', this)" class="filter-btn bg-gray-200 text-gray-700 px-4 py-2 rounded-lg font-medium hover:bg-gray-300 transition-colors">Selesai</button>
        <button onclick="filterSurahs('in-progress', this)" class="filter-btn bg-gray-200 text-gray-700 px-4 py-2 rounded-lg font-medium hover:bg-gray-300 transition-colors">Sedang Proses</button>
        <button onclick="filterSurahs('not-started', this)" class="filter-btn bg-gray-200 text-gray-700 px-4 py-2 rounded-lg font-medium hover:bg-gray-300 transition-colors">Belum Mulai</button>
      </div>

      <!-- Surah List -->
      <div class="grid gap-4" id="surahList"></div>

      <!-- Statistics Modal -->
      <div id="statsModal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center z-50">
        <div class="bg-white rounded-xl p-6 max-w-md w-full mx-4">
          <div class="flex justify-between items-center mb-4">
            <h3 class="text-xl font-semibold text-gray-800">Statistik Hafalan</h3>
            <button onclick="closeStatsModal()" class="text-gray-500 hover:text-gray-700">✕</button>
          </div>
          <div id="statsContent"></div>
        </div>
      </div>
    </div>
  </div>

  <script>
    // Global state
    let studentsData = [];
    let selectedStudent = null;
    let currentFilter = 'all';
    let surahData = {};
    let currentUserType = null;
    let currentUser = null;
    let schoolLogoUrl = '';
    let murojaahLinks = {};
    let progressData = {};
    let autoSyncInterval = null;

    // Pre-configured Google Sheets URLs (GANTI DENGAN URL ANDA)
    const STUDENTS_CSV_URL = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vRcJgA3V1hk6UVY_q2S1ynq0GOaF47BkiX6TJisJg277-9EKc_OUiRwUZWu7_fDmmV6m9JU5QsWYTup/pub?gid=0&single=true&output=csv';
    const PROGRESS_CSV_URL = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vRcJgA3V1hk6UVY_q2S1ynq0GOaF47BkiX6TJisJg277-9EKc_OUiRwUZWu7_fDmmV6m9JU5QsWYTup/pub?gid=1567102234&single=true&output=csv';
    const APPS_SCRIPT_URL = 'https://script.google.com/macros/s/AKfycbxmroGbJxkb8t49jsYo6TdR7A-q95dgRFrMcHabQ8xe1qEiZB4iuFO3j_T5p_gEq2sQ/exec';
    const MUROJAAH_CSV_URL = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vRcJgA3V1hk6UVY_q2S1ynq0GOaF47BkiX6TJisJg277-9EKc_OUiRwUZWu7_fDmmV6m9JU5QsWYTup/pub?gid=1677915850&single=true&output=csv';

    // Juz 30 Surahs
    const juz30Surahs = [
      { number: 78, name: "An-Naba'", arabic: "النَّبَأ", verses: 40, meaning: "Berita Besar" },
      { number: 79, name: "An-Nazi'at", arabic: "النَّازِعَات", verses: 46, meaning: "Malaikat Yang Mencabut" },
      { number: 80, name: "Abasa", arabic: "عَبَسَ", verses: 42, meaning: "Dia Bermuka Masam" },
      { number: 81, name: "At-Takwir", arabic: "التَّكْوِير", verses: 29, meaning: "Menggulung" },
      { number: 82, name: "Al-Infitar", arabic: "الانفِطَار", verses: 19, meaning: "Terbelah" },
      { number: 83, name: "Al-Mutaffifin", arabic: "المُطَفِّفِين", verses: 36, meaning: "Orang Yang Curang" },
      { number: 84, name: "Al-Inshiqaq", arabic: "الانشِقَاق", verses: 25, meaning: "Terbelah" },
      { number: 85, name: "Al-Buruj", arabic: "البُرُوج", verses: 22, meaning: "Gugusan Bintang" },
      { number: 86, name: "At-Tariq", arabic: "الطَّارِق", verses: 17, meaning: "Yang Datang Di Malam Hari" },
      { number: 87, name: "Al-A'la", arabic: "الأَعْلَى", verses: 19, meaning: "Yang Paling Tinggi" },
      { number: 88, name: "Al-Ghashiyah", arabic: "الغَاشِيَة", verses: 26, meaning: "Hari Pembalasan" },
      { number: 89, name: "Al-Fajr", arabic: "الفَجْر", verses: 30, meaning: "Fajar" },
      { number: 90, name: "Al-Balad", arabic: "البَلَد", verses: 20, meaning: "Negeri" },
      { number: 91, name: "Ash-Shams", arabic: "الشَّمْس", verses: 15, meaning: "Matahari" },
      { number: 92, name: "Al-Lail", arabic: "اللَّيْل", verses: 21, meaning: "Malam" },
      { number: 93, name: "Ad-Duha", arabic: "الضُّحَى", verses: 11, meaning: "Waktu Duha" },
      { number: 94, name: "Ash-Sharh", arabic: "الشَّرْح", verses: 8, meaning: "Kelapangan" },
      { number: 95, name: "At-Tin", arabic: "التِّين", verses: 8, meaning: "Buah Tin" },
      { number: 96, name: "Al-Alaq", arabic: "العَلَق", verses: 19, meaning: "Segumpal Darah" },
      { number: 97, name: "Al-Qadr", arabic: "القَدْر", verses: 5, meaning: "Kemuliaan" },
      { number: 98, name: "Al-Bayyinah", arabic: "البَيِّنَة", verses: 8, meaning: "Bukti Yang Nyata" },
      { number: 99, name: "Az-Zalzalah", arabic: "الزَّلْزَلَة", verses: 8, meaning: "Kegoncangan" },
      { number: 100, name: "Al-Adiyat", arabic: "العَادِيَات", verses: 11, meaning: "Kuda Yang Berlari Kencang" },
      { number: 101, name: "Al-Qari'ah", arabic: "القَارِعَة", verses: 11, meaning: "Hari Kiamat" },
      { number: 102, name: "At-Takathur", arabic: "التَّكَاثُر", verses: 8, meaning: "Bermegah-megahan" },
      { number: 103, name: "Al-Asr", arabic: "العَصْر", verses: 3, meaning: "Masa" },
      { number: 104, name: "Al-Humazah", arabic: "الهُمَزَة", verses: 9, meaning: "Pengumpat" },
      { number: 105, name: "Al-Fil", arabic: "الفِيل", verses: 5, meaning: "Gajah" },
      { number: 106, name: "Quraish", arabic: "قُرَيْش", verses: 4, meaning: "Suku Quraisy" },
      { number: 107, name: "Al-Ma'un", arabic: "المَاعُون", verses: 7, meaning: "Barang Yang Berguna" },
      { number: 108, name: "Al-Kawthar", arabic: "الكَوْثَر", verses: 3, meaning: "Nikmat Yang Banyak" },
      { number: 109, name: "Al-Kafirun", arabic: "الكَافِرُون", verses: 6, meaning: "Orang-orang Kafir" },
      { number: 110, name: "An-Nasr", arabic: "النَّصْر", verses: 3, meaning: "Pertolongan" },
      { number: 111, name: "Al-Masad", arabic: "المَسَد", verses: 5, meaning: "Sabut" },
      { number: 112, name: "Al-Ikhlas", arabic: "الإِخْلَاص", verses: 4, meaning: "Memurnikan Keesaan Allah" },
      { number: 113, name: "Al-Falaq", arabic: "الفَلَق", verses: 5, meaning: "Waktu Subuh" },
      { number: 114, name: "An-Nas", arabic: "النَّاس", verses: 6, meaning: "Manusia" }
    ];

    // Cloud info functions
    function showCloudInfo() {
      document.getElementById('cloudInfoModal').classList.remove('hidden');
      document.getElementById('cloudInfoModal').classList.add('flex');
    }

    function closeCloudInfo() {
      document.getElementById('cloudInfoModal').classList.add('hidden');
      document.getElementById('cloudInfoModal').classList.remove('flex');
    }

    function forceSync() {
      showSyncIndicator();
      loadAllData().then(() => {
        showSyncSuccess();
        updateQuickStats();
        if (selectedStudent) {
          initializeHafalanData();
        }
      }).catch(() => {
        showSyncError();
      });
    }

    function showSyncIndicator() {
      const indicator = document.getElementById('cloudStatus');
      indicator.innerHTML = `
        <div class="bg-blue-600 text-white px-3 py-1 rounded-full text-xs flex items-center space-x-2">
          <div class="animate-spin rounded-full h-2 w-2 border-b border-white"></div>
          <span>🔄 Syncing...</span>
        </div>`;
    }

    function showSyncSuccess() {
      const indicator = document.getElementById('cloudStatus');
      indicator.innerHTML = `
        <div class="bg-emerald-600 text-white px-3 py-1 rounded-full text-xs flex items-center space-x-2 cloud-sync cursor-pointer" onclick="showCloudInfo()">
          <div class="w-2 h-2 bg-white rounded-full"></div>
          <span>✅ Synced</span>
          <div id="autoRefreshIcon" class="auto-refresh">🔄</div>
        </div>`;
      setTimeout(() => {
        indicator.innerHTML = `
          <div class="bg-emerald-600 text-white px-3 py-1 rounded-full text-xs flex items-center space-x-2 cloud-sync cursor-pointer" onclick="showCloudInfo()">
            <div class="w-2 h-2 bg-white rounded-full"></div>
            <span>☁️ Cloud Ready</span>
            <div id="autoRefreshIcon" class="auto-refresh">🔄</div>
          </div>`;
      }, 3000);
    }

    function showSyncError() {
      const indicator = document.getElementById('cloudStatus');
      indicator.innerHTML = `
        <div class="bg-red-600 text-white px-3 py-1 rounded-full text-xs flex items-center space-x-2 cursor-pointer" onclick="forceSync()">
          <div class="w-2 h-2 bg-white rounded-full"></div>
          <span>❌ Retry</span>
        </div>`;
    }

    // Auto-sync setup
    function startAutoSync() {
      if (autoSyncInterval) clearInterval(autoSyncInterval);
      
      autoSyncInterval = setInterval(async () => {
        try {
          await loadProgressData();
          updateQuickStats();
          if (selectedStudent) {
            const nisn = selectedStudent.NISN || selectedStudent.nisn;
            if (progressData[nisn]) {
              surahData = progressData[nisn];
              updateProgress();
              renderSurahs();
            }
          }
        } catch (e) {
          console.warn('Auto-sync failed:', e);
        }
      }, 30000); // Sync every 30 seconds
    }

    // Navigation
    function showPage(pageId) {
      document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
      document.getElementById(pageId).classList.add('active');
      updateHeaderLogos();
    }
    function backToMainMenu() {
      showPage('mainMenuPage');
      document.getElementById('loginForm').reset();
      document.getElementById('loginError').classList.add('hidden');
    }
    function backToClassSelection() { showPage('classPage'); }
    function backToStudentSelection() {
      const className = selectedStudent?.Kelas || selectedStudent?.kelas || selectedStudent?.KELAS || '';
      document.getElementById('selectedClassName').textContent = className;
      showPage('studentPage');
      renderStudentList(className);
    }
    function logout() {
      currentUser = null; currentUserType = null; selectedStudent = null; surahData = {};
      if (autoSyncInterval) clearInterval(autoSyncInterval);
      showPage('mainMenuPage');
      document.getElementById('loginForm').reset();
      document.getElementById('loginError').classList.add('hidden');
    }

    // Login flow
    function showLoginPage(userType) {
      currentUserType = userType;
      if (userType === 'teacher') {
        document.getElementById('userTypeTitle').textContent = 'Guru';
        document.getElementById('teacherLoginFields').style.display = 'block';
        document.getElementById('studentLoginFields').style.display = 'none';
        document.getElementById('loginButton').className = 'w-full bg-emerald-600 text-white py-3 rounded-lg font-medium hover:bg-emerald-700 transition-colors';
      } else {
        document.getElementById('userTypeTitle').textContent = 'Siswa';
        document.getElementById('teacherLoginFields').style.display = 'none';
        document.getElementById('studentLoginFields').style.display = 'block';
        document.getElementById('loginButton').className = 'w-full bg-blue-600 text-white py-3 rounded-lg font-medium hover:bg-blue-700 transition-colors';
      }
      showPage('loginPage');
    }

    function handleLogin(e) {
      e.preventDefault();
      const errorDiv = document.getElementById('loginError');
      errorDiv.classList.add('hidden');

      if (currentUserType === 'teacher') {
        const u = document.getElementById('username').value;
        const p = document.getElementById('password').value;
        if (u === 'admin' && p === 'admin') {
          currentUser = { type: 'teacher', username: 'admin' };
          loadAllData().then(() => {
            renderClassList();
            showPage('classPage');
            startAutoSync();
          });
        } else showLoginError('Username atau password salah!');
      } else {
        const nisn = document.getElementById('nisn').value;
        if (!studentsData.length) {
          loadAllData().then(() => validateStudentLogin(nisn));
        } else validateStudentLogin(nisn);
      }
    }
    function showLoginError(msg) {
      const div = document.getElementById('loginError');
      div.textContent = msg; div.classList.remove('hidden');
    }
    function validateStudentLogin(nisn) {
      const student = studentsData.find(s => (s.NISN || s.nisn) === nisn);
      if (student) {
        currentUser = { type: 'student', nisn };
        selectedStudent = student;
        updateStudentIdentity(student);
        initializeHafalanData();
        showPage('hafalanPage');
        startAutoSync();
      } else showLoginError('NISN tidak ditemukan dalam database!');
    }

    // Identity card helpers
    function normalizeGender(raw) {
      const v = (raw || '').toString().trim().toLowerCase();
      if (!v) return '';
      if (['l','laki-laki','laki', 'lakilaki', 'male', 'm'].includes(v)) return 'Laki-laki';
      if (['p','perempuan','wanita','female','f'].includes(v)) return 'Perempuan';
      return raw;
    }
    function applyGenderBadge(el, gender) {
      if (!el) return;
      if (gender === 'Laki-laki') {
        el.className = 'inline-block px-2.5 py-1 rounded-full text-white text-sm bg-blue-600';
        el.textContent = 'Laki-laki';
      } else if (gender === 'Perempuan') {
        el.className = 'inline-block px-2.5 py-1 rounded-full text-white text-sm bg-purple-600';
        el.textContent = 'Perempuan';
      } else {
        el.className = 'inline-block px-2.5 py-1 rounded-full text-gray-700 bg-gray-100 text-sm';
        el.textContent = (gender || '-');
      }
    }

    // Data loading functions
    async function loadAllData() {
      try {
        await Promise.all([
          loadStudentsData(),
          loadProgressData(),
          loadMurojaahLinks()
        ]);
        updateQuickStats();
      } catch (e) {
        console.error('Failed to load data:', e);
        throw e;
      }
    }

    async function loadStudentsData() {
      try {
        const resp = await fetch(STUDENTS_CSV_URL);
        const csvText = await resp.text();
        const lines = csvText.split('\n').filter(Boolean);
        const headers = splitCsvRow(lines[0]).map(h => h.trim());
        studentsData = [];
        let logoFromCsv = '';

        for (let i = 1; i < lines.length; i++) {
          const row = splitCsvRow(lines[i]);
          const item = {};
          headers.forEach((h, idx) => item[h] = (row[idx] || '').trim());
          const genderF = row[5] || item['Jenis Kelamin'] || item['jenis_kelamin'] || item.GENDER || '';
          item.__GENDER = normalizeGender(genderF);
          studentsData.push(item);

          if (!logoFromCsv) logoFromCsv = item.Logo || item.logo || item.LOGO || '';
        }

        if (logoFromCsv && logoFromCsv.startsWith('https://')) {
          schoolLogoUrl = logoFromCsv;
          updateHeaderLogos();
        }
      } catch (e) {
        console.error('Failed to load students data:', e);
        throw e;
      }
    }

    async function loadProgressData() {
      try {
        const response = await fetch(PROGRESS_CSV_URL);
        const csvText = await response.text();
        const lines = csvText.split('\n').filter(Boolean);
        
        progressData = {};
        
        if (lines.length > 1) {
          const headers = splitCsvRow(lines[0]);
          
          for (let i = 1; i < lines.length; i++) {
            const row = splitCsvRow(lines[i]);
            const nisn = row[0];
            
            if (nisn) {
              progressData[nisn] = {};
              
              for (let j = 1; j < headers.length && j < row.length; j++) {
                const surahNum = 77 + j;
                const cellData = row[j] || '';
                
                if (cellData) {
                  const parts = cellData.split('|');
                  progressData[nisn][surahNum] = {
                    status: parts[0] || 'not-started',
                    kelancaran: parseInt(parts[1]) || 0,
                    tajwid: parseInt(parts[2]) || 0,
                    makhorijul: parseInt(parts[3]) || 0,
                    totalScore: parseInt(parts[4]) || 0,
                    notes: parts[5] || '',
                    lastTested: parts[6] || null,
                    testHistory: []
                  };
                } else {
                  progressData[nisn][surahNum] = {
                    status: 'not-started',
                    kelancaran: 0, tajwid: 0, makhorijul: 0, totalScore: 0,
                    notes: '', lastTested: null, testHistory: []
                  };
                }
              }
            }
          }
        }
      } catch (e) {
        console.warn('Failed to load progress data:', e);
        progressData = {};
        studentsData.forEach(student => {
          const nisn = student.NISN || student.nisn;
          const key = `hafalan_${nisn}`;
          const saved = localStorage.getItem(key);
          if (saved) {
            try {
              progressData[nisn] = JSON.parse(saved);
            } catch (e) {
              console.warn(`Error parsing local data for ${nisn}:`, e);
            }
          }
        });
      }
    }

    async function loadMurojaahLinks() {
      try {
        const resp = await fetch(MUROJAAH_CSV_URL);
        const csv = await resp.text();
        const rows = csv.split('\n').filter(r => r.trim().length);
        murojaahLinks = {};
        let suratNumber = 78;
        for (let i = 1; i < rows.length && suratNumber <= 114; i++, suratNumber++) {
          const cols = splitCsvRow(rows[i]);
          const link = (cols[1] || '').trim();
          if (link && /^https:\/\//i.test(link)) {
            murojaahLinks[suratNumber] = link;
          }
        }
      } catch (e) {
        console.warn('Failed to load murojaah links:', e);
      }
    }

    // Save progress data
    async function saveProgressToCloud(nisn, data) {
      try {
        const sheetData = {};
        for (let surahNum = 78; surahNum <= 114; surahNum++) {
          const surahData = data[surahNum];
          if (surahData && surahData.status !== 'not-started') {
            sheetData[`surah_${surahNum}`] = [
              surahData.status,
              surahData.kelancaran,
              surahData.tajwid,
              surahData.makhorijul,
              surahData.totalScore,
              surahData.notes || '',
              surahData.lastTested || ''
            ].join('|');
          } else {
            sheetData[`surah_${surahNum}`] = '';
          }
        }
        
        const response = await fetch(APPS_SCRIPT_URL, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            action: 'saveProgress',
            data: {
              nisn: nisn,
              ...sheetData
            }
          })
        });
        
        const result = await response.json();
        
        if (result.status === 'success') {
          progressData[nisn] = data;
          const key = `hafalan_${nisn}`;
          localStorage.setItem(key, JSON.stringify(data));
          return true;
        } else {
          throw new Error(result.message || 'Save failed');
        }
        
      } catch (e) {
        console.error('Error saving to cloud:', e);
        const key = `hafalan_${nisn}`;
        localStorage.setItem(key, JSON.stringify(data));
        return false;
      }
    }

    // Quick stats update
    function updateQuickStats() {
      const totalStudents = studentsData.length;
      let totalCompleted = 0;
      let totalProgress = 0;

      studentsData.forEach(student => {
        const nisn = student.NISN || student.nisn;
        const studentProgress = progressData[nisn];
        if (studentProgress) {
          const completed = Object.values(studentProgress).filter(x => x.status === 'completed').length;
          if (completed === 37) totalCompleted++;
          totalProgress += (completed / 37) * 100;
        }
      });

      const avgProgress = totalStudents > 0 ? Math.round(totalProgress / totalStudents) : 0;

      document.getElementById('totalStudents').textContent = totalStudents;
      document.getElementById('totalCompleted').textContent = totalCompleted;
      document.getElementById('avgProgress').textContent = avgProgress + '%';
    }

    // Split CSV helper
    function splitCsvRow(row) {
      const out = [];
      let cur = '';
      let insideQuotes = false;
      for (let i = 0; i < row.length; i++) {
        const ch = row[i];
        if (ch === '"') {
          if (insideQuotes && row[i+1] === '"') { cur += '"'; i++; }
          else { insideQuotes = !insideQuotes; }
        } else if (ch === ',' && !insideQuotes) {
          out.push(cur); cur = '';
        } else {
          cur += ch;
        }
      }
      out.push(cur);
      return out;
    }

    // Header logo
    function updateHeaderLogos() {
      const imgs = document.querySelectorAll('.header-logo');
      imgs.forEach(img => {
        if (schoolLogoUrl) {
          img.src = schoolLogoUrl;
          img.style.display = 'block';
        } else {
          img.src = '';
          img.style.display = 'none';
        }
      });
    }

    // Class list
    function renderClassList() {
      const classes = [...new Set(studentsData.map(s => s.Kelas || s.kelas || s.KELAS))].filter(Boolean).sort();
      const container = document.getElementById('classList');
      container.innerHTML = '';
      classes.forEach(cls => {
        const count = studentsData.filter(s => (s.Kelas || s.kelas || s.KELAS) === cls).length;
        const el = document.createElement('div');
        el.className = 'bg-emerald-50 border-2 border-emerald-200 rounded-lg p-4 cursor-pointer hover:bg-emerald-100 hover:border-emerald-300 transition-all';
        el.onclick = () => selectClass(cls);
        el.innerHTML = `
          <div class="flex justify-between items-center">
            <div>
              <h4 class="text-lg font-semibold text-emerald-800">Kelas ${cls}</h4>
              <p class="text-emerald-600">${count} siswa</p>
            </div>
            <div class="text-2xl">📚</div>
          </div>`;
        container.appendChild(el);
      });
    }
    function selectClass(className) {
      document.getElementById('selectedClassName').textContent = className;
      showPage('studentPage');
      renderStudentList(className);
    }

    function renderStudentList(className) {
      const list = studentsData.filter(s => (s.Kelas || s.kelas || s.KELAS) === className);
      const container = document.getElementById('studentList');
      container.innerHTML = '';
      list.forEach(student => {
        const nisn = student.NISN || student.nisn;
        let completed = 0, inProgress = 0, notStarted = 37;
        let percentage = 0;

        let studentProgress = progressData[nisn];
        if (!studentProgress) {
          const key = `hafalan_${nisn}`;
          const saved = localStorage.getItem(key);
          if (saved) {
            try {
              studentProgress = JSON.parse(saved);
            } catch (e) {
              console.warn(`Error parsing data for ${nisn}:`, e);
            }
          }
        }

        if (studentProgress) {
          completed = Object.values(studentProgress).filter(x => x.status === 'completed').length;
          inProgress = Object.values(studentProgress).filter(x => x.status === 'in-progress').length;
          notStarted = Object.values(studentProgress).filter(x => x.status === 'not-started').length;
          percentage = Math.round((completed / 37) * 100);
        }

        let overall = 'not-started';
        if (completed === 37) overall = 'completed';
        else if (completed > 0 || inProgress > 0) overall = 'in-progress';

        const name = student.Nama || student.nama || student.NAMA || 'Nama tidak tersedia';
        const nisnDisplay = student.NISN || student.nisn || '-';
        const genderNorm = student.__GENDER || '-';
        const photo = student.Foto || student.foto || student.FOTO || '';

        const genderBadgeSmall = genderNorm === 'Laki-laki'
          ? '<span class="inline-block px-2 py-0.5 rounded-full text-white text-xs bg-blue-600">Laki-laki</span>'
          : genderNorm === 'Perempuan'
            ? '<span class="inline-block px-2 py-0.5 rounded-full text-white text-xs bg-purple-600">Perempuan</span>'
            : `<span class="inline-block px-2 py-0.5 rounded-full text-gray-700 bg-gray-100 text-xs">${genderNorm}</span>`;

        const badge =
          overall === 'completed' ? '<span class="bg-emerald-100 text-emerald-800 text-xs px-2 py-1 rounded-full">✅ Selesai</span>' :
          overall === 'in-progress' ? '<span class="bg-amber-100 text-amber-800 text-xs px-2 py-1 rounded-full">📚 Proses</span>' :
          '<span class="bg-gray-100 text-gray-600 text-xs px-2 py-1 rounded-full">⏳ Belum</span>';

        const card = document.createElement('div');
        card.className = 'bg-gray-50 border border-gray-200 rounded-lg p-4 cursor-pointer hover:bg-gray-100 hover:border-gray-300 transition-all student-card';
        card.setAttribute('data-progress-status', overall);
        card.onclick = () => selectStudent(student);
        card.innerHTML = `
          <div class="flex items-center space-x-4">
            <img src="${photo}" alt="Foto ${name}" class="w-12 h-12 rounded-full object-cover border-2 border-gray-300"
                 onerror="this.src='data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDgiIGhlaWdodD0iNDgiIHZpZXdCb3g9IjAgMCA0OCA0OCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPGNpcmNsZSBjeD0iMjQiIGN5PSIyNCIgcj0iMjQiIGZpbGw9IiNGM0Y0RjYiLz4KPGNpcmNsZSBjeD0iMjQiIGN5PSIxOSIgcj0iNyIgZmlsbD0iIzlDQTNBRiIvPgo8cGF0aCBkPSJNMTIgNDBDMTIgMzMuNiAxNi44IDI5IDI0IDI5UzM2IDMzLjYgMzYgNDAiIGZpbGw9IiM5Q0EzQUYiLz4KPC9zdmc+Cg=='; this.alt='Foto tidak tersedia';">
            <div class="flex-1">
              <div class="flex items-center justify-between mb-1">
                <h4 class="font-semibold text-gray-800">${name}</h4>
                ${badge}
              </div>
              <p class="text-sm text-gray-600">NISN: ${nisnDisplay}</p>
              <p class="text-xs text-gray-500 mt-1">${genderBadgeSmall}</p>
              ${percentage > 0 ? `
                <div class="mt-2">
                  <div class="flex justify-between text-xs text-gray-600 mb-1">
                    <span>Progress</span><span>${percentage}%</span>
                  </div>
                  <div class="w-full bg-gray-200 rounded-full h-1.5">
                    <div class="bg-emerald-500 h-1.5 rounded-full" style="width:${percentage}%"></div>
                  </div>
                  <div class="flex justify-between text-xs text-gray-500 mt-1">
                    <span>✅ ${completed}</span><span>📚 ${inProgress}</span><span>⏳ ${notStarted}</span>
                  </div>
                </div>` : ''
              }
            </div>
            <div class="text-emerald-600">→</div>
          </div>`;
        container.appendChild(card);
      });
    }

    // Student page initialization
    function selectStudent(student) {
      selectedStudent = student;
      updateStudentIdentity(student);
      initializeHafalanData();
      showPage('hafalanPage');
    }
    
    function updateStudentIdentity(student) {
      const name = student.Nama || student.nama || student.NAMA || '-';
      const nisn = student.NISN || student.nisn || '-';
      const kelas = student.Kelas || student.kelas || student.KELAS || '-';
      const genderNorm = student.__GENDER || '-';
      const photoUrl = student.Foto || student.foto || student.FOTO || '';

      document.getElementById('studentName').textContent = name;
      document.getElementById('studentNISN').textContent = nisn;
      document.getElementById('studentClass').textContent = kelas;

      const badge = document.getElementById('genderBadge');
      applyGenderBadge(badge, genderNorm);

      document.getElementById('studentPhoto').src = photoUrl;

      if (currentUser?.type === 'student') document.getElementById('backButton').style.display = 'none';
      else document.getElementById('backButton').style.display = 'inline-block';
    }
    
    function initializeHafalanData() {
      const nisn = selectedStudent.NISN || selectedStudent.nisn;
      
      if (progressData[nisn]) {
        surahData = progressData[nisn];
      } else {
        const key = `hafalan_${nisn}`;
        const saved = localStorage.getItem(key);
        if (saved) {
          try {
            surahData = JSON.parse(saved);
          } catch (e) {
            console.warn('Error parsing localStorage data:', e);
            surahData = {};
          }
        } else {
          surahData = {};
          juz30Surahs.forEach(s => {
            surahData[s.number] = {
              status: 'not-started',
              kelancaran: 0, tajwid: 0, makhorijul: 0, totalScore: 0,
              notes: '', lastTested: null, testHistory: []
            };
          });
        }
      }
      
      updateProgress();
      renderSurahs();
    }
    
    async function saveHafalanData() {
      if (!selectedStudent) return;
      
      const nisn = selectedStudent.NISN || selectedStudent.nisn;
      
      showSavingIndicator();
      
      try {
        const success = await saveProgressToCloud(nisn, surahData);
        
        if (success) {
          showSaveSuccess();
        } else {
          throw new Error('Cloud save failed');
        }
      } catch (e) {
        console.warn('Cloud save failed, using localStorage:', e);
        const key = `hafalan_${nisn}`;
        localStorage.setItem(key, JSON.stringify(surahData));
        showSaveWarning();
      }
      
      updateProgress();
    }
    
    function showSavingIndicator() {
      const existing = document.getElementById('saveIndicator');
      if (existing) existing.remove();
      
      const indicator = document.createElement('div');
      indicator.id = 'saveIndicator';
      indicator.className = 'fixed top-4 right-4 bg-blue-600 text-white px-4 py-2 rounded-lg shadow-lg z-50 flex items-center space-x-2';
      indicator.innerHTML = `
        <div class="animate-spin rounded-full h-4 w-4 border-b-2 border-white"></div>
        <span>Menyimpan ke cloud...</span>
      `;
      document.body.appendChild(indicator);
    }
    
    function showSaveSuccess() {
      const indicator = document.getElementById('saveIndicator');
      if (indicator) {
        indicator.className = 'fixed top-4 right-4 bg-emerald-600 text-white px-4 py-2 rounded-lg shadow-lg z-50 flex items-center space-x-2';
        indicator.innerHTML = `
          <span>✅</span>
          <span>Tersimpan ke cloud!</span>
        `;
        setTimeout(() => indicator.remove(), 3000);
      }
    }
    
    function showSaveWarning() {
      const indicator = document.getElementById('saveIndicator');
      if (indicator) {
        indicator.className = 'fixed top-4 right-4 bg-amber-600 text-white px-4 py-2 rounded-lg shadow-lg z-50 flex items-center space-x-2';
        indicator.innerHTML = `
          <span>⚠️</span>
          <span>Tersimpan offline</span>
        `;
        setTimeout(() => indicator.remove(), 3000);
      }
    }
    
    function updateProgress() {
      const completed = Object.values(surahData).filter(s => s.status === 'completed').length;
      const inProgress = Object.values(surahData).filter(s => s.status === 'in-progress').length;
      const notStarted = Object.values(surahData).filter(s => s.status === 'not-started').length;
      document.getElementById('completedCount').textContent = completed;
      document.getElementById('inProgressCount').textContent = inProgress;
      document.getElementById('notStartedCount').textContent = notStarted;
      const percentage = Math.round((completed / juz30Surahs.length) * 100);
      document.getElementById('progressBar').style.width = percentage + '%';
      document.getElementById('progressPercentage').textContent = percentage + '%';
    }

    // Render Surah cards
    function renderSurahs() {
      const container = document.getElementById('surahList');
      container.innerHTML = '';
      const filtered = juz30Surahs.filter(s => currentFilter === 'all' ? true : (surahData[s.number]?.status === currentFilter));

      filtered.forEach(surah => {
        const data = surahData[surah.number];
        const statusColors = {
          'completed': 'bg-emerald-50 border-emerald-400 shadow-[0_0_0_3px_rgba(16,185,129,0.15)]',
          'in-progress': 'bg-amber-50 border-amber-300',
          'not-started': 'bg-gray-100 border-gray-300'
        };
        const statusIcons = { 'completed':'✅','in-progress':'📚','not-started':'⏳' };
        const statusTexts = { 'completed':'Selesai','in-progress':'Sedang Proses','not-started':'Belum Mulai' };
        const isReadOnly = currentUser?.type === 'student';

        const link = murojaahLinks[surah.number];
        const hasLink = !!link;

        const card = document.createElement('div');
        card.className = `${statusColors[data.status]} border-2 rounded-xl p-4 transition-all hover:shadow-lg`;
        card.setAttribute('data-surah', surah.number);

        card.innerHTML = `
          <div class="flex items-center justify-between mb-3">
            <div class="flex items-center space-x-3">
              <div class="w-10 h-10 bg-emerald-600 text-white rounded-full flex items-center justify-center font-bold">${surah.number}</div>
              <div>
                <h3 class="font-semibold text-gray-800">${surah.name}</h3>
                <p class="text-sm text-gray-600 arabic">${surah.arabic}</p>
              </div>
            </div>
            <div class="text-right">
              <div class="inline-flex items-center space-x-2 mb-1 px-2 py-1 rounded-full ${data.status==='completed' ? 'bg-emerald-100 text-emerald-800 border border-emerald-300' : data.status==='in-progress' ? 'bg-amber-100 text-amber-800 border border-amber-300' : 'bg-gray-100 text-gray-700 border border-gray-300'}">
                <span class="text-lg">${statusIcons[data.status]}</span>
                <span class="text-sm font-semibold">${statusTexts[data.status]}</span>
              </div>
              <p class="text-xs text-gray-500">${surah.verses} ayat • ${surah.meaning}</p>
            </div>
          </div>

          ${data.status !== 'not-started' && data.totalScore ? `
            <div class="mb-3">
              <div class="flex justify-between items-center mb-1">
                <span class="text-sm text-gray-600">Nilai Terakhir:</span>
                <span class="font-semibold ${data.totalScore >= 80 ? 'text-emerald-600' : data.totalScore >= 60 ? 'text-amber-600' : 'text-red-600'}">${data.totalScore}/100</span>
              </div>
              <div class="w-full bg-gray-200 rounded-full h-2">
                <div class="h-2 rounded-full ${data.totalScore >= 80 ? 'bg-emerald-500' : data.totalScore >= 60 ? 'bg-amber-500' : 'bg-red-500'}" style="width:${data.totalScore}%"></div>
              </div>
            </div>` : ''
          }

          ${data.lastTested ? `<p class="text-xs text-gray-500 mb-3">Terakhir ditest: ${new Date(data.lastTested).toLocaleDateString('id-ID')}</p>` : ''}

          <div class="flex flex-wrap gap-2">
            ${!isReadOnly ? `
              <button onclick="testSurah(${surah.number})" class="flex-1 bg-emerald-600 text-white px-3 py-2 rounded-lg text-sm font-medium hover:bg-emerald-700 transition-colors">
                ${data.status === 'not-started' ? 'Mulai Tes' : 'Tes Ulang'}
              </button>` : ''
            }

            ${hasLink ? `
              <a href="${link}" target="_blank" rel="noopener"
                 class="${isReadOnly ? 'flex-1' : ''} inline-flex items-center justify-center gap-2 bg-blue-600 text-white px-3 py-2 rounded-lg text-sm font-semibold hover:bg-blue-700 transition-colors">
                 <span>🎧▶️</span><span>Murojaah</span>
              </a>` :
              `<span class="${isReadOnly ? 'flex-1' : ''} inline-flex items-center justify-center px-3 py-2 rounded-lg text-sm font-medium bg-gray-100 text-gray-500 border border-gray-200">Tidak tersedia</span>`
            }

            <button onclick="viewDetails(${surah.number})"
                    class="${isReadOnly ? 'flex-1' : ''} bg-gray-200 text-gray-700 px-3 py-2 rounded-lg text-sm font-medium hover:bg-gray-300 transition-colors">
              Detail
            </button>
          </div>
        `;
        container.appendChild(card);
      });
    }

    // Confetti generator
    function launchConfetti({count = 120, duration = 1800} = {}) {
      const colors = ['#10B981','#34D399','#059669','#A7F3D0','#6EE7B7','#FBBF24','#F59E0B'];
      const body = document.body;
      for (let i = 0; i < count; i++) {
        const piece = document.createElement('div');
        piece.className = 'confetti';
        const size = 6 + Math.random() * 8;
        piece.style.width = size + 'px';
        piece.style.height = (size * 1.4) + 'px';
        piece.style.left = Math.random() * 100 + 'vw';
        piece.style.background = colors[Math.floor(Math.random()*colors.length)];
        piece.style.animationDuration = (1 + Math.random() * 1.2) + 's';
        piece.style.transform = `rotate(${Math.random()*360}deg)`;
        body.appendChild(piece);

        const horizontalDrift = (Math.random() * 2 - 1) * 60;
        const delay = Math.random() * 200;
        setTimeout(() => {
          piece.animate([
            { transform: `translateX(0px)` },
            { transform: `translateX(${horizontalDrift}px)` },
            { transform: `translateX(0px)` }
          ], { duration: 1000 + Math.random()*800, iterations: Infinity });
        }, delay);

        setTimeout(() => piece.remove(), duration + 1200);
      }
      setTimeout(() => {
        document.querySelectorAll('.confetti').forEach(el => el.remove());
      }, duration + 1600);
    }

    // Testing modal (teacher only)
    function testSurah(surahNumber) {
      if (currentUser?.type === 'student') {
        alert('Siswa tidak dapat melakukan tes. Hanya dapat melihat progress.');
        return;
      }
      const surah = juz30Surahs.find(s => s.number === surahNumber);
      showTestModal(surahNumber, surah);
    }
    function showTestModal(surahNumber, surah) {
      const modalHtml = `
        <div id="testModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
          <div class="bg-white rounded-xl p-6 max-w-lg w-full mx-4 max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center mb-6">
              <h3 class="text-xl font-semibold text-gray-800">Penilaian Hafalan</h3>
              <button onclick="closeTestModal()" class="text-gray-500 hover:text-gray-700 text-xl">✕</button>
            </div>

            <div class="text-center mb-6">
              <h4 class="text-lg font-semibold text-emerald-800">${surah.name}</h4>
              <p class="text-gray-600 arabic text-xl">${surah.arabic}</p>
              <p class="text-sm text-gray-500">${surah.meaning} • ${surah.verses} ayat</p>
            </div>

            <form id="testForm" onsubmit="submitTest(event, ${surahNumber})">
              <div class="space-y-6">
                <div class="bg-blue-50 p-4 rounded-lg">
                  <label class="block text-sm font-semibold text-blue-800 mb-3">🎯 Kelancaran Hafalan (0-100)</label>
                  <input type="number" id="kelancaran" min="0" max="100" required
                         class="w-full px-3 py-2 border border-blue-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 mb-2">
                  <div class="text-xs text-blue-600">
                    <p><strong>≥60:</strong> Selesai</p>
                    <p><strong><60:</strong> Sedang Proses</p>
                  </div>
                </div>

                <div class="bg-green-50 p-4 rounded-lg">
                  <label class="block text-sm font-semibold text-green-800 mb-3">📖 Tajwid (0-100)</label>
                  <input type="number" id="tajwid" min="0" max="100" required
                         class="w-full px-3 py-2 border border-green-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 mb-2">
                </div>

                <div class="bg-purple-50 p-4 rounded-lg">
                  <label class="block text-sm font-semibold text-purple-800 mb-3">🗣️ Makhorijul Huruf (0-100)</label>
                  <input type="number" id="makhorijul" min="0" max="100" required
                         class="w-full px-3 py-2 border border-purple-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-purple-500 mb-2">
                </div>

                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-2">📝 Catatan Tambahan</label>
                  <textarea id="testNotes" rows="3" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500" placeholder="Catatan perbaikan atau komentar..."></textarea>
                </div>

                <div class="bg-gray-50 p-4 rounded-lg">
                  <div class="flex justify-between items-center">
                    <span class="font-semibold text-gray-800">Nilai Total:</span>
                    <span id="totalScore" class="text-2xl font-bold text-emerald-600">0</span>
                  </div>
                  <div class="text-xs text-gray-600 mt-1">Rata-rata dari ketiga aspek penilaian</div>
                </div>
              </div>

              <div class="flex space-x-3 mt-6">
                <button type="button" onclick="closeTestModal()" class="flex-1 bg-gray-200 text-gray-700 py-3 rounded-lg font-medium hover:bg-gray-300 transition-colors">Batal</button>
                <button type="submit" class="flex-1 bg-emerald-600 text-white py-3 rounded-lg font-medium hover:bg-emerald-700 transition-colors">Simpan Penilaian</button>
              </div>
            </form>
          </div>
        </div>`;
      document.body.insertAdjacentHTML('beforeend', modalHtml);
      ['kelancaran','tajwid','makhorijul'].forEach(id => document.getElementById(id).addEventListener('input', calculateTotalScore));
    }
    function closeTestModal() { document.getElementById('testModal')?.remove(); }
    function calculateTotalScore() {
      const k = parseInt(document.getElementById('kelancaran').value) || 0;
      const t = parseInt(document.getElementById('tajwid').value) || 0;
      const m = parseInt(document.getElementById('makhorijul').value) || 0;
      const total = Math.round((k + t + m) / 3);
      const el = document.getElementById('totalScore');
      el.textContent = total;
      el.className = 'text-2xl font-bold ' + (total >= 80 ? 'text-emerald-600' : total >= 60 ? 'text-amber-600' : 'text-red-600');
    }
    function submitTest(e, surahNumber) {
      e.preventDefault();
      const k = parseInt(document.getElementById('kelancaran').value);
      const t = parseInt(document.getElementById('tajwid').value);
      const m = parseInt(document.getElementById('makhorijul').value);
      const notes = document.getElementById('testNotes').value;
      const total = Math.round((k + t + m) / 3);

      const prevStatus = surahData[surahNumber].status;

      if (k >= 60) {
        surahData[surahNumber].status = 'completed';
      } else if (k > 0 || t > 0 || m > 0) {
        surahData[surahNumber].status = 'in-progress';
      } else {
        surahData[surahNumber].status = 'not-started';
      }

      surahData[surahNumber].kelancaran = k;
      surahData[surahNumber].tajwid = t;
      surahData[surahNumber].makhorijul = m;
      surahData[surahNumber].totalScore = total;
      surahData[surahNumber].notes = notes;
      surahData[surahNumber].lastTested = new Date().toISOString();
      surahData[surahNumber].testHistory.push({ date: new Date().toISOString(), kelancaran: k, tajwid: t, makhorijul: m, totalScore: total, notes });

      saveHafalanData();
      renderSurahs();
      closeTestModal();

      const surah = juz30Surahs.find(s => s.number === surahNumber);
      if (prevStatus !== 'completed' && surahData[surahNumber].status === 'completed') {
        launchConfetti({ count: 120, duration: 1800 });
      }

      let msg = `Penilaian ${surah.name} selesai!\n\n📊 Hasil Penilaian:\n🎯 Kelancaran: ${k}/100\n📖 Tajwid: ${t}/100\n🗣️ Makhorijul Huruf: ${m}/100\n📈 Nilai Total: ${total}/100\n\n`;
      msg += (k >= 60)
        ? '🎉 Status: Selesai. Mantap! Terus pertahankan.'
        : '📚 Status: Sedang Proses. Semangat! Tingkatkan kelancaran hingga 60.';
      alert(msg);
    }

    // Details modal
    function viewDetails(surahNumber) {
      const surah = juz30Surahs.find(s => s.number === surahNumber);
      const d = surahData[surahNumber];
      let assessmentHtml = '';
      if (d.totalScore > 0) {
        assessmentHtml = `
          <div class="bg-gray-50 p-3 rounded-lg mb-4">
            <h5 class="font-semibold text-gray-800 mb-3">📊 Rincian Penilaian Terakhir:</h5>
            <div class="grid grid-cols-3 gap-3 text-center mb-3">
              <div class="bg-blue-100 p-2 rounded"><div class="text-xs text-blue-600 font-medium">Kelancaran</div><div class="text-lg font-bold text-blue-800">${d.kelancaran||0}</div></div>
              <div class="bg-green-100 p-2 rounded"><div class="text-xs text-green-600 font-medium">Tajwid</div><div class="text-lg font-bold text-green-800">${d.tajwid||0}</div></div>
              <div class="bg-purple-100 p-2 rounded"><div class="text-xs text-purple-600 font-medium">Makhorijul</div><div class="text-lg font-bold text-purple-800">${d.makhorijul||0}</div></div>
            </div>
            <div class="text-center bg-white p-2 rounded border">
              <div class="text-xs text-gray-600 font-medium">Nilai Total</div>
              <div class="text-xl font-bold ${d.totalScore>=80?'text-emerald-600':d.totalScore>=60?'text-amber-600':'text-red-600'}">${d.totalScore}/100</div>
            </div>
          </div>`;
      }

      let historyHtml = '';
      if (d.testHistory.length) {
        historyHtml = '<h4 class="font-semibold mb-2">📈 Riwayat Tes:</h4>';
        d.testHistory.slice(-5).reverse().forEach(t => {
          const total = t.totalScore || 0;
          historyHtml += `
            <div class="bg-gray-50 p-3 rounded mb-2">
              <div class="flex justify-between items-center mb-2">
                <span class="text-sm font-medium">${new Date(t.date).toLocaleDateString('id-ID')}</span>
                <span class="font-bold text-lg ${total>=80?'text-emerald-600':total>=60?'text-amber-600':'text-red-600'}">${total}/100</span>
              </div>
              <div class="grid grid-cols-3 gap-2 text-xs mb-2">
                <div class="text-center"><div class="text-blue-600">🎯 ${t.kelancaran ?? '-'}</div></div>
                <div class="text-center"><div class="text-green-600">📖 ${t.tajwid ?? '-'}</div></div>
                <div class="text-center"><div class="text-purple-600">🗣️ ${t.makhorijul ?? '-'}</div></div>
              </div>
              ${t.notes ? `<p class="text-xs text-gray-600 italic">"${t.notes}"</p>` : ''}
            </div>`;
        });
      }

      const isReadOnly = currentUser?.type === 'student';
      document.getElementById('statsContent').innerHTML = `
        <div class="text-center mb-4">
          <h4 class="text-lg font-semibold">${surah.name}</h4>
          <p class="text-gray-600 arabic text-xl">${surah.arabic}</p>
          <p class="text-sm text-gray-500">${surah.meaning} • ${surah.verses} ayat</p>
        </div>

        <div class="space-y-3 mb-4">
          <div class="flex justify-between"><span>Status:</span><span class="font-semibold">${d.status==='completed'?'Selesai':d.status==='in-progress'?'Sedang Proses':'Belum Mulai'}</span></div>
          ${d.totalScore>0?`<div class="flex justify-between"><span>Nilai Total Terakhir:</span><span class="font-semibold text-lg ${d.totalScore>=80?'text-emerald-600':d.totalScore>=60?'text-amber-600':'text-red-600'}">${d.totalScore}/100</span></div>`:''}
          ${d.lastTested?`<div class="flex justify-between"><span>Terakhir Ditest:</span><span>${new Date(d.lastTested).toLocaleDateString('id-ID')}</span></div>`:''}
          <div class="flex justify-between"><span>Total Tes:</span><span>${d.testHistory.length} kali</span></div>
        </div>

        ${assessmentHtml}
        ${historyHtml}

        ${!isReadOnly?`
          <div class="mt-4 pt-4 border-t">
            <button onclick="testSurah(${surahNumber}); closeStatsModal();" class="w-full bg-emerald-600 text-white py-2 rounded-lg font-medium hover:bg-emerald-700 transition-colors">
              ${d.status==='not-started'?'Mulai Tes':'Tes Ulang'}
            </button>
          </div>`:''}
      `;
      document.getElementById('statsModal').classList.remove('hidden');
      document.getElementById('statsModal').classList.add('flex');
    }
    function closeStatsModal(){ const m=document.getElementById('statsModal'); m.classList.add('hidden'); m.classList.remove('flex'); }

    // Filters
    function filterSurahs(filter, el){
      currentFilter = filter;
      document.querySelectorAll('.filter-btn').forEach(b => b.className = 'filter-btn bg-gray-200 text-gray-700 px-4 py-2 rounded-lg font-medium hover:bg-gray-300 transition-colors');
      if (el) el.className = 'filter-btn bg-emerald-600 text-white px-4 py-2 rounded-lg font-medium hover:bg-emerald-700 transition-colors';
      renderSurahs();
    }

    // Simple search/filter for students list
    function filterStudents() {
      const q = (document.getElementById('studentSearch').value || '').toLowerCase();
      document.querySelectorAll('#studentList .student-card').forEach(card => {
        const name = card.querySelector('h4')?.textContent.toLowerCase() || '';
        card.style.display = name.includes(q) ? '' : 'none';
      });
    }
    function filterStudentsByProgress(type) {
      document.querySelectorAll('.student-filter-btn').forEach(b => {
        b.className = 'student-filter-btn bg-gray-200 text-gray-700 px-3 py-1 rounded-lg text-sm font-medium hover:bg-gray-300 transition-colors';
      });
      const btn = {
        'all':'filterAll','in-progress':'filterInProgress','completed':'filterCompleted','not-started':'filterNotStarted'
      }[type];
      document.getElementById(btn).className = 'student-filter-btn bg-emerald-600 text-white px-3 py-1 rounded-lg text-sm font-medium hover:bg-emerald-700 transition-colors';

      document.querySelectorAll('#studentList .student-card').forEach(card => {
        const status = card.getAttribute('data-progress-status');
        card.style.display = (type==='all' || type===status) ? '' : 'none';
      });
    }

    // Initialize application
    loadAllData().finally(() => {
      updateHeaderLogos();
      updateQuickStats();
    });
  </script>

  <!-- Footer Note -->
  <footer class="w-full py-6 mt-10">
    <div class="container mx-auto px-4">
      <p class="text-center text-sm text-gray-600">APLIKASI PENCATAT HAFALAN AL-QURAN JUZ 30 — Version 15 • Created By <span class="font-semibold">Didi Supardi</span></p>
      <p class="text-center text-xs text-gray-500 mt-1">☁️ Cloud Database • 🔄 Real-Time Sync • 📱 Multi-Device Ready</p>
    </div>
  </footer>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'984956207498fd7f',t:'MTc1ODc5MDg4MS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>

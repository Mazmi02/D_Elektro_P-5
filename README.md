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


  { "en": "Apa Itu Komputasi Serverless?", "id": "Model Eksekusi Cloud Tanpa Mengelola Server." },
  { "en": "Apa Itu Function as a Service (FaaS)?", "id": "Platform Komputasi Serverless Berbasis Event." },
  { "en": "Apa Keuntungan Komputasi Serverless?", "id": "Skalabilitas Otomatis Dan Biaya Berbasis Penggunaan." },
  { "en": "Apa Itu AWS (Amazon Web Services) Lambda?", "id": "Contoh Layanan Komputasi Serverless Populer." },
  { "en": "Apa Itu Kontainerisasi?", "id": "Mengemas Aplikasi Beserta Semua Dependensinya." },
  { "en": "Apa Itu Docker?", "id": "Platform Populer Untuk Membuat, Menjalankan Kontainer." },
  { "en": "Apa Beda Kontainer Dan Mesin Virtual?", "id": "Kontainer Berbagi Kernel Sistem Operasi Host." },
  { "en": "Apa Itu Kubernetes?", "id": "Sistem Orkestrasi Kontainer Open-Source." },
  { "en": "Apa Itu Orkestrasi Kontainer?", "id": "Otomatisasi Penerapan, Penskalaan, Manajemen Kontainer." },
  { "en": "Apa Itu Pod Dalam Kubernetes?", "id": "Unit Penerapan Terkecil, Berisi Satu/Lebih Kontainer." },
  { "en": "Apa Itu Service Mesh?", "id": "Lapisan Infrastruktur Mengelola Komunikasi Antar Layanan." },
  { "en": "Apa Itu Istio?", "id": "Contoh Implementasi Service Mesh Open-Source." },
  { "en": "Apa Itu Infrastructure as Code (IaC)?", "id": "Mengelola, Menyediakan Infrastruktur Melalui Kode." },
  { "en": "Apa Itu Terraform?", "id": "Alat Perangkat Lunak IaC Populer." },
  { "en": "Apa Itu Ansible?", "id": "Alat Otomasi Manajemen Konfigurasi." },
  { "en": "Apa Itu Pipeline CI/CD?", "id": "Alur Kerja Otomatis Pembangunan, Pengujian, Penerapan." },
  { "en": "Apa Itu GitOps?", "id": "Praktik Manajemen Infrastruktur, Aplikasi Berbasis Git." },
  { "en": "Apa Itu Bahasa Pemrograman Go?", "id": "Bahasa Pemrograman Dikembangkan Di Google." },
  { "en": "Apa Itu Bahasa Pemrograman Rust?", "id": "Bahasa Pemrograman Berfokus Keamanan, Konkurensi." },
  { "en": "Apa Itu WebAssembly (Wasm)?", "id": "Format Instruksi Biner Portabel Untuk Web." },
  { "en": "Apa Itu Progressive Web App (PWA)?", "id": "Aplikasi Web Terasa Seperti Aplikasi Asli." },
  { "en": "Apa Itu Single Page Application (SPA)?", "id": "Aplikasi Web Memuat Satu Dokumen HTML." },
  { "en": "Sebutkan Kerangka Kerja JavaScript Populer?", "id": "React, Angular, Dan Vue.js." },
  { "en": "Apa Itu Node.js?", "id": "Runtime JavaScript Sisi Server." },
  { "en": "Apa Itu TypeScript?", "id": "Superset JavaScript Dengan Pengetikan Statis." },
  { "en": "Apa Itu Sistem Desain (Design System)?", "id": "Kumpulan Komponen Desain Dapat Digunakan Kembali." },
  { "en": "Apa Itu Desain Atomik?", "id": "Metodologi Untuk Membuat Sistem Desain." },
  { "en": "Apa Itu Aksesibilitas Web (a11y)?", "id": "Membuat Web Dapat Diakses Semua Orang." },
  { "en": "Apa Itu WCAG (Web Content Accessibility Guidelines)?", "id": "Panduan Untuk Aksesibilitas Konten Web." },
  { "en": "Apa Itu ARIA (Accessible Rich Internet Applications)?", "id": "Atribut Meningkatkan Aksesibilitas Aplikasi Web." },
  { "en": "Apa Itu Pembaca Layar (Screen Reader)?", "id": "Perangkat Lunak Membantu Pengguna Tunanetra." },
  { "en": "Apa Itu Optimisasi Mesin Pencari (SEO)?", "id": "Meningkatkan Visibilitas Situs Web Di Google." },
  { "en": "Apa Itu Kata Kunci (Keyword)?", "id": "Istilah Pencarian Yang Ditargetkan Dalam SEO." },
  { "en": "Apa Itu Backlink?", "id": "Tautan Dari Satu Situs Web Ke Situs Lain." },
  { "en": "Apa Itu Otoritas Domain (Domain Authority)?", "id": "Skor Peringkat Memprediksi Kemampuan Peringkat." },
  { "en": "Apa Itu Pemasaran Digital?", "id": "Pemasaran Produk Menggunakan Saluran Digital." },
  { "en": "Apa Itu Pemasaran Konten?", "id": "Membuat, Mendistribusikan Konten Bernilai." },
  { "en": "Apa Itu Pemasaran Media Sosial?", "id": "Menggunakan Platform Media Sosial Untuk Pemasaran." },
  { "en": "Apa Itu Pay-Per-Click (PPC) Advertising?", "id": "Model Iklan Membayar Setiap Klik." },
  { "en": "Apa Itu Analitik Web?", "id": "Pengukuran, Analisis Data Penggunaan Web." },
  { "en": "Apa Itu Google Analytics?", "id": "Layanan Analitik Web Populer." },
  { "en": "Apa Itu Tingkat Konversi (Conversion Rate)?", "id": "Persentase Pengunjung Menyelesaikan Aksi Diinginkan." },
  { "en": "Apa Itu A/B Testing?", "id": "Membandingkan Dua Versi Halaman Web." },
  { "en": "Apa Itu Peta Panas (Heatmap)?", "id": "Visualisasi Data Tempat Pengguna Mengklik." },
  { "en": "Apa Itu Ekonomi?", "id": "Studi Produksi, Distribusi, Konsumsi Barang." },
  { "en": "Apa Itu Mikroekonomi?", "id": "Mempelajari Perilaku Individu Dan Perusahaan." },
  { "en": "Apa Itu Makroekonomi?", "id": "Mempelajari Kinerja Ekonomi Secara Keseluruhan." },
  { "en": "Apa Itu Permintaan Dan Penawaran?", "id": "Model Penentuan Harga Dalam Pasar." },
  { "en": "Apa Itu Ekuilibrium Pasar?", "id": "Harga Di Mana Permintaan Sama Dengan Penawaran." },
  { "en": "Apa Itu Elastisitas?", "id": "Ukuran Responsivitas Satu Variabel Ekonomi." },
  { "en": "Apa Itu Produk Domestik Bruto (PDB)?", "id": "Nilai Pasar Semua Barang, Jasa Final." },
  { "en": "Apa Itu Inflasi?", "id": "Tingkat Kenaikan Harga Barang, Jasa." },
  { "en": "Apa Itu Indeks Harga Konsumen (IHK)?", "id": "Ukuran Rata-rata Perubahan Harga Konsumen." },
  { "en": "Apa Itu Deflasi?", "id": "Penurunan Tingkat Harga Umum Barang." },
  { "en": "Apa Itu Pengangguran?", "id": "Orang Aktif Mencari Kerja Tanpa Pekerjaan." },
  { "en": "Apa Itu Kebijakan Moneter?", "id": "Tindakan Bank Sentral Mengatur Pasokan Uang." },
  { "en": "Apa Itu Kebijakan Fiskal?", "id": "Penggunaan Pengeluaran, Pajak Pemerintah." },
  { "en": "Apa Itu Suku Bunga?", "id": "Biaya Meminjam Uang." },
  { "en": "Apa Itu Bank Sentral?", "id": "Lembaga Mengelola Mata Uang, Kebijakan Moneter." },
  { "en": "Apa Itu Resesi?", "id": "Penurunan Signifikan Aktivitas Ekonomi." },
  { "en": "Apa Itu Depresi Ekonomi?", "id": "Resesi Yang Parah Dan Berkepanjangan." },
  { "en": "Apa Itu Keunggulan Komparatif?", "id": "Kemampuan Memproduksi Barang Biaya Peluang Rendah." },
  { "en": "Apa Itu Proteksionisme?", "id": "Membatasi Perdagangan Internasional." },
  { "en": "Apa Itu Tarif?", "id": "Pajak Atas Barang Impor." },
  { "en": "Apa Itu Kuota?", "id": "Batasan Kuantitas Barang Impor." },
  { "en": "Apa Itu Neraca Perdagangan?", "id": "Selisih Antara Ekspor Dan Impor." },
  { "en": "Apa Itu Kurs Valuta Asing?", "id": "Nilai Tukar Satu Mata Uang." },
  { "en": "Apa Itu Pasar Saham?", "id": "Pasar Tempat Saham Perusahaan Diperdagangkan." },
  { "en": "Apa Itu Obligasi (Bond)?", "id": "Instrumen Utang Dengan Bunga Tetap." },
  { "en": "Apa Itu Aset?", "id": "Sumber Daya Ekonomi Bernilai." },
  { "en": "Apa Itu Liabilitas?", "id": "Kewajiban Atau Utang Finansial." },
  { "en": "Apa Itu Ekuitas?", "id": "Nilai Kepemilikan Dalam Suatu Aset." },
  { "en": "Apa Itu Laporan Laba Rugi?", "id": "Meringkas Pendapatan, Biaya Selama Periode." },
  { "en": "Apa Itu Neraca (Balance Sheet)?", "id": "Laporan Posisi Keuangan Perusahaan." },
  { "en": "Apa Itu Laporan Arus Kas?", "id": "Menunjukkan Pergerakan Kas Masuk, Keluar." },
  { "en": "Apa Itu Akuntansi?", "id": "Pengukuran, Pemrosesan, Komunikasi Informasi Keuangan." },
  { "en": "Apa Itu Audit?", "id": "Pemeriksaan Independen Informasi Keuangan." },
  { "en": "Apa Itu Depresiasi?", "id": "Penurunan Nilai Aset Seiring Waktu." },
  { "en": "Apa Itu Amortisasi?", "id": "Penyebaran Biaya Aset Tak Berwujud." },
  { "en": "Apa Itu Modal Ventura (Venture Capital)?", "id": "Pendanaan Untuk Perusahaan Rintisan (Startup)." },
  { "en": "Apa Itu Penawaran Umum Perdana (IPO)?", "id": "Penawaran Saham Perusahaan Ke Publik." },
  { "en": "Apa Itu Dividen?", "id": "Pembagian Laba Kepada Pemegang Saham." },
  { "en": "Apa Itu Kapitalisasi Pasar?", "id": "Total Nilai Pasar Saham Perusahaan." },
  { "en": "Apa Itu Analisis Fundamental?", "id": "Mengevaluasi Nilai Intrinsik Suatu Aset." },
  { "en": "Apa Itu Analisis Teknikal?", "id": "Menganalisis Statistik Dari Aktivitas Pasar." },
  { "en": "Apa Itu Indeks Pasar Saham?", "id": "Ukuran Bagian Pasar Saham." },
  { "en": "Apa Itu Dow Jones Industrial Average (DJIA)?", "id": "Indeks Pasar Saham Terkenal." },
  { "en": "Apa Itu S&P 500?", "id": "Indeks 500 Perusahaan Besar Amerika." },
  { "en": "Apa Itu Reksadana (Mutual Fund)?", "id": "Kumpulan Dana Investasi Dikelola Profesional." },
  { "en": "Apa Itu Exchange-Traded Fund (ETF)?", "id": "Reksadana Yang Diperdagangkan Seperti Saham." },
  { "en": "Apa Itu Hedge Fund?", "id": "Dana Investasi Alternatif." },
  { "en": "Apa Itu Derivatif?", "id": "Kontrak Keuangan Nilainya Dari Aset." },
  { "en": "Apa Itu Opsi (Option)?", "id": "Kontrak Memberi Hak Membeli, Menjual." },
  { "en": "Apa Itu Kontrak Berjangka (Futures Contract)?", "id": "Perjanjian Membeli, Menjual Aset Nanti." },
  { "en": "Apa Itu Manajemen Risiko?", "id": "Mengidentifikasi, Menganalisis, Memitigasi Risiko." },
  { "en": "Apa Itu Diversifikasi?", "id": "Menyebar Investasi Mengurangi Risiko." },
  { "en": "Apa Itu Asuransi?", "id": "Perlindungan Dari Kerugian Finansial." },
  { "en": "Apa Itu Premi?", "id": "Pembayaran Untuk Polis Asuransi." },
  { "en": "Apa Itu Deductible?", "id": "Jumlah Dibayar Sebelum Asuransi Aktif." },
  { "en": "Apa Itu Bunga Majemuk?", "id": "Bunga Dihitung Atas Pokok, Bunga." },
  { "en": "Apa Itu Hukum Ampere?", "id": "Menghubungkan Arus Dengan Sirkulasi Medan Magnet." },
  { "en": "Apa Itu Gaya Gerak Magnet (MMF)?", "id": "Penyebab Timbulnya Fluks Magnetik." },
  { "en": "Apa Satuan Gaya Gerak Magnet?", "id": "Ampere-Lilit (Ampere-Turns)." },
  { "en": "Apa Itu Reluktansi?", "id": "Hambatan Terhadap Pembentukan Fluks Magnetik." },
  { "en": "Apa Itu Permeabilitas?", "id": "Kemampuan Material Mendukung Medan Magnet." },
  { "en": "Apa Itu Kurva B-H?", "id": "Grafik Hubungan Kerapatan Fluks, Kuat Medan." },
  { "en": "Apa Itu Saturasi Magnetik?", "id": "Kondisi Material Tak Dapat Dimagnetisasi Lagi." },
  { "en": "Apa Itu Histeresis?", "id": "Keterlambatan Magnetisasi Terhadap Perubahan Medan." },
  { "en": "Apa Itu Kerugian Histeresis?", "id": "Energi Hilang Akibat Proses Histeresis." },
  { "en": "Apa Itu Arus Eddy?", "id": "Arus Sirkulasi Dalam Material Konduktif." },
  { "en": "Bagaimana Mengurangi Kerugian Arus Eddy?", "id": "Menggunakan Inti Berlaminasi Atau Inti Ferit." },
  { "en": "Apa Itu Transformator Ideal?", "id": "Transformator Tanpa Kerugian Energi Sama Sekali." },
  { "en": "Apa Itu Rasio Lilitan Transformator?", "id": "Rasio Jumlah Lilitan Primer, Sekunder." },
  { "en": "Apa Itu Uji Sirkuit Terbuka Trafo?", "id": "Menentukan Kerugian Inti Besi." },
  { "en": "Apa Itu Uji Hubung Singkat Trafo?", "id": "Menentukan Kerugian Tembaga (Beban Penuh)." },
  { "en": "Apa Itu Regulasi Tegangan Transformator?", "id": "Perubahan Tegangan Sekunder Dari Tanpa Beban." },
  { "en": "Apa Itu Efisiensi Transformator?", "id": "Rasio Daya Output Terhadap Daya Input." },
  { "en": "Apa Itu Autotransformer?", "id": "Transformator Dengan Hanya Satu Lilitan." },
  { "en": "Apa Itu Mesin DC (Direct Current)?", "id": "Mesin Listrik Beroperasi Dengan Arus Searah." },
  { "en": "Apa Fungsi Komutator Pada Mesin DC?", "id": "Membalik Arah Arus Dalam Lilitan Jangkar." },
  { "en": "Apa Fungsi Sikat (Brush) Pada Mesin DC?", "id": "Menghubungkan Sirkuit Eksternal Ke Komutator." },
  { "en": "Apa Itu Jangkar (Armature)?", "id": "Bagian Berputar Dari Mesin DC." },
  { "en": "Apa Itu Reaksi Jangkar?", "id": "Efek Medan Magnet Jangkar Terhadap Medan Utama." },
  { "en": "Apa Itu Interpole?", "id": "Kutub Tambahan Mengurangi Percikan Api Komutasi." },
  { "en": "Apa Itu GGL (Gaya Gerak Listrik) Balik?", "id": "Tegangan Diinduksi Melawan Arus Sumber." },
  { "en": "Bagaimana Mengontrol Kecepatan Motor DC Shunt?", "id": "Mengatur Tegangan Jangkar Atau Arus Medan." },
  { "en": "Apa Karakteristik Motor DC Seri?", "id": "Torsi Awal Sangat Tinggi." },
  { "en": "Apa Itu Mesin Sinkron?", "id": "Mesin AC Berputar Pada Kecepatan Sinkron." },
  { "en": "Apa Itu Kecepatan Sinkron?", "id": "Kecepatan Putar Medan Magnet Stator." },
  { "en": "Apa Itu Sudut Daya (Sudut Torsi)?", "id": "Sudut Antara Medan Rotor Dan Stator." },
  { "en": "Apa Itu Hunting Pada Mesin Sinkron?", "id": "Osilasi Rotor Di Sekitar Kecepatan Sinkron." },
  { "en": "Apa Fungsi Lilitan Peredam (Damper Winding)?", "id": "Meredam Osilasi Hunting Pada Mesin Sinkron." },
  { "en": "Apa Itu Kondensor Sinkron?", "id": "Motor Sinkron Tanpa Beban, Penghasil VAR." },
  { "en": "Apa Itu Kurva-V Mesin Sinkron?", "id": "Grafik Arus Jangkar Terhadap Arus Medan." },
  { "en": "Apa Itu Motor Induksi Tiga Fasa?", "id": "Motor AC Paling Umum Digunakan Industri." },
  { "en": "Apa Itu Medan Magnet Berputar?", "id": "Dihasilkan Oleh Suplai Tiga Fasa Stator." },
  { "en": "Apa Itu Slip?", "id": "Perbedaan Persentase Kecepatan Sinkron, Rotor." },
  { "en": "Berapa Nilai Slip Saat Start?", "id": "Satu (1)." },
  { "en": "Berapa Nilai Slip Pada Kecepatan Sinkron?", "id": "Nol (0)." },
  { "en": "Apa Itu Torsi Breakdown (Pull-out)?", "id": "Torsi Maksimum Yang Dapat Dihasilkan Motor." },
  { "en": "Apa Itu Rotor Sangkar Tupai?", "id": "Jenis Rotor Paling Umum, Sederhana, Kuat." },
  { "en": "Apa Itu Rotor Lilit (Wound Rotor)?", "id": "Motor Induksi Resistansi Rotornya Dapat Diubah." },
  { "en": "Apa Itu Metode Start Bintang-Delta?", "id": "Mengurangi Arus Mula Motor Induksi." },
  { "en": "Apa Itu Pengereman Regeneratif?", "id": "Motor Bekerja Sebagai Generator, Mengembalikan Energi." },
  { "en": "Apa Itu Pengereman Plugging?", "id": "Membalik Urutan Fasa Untuk Pengereman Cepat." },
  { "en": "Apa Itu Pengereman Dinamis?", "id": "Energi Dibuang Sebagai Panas Dalam Resistor." },
  { "en": "Apa Itu Motor Induksi Satu Fasa?", "id": "Motor Umum Digunakan Dalam Peralatan Rumah Tangga." },
  { "en": "Mengapa Motor Satu Fasa Tidak Self-Starting?", "id": "Hanya Menghasilkan Medan Magnet Berdenyut." },
  { "en": "Apa Itu Lilitan Bantu (Auxiliary Winding)?", "id": "Lilitan Tambahan Untuk Membantu Start." },
  { "en": "Apa Itu Motor Fasa Terpisah (Split-Phase)?", "id": "Menggunakan Lilitan Bantu Resistansi Tinggi." },
  { "en": "Apa Itu Motor Kapasitor-Start?", "id": "Menggunakan Kapasitor Seri Dengan Lilitan Bantu." },
  { "en": "Apa Itu Motor Kapasitor-Run?", "id": "Kapitor Tetap Terhubung Selama Operasi." },
  { "en": "Apa Itu Motor Shaded-Pole?", "id": "Motor Satu Fasa Paling Sederhana, Murah." },
  { "en": "Apa Itu Motor Universal?", "id": "Dapat Beroperasi Pada Suplai AC, DC." },
  { "en": "Apa Itu Mesin Fraksional Tenaga Kuda?", "id": "Motor Dengan Rating Kurang Dari Satu Tenaga Kuda." },
  { "en": "Apa Itu Sistem Proteksi Tenaga?", "id": "Melindungi Sistem Listrik Dari Gangguan." },
  { "en": "Apa Sifat Utama Relai Proteksi?", "id": "Selektivitas, Kecepatan, Sensitivitas, Keandalan." },
  { "en": "Apa Itu Selektivitas Proteksi?", "id": "Hanya Memutuskan Bagian Terganggu Dari Sistem." },
  { "en": "Apa Itu Zona Proteksi?", "id": "Area Spesifik Yang Dilindungi Oleh Relai." },
  { "en": "Apa Itu Proteksi Cadangan (Backup)?", "id": "Beroperasi Jika Proteksi Utama Gagal." },
  { "en": "Apa Itu Relai Arus Lebih?", "id": "Bekerja Saat Arus Melebihi Nilai Set." },
  { "en": "Apa Itu Relai Waktu Tunda Arus Lebih?", "id": "Waktu Operasi Bergantung Pada Besaran Arus." },
  { "en": "Apa Itu Relai Arus Lebih Seketika?", "id": "Bekerja Seketika Tanpa Penundaan Waktu." },
  { "en": "Apa Itu Relai Diferensial?", "id": "Bekerja Berdasarkan Perbedaan Antara Dua Arus." },
  { "en": "Apa Yang Dilindungi Relai Diferensial?", "id": "Transformator, Generator, Dan Busbar." },
  { "en": "Apa Itu Relai Jarak (Impedansi)?", "id": "Mengukur Impedansi Untuk Mendeteksi Lokasi Gangguan." },
  { "en": "Apa Yang Dilindungi Relai Jarak?", "id": "Saluran Transmisi Listrik Jarak Jauh." },
  { "en": "Apa Itu Relai Frekuensi?", "id": "Mendeteksi Perubahan Frekuensi Sistem." },
  { "en": "Apa Itu Pemutus Sirkuit Udara (ACB)?", "id": "Menggunakan Udara Untuk Memadamkan Busur Api." },
  { "en": "Apa Itu Pemutus Sirkuit Minyak?", "id": "Menggunakan Minyak Sebagai Media Pemadam." },
  { "en": "Apa Itu Pemutus Sirkuit Vakum (VCB)?", "id": "Busur Api Padam Dalam Ruang Hampa." },
  { "en": "Apa Itu Pemutus Sirkuit SF6?", "id": "Menggunakan Gas Sulfur Heksafluorida." },
  { "en": "Apa Keuntungan Gas SF6?", "id": "Kekuatan Dielektrik Sangat Tinggi." },
  { "en": "Apa Itu Arrester Surja (Lightning Arrester)?", "id": "Melindungi Peralatan Dari Lonjakan Tegangan." },
  { "en": "Bagaimana Cara Kerja Arrester Surja?", "id": "Mengalihkan Arus Surja Berlebih Ke Tanah." },
  { "en": "Apa Itu Sistem Pentanahan (Grounding)?", "id": "Koneksi Konduktif Ke Bumi." },
  { "en": "Apa Tujuan Utama Pentanahan?", "id": "Keselamatan Personel Dan Proteksi Peralatan." },
  { "en": "Apa Itu Pentanahan Solid?", "id": "Netral Terhubung Langsung Ke Tanah." },
  { "en": "Apa Itu Pentanahan Tahanan?", "id": "Netral Terhubung Ke Tanah Melalui Resistor." },
  { "en": "Apa Itu Pentanahan Reaktansi?", "id": "Netral Terhubung Ke Tanah Melalui Reaktor." },
  { "en": "Apa Itu Sistem Tak Ditanahkan (Ungrounded)?", "id": "Sistem Tanpa Koneksi Intensional Ke Tanah." },
  { "en": "Apa Itu Kawat Tanah (Shield Wire)?", "id": "Kawat Di Atas Saluran Transmisi." },
  { "en": "Apa Fungsi Kawat Tanah?", "id": "Melindungi Saluran Dari Sambaran Petir Langsung." },
  { "en": "Apa Itu Tegangan Sentuh?", "id": "Beda Potensial Antara Benda, Kaki Seseorang." },
  { "en": "Apa Itu Tegangan Langkah?", "id": "Beda Potensial Antara Dua Kaki Seseorang." },
  { "en": "Apa Itu Grid Pentanahan Gardu Induk?", "id": "Jaringan Konduktor Mengontrol Potensial Tanah." },
  { "en": "Apa Itu Isolasi Listrik?", "id": "Material Yang Menghambat Aliran Arus Listrik." },
  { "en": "Apa Itu Kekuatan Dielektrik?", "id": "Kuat Medan Listrik Maksimum Sebelum Tembus." },
  { "en": "Apa Itu Flashover?", "id": "Pelepasan Muatan Di Sepanjang Permukaan Isolator." },
  { "en": "Apa Itu Puncture (Tembus)?", "id": "Kerusakan Isolasi Melalui Badan Material." },
  { "en": "Apa Itu Jarak Rambat (Creepage Distance)?", "id": "Jarak Terpendek Di Sepanjang Permukaan Isolator." },
  { "en": "Apa Itu Jarak Bebas (Clearance Distance)?", "id": "Jarak Terpendek Melalui Udara." },
  { "en": "Apa Itu Insulator Pin?", "id": "Isolator Digunakan Pada Jaringan Distribusi." },
  { "en": "Apa Itu Insulator Gantung (Suspension)?", "id": "Rangkaian Piringan Untuk Tegangan Tinggi." },
  { "en": "Apa Itu Insulator Strain?", "id": "Digunakan Pada Tiang Sudut Atau Ujung." },
  { "en": "Apa Itu Insulator Polimer?", "id": "Isolator Dibuat Dari Bahan Komposit." },
  { "en": "Apa Itu Efek Korona?", "id": "Pelepasan Muatan Listrik Di Udara." },
  { "en": "Kapan Efek Korona Terjadi?", "id": "Pada Konduktor Tegangan Sangat Tinggi." },
  { "en": "Apa Dampak Negatif Korona?", "id": "Kerugian Daya, Derau Radio, Produksi Ozon." },
  { "en": "Apa Itu Cincin Korona (Grading Ring)?", "id": "Membantu Meratakan Distribusi Medan Listrik." },
  { "en": "Apa Itu Kabel Bawah Laut?", "id": "Kabel Dirancang Untuk Diletakkan Di Dasar Laut." },
  { "en": "Apa Tantangan Utama Kabel Bawah Laut?", "id": "Tekanan Air Tinggi Dan Korosi Air Asin." },
  { "en": "Apa Itu Gardu Induk Berisolasi Gas (GIS)?", "id": "Gardu Induk Kompak Menggunakan Gas SF6." },
  { "en": "Apa Keuntungan GIS (Gas-Insulated Substation)?", "id": "Ukuran Jauh Lebih Kecil, Keandalan Tinggi." },
  { "en": "Apa Itu Gardu Induk Berisolasi Udara (AIS)?", "id": "Gardu Induk Konvensional Menggunakan Udara Terbuka." },
  { "en": "Apa Itu Sistem Tenaga Fleksibel AC (FACTS)?", "id": "Perangkat Elektronika Daya Peningkat Kontrol Jaringan." },
  { "en": "Apa Itu Static VAR Compensator (SVC)?", "id": "Perangkat FACTS (Flexible AC Transmission Systems) Pengatur Tegangan Cepat." },
  { "en": "Apa Itu STATCOM (Static Synchronous Compensator)?", "id": "SVC (Static VAR Compensator) Berbasis Konverter Sumber Tegangan." },
  { "en": "Apa Itu Unified Power Flow Controller (UPFC)?", "id": "Perangkat FACTS (Flexible AC Transmission Systems) Paling Serbaguna." },
  { "en": "Apa Fungsi UPFC (Unified Power Flow Controller)?", "id": "Mengontrol Aliran Daya Aktif, Reaktif, Tegangan." },
  { "en": "Apa Itu Thyristor Controlled Series Capacitor (TCSC)?", "id": "Kapasitor Seri Reaktansinya Dapat Diatur." },
  { "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Karakteristik Tegangan, Arus, Frekuensi Listrik." },
  { "en": "Apa Itu Harmonik?", "id": "Komponen Frekuensi Kelipatan Dari Frekuensi Dasar." },
  { "en": "Apa Sumber Utama Harmonik?", "id": "Beban Non-linear Seperti Konverter Daya." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran Total Distorsi Harmonik." },
  { "en": "Apa Dampak Negatif Harmonik?", "id": "Pemanasan Berlebih, Gangguan, Penurunan Efisiensi." },
  { "en": "Apa Itu Filter Harmonik Pasif?", "id": "Jaringan LC (Induktor-Kapasitor) Ditala Ke Frekuensi Harmonik." },
  { "en": "Apa Itu Filter Harmonik Aktif?", "id": "Menginjeksikan Arus Kompensasi Menghilangkan Harmonik." },
  { "en": "Apa Itu Sag Tegangan (Voltage Sag)?", "id": "Penurunan Tegangan Jangka Pendek." },
  { "en": "Apa Itu Swell Tegangan (Voltage Swell)?", "id": "Kenaikan Tegangan Jangka Pendek Ketidakseimbangan." },
  { "en": "Apa Itu Transien?", "id": "Perubahan Tegangan Atau Arus Durasi Singkat." },
  { "en": "Apa Itu Flicker Tegangan?", "id": "Variasi Amplitudo Tegangan Yang Terlihat." },
  { "en": "Apa Itu Notching Tegangan?", "id": "Gangguan Periodik Akibat Komutasi Konverter." },
  { "en": "Apa Itu Ketidakseimbangan Tegangan (Voltage Unbalance)?", "id": "Besaran Tegangan Antar Fasa Tidak Sama." },
  { "en": "Apa Itu Sistem SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan, Kontrol Jarak Jauh." },
  { "en": "Apa Itu Remote Terminal Unit (RTU)?", "id": "Perangkat Lapangan Dalam Sistem SCADA." },
  { "en": "Apa Itu Master Station Dalam SCADA?", "id": "Pusat Kontrol Menerima Data Dari RTU." },
  { "en": "Apa Itu Human-Machine Interface (HMI)?", "id": "Antarmuka Grafis Bagi Operator." },
  { "en": "Apa Itu Protokol DNP3?", "id": "Protokol Komunikasi Umum Dalam Sistem Tenaga." },
  { "en": "Apa Itu Standar IEC 61850?", "id": "Standar Komunikasi Otomasi Gardu Induk." },
  { "en": "Apa Itu Pesan GOOSE?", "id": "Generic Object Oriented Substation Event." },
  { "en": "Apa Fungsi Pesan GOOSE?", "id": "Komunikasi Cepat Antar Perangkat Cerdas (IED)." },
  { "en": "Apa Itu Intelligent Electronic Device (IED)?", "id": "Perangkat Mikroprosesor Dalam Otomasi Gardu Induk." },
  { "en": "Apa Itu Pembangkit Listrik?", "id": "Fasilitas Pembangkit Energi Listrik." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Termal?", "id": "Menggunakan Panas Untuk Menghasilkan Listrik." },
  { "en": "Apa Itu Siklus Rankine?", "id": "Siklus Termodinamika Ideal Pembangkit Uap." },
  { "en": "Apa Itu Boiler (Ketel Uap)?", "id": "Memanaskan Air Menjadi Uap Bertekanan Tinggi." },
  { "en": "Apa Itu Turbin Uap?", "id": "Mengubah Energi Panas Uap Menjadi Rotasi." },
  { "en": "Apa Itu Kondensor?", "id": "Mengubah Uap Kembali Menjadi Air." },
  { "en": "Apa Itu Menara Pendingin?", "id": "Membuang Panas Sisa Dari Kondensor." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Nuklir?", "id": "Menggunakan Reaksi Fisi Nuklir." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air?", "id": "Menggunakan Energi Potensial Air." },
  { "en": "Apa Itu Turbin Francis?", "id": "Jenis Turbin Air Paling Umum." },
  { "en": "Apa Itu Turbin Pelton?", "id": "Turbin Air Impuls Untuk Head Tinggi." },
  { "en": "Apa Itu Turbin Kaplan?", "id": "Turbin Air Baling-baling Untuk Head Rendah." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Angin?", "id": "Mengubah Energi Kinetik Angin Menjadi Listrik." },
  { "en": "Apa Itu Batas Betz?", "id": "Efisiensi Teoritis Maksimum Turbin Angin." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Surya Fotovoltaik?", "id": "Mengubah Cahaya Matahari Langsung Jadi Listrik." },
  { "en": "Apa Itu Maximum Power Point Tracking (MPPT)?", "id": "Memaksimalkan Output Daya Panel Surya." },
  { "en": "Apa Itu Inverter Terikat Jaringan (Grid-Tied)?", "id": "Menyinkronkan Output DC Ke Jaringan AC." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Panas Bumi?", "id": "Menggunakan Panas Dari Dalam Bumi." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Gas (PLTG)?", "id": "Menggunakan Turbin Gas Memutar Generator." },
  { "en": "Apa Itu Siklus Brayton?", "id": "Siklus Termodinamika Ideal Turbin Gas." },
  { "en": "Apa Itu Pembangkit Siklus Gabungan (PLTGU)?", "id": "Menggabungkan Siklus Turbin Gas, Uap." },
  { "en": "Apa Keuntungan Siklus Gabungan?", "id": "Efisiensi Termal Sangat Tinggi." },
  { "en": "Apa Itu Cogeneration (CHP)?", "id": "Menghasilkan Listrik Dan Panas Berguna Bersamaan." },
  { "en": "Apa Itu Analisis Aliran Beban?", "id": "Nama Lain Untuk Studi Aliran Daya." },
  { "en": "Apa Itu Bus Ayun (Slack Bus)?", "id": "Bus Referensi Menyeimbangkan Daya Sistem." },
  { "en": "Apa Itu Bus Beban (PQ Bus)?", "id": "Bus Konsumsi Daya Aktif, Reaktif." },
  { "en": "Apa Itu Bus Generator (PV Bus)?", "id": "Bus Mengatur Daya Aktif, Tegangan." },
  { "en": "Apa Itu Metode Newton-Raphson?", "id": "Metode Iteratif Cepat Analisis Aliran Daya." },
  { "en": "Apa Itu Metode Gauss-Seidel?", "id": "Metode Iteratif Lebih Sederhana Aliran Daya." },
  { "en": "Apa Itu Matriks Admitansi Bus (Ybus)?", "id": "Matriks Merepresentasikan Jaringan Sistem Tenaga." },
  { "en": "Apa Itu Analisis Kontingensi?", "id": "Mempelajari Dampak Kegagalan Komponen Sistem." },
  { "en": "Apa Itu Kontrol Ekonomis (Economic Dispatch)?", "id": "Membagi Beban Antar Pembangkit Paling Ekonomis." },
  { "en": "Apa Itu Faktor Penalti?", "id": "Memperhitungkan Kerugian Transmisi Dalam Dispatch." },
  { "en": "Apa Itu Stabilitas Sistem Tenaga?", "id": "Kemampuan Sistem Kembali Ke Keseimbangan." },
  { "en": "Apa Itu Stabilitas Sudut Rotor?", "id": "Kemampuan Generator Tetap Sinkron." },
  { "en": "Apa Itu Stabilitas Tegangan?", "id": "Kemampuan Sistem Menjaga Tegangan Diterima." },
  { "en": "Apa Itu Runtuh Tegangan (Voltage Collapse)?", "id": "Penurunan Tegangan Drastis Tak Terkendali." },
  { "en": "Apa Itu Persamaan Ayunan (Swing Equation)?", "id": "Mendeskripsikan Dinamika Sudut Rotor Generator." },
  { "en": "Apa Itu Kriteria Luas Sama?", "id": "Metode Grafis Analisis Stabilitas Transien." },
  { "en": "Apa Itu Waktu Pembersihan Kritis (CCT)?", "id": "Waktu Maksimum Gangguan Sebelum Ketidakstabilan." },
  { "en": "Apa Itu Osilasi Daya?", "id": "Fluktuasi Daya Antar Area Sistem Tenaga." },
  { "en": "Apa Itu Power System Stabilizer (PSS)?", "id": "Kontroler Tambahan Meredam Osilasi Daya." },
  { "en": "Apa Itu Pemodelan Sinyal Kecil?", "id": "Menganalisis Stabilitas Di Sekitar Titik Operasi." },
  { "en": "Apa Itu Nilai Eigen (Eigenvalue) Sistem?", "id": "Menentukan Mode Osilasi Sistem." },
  { "en": "Apa Itu Relay Numerik?", "id": "Relai Proteksi Berbasis Mikroprosesor." },
  { "en": "Apa Itu Skema Proteksi Pilot?", "id": "Menggunakan Kanal Komunikasi Antar Ujung Saluran." },
  { "en": "Apa Itu Proteksi Diferensial Arus?", "id": "Membandingkan Arus Masuk, Keluar Zona." },
  { "en": "Apa Itu Perekam Gangguan Digital (DFR)?", "id": "Merekam Bentuk Gelombang Selama Gangguan." },
  { "en": "Apa Itu Pencari Lokasi Gangguan?", "id": "Mengestimasi Lokasi Fisik Gangguan Saluran." },
  { "en": "Apa Itu Sinkrofasor (Synchrophasor)?", "id": "Pengukuran Fasor Tersinkronisasi Waktu Akurat." },
  { "en": "Apa Itu Wide Area Monitoring System (WAMS)?", "id": "Sistem Pemantauan Jaringan Menggunakan Sinkrofasor." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?", "id": "Model Komputasi Terinspirasi Jaringan Saraf Biologis." },
  { "en": "Aplikasi Apa ANN (Artificial Neural Network) Dalam Sistem Tenaga?", "id": "Peramalan Beban, Deteksi Gangguan." },
  { "en": "Apa Itu Logika Fuzzy?", "id": "Logika Berurusan Dengan Kebenaran Parsial." },
  { "en": "Aplikasi Apa Logika Fuzzy Dalam Sistem Tenaga?", "id": "Kontrol Pembangkit, Diagnosis Gangguan." },
  { "en": "Apa Itu Algoritma Genetika?", "id": "Algoritma Optimisasi Terinspirasi Evolusi." },
  { "en": "Aplikasi Apa Algoritma Genetika Dalam Sistem Tenaga?", "id": "Penjadwalan Pembangkit Optimal." },
  { "en": "Apa Itu Pasar Listrik?", "id": "Mekanisme Pasar Untuk Perdagangan Listrik." },
  { "en": "Apa Itu Pasar Spot Listrik?", "id": "Perdagangan Listrik Jangka Sangat Pendek." },
  { "en": "Apa Itu Independent System Operator (ISO)?", "id": "Operator Independen Mengelola Jaringan Listrik." },
  { "en": "Apa Itu Jasa Tambahan (Ancillary Services)?", "id": "Layanan Pendukung Keandalan Jaringan." },
  { "en": "Apa Itu Regulasi Frekuensi?", "id": "Jasa Tambahan Menjaga Frekuensi Sistem." },
  { "en": "Apa Itu Cadangan Operasi?", "id": "Kapasitas Pembangkit Siaga Untuk Kontingensi." },
  { "en": "Apa Itu Elektroluminesensi?", "id": "Emisi Cahaya Dari Material Akibat Arus Listrik." },
  { "en": "Apa Itu Dioda Terowongan (Tunnel Diode)?", "id": "Dioda Menunjukkan Resistansi Diferensial Negatif." },
  { "en": "Apa Aplikasi Dioda Terowongan?", "id": "Osilator Dan Amplifier Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Dioda Gunn?", "id": "Dioda Untuk Membangkitkan Gelombang Mikro." },
  { "en": "Apa Itu Efek Gunn?", "id": "Timbulnya Osilasi Frekuensi Tinggi Pada Semikonduktor." },
  { "en": "Apa Itu Dioda IMPATT?", "id": "Dioda Pembangkit Gelombang Mikro Berdaya Tinggi." },
  { "en": "Apa Kepanjangan DARI IMPATT?", "id": "Impact Ionization Avalanche Transit-Time." },
  { "en": "Apa Itu Dioda Varactor?", "id": "Dioda Berfungsi Sebagai Kapasitor Terkendali Tegangan." },
  { "en": "Apa Aplikasi Dioda Varactor?", "id": "Sirkuit Penalaan Frekuensi (Tuning Circuit)." },
  { "en": "Apa Itu Dioda Step Recovery?", "id": "Dioda Dengan Waktu Pemulihan Balik Sangat Cepat." },
  { "en": "Apa Aplikasi Dioda Step Recovery?", "id": "Pembangkit Harmonik Dan Pembentuk Pulsa." },
  { "en": "Apa Itu Dioda PIN?", "id": "Dioda Dengan Lapisan Intrinsik Di Antaranya." },
  { "en": "Apa Aplikasi Dioda PIN?", "id": "Sakelar RF Dan Atenuator Variabel." },
  { "en": "Apa Itu Atenuator?", "id": "Rangkaian Untuk Melemahkan Kekuatan Sinyal." },
  { "en": "Apa Itu Termistor NTC (Negative Temperature Coefficient)?", "id": "Resistansi Turun Saat Suhu Naik." },
  { "en": "Apa Itu Termistor PTC (Positive Temperature Coefficient)?", "id": "Resistansi Naik Saat Suhu Naik." },
  { "en": "Apa Aplikasi Termistor NTC?", "id": "Pengukuran Suhu, Pembatas Arus Mula." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Berbasis Resistansi Logam." },
  { "en": "Bahan Apa Paling Umum Untuk RTD?", "id": "Platina (Platinum)." },
  { "en": "Mana Yang Lebih Linear, Termistor Atau RTD?", "id": "RTD (Resistance Temperature Detector) Lebih Linear." },
  { "en": "Apa Itu Pengukur Regangan (Strain Gauge)?", "id": "Sensor Mengukur Deformasi Mekanis." },
  { "en": "Apa Itu Faktor Pengukur (Gauge Factor)?", "id": "Ukuran Sensitivitas Pengukur Regangan." },
  { "en": "Apa Itu Load Cell?", "id": "Sensor Mengubah Gaya Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Akselerometer?", "id": "Sensor Mengukur Percepatan Dan Getaran." },
  { "en": "Apa Itu Giroskop?", "id": "Sensor Mengukur Kecepatan Atau Orientasi Sudut." },
  { "en": "Apa Itu Piezoelektrik Terbalik?", "id": "Tegangan Listrik Menyebabkan Perubahan Bentuk." },
  { "en": "Apa Aplikasi Efek Piezoelektrik Terbalik?", "id": "Aktuator Piezo, Buzzer, Injektor Bahan Bakar." },
  { "en": "Apa Itu Buzzer Piezoelektrik?", "id": "Perangkat Penghasil Suara Menggunakan Kristal Piezo." },
  { "en": "Apa Itu Transduser Ultrasonik?", "id": "Mengubah Listrik Menjadi Gelombang Suara Ultrasonik." },
  { "en": "Apa Itu Linear Variable Differential Transformer (LVDT)?", "id": "Sensor Posisi Linear Yang Sangat Akurat." },
  { "en": "Apa Prinsip Kerja LVDT?", "id": "Induktansi Timbal Balik Antara Kumparan." },
  { "en": "Apa Itu Resolver?", "id": "Sensor Posisi Sudut Rotari Analog." },
  { "en": "Apa Itu Sinkro (Synchro)?", "id": "Jenis Transformator Rotari Pengirim Sudut." },
  { "en": "Apa Itu Encoder Absolut?", "id": "Memberikan Posisi Sudut Absolut Unik." },
  { "en": "Apa Itu Encoder Inkremental?", "id": "Menghasilkan Pulsa Untuk Setiap Kenaikan Gerakan." },
  { "en": "Apa Itu Saluran Kuadratur Pada Encoder?", "id": "Dua Sinyal (A, B) Menentukan Arah." },
  { "en": "Apa Itu Proximity Sensor Induktif?", "id": "Mendeteksi Objek Logam Tanpa Kontak." },
  { "en": "Apa Itu Proximity Sensor Kapasitif?", "id": "Mendeteksi Objek Logam Maupun Non-logam." },
  { "en": "Apa Itu Sensor Fotoelektrik?", "id": "Mendeteksi Objek Menggunakan Berkas Cahaya." },
  { "en": "Apa Itu Mode Thru-Beam Sensor Fotoelektrik?", "id": "Pemancar Dan Penerima Terpisah." },
  { "en": "Apa Itu Mode Retro-Reflective?", "id": "Menggunakan Reflektor Untuk Memantulkan Berkas." },
  { "en": "Apa Itu Mode Diffuse?", "id": "Objek Sendiri Memantulkan Berkas Cahaya." },
  { "en": "Apa Itu Sensor Visi?", "id": "Menggunakan Kamera Untuk Inspeksi Otomatis." },
  { "en": "Apa Itu Kontrol Numerik Komputer (CNC)?", "id": "Kontrol Otomatis Mesin Perkakas." },
  { "en": "Apa Itu Kode-G?", "id": "Bahasa Pemrograman Paling Umum Mesin CNC." },
  { "en": "Apa Itu Manufaktur Aditif (3D Printing)?", "id": "Membangun Objek Lapisan Demi Lapisan." },
  { "en": "Apa Itu Fused Deposition Modeling (FDM)?", "id": "Teknik Umum Pencetakan 3D Menggunakan Filamen." },
  { "en": "Apa Itu Stereolithography (SLA)?", "id": "Teknik Pencetakan 3D Menggunakan Resin Fotosensitif." },
  { "en": "Apa Itu Selective Laser Sintering (SLS)?", "id": "Menggunakan Laser Untuk Menyinter Material Bubuk." },
  { "en": "Apa Itu Sistem Siber-Fisik (CPS)?", "id": "Integrasi Erat Antara Komputasi Dan Proses Fisik." },
  { "en": "Apa Itu Industri 4.0?", "id": "Revolusi Industri Keempat (Otomasi Cerdas)." },
  { "en": "Apa Itu Digital Twin?", "id": "Representasi Digital Virtual Dari Objek Fisik." },
  { "en": "Apa Manfaat Digital Twin?", "id": "Simulasi, Pemantauan, Dan Prediksi Kinerja." },
  { "en": "Apa Itu Pemeliharaan Prediktif?", "id": "Memprediksi Kapan Kegagalan Akan Terjadi." },
  { "en": "Apa Itu Analisis Getaran?", "id": "Teknik Umum Dalam Pemeliharaan Prediktif Mesin." },
  { "en": "Apa Itu Termografi Inframerah?", "id": "Menggunakan Kamera Termal Mendeteksi Masalah Panas." },
  { "en": "Apa Itu Analisis Oli?", "id": "Menganalisis Partikel Aus Dalam Oli Pelumas." },
  { "en": "Apa Itu Root Cause Analysis (RCA)?", "id": "Metode Penemuan Akar Penyebab Suatu Masalah." },
  { "en": "Apa Itu Keandalan (Reliability)?", "id": "Probabilitas Sistem Berfungsi Tanpa Gagal." },
  { "en": "Apa Itu Ketersediaan (Availability)?", "id": "Probabilitas Sistem Beroperasi Saat Dibutuhkan." },
  { "en": "Apa Itu Maintainability?", "id": "Kemudahan Suatu Sistem Untuk Diperbaiki." },
  { "en": "Apa Itu Mean Time To Failure (MTTF)?", "id": "Waktu Rata-rata Hingga Kegagalan Pertama." },
  { "en": "Apa Itu Mean Time Between Failures (MTBF)?", "id": "Waktu Rata-rata Antar Kegagalan Berurutan." },
  { "en": "Apa Itu Mean Time To Repair (MTTR)?", "id": "Waktu Rata-rata Untuk Memperbaiki Sistem." },
  { "en": "Apa Itu Redundansi?", "id": "Duplikasi Komponen Kritis Meningkatkan Keandalan." },
  { "en": "Apa Itu Redundansi Aktif?", "id": "Semua Komponen Redundan Beroperasi Bersamaan." },
  { "en": "Apa Itu Redundansi Pasif (Standby)?", "id": "Komponen Cadangan Mengambil Alih Saat Gagal." },
  { "en": "Apa Itu Fail-Safe Design?", "id": "Desain Meminimalkan Kerusakan Saat Terjadi Kegagalan." },
  { "en": "Apa Itu Fault Tolerance?", "id": "Kemampuan Sistem Beroperasi Meski Ada Kegagalan." },
  { "en": "Apa Itu Sistem N-Modular Redundancy (NMR)?", "id": "Menggunakan N Modul Dan Voting." },
  { "en": "Apa Itu Triple Modular Redundancy (TMR)?", "id": "Menggunakan Tiga Modul Identik Dan Voting." },
  { "en": "Apa Itu Safety Integrity Level (SIL)?", "id": "Ukuran Kinerja Keselamatan Sistem." },
  { "en": "Apa Itu Hazard and Operability Study (HAZOP)?", "id": "Studi Identifikasi Bahaya, Masalah Operabilitas." },
  { "en": "Apa Itu Keselamatan Fungsional?", "id": "Bagian Keselamatan Bergantung Pada Sistem Kontrol." },
  { "en": "Apa Itu Standar IEC 61508?", "id": "Standar Internasional Untuk Keselamatan Fungsional." },
  { "en": "Apa Itu Probability of Failure on Demand (PFD)?", "id": "Probabilitas Sistem Gagal Berfungsi Saat Diperlukan." },
  { "en": "Apa Itu Desain Berpusat Pada Manusia?", "id": "Proses Desain Berfokus Kebutuhan, Batasan Pengguna." },
  { "en": "Apa Itu Usability?", "id": "Kemudahan Penggunaan Suatu Produk." },
  { "en": "Apa Itu Heuristic Evaluation?", "id": "Metode Inspeksi Usability." },
  { "en": "Apa Itu User Persona?", "id": "Karakter Fiksi Mewakili Target Pengguna." },
  { "en": "Apa Itu Wireframing?", "id": "Membuat Sketsa Struktur Dasar Antarmuka." },
  { "en": "Apa Itu Prototyping?", "id": "Membuat Model Awal Suatu Produk." },
  { "en": "Apa Itu Pengujian Pengguna (User Testing)?", "id": "Mengamati Pengguna Berinteraksi Dengan Produk." },
  { "en": "Apa Itu Model Mental?", "id": "Pemahaman Seseorang Tentang Cara Kerja Sesuatu." },
  { "en": "Apa Itu Affordance?", "id": "Sifat Objek Menunjukkan Bagaimana Menggunakannya." },
  { "en": "Apa Itu Signifier?", "id": "Petunjuk Komunikasi Di Mana Aksi Dilakukan." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Informasi Kembali Ke Pengguna Tentang Aksi." },
  { "en": "Apa Itu Konstrain (Constraint)?", "id": "Batasan Yang Membatasi Pilihan Pengguna." },
  { "en": "Apa Itu Pemetaan (Mapping)?", "id": "Hubungan Antara Kontrol Dan Efeknya." },
  { "en": "Apa Itu Hukum Miller (Psikologi)?", "id": "Manusia Dapat Menyimpan 7±2 Item." },
  { "en": "Apa Itu Hukum Tesler (Hukum Kekekalan Kompleksitas)?", "id": "Setiap Sistem Memiliki Kompleksitas Inheren." },
  { "en": "Apa Itu Efek Von Restorff?", "id": "Item Yang Menonjol Lebih Mungkin Diingat." },
  { "en": "Apa Itu Efek Posisi Serial?", "id": "Kecenderungan Mengingat Item Pertama, Terakhir." },
  { "en": "Apa Itu Teori Gestalt?", "id": "Prinsip Persepsi Visual." },
  { "en": "Apa Itu Prinsip Kedekatan (Proximity)?", "id": "Objek Berdekatan Dipersepsikan Sebagai Kelompok." },
  { "en": "Apa Itu Prinsip Kesamaan (Similarity)?", "id": "Objek Serupa Dipersepsikan Sebagai Kelompok." },
  { "en": "Apa Itu Prinsip Penutupan (Closure)?", "id": "Otak Cenderung Melengkapi Bentuk Tak Lengkap." },
  { "en": "Apa Itu Prinsip Figure-Ground?", "id": "Persepsi Memisahkan Objek Dari Latar Belakang." },
  { "en": "Apa Itu Konduksi Listrik?", "id": "Aliran Pembawa Muatan Listrik Melalui Material." },
  { "en": "Apa Itu Konduktivitas Listrik?", "id": "Ukuran Kemampuan Material Menghantarkan Arus Listrik." },
  { "en": "Apa Itu Resistivitas Listrik?", "id": "Ukuran Kemampuan Material Menentang Arus Listrik." },
  { "en": "Apa Hubungan Konduktivitas Dan Resistivitas?", "id": "Satu Adalah Kebalikan Dari Yang Lain." },
  { "en": "Apa Itu Hukum Ohm (Bentuk Mikroskopis)?", "id": "Kerapatan Arus Proporsional Terhadap Medan Listrik." },
  { "en": "Apa Itu Kerapatan Arus (J)?", "id": "Jumlah Arus Listrik Per Satuan Luas." },
  { "en": "Apa Itu Model Elektron Drude?", "id": "Model Klasik Menjelaskan Konduktivitas Logam." },
  { "en": "Apa Itu Waktu Relaksasi Momentum?", "id": "Waktu Rata-rata Elektron Sebelum Mengalami Tumbukan." },
  { "en": "Apa Itu Kecepatan Hanyut (Drift Velocity)?", "id": "Kecepatan Rata-rata Pembawa Muatan." },
  { "en": "Apa Itu Mobilitas Pembawa Muatan?", "id": "Seberapa Cepat Pembawa Muatan Bergerak." },
  { "en": "Apa Itu Elektrolit?", "id": "Zat Menghasilkan Larutan Konduktif Listrik." },
  { "en": "Apa Itu Ion?", "id": "Atom Atau Molekul Dengan Muatan Listrik." },
  { "en": "Apa Itu Anion?", "id": "Ion Bermuatan Negatif." },
  { "en": "Apa Itu Kation?", "id": "Ion Bermuatan Positif." },
  { "en": "Apa Itu Plasma?", "id": "Keadaan Materi Keempat, Gas Terionisasi." },
  { "en": "Apa Itu Teori Pita Energi?", "id": "Model Kuantum Menjelaskan Konduktivitas Padatan." },
  { "en": "Apa Itu Pita Valensi?", "id": "Pita Energi Tertinggi Terisi Elektron." },
  { "en": "Apa Itu Pita Konduksi?", "id": "Pita Energi Di Atas Pita Valensi." },
  { "en": "Apa Itu Celah Pita (Band Gap)?", "id": "Pemisahan Energi Antara Pita Valensi, Konduksi." },
  { "en": "Mengapa Logam Adalah Konduktor Baik?", "id": "Pita Valensi Dan Konduksi Tumpang Tindih." },
  { "en": "Mengapa Isolator Adalah Konduktor Buruk?", "id": "Memiliki Celah Pita Energi Yang Sangat Lebar." },
  { "en": "Apa Karakteristik Semikonduktor?", "id": "Celah Pita Energi Sedang Antara Konduktor, Isolator." },
  { "en": "Apa Itu Permukaan Fermi?", "id": "Batas Dalam Ruang Momentum Antara Keadaan." },
  { "en": "Apa Itu Statistik Fermi-Dirac?", "id": "Mendeskripsikan Distribusi Energi Fermion." },
  { "en": "Apa Itu Statistik Bose-Einstein?", "id": "Mendeskripsikan Distribusi Energi Boson." },
  { "en": "Apa Itu Fermion?", "id": "Partikel Dengan Spin Setengah-Bulat (Elektron)." },
  { "en": "Apa Itu Boson?", "id": "Partikel Dengan Spin Bulat (Foton)." },
  { "en": "Apa Itu Efek Hall Kuantum?", "id": "Efek Mekanika Kuantum Dari Efek Hall." },
  { "en": "Apa Itu Isolator Topologis?", "id": "Material Isolator Di Dalam, Konduktif Permukaan." },
  { "en": "Apa Itu Semimetal Dirac?", "id": "Material Dengan Sifat Elektronik Unik." },
  { "en": "Apa Itu Dielektrik?", "id": "Material Isolator Listrik Yang Dapat Dipolarisasi." },
  { "en": "Apa Itu Polarisasi Dielektrik?", "id": "Pergeseran Muatan Positif, Negatif Dalam Dielektrik." },
  { "en": "Apa Itu Konstanta Dielektrik?", "id": "Ukuran Kemampuan Material Menyimpan Energi Listrik." },
  { "en": "Apa Itu Kerusakan Dielektrik?", "id": "Kegagalan Material Isolator Menjadi Konduktif." },
  { "en": "Apa Itu Susceptibilitas Listrik?", "id": "Ukuran Seberapa Mudah Material Dipolarisasi." },
  { "en": "Apa Itu Magnetisme?", "id": "Fenomena Fisik Dihasilkan Oleh Muatan Bergerak." },
  { "en": "Apa Itu Diamagnetisme?", "id": "Material Lemah Ditolak Medan Magnet." },
  { "en": "Apa Itu Paramagnetisme?", "id": "Material Lemah Ditarik Medan Magnet." },
  { "en": "Apa Itu Feromagnetisme?", "id": "Mekanisme Material Membentuk Magnet Permanen." },
  { "en": "Apa Itu Domain Magnetik?", "id": "Wilayah Di Mana Momen Magnetik Sejajar." },
  { "en": "Apa Itu Antiferomagnetisme?", "id": "Momen Magnetik Tetangga Berjajar Berlawanan." },
  { "en": "Apa Itu Ferrimagnetisme?", "id": "Momen Magnetik Tetangga Berlawanan, Tak Sama." },
  { "en": "Apa Itu Titik Curie?", "id": "Suhu Di Atasnya Material Feromagnetik." },
  { "en": "Apa Itu Susceptibilitas Magnetik?", "id": "Ukuran Seberapa Magnetis Material." },
  { "en": "Apa Itu Magnetoresistansi?", "id": "Perubahan Resistansi Akibat Medan Magnet." },
  { "en": "Apa Itu Giant Magnetoresistance (GMR)?", "id": "Efek Magnetoresistansi Kuantum Yang Besar." },
  { "en": "Apa Itu Spintronik?", "id": "Elektronik Yang Memanfaatkan Spin Elektron." },
  { "en": "Apa Itu Teori Relativitas?", "id": "Teori Fisika Dikembangkan Oleh Einstein." },
  { "en": "Apa Itu Kerangka Acuan Inersia?", "id": "Kerangka Acuan Tidak Mengalami Percepatan." },
  { "en": "Apa Itu Transformasi Lorentz?", "id": "Transformasi Koordinat Antara Kerangka Acuan." },
  { "en": "Apa Itu Paradoks Kembar?", "id": "Eksperimen Pikiran Dalam Relativitas Khusus." },
  { "en": "Apa Itu Ruang-Waktu Minkowski?", "id": "Model Geometris Ruang-Waktu." },
  { "en": "Apa Itu Tensor Metrik?", "id": "Menentukan Geometri Ruang-Waktu." },
  { "en": "Apa Itu Geodesik?", "id": "Jalur Terpendek Antara Dua Titik." },
  { "en": "Apa Itu Persamaan Medan Einstein?", "id": "Menghubungkan Geometri Ruang-Waktu Dengan Materi." },
  { "en": "Apa Itu Tensor Energi-Stres?", "id": "Mendeskripsikan Kepadatan, Aliran Energi, Momentum." },
  { "en": "Apa Itu Solusi Schwarzschild?", "id": "Solusi Persamaan Medan Untuk Benda Bulat." },
  { "en": "Apa Itu Radius Schwarzschild?", "id": "Radius Horison Peristiwa Lubang Hitam." },
  { "en": "Apa Itu Lubang Cacing (Wormhole)?", "id": "Jalan Pintas Hipotetis Melalui Ruang-Waktu." },
  { "en": "Apa Itu Konstanta Kosmologis?", "id": "Istilah Ditambahkan Einstein Ke Persamaan Medan." },
  { "en": "Apa Itu Prinsip Kosmologis?", "id": "Alam Semesta Homogen, Isotropik." },
  { "en": "Apa Itu Metrik FLRW?", "id": "Solusi Persamaan Medan Mendeskripsikan Alam Semesta." },
  { "en": "Apa Itu Big Crunch?", "id": "Skenario Hipotetis Keruntuhan Alam Semesta." },
  { "en": "Apa Itu Big Rip?", "id": "Skenario Hipotetis Alam Semesta Tercabik." },
  { "en": "Apa Itu Nukleosintesis Big Bang?", "id": "Pembentukan Inti Atom Ringan." },
  { "en": "Apa Itu Masalah Horison?", "id": "Teka-teki Dalam Model Big Bang Standar." },
  { "en": "Apa Itu Masalah Kerataan?", "id": "Teka-teki Lainnya Dalam Kosmologi." },
  { "en": "Apa Itu Teori Bidang Kuantum (QFT)?", "id": "Kerangka Teoretis Menggabungkan Medan Klasik." },
  { "en": "Apa Itu Medan Kuantum?", "id": "Medan Fisik Yang Kuantitasnya Dioperasikan." },
  { "en": "Apa Itu Partikel Virtual?", "id": "Fluktuasi Kuantum Sementara Dalam Medan." },
  { "en": "Apa Itu Diagram Feynman?", "id": "Representasi Grafis Interaksi Partikel." },
  { "en": "Apa Itu Renormalisasi?", "id": "Teknik Mengatasi Ketidakterbatasan Dalam QFT." },
  { "en": "Apa Itu Pelanggaran Simetri?", "id": "Hukum Fisika Simetris, Keadaan Tidak." },
  { "en": "Apa Itu Mekanisme Higgs?", "id": "Menjelaskan Bagaimana Partikel Mendapatkan Massa." },
  { "en": "Apa Itu Teori Gauge?", "id": "Jenis Teori Bidang Berdasarkan Simetri." },
  { "en": "Apa Itu Konfinemen Warna?", "id": "Kuark Tidak Pernah Diamati Secara Terisolasi." },
  { "en": "Apa Itu Kebebasan Asimtotik?", "id": "Interaksi Kuat Melemah Pada Energi Tinggi." },
  { "en": "Apa Itu Teori Unifikasi Besar (GUT)?", "id": "Model Menyatukan Gaya Kuat, Lemah, Elektromagnetik." },
  { "en": "Apa Itu Peluruhan Proton?", "id": "Proses Hipotetis Diprediksi Beberapa Teori GUT." },
  { "en": "Apa Itu Gravitasi Kuantum?", "id": "Bidang Fisika Mencari Teori Kuantum." },
  { "en": "Apa Itu Gravitasi Kuantum Loop?", "id": "Pendekatan Teori Gravitasi Kuantum." },
  { "en": "Apa Itu Busa Ruang-Waktu?", "id": "Fluktuasi Kuantum Geometri Ruang-Waktu." },
  { "en": "Apa Itu Prinsip Holografik?", "id": "Deskripsi Ruang Dapat Dianggap Terkode." },
  { "en": "Apa Itu Korespondensi AdS/CFT?", "id": "Hubungan Antara Teori Gravitasi, Teori Bidang." },
  { "en": "Apa Itu Dualitas Gelombang-Partikel?", "id": "Konsep Partikel Menunjukkan Sifat Gelombang." },
  { "en": "Apa Itu Paket Gelombang?", "id": "Lokalisasi Gelombang Yang Berfungsi Seperti Partikel." },
  { "en": "Apa Itu Hubungan De Broglie?", "id": "Menghubungkan Panjang Gelombang Dengan Momentum Partikel." },
  { "en": "Apa Itu Eksperimen Davisson-Germer?", "id": "Mengkonfirmasi Sifat Gelombang Elektron." },
  { "en": "Apa Itu Model Atom Bohr?", "id": "Model Awal Atom Hidrogen." },
  { "en": "Apa Itu Bilangan Kuantum?", "id": "Angka Mendeskripsikan Keadaan Kuantum Elektron." },
  { "en": "Apa Itu Prinsip Pengecualian Pauli?", "id": "Dua Fermion Identik Tidak Dapat Menempati." },
  { "en": "Apa Itu Orbital Atom?", "id": "Fungsi Matematis Mendeskripsikan Perilaku Elektron." },
  { "en": "Apa Itu Ikatan Kimia?", "id": "Gaya Tarik Tahan Atom Bersama." },
  { "en": "Apa Itu Struktur Lewis?", "id": "Diagram Menunjukkan Ikatan Antar Atom." },
  { "en": "Apa Itu Elektronegativitas?", "id": "Kecenderungan Atom Menarik Elektron." },
  { "en": "Apa Itu Momen Dipol Listrik?", "id": "Ukuran Pemisahan Muatan Listrik." },
  { "en": "Apa Itu Gaya Van der Waals?", "id": "Gaya Tarik Atau Tolak Lemah." },
  { "en": "Apa Itu Ikatan Hidrogen?", "id": "Jenis Interaksi Dipol-Dipol Khusus." },
  { "en": "Apa Itu Kimia Organik?", "id": "Studi Senyawa Berbasis Karbon." },
  { "en": "Apa Itu Hidrokarbon?", "id": "Senyawa Terdiri Dari Hidrogen, Karbon." },
  { "en": "Apa Itu Isomer?", "id": "Molekul Rumus Sama, Susunan Berbeda." },
  { "en": "Apa Itu Polimer?", "id": "Molekul Besar Terdiri Dari Unit Berulang." },
  { "en": "Apa Itu Teori Kendali (Control Theory)?", "id": "Ilmu Menganalisis Dan Mendesain Sistem Dinamis." },
  { "en": "Apa Itu Sistem Waktu Kontinu?", "id": "Sistem Variabelnya Didefinisikan Setiap Saat." },
  { "en": "Apa Itu Sistem Waktu Diskret?", "id": "Sistem Variabelnya Didefinisikan Pada Waktu Tertentu." },
  { "en": "Apa Itu Titik Setimbang (Equilibrium Point)?", "id": "Keadaan Di Mana Sistem Tetap Diam." },
  { "en": "Apa Itu Stabilitas Lyapunov?", "id": "Konsep Stabilitas Untuk Sistem Dinamis." },
  { "en": "Apa Itu Fungsi Lyapunov?", "id": "Fungsi Skalar Untuk Membuktikan Stabilitas." },
  { "en": "Apa Itu Kriteria Stabilitas Routh-Hurwitz?", "id": "Metode Aljabar Menentukan Stabilitas Sistem." },
  { "en": "Apa Itu Plot Nyquist?", "id": "Plot Respon Frekuensi Dalam Bidang Kompleks." },
  { "en": "Apa Itu Kriteria Stabilitas Nyquist?", "id": "Menentukan Stabilitas Loop Tertutup Dari Plot." },
  { "en": "Apa Itu Tipe Sistem Kontrol?", "id": "Jumlah Integrator Dalam Loop Terbuka." },
  { "en": "Apa Pengaruh Tipe Sistem Terhadap Error?", "id": "Menentukan Kemampuan Sistem Melacak Input." },
  { "en": "Apa Itu Kompensator Lead?", "id": "Meningkatkan Margin Fasa Dan Kecepatan Respon." },
  { "en": "Apa Itu Kompensator Lag?", "id": "Mengurangi Error Keadaan Tunak (Steady-State)." },
  { "en": "Apa Itu Kompensator Lead-Lag?", "id": "Menggabungkan Keuntungan Dari Kompensator Lead, Lag." },
  { "en": "Apa Itu Kontroler Proporsional-Integral (PI)?", "id": "Menggabungkan Aksi Proporsional Dan Integral." },
  { "en": "Apa Itu Kontroler Proporsional-Derivatif (PD)?", "id": "Menggabungkan Aksi Proporsional Dan Derivatif." },
  { "en": "Apa Itu Anti-Windup?", "id": "Mencegah Akumulasi Berlebih Istilah Integral." },
  { "en": "Apa Itu Kontroler Dahmen?", "id": "Metode Desain Kontroler Berbasis Model Proses." },
  { "en": "Apa Itu Waktu Mati (Dead Time)?", "id": "Penundaan Waktu Murni Dalam Suatu Proses." },
  { "en": "Bagaimana Cara Menangani Waktu Mati?", "id": "Menggunakan Prediktor Smith Atau Kontroler Khusus." },
  { "en": "Apa Itu Prediktor Smith?", "id": "Struktur Kontrol Untuk Sistem Dengan Waktu Mati." },
  { "en": "Apa Itu Kendali Optimal?", "id": "Menemukan Sinyal Kontrol Meminimalkan Fungsi Biaya." },
  { "en": "Apa Itu Persamaan Riccati?", "id": "Persamaan Matriks Dalam Teori Kendali Optimal." },
  { "en": "Apa Itu Kendali Kuat (Robust Control)?", "id": "Desain Kontroler Tahan Terhadap Ketidakpastian Model." },
  { "en": "Apa Itu Ketidakpastian Terstruktur?", "id": "Ketidakpastian Dengan Struktur Matematis Yang Dikenal." },
  { "en": "Apa Itu Teorema Small Gain?", "id": "Syarat Stabilitas Untuk Sistem Terinterkoneksi." },
  { "en": "Apa Itu Kontrol Adaptif?", "id": "Kontroler Menyesuaikan Parameternya Secara Online." },
  { "en": "Apa Itu Model Reference Adaptive Control (MRAC)?", "id": "Sistem Adaptif Mengikuti Model Referensi." },
  { "en": "Apa Itu Self-Tuning Regulator (STR)?", "id": "Sistem Adaptif Mengidentifikasi, Mengontrol Proses." },
  { "en": "Apa Itu Jaringan Listrik Cerdas?", "id": "Jaringan Listrik Dengan Komunikasi, Kontrol Cerdas." },
  { "en": "Apa Itu Demand Side Management (DSM)?", "id": "Memodifikasi Permintaan Konsumen Untuk Mencocokkan Pasokan." },
  { "en": "Apa Itu Penyimpanan Energi Skala Grid?", "id": "Menyimpan Energi Listrik Dalam Jumlah Besar." },
  { "en": "Apa Itu Pembangkit Listrik Virtual (VPP)?", "id": "Agregasi Sumber Daya Energi Terdistribusi (DER)." },
  { "en": "Apa Itu Respon Frekuensi (Sistem Tenaga)?", "id": "Kemampuan Jaringan Menstabilkan Frekuensi." },
  { "en": "Apa Itu Inersia Sintetis?", "id": "Inverter Meniru Respon Inersia Generator Sinkron." },
  { "en": "Apa Itu Analisis Data Fasor (Synchrophasor)?", "id": "Menggunakan Data PMU Untuk Pemantauan Jaringan." },
  { "en": "Apa Itu State Estimation Dalam Sistem Tenaga?", "id": "Mengestimasi Keadaan Jaringan Berdasarkan Pengukuran." },
  { "en": "Apa Itu Weighted Least Squares (WLS)?", "id": "Metode Umum Digunakan Dalam State Estimation." },
  { "en": "Apa Itu Deteksi Data Buruk?", "id": "Mengidentifikasi Pengukuran Yang Salah Dalam Data." },
  { "en": "Apa Itu Keteramatan Jaringan?", "id": "Kemampuan Mengestimasi Keadaan Jaringan Penuh." },
  { "en": "Apa Itu Pulau Jaringan (Islanding)?", "id": "Bagian Jaringan Terisolasi Dari Jaringan Utama." },
  { "en": "Apa Itu Proteksi Anti-Islanding?", "id": "Mendeteksi, Memutuskan Inverter Saat Terjadi Islanding." },
  { "en": "Apa Itu Ride-Through?", "id": "Kemampuan Generator Tetap Terhubung Saat Gangguan." },
  { "en": "Apa Itu Low Voltage Ride Through (LVRT)?", "id": "Kemampuan Melewati Gangguan Tegangan Rendah." },
  { "en": "Apa Itu High Voltage Ride Through (HVRT)?", "id": "Kemampuan Melewati Gangguan Tegangan Tinggi." },
  { "en": "Apa Itu Frequency Ride Through (FRT)?", "id": "Kemampuan Melewati Gangguan Frekuensi." },
  { "en": "Apa Itu Pasar Energi?", "id": "Pasar Komoditas Untuk Perdagangan Listrik." },
  { "en": "Apa Itu Pasar Kapasitas?", "id": "Membayar Pembangkit Untuk Siap Beroperasi." },
  { "en": "Apa Itu Pasar Jasa Tambahan?", "id": "Pasar Untuk Layanan Pendukung Keandalan." },
  { "en": "Apa Itu Transactive Energy?", "id": "Penggunaan Sinyal Ekonomi Mengkoordinasikan Jaringan." },
  { "en": "Apa Itu Kontrol Volt/VAR?", "id": "Mengatur Tegangan, Aliran Daya Reaktif." },
  { "en": "Apa Itu Optimisasi Volt/VAR (VVO)?", "id": "Mengoptimalkan Profil Tegangan, Mengurangi Kerugian." },
  { "en": "Apa Itu Conservation Voltage Reduction (CVR)?", "id": "Menurunkan Tegangan Sedikit Untuk Menghemat Energi." },
  { "en": "Apa Itu Model ZIP Beban?", "id": "Model Beban Listrik (Impedansi, Arus, Daya)." },
  { "en": "Apa Itu Peramalan Beban Jangka Pendek?", "id": "Memprediksi Beban Listrik Jam Hingga Hari." },
  { "en": "Apa Itu Peramalan Beban Jangka Panjang?", "id": "Memprediksi Beban Listrik Bulan Hingga Tahun." },
  { "en": "Apa Itu Analisis Deret Waktu?", "id": "Metode Statistik Untuk Data Berurutan Waktu." },
  { "en": "Apa Itu Model ARIMA?", "id": "AutoRegressive Integrated Moving Average." },
  { "en": "Apa Itu Unit Commitment?", "id": "Menentukan Jadwal Pembangkit Mana Yang Beroperasi." },
  { "en": "Apa Itu Optimisasi Hidrotermal?", "id": "Penjadwalan Optimal Pembangkit Air, Termal." },
  { "en": "Apa Itu Keamanan Siber Sistem Tenaga?", "id": "Melindungi Jaringan Listrik Dari Serangan Siber." },
  { "en": "Apa Itu Standar NERC CIP?", "id": "Standar Keamanan Siber Untuk Jaringan Listrik." },
  { "en": "Apa Itu Deteksi Intrusi?", "id": "Mendeteksi Akses Tidak Sah Ke Jaringan." },
  { "en": "Apa Itu Serangan False Data Injection (FDI)?", "id": "Menyuntikkan Data Palsu Mengelabui Sistem." },
  { "en": "Apa Itu Geographically Dispersed Generation?", "id": "Pembangkitan Tersebar Di Lokasi Geografis Berbeda." },
  { "en": "Apa Itu Komputasi Kinerja Tinggi (HPC)?", "id": "Komputasi Super Untuk Masalah Kompleks." },
  { "en": "Aplikasi Apa HPC (High-Performance Computing) Dalam Sistem Tenaga?", "id": "Simulasi Dinamis Skala Besar, Analisis." },
  { "en": "Apa Itu Co-Simulasi?", "id": "Mensimulasikan Sistem Berbeda Secara Bersamaan." },
  { "en": "Apa Itu Model-Based Design (MBD)?", "id": "Menggunakan Model Sepanjang Siklus Pengembangan." },
  { "en": "Apa Itu Hardware-in-the-Loop (HIL) Simulation?", "id": "Menguji Kontroler Fisik Dengan Proses Tersimulasi." },
  { "en": "Mengapa HIL (Hardware-in-the-Loop) Penting?", "id": "Memungkinkan Pengujian Aman, Realistis, Berulang." },
  { "en": "Apa Itu Real-Time Digital Simulator (RTDS)?", "id": "Simulator Perangkat Keras Untuk Sistem Tenaga." },
  { "en": "Apa Itu Analisis Risiko Probabilistik (PRA)?", "id": "Mengevaluasi Risiko Secara Kuantitatif." },
  { "en": "Apa Itu Indeks Keandalan Sistem Tenaga?", "id": "SAIDI, SAIFI, CAIDI, Dan Lainnya." },
  { "en": "Apa Kepanjangan SAIDI?", "id": "System Average Interruption Duration Index." },
  { "en": "Apa Kepanjangan SAIFI?", "id": "System Average Interruption Frequency Index." },
  { "en": "Apa Itu Penilaian Aset (Asset Management)?", "id": "Manajemen Siklus Hidup Aset Fisik." },
  { "en": "Apa Itu Pemeliharaan Berbasis Kondisi (CBM)?", "id": "Pemeliharaan Berdasarkan Kondisi Aktual Aset." },
  { "en": "Apa Itu Pemantauan Kondisi Online?", "id": "Memantau Kesehatan Aset Secara Terus-menerus." },
  { "en": "Apa Itu Analisis Gas Terlarut (DGA)?", "id": "Mendiagnosis Kesehatan Transformator Dari Gas." },
  { "en": "Apa Itu Emisi Akustik?", "id": "Gelombang Stres Dihasilkan Oleh Cacat Material." },
  { "en": "Apa Itu Termografi?", "id": "Menggunakan Pencitraan Inframerah Mendeteksi Panas." },
  { "en": "Apa Itu Pemindaian Ultraviolet (UV)?", "id": "Mendeteksi Pelepasan Korona Pada Peralatan." },
  { "en": "Apa Itu Resistansi Isolasi?", "id": "Ukuran Kualitas Material Isolasi." },
  { "en": "Apa Itu Uji Megger?", "id": "Pengujian Untuk Mengukur Resistansi Isolasi." },
  { "en": "Apa Itu Indeks Polarisasi (PI)?", "id": "Rasio Pengukuran Resistansi Isolasi." },
  { "en": "Apa Itu Rasio Penyerapan Dielektrik (DAR)?", "id": "Rasio Lainnya Pengukuran Resistansi Isolasi." },
  { "en": "Apa Itu Kapasitansi Dan Faktor Daya?", "id": "Uji Untuk Mengevaluasi Kondisi Isolasi." },
  { "en": "Apa Itu Penuaan Aset?", "id": "Degradasi Aset Fisik Seiring Waktu." },
  { "en": "Apa Itu Model Umur Aset?", "id": "Memprediksi Sisa Umur Pakai Aset." },
  { "en": "Apa Itu Keamanan Proses?", "id": "Pencegahan Pelepasan Energi Berbahaya." },
  { "en": "Apa Itu Analisis Lapisan Proteksi (LOPA)?", "id": "Metode Analisis Risiko Semi-Kuantitatif." },
  { "en": "Apa Itu Safety Instrumented System (SIS)?", "id": "Sistem Otomatis Terpisah Untuk Keamanan." },
  { "en": "Apa Itu Safety Instrumented Function (SIF)?", "id": "Fungsi Keamanan Dilakukan Oleh SIS." },
  { "en": "Apa Itu Permintaan (Demand) Dalam Konteks SIF?", "id": "Kejadian Berbahaya Yang Membutuhkan Aksi SIF." },
  { "en": "Apa Itu Probability of Failure on Demand (PFD)?", "id": "Probabilitas SIF Gagal Berfungsi Saat Diminta." },
  { "en": "Apa Itu Mode Operasi Permintaan Rendah?", "id": "Permintaan Terjadi Kurang Dari Sekali Setahun." },
  { "en": "Apa Itu Mode Operasi Permintaan Tinggi?", "id": "Permintaan Terjadi Lebih Dari Sekali Setahun." },
  { "en": "Apa Itu Voting 1oo2 (Satu dari Dua)?", "id": "Sistem Trip Jika Salah Satu Sensor Aktif." },
  { "en": "Apa Itu Voting 2oo3 (Dua dari Tiga)?", "id": "Sistem Trip Jika Dua Sensor Aktif." },
  { "en": "Apa Itu Uji Pembuktian (Proof Test)?", "id": "Pengujian Periodik Mendeteksi Kegagalan Tersembunyi." },
  { "en": "Apa Itu Kegagalan Mode Umum?", "id": "Satu Penyebab Menyebabkan Beberapa Komponen Gagal." },
  { "en": "Apa Itu Konverter SEPIC (Single-Ended Primary-Inductor Converter)?", "id": "Dapat Menaikkan Atau Menurunkan Tegangan DC." },
  { "en": "Apa Itu Konverter Zeta?", "id": "Topologi Konverter Buck-Boost Non-inverting." },
  { "en": "Apa Itu Kontrol Mode Tegangan?", "id": "Umpan Balik Menggunakan Tegangan Output Langsung." },
  { "en": "Apa Itu Kontrol Mode Arus?", "id": "Umpan Balik Menggunakan Arus Induktor." },
  { "en": "Apa Itu Kompensasi Kemiringan?", "id": "Mencegah Osilasi Subharmonik Kontrol Mode Arus." },
  { "en": "Apa Itu Gerbang Logika Dinamis?", "id": "Menggunakan Kapasitor Untuk Menyimpan Keadaan Logika." },
  { "en": "Apa Kerugian Gerbang Logika Dinamis?", "id": "Membutuhkan Sinyal Clock Penyegar (Refresh)." },
  { "en": "Apa Itu Logika Domino?", "id": "Jenis Gerbang Logika Dinamis Yang Bertingkat." },
  { "en": "Apa Itu Skew Jam (Clock Skew)?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
  { "en": "Apa Itu Jitter Jam (Clock Jitter)?", "id": "Variasi Periode Sinyal Clock Dari Ideal." },
  { "en": "Apa Itu Jaringan Distribusi Jam?", "id": "Sirkuit Mendistribusikan Sinyal Clock Merata." },
  { "en": "Apa Itu Gated Clocking?", "id": "Teknik Menghemat Daya Menonaktifkan Clock." },
  { "en": "Apa Itu Waktu Setup (Setup Time)?", "id": "Waktu Data Stabil Sebelum Tepi Clock." },
  { "en": "Apa Itu Waktu Tahan (Hold Time)?", "id": "Waktu Data Stabil Setelah Tepi Clock." },
  { "en": "Apa Akibat Pelanggaran Waktu Setup/Tahan?", "id": "Metastabilitas." },
  { "en": "Apa Itu Metastabilitas?", "id": "Keadaan Tidak Stabil Antara Logika 0, 1." },
  { "en": "Apa Itu Sinkronizer Dua Flip-Flop?", "id": "Mengurangi Probabilitas Kegagalan Akibat Metastabilitas." },
  { "en": "Apa Itu Mean Time Between Failures (MTBF)?", "id": "Waktu Rata-Rata Antara Dua Kegagalan." },
  { "en": "Apa Itu Penempatan Kutub (Pole Placement)?", "id": "Teknik Desain Kontroler Umpan Balik Keadaan." },
  { "en": "Apa Itu Pengamat Keadaan (State Observer)?", "id": "Mengestimasi Keadaan Internal Sistem Tidak Terukur." },
  { "en": "Apa Itu Prinsip Pemisahan Dalam Kontrol?", "id": "Desain Kontroler, Pengamat Dapat Dilakukan Terpisah." },
  { "en": "Apa Itu Polarisasi Gelombang Eliptikal?", "id": "Ujung Vektor Medan Listrik Membentuk Elips." },
  { "en": "Apa Itu Rasio Aksial (Axial Ratio)?", "id": "Ukuran Kemurnian Polarisasi Sirkular." },
  { "en": "Apa Itu Antena Array Celah (Slot Array)?", "id": "Array Antena Dibuat Dari Celah Konduktor." },
  { "en": "Apa Itu Balun?", "id": "Menghubungkan Saluran Seimbang Ke Tidak Seimbang." },
  { "en": "Apa Itu Penguat Diferensial Penuh?", "id": "Amplifier Dengan Input Dan Output Diferensial." },
  { "en": "Apa Itu Umpan Balik Common-Mode (CMFB)?", "id": "Menstabilkan Level DC Output Amplifier Diferensial." },
  { "en": "Apa Itu Penguat Transimpedansi (TIA)?", "id": "Mengubah Sinyal Arus Menjadi Sinyal Tegangan." },
  { "en": "Di Mana Amplifier Transimpedansi Sering Digunakan?", "id": "Penerima Optik Dengan Fotodioda." },
  { "en": "Apa Itu Rangkaian Kapasitor Bertukar?", "id": "Mensimulasikan Resistor Menggunakan Kapasitor, Sakelar." },
  { "en": "Apa Itu Analisis Monte Carlo?", "id": "Analisis Statistik Menggunakan Angka Acak." },
  { "en": "Apa Itu Analisis Kasus Terburuk (Worst-Case)?", "id": "Menganalisis Performa Sirkuit Pada Kondisi Ekstrem." },
  { "en": "Apa Itu Keandalan Perangkat Lunak?", "id": "Probabilitas Operasi Bebas Kegagalan Perangkat Lunak." },
  { "en": "Apa Itu Model Pertumbuhan Keandalan?", "id": "Memodelkan Peningkatan Keandalan Seiring Waktu." },
  { "en": "Apa Itu Proses Manufaktur CMOS?", "id": "Urutan Langkah Membuat Chip CMOS." },
  { "en": "Apa Itu Proses N-Well?", "id": "Proses CMOS (Complementary Metal-Oxide-Semiconductor) Di Mana PMOS Dibuat Sumur-N." },
  { "en": "Apa Itu Proses P-Well?", "id": "Proses CMOS (Complementary Metal-Oxide-Semiconductor) Di Mana NMOS Dibuat Sumur-P." },
  { "en": "Apa Itu Proses Twin-Tub?", "id": "Proses CMOS (Complementary Metal-Oxide-Semiconductor) Menggunakan Sumur-N Dan Sumur-P." },
  { "en": "Apa Itu Latchup?", "id": "Hubung Singkat Parasitik Dalam Struktur CMOS." },
  { "en": "Bagaimana Mencegah Latchup?", "id": "Menggunakan Cincin Pelindung (Guard Rings)." },
  { "en": "Apa Itu Aturan Desain (Design Rules)?", "id": "Aturan Geometris Untuk Desain Layout IC." },
  { "en": "Apa Itu Design Rule Check (DRC)?", "id": "Verifikasi Otomatis Terhadap Aturan Desain." },
  { "en": "Apa Itu Layout Versus Schematic (LVS)?", "id": "Verifikasi Layout Sesuai Dengan Skematik." },
  { "en": "Apa Itu Ekstraksi Parasitik?", "id": "Menghitung Komponen Parasitik Dari Layout." },
  { "en": "Apa Itu Verifikasi Fisik?", "id": "Proses Pemeriksaan DRC, LVS, Ekstraksi." },
  { "en": "Apa Itu Material Dielektrik Low-k?", "id": "Material Mengurangi Kapasitansi Antar Kawat." },
  { "en": "Apa Tujuan Material Dielektrik Low-k?", "id": "Mengurangi Penundaan, Crosstalk Dalam Interkoneksi." },
  { "en": "Apa Itu Interkoneksi Tembaga (Copper Interconnect)?", "id": "Menggantikan Aluminium Untuk Resistansi Lebih Rendah." },
  { "en": "Apa Itu Proses Damascene?", "id": "Proses Fabrikasi Untuk Interkoneksi Tembaga." },
  { "en": "Apa Itu Chemical-Mechanical Planarization (CMP)?", "id": "Proses Perataan Permukaan Wafer." },
  { "en": "Apa Itu Silikon Polikristalin (Polysilicon)?", "id": "Digunakan Sebagai Material Gerbang (Gate) MOSFET." },
  { "en": "Apa Itu Salicide (Self-Aligned Silicide)?", "id": "Mengurangi Resistansi Gerbang, Source, Drain." },
  { "en": "Apa Itu Shallow Trench Isolation (STI)?", "id": "Teknik Isolasi Antar Transistor Modern." },
  { "en": "Apa Itu FinFET?", "id": "Transistor Multi-Gerbang Non-Planar." },
  { "en": "Apa Keuntungan FinFET?", "id": "Kontrol Gerbang Lebih Baik, Arus Bocor Rendah." },
  { "en": "Apa Itu Teknologi Silicon-On-Insulator (SOI)?", "id": "Lapisan Oksida Terkubur Mengisolasi Substrat." },
  { "en": "Apa Keuntungan SOI (Silicon-On-Insulator)?", "id": "Mengurangi Kapasitansi Parasitik, Tahan Radiasi." },
  { "en": "Apa Itu Sistem-dalam-Paket (SiP)?", "id": "Beberapa Die Berbeda Dalam Satu Paket." },
  { "en": "Apa Itu Sistem-pada-Chip (SoC)?", "id": "Semua Komponen Sistem Dalam Satu Die." },
  { "en": "Apa Itu Through-Silicon Via (TSV)?", "id": "Koneksi Vertikal Tembus Substrat Silikon." },
  { "en": "Apa Itu Stacking Die 3D?", "id": "Menumpuk Beberapa Die Secara Vertikal." },
  { "en": "Apa Itu Memori Volatile?", "id": "Kehilangan Data Saat Daya Dimatikan." },
  { "en": "Apa Itu Memori Non-Volatile?", "id": "Menyimpan Data Tanpa Catu Daya." },
  { "en": "Apa Itu Sel Memori SRAM 6T?", "id": "Enam Transistor Membentuk Sel Memori Statis." },
  { "en": "Apa Itu Sel Memori DRAM 1T1C?", "id": "Satu Transistor, Satu Kapasitor Sel Dinamis." },
  { "en": "Apa Itu Sense Amplifier?", "id": "Sirkuit Membaca Perubahan Tegangan Kecil DRAM." },
  { "en": "Apa Itu Refresh DRAM?", "id": "Proses Membaca, Menulis Ulang Data Periodik." },
  { "en": "Apa Itu Arsitektur NOR Flash?", "id": "Memungkinkan Akses Acak Cepat." },
  { "en": "Apa Itu Arsitektur NAND Flash?", "id": "Kepadatan Tinggi, Akses Berbasis Halaman." },
  { "en": "Apa Itu Floating Gate Transistor?", "id": "Elemen Penyimpanan Muatan Memori Flash." },
  { "en": "Apa Itu Wear Leveling?", "id": "Mendistribusikan Siklus Hapus/Tulis Merata." },
  { "en": "Apa Itu MRAM (Magnetoresistive RAM)?", "id": "Memori Non-Volatile Menggunakan Efek Magnetik." },
  { "en": "Apa Itu ReRAM (Resistive RAM)?", "id": "Memori Non-Volatile Berbasis Perubahan Resistansi." },
  { "en": "Apa Itu FeRAM (Ferroelectric RAM)?", "id": "Memori Non-Volatile Berbasis Material Feroelektrik." },
  { "en": "Apa Itu Penguat Indra (Sense Amplifier)?", "id": "Amplifier Mendeteksi Sinyal Kecil Dari Bitcell." },
  { "en": "Apa Itu Arsitektur Memori Hierarkis?", "id": "Menggunakan Beberapa Level Memori Berbeda." },
  { "en": "Apa Itu Cache L1, L2, L3?", "id": "Level Berbeda Memori Cache Prosesor." },
  { "en": "Apa Itu Koherensi Cache?", "id": "Menjaga Konsistensi Data Di Beberapa Cache." },
  { "en": "Apa Itu Protokol MESI?", "id": "Protokol Koherensi Cache (Modified, Exclusive, Shared, Invalid)." },
  { "en": "Apa Itu Bus Snooping?", "id": "Teknik Memantau Lalu Lintas Bus Koherensi." },
  { "en": "Apa Itu Sistem Terdistribusi?", "id": "Komponen Berbeda Berkomunikasi Melalui Jaringan." },
  { "en": "Apa Itu Fault Tolerance?", "id": "Kemampuan Sistem Beroperasi Meski Ada Kegagalan." },
  { "en": "Apa Itu Redundansi?", "id": "Duplikasi Komponen Kritis Untuk Keandalan." },
  { "en": "Apa Itu Komputasi Grid?", "id": "Menggunakan Sumber Daya Terdistribusi Geografis." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Menyediakan Layanan Komputasi Melalui Internet." },
  { "en": "Apa Itu Virtualisasi?", "id": "Membuat Representasi Virtual Sumber Daya." },
  { "en": "Apa Itu Hypervisor?", "id": "Perangkat Lunak Yang Membuat, Menjalankan VM." },
  { "en": "Apa Itu Mesin Virtual (VM)?", "id": "Emulasi Sistem Komputer Lengkap." },
  { "en": "Apa Itu Kontainer?", "id": "Bentuk Virtualisasi Ringan Tingkat OS." },
  { "en": "Apa Itu Docker?", "id": "Platform Populer Untuk Aplikasi Berkontainer." },
  { "en": "Apa Itu Kubernetes?", "id": "Platform Orkestrasi Kontainer." },
  { "en": "Apa Itu Orkestrasi?", "id": "Manajemen Otomatis Sistem Atau Layanan Kompleks." },
  { "en": "Apa Itu Arsitektur Microservices?", "id": "Aplikasi Dibangun Sebagai Kumpulan Layanan Kecil." },
  { "en": "Apa Itu API Gateway?", "id": "Titik Masuk Tunggal Untuk Semua Permintaan." },
  { "en": "Apa Itu Service Discovery?", "id": "Cara Layanan Saling Menemukan Secara Dinamis." },
  { "en": "Apa Itu Load Balancing?", "id": "Mendistribusikan Lalu Lintas Ke Beberapa Server." },
  { "en": "Apa Itu Skalabilitas Horizontal?", "id": "Menambahkan Lebih Banyak Mesin Ke Sistem." },
  { "en": "Apa Itu Skalabilitas Vertikal?", "id": "Meningkatkan Sumber Daya Satu Mesin." },
  { "en": "Apa Itu Teorema CAP?", "id": "Konsistensi, Ketersediaan, Toleransi Partisi." },
  { "en": "Apa Itu Konsistensi Akhirnya?", "id": "Data Akan Menjadi Konsisten Seiring Waktu." },
  { "en": "Apa Itu Basis Data Terdistribusi?", "id": "Database Disimpan Di Beberapa Lokasi Fisik." },
  { "en": "Apa Itu Sharding?", "id": "Membagi Database Secara Horizontal Berdasarkan Kunci." },
  { "en": "Apa Itu Replikasi?", "id": "Menyalin Data Untuk Redundansi Atau Ketersediaan." },
  { "en": "Apa Itu Replikasi Master-Slave?", "id": "Satu Master Menerima Tulisan, Banyak Slave Membaca." },
  { "en": "Apa Itu Replikasi Master-Master?", "id": "Beberapa Node Dapat Menerima Operasi Tulis." },
  { "en": "Apa Itu Jaringan Overlay?", "id": "Jaringan Virtual Dibangun Di Atas Jaringan Lain." },
  { "en": "Apa Itu Peer-to-Peer (P2P) Network?", "id": "Jaringan Terdesentralisasi Tanpa Server Pusat." },
  { "en": "Apa Itu Distributed Hash Table (DHT)?", "id": "Sistem Terdistribusi Menyediakan Layanan Pencarian." },
  { "en": "Apa Itu Konsensus Dalam Sistem Terdistribusi?", "id": "Mencapai Kesepakatan Antara Beberapa Proses." },
  { "en": "Apa Itu Algoritma Paxos?", "id": "Keluarga Protokol Untuk Mencapai Konsensus." },
  { "en": "Apa Itu Algoritma Raft?", "id": "Algoritma Konsensus Yang Dirancang Mudah Dipahami." },
  { "en": "Apa Itu Toleransi Kesalahan Bizantium (Byzantine Fault Tolerance)?", "id": "Kemampuan Sistem Menangani Kegagalan Sewenang-wenang." },
  { "en": "Apa Itu Masalah Jenderal Bizantium?", "id": "Masalah Komunikasi Dalam Mencapai Konsensus." },
  { "en": "Apa Itu Remote Procedure Call (RPC)?", "id": "Memanggil Prosedur Di Komputer Lain." },
  { "en": "Apa Itu Serialisasi Data?", "id": "Mengubah Struktur Data Menjadi Format Penyimpanan." },
  { "en": "Apa Itu Deserialisasi Data?", "id": "Membangun Kembali Struktur Data Dari Format." },
  { "en": "Apa Itu Protocol Buffers?", "id": "Mekanisme Serialisasi Data Biner Google." },
  { "en": "Apa Itu Apache Thrift?", "id": "Kerangka Kerja RPC (Remote Procedure Call) Lainnya." },
  { "en": "Apa Itu Caching?", "id": "Menyimpan Data Sementara Untuk Akses Lebih Cepat." },
  { "en": "Apa Itu Cache Invalidation?", "id": "Proses Menghapus Data Cache Yang Kedaluwarsa." },
  { "en": "Apa Itu Strategi Write-Through Cache?", "id": "Menulis Ke Cache Dan Penyimpanan Utama." },
  { "en": "Apa Itu Strategi Write-Back Cache?", "id": "Menulis Ke Cache, Memperbarui Penyimpanan Nanti." },
  { "en": "Apa Itu Content Delivery Network (CDN)?", "id": "Jaringan Server Mendistribusikan Konten Secara Global." },
  { "en": "Apa Manfaat CDN (Content Delivery Network)?", "id": "Mengurangi Latensi, Meningkatkan Ketersediaan Konten." },
  { "en": "Apa Itu Model OSI (Open Systems Interconnection)?", "id": "Kerangka Konseptual Tujuh Lapisan Jaringan." },
  { "en": "Apa Fungsi Lapisan Fisik?", "id": "Transmisi Bit Mentah Melalui Media Fisik." },
  { "en": "Apa Fungsi Lapisan Tautan Data?", "id": "Transfer Data Andal Antar Node Berdekatan." },
  { "en": "Apa Itu Framing?", "id": "Membagi Aliran Bit Menjadi Unit Data." },
  { "en": "Apa Fungsi Lapisan Jaringan?", "id": "Pengalamatan Logis Dan Perutean Paket." },
  { "en": "Apa Fungsi Lapisan Transport?", "id": "Menyediakan Koneksi Ujung-ke-Ujung (End-to-End)." },
  { "en": "Apa Itu Multiplexing Lapisan Transport?", "id": "Memungkinkan Banyak Aplikasi Berbagi Satu Koneksi." },
  { "en": "Apa Fungsi Lapisan Sesi?", "id": "Mengelola, Mengakhiri Sesi Antar Aplikasi." },
  { "en": "Apa Fungsi Lapisan Presentasi?", "id": "Menangani Sintaks, Semantik Data (Enkripsi)." },
  { "en": "Apa Fungsi Lapisan Aplikasi?", "id": "Menyediakan Layanan Jaringan Untuk Aplikasi Pengguna." },
  { "en": "Apa Itu Encapsulation?", "id": "Menambahkan Header Ke Data Di Setiap Lapisan." },
  { "en": "Apa Itu Decapsulation?", "id": "Menghapus Header Dari Data Di Setiap Lapisan." },
  { "en": "Apa Itu Internet Protocol (IP) Suite?", "id": "Kumpulan Protokol Komunikasi Untuk Internet." },
  { "en": "Apa Itu Alamat IPv4?", "id": "Alamat Numerik 32-bit." },
  { "en": "Apa Itu Alamat IPv6?", "id": "Alamat Numerik 128-bit." },
  { "en": "Mengapa IPv6 Diciptakan?", "id": "Untuk Mengatasi Kehabisan Alamat IPv4." },
  { "en": "Apa Itu Subnet Mask?", "id": "Memisahkan Bagian Jaringan, Host Alamat IP." },
  { "en": "Apa Itu Classless Inter-Domain Routing (CIDR)?", "id": "Metode Alokasi Alamat IP Fleksibel." },
  { "en": "Apa Itu Gateway Default?", "id": "Router Keluar Dari Jaringan Lokal." },
  { "en": "Apa Itu Perutean (Routing)?", "id": "Proses Menentukan Jalur Paket Melalui Jaringan." },
  { "en": "Apa Itu Tabel Perutean?", "id": "Database Jalur Ke Tujuan Jaringan." },
  { "en": "Apa Itu Metrik Perutean?", "id": "Ukuran Digunakan Memilih Jalur Terbaik." },
  { "en": "Apa Itu Protokol Perutean Distance-Vector?", "id": "Router Berbagi Tabel Perutean Dengan Tetangga." },
  { "en": "Apa Itu Masalah Count-to-Infinity?", "id": "Masalah Dalam Protokol Distance-Vector." },
  { "en": "Apa Itu Protokol Perutean Link-State?", "id": "Router Membangun Peta Topologi Jaringan." },
  { "en": "Apa Itu Algoritma Dijkstra?", "id": "Digunakan Dalam Protokol Link-State." },
  { "en": "Apa Itu Open Shortest Path First (OSPF)?", "id": "Protokol Perutean Link-State Internal." },
  { "en": "Apa Itu Border Gateway Protocol (BGP)?", "id": "Protokol Perutean Eksterior Inti Internet." },
  { "en": "Apa Itu Autonomous System (AS)?", "id": "Sekelompok Jaringan Di Bawah Administrasi Sama." },
  { "en": "Apa Itu Transmission Control Protocol (TCP)?", "id": "Protokol Transport Andal, Berorientasi Koneksi." },
  { "en": "Apa Itu Tiga Arah Jabat Tangan TCP?", "id": "Proses Membangun Koneksi TCP." },
  { "en": "Apa Itu Kontrol Aliran TCP?", "id": "Mencegah Pengirim Membanjiri Penerima." },
  { "en": "Apa Itu Kontrol Kemacetan TCP?", "id": "Mengelola Kemacetan Dalam Jaringan." },
  { "en": "Apa Itu User Datagram Protocol (UDP)?", "id": "Protokol Transport Cepat, Tidak Andal." },
  { "en": "Kapan UDP (User Datagram Protocol) Digunakan?", "id": "Untuk Aplikasi Real-time (Streaming, Game)." },
  { "en": "Apa Itu Domain Name System (DNS)?", "id": "Menerjemahkan Nama Domain Mudah Dibaca Manusia." },
  { "en": "Apa Itu Resolver DNS?", "id": "Server Menerima Kueri DNS Dari Klien." },
  { "en": "Apa Itu Catatan A (A Record) DNS?", "id": "Memetakan Nama Host Ke Alamat IPv4." },
  { "en": "Apa Itu Catatan AAAA (AAAA Record) DNS?", "id": "Memetakan Nama Host Ke Alamat IPv6." },
  { "en": "Apa Itu Catatan MX (MX Record) DNS?", "id": "Menentukan Server Email Untuk Suatu Domain." },
  { "en": "Apa Itu Caching DNS?", "id": "Menyimpan Hasil Kueri DNS Sementara." },
  { "en": "Apa Itu Dynamic Host Configuration Protocol (DHCP)?", "id": "Memberikan Alamat IP Otomatis Ke Perangkat." },
  { "en": "Apa Itu Sewa (Lease) DHCP?", "id": "Periode Waktu Alamat IP Dialokasikan." },
  { "en": "Apa Itu Address Resolution Protocol (ARP)?", "id": "Memetakan Alamat IP Ke Alamat Fisik MAC." },
  { "en": "Apa Itu Gratuitous ARP?", "id": "Broadcast ARP Untuk Mengumumkan Alamat IP." },
  { "en": "Apa Itu Internet Control Message Protocol (ICMP)?", "id": "Untuk Pesan Diagnostik, Kesalahan Jaringan." },
  { "en": "Apa Itu Perintah `ping`?", "id": "Menggunakan ICMP (Internet Control Message Protocol) Menguji Konektivitas." },
  { "en": "Apa Itu Perintah `traceroute`?", "id": "Melacak Jalur Paket Ke Tujuan." },
  { "en": "Apa Itu Network Address Translation (NAT)?", "id": "Memungkinkan Banyak Perangkat Berbagi Satu IP." },
  { "en": "Apa Itu Port Address Translation (PAT)?", "id": "Jenis NAT (Network Address Translation) Paling Umum." },
  { "en": "Apa Itu Virtual Private Network (VPN)?", "id": "Membuat Koneksi Aman Melalui Jaringan Publik." },
  { "en": "Apa Itu Tunneling Protocol?", "id": "Membungkus Satu Protokol Jaringan Dalam Lainnya." },
  { "en": "Apa Itu IPsec (Internet Protocol Security)?", "id": "Kumpulan Protokol Mengamankan Komunikasi IP." },
  { "en": "Apa Itu SSL/TLS?", "id": "Protokol Kriptografi Mengamankan Komunikasi Web." },
  { "en": "Apa Itu Hypertext Transfer Protocol (HTTP)?", "id": "Protokol Fondasi Untuk World Wide Web." },
  { "en": "Apa Itu Kode Status HTTP?", "id": "Kode Tiga Digit Menunjukkan Hasil Permintaan." },
  { "en": "Apa Arti Kode Status 200 OK?", "id": "Permintaan Berhasil Diproses." },
  { "en": "Apa Arti Kode Status 404 Not Found?", "id": "Sumber Daya Yang Diminta Tidak Ditemukan." },
  { "en": "Apa Arti Kode Status 500 Internal Server Error?", "id": "Terjadi Kesalahan Umum Di Sisi Server." },
  { "en": "Apa Itu Metode GET HTTP?", "id": "Meminta Representasi Sumber Daya Tertentu." },
  { "en": "Apa Itu Metode POST HTTP?", "id": "Mengirim Data Untuk Diproses Ke Server." },
  { "en": "Apa Itu Cookie HTTP?", "id": "Data Kecil Dikirim Server Ke Browser." },
  { "en": "Apa Itu Sesi?", "id": "Cara Server Melacak Pengguna Antar Permintaan." },
  { "en": "Apa Itu File Transfer Protocol (FTP)?", "id": "Protokol Standar Transfer Berkas Komputer." },
  { "en": "Apa Itu Mode Aktif FTP?", "id": "Klien Memberitahu Server Port Mana Digunakan." },
  { "en": "Apa Itu Mode Pasif FTP?", "id": "Server Memberitahu Klien Port Mana Digunakan." },
  { "en": "Apa Itu Simple Mail Transfer Protocol (SMTP)?", "id": "Standar Pengiriman Email." },
  { "en": "Apa Itu Post Office Protocol (POP3)?", "id": "Standar Penerimaan Email Dari Server." },
  { "en": "Apa Itu Internet Message Access Protocol (IMAP)?", "id": "Standar Lain Untuk Mengakses Email." },
  { "en": "Apa Perbedaan Utama POP3 Dan IMAP?", "id": "IMAP Menyinkronkan Email Antar Beberapa Perangkat." },
  { "en": "Apa Itu Secure Shell (SSH)?", "id": "Protokol Jaringan Kriptografi Aman." },
  { "en": "Apa Itu Port Forwarding?", "id": "Meneruskan Lalu Lintas Dari Satu Port Jaringan." },
  { "en": "Apa Itu Telnet?", "id": "Protokol Jaringan Lama, Tidak Aman." },
  { "en": "Apa Itu Jaringan Nirkabel?", "id": "Jaringan Komputer Tanpa Menggunakan Kabel." },
  { "en": "Apa Itu IEEE 802.11?", "id": "Kumpulan Standar Untuk WLAN (Wi-Fi)." },
  { "en": "Apa Itu Basic Service Set (BSS)?", "id": "Sekelompok Stasiun Nirkabel Berkomunikasi." },
  { "en": "Apa Itu Extended Service Set (ESS)?", "id": "Dua Atau Lebih BSS Terhubung Bersama." },
  { "en": "Apa Itu CSMA/CA (Carrier Sense Multiple Access/Collision Avoidance)?", "id": "Protokol Akses Media Digunakan Wi-Fi." },
  { "en": "Apa Itu Masalah Terminal Tersembunyi?", "id": "Dua Stasiun Tak Dapat Mendengar Satu Sama Lain." },
  { "en": "Bagaimana RTS/CTS (Request to Send/Clear to Send) Membantu?", "id": "Mereservasi Media Untuk Mencegah Tabrakan." },
  { "en": "Apa Itu Jaringan Ad Hoc?", "id": "Jaringan Nirkabel Terdesentralisasi." },
  { "en": "Apa Itu Mode Infrastruktur?", "id": "Stasiun Terhubung Ke Access Point (AP)." },
  { "en": "Apa Itu Wi-Fi Direct?", "id": "Standar Koneksi Langsung Antar Perangkat." },
  { "en": "Apa Itu Bluetooth?", "id": "Standar Teknologi Nirkabel Jarak Pendek." },
  { "en": "Apa Itu Piconet Dalam Bluetooth?", "id": "Jaringan Kecil Hingga Delapan Perangkat." },
  { "en": "Apa Itu Perangkat Master Dalam Piconet?", "id": "Perangkat Yang Menginisiasi Dan Mengontrol Jaringan." },
  { "en": "Apa Itu Scatternet?", "id": "Dua Atau Lebih Piconet Yang Terhubung." },
  { "en": "Apa Itu Profil Bluetooth?", "id": "Spesifikasi Fungsionalitas Untuk Aplikasi Tertentu." },
  { "en": "Apa Itu Profil A2DP (Advanced Audio Distribution Profile)?", "id": "Untuk Streaming Audio Stereo Berkualitas Tinggi." },
  { "en": "Apa Itu Profil HSP (Headset Profile)?", "id": "Menyediakan Fungsionalitas Headset Nirkabel Dasar." },
  { "en": "Apa Itu Bluetooth Classic?", "id": "Versi Original Bluetooth Untuk Streaming Data." },
  { "en": "Apa Itu Bluetooth Low Energy (BLE)?", "id": "Versi Bluetooth Hemat Daya Untuk IoT." },
  { "en": "Apa Itu Jaringan Seluler?", "id": "Jaringan Komunikasi Terdistribusi Area Luas." },
  { "en": "Apa Itu Generasi Pertama (1G)?", "id": "Jaringan Seluler Analog Untuk Suara." },
  { "en": "Apa Itu Generasi Kedua (2G)?", "id": "Jaringan Seluler Digital Pertama (GSM, CDMA)." },
  { "en": "Apa Itu Generasi Ketiga (3G)?", "id": "Menawarkan Kecepatan Data Lebih Tinggi (Mobile Internet)." },
  { "en": "Apa Itu Generasi Keempat (4G LTE)?", "id": "Jaringan Seluler Broadband Berbasis IP." },
  { "en": "Apa Itu Generasi Kelima (5G)?", "id": "Jaringan Seluler Generasi Terbaru." },
  { "en": "Apa Itu Latensi?", "id": "Waktu Tunda Dalam Transmisi Data." },
  { "en": "Apa Keuntungan Utama Jaringan 5G?", "id": "Kecepatan Tinggi, Latensi Rendah, Konektivitas Masif." },
  { "en": "Apa Itu Gelombang Milimeter (mmWave)?", "id": "Pita Frekuensi Sangat Tinggi Untuk 5G." },
  { "en": "Apa Itu Massive MIMO (Multiple-Input Multiple-Output)?", "id": "Menggunakan Ratusan Antena Di Base Station." },
  { "en": "Apa Itu Network Slicing?", "id": "Membuat Jaringan Virtual Khusus Di Atas Fisik." },
  { "en": "Apa Itu Radio Kognitif?", "id": "Radio Cerdas Yang Dapat Beradaptasi Lingkungan." },
  { "en": "Apa Itu Dynamic Spectrum Access (DSA)?", "id": "Menggunakan Spektrum Frekuensi Secara Oportunistik." },
  { "en": "Apa Itu Sensing Spektrum?", "id": "Mendeteksi Pengguna Utama (Primary User) Spektrum." },
  { "en": "Apa Itu Software-Defined Radio (SDR)?", "id": "Fungsi Radio Diimplementasikan Dalam Perangkat Lunak." },
  { "en": "Apa Itu Pemrosesan Sinyal Analog?", "id": "Memproses Sinyal Dalam Bentuk Kontinunya." },
  { "en": "Apa Itu Pemrosesan Sinyal Digital?", "id": "Memproses Sinyal Dalam Bentuk Diskretnya." },
  { "en": "Apa Itu Transformasi Fourier?", "id": "Menguraikan Sinyal Menjadi Komponen Frekuensinya." },
  { "en": "Apa Itu Domain Waktu?", "id": "Representasi Sinyal Sebagai Fungsi Waktu." },
  { "en": "Apa Itu Domain Frekuensi?", "id": "Representasi Sinyal Sebagai Fungsi Frekuensi." },
  { "en": "Apa Itu Fast Fourier Transform (FFT)?", "id": "Algoritma Cepat Untuk Menghitung DFT." },
  { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Menggabungkan Dua Sinyal." },
  { "en": "Apa Itu Korelasi?", "id": "Ukuran Kemiripan Antara Dua Sinyal." },
  { "en": "Apa Itu Autokorelasi?", "id": "Korelasi Sinyal Dengan Dirinya Sendiri." },
  { "en": "Apa Itu Korelasi Silang?", "id": "Korelasi Antara Dua Sinyal Yang Berbeda." },
  { "en": "Apa Itu Filter Digital?", "id": "Sistem Memodifikasi Karakteristik Sinyal Digital." },
  { "en": "Apa Itu Filter Finite Impulse Response (FIR)?", "id": "Filter Digital Tanpa Umpan Balik." },
  { "en": "Apa Itu Filter Infinite Impulse Response (IIR)?", "id": "Filter Digital Dengan Umpan Balik." },
  { "en": "Apa Itu Jendela (Windowing) Dalam DSP?", "id": "Fungsi Matematis Mengurangi Kebocoran Spektral." },
  { "en": "Apa Itu Efek Gibbs?", "id": "Riak Dekat Diskontinuitas Dalam Sinyal." },
  { "en": "Apa Itu Teori Kendali?", "id": "Ilmu Mengontrol Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Sistem Loop Terbuka?", "id": "Kontrol Tanpa Menggunakan Umpan Balik." },
  { "en": "Apa Itu Sistem Loop Tertutup?", "id": "Kontrol Menggunakan Umpan Balik Dari Output." },
  { "en": "Apa Itu Fungsi Transfer?", "id": "Model Matematis Sistem Dalam Domain Laplace." },
  { "en": "Apa Itu Stabilitas Sistem?", "id": "Kemampuan Sistem Kembali Ke Keseimbangan." },
  { "en": "Apa Itu Respon Langkah (Step Response)?", "id": "Output Sistem Terhadap Input Langkah." },
  { "en": "Apa Itu Lewatan (Overshoot)?", "id": "Output Melebihi Nilai Setpoint Yang Diinginkan." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Respon Mencapai Persentase Tertentu." },
  { "en": "Apa Itu Pengontrol PID (Proportional-Integral-Derivative)?", "id": "Mekanisme Kontrol Umpan Balik Paling Umum." },
  { "en": "Apa Itu Robotika?", "id": "Desain, Konstruksi, Operasi, Penggunaan Robot." },
  { "en": "Apa Itu Robot Industri?", "id": "Robot Digunakan Dalam Aplikasi Manufaktur." },
  { "en": "Apa Itu Derajat Kebebasan (DOF)?", "id": "Jumlah Sumbu Gerak Independen Robot." },
  { "en": "Apa Itu Kinematika Robot?", "id": "Studi Gerakan Robot Tanpa Mempertimbangkan Gaya." },
  { "en": "Apa Itu Aktuator Robot?", "id": "Komponen Yang Menggerakkan Sendi Robot." },
  { "en": "Apa Itu End-Effector?", "id": "Alat Terpasang Di Ujung Lengan Robot." },
  { "en": "Apa Itu Visi Komputer?", "id": "Komputer 'Melihat' Dan Menginterpretasi Dunia Visual." },
  { "en": "Apa Itu Kecerdasan Buatan (AI)?", "id": "Simulasi Kecerdasan Manusia Oleh Mesin." },
  { "en": "Apa Itu Pembelajaran Mesin (Machine Learning)?", "id": "Algoritma Belajar Dari Data Tanpa Diprogram." },
  { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "Model Komputasi Terinspirasi Jaringan Saraf Biologis." },
  { "en": "Apa Itu Deep Learning?", "id": "Pembelajaran Mesin Menggunakan Jaringan Saraf Dalam." },
  { "en": "Apa Itu Sistem Tertanam?", "id": "Sistem Komputer Dalam Perangkat Lebih Besar." },
  { "en": "Apa Itu Real-Time System?", "id": "Sistem Harus Merespon Dalam Batas Waktu." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Pada Satu Sirkuit Terpadu." },
  { "en": "Apa Itu Sensor?", "id": "Mengubah Besaran Fisik Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Transduser?", "id": "Mengonversi Satu Bentuk Energi Ke Lainnya." },
  { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu Berdasarkan Efek Seebeck." },
  { "en": "Apa Itu Strain Gauge?", "id": "Mengukur Regangan Mekanis Suatu Benda." },
  { "en": "Apa Itu Akselerometer?", "id": "Mengukur Percepatan Linier Dan Getaran." },
  { "en": "Apa Itu Giroskop?", "id": "Mengukur Kecepatan Atau Orientasi Sudut." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Motor Yang Ditenagai Oleh Arus Searah." },
  { "en": "Apa Itu Motor AC (Alternating Current)?", "id": "Motor Yang Ditenagai Oleh Arus Bolak-Balik." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor Bergerak Dalam Langkah-langkah Sudut Diskret." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor Dengan Umpan Balik Posisi Presisi." },
  { "en": "Apa Itu Elektronika Daya?", "id": "Kontrol Dan Konversi Daya Listrik." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah AC (Alternating Current) Menjadi DC (Direct Current)." },
  { "en": "Apa Itu Inverter?", "id": "Mengubah DC (Direct Current) Menjadi AC (Alternating Current)." },
  { "en": "Apa Itu Konverter DC-DC?", "id": "Mengubah Satu Level Tegangan DC Lainnya." },
  { "en": "Apa Itu Konverter Buck?", "id": "Konverter DC-DC Penurun Tegangan." },
  { "en": "Apa Itu Konverter Boost?", "id": "Konverter DC-DC Penaik Tegangan." },
  { "en": "Apa Itu Thyristor (SCR)?", "id": "Sakelar Semikonduktor Daya Terkendali." },
  { "en": "Apa Itu TRIAC (Triode for Alternating Current)?", "id": "Sakelar Semikonduktor Dua Arah Untuk AC." },
  { "en": "Apa Itu IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Sakelar Semikonduktor Menggabungkan MOSFET, BJT." },
  { "en": "Apa Itu Sistem Tenaga?", "id": "Pembangkitan, Transmisi, Distribusi Tenaga Listrik." },
  { "en": "Apa Itu Generator Sinkron?", "id": "Pembangkit Listrik AC (Alternating Current) Utama." },
  { "en": "Apa Itu Jaringan Transmisi?", "id": "Menyalurkan Daya Jarak Jauh Tegangan Tinggi." },
  { "en": "Apa Itu Jaringan Distribusi?", "id": "Menyalurkan Daya Ke Konsumen Tegangan Rendah." },
  { "en": "Apa Itu Gardu Induk?", "id": "Mengubah Level Tegangan Dalam Sistem Tenaga." },
  { "en": "Apa Itu Pemutus Sirkuit?", "id": "Melindungi Peralatan Dari Arus Gangguan." },
  { "en": "Apa Itu Relai Proteksi?", "id": "Mendeteksi Gangguan, Memicu Pemutus Sirkuit." },
  { "en": "Apa Itu Kualitas Daya?", "id": "Karakteristik Tegangan, Arus, Frekuensi Listrik." },
  { "en": "Apa Itu Harmonik?", "id": "Frekuensi Kelipatan Dari Frekuensi Dasar." },
  { "en": "Apa Itu Faktor Daya?", "id": "Rasio Daya Nyata Terhadap Daya Semu." },
  { "en": "Apa Itu Energi Terbarukan?", "id": "Energi Dari Sumber Daya Alam Berkelanjutan." },
  { "en": "Apa Itu Tenaga Surya Fotovoltaik?", "id": "Mengubah Cahaya Matahari Menjadi Listrik." },
  { "en": "Apa Itu Tenaga Angin?", "id": "Menggunakan Angin Memutar Turbin, Generator." },
  { "en": "Apa Itu Smart Grid?", "id": "Jaringan Listrik Modern Dengan Teknologi Informasi." },
  { "en": "Apa Itu Optik?", "id": "Cabang Fisika Mempelajari Perilaku Cahaya." },
  { "en": "Apa Itu Refleksi?", "id": "Pemantulan Cahaya Dari Suatu Permukaan." },
  { "en": "Apa Itu Refraksi?", "id": "Pembelokan Cahaya Saat Melintasi Medium." },
  { "en": "Apa Itu Lensa?", "id": "Komponen Optik Memfokuskan, Menyebarkan Cahaya." },
  { "en": "Apa Itu Difraksi?", "id": "Penyebaran Gelombang Saat Melewati Hambatan." },
  { "en": "Apa Itu Interferensi?", "id": "Superposisi Gelombang Menghasilkan Pola Baru." },
  { "en": "Apa Itu Polarisasi?", "id": "Orientasi Osilasi Gelombang Elektromagnetik." },
  { "en": "Apa Itu Laser?", "id": "Sumber Cahaya Koheren, Monokromatik, Terarah." },
  { "en": "Apa Itu Elektromagnetisme Komputasi?", "id": "Menyelesaikan Masalah Elektromagnetik Dengan Komputer." },
  { "en": "Apa Itu Metode Elemen Hingga (FEM)?", "id": "Metode Numerik Untuk Menyelesaikan Persamaan Diferensial." },
  { "en": "Apa Itu Metode Beda Hingga Domain Waktu (FDTD)?", "id": "Menyelesaikan Persamaan Maxwell Langsung Dalam Waktu." },
  { "en": "Apa Itu Metode Momen (MoM)?", "id": "Metode Numerik Menganalisis Antena, Hamburan." },
  { "en": "Apa Itu Sel Yee?", "id": "Grid Sel Dasar Dalam Metode FDTD." },
  { "en": "Apa Itu Kondisi Batas Penyerapan (ABC)?", "id": "Mensimulasikan Ruang Terbuka Dalam Komputasi." },
  { "en": "Apa Itu Perfectly Matched Layer (PML)?", "id": "Lapisan Penyerapan Buatan Untuk Menghentikan Gelombang." },
  { "en": "Apa Itu Bioelektromagnetisme?", "id": "Studi Interaksi Medan Elektromagnetik, Sistem Biologis." },
  { "en": "Apa Itu Specific Absorption Rate (SAR)?", "id": "Laju Penyerapan Energi RF Oleh Tubuh." },
  { "en": "Apa Itu Pencitraan Impedansi Listrik?", "id": "Teknik Pencitraan Berdasarkan Konduktivitas Listrik." },
  { "en": "Apa Itu Termodinamika?", "id": "Studi Panas, Kerja, Energi, Dan Entropi." },
  { "en": "Apa Itu Sistem Terbuka?", "id": "Dapat Bertukar Energi Dan Materi." },
  { "en": "Apa Itu Sistem Tertutup?", "id": "Dapat Bertukar Energi, Tetapi Bukan Materi." },
  { "en": "Apa Itu Sistem Terisolasi?", "id": "Tidak Dapat Bertukar Energi Atau Materi." },
  { "en": "Apa Itu Proses Reversibel?", "id": "Proses Yang Dapat Dibalikkan Tanpa Kerugian." },
  { "en": "Apa Itu Proses Ireversibel?", "id": "Proses Yang Tidak Dapat Dibalikkan Begitu Saja." },
  { "en": "Apa Itu Proses Adiabatik?", "id": "Proses Tanpa Perpindahan Panas." },
  { "en": "Apa Itu Proses Isothermal?", "id": "Proses Pada Suhu Konstan." },
  { "en": "Apa Itu Proses Isobarik?", "id": "Proses Pada Tekanan Konstan." },
  { "en": "Apa Itu Proses Isokorik?", "id": "Proses Pada Volume Konstan." },
  { "en": "Apa Itu Entalpi?", "id": "Ukuran Total Energi Dalam Sistem." },
  { "en": "Apa Itu Energi Bebas Gibbs?", "id": "Energi Yang Tersedia Untuk Melakukan Kerja." },
  { "en": "Apa Itu Mesin Panas?", "id": "Mengubah Energi Panas Menjadi Kerja Mekanis." },
  { "en": "Apa Itu Pompa Panas?", "id": "Memindahkan Panas Dari Dingin Ke Panas." },
  { "en": "Apa Itu Koefisien Kinerja (COP)?", "id": "Ukuran Efisiensi Pompa Panas." },
  { "en": "Apa Itu Mekanika Statistik?", "id": "Menghubungkan Sifat Mikro Ke Makro." },
  { "en": "Apa Itu Ensemble Mikrokanonikal?", "id": "Sistem Terisolasi Dengan Energi Tetap." },
  { "en": "Apa Itu Ensemble Kanonikal?", "id": "Sistem Kontak Reservoir Panas Suhu Tetap." },
  { "en": "Apa Itu Fungsi Partisi?", "id": "Menghubungkan Statistik Dengan Sifat Termodinamika." },
  { "en": "Apa Itu Statistika Maxwell-Boltzmann?", "id": "Mendeskripsikan Partikel Klasik Yang Dapat Dibedakan." },
  { "en": "Apa Itu Teknik Nuklir?", "id": "Penerapan Fisika Nuklir Dan Reaksi Atom." },
  { "en": "Apa Itu Reaksi Berantai?", "id": "Reaksi Fisi Yang Menopang Dirinya Sendiri." },
  { "en": "Apa Itu Massa Kritis?", "id": "Massa Minimum Untuk Reaksi Berantai." },
  { "en": "Apa Itu Penampang Lintang Nuklir?", "id": "Probabilitas Terjadinya Reaksi Nuklir." },
  { "en": "Apa Itu Aktivasi Neutron?", "id": "Proses Membuat Material Menjadi Radioaktif." },
  { "en": "Apa Itu Detektor Radiasi?", "id": "Perangkat Mengukur Radiasi Pengion." },
  { "en": "Apa Itu Pencacah Geiger?", "id": "Jenis Detektor Radiasi Berisi Gas." },
  { "en": "Apa Itu Detektor Sintilasi?", "id": "Menggunakan Material Yang Bercahaya Saat Terkena." },
  { "en": "Apa Itu Spektroskopi Gamma?", "id": "Menganalisis Spektrum Energi Radiasi Gamma." },
  { "en": "Apa Itu Kedokteran Nuklir?", "id": "Penggunaan Radioisotop Untuk Diagnosis, Terapi." },
  { "en": "Apa Itu Positron Emission Tomography (PET)?", "id": "Teknik Pencitraan Medis Fungsional." },
  { "en": "Apa Itu Reaktor Fusi?", "id": "Perangkat Eksperimental Mengendalikan Reaksi Fusi." },
  { "en": "Apa Itu Kriteria Lawson?", "id": "Syarat Untuk Mencapai Fusi Berkelanjutan." },
  { "en": "Apa Itu Rekayasa Perangkat Lunak?", "id": "Disiplin Teknik Untuk Pengembangan Perangkat Lunak." },
  { "en": "Apa Itu Pemrograman Berorientasi Aspek (AOP)?", "id": "Memisahkan Masalah Lintas Sektor." },
  { "en": "Apa Itu Pemrograman Meta?", "id": "Program Yang Menulis Atau Memanipulasi Program." },
  { "en": "Apa Itu Domain-Specific Language (DSL)?", "id": "Bahasa Disesuaikan Untuk Domain Masalah." },
  { "en": "Apa Itu Kualitas Kode?", "id": "Ukuran Seberapa Baik Kode Ditulis." },
  { "en": "Apa Itu Code Smell?", "id": "Indikasi Masalah Lebih Dalam Kode." },
  { "en": "Apa Itu Utang Teknis (Technical Debt)?", "id": "Implikasi Pengerjaan Ulang Akibat Jalan Pintas." },
  { "en": "Apa Itu Pengujian Regresi?", "id": "Memastikan Perubahan Baru Tidak Merusak Fungsi." },
  { "en": "Apa Itu Pengujian Mutasi?", "id": "Mengubah Kode Untuk Menguji Kualitas Tes." },
  { "en": "Apa Itu Pengujian Alfa?", "id": "Pengujian Internal Sebelum Rilis." },
  { "en": "Apa Itu Pengujian Beta?", "id": "Pengujian Eksternal Oleh Pengguna Terbatas." },
  { "en": "Apa Itu Pengujian End-to-End?", "id": "Menguji Alur Aplikasi Lengkap." },
  { "en": "Apa Itu Code Review?", "id": "Pemeriksaan Manual Kode Oleh Tim." },
  { "en": "Apa Itu Static Program Analysis?", "id": "Menganalisis Kode Tanpa Menjalankannya." },
  { "en": "Apa Itu Linter?", "id": "Alat Analisis Statis Untuk Gaya Kode." },
  { "en": "Apa Itu Build Automation?", "id": "Otomatisasi Proses Kompilasi Dan Pengemasan." },
  { "en": "Apa Itu Maven?", "id": "Alat Otomasi Build Untuk Proyek Java." },
  { "en": "Apa Itu Gradle?", "id": "Alat Otomasi Build Modern Lainnya." },
  { "en": "Apa Itu Dependency Management?", "id": "Mengelola Pustaka Eksternal Yang Digunakan Proyek." },
  { "en": "Apa Itu Monorepo?", "id": "Repositori Kode Tunggal Untuk Banyak Proyek." },
  { "en": "Apa Itu Polyrepo?", "id": "Setiap Proyek Memiliki Repositori Kodenya Sendiri." },
  { "en": "Apa Itu Semantic Versioning (SemVer)?", "id": "Skema Standar Untuk Versi Perangkat Lunak." },
  { "en": "Apa Itu Model-View-Controller (MVC)?", "id": "Pola Arsitektur Perangkat Lunak." },
  { "en": "Apa Itu Model-View-ViewModel (MVVM)?", "id": "Pola Arsitektur Turunan Dari MVC." },
  { "en": "Apa Itu Command Query Responsibility Segregation (CQRS)?", "id": "Pola Memisahkan Operasi Baca Dan Tulis." },
  { "en": "Apa Itu Event Sourcing?", "id": "Menyimpan Semua Perubahan Sebagai Urutan Event." },
  { "en": "Apa Itu Arsitektur Heksagonal (Ports and Adapters)?", "id": "Pola Arsitektur Memisahkan Logika Inti." },
  { "en": "Apa Itu Kode Terkelola (Managed Code)?", "id": "Kode Dijalankan Di Bawah Runtime (CLR, JVM)." },
  { "en": "Apa Itu Kode Asli (Native Code)?", "id": "Kode Dikompilasi Langsung Ke Instruksi Mesin." },
  { "en": "Apa Itu Common Language Runtime (CLR)?", "id": "Lingkungan Eksekusi Untuk Platform .NET." },
  { "en": "Apa Itu Java Virtual Machine (JVM)?", "id": "Mesin Virtual Menjalankan Bytecode Java." },
  { "en": "Apa Itu Bytecode?", "id": "Representasi Kode Antara Yang Portabel." },
  { "en": "Apa Itu Pengetikan Statis (Static Typing)?", "id": "Pemeriksaan Tipe Dilakukan Saat Kompilasi." },
  { "en": "Apa Itu Pengetikan Dinamis (Dynamic Typing)?", "id": "Pemeriksaan Tipe Dilakukan Saat Runtime." },
  { "en": "Apa Keuntungan Pengetikan Statis?", "id": "Menemukan Error Lebih Awal, Performa Baik." },
  { "en": "Apa Keuntungan Pengetikan Dinamis?", "id": "Fleksibilitas Dan Pengembangan Lebih Cepat." },
  { "en": "Apa Itu Tipe Kuat (Strong Typing)?", "id": "Bahasa Menerapkan Aturan Tipe Secara Ketat." },
  { "en": "Apa Itu Tipe Lemah (Weak Typing)?", "id": "Bahasa Memungkinkan Konversi Tipe Implisit." },
  { "en": "Apa Itu Inferensi Tipe (Type Inference)?", "id": "Kompiler Otomatis Menentukan Tipe Variabel." },
  { "en": "Apa Itu Generics (Pemrograman)?", "id": "Memungkinkan Tipe Menjadi Parameter Algoritma." },
  { "en": "Apa Itu Closure?", "id": "Fungsi Yang Mengingat Lingkungan Leksikalnya." },
  { "en": "Apa Itu Fungsi Tingkat Tinggi (Higher-Order Function)?", "id": "Fungsi Mengambil, Mengembalikan Fungsi Lain." },
  { "en": "Apa Itu Currying?", "id": "Mengubah Fungsi Banyak Argumen." },
  { "en": "Apa Itu Immutability?", "id": "Sifat Objek Yang Tidak Dapat Diubah." },
  { "en": "Apa Itu Efek Samping (Side Effect)?", "id": "Fungsi Memodifikasi Keadaan Di Luar Lingkupnya." },
  { "en": "Apa Itu Fungsi Murni (Pure Function)?", "id": "Fungsi Tanpa Efek Samping." },
  { "en": "Apa Itu Pemrograman Deklaratif?", "id": "Menjelaskan 'Apa' Hasilnya, Bukan 'Bagaimana'." },
  { "en": "Apa Itu Pemrograman Imperatif?", "id": "Menjelaskan Langkah-langkah Mengubah Keadaan Program." },
  { "en": "Apa Itu SQL (Structured Query Language)?", "id": "Contoh Bahasa Pemrograman Deklaratif." },
  { "en": "Apa Itu Bahasa C?", "id": "Contoh Bahasa Pemrograman Imperatif." },
  { "en": "Apa Itu Representasi Komplemen Dua?", "id": "Metode Representasi Bilangan Bertanda." },
  { "en": "Apa Itu Endianness?", "id": "Urutan Byte Dalam Representasi Data." },
  { "en": "Apa Itu Big-Endian?", "id": "Byte Paling Signifikan Disimpan Pertama." },
  { "en": "Apa Itu Little-Endian?", "id": "Byte Paling Tidak Signifikan Disimpan Pertama." },
  { "en": "Apa Itu Bilangan Titik Tetap (Fixed-Point)?", "id": "Representasi Bilangan Dengan Posisi Koma Tetap." },
  { "en": "Apa Itu Bilangan Titik Mengambang (Floating-Point)?", "id": "Representasi Bilangan Dengan Posisi Koma Fleksibel." },
  { "en": "Apa Itu Standar IEEE 754?", "id": "Standar Teknis Untuk Aritmetika Titik Mengambang." },
  { "en": "Apa Itu Error Pembulatan?", "id": "Error Akibat Keterbatasan Presisi Representasi." },
  { "en": "Apa Itu Kecerdasan Buatan (AI)?", "id": "Simulasi Kecerdasan Manusia Oleh Mesin." },
  { "en": "Apa Beda AI (Artificial Intelligence) Lemah Dan Kuat?", "id": "Lemah Spesifik, Kuat Punya Kesadaran Umum." },
  { "en": "Apa Itu Pembelajaran Mesin (Machine Learning)?", "id": "Subset AI Yang Belajar Dari Data." },
  { "en": "Apa Itu Pembelajaran Terawasi (Supervised Learning)?", "id": "Belajar Dari Data Yang Sudah Diberi Label." },
  { "en": "Apa Itu Pembelajaran Tak Terawasi?", "id": "Menemukan Pola Dalam Data Tanpa Label." },
  { "en": "Apa Itu Pembelajaran Penguatan (Reinforcement Learning)?", "id": "Belajar Melalui Trial-and-Error Dan Hadiah." },
  { "en": "Apa Itu Klasifikasi Dalam Pembelajaran Mesin?", "id": "Memprediksi Label Kategori (Contoh: Spam/Bukan)." },
  { "en": "Apa Itu Regresi Dalam Pembelajaran Mesin?", "id": "Memprediksi Nilai Kontinu (Contoh: Harga)." },
  { "en": "Apa Itu Clustering?", "id": "Mengelompokkan Data Serupa Tanpa Label Awal." },
  { "en": "Apa Itu Rekayasa Fitur (Feature Engineering)?", "id": "Memilih, Mengubah Variabel Input Untuk Model." },
  { "en": "Apa Itu Overfitting?", "id": "Model Terlalu Cocok Data Latih, Gagal Generalisasi." },
  { "en": "Apa Itu Underfitting?", "id": "Model Terlalu Sederhana Menangkap Pola Data." },
  { "en": "Apa Itu Validasi Silang (Cross-Validation)?", "id": "Teknik Mengevaluasi Kinerja Model Secara Objektif." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?", "id": "Model Komputasi Terinspirasi Struktur Otak." },
  { "en": "Apa Itu Pembelajaran Mendalam (Deep Learning)?", "id": "Jaringan Saraf Dengan Banyak Lapisan Tersembunyi." },
  { "en": "Apa Itu Fungsi Aktivasi?", "id": "Memperkenalkan Non-Linearitas Ke Dalam Neuron." },
  { "en": "Apa Itu Propagasi Balik (Backpropagation)?", "id": "Algoritma Untuk Melatih Jaringan Saraf Tiruan." },
  { "en": "Apa Itu Jaringan Saraf Konvolusional (CNN)?", "id": "Arsitektur Deep Learning Untuk Analisis Gambar." },
  { "en": "Apa Itu Jaringan Saraf Berulang (RNN)?", "id": "Arsitektur Jaringan Untuk Memproses Data Sekuensial." },
  { "en": "Apa Itu Long Short-Term Memory (LSTM)?", "id": "Jenis RNN Mengatasi Masalah Gradien Hilang." },
  { "en": "Apa Itu Pemrosesan Bahasa Alami (NLP)?", "id": "AI Memahami, Memproses Bahasa Manusia." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "AI Menganalisis, Memahami Informasi Visual." },
  { "en": "Apa Itu Sistem Rekomendasi?", "id": "Memprediksi Preferensi Pengguna Terhadap Item." },
  { "en": "Apa Itu Pemfilteran Kolaboratif?", "id": "Metode Sistem Rekomendasi Berbasis Pengguna." },
  { "en": "Apa Itu Robotika?", "id": "Desain, Konstruksi, Operasi, Dan Penggunaan Robot." },
  { "en": "Apa Itu Aktuator?", "id": "Komponen Robot Yang Menghasilkan Gerakan Fisik." },
  { "en": "Apa Itu Sensor?", "id": "Komponen Memberi Informasi Lingkungan Ke Robot." },
  { "en": "Apa Itu Kinematika?", "id": "Studi Gerakan Tanpa Mempertimbangkan Penyebabnya." },
  { "en": "Apa Itu Perencanaan Gerak (Motion Planning)?", "id": "Menemukan Jalur Gerak Bebas Hambatan." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Saling Terhubung." },
  { "en": "Apa Itu Komputasi Tepi (Edge Computing)?", "id": "Memproses Data Lebih Dekat Ke Sumbernya." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Menyediakan Layanan Komputasi Melalui Internet." },
  { "en": "Apa Itu Big Data?", "id": "Kumpulan Data Sangat Besar, Kompleks, Cepat." },
  { "en": "Apa Saja Tiga V Dari Big Data?", "id": "Volume, Velocity (Kecepatan), Dan Variety (Variasi)." },
  { "en": "Apa Itu Blockchain?", "id": "Buku Besar Digital Terdistribusi Yang Aman." },
  { "en": "Apa Itu Cryptocurrency?", "id": "Aset Digital Menggunakan Kriptografi Untuk Keamanan." },
  { "en": "Apa Itu Kontrak Pintar (Smart Contract)?", "id": "Kontrak Otomatis Dijalankan Di Blockchain." },
  { "en": "Apa Itu Realitas Virtual (VR)?", "id": "Simulasi Lingkungan Tiga Dimensi Yang Imersif." },
  { "en": "Apa Itu Realitas Tertambah (AR)?", "id": "Melapisi Informasi Digital Ke Dunia Nyata." },
  { "en": "Apa Itu Komputasi Kuantum?", "id": "Komputasi Menggunakan Fenomena Mekanika Kuantum." },
  { "en": "Apa Itu Qubit?", "id": "Unit Dasar Informasi Dalam Komputer Kuantum." },
  { "en": "Apa Itu Superposisi?", "id": "Qubit Berada Dalam Beberapa Keadaan Sekaligus." },
  { "en": "Apa Itu Keterikatan (Entanglement)?", "id": "Koneksi Kuantum Antara Dua Atau Lebih Qubit." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Praktik Melindungi Sistem Dari Serangan Digital." },
  { "en": "Apa Itu Malware?", "id": "Perangkat Lunak Jahat (Virus, Ransomware)." },
  { "en": "Apa Itu Phishing?", "id": "Upaya Penipuan Mendapatkan Informasi Sensitif." },
  { "en": "Apa Itu Enkripsi?", "id": "Mengubah Data Menjadi Kode Rahasia." },
  { "en": "Apa Itu Firewall?", "id": "Sistem Keamanan Jaringan Memantau Lalu Lintas." },
  { "en": "Apa Itu Autentikasi Dua Faktor (2FA)?", "id": "Membutuhkan Dua Metode Verifikasi Identitas." },
  { "en": "Apa Itu Biometrik?", "id": "Identifikasi Berdasarkan Karakteristik Fisik Unik." },
  { "en": "Apa Itu Rekayasa Perangkat Lunak?", "id": "Aplikasi Prinsip Rekayasa Pengembangan Perangkat Lunak." },
  { "en": "Apa Itu Siklus Hidup Pengembangan (SDLC)?", "id": "Proses Untuk Merencanakan, Membuat Perangkat Lunak." },
  { "en": "Apa Itu Agile?", "id": "Metodologi Pengembangan Iteratif Dan Kolaboratif." },
  { "en": "Apa Itu Scrum?", "id": "Kerangka Kerja Agile Untuk Mengelola Proyek." },
  { "en": "Apa Itu DevOps?", "id": "Kultur Menggabungkan Pengembangan, Operasi Perangkat Lunak." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Mengintegrasikan Kode Baru Ke Repositori Utama." },
  { "en": "Apa Itu Continuous Delivery (CD)?", "id": "Otomatisasi Rilis Perangkat Lunak Ke Produksi." },
  { "en": "Apa Itu Kontrol Versi?", "id": "Sistem Merekam Perubahan Kode Seiring Waktu." },
  { "en": "Apa Itu Git?", "id": "Sistem Kontrol Versi Terdistribusi Populer." },
  { "en": "Apa Itu Antarmuka Pengguna (UI)?", "id": "Bagian Visual Aplikasi Berinteraksi Pengguna." },
  { "en": "Apa Itu Pengalaman Pengguna (UX)?", "id": "Keseluruhan Pengalaman Seseorang Menggunakan Produk." },
  { "en": "Apa Itu Database?", "id": "Kumpulan Data Terorganisir Secara Elektronik." },
  { "en": "Apa Itu SQL (Structured Query Language)?", "id": "Bahasa Standar Untuk Mengelola Database Relasional." },
  { "en": "Apa Itu Database NoSQL?", "id": "Database Non-relasional Untuk Data Tak Terstruktur." },
  { "en": "Apa Itu Jaringan Komputer?", "id": "Dua Atau Lebih Komputer Saling Terhubung." },
  { "en": "Apa Itu Internet?", "id": "Jaringan Global Dari Jaringan Komputer." },
  { "en": "Apa Itu Alamat IP (Internet Protocol)?", "id": "Label Numerik Unik Untuk Perangkat Jaringan." },
  { "en": "Apa Itu Router?", "id": "Perangkat Meneruskan Paket Data Antar Jaringan." },
  { "en": "Apa Itu Switch?", "id": "Perangkat Menghubungkan Perangkat Dalam Jaringan Lokal." },
  { "en": "Apa Itu Protokol?", "id": "Aturan Yang Mengatur Komunikasi Data." },
  { "en": "Apa Itu TCP/IP (Transmission Control Protocol/Internet Protocol)?", "id": "Kumpulan Protokol Komunikasi Inti Internet." },
  { "en": "Apa Itu HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Untuk Mengakses World Wide Web." },
  { "en": "Apa Itu Domain Name System (DNS)?", "id": "Menerjemahkan Nama Domain Menjadi Alamat IP." },
  { "en": "Apa Itu Sistem Operasi (OS)?", "id": "Perangkat Lunak Mengelola Perangkat Keras Komputer." },
  { "en": "Apa Itu Kernel?", "id": "Komponen Inti Dari Sistem Operasi." },
  { "en": "Apa Itu Proses?", "id": "Instans Program Komputer Yang Sedang Dijalankan." },
  { "en": "Apa Itu Thread?", "id": "Unit Eksekusi Terkecil Dalam Suatu Proses." },
  { "en": "Apa Itu Manajemen Memori?", "id": "Fungsi OS Mengelola Memori Utama." },
  { "en": "Apa Itu Arsitektur Komputer?", "id": "Desain Fungsional, Organisasi Sistem Komputer." },
  { "en": "Apa Itu CPU (Central Processing Unit)?", "id": "Otak Komputer Yang Menjalankan Instruksi." },
  { "en": "Apa Itu Memori Akses Acak (RAM)?", "id": "Memori Komputer Utama Yang Volatil." },
  { "en": "Apa Itu Unit Logika Aritmetika (ALU)?", "id": "Bagian CPU Melakukan Operasi Aritmetika, Logika." },
  { "en": "Apa Itu Set Instruksi?", "id": "Kumpulan Perintah Dasar Dipahami Prosesor." },
  { "en": "Apa Itu Algoritma?", "id": "Prosedur Langkah-demi-Langkah Untuk Menyelesaikan Masalah." },
  { "en": "Apa Itu Struktur Data?", "id": "Cara Mengorganisir, Menyimpan Data Di Komputer." },
  { "en": "Apa Itu Kompleksitas Algoritma?", "id": "Ukuran Sumber Daya Yang Digunakan Algoritma." },
  { "en": "Apa Itu Notasi Big O?", "id": "Mendeskripsikan Kinerja Batas Atas Suatu Algoritma." },
  { "en": "Apa Itu Pengurutan (Sorting)?", "id": "Menyusun Elemen Dalam Urutan Tertentu." },
  { "en": "Apa Itu Pencarian (Searching)?", "id": "Menemukan Item Tertentu Dalam Kumpulan Data." },
  { "en": "Apa Itu Rekursi?", "id": "Teknik Di Mana Fungsi Memanggil Dirinya Sendiri." },
  { "en": "Apa Itu Paradigma Pemrograman?", "id": "Gaya Atau Cara Mengklasifikasikan Bahasa Pemrograman." },
  { "en": "Apa Itu Pemrograman Berorientasi Objek (OOP)?", "id": "Paradigma Berdasarkan Konsep 'Objek'." },
  { "en": "Apa Itu Pemrograman Fungsional?", "id": "Paradigma Memperlakukan Komputasi Sebagai Evaluasi Fungsi." },
  { "en": "Apa Itu Kompiler?", "id": "Menerjemahkan Seluruh Program Sebelum Dijalankan." },
  { "en": "Apa Itu Interpreter?", "id": "Menerjemahkan Dan Menjalankan Program Baris Demi Baris." },
  { "en": "Apa Itu Debugging?", "id": "Proses Menemukan Dan Memperbaiki Error." },
  { "en": "Apa Itu Elektronika?", "id": "Ilmu Mengontrol Aliran Elektron." },
  { "en": "Apa Itu Sirkuit Analog?", "id": "Sirkuit Yang Memproses Sinyal Analog Kontinu." },
  { "en": "Apa Itu Sirkuit Digital?", "id": "Sirkuit Memproses Sinyal Digital Diskret." },
  { "en": "Apa Itu Semikonduktor?", "id": "Material Sifatnya Antara Konduktor, Isolator." },
  { "en": "Apa Itu Transistor?", "id": "Perangkat Semikonduktor Menguatkan, Mengalihkan Sinyal." },
  { "en": "Apa Itu Sirkuit Terpadu (IC)?", "id": "Satu Set Sirkuit Elektronik Pada Chip." },
  { "en": "Apa Itu Hukum Ohm?", "id": "Menjelaskan Hubungan Antara Tegangan, Arus, Resistansi." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Pasif Penyimpan Energi Medan Listrik." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Pasif Penyimpan Energi Medan Magnet." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Mengizinkan Arus Mengalir Satu Arah." },
  { "en": "Apa Itu Teori Antrian?", "id": "Studi Matematis Tentang Garis Tunggu Atau Antrian." },
  { "en": "Apa Itu Proses Kedatangan Poisson?", "id": "Model Probabilistik Untuk Kedatangan Acak." },
  { "en": "Apa Itu Distribusi Waktu Layanan Eksponensial?", "id": "Asumsi Umum Untuk Waktu Penyelesaian Layanan." },
  { "en": "Apa Itu Notasi Kendall?", "id": "Notasi Standar Untuk Mendeskripsikan Model Antrian." },
  { "en": "Apa Arti M/M/1 Dalam Notasi Kendall?", "id": "Kedatangan Markovian, Layanan Markovian, Satu Server." },
  { "en": "Apa Itu Hukum Little?", "id": "Menghubungkan Jumlah Pelanggan, Laju Kedatangan, Waktu Tunggu." },
  { "en": "Apa Itu Pemanfaatan (Utilization) Server?", "id": "Fraksi Waktu Server Sibuk." },
  { "en": "Apa Itu Teori Permainan (Game Theory)?", "id": "Studi Model Matematis Interaksi Strategis." },
  { "en": "Apa Itu Permainan Zero-Sum?", "id": "Keuntungan Satu Pemain Sama Dengan Kerugian Pemain Lain." },
  { "en": "Apa Itu Ekuilibrium Nash?", "id": "Strategi Optimal, Mengingat Strategi Pemain Lain." },
  { "en": "Apa Itu Dilema Tahanan?", "id": "Contoh Paradoks Dalam Pengambilan Keputusan." },
  { "en": "Apa Itu Strategi Dominan?", "id": "Strategi Terbaik Terlepas Dari Tindakan Lawan." },
  { "en": "Apa Itu Riset Operasi (Operations Research)?", "id": "Penerapan Metode Analitis Untuk Pengambilan Keputusan." },
  { "en": "Apa Itu Pemrograman Linear?", "id": "Teknik Optimisasi Untuk Masalah Dengan Kendala Linear." },
  { "en": "Apa Itu Metode Simpleks?", "id": "Algoritma Untuk Menyelesaikan Masalah Pemrograman Linear." },
  { "en": "Apa Itu Masalah Transportasi?", "id": "Jenis Masalah Optimisasi Jaringan." },
  { "en": "Apa Itu Analisis Jalur Kritis (CPM)?", "id": "Algoritma Penjadwalan Proyek." },
  { "en": "Apa Itu Program Evaluation and Review Technique (PERT)?", "id": "Metode Manajemen Proyek Memperhitungkan Ketidakpastian." },
  { "en": "Apa Itu Rantai Markov?", "id": "Model Stokastik Dengan Sifat Tanpa Memori." },
  { "en": "Apa Itu Matriks Transisi?", "id": "Menentukan Probabilitas Pindah Antar Keadaan." },
  { "en": "Apa Itu Simulasi Monte Carlo?", "id": "Metode Komputasi Menggunakan Sampling Acak." },
  { "en": "Apa Itu Rekayasa Keuangan?", "id": "Aplikasi Teknik, Matematika Dalam Keuangan." },
  { "en": "Apa Itu Model Black-Scholes?", "id": "Model Matematis Untuk Menentukan Harga Opsi." },
  { "en": "Apa Itu Volatilitas?", "id": "Ukuran Statistik Variasi Harga Aset." },
  { "en": "Apa Itu Rekayasa Sistem?", "id": "Pendekatan Interdisipliner Merancang Sistem Kompleks." },
  { "en": "Apa Itu Siklus Hidup Sistem?", "id": "Tahapan Pengembangan Dari Konsep Hingga Pensiun." },
  { "en": "Apa Itu Analisis Persyaratan?", "id": "Proses Mendefinisikan Kebutuhan Pengguna, Sistem." },
  { "en": "Apa Itu Verifikasi?", "id": "Memastikan Sistem Dibangun Dengan Benar (Sesuai Desain)." },
  { "en": "Apa Itu Validasi?", "id": "Memastikan Sistem Yang Benar Dibangun (Sesuai Kebutuhan)." },
  { "en": "Apa Itu Arsitektur Sistem?", "id": "Struktur Konseptual Tingkat Tinggi Suatu Sistem." },
  { "en": "Apa Itu Analisis Trade-off?", "id": "Proses Mengevaluasi Berbagai Pilihan Desain." },
  { "en": "Apa Itu Rekayasa Keandalan?", "id": "Memastikan Sistem Berfungsi Sesuai Harapan." },
  { "en": "Apa Itu Analisis Pohon Kegagalan (FTA)?", "id": "Metode Deduktif Top-Down Untuk Analisis Kegagalan." },
  { "en": "Apa Itu Failure Modes and Effects Analysis (FMEA)?", "id": "Metode Induktif Bottom-Up Analisis Kegagalan." },
  { "en": "Apa Itu Rekayasa Faktor Manusia (Ergonomi)?", "id": "Mendesain Sistem Sesuai Kemampuan, Batasan Manusia." },
  { "en": "Apa Itu Usability?", "id": "Kemudahan Suatu Sistem Untuk Digunakan." },
  { "en": "Apa Itu Beban Kerja Kognitif?", "id": "Jumlah Upaya Mental Yang Dibutuhkan." },
  { "en": "Apa Itu Rekayasa Industri?", "id": "Mengoptimalkan Proses Dan Sistem Kompleks." },
  { "en": "Apa Itu Lean Manufacturing?", "id": "Filosofi Produksi Berfokus Pada Penghilangan Pemborosan." },
  { "en": "Apa Itu Six Sigma?", "id": "Metodologi Peningkatan Kualitas Berbasis Data." },
  { "en": "Apa Itu DMAIC Dalam Six Sigma?", "id": "Define, Measure, Analyze, Improve, Control." },
  { "en": "Apa Itu Kontrol Proses Statistik (SPC)?", "id": "Menggunakan Metode Statistik Memantau Proses." },
  { "en": "Apa Itu Peta Kontrol (Control Chart)?", "id": "Grafik Memantau Variasi Proses Seiring Waktu." },
  { "en": "Apa Itu Manajemen Rantai Pasokan?", "id": "Manajemen Aliran Barang, Jasa, Informasi." },
  { "en": "Apa Itu Efek Bullwhip?", "id": "Variabilitas Permintaan Meningkat Di Hulu Rantai Pasok." },
  { "en": "Apa Itu Teknik Sipil?", "id": "Merancang, Membangun, Memelihara Infrastruktur Publik." },
  { "en": "Apa Itu Analisis Struktural?", "id": "Menentukan Efek Beban Pada Struktur Fisik." },
  { "en": "Apa Itu Tegangan (Stress)?", "id": "Gaya Internal Per Satuan Luas." },
  { "en": "Apa Itu Regangan (Strain)?", "id": "Ukuran Deformasi Material." },
  { "en": "Apa Itu Beton Bertulang?", "id": "Beton Diperkuat Dengan Batang Baja (Rebar)." },
  { "en": "Apa Itu Rekayasa Geoteknik?", "id": "Mempelajari Perilaku Material Bumi (Tanah, Batuan)." },
  { "en": "Apa Itu Mekanika Tanah?", "id": "Cabang Teknik Geoteknik." },
  { "en": "Apa Itu Rekayasa Transportasi?", "id": "Perencanaan, Desain, Operasi Sistem Transportasi." },
  { "en": "Apa Itu Rekayasa Sumber Daya Air?", "id": "Manajemen Kuantitas Dan Kualitas Air." },
  { "en": "Apa Itu Hidrologi?", "id": "Ilmu Pergerakan, Distribusi, Kualitas Air." },
  { "en": "Apa Itu Rekayasa Lingkungan?", "id": "Mengatasi Masalah Lingkungan Menggunakan Prinsip Teknik." },
  { "en": "Apa Itu Teknik Mesin?", "id": "Disiplin Teknik Untuk Sistem Mekanis." },
  { "en": "Apa Itu Desain Berbantuan Komputer (CAD)?", "id": "Penggunaan Komputer Untuk Membantu Desain." },
  { "en": "Apa Itu Analisis Elemen Hingga (FEA)?", "id": "Metode Numerik Untuk Memecahkan Masalah Teknik." },
  { "en": "Apa Itu Manufaktur Berbantuan Komputer (CAM)?", "id": "Penggunaan Komputer Untuk Mengontrol Mesin Manufaktur." },
  { "en": "Apa Itu Mesin CNC (Computer Numerical Control)?", "id": "Mesin Perkakas Yang Dikontrol Secara Otomatis." },
  { "en": "Apa Itu Perpindahan Panas?", "id": "Transfer Energi Termal Antar Sistem Fisik." },
  { "en": "Apa Itu Konduksi?", "id": "Transfer Panas Melalui Kontak Langsung." },
  { "en": "Apa Itu Konveksi?", "id": "Transfer Panas Melalui Gerakan Fluida." },
  { "en": "Apa Itu Radiasi?", "id": "Transfer Panas Melalui Gelombang Elektromagnetik." },
  { "en": "Apa Itu Mesin Pembakaran Dalam?", "id": "Mesin Panas Pembakaran Terjadi Di Dalamnya." },
  { "en": "Apa Itu Teknik Kimia?", "id": "Mengubah Bahan Mentah Menjadi Produk Bernilai." },
  { "en": "Apa Itu Kinetika Reaksi?", "id": "Studi Laju Reaksi Kimia." },
  { "en": "Apa Itu Katalis?", "id": "Zat Yang Mempercepat Reaksi Kimia." },
  { "en": "Apa Itu Perpindahan Massa?", "id": "Pergerakan Massa Dari Satu Lokasi Ke Lokasi Lain." },
  { "en": "Apa Itu Distilasi?", "id": "Proses Pemisahan Berdasarkan Perbedaan Titik Didih." },
  { "en": "Apa Itu Teknik Dirgantara?", "id": "Pengembangan Pesawat Terbang Dan Pesawat Luar Angkasa." },
  { "en": "Apa Itu Aerodinamika?", "id": "Studi Gerakan Udara Dan Interaksinya." },
  { "en": "Apa Itu Gaya Angkat (Lift)?", "id": "Gaya Aerodinamis Yang Menentang Gravitasi." },
  { "en": "Apa Itu Gaya Hambat (Drag)?", "id": "Gaya Aerodinamis Yang Menentang Gerakan." },
  { "en": "Apa Itu Bilangan Mach?", "id": "Rasio Kecepatan Objek Terhadap Kecepatan Suara." },
  { "en": "Apa Itu Propulsi?", "id": "Gaya Dorong Untuk Menggerakkan Objek." },
  { "en": "Apa Itu Mekanika Orbital?", "id": "Studi Gerakan Benda Di Ruang Angkasa." },
  { "en": "Apa Itu Kecepatan Lepas (Escape Velocity)?", "id": "Kecepatan Minimum Untuk Lepas Dari Gravitasi." },
  { "en": "Apa Itu Teknik Biomedis?", "id": "Aplikasi Prinsip Teknik Dalam Kedokteran." },
  { "en": "Apa Itu Pencitraan Medis?", "id": "Teknik Visualisasi Interior Tubuh Manusia." },
  { "en": "Apa Itu Biomekanika?", "id": "Studi Mekanika Sistem Biologis." },
  { "en": "Apa Itu Biomaterial?", "id": "Material Yang Dirancang Berinteraksi Dengan Tubuh." },
  { "en": "Apa Itu Teknik Jaringan?", "id": "Membangun Jaringan Biologis Fungsional." },
  { "en": "Apa Itu Rekayasa Pertanian?", "id": "Penerapan Teknik Dalam Bidang Pertanian." },
  { "en": "Apa Itu Irigasi Presisi?", "id": "Pemberian Air Tepat Sesuai Kebutuhan Tanaman." },
  { "en": "Apa Itu Teknik Metalurgi?", "id": "Studi Perilaku Fisik, Kimia Logam." },
  { "en": "Apa Itu Paduan (Alloy)?", "id": "Campuran Logam Dengan Elemen Lain." },
  { "en": "Apa Itu Perlakuan Panas (Heat Treatment)?", "id": "Mengubah Sifat Logam Melalui Pemanasan, Pendinginan." },
  { "en": "Apa Itu Annealing?", "id": "Perlakuan Panas Untuk Melunakkan Logam." },
  { "en": "Apa Itu Quenching?", "id": "Pendinginan Cepat Untuk Mengeraskan Logam." },
  { "en": "Apa Itu Tempering?", "id": "Mengurangi Kerapuhan Logam Yang Dikeraskan." },
  { "en": "Apa Itu Teknik Keramik?", "id": "Ilmu Pembuatan Produk Dari Bahan Anorganik." },
  { "en": "Apa Itu Teknik Polimer?", "id": "Berurusan Dengan Desain, Manufaktur Polimer." },
  { "en": "Apa Itu Material Komposit?", "id": "Material Terdiri Dari Dua Atau Lebih Bahan." },
  { "en": "Apa Itu Serat Karbon?", "id": "Material Sangat Kuat, Ringan Untuk Komposit." },
  { "en": "Apa Itu Nanoteknologi?", "id": "Manipulasi Materi Pada Skala Nanometer." },
  { "en": "Apa Itu Bahan Nano?", "id": "Material Dengan Setidaknya Satu Dimensi Nanometer." },
  { "en": "Apa Itu Akustik?", "id": "Ilmu Tentang Suara, Getaran." },
  { "en": "Apa Itu Desibel (dB)?", "id": "Satuan Logaritmik Mengukur Intensitas Suara." },
  { "en": "Apa Itu Frekuensi Resonansi?", "id": "Frekuensi Alami Getaran Suatu Objek." },
  { "en": "Apa Itu Peredaman Akustik?", "id": "Proses Mengurangi Energi Suara." },
  { "en": "Apa Itu Pembatalan Derau Aktif?", "id": "Menggunakan Anti-suara Untuk Membatalkan Suara." },
  { "en": "Apa Itu Sonar?", "id": "Menggunakan Perambatan Suara Untuk Navigasi." },
  { "en": "Apa Itu Ultrasonografi?", "id": "Teknik Pencitraan Menggunakan Gelombang Ultrasonik." },
  { "en": "Apa Itu Teori Grafik?", "id": "Studi Matematika Tentang Simpul Dan Sisi." },
  { "en": "Apa Itu Simpul Dalam Teori Grafik?", "id": "Titik Atau Entitas Fundamental Dalam Grafik." },
  { "en": "Apa Itu Sisi Dalam Teori Grafik?", "id": "Koneksi Yang Menghubungkan Dua Simpul." },
  { "en": "Apa Itu Grafik Berarah (Digraph)?", "id": "Grafik Di Mana Sisi Memiliki Arah." },
  { "en": "Apa Itu Grafik Berbobot?", "id": "Grafik Di Mana Sisi Memiliki Nilai." },
  { "en": "Apa Itu Jalur Dalam Grafik?", "id": "Urutan Sisi Yang Menghubungkan Simpul." },
  { "en": "Apa Itu Siklus Dalam Grafik?", "id": "Jalur Yang Dimulai Dan Berakhir Di Simpul Sama." },
  { "en": "Apa Itu Pohon (Tree)?", "id": "Grafik Terhubung Yang Tidak Mengandung Siklus." },
  { "en": "Apa Itu Algoritma Dijkstra?", "id": "Menemukan Jalur Terpendek Antara Simpul." },
  { "en": "Apa Itu Algoritma Prim?", "id": "Menemukan Pohon Rentang Minimum Untuk Grafik." },
  { "en": "Apa Itu Pohon Rentang Minimum (MST)?", "id": "Pohon Mencakup Semua Simpul Bobot Minimum." },
  { "en": "Apa Itu Teori Antrian?", "id": "Studi Matematis Tentang Garis Tunggu." },
  { "en": "Apa Itu Proses Poisson?", "id": "Model Probabilistik Untuk Kedatangan Acak." },
  { "en": "Apa Itu Notasi Kendall?", "id": "Notasi Standar Untuk Mendeskripsikan Model Antrian." },
  { "en": "Apa Itu Hukum Little?", "id": "Menghubungkan Jumlah, Laju, Waktu Dalam Sistem." },
  { "en": "Apa Itu Teori Permainan?", "id": "Studi Pengambilan Keputusan Strategis." },
  { "en": "Apa Itu Ekuilibrium Nash?", "id": "Kondisi Stabil Dalam Permainan Strategis." },
  { "en": "Apa Itu Permainan Zero-Sum?", "id": "Keuntungan Satu Pihak Adalah Kerugian Pihak Lain." },
  { "en": "Apa Itu Riset Operasi?", "id": "Penerapan Metode Analitis Untuk Pengambilan Keputusan." },
  { "en": "Apa Itu Pemrograman Linear?", "id": "Metode Optimisasi Untuk Masalah Linear." },
  { "en": "Apa Itu Metode Simpleks?", "id": "Algoritma Untuk Menyelesaikan Masalah Pemrograman Linear." },
  { "en": "Apa Itu Simulasi Monte Carlo?", "id": "Teknik Komputasi Menggunakan Angka Acak." },
  { "en": "Apa Itu Rantai Markov?", "id": "Model Stokastik Dengan Sifat Tanpa Memori." },
  { "en": "Apa Itu Rekayasa Sistem?", "id": "Pendekatan Merancang Dan Mengelola Sistem Kompleks." },
  { "en": "Apa Itu Siklus Hidup Sistem?", "id": "Tahapan Pengembangan Dari Awal Hingga Akhir." },
  { "en": "Apa Itu Analisis Persyaratan?", "id": "Mendefinisikan Kebutuhan Pengguna Dan Sistem." },
  { "en": "Apa Itu Verifikasi Dan Validasi (V&V)?", "id": "Memastikan Sistem Dibangun Benar, Sesuai Kebutuhan." },
  { "en": "Apa Itu Arsitektur Sistem?", "id": "Struktur Konseptual Tingkat Tinggi Suatu Sistem." },
  { "en": "Apa Itu Rekayasa Keandalan?", "id": "Memastikan Sistem Berfungsi Sesuai Harapan." },
  { "en": "Apa Itu Analisis Pohon Kegagalan (FTA)?", "id": "Analisis Top-Down Untuk Identifikasi Kegagalan." },
  { "en": "Apa Itu Analisis Mode Kegagalan (FMEA)?", "id": "Analisis Bottom-Up Untuk Identifikasi Mode Kegagalan." },
  { "en": "Apa Itu Ergonomi (Faktor Manusia)?", "id": "Mendesain Sistem Sesuai Kemampuan Manusia." },
  { "en": "Apa Itu Usability?", "id": "Kemudahan Suatu Sistem Untuk Digunakan." },
  { "en": "Apa Itu Rekayasa Industri?", "id": "Optimalisasi Proses Dan Sistem Kompleks." },
  { "en": "Apa Itu Manufaktur Ramping (Lean Manufacturing)?", "id": "Filosofi Produksi Berfokus Pada Minimalisasi Pemborosan." },
  { "en": "Apa Itu Six Sigma?", "id": "Metodologi Peningkatan Kualitas Berbasis Data." },
  { "en": "Apa Itu Kontrol Proses Statistik (SPC)?", "id": "Menggunakan Statistik Untuk Memantau Proses." },
  { "en": "Apa Itu Peta Kontrol (Control Chart)?", "id": "Grafik Memantau Variasi Proses Seiring Waktu." },
  { "en": "Apa Itu Manajemen Rantai Pasokan?", "id": "Manajemen Aliran Barang, Jasa, Dan Informasi." },
  { "en": "Apa Itu Efek Bullwhip?", "id": "Peningkatan Variabilitas Permintaan Di Rantai Pasokan." },
  { "en": "Apa Itu Teknik Sipil?", "id": "Merancang, Membangun, Dan Memelihara Infrastruktur." },
  { "en": "Apa Itu Analisis Struktural?", "id": "Menentukan Efek Beban Pada Struktur Fisik." },
  { "en": "Apa Itu Beton Bertulang?", "id": "Beton Diperkuat Dengan Batang Baja." },
  { "en": "Apa Itu Rekayasa Geoteknik?", "id": "Mempelajari Perilaku Material Bumi." },
  { "en": "Apa Itu Rekayasa Transportasi?", "id": "Perencanaan Dan Desain Sistem Transportasi." },
  { "en": "Apa Itu Rekayasa Lingkungan?", "id": "Mengatasi Masalah Lingkungan Dengan Prinsip Teknik." },
  { "en": "Apa Itu Teknik Mesin?", "id": "Disiplin Teknik Untuk Sistem Mekanis." },
  { "en": "Apa Itu Desain Berbantuan Komputer (CAD)?", "id": "Penggunaan Komputer Untuk Membantu Desain." },
  { "en": "Apa Itu Analisis Elemen Hingga (FEA)?", "id": "Metode Numerik Untuk Memecahkan Masalah Teknik." },
  { "en": "Apa Itu Manufaktur Berbantuan Komputer (CAM)?", "id": "Penggunaan Komputer Untuk Mengontrol Mesin Manufaktur." },
  { "en": "Apa Itu Perpindahan Panas?", "id": "Transfer Energi Termal Antar Sistem." },
  { "en": "Apa Itu Mesin Pembakaran Dalam?", "id": "Mesin Panas Pembakaran Terjadi Di Dalamnya." },
  { "en": "Apa Itu Teknik Kimia?", "id": "Mengubah Bahan Mentah Menjadi Produk." },
  { "en": "Apa Itu Kinetika Reaksi?", "id": "Studi Laju Reaksi Kimia." },
  { "en": "Apa Itu Katalis?", "id": "Zat Yang Mempercepat Reaksi Kimia." },
  { "en": "Apa Itu Perpindahan Massa?", "id": "Pergerakan Massa Dari Satu Lokasi." },
  { "en": "Apa Itu Distilasi?", "id": "Proses Pemisahan Berdasarkan Perbedaan Titik Didih." },
  { "en": "Apa Itu Teknik Dirgantara?", "id": "Pengembangan Pesawat Terbang Dan Pesawat Luar Angkasa." },
  { "en": "Apa Itu Aerodinamika?", "id": "Studi Gerakan Udara Dan Interaksinya." },
  { "en": "Apa Itu Gaya Angkat (Lift)?", "id": "Gaya Aerodinamis Yang Menentang Gravitasi." },
  { "en": "Apa Itu Gaya Hambat (Drag)?", "id": "Gaya Aerodinamis Yang Menentang Gerakan." },
  { "en": "Apa Itu Mekanika Orbital?", "id": "Studi Gerakan Benda Di Ruang Angkasa." },
  { "en": "Apa Itu Teknik Biomedis?", "id": "Aplikasi Prinsip Teknik Dalam Kedokteran." },
  { "en": "Apa Itu Pencitraan Medis?", "id": "Teknik Visualisasi Interior Tubuh Manusia." },
  { "en": "Apa Itu Biomaterial?", "id": "Material Dirancang Berinteraksi Dengan Sistem Biologis." },
  { "en": "Apa Itu Teknik Metalurgi?", "id": "Studi Perilaku Fisik, Kimia Logam." },
  { "en": "Apa Itu Paduan (Alloy)?", "id": "Campuran Logam Dengan Elemen Lain." },
  { "en": "Apa Itu Perlakuan Panas (Heat Treatment)?", "id": "Mengubah Sifat Logam Melalui Pemanasan." },
  { "en": "Apa Itu Material Komposit?", "id": "Material Terdiri Dari Dua Atau Lebih Bahan." },
  { "en": "Apa Itu Nanoteknologi?", "id": "Manipulasi Materi Pada Skala Nanometer." },
  { "en": "Apa Itu Sel Bahan Bakar (Fuel Cell)?", "id": "Mengubah Energi Kimia Menjadi Listrik." },
  { "en": "Apa Itu Akustik?", "id": "Ilmu Tentang Suara Dan Getaran." },
  { "en": "Apa Itu Desibel (dB)?", "id": "Satuan Logaritmik Mengukur Intensitas Suara." },
  { "en": "Apa Itu Pembatalan Derau Aktif?", "id": "Mengurangi Derau Dengan Menghasilkan Anti-derau." },
  { "en": "Apa Itu Ultrasonografi?", "id": "Teknik Pencitraan Menggunakan Gelombang Ultrasonik." },
  { "en": "Apa Itu Etika Rekayasa?", "id": "Prinsip Moral Diterapkan Dalam Praktik Rekayasa." },
  { "en": "Apa Itu Kekayaan Intelektual?", "id": "Kreasi Pikiran Seperti Penemuan, Karya Sastra." },
  { "en": "Apa Itu Paten?", "id": "Hak Eksklusif Diberikan Untuk Suatu Penemuan." },
  { "en": "Apa Itu Hak Cipta?", "id": "Hak Hukum Melindungi Karya Orisinal." },
  { "en": "Apa Itu Merek Dagang?", "id": "Tanda Pengenal Barang Atau Jasa." },
  { "en": "Apa Itu Rahasia Dagang?", "id": "Informasi Rahasia Yang Memberi Keunggulan." },
  { "en": "Apa Itu Lisensi?", "id": "Izin Menggunakan Kekayaan Intelektual." },
  { "en": "Apa Itu Standar Teknik?", "id": "Spesifikasi Teknis Yang Disetujui Bersama." },
  { "en": "Siapa Itu IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Organisasi Profesional Teknik Terbesar Dunia." },
  { "en": "Siapa Itu ISO (International Organization for Standardization)?", "id": "Organisasi Internasional Pengembang Standar." },
  { "en": "Apa Itu American National Standards Institute (ANSI)?", "id": "Organisasi Standar Swasta Amerika Serikat." },
  { "en": "Apa Itu Kewirausahaan Teknologi?", "id": "Mendirikan Bisnis Berbasis Inovasi Teknologi." },
  { "en": "Apa Itu Model Bisnis?", "id": "Rencana Perusahaan Untuk Menghasilkan Keuntungan." },
  { "en": "Apa Itu Riset Pasar?", "id": "Mengumpulkan Informasi Tentang Pasar Target." },
  { "en": "Apa Itu Analisis Kompetitif?", "id": "Mengevaluasi Kekuatan Dan Kelemahan Pesaing." },
  { "en": "Apa Itu Proposisi Nilai?", "id": "Manfaat Yang Ditawarkan Produk Kepada Pelanggan." },
  { "en": "Apa Itu Minimum Viable Product (MVP)?", "id": "Versi Awal Produk Dengan Fitur Minimal." },
  { "en": "Apa Itu Pendanaan Awal (Seed Funding)?", "id": "Modal Awal Untuk Mendirikan Perusahaan." },
  { "en": "Apa Itu Modal Ventura (Venture Capital)?", "id": "Pendanaan Untuk Perusahaan Rintisan Berpotensi." },
  { "en": "Apa Itu Angel Investor?", "id": "Individu Kaya Yang Berinvestasi." },
  { "en": "Apa Itu Crowdfunding?", "id": "Mengumpulkan Dana Dari Sejumlah Besar Orang." },
  { "en": "Apa Itu Pemasaran?", "id": "Aktivitas Mempromosikan Produk Atau Jasa." },
  { "en": "Apa Itu Branding?", "id": "Menciptakan Identitas Unik Untuk Perusahaan." },
  { "en": "Apa Itu Strategi Masuk Pasar?", "id": "Rencana Peluncuran Produk Baru." },
  { "en": "Apa Itu Penskalaan (Scaling) Bisnis?", "id": "Mengembangkan Bisnis Untuk Menangani Pertumbuhan." },
  { "en": "Apa Itu Validasi Model?", "id": "Proses Mengevaluasi Kinerja Model Terlatih." },
  { "en": "Apa Itu Himpunan Data Pelatihan?", "id": "Data Digunakan Untuk Melatih Model Pembelajaran Mesin." },
  { "en": "Apa Itu Himpunan Data Validasi?", "id": "Data Untuk Menyesuaikan Hyperparameter Model." },
  { "en": "Apa Itu Himpunan Data Pengujian?", "id": "Data Untuk Mengevaluasi Kinerja Akhir Model." },
  { "en": "Apa Itu Matriks Konfusi?", "id": "Tabel Mengevaluasi Kinerja Model Klasifikasi." },
  { "en": "Apa Itu Akurasi?", "id": "Rasio Prediksi Benar Terhadap Total Prediksi." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Rasio Positif Benar Terhadap Semua Prediksi Positif." },
  { "en": "Apa Itu Perolehan (Recall/Sensitivity)?", "id": "Rasio Positif Benar Terhadap Semua Kasus Aktual." },
  { "en": "Apa Itu Skor F1?", "id": "Rata-rata Harmonik Dari Presisi Dan Perolehan." },
  { "en": "Apa Itu Kurva ROC (Receiver Operating Characteristic)?", "id": "Grafik Kinerja Klasifikasi Biner." },
  { "en": "Apa Itu Area Di Bawah Kurva (AUC)?", "id": "Ukuran Kinerja Model Klasifikasi." },
  { "en": "Apa Itu Bias Dalam AI?", "id": "Error Sistematis Dalam Prediksi Model." },
  { "en": "Apa Itu Varians Dalam AI?", "id": "Sensitivitas Model Terhadap Variasi Data Latih." },
  { "en": "Apa Itu Dilema Bias-Varians?", "id": "Trade-off Antara Kesalahan Bias Dan Varians." },
  { "en": "Apa Itu Regularisasi?", "id": "Teknik Untuk Mencegah Overfitting." },
  { "en": "Apa Itu Regularisasi L1 (Lasso)?", "id": "Menambahkan Pinalti Absolut Ke Fungsi Kerugian." },
  { "en": "Apa Itu Regularisasi L2 (Ridge)?", "id": "Menambahkan Pinalti Kuadrat Ke Fungsi Kerugian." },
  { "en": "Apa Itu Pengoptimalan Hyperparameter?", "id": "Proses Menemukan Kombinasi Hyperparameter Terbaik." },
  { "en": "Apa Itu Pencarian Grid (Grid Search)?", "id": "Metode Pencarian Hyperparameter Secara Menyeluruh." },
  { "en": "Apa Itu Pencarian Acak (Random Search)?", "id": "Mencari Hyperparameter Secara Acak Dalam Ruang." },
  { "en": "Apa Itu Sistem Pakar (Expert System)?", "id": "Sistem AI Meniru Kemampuan Pengambilan Keputusan Ahli." },
  { "en": "Apa Itu Basis Pengetahuan?", "id": "Penyimpanan Fakta Dan Aturan Dalam Sistem Pakar." },
  { "en": "Apa Itu Mesin Inferensi?", "id": "Menerapkan Aturan Logis Ke Basis Pengetahuan." },
  { "en": "Apa Itu Logika Fuzzy?", "id": "Logika Berurusan Dengan Kebenaran Parsial." },
  { "en": "Apa Itu Algoritma Genetika?", "id": "Algoritma Optimisasi Terinspirasi Evolusi Biologis." },
  { "en": "Apa Itu Seleksi Alam Buatan?", "id": "Proses Seleksi Dalam Algoritma Genetika." },
  { "en": "Apa Itu Crossover (Pindah Silang)?", "id": "Menggabungkan Solusi Induk Menciptakan Keturunan." },
  { "en": "Apa Itu Mutasi?", "id": "Perubahan Acak Dalam Solusi Calon." },
  { "en": "Apa Itu Swarm Intelligence?", "id": "Perilaku Kolektif Sistem Terdesentralisasi." },
  { "en": "Apa Itu Particle Swarm Optimization (PSO)?", "id": "Algoritma Optimisasi Terinspirasi Perilaku Kawanan." },
  { "en": "Apa Itu Ant Colony Optimization (ACO)?", "id": "Algoritma Optimisasi Terinspirasi Semut." },
  { "en": "Apa Itu Representasi Pengetahuan?", "id": "Cara Informasi Disimpan Dan Dimanipulasi Oleh AI." },
  { "en": "Apa Itu Ontologi?", "id": "Representasi Formal Konsep Dan Hubungannya." },
  { "en": "Apa Itu Penalaran (Reasoning) Dalam AI?", "id": "Proses Menarik Kesimpulan Logis." },
  { "en": "Apa Itu AI Yang Dapat Dijelaskan (XAI)?", "id": "AI Yang Keputusannya Dapat Dipahami Manusia." },
  { "en": "Apa Itu Etika AI?", "id": "Prinsip Moral Memandu Desain, Penggunaan AI." },
  { "en": "Apa Itu Kotak Hitam (Black Box) AI?", "id": "Model AI Sulit Diinterpretasikan." },
  { "en": "Apa Itu Pembelajaran Transfer (Transfer Learning)?", "id": "Menggunakan Pengetahuan Model Untuk Tugas Lain." },
  { "en": "Apa Itu Fine-Tuning?", "id": "Menyesuaikan Model Pra-terlatih Dengan Data Baru." },
  { "en": "Apa Itu Generative Adversarial Network (GAN)?", "id": "Jaringan Saraf Untuk Menghasilkan Data Sintetis." },
  { "en": "Apa Itu Autoencoder?", "id": "Jaringan Saraf Untuk Kompresi Data." },
  { "en": "Apa Itu Analisis Sentimen?", "id": "Menentukan Nada Emosional Dalam Teks." },
  { "en": "Apa Itu Pengenalan Entitas Bernama (NER)?", "id": "Mengidentifikasi Entitas (Nama, Lokasi) Dalam Teks." },
  { "en": "Apa Itu Peringkasan Teks?", "id": "Membuat Ringkasan Singkat Dari Dokumen." },
  { "en": "Apa Itu Terjemahan Mesin?", "id": "Menerjemahkan Teks Secara Otomatis." },
  { "en": "Apa Itu Pengenalan Ucapan?", "id": "Mengubah Bahasa Lisan Menjadi Teks." },
  { "en": "Apa Itu Sintesis Ucapan?", "id": "Menghasilkan Ucapan Manusia Buatan." },
  { "en": "Apa Itu Deteksi Objek?", "id": "Menemukan Dan Mengklasifikasikan Objek Dalam Gambar." },
  { "en": "Apa Itu Segmentasi Gambar?", "id": "Membagi Gambar Menjadi Segmen Bermakna." },
  { "en": "Apa Itu Pengenalan Karakter Optik (OCR)?", "id": "Mengubah Gambar Teks Menjadi Teks Digital." },
  { "en": "Apa Itu Agen Cerdas?", "id": "Entitas Otonom Berinteraksi Dengan Lingkungan." },
  { "en": "Apa Itu Sistem Multi-Agen?", "id": "Sekelompok Agen Bekerja Sama Atau Bersaing." },
  { "en": "Apa Itu Ruang Kerja (Workspace) Robot?", "id": "Volume Ruang Yang Dapat Dicapai End-Effector." },
  { "en": "Apa Itu Singularitas Robot?", "id": "Konfigurasi Di Mana Robot Kehilangan Derajat Kebebasan." },
  { "en": "Apa Itu Dinamika Robot?", "id": "Studi Gerakan Robot Mempertimbangkan Gaya." },
  { "en": "Apa Itu Kontrol Kepatuhan (Compliance Control)?", "id": "Mengontrol Interaksi Robot Dengan Lingkungan." },
  { "en": "Apa Itu Robotika Kolaboratif (Cobot)?", "id": "Robot Dirancang Bekerja Aman Di Sekitar Manusia." },
  { "en": "Apa Itu Teleoperasi?", "id": "Mengendalikan Robot Dari Jarak Jauh." },
  { "en": "Apa Itu Haptics?", "id": "Teknologi Umpan Balik Sentuhan." },
  { "en": "Apa Itu Komputasi Tepi Cerdas (AI at the Edge)?", "id": "Menjalankan Model AI Langsung Di Perangkat." },
  { "en": "Apa Itu Pembelajaran Federasi?", "id": "Melatih Model Terdistribusi Tanpa Memindahkan Data." },
  { "en": "Apa Itu Privasi Diferensial?", "id": "Teknik Menambahkan Derau Untuk Melindungi Privasi." },
  { "en": "Apa Itu Enkripsi Homomorfik?", "id": "Melakukan Komputasi Langsung Pada Data Terenkripsi." },
  { "en": "Apa Itu Bukti Tanpa Pengetahuan (Zero-Knowledge Proof)?", "id": "Membuktikan Pengetahuan Tanpa Mengungkapkannya." },
  { "en": "Apa Itu Serangan Adversarial?", "id": "Input Dirancang Menipu Model Pembelajaran Mesin." },
  { "en": "Apa Itu Pertahanan Adversarial?", "id": "Teknik Membuat Model Tahan Serangan Adversarial." },
  { "en": "Apa Itu Arsitektur Perangkat Lunak?", "id": "Struktur Tingkat Tinggi Suatu Sistem." },
  { "en": "Apa Itu Pola Desain (Design Pattern)?", "id": "Solusi Umum, Dapat Digunakan Kembali." },
  { "en": "Apa Itu Pola Singleton?", "id": "Memastikan Suatu Kelas Hanya Memiliki Satu Instans." },
  { "en": "Apa Itu Pola Factory?", "id": "Membuat Objek Tanpa Mengekspos Logika Penciptaan." },
  { "en": "Apa Itu Pola Observer?", "id": "Mendefinisikan Ketergantungan Satu-ke-Banyak." },
  { "en": "Apa Itu Pola Strategi (Strategy Pattern)?", "id": "Memungkinkan Algoritma Dipilih Saat Runtime." },
  { "en": "Apa Itu Prinsip SOLID?", "id": "Lima Prinsip Desain Berorientasi Objek." },
  { "en": "Apa Itu Prinsip Tanggung Jawab Tunggal?", "id": "Setiap Kelas Harus Punya Satu Tanggung Jawab." },
  { "en": "Apa Itu Prinsip Terbuka/Tertutup?", "id": "Terbuka Untuk Ekstensi, Tertutup Untuk Modifikasi." },
  { "en": "Apa Itu Prinsip Substitusi Liskov?", "id": "Objek Kelas Turunan Harus Dapat Menggantikan." },
  { "en": "Apa Itu Inversi Ketergantungan (Dependency Inversion)?", "id": "Bergantung Pada Abstraksi, Bukan Implementasi." },
  { "en": "Apa Itu Injeksi Ketergantungan (Dependency Injection)?", "id": "Memberikan Ketergantungan Dari Sumber Eksternal." },
  { "en": "Apa Itu Pemrograman Berorientasi Aspek (AOP)?", "id": "Memisahkan Masalah Lintas Sektor (Logging)." },
  { "en": "Apa Itu Kode Terkelola (Managed Code)?", "id": "Kode Yang Dijalankan Oleh Runtime (JVM, CLR)." },
  { "en": "Apa Itu Pengumpulan Sampah (Garbage Collection)?", "id": "Manajemen Memori Otomatis." },
  { "en": "Apa Itu утечка памяти (Memory Leak)?", "id": "Memori Yang Tidak Lagi Digunakan, Gagal Dilepaskan." },
  { "en": "Apa Itu Concurrency?", "id": "Kemampuan Menjalankan Beberapa Tugas Tumpang Tindih." },
  { "en": "Apa Itu Paralelisme?", "id": "Kemampuan Menjalankan Beberapa Tugas Secara Bersamaan." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Mekanisme Kunci Mencegah Akses Bersamaan." },
  { "en": "Apa Itu Semaphore?", "id": "Mengontrol Akses Ke Kumpulan Sumber Daya." },
  { "en": "Apa Itu Kondisi Balapan (Race Condition)?", "id": "Hasil Bergantung Urutan Eksekusi Yang Tak Terduga." },
  { "en": "Apa Itu Pemrograman Asinkron?", "id": "Eksekusi Non-blocking, Tidak Menunggu Tugas Selesai." },
  { "en": "Apa Itu Callback?", "id": "Fungsi Dijalankan Setelah Tugas Lain Selesai." },
  { "en": "Apa Itu Promise?", "id": "Objek Mewakili Penyelesaian Operasi Asinkron." },
  { "en": "Apa Itu Async/Await?", "id": "Sintaksis Menyederhanakan Pemrograman Asinkron." },
  { "en": "Apa Itu REST (Representational State Transfer)?", "id": "Gaya Arsitektur Untuk Membangun Layanan Web." },
  { "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Pertukaran Data Ringan, Mudah Dibaca." },
  { "en": "Apa Itu API (Application Programming Interface) Gateway?", "id": "Titik Masuk Tunggal Untuk Semua Permintaan Klien." },
  { "en": "Apa Itu Rate Limiting?", "id": "Membatasi Jumlah Permintaan Dari Klien." },
  { "en": "Apa Itu Otentikasi?", "id": "Memverifikasi Identitas Pengguna." },
  { "en": "Apa Itu Otorisasi?", "id": "Memberikan Izin Akses Sumber Daya." },
  { "en": "Apa Itu OAuth?", "id": "Standar Terbuka Untuk Otorisasi Terdelegasi." },
  { "en": "Apa Itu JSON Web Token (JWT)?", "id": "Standar Kompak Mengirim Informasi Antar Pihak." },
  { "en": "Apa Itu Metode Elemen Hingga (FEM)?", "id": "Metode Numerik Menganalisis Masalah Rekayasa." },
  { "en": "Apa Itu Diskritisasi Dalam FEM (Finite Element Method)?", "id": "Membagi Objek Menjadi Elemen-elemen Kecil." },
  { "en": "Apa Itu Jala (Mesh)?", "id": "Kumpulan Elemen Dan Simpul Dalam Analisis." },
  { "en": "Apa Itu Simpul (Node) Dalam FEM?", "id": "Titik Di Mana Variabel Dihitung." },
  { "en": "Apa Itu Fungsi Bentuk (Shape Function)?", "id": "Menginterpolasi Solusi Di Dalam Sebuah Elemen." },
  { "en": "Apa Itu Matriks Kekakuan (Stiffness Matrix)?", "id": "Menghubungkan Gaya Dan Perpindahan Dalam Elemen." },
  { "en": "Apa Itu Analisis Statis?", "id": "Menganalisis Struktur Di Bawah Beban Konstan." },
  { "en": "Apa Itu Analisis Dinamis?", "id": "Menganalisis Struktur Di Bawah Beban Berubah." },
  { "en": "Apa Itu Analisis Modal?", "id": "Menentukan Frekuensi Alami, Mode Getaran." },
  { "en": "Apa Itu Analisis Termal?", "id": "Menganalisis Distribusi Suhu Dalam Objek." },
  { "en": "Apa Itu Dinamika Fluida Komputasi (CFD)?", "id": "Mensimulasikan Aliran Fluida Menggunakan Komputer." },
  { "en": "Apa Itu Persamaan Navier-Stokes?", "id": "Persamaan Yang Mengatur Gerakan Fluida." },
  { "en": "Apa Itu Volume Kontrol (Control Volume)?", "id": "Elemen Diskret Dalam Analisis CFD." },
  { "en": "Apa Itu Model Turbulensi?", "id": "Aproksimasi Matematis Untuk Aliran Turbulen." },
  { "en": "Apa Itu Kondisi Batas?", "id": "Spesifikasi Kondisi Di Tepi Domain Simulasi." },
  { "en": "Apa Itu Pembuatan Jala (Mesh Generation)?", "id": "Proses Membuat Jala Untuk Simulasi." },
  { "en": "Apa Itu Jala Terstruktur?", "id": "Jala Dengan Topologi Grid Teratur." },
  { "en": "Apa Itu Jala Tak Terstruktur?", "id": "Jala Dengan Konektivitas Elemen Fleksibel." },
  { "en": "Apa Itu Penyempurnaan Jala (Mesh Refinement)?", "id": "Meningkatkan Resolusi Jala Di Area Tertentu." },
  { "en": "Apa Itu Pasca-pemrosesan (Post-processing)?", "id": "Menganalisis, Memvisualisasikan Hasil Simulasi." },
  { "en": "Apa Itu Optimisasi Topologi?", "id": "Menemukan Distribusi Material Optimal." },
  { "en": "Apa Itu Desain Berbantuan Komputer (CAD)?", "id": "Menggunakan Komputer Untuk Mendesain Produk." },
  { "en": "Apa Itu Manufaktur Berbantuan Komputer (CAM)?", "id": "Menggunakan Komputer Mengontrol Proses Manufaktur." },
  { "en": "Apa Itu Rekayasa Berbantuan Komputer (CAE)?", "id": "Menggunakan Perangkat Lunak Menganalisis Desain." },
  { "en": "Apa Itu Manajemen Siklus Hidup Produk (PLM)?", "id": "Mengelola Seluruh Siklus Hidup Produk." },
  { "en": "Apa Itu Desain Parametrik?", "id": "Desain Digerakkan Oleh Parameter Dan Hubungan." },
  { "en": "Apa Itu Pemodelan Solid?", "id": "Representasi Tiga Dimensi Objek Padat." },
  { "en": "Apa Itu Pemodelan Permukaan?", "id": "Representasi Kulit Objek Tiga Dimensi." },
  { "en": "Apa Itu Rakitan (Assembly) Dalam CAD?", "id": "Menggabungkan Beberapa Bagian Menjadi Satu." },
  { "en": "Apa Itu Gambar Teknik?", "id": "Representasi Grafis Rinci Suatu Desain." },
  { "en": "Apa Itu Toleransi Geometris (GD&T)?", "id": "Sistem Simbol Mendefinisikan Toleransi Manufaktur." },
  { "en": "Apa Itu Prototipe Cepat (Rapid Prototyping)?", "id": "Teknik Pembuatan Cepat Model Fisik." },
  { "en": "Apa Itu Manufaktur Aditif?", "id": "Nama Lain Untuk Pencetakan 3D." },
  { "en": "Apa Itu Manufaktur Subtraktif?", "id": "Menghilangkan Material Dari Blok Awal." },
  { "en": "Sebutkan Contoh Manufaktur Subtraktif?", "id": "Pemesinan CNC, Penggilingan, Pembubutan." },
  { "en": "Apa Itu Mesin Bubut (Lathe)?", "id": "Mesin Perkakas Memutar Benda Kerja." },
  { "en": "Apa Itu Mesin Giling (Milling Machine)?", "id": "Mesin Perkakas Menggunakan Pemotong Berputar." },
  { "en": "Apa Itu Kode-G?", "id": "Bahasa Pemrograman Untuk Mesin CNC." },
  { "en": "Apa Itu Toolpath?", "id": "Jalur Yang Diikuti Alat Pemotong." },
  { "en": "Apa Itu Jig?", "id": "Perangkat Memegang, Memandu Alat Kerja." },
  { "en": "Apa Itu Fixture?", "id": "Perangkat Memegang Benda Kerja Dengan Aman." },
  { "en": "Apa Itu Injection Molding?", "id": "Proses Manufaktur Mencetak Plastik Cair." },
  { "en": "Apa Itu Cetakan (Mold)?", "id": "Rongga Berbentuk Untuk Bahan Cair." },
  { "en": "Apa Itu Casting?", "id": "Proses Menuang Logam Cair Ke Cetakan." },
  { "en": "Apa Itu Forging?", "id": "Membentuk Logam Menggunakan Gaya Tekan." },
  { "en": "Apa Itu Stamping?", "id": "Membentuk Logam Lembaran Menggunakan Cetakan." },
  { "en": "Apa Itu Pengelasan (Welding)?", "id": "Proses Penyambungan Material." },
  { "en": "Apa Itu Pengelasan Busur Listrik?", "id": "Menggunakan Busur Listrik Untuk Melelehkan Logam." },
  { "en": "Apa Itu Pengelasan Gas?", "id": "Menggunakan Nyala Api Gas Untuk Melelehkan." },
  { "en": "Apa Itu Soldering?", "id": "Menyambung Menggunakan Logam Pengisi Titik Leleh Rendah." },
  { "en": "Apa Itu Brazing?", "id": "Mirip Soldering, Titik Leleh Lebih Tinggi." },
  { "en": "Apa Itu Perlakuan Panas (Heat Treatment)?", "id": "Mengubah Sifat Material Dengan Pemanasan." },
  { "en": "Apa Itu Annealing?", "id": "Melunakkan Material, Menghilangkan Stres Internal." },
  { "en": "Apa Itu Quenching?", "id": "Pendinginan Cepat Untuk Mengeraskan Material." },
  { "en": "Apa Itu Tempering?", "id": "Mengurangi Kerapuhan Material Yang Dikeraskan." },
  { "en": "Apa Itu Case Hardening?", "id": "Mengeraskan Hanya Permukaan Suatu Komponen." },
  { "en": "Apa Itu Metalurgi?", "id": "Ilmu Dan Teknologi Tentang Logam." },
  { "en": "Apa Itu Paduan (Alloy)?", "id": "Campuran Logam Dengan Elemen Lain." },
  { "en": "Apa Itu Baja?", "id": "Paduan Besi Dan Karbon." },
  { "en": "Apa Itu Baja Tahan Karat?", "id": "Baja Dengan Penambahan Kromium." },
  { "en": "Apa Itu Aluminium?", "id": "Logam Ringan, Tahan Korosi." },
  { "en": "Apa Itu Titanium?", "id": "Logam Kuat, Ringan, Tahan Korosi." },
  { "en": "Apa Itu Tembaga?", "id": "Logam Konduktivitas Listrik, Termal Tinggi." },
  { "en": "Apa Itu Kuningan (Brass)?", "id": "Paduan Tembaga Dan Seng." },
  { "en": "Apa Itu Perunggu (Bronze)?", "id": "Paduan Tembaga, Biasanya Dengan Timah." },
  { "en": "Apa Itu Polimer?", "id": "Molekul Rantai Panjang (Plastik)." },
  { "en": "Apa Itu Termoplastik?", "id": "Polimer Dapat Dilelehkan, Dibentuk Ulang." },
  { "en": "Apa Itu Termoset?", "id": "Polimer Mengeras Secara Permanen Saat Dipanaskan." },
  { "en": "Apa Itu Elastomer?", "id": "Polimer Dengan Sifat Elastis (Karet)." },
  { "en": "Apa Itu Material Komposit?", "id": "Material Terdiri Dari Dua Bahan Berbeda." },
  { "en": "Apa Itu Serat Karbon?", "id": "Material Penguat Sangat Kuat, Ringan." },
  { "en": "Apa Itu Fiberglass?", "id": "Plastik Diperkuat Dengan Serat Kaca." },
  { "en": "Apa Itu Keramik?", "id": "Material Anorganik, Non-logam." },
  { "en": "Apa Sifat Umum Keramik?", "id": "Keras, Rapuh, Tahan Panas, Isolator." },
  { "en": "Apa Itu Korosi?", "id": "Kerusakan Bertahap Material Akibat Reaksi." },
  { "en": "Apa Itu Karat?", "id": "Bentuk Korosi Besi Dan Baja." },
  { "en": "Bagaimana Cara Mencegah Korosi?", "id": "Pengecatan, Pelapisan, Proteksi Katodik." },
  { "en": "Apa Itu Proteksi Katodik?", "id": "Menjadikan Permukaan Logam Sebagai Katoda." },
  { "en": "Apa Itu Anoda Korban?", "id": "Logam Lebih Reaktif Dikorbankan Melindungi." },
  { "en": "Apa Itu Pelapisan Galvanis?", "id": "Melapisi Baja Dengan Lapisan Seng." },
  { "en": "Apa Itu Kelelahan Material (Fatigue)?", "id": "Kegagalan Di Bawah Beban Berulang." },
  { "en": "Apa Itu Batas Kelelahan (Fatigue Limit)?", "id": "Tegangan Di Bawahnya Kelelahan Tak Terjadi." },
  { "en": "Apa Itu Rayapan (Creep)?", "id": "Deformasi Lambat Di Bawah Tegangan Konstan." },
  { "en": "Kapan Rayapan (Creep) Menjadi Signifikan?", "id": "Pada Suhu Tinggi, Dekat Titik Leleh." },
  { "en": "Apa Itu Kekerasan (Hardness)?", "id": "Ketahanan Material Terhadap Deformasi Lokal." },
  { "en": "Apa Itu Uji Kekerasan Rockwell?", "id": "Metode Umum Pengukuran Kekerasan." },
  { "en": "Apa Itu Uji Kekerasan Brinell?", "id": "Metode Pengukuran Kekerasan Lainnya." },
  { "en": "Apa Itu Ketangguhan (Toughness)?", "id": "Kemampuan Material Menyerap Energi." },
  { "en": "Apa Itu Pengujian Impak Charpy?", "id": "Metode Pengukuran Ketangguhan Material." },
  { "en": "Apa Itu Mekanika Patahan (Fracture Mechanics)?", "id": "Studi Perambatan Retak Dalam Material." },
  { "en": "Apa Itu Ketangguhan Retak (Fracture Toughness)?", "id": "Ketahanan Material Terhadap Perambatan Retak." },
  { "en": "Apa Itu Pengujian Non-Destruktif (NDT)?", "id": "Mengevaluasi Sifat Material Tanpa Merusak." },
  { "en": "Apa Itu Pengujian Ultrasonik?", "id": "Menggunakan Gelombang Suara Mendeteksi Cacat." },
  { "en": "Apa Itu Pengujian Radiografi?", "id": "Menggunakan Sinar-X Mendeteksi Cacat Internal." },
  { "en": "Apa Itu Pengujian Partikel Magnetik?", "id": "Mendeteksi Cacat Permukaan Material Feromagnetik." },
  { "en": "Apa Itu Pengujian Penetran Cair?", "id": "Mendeteksi Retak Permukaan Yang Terbuka." },
  { "en": "Apa Itu Tribologi?", "id": "Ilmu Gesekan, Aus, Dan Pelumasan." },
  { "en": "Apa Itu Gesekan (Friction)?", "id": "Gaya Menentang Gerakan Relatif." },
  { "en": "Apa Itu Keausan (Wear)?", "id": "Hilangnya Material Dari Permukaan." },
  { "en": "Apa Itu Pelumasan (Lubrication)?", "id": "Mengurangi Gesekan Dan Keausan." },
  { "en": "Apa Itu Pelumas?", "id": "Zat Yang Digunakan Untuk Pelumasan." },
  { "en": "Apa Itu Bantalan (Bearing)?", "id": "Elemen Mesin Mengurangi Gesekan Antar Bagian." },
  { "en": "Apa Itu Bantalan Bola (Ball Bearing)?", "id": "Menggunakan Bola Untuk Memisahkan Cincin Bantalan." },
  { "en": "Apa Itu Bantalan Rol (Roller Bearing)?", "id": "Menggunakan Silinder Untuk Menahan Beban." },
  { "en": "Apa Itu Bantalan Biasa (Plain Bearing)?", "id": "Bantalan Tanpa Elemen Bergulir, Berbasis Gesekan." },
  { "en": "Apa Itu Bantalan Fluida Dinamis?", "id": "Permukaan Dipisahkan Lapisan Tipis Fluida." },
  { "en": "Apa Itu Roda Gigi (Gear)?", "id": "Komponen Mekanis Mentransmisikan Torsi, Gerakan." },
  { "en": "Apa Itu Roda Gigi Lurus (Spur Gear)?", "id": "Roda Gigi Dengan Gigi Lurus Paralel Sumbu." },
  { "en": "Apa Itu Roda Gigi Heliks (Helical Gear)?", "id": "Roda Gigi Dengan Gigi Miring." },
  { "en": "Apa Keuntungan Roda Gigi Heliks?", "id": "Operasi Lebih Halus Dan Tenang." },
  { "en": "Apa Itu Roda Gigi Bevel?", "id": "Mentransmisikan Daya Antar Poros Bersilangan." },
  { "en": "Apa Itu Roda Gigi Cacing (Worm Gear)?", "id": "Menyediakan Rasio Reduksi Kecepatan Tinggi." },
  { "en": "Apa Itu Rasio Roda Gigi?", "id": "Rasio Kecepatan Sudut Antar Roda Gigi." },
  { "en": "Apa Itu Kereta Roda Gigi (Gear Train)?", "id": "Serangkaian Roda Gigi Yang Saling Berhubungan." },
  { "en": "Apa Itu Kopling (Coupling)?", "id": "Perangkat Menghubungkan Dua Poros." },
  { "en": "Apa Itu Rem (Brake)?", "id": "Perangkat Mekanis Menghambat Gerakan." },
  { "en": "Apa Itu Kopling Gesek (Clutch)?", "id": "Memungkinkan Sambungan, Pelepasan Dua Poros." },
  { "en": "Apa Itu Pegas (Spring)?", "id": "Elemen Mekanis Elastis Penyimpan Energi." },
  { "en": "Apa Itu Hukum Hooke?", "id": "Gaya Pegas Proporsional Dengan Perpindahan." },
  { "en": "Apa Itu Peredam (Damper)?", "id": "Perangkat Menyerap, Meredam Impuls Kejut." },
  { "en": "Apa Itu Viskositas?", "id": "Ukuran Ketahanan Fluida Terhadap Aliran." },
  { "en": "Apa Itu Mekanisme Empat Batang?", "id": "Mekanisme Sederhana Terdiri Dari Empat Batang." },
  { "en": "Apa Itu Analisis Kinematik?", "id": "Studi Gerakan Tanpa Mempertimbangkan Gaya." },
  { "en": "Apa Itu Analisis Kinetik?", "id": "Studi Gerakan Dengan Mempertimbangkan Gaya." },
  { "en": "Apa Itu Getaran Mekanis?", "id": "Osilasi Mekanis Di Sekitar Titik Setimbang." },
  { "en": "Apa Itu Frekuensi Alami?", "id": "Frekuensi Getaran Sistem Tanpa Gaya Luar." },
  { "en": "Apa Itu Resonansi?", "id": "Getaran Amplitudo Besar Saat Frekuensi Paksa." },
  { "en": "Apa Itu Redaman (Damping)?", "id": "Disipasi Energi Dalam Sistem Bergetar." },
  { "en": "Apa Itu Faktor Redaman?", "id": "Ukuran Seberapa Cepat Osilasi Meluruh." },
  { "en": "Apa Itu Isolasi Getaran?", "id": "Mencegah Transmisi Getaran." },
  { "en": "Apa Itu Analisis Modal?", "id": "Studi Sifat Dinamis Struktur." },
  { "en": "Apa Itu Kelelahan Logam?", "id": "Kegagalan Material Akibat Beban Siklik." },
  { "en": "Apa Itu Kurva S-N?", "id": "Grafik Tegangan Versus Jumlah Siklus." },
  { "en": "Apa Itu Konsentrasi Tegangan?", "id": "Lokalisasi Tegangan Tinggi Di Sekitar Diskontinuitas." },
  { "en": "Apa Itu Mekanika Kontak?", "id": "Studi Deformasi Benda Padat Bersentuhan." },
  { "en": "Apa Itu Tegangan Hertzian?", "id": "Tegangan Lokal Akibat Kontak Dua Permukaan." },
  { "en": "Apa Itu Aktuator?", "id": "Komponen Sistem Penggerak Gerakan." },
  { "en": "Apa Itu Aktuator Hidrolik?", "id": "Menggunakan Fluida Bertekanan Untuk Menghasilkan Gaya." },
  { "en": "Apa Itu Aktuator Pneumatik?", "id": "Menggunakan Udara Bertekanan Untuk Menghasilkan Gaya." },
  { "en": "Apa Itu Aktuator Listrik?", "id": "Menggunakan Energi Listrik Untuk Menghasilkan Gerakan." },
  { "en": "Apa Itu Selenoida?", "id": "Aktuator Elektromagnetik Menghasilkan Gerak Linear." },
  { "en": "Apa Itu Motor Linier?", "id": "Motor Listrik Menghasilkan Gerak Lurus." },
  { "en": "Apa Itu Aktuator Piezoelektrik?", "id": "Menggunakan Efek Piezoelektrik Terbalik." },
  { "en": "Apa Itu Shape Memory Alloy (SMA)?", "id": "Paduan Mengingat Bentuk Aslinya." },
  { "en": "Apa Itu Rekayasa Optik?", "id": "Desain Komponen Dan Sistem Optik." },
  { "en": "Apa Itu Aberasi Optik?", "id": "Penyimpangan Dari Pencitraan Sempurna." },
  { "en": "Apa Itu Aberasi Sferis?", "id": "Cacat Lensa Di Mana Sinar Paralel." },
  { "en": "Apa Itu Aberasi Kromatik?", "id": "Cacat Lensa Di Mana Warna Berbeda." },
  { "en": "Apa Itu Astigmatisme?", "id": "Cacat Optik Di Mana Sinar Tidak Konvergen." },
  { "en": "Apa Itu Lapisan Anti-Refleksi?", "id": "Lapisan Tipis Mengurangi Pantulan Cahaya." },
  { "en": "Apa Itu Interferometer?", "id": "Instrumen Menggunakan Interferensi Gelombang." },
  { "en": "Apa Itu Interferometer Michelson?", "id": "Jenis Interferometer Optik Yang Umum." },
  { "en": "Apa Itu Interferometer Fabry-Pérot?", "id": "Rongga Optik Terdiri Dari Dua Cermin." },
  { "en": "Apa Itu Spektrometer?", "id": "Mengukur Sifat Cahaya Terhadap Panjang Gelombang." },
  { "en": "Apa Itu Kisi Difraksi?", "id": "Elemen Optik Memisahkan Cahaya." },
  { "en": "Apa Itu Holografi?", "id": "Teknik Merekam Gambar Tiga Dimensi." },
  { "en": "Apa Itu Optik Fourier?", "id": "Menerapkan Transformasi Fourier Pada Optik." },
  { "en": "Apa Itu Optik Non-linear?", "id": "Studi Interaksi Cahaya Intensitas Tinggi." },
  { "en": "Apa Itu Pembangkitan Harmonik Kedua?", "id": "Proses Optik Non-linear Menggandakan Frekuensi." },
  { "en": "Apa Itu Efek Kerr?", "id": "Perubahan Indeks Bias Akibat Medan Listrik." },
  { "en": "Apa Itu Mikroskop?", "id": "Instrumen Optik Untuk Melihat Objek Kecil." },
  { "en": "Apa Itu Teleskop?", "id": "Instrumen Optik Untuk Melihat Objek Jauh." },
  { "en": "Apa Itu Teleskop Refraktor?", "id": "Menggunakan Lensa Untuk Mengumpulkan Cahaya." },
  { "en": "Apa Itu Teleskop Reflektor?", "id": "Menggunakan Cermin Untuk Mengumpulkan Cahaya." },
  { "en": "Apa Itu Radiometri?", "id": "Ilmu Pengukuran Radiasi Elektromagnetik." },
  { "en": "Apa Itu Fotometri?", "id": "Ilmu Pengukuran Cahaya Terkait Persepsi." },
  { "en": "Apa Itu Lumen?", "id": "Satuan Fluks Cahaya." },
  { "en": "Apa Itu Lux?", "id": "Satuan Iluminansi (Fluks Cahaya Per Area)." },
  { "en": "Apa Itu Fotonika?", "id": "Teknologi Menghasilkan, Mengontrol Foton." },
  { "en": "Apa Itu Fotonik Silikon?", "id": "Komponen Fotonik Dibuat Di Atas Silikon." },
  { "en": "Apa Itu Kristal Fotonik?", "id": "Struktur Periodik Mempengaruhi Gerakan Foton." },
  { "en": "Apa Itu Komputasi Geografis?", "id": "Disiplin Menggunakan Komputasi Memecahkan Masalah Geografis." },
  { "en": "Apa Itu Sistem Informasi Geografis (GIS)?", "id": "Sistem Menangkap, Menyimpan, Menganalisis Data Geografis." },
  { "en": "Apa Itu Data Raster?", "id": "Model Data Geospasial Berbasis Grid Sel." },
  { "en": "Apa Itu Data Vektor?", "id": "Model Data Geospasial Berbasis Titik, Garis, Poligon." },
  { "en": "Apa Itu Penginderaan Jauh (Remote Sensing)?", "id": "Memperoleh Informasi Tanpa Kontak Fisik." },
  { "en": "Apa Itu Fotogrametri?", "id": "Ilmu Pengukuran Dari Foto." },
  { "en": "Apa Itu LiDAR (Light Detection and Ranging)?", "id": "Metode Penginderaan Jauh Menggunakan Laser." },
  { "en": "Apa Itu Sistem Navigasi Satelit Global (GNSS)?", "id": "Sistem Satelit Menyediakan Penentuan Posisi." },
  { "en": "Apa Itu Kartografi?", "id": "Ilmu Dan Seni Membuat Peta." },
  { "en": "Apa Itu Geostatistik?", "id": "Cabang Statistik Untuk Data Spasial." },
  { "en": "Apa Itu Kriging?", "id": "Metode Interpolasi Geostatistik." },
  { "en": "Apa Itu Analisis Jaringan (Spasial)?", "id": "Menganalisis Pergerakan Sepanjang Jaringan." },
  { "en": "Apa Itu Topologi (GIS)?", "id": "Hubungan Spasial Antara Objek Geografis." },
  { "en": "Apa Itu Geocoding?", "id": "Mengubah Deskripsi Lokasi Menjadi Koordinat." },
  { "en": "Apa Itu Analisis Spasial?", "id": "Menganalisis Pola, Hubungan Data Geografis." },
  { "en": "Apa Itu Model Elevasi Digital (DEM)?", "id": "Representasi Digital Permukaan Topografi." },
  { "en": "Apa Itu Interaksi Manusia-Komputer (HCI)?", "id": "Studi Desain, Penggunaan Antarmuka Komputer." },
  { "en": "Apa Itu Desain Berpusat Pada Pengguna (UCD)?", "id": "Filosofi Desain Berfokus Pada Pengguna." },
  { "en": "Apa Itu Pengalaman Pengguna (UX)?", "id": "Persepsi, Respon Seseorang Menggunakan Produk." },
  { "en": "Apa Itu Riset UX?", "id": "Memahami Perilaku, Kebutuhan, Motivasi Pengguna." },
  { "en": "Apa Itu Persona Pengguna?", "id": "Representasi Fiksi Kelompok Pengguna Target." },
  { "en": "Apa Itu Arsitektur Informasi?", "id": "Mengorganisir, Menstrukturkan Konten Secara Efektif." },
  { "en": "Apa Itu Desain Interaksi (IxD)?", "id": "Mendesain Interaksi Antara Pengguna, Produk." },
  { "en": "Apa Itu Antarmuka Pengguna Grafis (GUI)?", "id": "Antarmuka Menggunakan Ikon, Elemen Visual." },
  { "en": "Apa Itu Wireframe?", "id": "Kerangka Visual Dasar Suatu Antarmuka." },
  { "en": "Apa Itu Mockup?", "id": "Model Visual Statis, Fidelitas Tinggi." },
  { "en": "Apa Itu Prototipe?", "id": "Model Interaktif Awal Suatu Produk." },
  { "en": "Apa Itu Pengujian Usability?", "id": "Mengevaluasi Kemudahan Penggunaan Dengan Pengguna." },
  { "en": "Apa Itu Metrik Usability?", "id": "Tingkat Keberhasilan, Waktu Tugas, Kepuasan." },
  { "en": "Apa Itu Heuristik Usability Nielsen?", "id": "Sepuluh Prinsip Umum Untuk Desain Interaksi." },
  { "en": "Apa Itu Aksesibilitas (a11y)?", "id": "Desain Produk Dapat Diakses Semua Orang." },
  { "en": "Apa Itu Desain Inklusif?", "id": "Mendesain Untuk Beragam Orang, Kemampuan." },
  { "en": "Apa Itu Antarmuka Pengguna Suara (VUI)?", "id": "Antarmuka Berinteraksi Melalui Suara." },
  { "en": "Apa Itu Desain Responsif?", "id": "Desain Beradaptasi Berbagai Ukuran Layar." },
  { "en": "Apa Itu Sistem Desain?", "id": "Kumpulan Komponen, Standar Desain Terpusat." }



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

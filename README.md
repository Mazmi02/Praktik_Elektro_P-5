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


  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Aplikasi Elektronika Solid-State Untuk Kontrol Konversi Daya." },
  { "en": "Apa Itu Konverter AC ke DC (AC-DC Converter)?", "id": "Mengubah Tegangan AC Menjadi Tegangan DC." },
  { "en": "Apa Nama Lain Konverter AC ke DC?", "id": "Penyearah Atau Rectifier." },
  { "en": "Apa Itu Konverter DC ke DC (DC-DC Converter)?", "id": "Mengubah Level Tegangan DC Ke Level Lain." },
  { "en": "Apa Nama Lain Konverter DC ke DC?", "id": "Chopper Atau Regulator Switching." },
  { "en": "Apa Itu Konverter DC ke AC (DC-AC Converter)?", "id": "Mengubah Tegangan DC Menjadi Tegangan AC." },
  { "en": "Apa Nama Lain Konverter DC ke AC?", "id": "Inverter." },
  { "en": "Apa Itu Konverter AC ke AC (AC-AC Converter)?", "id": "Mengubah Tegangan AC Ke Level Atau Frekuensi Lain." },
  { "en": "Apa Itu Penyearah Setengah Gelombang (Half-Wave)?", "id": "Penyearah Paling Sederhana Hanya Menggunakan Satu Dioda." },
  { "en": "Apa Itu Penyearah Gelombang Penuh (Full-Wave)?", "id": "Penyearah Menggunakan Kedua Siklus AC (Jembatan/Trafo CT)." },
  { "en": "Apa Itu Penyearah Jembatan (Bridge Rectifier)?", "id": "Penyearah Gelombang Penuh Menggunakan Empat Dioda." },
  { "en": "Apa Itu Kapasitor Filter (Filter Capacitor)?", "id": "Kapasitor Menghaluskan Tegangan DC Setelah Penyearah." },
  { "en": "Apa Itu Riak (Ripple) Tegangan DC?", "id": "Fluktuasi Sisa AC Pada Tegangan DC Hasil Penyearah." },
  { "en": "Apa Itu Penyearah Terkendali (Controlled Rectifier)?", "id": "Penyearah Menggunakan Thyristor SCR Mengatur Output DC." },
  { "en": "Apa Itu Sudut Penyulutan (Firing Angle) SCR?", "id": "Sudut Tundaan SCR Dihidupkan Mengontrol Tegangan Output." },
  { "en": "Apa Itu Konverter Buck (Buck Converter)?", "id": "Konverter DC-DC Menurunkan Tegangan (Step-Down)." },
  { "en": "Apa Itu Konverter Boost (Boost Converter)?", "id": "Konverter DC-DC Menaikkan Tegangan (Step-Up)." },
  { "en": "Apa Itu Konverter Buck-Boost (Buck-Boost Converter)?", "id": "Konverter DC-DC Membalik Polaritas Tegangan (Naik/Turun)." },
  { "en": "Apa Itu Konverter Cuk (Cuk Converter)?", "id": "Konverter DC-DC Membalik Polaritas (Menggunakan Kapasitor)." },
  { "en": "Apa Itu Konverter SEPIC (SEPIC Converter)?", "id": "Konverter DC-DC Tidak Membalik Polaritas (Naik/Turun)." },
  { "en": "Apa Itu Mode Konduksi Kontinu (CCM)?", "id": "Arus Induktor Konverter Tidak Pernah Mencapai Nol." },
  { "en": "Apa Kepanjangan CCM (Continuous Conduction Mode)?", "id": "Mode Konduksi Kontinu." },
  { "en": "Apa Itu Mode Konduksi Diskrit (DCM)?", "id": "Arus Induktor Konverter Mencapai Nol Dalam Satu Siklus." },
  { "en": "Apa Kepanjangan DCM (Discontinuous Conduction Mode)?", "id": "Mode Konduksi Diskrit." },
  { "en": "Apa Itu Siklus Kerja (Duty Cycle)?", "id": "Rasio Waktu ON Terhadap Total Periode Switching." },
  { "en": "Apa Itu Konverter Terisolasi (Isolated Converter)?", "id": "Konverter DC-DC Input Output Terisolasi Galvanik (Trafo)." },
  { "en": "Apa Itu Konverter Flyback (Flyback Converter)?", "id": "Konverter DC-DC Terisolasi (Buck-Boost) Menyimpan Energi." },
  { "en": "Apa Itu Konverter Forward (Forward Converter)?", "id": "Konverter DC-DC Terisolasi (Buck) Transfer Energi Langsung." },
  { "en": "Apa Itu Konverter Jembatan Penuh (Full-Bridge)?", "id": "Topologi Konverter Menggunakan Empat Saklar (Daya Tinggi)." },
  { "en": "Apa Itu Konverter Setengah Jembatan (Half-Bridge)?", "id": "Topologi Konverter Menggunakan Dua Saklar." },
  { "en": "Apa Itu SMPS (Switch-Mode Power Supply)?", "id": "Catu Daya Efisiensi Tinggi Menggunakan Konverter Switching." },
  { "en": "Apa Keunggulan SMPS (Switch-Mode Power Supply) Dari Linear?", "id": "Efisiensi Tinggi, Ukuran Kecil, Berat Ringan." },
  { "en": "Apa Kelemahan SMPS (Switch-Mode Power Supply)?", "id": "Kompleks, Menghasilkan Noise EMI Frekuensi Tinggi." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Mengubah Tegangan DC Menjadi Tegangan AC." },
  { "en": "Apa Itu Inverter Gelombang Kotak (Square Wave)?", "id": "Inverter Paling Sederhana Kualitas Output Buruk." },
  { "en": "Apa Itu Inverter Gelombang Sinus Modifikasi (Modified Sine)?", "id": "Inverter Kompromi Antara Kotak Dan Sinus Murni." },
  { "en": "Apa Itu Inverter Gelombang Sinus Murni (Pure Sine Wave)?", "id": "Inverter Kualitas Output Terbaik (Mirip PLN)." },
  { "en": "Apa Itu SPWM (Sinusoidal Pulse Width Modulation)?", "id": "Teknik Switching Inverter Menghasilkan Gelombang Sinus." },
  { "en": "Apa Itu VSI (Voltage Source Inverter)?", "id": "Inverter Sumber Tegangan (Input DC Kaku Kapasitor)." },
  { "en": "Apa Itu CSI (Current Source Inverter)?", "id": "Inverter Sumber Arus (Input DC Kaku Induktor)." },
  { "en": "Apa Itu Inverter Tiga Fasa (Three-Phase Inverter)?", "id": "Inverter Menghasilkan Output AC Tiga Fasa (Enam Saklar)." },
  { "en": "Apa Itu Inverter Multilevel (Multilevel Inverter)?", "id": "Inverter Topologi Kompleks (Output Mirip Tangga) Tegangan Tinggi." },
  { "en": "Apa Itu Dioda (Diode) Daya?", "id": "Dioda Dirancang Menangani Arus Tegangan Tinggi." },
  { "en": "Apa Itu Waktu Pemulihan Balik (Reverse Recovery Time/Trr)?", "id": "Waktu Dioda Pulih (Berhenti Konduksi) Saat Balik Arah." },
  { "en": "Apa Itu Dioda Pemulihan Cepat (Fast Recovery)?", "id": "Dioda Dengan Waktu Pemulihan Balik Sangat Singkat." },
  { "en": "Apa Itu Dioda Schottky (Schottky Diode)?", "id": "Dioda Sambungan Logam-Semikonduktor (Tegangan Maju Rendah)." },
  { "en": "Apa Keunggulan Dioda Schottky?", "id": "Tegangan Maju Sangat Rendah, Switching Sangat Cepat." },
  { "en": "Apa Itu Thyristor (Thyristor/SCR)?", "id": "Saklar Semikonduktor Empat Lapis (PNPN) Terkunci." },
  { "en": "Apa Kepanjangan SCR (Silicon-Controlled Rectifier)?", "id": "Penyearah Terkendali Silikon (Nama Lain Thyristor)." },
  { "en": "Bagaimana Cara Menyalakan SCR (Silicon-Controlled Rectifier)?", "id": "Memberi Pulsa Arus Singkat Ke Gerbang (Gate)." },
  { "en": "Bagaimana Cara Mematikan SCR (Silicon-Controlled Rectifier)?", "id": "Arus Anoda Turun Di Bawah Arus Penahan (Holding Current)." },
  { "en": "Apa Itu Arus Latching (Latching Current) SCR?", "id": "Arus Minimum Agar SCR Tetap Menyala Setelah Pulsa Gate." },
  { "en": "Apa Itu Arus Penahan (Holding Current) SCR?", "id": "Arus Minimum Agar SCR Tetap Menyala (Lebih Rendah Latching)." },
  { "en": "Apa Itu TRIAC (Triode for Alternating Current)?", "id": "Thyristor Dua Arah (Dapat Mengontrol) Beban AC." },
  { "en": "Apa Itu DIAC (Diode for Alternating Current)?", "id": "Dioda Pemicu Dua Arah (Menyalakan TRIAC)." },
  { "en": "Apa Itu GTO (Gate Turn-Off Thyristor)?", "id": "Thyristor Dapat Dimatikan Paksa Melalui Gerbang (Gate)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) Daya?", "id": "Saklar Semikonduktor Dikontrol Tegangan (Switching Cepat)." },
  { "en": "Apa Keunggulan MOSFET (Metal-Oxide-Semiconductor FET) Daya?", "id": "Switching Sangat Cepat, Dikontrol Tegangan, Tahan Panas." },
  { "en": "Apa Itu Rds(on) (Rds-on) MOSFET?", "id": "Resistansi Drain-Source Saat MOSFET ON (Sepenuhnya Terbuka)." },
  { "en": "Apa Itu IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Gabungan (Input MOSFET) Dan (Output BJT) Daya." },
  { "en": "Apa Keunggulan IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Dikontrol Tegangan (MOSFET) Kemampuan Arus Tinggi (BJT)." },
  { "en": "Apa Aplikasi IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Motor Drive VFD, Inverter Surya, Pengelasan." },
  { "en": "Apa Itu Sirkuit Penggerak Gerbang (Gate Driver)?", "id": "Sirkuit Menguatkan Sinyal Kontrol (Menggerakkan) Gate MOSFET/IGBT." },
  { "en": "Mengapa Perlu Penggerak Gerbang (Gate Driver)?", "id": "Gate (MOSFET/IGBT) Memiliki Kapasitansi (Butuh Arus Besar Cepat)." },
  { "en": "Apa Itu Penggerak Sisi Tinggi (High-Side Driver)?", "id": "Gate Driver Menggerakkan Saklar (Terkoneksi) Ke Rel Positif." },
  { "en": "Apa Itu Penggerak Sisi Rendah (Low-Side Driver)?", "id": "Gate Driver Menggerakkan Saklar (Terkoneksi) Ke Ground." },
  { "en": "Apa Itu Catu Daya Terapung (Floating Supply) Driver?", "id": "Catu Daya (Terisolasi) Untuk Penggerak Sisi Tinggi." },
  { "en": "Apa Itu Bootstrap (Bootstrap Supply) Driver?", "id": "Teknik (Mengisi Kapasitor) Membuat Catu Daya Terapung." },
  { "en": "Apa Itu Waktu Mati (Dead Time) Inverter?", "id": "Tundaan (Sengaja) Mencegah (Dua Saklar) (High/Low) ON Bersamaan." },
  { "en": "Apa Itu Tembak-Tembus (Shoot-Through)?", "id": "Kondisi (Gagal) Saklar High Side Low Side ON Bersamaan." },
  { "en": "Apa Itu Penjepit (Clamping) Tegangan Gerbang?", "id": "Menggunakan (Dioda Zener) Melindungi (Gate) Dari Overvoltage." },
  { "en": "Apa Itu Kapasitansi Miller (Miller Capacitance) MOSFET?", "id": "Kapasitansi (Gate-Drain) Menyebabkan (Dataran Tinggi Miller)." },
  { "en": "Apa Itu Dataran Tinggi Miller (Miller Plateau)?", "id": "Kondisi (Tegangan Gate) Tetap (Saat Switching) Mengisi Cgd." },
  { "en": "Apa Itu SOA (Safe Operating Area)?", "id": "Grafik (Area Aman) Batas (Tegangan, Arus, Daya) Transistor." },
  { "en": "Apa Itu Kegagalan Mode Bersama (Common-Mode Failure)?", "id": "Kegagalan (Sistem) Akibat (Penyebab) Tunggal (Catu Daya)." },
  { "en": "Apa Itu Desain Redundan (Redundant Design)?", "id": "Memiliki (Komponen Cadangan) Mengambil Alih (Saat Gagal)." },
  { "en": "Apa Itu Redundansi N+1 (N+1 Redundancy)?", "id": "Sistem (Membutuhkan N Unit) Ditambah (Satu Unit Cadangan)." },
  { "en": "Apa Itu Hot Swap (Hot-Swap)?", "id": "Kemampuan (Mengganti) Komponen (Tanpa Mematikan) Sistem." },
  { "en": "Apa Itu Soft Start (Soft-Start)?", "id": "Teknik (Menyalakan) Catu Daya (Perlahan) Membatasi (Arus Inrush)." },
  { "en": "Apa Itu Perlindungan Arus Lebih (Overcurrent Protection/OCP)?", "id": "Mematikan Sirkuit Jika Arus Melebihi Batas Aman." },
  { "en": "Apa Itu Perlindungan Tegangan Lebih (Overvoltage Protection/OVP)?", "id": "Mematikan Sirkuit Jika Tegangan Melebihi Batas Aman." },
  { "en": "Apa Itu Perlindungan Suhu Lebih (Overtemperature Protection/OTP)?", "id": "Mematikan Sirkuit Jika Suhu Melebihi Batas Aman." },
  { "en": "Apa Itu VFD (Variable Frequency Drive)?", "id": "Kontroler (Inverter) Mengatur (Kecepatan) Motor AC (Induksi)." },
  { "en": "Bagaimana VFD (Variable Frequency Drive) Mengatur Kecepatan?", "id": "Mengubah Frekuensi Dan Tegangan Output (Kontrol V/f)." },
  { "en": "Apa Itu Kontrol V/f (V/f Control)?", "id": "Metode (Kontrol VFD) Menjaga (Rasio Tegangan/Frekuensi) Konstan." },
  { "en": "Apa Itu Kontrol Vektor (Vector Control/FOC)?", "id": "Metode (Kontrol VFD) Canggih (Mengontrol Torsi) Presisi." },
  { "en": "Apa Kepanjangan FOC (Field-Oriented Control)?", "id": "Kontrol Berorientasi Medan (Kontrol Vektor)." },
  { "en": "Apa Itu Pengereman Dinamis (Dynamic Braking) VFD?", "id": "Membuang (Energi Regeneratif) Motor (Menjadi Panas) Di Resistor." },
  { "en": "Apa Itu Pengereman Regeneratif (Regenerative Braking)?", "id": "Mengembalikan (Energi Pengereman) Motor (Kembali) Ke Catu Daya." },
  { "en": "Apa Itu Bus DC (DC Bus/Link) VFD?", "id": "Bagian (Tengah VFD) (Kapasitor) Menyimpan (Energi DC) Penyearah." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Catu Daya (Baterai) Memberi (Daya Darurat) Saat PLN Mati." },
  { "en": "Apa Itu UPS Standby (Offline)?", "id": "UPS (Paling Umum) (Baterai Diam) Beralih (Saat Mati Listrik)." },
  { "en": "Apa Itu UPS Line-Interactive (Line-Interactive)?", "id": "UPS (Standby) Dengan (AVR) Menstabilkan Tegangan." },
  { "en": "Apa Itu UPS Online (Double-Conversion)?", "id": "UPS (Kualitas Terbaik) (Beban Selalu) Ditenagai (Inverter)." },
  { "en": "Apa Keunggulan UPS Online?", "id": "Kualitas Listrik Sempurna (Tidak Ada Waktu Transfer)." },
  { "en": "Apa Itu Waktu Transfer (Transfer Time) UPS?", "id": "Waktu (Tunda) UPS (Standby) Beralih Ke Baterai." },
  { "en": "Apa Itu AVR (Automatic Voltage Regulator)?", "id": "Perangkat (Menstabilkan) Tegangan (Jaringan Listrik) Naik Turun." },
  { "en": "Apa Itu Stavolt (Stavolt)?", "id": "Nama (Merek) Populer (Untuk AVR) Di Indonesia." },
  { "en": "Apa Itu Trafo Step-Down (Step-Down)?", "id": "Trafo (Menurunkan) Tegangan (Lilitan Sekunder < Primer)." },
  { "en": "Apa Itu Trafo Step-Up (Step-Up)?", "id": "Trafo (Menaikkan) Tegangan (Lilitan Sekunder > Primer)." },
  { "en": "Apa Itu Trafo Isolasi (Isolation Transformer)?", "id": "Trafo (Rasio 1:1) (Isolasi Galvanik) Keselamatan." },
  { "en": "Apa Itu Autotransformer (Autotransformer)?", "id": "Trafo (Satu Lilitan) (Tidak Ada Isolasi) (Efisien)." },
  { "en": "Apa Itu Antarmuka Grafis Pengguna (GUI)?", "id": "Antarmuka Program Berbasis Visual Tombol Jendela." },
  { "en": "Apa Kepanjangan GUI (Graphical User Interface)?", "id": "Antarmuka Grafis Pengguna." },
  { "en": "Apa Itu Antarmuka Baris Perintah (CLI)?", "id": "Antarmuka Program Berbasis Teks Perintah." },
  { "en": "Apa Kepanjangan CLI (Command Line Interface)?", "id": "Antarmuka Baris Perintah." },
  { "en": "Apa Itu Komputasi Paralel (Parallel Computing)?", "id": "Menjalankan Banyak Instruksi Perhitungan Secara Bersamaan." },
  { "en": "Apa Itu Komputasi Sekuensial (Sequential Computing)?", "id": "Menjalankan Instruksi Perhitungan Satu Demi Satu Berurutan." },
  { "en": "Apa Itu Multithreading (Multithreading)?", "id": "Menjalankan Beberapa Bagian Program Thread Secara Bersamaan." },
  { "en": "Apa Itu Proses (Process) Komputer?", "id": "Sebuah Instansi Program Yang Sedang Berjalan (Memiliki Ruang Memori Sendiri)." },
  { "en": "Apa Itu Thread (Thread) Komputer?", "id": "Bagian Aliran Eksekusi Dalam Sebuah Proses." },
  { "en": "Apa Itu Multi-Core (Multi-Core)?", "id": "Memiliki Lebih Dari Satu CPU Inti Pemrosesan Dalam Satu Chip." },
  { "en": "Apa Itu Komputasi Terdistribusi (Distributed Computing)?", "id": "Menggunakan Jaringan Komputer Bekerja Sama Menyelesaikan Tugas." },
  { "en": "Apa Itu Komputasi Cluster (Cluster Computing)?", "id": "Komputasi Menggunakan Sekumpulan Komputer Terhubung Ketat (Homogen)." },
  { "en": "Apa Itu Komputasi Grid (Grid Computing)?", "id": "Komputasi Menggunakan Sumber Daya Komputer Terdistribusi Luas (Heterogen)." },
  { "en": "Apa Itu Komputasi Cloud (Cloud Computing)?", "id": "Layanan Komputasi Melalui Internet (Virtualisasi Sumber Daya)." },
  { "en": "Apa Itu Virtualisasi (Virtualization)?", "id": "Menciptakan Versi Virtual Sumber Daya Komputer (Hardware, OS)." },
  { "en": "Apa Itu Mesin Virtual (Virtual Machine/VM)?", "id": "Emulasi Sistem Komputer Lengkap (Guest OS Berjalan Di Host)." },
  { "en": "Apa Itu Hypervisor (Hypervisor)?", "id": "Perangkat Lunak Yang Mengelola Dan Menjalankan Mesin Virtual." },
  { "en": "Apa Itu Tipe 1 Hypervisor (Bare-Metal)?", "id": "Hypervisor Berjalan Langsung Di Atas Hardware Fisik Server." },
  { "en": "Apa Itu Tipe 2 Hypervisor (Hosted)?", "id": "Hypervisor Berjalan Di Atas Sistem Operasi Host." },
  { "en": "Apa Itu Container (Kontainer) Perangkat Lunak?", "id": "Paket Ringan Mengemas Kode Aplikasi Dan Dependensinya." },
  { "en": "Apa Beda Mesin Virtual (VM) Dan Kontainer (Container)?", "id": "VM Punya OS Sendiri, Kontainer Berbagi OS Host (Lebih Ringan)." },
  { "en": "Apa Itu Docker (Docker)?", "id": "Platform Populer Untuk Membangun Dan Menjalankan Kontainer." },
  { "en": "Apa Itu Kubernetes (Kubernetes/K8s)?", "id": "Sistem Orkestrasi Kontainer Otomatisasi Deployment Skala Besar." },
  { "en": "Apa Itu Orkestrasi (Orchestration)?", "id": "Manajemen Otomatis Koordinasi Sumber Daya Komputer Kompleks." },
  { "en": "Apa Itu Microservices (Layanan Mikro)?", "id": "Arsitektur Membangun Aplikasi Sebagai Kumpulan Layanan Kecil Independen." },
  { "en": "Apa Itu Monolitik (Monolithic) Arsitektur?", "id": "Membangun Aplikasi Sebagai Satu Unit Besar Terpadu." },
  { "en": "Apa Itu Load Balancer (Penyeimbang Beban)?", "id": "Mendistribusikan Lalu Lintas Jaringan Masuk Ke Beberapa Server." },
  { "en": "Apa Itu Failover (Failover)?", "id": "Mekanisme Otomatis Mengalihkan Beban Ke Sistem Cadangan Saat Gagal." },
  { "en": "Apa Itu Failback (Failback)?", "id": "Mekanisme Mengalihkan Beban Kembali Ke Sistem Utama Setelah Perbaikan." },
  { "en": "Apa Itu Penyimpanan Cloud (Cloud Storage)?", "id": "Layanan Penyimpanan Data Melalui Internet (Contoh: S3, Azure Blob)." },
  { "en": "Apa Itu Serverless (Tanpa Server)?", "id": "Model Komputasi Cloud (Provider Mengelola Infrastruktur Server)." },
  { "en": "Apa Itu Fungsi Sebagai Layanan (FaaS)?", "id": "Model Serverless (Hanya Menjalankan Potongan Kode Fungsi) Berdasarkan Peristiwa." },
  { "en": "Apa Itu Infrastruktur Sebagai Kode (IaC)?", "id": "Mengelola Dan Menyediakan Infrastruktur Melalui File Definisi Kode." },
  { "en": "Apa Itu Terraform (Terraform)?", "id": "Alat Populer Untuk Mengimplementasikan Infrastruktur Sebagai Kode (IaC)." },
  { "en": "Apa Itu Ansible (Ansible)?", "id": "Alat Otomatisasi (Konfigurasi, Deployment, Orkestrasi) Tanpa Agen Klien." },
  { "en": "Apa Itu GitOps (GitOps)?", "id": "Metode CI/CD Menggunakan Git Sebagai Sumber Kebenaran Utama (Deklaratif)." },
  { "en": "Apa Itu CI/CD Pipeline (CI/CD Pipeline)?", "id": "Serangkaian Langkah Otomatis Untuk Integrasi Dan Pengiriman Kode." },
  { "en": "Apa Itu Continuous Integration (Integrasi Berkelanjutan)?", "id": "Otomatisasi Pembangunan Dan Pengujian Kode Setelah Setiap Commit." },
  { "en": "Apa Itu Continuous Delivery (Pengiriman Berkelanjutan)?", "id": "Otomatisasi Kode Siap Rilis Ke Produksi (Perlu Persetujuan Manual)." },
  { "en": "Apa Itu Continuous Deployment (Penerapan Berkelanjutan)?", "id": "Otomatisasi Penuh Rilis Kode Langsung Ke Produksi." },
  { "en": "Apa Itu Test-Driven Development (TDD)?", "id": "Metode Pengembangan Kode Ditulis Setelah Tes Gagal Dibuat." },
  { "en": "Apa Itu Behaviour-Driven Development (BDD)?", "id": "Metode Pengembangan Berdasarkan Perilaku Yang Diinginkan Pengguna." },
  { "en": "Apa Itu Software Development Kit (SDK)?", "id": "Kumpulan Alat Pustaka Untuk Membangun Aplikasi Tertentu." },
  { "en": "Apa Itu Application Programming Interface (API)?", "id": "Kontrak Antarmuka Perangkat Lunak Yang Memungkinkan Komunikasi." },
  { "en": "Apa Itu Representational State Transfer (REST)?", "id": "Gaya Arsitektur API Populer Berbasis HTTP." },
  { "en": "Apa Itu Server Web (Web Server)?", "id": "Perangkat Lunak Melayani Permintaan Halaman Web (Contoh: Nginx)." },
  { "en": "Apa Itu Database Server (Server Basis Data)?", "id": "Perangkat Lunak Mengelola Dan Menyimpan Data (Contoh: MySQL)." },
  { "en": "Apa Itu Web Browser (Peramban Web)?", "id": "Perangkat Lunak Mengakses Dan Menampilkan Halaman Web." },
  { "en": "Apa Itu Front-End (Front-End)?", "id": "Bagian Aplikasi Yang Berinteraksi Langsung Dengan Pengguna (UI)." },
  { "en": "Apa Itu Back-End (Back-End)?", "id": "Bagian Aplikasi Yang Berjalan Di Server (Logika Bisnis, Database)." },
  { "en": "Apa Itu Full-Stack (Full-Stack)?", "id": "Pengembang Yang Mampu Bekerja Di Kedua Sisi Front-End Dan Back-End." },
  { "en": "Apa Itu Protokol HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Aplikasi Standar Untuk World Wide Web." },
  { "en": "Apa Itu Protokol HTTPS (Hypertext Transfer Protocol Secure)?", "id": "HTTP Yang Diamankan Dengan Enkripsi TLS/SSL." },
  { "en": "Apa Itu RESTful API?", "id": "API Yang Mengikuti Prinsip-prinsip Arsitektur REST." },
  { "en": "Apa Itu URL (Uniform Resource Locator)?", "id": "Alamat Lengkap Sumber Daya Di Internet." },
  { "en": "Apa Itu HTML (HyperText Markup Language)?", "id": "Bahasa Markup Dasar Untuk Struktur Halaman Web." },
  { "en": "Apa Itu CSS (Cascading Style Sheets)?", "id": "Bahasa Mengatur Tampilan Gaya Halaman Web." },
  { "en": "Apa Itu JavaScript (JavaScript)?", "id": "Bahasa Pemrograman Membuat Halaman Web Interaktif." },
  { "en": "Apa Itu Cookie (Cookie)?", "id": "Data Kecil Disimpan Browser Untuk Melacak Sesi." },
  { "en": "Apa Itu Cache (Cache)?", "id": "Penyimpanan Data Sementara Untuk Akses Cepat." },
  { "en": "Apa Itu DNS (Domain Name System)?", "id": "Sistem Menerjemahkan Nama Domain Ke Alamat IP." },
  { "en": "Apa Itu Jaringan CDN (Content Delivery Network)?", "id": "Jaringan Server Terdistribusi Menyimpan Konten Web Statis." },
  { "en": "Apa Itu SEO (Search Engine Optimization)?", "id": "Proses Mengoptimalkan Situs Web Agar Peringkat Tinggi Di Mesin Pencari." },
  { "en": "Apa Itu Mesin Pencari (Search Engine)?", "id": "Program Komputer Mencari Dan Mengindeks Informasi Web." },
  { "en": "Apa Itu Algoritma Mesin Pencari?", "id": "Aturan Yang Digunakan Mesin Pencari Untuk Memberi Peringkat Halaman." },
  { "en": "Apa Itu Analisis Data (Data Analysis)?", "id": "Proses Memeriksa Data Untuk Mendapatkan Kesimpulan." },
  { "en": "Apa Itu Visualisasi Data (Data Visualization)?", "id": "Menyajikan Data Dalam Bentuk Grafik Atau Diagram Visual." },
  { "en": "Apa Itu Statistik Deskriptif (Descriptive Statistics)?", "id": "Statistik Meringkas Fitur Utama Kumpulan Data." },
  { "en": "Apa Itu Statistik Inferensial (Inferential Statistics)?", "id": "Statistik Membuat Kesimpulan Tentang Populasi Dari Sampel." },
  { "en": "Apa Itu Mean (Rata-rata)?", "id": "Nilai Rata-Rata Dari Kumpulan Data." },
  { "en": "Apa Itu Median (Median)?", "id": "Nilai Tengah Dari Kumpulan Data Yang Sudah Diurutkan." },
  { "en": "Apa Itu Modus (Mode)?", "id": "Nilai Yang Paling Sering Muncul Dalam Kumpulan Data." },
  { "en": "Apa Itu Standar Deviasi (Standard Deviation)?", "id": "Ukuran Penyebaran Data Dari Nilai Rata-Rata." },
  { "en": "Apa Itu Variansi (Variance)?", "id": "Rata-Rata Kuadrat Perbedaan Setiap Nilai Dari Nilai Rata-Rata." },
  { "en": "Apa Itu Regresi Linear (Linear Regression)?", "id": "Model Statistik Mencari Hubungan Linear Antara Dua Variabel." },
  { "en": "Apa Itu Variabel Dependen (Dependent Variable)?", "id": "Variabel Yang Nilainya Diprediksi (Output)." },
  { "en": "Apa Itu Variabel Independen (Independent Variable)?", "id": "Variabel Yang Digunakan Untuk Memprediksi (Input)." },
  { "en": "Apa Itu Koefisien Korelasi (Correlation Coefficient)?", "id": "Ukuran Kekuatan Dan Arah Hubungan Linear Antara Variabel." },
  { "en": "Apa Itu Nilai P (P-Value)?", "id": "Probabilitas Mengamati Data Ekstrem Jika Hipotesis Nol Benar." },
  { "en": "Apa Itu Hipotesis Nol (Null Hypothesis)?", "id": "Asumsi Awal (Tidak Ada Perbedaan) Yang Ingin Ditolak." },
  { "en": "Apa Itu Hipotesis Alternatif (Alternative Hypothesis)?", "id": "Pernyataan Yang Diyakini Benar Jika Hipotesis Nol Ditolak." },
  { "en": "Apa Itu T-Test (T-Test)?", "id": "Uji Statistik Membandingkan Rata-Rata Dua Kelompok." },
  { "en": "Apa Itu Uji ANOVA (Analysis of Variance)?", "id": "Uji Statistik Membandingkan Rata-Rata Lebih Dari Dua Kelompok." },
  { "en": "Apa Itu Data Kualitatif (Qualitative Data)?", "id": "Data Yang Tidak Dapat Diukur Secara Numerik (Warna, Nama)." },
  { "en": "Apa Itu Data Kuantitatif (Quantitative Data)?", "id": "Data Yang Dapat Diukur Dalam Bentuk Angka (Suhu, Tinggi)." },
  { "en": "Apa Itu Data Diskrit (Discrete Data)?", "id": "Data Kuantitatif Yang Hanya Bisa Memiliki Nilai Tertentu (Jumlah)." },
  { "en": "Apa Itu Data Kontinu (Continuous Data)?", "id": "Data Kuantitatif Yang Dapat Memiliki Nilai Apapun Dalam Rentang (Suhu)." },
  { "en": "Apa Itu Pemodelan Statistik (Statistical Modeling)?", "id": "Menggunakan Persamaan Matematika Mewakili Hubungan Antar Variabel." },
  { "en": "Apa Itu Model Prediktif (Predictive Model)?", "id": "Model Statistik Digunakan Untuk Membuat Prediksi Masa Depan." },
  { "en": "Apa Itu Model Inferensial (Inferential Model)?", "id": "Model Statistik Digunakan Untuk Memahami Hubungan Sebab Akibat." },
  { "en": "Apa Itu Analisis Deret Waktu (Time Series Analysis)?", "id": "Analisis Data Dikumpulkan Berurutan Selama Periode Waktu." },
  { "en": "Apa Itu Peramalan (Forecasting)?", "id": "Proses Membuat Prediksi Tentang Masa Depan Berdasarkan Data Masa Lalu." },
  { "en": "Apa Itu Data Mining (Penambangan Data)?", "id": "Proses Menemukan Pola Besar Dalam Kumpulan Data." },
  { "en": "Apa Itu Pemrosesan Sinyal Audio (Audio Processing)?", "id": "DSP Diterapkan Untuk Memanipulasi Suara Dan Musik." },
  { "en": "Apa Itu Ekualiser (Equalizer/EQ)?", "id": "Filter Audio Mengatur Penguatan Atau Pelemahan Frekuensi." },
  { "en": "Apa Itu Kompresor (Compressor) Audio?", "id": "Mengurangi Rentang Dinamis Suara Keras Secara Otomatis." },
  { "en": "Apa Itu Limiter (Limiter) Audio?", "id": "Kompresor Ekstrem Mencegah Sinyal Melebihi Ambang Batas." },
  { "en": "Apa Itu Gerbang Derau (Noise Gate) Audio?", "id": "Mematikan Sinyal Jika Levelnya Di Bawah Ambang Batas Noise." },
  { "en": "Apa Itu Gema (Reverb/Reverberation) Audio?", "id": "Efek Simulasi Pantulan Suara Dalam Suatu Ruangan." },
  { "en": "Apa Itu Tundaan (Delay/Echo) Audio?", "id": "Efek Mengulang Sinyal Asli Dengan Waktu Tunda." },
  { "en": "Apa Itu Chorus (Chorus) Audio?", "id": "Efek Suara Digandakan Sedikit Berubah Nada Dan Waktu." },
  { "en": "Apa Itu Flanger (Flanger) Audio?", "id": "Efek Modulasi Penundaan Cepat Menciptakan Suara Berputar." },
  { "en": "Apa Itu Filter High-Pass (High-Pass Filter/HPF)?", "id": "Filter Audio Meneruskan Frekuensi Tinggi Menghilangkan Bass." },
  { "en": "Apa Itu Filter Low-Pass (Low-Pass Filter/LPF)?", "id": "Filter Audio Meneruskan Frekuensi Rendah Menghilangkan Treble." },
  { "en": "Apa Itu Filter Band-Pass (Band-Pass Filter/BPF)?", "id": "Filter Audio Meneruskan Rentang Frekuensi Tertentu Saja." },
  { "en": "Apa Itu Filter Notch (Notch Filter)?", "id": "Filter Audio Menghilangkan Frekuensi Tunggal Yang Sangat Spesifik." },
  { "en": "Apa Itu Q-Factor (Q-Factor) Filter?", "id": "Ukuran Kualitas Atau Selektivitas Filter (Lebar Pita)." },
  { "en": "Apa Itu Titik Potong (Cutoff Frequency)?", "id": "Frekuensi Dimana Gain Filter Turun Sebesar 3 Desibel." },
  { "en": "Apa Itu Rentang Dinamis (Dynamic Range) Audio?", "id": "Perbedaan Level Suara Paling Lemah Dan Paling Keras." },
  { "en": "Apa Itu Rasio Kompresi (Compression Ratio)?", "id": "Rasio Perubahan Level Input Dibanding Perubahan Level Output." },
  { "en": "Apa Itu Ambang Batas (Threshold) Kompresor?", "id": "Level Dimana Kompresor Mulai Bekerja (Mengurangi Gain)." },
  { "en": "Apa Itu Attack Time (Waktu Serangan) Kompresor?", "id": "Waktu Kompresor Mulai Bekerja Penuh Setelah Sinyal Melebihi Ambang." },
  { "en": "Apa Itu Release Time (Waktu Lepas) Kompresor?", "id": "Waktu Kompresor Berhenti Bekerja Setelah Sinyal Turun Di Bawah Ambang." },
  { "en": "Apa Itu Make-up Gain (Make-up Gain)?", "id": "Penguatan Ditambahkan Setelah Kompresi Mengembalikan Level Volume." },
  { "en": "Apa Itu Transduser (Transducer)?", "id": "Perangkat Mengubah Energi Dari Satu Bentuk Ke Bentuk Lain." },
  { "en": "Apa Itu Transduser Input?", "id": "Transduser Yang Bertindak Sebagai Sensor (Mikrofon, Termokopel)." },
  { "en": "Apa Itu Transduser Output?", "id": "Transduser Yang Bertindak Sebagai Aktuator (Speaker, Motor)." },
  { "en": "Apa Itu Transmiter (Transmitter)?", "id": "Perangkat Mengubah Sinyal Sensor Menjadi Sinyal Transmisi (4-20 MA)." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Mengubah Perubahan Suhu Menjadi Perubahan Resistansi." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor Suhu Mengubah Perbedaan Suhu Menjadi Tegangan (Efek Seebeck)." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Sensor Suhu Mengubah Perubahan Suhu Menjadi Perubahan Resistansi." },
  { "en": "Apa Beda RTD (Resistance Temperature Detector) Dan Termistor?", "id": "RTD Logam Murni Lebih Linear, Termistor Semikonduktor Lebih Sensitif." },
  { "en": "Apa Itu Strain Gauge (Strain Gauge)?", "id": "Sensor Mengukur Regangan Perubahan Bentuk Mekanis." },
  { "en": "Apa Itu Sel Beban (Load Cell)?", "id": "Sensor Mengukur Gaya Berat Menggunakan Strain Gauge." },
  { "en": "Apa Itu Sensor Proximity (Proximity Sensor)?", "id": "Sensor Mendeteksi Keberadaan Objek Tanpa Kontak Fisik." },
  { "en": "Apa Itu Sensor Proximity Induktif?", "id": "Sensor Proximity Hanya Mendeteksi Objek Logam." },
  { "en": "Apa Itu Sensor Proximity Kapasitif?", "id": "Sensor Proximity Mendeteksi Objek Logam Dan Non-Logam." },
  { "en": "Apa Itu Sensor Fotoelektrik (Photoelectric Sensor)?", "id": "Sensor Mendeteksi Objek Menggunakan Berkas Cahaya." },
  { "en": "Apa Itu Tipe Thru-Beam (Thru-Beam) Sensor Fotoelektrik?", "id": "Pemancar Dan Penerima Terpisah Objek Memotong Cahaya." },
  { "en": "Apa Itu Tipe Retro-Reflective (Retro-Reflective) Sensor Fotoelektrik?", "id": "Menggunakan Reflektor Cermin Untuk Memantulkan Berkas Cahaya." },
  { "en": "Apa Itu Tipe Diffuse (Diffuse) Sensor Fotoelektrik?", "id": "Mendeteksi Objek Berdasarkan Pantulan Cahaya Dari Permukaan Objek." },
  { "en": "Apa Itu Sensor Ultrasonik (Ultrasonic Sensor)?", "id": "Sensor Mengukur Jarak Menggunakan Pantulan Gelombang Suara Tinggi." },
  { "en": "Apa Itu Lidar (Light Detection and Ranging)?", "id": "Sensor Jarak Jauh Menggunakan Pulsa Cahaya Laser." },
  { "en": "Apa Itu Sensor Kecepatan (Speed Sensor)?", "id": "Sensor Mengukur Kecepatan Linear Atau Sudut Putaran." },
  { "en": "Apa Itu Tachometer (Tachometer)?", "id": "Alat Mengukur Kecepatan Sudut Putaran (RPM)." },
  { "en": "Apa Itu Encoder (Encoder)?", "id": "Sensor Mengubah Gerakan Mekanis Menjadi Sinyal Listrik Digital." },
  { "en": "Apa Itu Giroskop (Gyroscope)?", "id": "Sensor Mengukur Kecepatan Rotasi Sudut." },
  { "en": "Apa Itu Akselerometer (Accelerometer)?", "id": "Sensor Mengukur Percepatan Linear Dan Getaran." },
  { "en": "Apa Itu IMU (Inertial Measurement Unit)?", "id": "Unit Sensor Gabungan Akselerometer Giroskop Magnetometer." },
  { "en": "Apa Itu Sensor Gaya (Force Sensor)?", "id": "Sensor Mengukur Gaya Yang Diberikan Pada Permukaan." },
  { "en": "Apa Itu Sensor Torsi (Torque Sensor)?", "id": "Sensor Mengukur Gaya Putar Torsi Yang Diberikan Poros." },
  { "en": "Apa Itu Sensor Kelembaban (Humidity Sensor)?", "id": "Sensor Mengukur Kandungan Uap Air Kelembaban Udara." },
  { "en": "Apa Itu Sensor Gas (Gas Sensor)?", "id": "Sensor Mendeteksi Konsentrasi Atau Kehadiran Gas Tertentu." },
  { "en": "Apa Itu Sensor PH (PH Sensor)?", "id": "Sensor Mengukur Tingkat Keasaman Atau Kebasaan Larutan." },
  { "en": "Apa Itu Sensor Konduktivitas (Conductivity Sensor)?", "id": "Sensor Mengukur Kemampuan Larutan Menghantarkan Listrik." },
  { "en": "Apa Itu Sensor Warna (Color Sensor)?", "id": "Sensor Mengukur Intensitas Cahaya Merah Hijau Biru (RGB)." },
  { "en": "Apa Itu Sensor Aliran (Flow Sensor/Flowmeter)?", "id": "Sensor Mengukur Laju Aliran Massa Atau Volume Fluida." },
  { "en": "Apa Itu Sensor Aliran Magnetik (Magnetic Flowmeter)?", "id": "Flowmeter Mengukur Aliran Cairan Konduktif Berbasis Hukum Faraday." },
  { "en": "Apa Itu Sensor Aliran Vortex (Vortex Flowmeter)?", "id": "Flowmeter Mengukur Aliran Berdasarkan Pusaran Eddy Yang Terbentuk." },
  { "en": "Apa Itu Sensor Ketinggian (Level Sensor)?", "id": "Sensor Mengukur Ketinggian Cairan Atau Material Curah Dalam Tangki." },
  { "en": "Apa Itu Sensor Ketinggian Tipe Pelampung (Float Level)?", "id": "Sensor Ketinggian Menggunakan Mekanisme Pelampung Fisik." },
  { "en": "Apa Itu Sensor Ketinggian Tipe Ultrasonik?", "id": "Sensor Ketinggian Menggunakan Gelombang Ultrasonik Tanpa Kontak." },
  { "en": "Apa Itu Sensor Ketinggian Tipe Radar?", "id": "Sensor Ketinggian Menggunakan Gelombang Mikro Radar Tanpa Kontak." },
  { "en": "Apa Itu Sensor Ketinggian Tipe Tekanan (Hydrostatic)?", "id": "Sensor Ketinggian Mengukur Tekanan Hidrostatis Cairan." },
  { "en": "Apa Itu Sensor Ketinggian Tipe Kapasitif?", "id": "Sensor Ketinggian Mengukur Perubahan Kapasitansi Karena Cairan." },
  { "en": "Apa Itu Sensor Ketinggian Tipe Optik?", "id": "Sensor Ketinggian Menggunakan Cahaya LED Mendeteksi Batas Cairan." },
  { "en": "Apa Itu Sensor Visi Mesin (Machine Vision Sensor)?", "id": "Kamera Industri Yang Digunakan Untuk Akuisisi Gambar Dan Inspeksi." },
  { "en": "Apa Itu Pencahayaan Terstruktur (Structured Light)?", "id": "Proyeksi Pola Cahaya Ke Objek Untuk Rekonstruksi Dimensi 3D." },
  { "en": "Apa Itu Pengenalan Pola (Pattern Recognition)?", "id": "Proses Komputer Mengidentifikasi Pola Mirip Dalam Data Atau Gambar." },
  { "en": "Apa Itu Uji Non-Destruktif (Non-Destructive Testing/NDT)?", "id": "Metode Pengujian Material Tanpa Menyebabkan Kerusakan." },
  { "en": "Apa Itu Uji Ultrasonik (Ultrasonic Testing/UT)?", "id": "NDT Menggunakan Gelombang Ultrasonik Mendeteksi Cacat Internal." },
  { "en": "Apa Itu Uji Radiografi (Radiographic Testing/RT)?", "id": "NDT Menggunakan Sinar-X Atau Gamma Mendeteksi Cacat Internal." },
  { "en": "Apa Itu Uji Partikel Magnetik (Magnetic Particle Testing/MT)?", "id": "NDT Mendeteksi Cacat Permukaan Dan Bawah Permukaan Logam Feromagnetik." },
  { "en": "Apa Itu Uji Cairan Penetrasi (Penetrant Testing/PT)?", "id": "NDT Mendeteksi Cacat Permukaan Terbuka Menggunakan Cairan Warna." },
  { "en": "Apa Itu Uji Arus Eddy (Eddy Current Testing/ET)?", "id": "NDT Mendeteksi Cacat Dan Mengukur Ketebalan Logam Konduktif." },
  { "en": "Apa Itu Uji Termografi (Thermography Testing)?", "id": "NDT Menggunakan Kamera Inframerah Mendeteksi Anomali Panas." },
  { "en": "Apa Itu Uji Visual (Visual Testing/VT)?", "id": "NDT Paling Sederhana Menggunakan Mata Telanjang Atau Lensa." },
  { "en": "Apa Itu Uji Phased Array (Phased Array UT)?", "id": "Uji Ultrasonik Canggih Menggunakan Banyak Elemen (Sensor) Yang Dapat Diprogram." },
  { "en": "Apa Itu Uji Time-of-Flight Diffraction (TOFD)?", "id": "Uji Ultrasonik Canggih Mengukur Waktu Difraksi Gelombang Untuk Tinggi Cacat." },
  { "en": "Apa Itu Korelasi (Correlation)?", "id": "Ukuran Kekuatan Dan Arah Hubungan Linear Antara Dua Variabel." },
  { "en": "Apa Itu Koefisien Korelasi Pearson?", "id": "Koefisien Statistik Mengukur Kekuatan Hubungan Linear Antara Variabel." },
  { "en": "Apa Itu Korelasi Positif?", "id": "Kedua Variabel Bergerak Ke Arah Yang Sama (Satu Naik, Lainnya Naik)." },
  { "en": "Apa Itu Korelasi Negatif?", "id": "Kedua Variabel Bergerak Berlawanan Arah (Satu Naik, Lainnya Turun)." },
  { "en": "Apa Itu Regresi (Regression) Statistik?", "id": "Metode Statistik Untuk Memprediksi Nilai Satu Variabel Berdasarkan Variabel Lain." },
  { "en": "Apa Itu Variabel Bebas (Independent Variable)?", "id": "Variabel Input Yang Digunakan Untuk Membuat Prediksi." },
  { "en": "Apa Itu Variabel Terikat (Dependent Variable)?", "id": "Variabel Output Yang Nilainya Diprediksi." },
  { "en": "Apa Itu Regresi Linear (Linear Regression)?", "id": "Model Regresi Mencari Hubungan Garis Lurus Terbaik." },
  { "en": "Apa Itu Koefisien Determinasi (R-squared)?", "id": "Ukuran Seberapa Baik Model Regresi Menjelaskan Variasi Data." },
  { "en": "Apa Itu Regresi Logistik (Logistic Regression)?", "id": "Model Regresi Digunakan Untuk Masalah Klasifikasi Biner (Ya/Tidak)." },
  { "en": "Apa Itu Kecerdasan Buatan (Artificial Intelligence/AI)?", "id": "Simulasi Proses Kecerdasan Manusia Oleh Mesin." },
  { "en": "Apa Itu Pembelajaran Mesin (Machine Learning/ML)?", "id": "Cabang AI Membuat Mesin Belajar Dari Data." },
  { "en": "Apa Itu Pembelajaran Mendalam (Deep Learning/DL)?", "id": "Subset ML Menggunakan Jaringan Saraf Tiruan Dalam." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (Artificial Neural Network/ANN)?", "id": "Model Komputasi Terinspirasi Struktur Otak Biologis." },
  { "en": "Apa Itu Pembelajaran Terbimbing (Supervised Learning)?", "id": "Model Dilatih Menggunakan Data Berlabel Pasangan Input Output." },
  { "en": "Apa Contoh Masalah Pembelajaran Terbimbing?", "id": "Klasifikasi Dan Regresi." },
  { "en": "Apa Itu Klasifikasi (Classification)?", "id": "Tugas ML Memprediksi Label Kategori Diskrit Ya/Tidak, A/B." },
  { "en": "Apa Itu Regresi (Regression) ML?", "id": "Tugas ML Memprediksi Nilai Numerik Kontinu Harga, Suhu." },
  { "en": "Apa Itu Pembelajaran Tanpa Terbimbing (Unsupervised Learning)?", "id": "Model Dilatih Menggunakan Data Tidak Berlabel Mencari Pola Tersembunyi." },
  { "en": "Apa Contoh Masalah Pembelajaran Tanpa Terbimbing?", "id": "Pengelompokan Clustering Dan Asosiasi." },
  { "en": "Apa Itu Pengelompokan (Clustering)?", "id": "Tugas ML Mengelompokkan Data Serupa Menjadi Cluster." },
  { "en": "Apa Contoh Algoritma Pengelompokan?", "id": "K-Means, DBSCAN, Hierarchical Clustering." },
  { "en": "Apa Itu Pembelajaran Penguatan (Reinforcement Learning/RL)?", "id": "Model Belajar Membuat Keputusan Melalui Uji Coba Dan Imbalan Hukuman." },
  { "en": "Apa Itu Agen (Agent) RL?", "id": "Entitas Yang Membuat Keputusan Dan Berinteraksi Dengan Lingkungan." },
  { "en": "Apa Itu Lingkungan (Environment) RL?", "id": "Sistem Tempat Agen Beroperasi Dan Menerima Imbalan." },
  { "en": "Apa Itu Imbalan (Reward) RL?", "id": "Sinyal Umpan Balik Positif Yang Diterima Agen Untuk Tindakan Baik." },
  { "en": "Apa Itu Kebijakan (Policy) RL?", "id": "Aturan Yang Digunakan Agen Untuk Memilih Tindakan Di Setiap Status." },
  { "en": "Apa Itu Model K-Nearest Neighbors (KNN)?", "id": "Algoritma Klasifikasi Berbasis Jarak K Tetangga Terdekat." },
  { "en": "Apa Itu Model Support Vector Machine (SVM)?", "id": "Algoritma Klasifikasi Mencari Hyperplane Pemisah Terbaik." },
  { "en": "Apa Itu Pohon Keputusan (Decision Tree)?", "id": "Model Klasifikasi Regresi Berbentuk Struktur Pohon Pertanyaan Ya/Tidak." },
  { "en": "Apa Itu Hutan Acak (Random Forest)?", "id": "Ensemble Learning Menggunakan Banyak Pohon Keputusan Mengambil Rata-Rata." },
  { "en": "Apa Itu Gradient Boosting (Gradient Boosting)?", "id": "Teknik Ensemble Membangun Pohon Secara Sekuensial Memperbaiki Kesalahan." },
  { "en": "Apa Itu XGBoost (Extreme Gradient Boosting)?", "id": "Implementasi Gradient Boosting Yang Sangat Cepat Dan Efisien." },
  { "en": "Apa Itu Neuron (Neuron) ANN?", "id": "Unit Dasar Jaringan Saraf Tiruan Menerima Input Menghasilkan Output." },
  { "en": "Apa Itu Bobot (Weight) Neuron?", "id": "Parameter Yang Disesuaikan Selama Pelatihan Mengatur Kekuatan Input." },
  { "en": "Apa Itu Bias (Bias) Neuron?", "id": "Nilai Konstanta Ditambahkan Ke Total Input Neuron Menggeser Fungsi Aktivasi." },
  { "en": "Apa Itu Fungsi Aktivasi (Activation Function)?", "id": "Fungsi Matematika Menentukan Output Neuron Berdasarkan Total Inputnya." },
  { "en": "Apa Contoh Fungsi Aktivasi?", "id": "Sigmoid, ReLU Rectified Linear Unit, Tanh." },
  { "en": "Apa Itu Lapisan Input (Input Layer)?", "id": "Lapisan ANN Menerima Data Mentah Input Model." },
  { "en": "Apa Itu Lapisan Tersembunyi (Hidden Layer)?", "id": "Lapisan ANN Melakukan Perhitungan Kompleks Ekstraksi Fitur." },
  { "en": "Apa Itu Lapisan Output (Output Layer)?", "id": "Lapisan ANN Menghasilkan Hasil Akhir Prediksi Model." },
  { "en": "Apa Itu Propagasi Maju (Forward Propagation)?", "id": "Proses Data Input Bergerak Dari Lapisan Input Ke Lapisan Output." },
  { "en": "Apa Itu Propagasi Balik (Backpropagation)?", "id": "Algoritma Menyebarkan Kesalahan Mundur Mengoptimalkan Bobot." },
  { "en": "Apa Itu Fungsi Kerugian (Loss Function/Cost Function)?", "id": "Mengukur Seberapa Buruk Prediksi Model Dibandingkan Nilai Sebenarnya." },
  { "en": "Apa Itu Pengoptimal (Optimizer) ML?", "id": "Algoritma Mengubah Bobot Dan Bias Mengurangi Fungsi Kerugian." },
  { "en": "Apa Contoh Algoritma Pengoptimal?", "id": "Stochastic Gradient Descent SGD, Adam, RMSprop." },
  { "en": "Apa Itu Tingkat Pembelajaran (Learning Rate)?", "id": "Ukuran Langkah Yang Diambil Pengoptimal Saat Mengubah Bobot." },
  { "en": "Apa Itu Overfitting (Overfitting)?", "id": "Model Belajar Terlalu Detail Data Pelatihan Gagal Pada Data Baru." },
  { "en": "Apa Itu Underfitting (Underfitting)?", "id": "Model Terlalu Sederhana Gagal Menangkap Pola Utama Data." },
  { "en": "Apa Itu Regulasi (Regularization)?", "id": "Teknik Mencegah Overfitting Dengan Menambahkan Penalti Ke Fungsi Kerugian." },
  { "en": "Apa Itu Dropout (Dropout)?", "id": "Teknik Regulasi Secara Acak Mematikan Neuron Selama Pelatihan." },
  { "en": "Apa Itu Validasi Silang (Cross-Validation)?", "id": "Teknik Menguji Model Pada Berbagai Subset Data Mengukur Kinerja." },
  { "en": "Apa Itu Matriks Kebingungan (Confusion Matrix)?", "id": "Tabel Mengukur Kinerja Model Klasifikasi True Positive, False Negative." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Rasio Prediksi Benar Terhadap Total Prediksi." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Rasio True Positive Terhadap Total Prediksi Positif True Positive + False Positive." },
  { "en": "Apa Itu Recall (Recall)?", "id": "Rasio True Positive Terhadap Total Nilai Positif Aktual True Positive + False Negative." },
  { "en": "Apa Itu Skor F1 (F1-Score)?", "id": "Rata-Rata Harmonik Antara Presisi Dan Recall." },
  { "en": "Apa Itu Kurva ROC (Receiver Operating Characteristic)?", "id": "Plot Menggambarkan Kinerja Model Klasifikasi Pada Semua Ambang Batas." },
  { "en": "Apa Itu AUC (Area Under the Curve)?", "id": "Area Di Bawah Kurva ROC Ukuran Kualitas Model Klasifikasi." },
  { "en": "Apa Itu Jaringan Saraf Konvolusional (CNN)?", "id": "ANN Khusus Dirancang Untuk Memproses Data Berbentuk Grid Gambar." },
  { "en": "Apa Itu Lapisan Konvolusional (Convolutional Layer)?", "id": "Lapisan CNN Menerapkan Filter Kernel Untuk Ekstraksi Fitur Gambar." },
  { "en": "Apa Itu Pooling (Pooling) CNN?", "id": "Lapisan CNN Mengurangi Dimensi Gambar Max Pooling, Average Pooling." },
  { "en": "Apa Itu Jaringan Saraf Berulang (Recurrent Neural Network/RNN)?", "id": "ANN Dirancang Untuk Memproses Data Sekuensial Teks, Deret Waktu." },
  { "en": "Apa Itu LSTM (Long Short-Term Memory)?", "id": "Tipe RNN Yang Mampu Mengatasi Masalah Gradien Hilang Memori Jangka Panjang." },
  { "en": "Apa Itu NLP (Natural Language Processing)?", "id": "Bidang AI Mengaktifkan Komputer Memahami Bahasa Manusia." },
  { "en": "Apa Itu Tokenisasi (Tokenization)?", "id": "Proses Membagi Teks Menjadi Unit Kecil Kata, Frasa, Simbol." },
  { "en": "Apa Itu Embedding Kata (Word Embedding)?", "id": "Representasi Kata Dalam Ruang Vektor Numerik Makna Mirip Dekat." },
  { "en": "Apa Itu Transformator (Transformer) AI?", "id": "Arsitektur Model Sekuensial Menggunakan Mekanisme Perhatian Attention." },
  { "en": "Apa Itu Mekanisme Perhatian (Attention Mechanism)?", "id": "Fitur Model Transformer Memungkinkan Fokus Pada Bagian Input Paling Relevan." },
  { "en": "Apa Itu Model Bahasa Besar (Large Language Model/LLM)?", "id": "Model DL Sangat Besar Dilatih Pada Data Teks Masif Memahami Dan Menghasilkan Bahasa." },
  { "en": "Apa Itu Transfer Learning (Pembelajaran Transfer)?", "id": "Menggunakan Model Yang Sudah Dilatih Sebelumnya Sebagai Titik Awal Tugas Baru." },
  { "en": "Apa Itu Fine-Tuning (Penyetelan Halus)?", "id": "Melatih Model Pra-Terlatih Sedikit Lebih Lanjut Dengan Data Khusus Tugas Baru." },
  { "en": "Apa Itu Bias Algoritma (Algorithmic Bias)?", "id": "Kecenderungan Tidak Adil Atau Diskriminatif Dalam Hasil Model AI." },
  { "en": "Apa Itu Tata Kelola AI (AI Governance)?", "id": "Sistem Dan Proses Memastikan AI Dikembangkan Dan Digunakan Secara Etis Aman." },
  { "en": "Apa Itu Etika AI (AI Ethics)?", "id": "Studi Tentang Implikasi Moral Dari Sistem Dan Aplikasi AI." },
  { "en": "Apa Itu Penjelasan AI (Explainable AI/XAI)?", "id": "Memastikan Model AI Dapat Menjelaskan Alasannya Membuat Prediksi Tertentu." },
  { "en": "Apa Itu Serangan Adversarial (Adversarial Attack)?", "id": "Input Yang Sedikit Dimodifikasi Untuk Mengelabui Model AI." },
  { "en": "Apa Itu Data Sintetis (Synthetic Data)?", "id": "Data Yang Dibuat Secara Buatan Bukan Dikumpulkan Dari Dunia Nyata." },
  { "en": "Apa Itu Gudang Data (Data Warehouse)?", "id": "Sistem Database Terpusat Menyimpan Data Historis Terstruktur." },
  { "en": "Apa Tujuan Gudang Data (Data Warehouse)?", "id": "Analisis Bisnis Dan Pelaporan (Business Intelligence)." },
  { "en": "Apa Itu Danau Data (Data Lake)?", "id": "Penyimpanan Terpusat Menyimpan Semua Data (Terstruktur/Tidak) Skala Besar." },
  { "en": "Apa Beda Gudang Data (Data Warehouse) Dan Danau Data (Data Lake)?", "id": "Warehouse (Data Terstruktur, Skema Jelas), Lake (Data Mentah, Fleksibel)." },
  { "en": "Apa Itu ETL (Extract, Transform, Load)?", "id": "Proses Ekstraksi, Transformasi, Pemuatan Data Ke Gudang Data." },
  { "en": "Apa Itu ELT (Extract, Load, Transform)?", "id": "Proses Ekstraksi, Pemuatan Data (Ke Data Lake), Lalu Transformasi." },
  { "en": "Apa Itu Intelijen Bisnis (Business Intelligence/BI)?", "id": "Proses Berbasis Teknologi Menganalisis Data Menyajikan Informasi." },
  { "en": "Apa Itu Dasbor (Dashboard) BI?", "id": "Tampilan Visual Interaktif Meringkas Metrik Dan KPI Bisnis." },
  { "en": "Apa Itu KPI (Key Performance Indicator)?", "id": "Indikator Kinerja Utama Ukuran Terukur Pencapaian Target." },
  { "en": "Apa Itu Visualisasi Data (Data Visualization)?", "id": "Representasi Data Dalam Bentuk Grafik Peta Atau Diagram." },
  { "en": "Apa Itu Data Besar (Big Data)?", "id": "Kumpulan Data Sangat Besar Kompleks Cepat (3V)." },
  { "en": "Apa Itu 3V (Tiga V) Big Data?", "id": "Volume (Ukuran Besar), Velocity (Kecepatan Tinggi), Variety (Ragam Jenis)." },
  { "en": "Apa Itu Apache Hadoop (Hadoop)?", "id": "Kerangka Kerja Perangkat Lunak Ekosistem Pemrosesan Big Data Terdistribusi." },
  { "en": "Apa Itu HDFS (Hadoop Distributed File System)?", "id": "Sistem File Terdistribusi Komponen Penyimpanan Utama Hadoop." },
  { "en": "Apa Itu MapReduce (MapReduce)?", "id": "Model Pemrograman Hadoop Memproses Data Besar Secara Paralel." },
  { "en": "Apa Itu Apache Spark (Spark)?", "id": "Kerangka Kerja Pemrosesan Big Data Alternatif Hadoop (Lebih Cepat, In-Memory)." },
  { "en": "Apa Keunggulan Spark (Spark) Dari MapReduce?", "id": "Pemrosesan Dalam Memori (In-Memory) Jauh Lebih Cepat." },
  { "en": "Apa Itu Pemrosesan Aliran (Stream Processing)?", "id": "Memproses Data Secara Real-Time Saat Data Datang Terus Menerus." },
  { "en": "Apa Itu Pemrosesan Batch (Batch Processing)?", "id": "Memproses Data Dalam Kumpulan Besar Batch Terjadwal." },
  { "en": "Apa Itu Apache Kafka (Kafka)?", "id": "Platform Streaming Pesan Terdistribusi (Event Streaming)." },
  { "en": "Apa Itu Pialang Pesan (Message Broker)?", "id": "Perantara Menerima Menyimpan Meneruskan Pesan Antar Aplikasi." },
  { "en": "Apa Itu Model Pub/Sub (Publish-Subscribe)?", "id": "Pola Pesan Pengirim (Publisher) Mengirim Pesan Ke Topik." },
  { "en": "Apa Itu Topik (Topic) Kafka?", "id": "Kategori Saluran Tempat Pesan Dipublikasikan." },
  { "en": "Apa Itu Produsen (Producer) Kafka?", "id": "Aplikasi Yang Menulis Pesan Ke Topik Kafka." },
  { "en": "Apa Itu Konsumen (Consumer) Kafka?", "id": "Aplikasi Yang Membaca Pesan Dari Topik Kafka." },
  { "en": "Apa Itu Sistem Operasi (Operating System/OS)?", "id": "Perangkat Lunak Mengelola Sumber Daya Perangkat Keras Komputer." },
  { "en": "Apa Itu Kernel (Kernel) Sistem Operasi?", "id": "Inti Utama Sistem Operasi Menjembatani Hardware Dan Software." },
  { "en": "Apa Itu Proses (Process) OS?", "id": "Instansi Program Yang Sedang Dieksekusi Sistem Operasi." },
  { "en": "Apa Itu Thread (Thread) OS?", "id": "Unit Eksekusi Terkecil Dalam Suatu Proses." },
  { "en": "Apa Beda Proses (Process) Dan Thread (Thread)?", "id": "Proses Punya Memori Sendiri, Thread Berbagi Memori Proses Induk." },
  { "en": "Apa Itu Penjadwalan CPU (CPU Scheduling)?", "id": "Proses OS Memilih Proses Mana Yang Akan Dijalankan CPU." },
  { "en": "Apa Itu Penjadwalan Preemptive (Preemptive)?", "id": "OS Dapat Menginterupsi Proses Yang Sedang Berjalan Paksa." },
  { "en": "Apa Itu Penjadwalan Non-Preemptive (Non-Preemptive)?", "id": "Proses Berjalan Sampai Selesai Atau Blokir Sukarela." },
  { "en": "Apa Itu Algoritma FCFS (First-Come, First-Served)?", "id": "Penjadwalan Non-Preemptive Proses Pertama Datang Dilayani Dulu." },
  { "en": "Apa Itu Algoritma SJF (Shortest Job First)?", "id": "Penjadwalan Proses Dengan Waktu Eksekusi Terpendek Dulu." },
  { "en": "Apa Itu Algoritma Round Robin (Round Robin)?", "id": "Penjadwalan Preemptive Setiap Proses Dapat Jatah Waktu Tetap." },
  { "en": "Apa Itu Quantum Waktu (Time Quantum) Round Robin?", "id": "Jatah Waktu Tetap Yang Diberikan Untuk Setiap Proses." },
  { "en": "Apa Itu Deadlock (Kebuntuan) OS?", "id": "Kondisi Dua Proses Saling Menunggu Sumber Daya Lain." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Mekanisme Sinkronisasi Mencegah Akses Bersamaan (Satu Thread)." },
  { "en": "Apa Itu Semaphore (Semaphore)?", "id": "Mekanisme Sinkronisasi Mengizinkan Sejumlah Thread Akses." },
  { "en": "Apa Itu Memori Virtual (Virtual Memory)?", "id": "Teknik OS Menggunakan Disk Sebagai Ekstensi Memori RAM." },
  { "en": "Apa Itu Paging (Paging) Memori?", "id": "Membagi Memori Virtual Menjadi Blok Ukuran Tetap (Page)." },
  { "en": "Apa Itu Kesalahan Halaman (Page Fault)?", "id": "Interupsi Terjadi Saat Halaman Diminta Tidak Ada Di RAM." },
  { "en": "Apa Itu Swap Space (Ruang Swap)?", "id": "Area Di Disk Yang Digunakan Untuk Memori Virtual." },
  { "en": "Apa Itu Thrashing (Thrashing) Memori?", "id": "Kondisi Sistem Sibuk Menukar Halaman (Page Fault Terus Menerus)." },
  { "en": "Apa Itu Sistem File (File System)?", "id": "Struktur Cara OS Mengatur Menyimpan Mengakses File Di Disk." },
  { "en": "Apa Itu FAT32 (File Allocation Table 32)?", "id": "Sistem File Lama Kompatibilitas Tinggi Batas Ukuran 4GB." },
  { "en": "Apa Itu NTFS (New Technology File System)?", "id": "Sistem File Modern Windows (Keamanan, Jurnal)." },
  { "en": "Apa Itu Ext4 (Fourth Extended Filesystem)?", "id": "Sistem File Standar Populer Untuk Linux." },
  { "en": "Apa Itu Jurnaling (Journaling) Sistem File?", "id": "Mencatat Perubahan Log Sebelum Dilakukan (Mencegah Korupsi)." },
  { "en": "Apa Itu Fragmentasi (Fragmentation) Disk?", "id": "Kondisi File Terpecah Menjadi Bagian Kecil Di Lokasi Berbeda." },
  { "en": "Apa Itu Defragmentasi (Defragmentation)?", "id": "Proses Menyusun Ulang File Agar Kontinu Di Disk." },
  { "en": "Apa Itu Panggilan Sistem (System Call)?", "id": "Cara Program Aplikasi Meminta Layanan Dari Kernel OS." },
  { "en": "Apa Itu Shell (Shell) OS?", "id": "Antarmuka Pengguna (CLI Atau GUI) Berinteraksi Dengan OS." },
  { "en": "Apa Itu BIOS (Basic Input/Output System)?", "id": "Firmware Di Motherboard Melakukan Inisialisasi Hardware Saat Boot." },
  { "en": "Apa Itu UEFI (Unified Extensible Firmware Interface)?", "id": "Pengganti BIOS Modern (Antarmuka Grafis, Boot Cepat)." },
  { "en": "Apa Itu Proses Boot (Boot Process)?", "id": "Urutan Proses Menyalakan Komputer Memuat Sistem Operasi." },
  { "en": "Apa Itu Bootloader (Bootloader)?", "id": "Program Kecil Memuat Kernel Sistem Operasi Ke Memori." },
  { "en": "Apa Itu MBR (Master Boot Record)?", "id": "Sektor Boot Pertama Di Hard Drive (Metode Booting Lama)." },
  { "en": "Apa Itu GPT (GUID Partition Table)?", "id": "Standar Partisi Modern (Pengganti MBR) (Dukung Disk Besar)." },
  { "en": "Apa Itu Driver (Driver) Perangkat Keras?", "id": "Perangkat Lunak Memungkinkan OS Berkomunikasi Dengan Hardware." },
  { "en": "Apa Itu Kernel Monolitik (Monolithic Kernel)?", "id": "Seluruh Layanan OS Berjalan Dalam Satu Ruang Kernel (Linux)." },
  { "en": "Apa Itu Kernel Mikro (Microkernel)?", "id": "Kernel Hanya Menyediakan Fungsi Paling Dasar (Pesan)." },
  { "en": "Apa Itu Sandbox (Sandbox)?", "id": "Lingkungan Terisolasi Menjalankan Program (Keamanan)." },
  { "en": "Apa Itu Hyper-Threading (Hyper-Threading)?", "id": "Teknologi Intel (Satu Inti Fisik) Bertindak (Dua Inti Logis)." },
  { "en": "Apa Itu Arsitektur 64-bit (64-bit)?", "id": "Arsitektur CPU Menggunakan Register Alamat 64-bit (RAM > 4GB)." },
  { "en": "Apa Itu Arsitektur 32-bit (32-bit)?", "id": "Arsitektur CPU Menggunakan Register Alamat 32-bit (RAM <= 4GB)." },
  { "en": "Apa Itu Komputasi Klien-Server (Client-Server)?", "id": "Model Klien Meminta Sumber Daya Server Menyediakan." },
  { "en": "Apa Itu Komputasi Peer-to-Peer (P2P)?", "id": "Model Setiap Komputer Bertindak Klien Dan Server." },
  { "en": "Apa Itu Socket (Soket) Jaringan?", "id": "Endpoint Komunikasi (Kombinasi IP Address Dan Port)." },
  { "en": "Apa Itu Protokol HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Aplikasi Web (Permintaan Dan Respon Teks)." },
  { "en": "Apa Itu Kode Status HTTP 200 (OK)?", "id": "Respon Sukses Permintaan Berhasil Diproses." },
  { "en": "Apa Itu Kode Status HTTP 404 (Not Found)?", "id": "Respon Kesalahan Sumber Daya Yang Diminta Tidak Ditemukan." },
  { "en": "Apa Itu Kode Status HTTP 500 (Internal Server Error)?", "id": "Respon Kesalahan Terjadi Masalah Internal Di Server." },
  { "en": "Apa Itu Metode HTTP GET (GET)?", "id": "Metode HTTP Meminta Mengambil Data Dari Server." },
  { "en": "Apa Itu Metode HTTP POST (POST)?", "id": "Metode HTTP Mengirim Data Baru Ke Server." },
  { "en": "Apa Itu Metode HTTP PUT (PUT)?", "id": "Metode HTTP Memperbarui Data Yang Sudah Ada Di Server." },
  { "en": "Apa Itu Metode HTTP DELETE (DELETE)?", "id": "Metode HTTP Menghapus Data Di Server." },
  { "en": "Apa Itu Cookie (Cookie) HTTP?", "id": "Data Kecil Disimpan Di Klien (Browser) Oleh Server." },
  { "en": "Apa Itu Sesi (Session) HTTP?", "id": "Mekanisme Server Melacak Interaksi Pengguna (Menyimpan Status)." },
  { "en": "Apa Itu HTTPS (HTTP Secure)?", "id": "Versi Aman HTTP Menggunakan Enkripsi SSL/TLS." },
  { "en": "Apa Itu SSL/TLS (Secure Sockets Layer/Transport Layer Security)?", "id": "Protokol Kriptografi Memberikan Keamanan Komunikasi Jaringan." },
  { "en": "Apa Itu Sertifikat SSL/TLS (SSL/TLS Certificate)?", "id": "Sertifikat Digital Mengautentikasi Identitas Situs Web." },
  { "en": "Apa Itu Otoritas Sertifikat (Certificate Authority/CA)?", "id": "Organisasi Terpercaya Menerbitkan Sertifikat SSL/TLS." },
  { "en": "Apa Itu FTP (File Transfer Protocol)?", "id": "Protokol Standar Transfer File Antar Komputer." },
  { "en": "Apa Itu SFTP (SSH File Transfer Protocol)?", "id": "FTP Aman Yang Berjalan Di Atas Protokol SSH." },
  { "en": "Apa Itu SSH (Secure Shell)?", "id": "Protokol Jaringan Kriptografi (Akses Terminal Aman)." },
  { "en": "Apa Itu SMTP (Simple Mail Transfer Protocol)?", "id": "Protokol Standar Mengirim Email." },
  { "en": "Apa Itu POP3 (Post Office Protocol 3)?", "id": "Protokol Mengunduh Mengambil Email Dari Server." },
  { "en": "Apa Itu IMAP (Internet Message Access Protocol)?", "id": "Protokol Mengakses Email Di Server (Sinkronisasi)." },
  { "en": "Apa Itu VoIP (Voice over IP)?", "id": "Teknologi Transmisi Suara Percakapan Melalui Jaringan IP." },
  { "en": "Apa Itu SIP (Session Initiation Protocol)?", "id": "Protokol Pensinyalan Populer Digunakan Untuk VoIP." },
  { "en": "Apa Itu RTP (Real-time Transport Protocol)?", "id": "Protokol Mengirimkan Data Audio Video Real-time (VoIP)." },
  { "en": "Apa Itu Jitter (Jitter) Jaringan?", "id": "Variasi Waktu Tunda Kedatangan Paket (Masalah VoIP)." },
  { "en": "Apa Itu Latensi (Latency) Jaringan?", "id": "Waktu Tunda Data Berpindah Dari Sumber Ke Tujuan." },
  { "en": "Apa Itu Bandwidth (Bandwidth)?", "id": "Kapasitas Maksimal Transfer Data Suatu Jaringan." },
  { "en": "Apa Itu Throughput (Throughput)?", "id": "Kecepatan Transfer Data Aktual Yang Berhasil Dicapai." },
  { "en": "Apa Itu Paket (Packet) Jaringan?", "id": "Unit Data Kecil Yang Dikirim Melalui Jaringan." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Label Numerik Unik Mengidentifikasi Perangkat Di Jaringan." },
  { "en": "Apa Itu Subnet Mask (Subnet Mask)?", "id": "Membagi Alamat IP Menjadi Alamat Jaringan Dan Alamat Host." },
  { "en": "Apa Itu Default Gateway (Gateway Default)?", "id": "Alamat Router Jalur Keluar Jaringan Lokal Ke Internet." },
  { "en": "Apa Itu NAT (Network Address Translation)?", "id": "Mengubah Alamat IP Privat Menjadi Satu Alamat IP Publik." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Transistor Efek Medan Dikontrol Tegangan Gerbang." },
  { "en": "Apa Itu Mode Peningkatan (Enhancement Mode) MOSFET?", "id": "Transistor Normal Mati (Off) Saat Tegangan Gerbang Nol." },
  { "en": "Apa Itu Mode Deplesi (Depletion Mode) MOSFET?", "id": "Transistor Normal Hidup (On) Saat Tegangan Gerbang Nol." },
  { "en": "Apa Itu MOSFET Saluran-N (N-Channel)?", "id": "MOSFET Menggunakan Elektron Sebagai Pembawa Muatan Utama." },
  { "en": "Apa Itu MOSFET Saluran-P (P-Channel)?", "id": "MOSFET Menggunakan Lubang (Hole) Sebagai Pembawa Muatan Utama." },
  { "en": "Apa Itu Rds(on) (Resistansi Drain-Source On)?", "id": "Resistansi Internal MOSFET Saat Kondisi On Penuh." },
  { "en": "Apa Itu Tegangan Ambang (Threshold Voltage/Vth)?", "id": "Tegangan Gerbang Minimum Mulai Menghidupkan MOSFET." },
  { "en": "Apa Itu Kapasitansi Gerbang (Gate Capacitance)?", "id": "Kapasitansi Input Gerbang (Membutuhkan Arus Pengisian)." },
  { "en": "Apa Itu Penggerak Gerbang (Gate Driver)?", "id": "Sirkuit IC Menyediakan Arus Kuat Mengisi Gate Cepat." },
  { "en": "Apa Itu Dataran Tinggi Miller (Miller Plateau)?", "id": "Kondisi Tegangan Gerbang Datar Saat Switching." },
  { "en": "Apa Itu Dioda Badan (Body Diode) MOSFET?", "id": "Dioda Parasitik Internal Antara Drain Dan Source." },
  { "en": "Apa Fungsi Dioda Badan (Body Diode)?", "id": "Berfungsi Sebagai Dioda Freewheeling Pada Beban Induktif." },
  { "en": "Apa Itu SOA (Safe Operating Area)?", "id": "Grafik Batas Aman Operasi Arus Tegangan Transistor." },
  { "en": "Apa Itu IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Gabungan Input MOSFET (Tegangan) Output BJT (Arus)." },
  { "en": "Apa Keunggulan IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Switching Cepat (MOSFET) Arus Tinggi (BJT)." },
  { "en": "Apa Aplikasi IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Inverter Daya Tinggi, VFD Motor, Pengelasan." },
  { "en": "Apa Itu Penjepitan (Clamping) Tegangan Gerbang?", "id": "Menggunakan Zener Melindungi Gerbang Dari Tegangan Lebih." },
  { "en": "Apa Itu Waktu Mati (Dead Time)?", "id": "Tundaan Waktu Mencegah Saklar High-Side Low-Side On Bersamaan." },
  { "en": "Apa Itu Tembak-Tembus (Shoot-Through)?", "id": "Kondisi Bencana Saklar High-Side Low-Side On Bersamaan." },
  { "en": "Apa Itu Penyearah Sinkron (Synchronous Rectifier)?", "id": "Mengganti Dioda Penyearah Dengan MOSFET (Efisiensi Tinggi)." },
  { "en": "Apa Itu Konverter Buck (Buck Converter)?", "id": "Topologi SMPS Menurunkan Tegangan DC (Step-Down)." },
  { "en": "Apa Itu Konverter Boost (Boost Converter)?", "id": "Topologi SMPS Menaikkan Tegangan DC (Step-Up)." },
  { "en": "Apa Itu Konverter Buck-Boost (Buck-Boost Converter)?", "id": "Topologi SMPS Membalik Polaritas Tegangan (Naik/Turun)." },
  { "en": "Apa Itu Mode Konduksi Kontinu (CCM)?", "id": "Arus Induktor SMPS Tidak Pernah Turun Ke Nol." },
  { "en": "Apa Itu Mode Konduksi Diskontinu (DCM)?", "id": "Arus Induktor SMPS Turun Ke Nol Setiap Siklus." },
  { "en": "Apa Itu Konverter Flyback (Flyback Converter)?", "id": "Konverter Terisolasi (Topologi Buck-Boost) Menyimpan Energi." },
  { "en": "Apa Itu Konverter Forward (Forward Converter)?", "id": "Konverter Terisolasi (Topologi Buck) Transfer Energi Langsung." },
  { "en": "Apa Itu Trafo Planar (Planar Transformer)?", "id": "Trafo Frekuensi Tinggi Menggunakan Lilitan PCB Datar." },
  { "en": "Apa Itu Kontrol Mode Tegangan (Voltage Mode Control)?", "id": "Kontrol SMPS Menggunakan Umpan Balik Tegangan Output." },
  { "en": "Apa Itu Kontrol Mode Arus (Current Mode Control)?", "id": "Kontrol SMPS Menggunakan Umpan Balik Arus Induktor." },
  { "en": "Apa Itu Respon Loop (Loop Response) Catu Daya?", "id": "Seberapa Cepat Catu Daya Merespon Perubahan Beban." },
  { "en": "Apa Itu Analisis Plot Bode (Bode Plot)?", "id": "Menganalisis Stabilitas (Margin Fasa/Gain) Catu Daya." },
  { "en": "Apa Itu Margin Fasa (Phase Margin)?", "id": "Ukuran Stabilitas Loop Kontrol (Harus Positif)." },
  { "en": "Apa Itu Margin Penguatan (Gain Margin)?", "id": "Ukuran Lain Stabilitas Loop Kontrol." },
  { "en": "Apa Itu LDO (Low-Dropout Regulator)?", "id": "Regulator Linear Tegangan Jatuh Input-Output Sangat Rendah." },
  { "en": "Apa Itu Arus Diam (Quiescent Current/Iq)?", "id": "Arus Konsumsi Internal Regulator Saat Tanpa Beban." },
  { "en": "Apa Itu PSRR (Power Supply Rejection Ratio)?", "id": "Kemampuan Regulator Menolak Riak (Noise) Dari Input." },
  { "en": "Apa Itu Regulasi Saluran (Line Regulation)?", "id": "Ukuran Perubahan Output Akibat Perubahan Input." },
  { "en": "Apa Itu Regulasi Beban (Load Regulation)?", "id": "Ukuran Perubahan Output Akibat Perubahan Beban." },
  { "en": "Apa Itu Referensi Celah Pita (Bandgap Reference)?", "id": "Sirkuit Menghasilkan Tegangan Referensi Sangat Stabil Suhu." },
  { "en": "Apa Itu Penguat Kelas D (Class D Amplifier)?", "id": "Penguat Switching (PWM) Efisiensi Sangat Tinggi." },
  { "en": "Bagaimana Penguat Kelas D (Class D) Bekerja?", "id": "Mengubah Sinyal Audio Menjadi Sinyal PWM Kecepatan Tinggi." },
  { "en": "Apa Itu Filter Output (Output Filter) Kelas D?", "id": "Filter LC (Low-Pass) Mengembalikan Sinyal PWM Menjadi Audio." },
  { "en": "Apa Itu Silikon Karbida (Silicon Carbide/SiC)?", "id": "Material Semikonduktor Celah Pita Lebar (Tegangan Tinggi)." },
  { "en": "Apa Itu Gallium Nitrida (Gallium Nitride/GaN)?", "id": "Material Semikonduktor Celah Pita Lebar (Frekuensi Tinggi)." },
  { "en": "Apa Keunggulan SiC (Silicon Carbide) Dan GaN (Gallium Nitride)?", "id": "Switching Lebih Cepat, Rugi-Rugi Rendah, Tahan Panas." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Jalur PCB Frekuensi Tinggi (Harus Cocok Impedansi)." },
  { "en": "Apa Itu Impedansi Karakteristik (Characteristic Impedance)?", "id": "Impedansi Alami Saluran Transmisi (Contoh: 50 Ohm)." },
  { "en": "Apa Itu Pantulan (Reflection) Sinyal?", "id": "Sinyal Memantul Balik Akibat Ketidakcocokan Impedansi." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Membuat Impedansi Beban Sama Dengan Impedansi Saluran." },
  { "en": "Apa Itu Resistor Terminasi (Termination Resistor)?", "id": "Resistor Di Ujung Saluran Mencegah Pantulan Sinyal." },
  { "en": "Apa Itu S-Parameters (S-Parameters)?", "id": "Parameter Menggambarkan Perilaku Jaringan RF (Refleksi, Transmisi)." },
  { "en": "Apa Itu S11 (S11 Parameter)?", "id": "Koefisien Pantulan (Reflection) Port Input." },
  { "en": "Apa Itu S21 (S21 Parameter)?", "id": "Koefisien Transmisi (Gain/Loss) Port 1 Ke Port 2." },
  { "en": "Apa Itu Return Loss (Return Loss)?", "id": "Ukuran (Seberapa Kecil) Sinyal Yang Dipantulkan (dB)." },
  { "en": "Apa Itu Insertion Loss (Insertion Loss)?", "id": "Ukuran (Seberapa Besar) Pelemahan Sinyal (Akibat Komponen)." },
  { "en": "Apa Itu VNA (Vector Network Analyzer)?", "id": "Alat Uji Presisi Mengukur S-Parameters Jaringan RF." },
  { "en": "Apa Itu Penganalisis Spektrum (Spectrum Analyzer)?", "id": "Alat Uji Mengukur Kekuatan Sinyal Domain Frekuensi." },
  { "en": "Apa Itu Penganalisis Logika (Logic Analyzer)?", "id": "Alat Uji Merekam Menganalisis Banyak Sinyal Digital." },
  { "en": "Apa Itu Osiloskop Sinyal Campuran (MSO)?", "id": "Gabungan Osiloskop (Analog) Dan Penganalisis Logika (Digital)." },
  { "en": "Apa Itu Probe Diferensial (Differential Probe)?", "id": "Probe Osiloskop Mengukur Perbedaan Tegangan (Aman, Non-Ground)." },
  { "en": "Apa Itu Probe Arus (Current Probe)?", "id": "Probe Osiloskop Mengukur Arus (Bentuk Gelombang Arus)." },
  { "en": "Apa Itu Jitter (Jitter) Sinyal?", "id": "Deviasi Waktu Tepi Sinyal Digital Dari Posisi Ideal." },
  { "en": "Apa Itu Diagram Mata (Eye Diagram)?", "id": "Tampilan Osiloskop Menganalisis Kualitas Sinyal Digital (Jitter, Noise)." },
  { "en": "Apa Itu Integritas Sinyal (Signal Integrity/SI)?", "id": "Studi Menjaga Kualitas Sinyal Digital Cepat (Distorsi)." },
  { "en": "Apa Itu Integritas Daya (Power Integrity/PI)?", "id": "Studi Menjaga Kualitas Jaringan Catu Daya PCB (Noise)." },
  { "en": "Apa Itu Kapasitor Decoupling (Decoupling Capacitor)?", "id": "Kapasitor Lokal (Dekat IC) Menyediakan Arus Cepat Sesaat." },
  { "en": "Apa Itu Kapasitor Bypass (Bypass Capacitor)?", "id": "Kapasitor Menyaring Noise Frekuensi Tinggi Ke Ground." },
  { "en": "Apa Itu Induktansi Parasitik (Parasitic Inductance)?", "id": "Induktansi Liar Tidak Diinginkan Pada Jalur PCB Kaki IC." },
  { "en": "Apa Itu Kapasitansi Parasitik (Parasitic Capacitance)?", "id": "Kapasitansi Liar Tidak Diinginkan Antar Jalur PCB." },
  { "en": "Apa Itu Crosstalk (Crosstalk/Silang Bicara)?", "id": "Gangguan Sinyal Antar Jalur PCB Yang Berdekatan." },
  { "en": "Apa Itu Ground Bounce (Pantulan Tanah)?", "id": "Noise Lonjakan Tegangan Di Jalur Ground Akibat Switching Cepat." },
  { "en": "Apa Itu Pensinyalan Diferensial (Differential Signaling)?", "id": "Menggunakan Dua Jalur Sinyal Berlawanan Fasa (Tahan Noise)." },
  { "en": "Apa Itu Penolakan Mode Bersama (Common-Mode Rejection)?", "id": "Kemampuan Penerima Diferensial Menolak Noise Yang Sama (CMRR)." },
  { "en": "Apa Itu LVDS (Low-Voltage Differential Signaling)?", "id": "Standar Pensinyalan Diferensial Kecepatan Tinggi Tegangan Rendah." },
  { "en": "Apa Itu Jalur Microstrip (Microstrip Line)?", "id": "Jalur Transmisi PCB (Di Lapisan Luar) Atas Ground Plane." },
  { "en": "Apa Itu Jalur Stripline (Stripline)?", "id": "Jalur Transmisi PCB (Terkubur) Di Antara Dua Ground Plane." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant/Er)?", "id": "Sifat Material PCB Mempengaruhi Impedansi Dan Kecepatan." },
  { "en": "Apa Itu Rugi Tangen (Loss Tangent/Tan Delta)?", "id": "Ukuran Kerugian (Pelemahan) Sinyal RF Dalam Material PCB." },
  { "en": "Apa Itu EMI (Electromagnetic Interference)?", "id": "Interferensi Gangguan Elektromagnetik Dari Sumber Eksternal." },
  { "en": "Apa Itu EMC (Electromagnetic Compatibility)?", "id": "Kemampuan Alat Bekerja Normal Tanpa Mengganggu Terganggu EMI." },
  { "en": "Apa Itu Emisi (Emissions) EMI?", "id": "Pancaran Noise EMI Yang Dihasilkan Oleh Perangkat." },
  { "en": "Apa Itu Imunitas (Immunity/Susceptibility) EMI?", "id": "Kemampuan Perangkat Bertahan Terhadap Gangguan EMI." },
  { "en": "Apa Itu Emisi Terkonduksi (Conducted Emissions)?", "id": "Noise EMI Merambat Melalui Kabel Daya Atau Sinyal." },
  { "en": "Apa Itu Emisi Terpancar (Radiated Emissions)?", "id": "Noise EMI Memancar Melalui Udara Sebagai Gelombang Radio." },
  { "en": "Apa Itu LISN (Line Impedance Stabilization Network)?", "id": "Alat Uji EMC Mengukur Emisi Terkonduksi." },
  { "en": "Apa Itu Ruang Anechoic (Anechoic Chamber)?", "id": "Ruangan Kedap Pantulan RF Untuk Uji Emisi Terpancar." },
  { "en": "Apa Itu Pistol ESD (ESD Gun)?", "id": "Alat Uji Imunitas Mensimulasikan Pelepasan Muatan Listrik Statis." },
  { "en": "Apa Itu Uji EFT/Burst (Electrical Fast Transient)?", "id": "Uji Imunitas Terhadap Rentetan Lonjakan Cepat Transien." },
  { "en": "Apa Itu Uji Surja (Surge Test)?", "id": "Uji Imunitas Terhadap Lonjakan Energi Besar (Simulasi Petir)." },
  { "en": "Apa Itu Filter Saluran Daya (Power Line Filter)?", "id": "Filter (LC) Mencegah Noise EMI Masuk Keluar Lewat Kabel Daya." },
  { "en": "Apa Itu Kapasitor Kelas-X (X-Capacitor)?", "id": "Kapasitor (Across-the-Line) Meredam Noise Diferensial." },
  { "en": "Apa Itu Kapasitor Kelas-Y (Y-Capacitor)?", "id": "Kapasitor (Line-to-Ground) Meredam Noise Common Mode." },
  { "en": "Apa Itu Induktor Mode Bersama (Common Mode Choke)?", "id": "Induktor (Dua Lilitan) Meredam Noise Common Mode." },
  { "en": "Apa Itu Pelindung (Shielding) Kabel?", "id": "Lapisan (Foil/Jalinan) Konduktif Melindungi Kabel Dari Noise." },
  { "en": "Apa Itu Grounding (Pembumian) Sasis?", "id": "Menghubungkan Casing Logam Perangkat Ke Tanah (Keselamatan, EMI)." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "Chip (Logika Terprogram) Berisi Blok Logika Dan Interkoneksi." },
  { "en": "Apa Itu Blok Logika (Logic Block) FPGA?", "id": "Unit (Dasar) FPGA Berisi LUT Dan Flip-Flop." },
  { "en": "Apa Itu LUT (Look-Up Table) FPGA?", "id": "Memori (RAM) Kecil Mengimplementasikan Fungsi Logika Kombinasional." },
  { "en": "Apa Itu HDL (Hardware Description Language)?", "id": "Bahasa (Teks) Mendeskripsikan Sirkuit Digital (VHDL, Verilog)." },
  { "en": "Apa Itu Sintesis (Synthesis) HDL?", "id": "Proses (Kompilasi) Kode HDL Menjadi Jaringan Gerbang Logika." },
  { "en": "Apa Itu Penempatan Dan Perutean (Place & Route)?", "id": "Proses (Perangkat Lunak FPGA) Menempatkan Logika Merutekan Kabel." },
  { "en": "Apa Itu ASIC (Application-Specific Integrated Circuit)?", "id": "Chip (Dirancang Khusus) Permanen Untuk Satu Fungsi." },
  { "en": "Apa Beda FPGA (Field-Programmable Gate Array) Dan ASIC (Application-Specific Integrated Circuit)?", "id": "FPGA (Fleksibel, Dapat Diprogram), ASIC (Permanen, Kinerja Tinggi, Mahal NRE)." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Khusus Tertanam Dalam Perangkat Lebih Besar." },
  { "en": "Apa Contoh Sistem Tertanam (Embedded System)?", "id": "Microwave, Mesin Cuci, Sistem Kontrol Mobil." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller/MCU)?", "id": "Komputer Kecil Dalam Satu Chip (CPU, RAM, ROM, I/O)." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor/MPU)?", "id": "Hanya CPU Dalam Satu Chip (Butuh RAM, ROM Eksternal)." },
  { "en": "Apa Beda Mikrokontroler (Microcontroller) Dan Mikroprosesor (Microprocessor)?", "id": "Mikrokontroler (Sistem Lengkap), Mikroprosesor (Hanya CPU)." },
  { "en": "Apa Itu Arsitektur Harvard (Harvard Architecture)?", "id": "Arsitektur Memori Program Dan Memori Data Terpisah." },
  { "en": "Apa Itu Arsitektur Von Neumann (Von Neumann Architecture)?", "id": "Arsitektur Memori Program Dan Memori Data Bersatu." },
  { "en": "Apa Keunggulan Arsitektur Harvard?", "id": "Akses Memori Program Dan Data Bersamaan (Lebih Cepat)." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Memori Non-Volatil Menyimpan Kode Program Firmware." },
  { "en": "Apa Itu SRAM (Static RAM)?", "id": "Memori Volatil Cepat Menyimpan Variabel Data Kerja." },
  { "en": "Apa Itu EEPROM (Electrically Erasable PROM)?", "id": "Memori Non-Volatil Lambat Menyimpan Data Konfigurasi." },
  { "en": "Apa Itu Register (Register) CPU?", "id": "Memori Internal CPU Sangat Cepat (Penyimpanan Sementara)." },
  { "en": "Apa Itu Osilator (Oscillator) Mikrokontroler?", "id": "Pembangkit Sinyal Detak Jam (Clock) CPU." },
  { "en": "Apa Itu Osilator Kristal (Crystal Oscillator)?", "id": "Osilator Eksternal Menggunakan Kristal Kuarsa (Sangat Akurat)." },
  { "en": "Apa Itu Osilator Internal (Internal Oscillator)?", "id": "Osilator Bawaan Chip (Kurang Akurat, Murah)." },
  { "en": "Apa Itu Watchdog Timer (WDT)?", "id": "Timer Keamanan Mereset MCU Jika Program Macet (Hang)." },
  { "en": "Apa Fungsi 'Menendang' Watchdog (Kicking the Watchdog)?", "id": "Program Memberi Sinyal Ke WDT (Sistem Masih Berjalan Normal)." },
  { "en": "Apa Itu Brown-Out Detector (BOD)?", "id": "Sirkuit Mereset MCU Jika Tegangan Catu Daya Turun." },
  { "en": "Apa Itu Mode Tidur (Sleep Mode) Mikrokontroler?", "id": "Mode Hemat Daya Mematikan Sebagian Besar Fungsi Chip." },
  { "en": "Apa Itu Sumber Bangun (Wake-up Source)?", "id": "Sinyal Interupsi Eksternal Atau Timer Membangunkan MCU." },
  { "en": "Apa Itu GPIO (General Purpose Input/Output)?", "id": "Pin Digital Serbaguna (Bisa Input Atau Output)." },
  { "en": "Apa Itu Resistor Pull-Up (Pull-Up Resistor)?", "id": "Resistor Menarik Pin Ke VCC (Mencegah Mengambang)." },
  { "en": "Apa Itu Resistor Pull-Down (Pull-Down Resistor)?", "id": "Resistor Menarik Pin Ke Ground (Mencegah Mengambang)." },
  { "en": "Apa Itu Keadaan Mengambang (Floating State)?", "id": "Kondisi Pin Input Tidak Terhubung (Nilai Tidak Jelas)." },
  { "en": "Apa Itu Output Open-Drain (Open-Drain)?", "id": "Output Hanya Bisa Menarik Ke Ground (Butuh Pull-Up)." },
  { "en": "Apa Itu Output Push-Pull (Push-Pull)?", "id": "Output Standar (Bisa Mendorong Ke VCC, Menarik Ke Ground)." },
  { "en": "Apa Itu Interupsi (Interrupt)?", "id": "Sinyal Memotong Eksekusi Program Utama (Tugas Prioritas)." },
  { "en": "Apa Itu ISR (Interrupt Service Routine)?", "id": "Fungsi Kode Yang Dijalankan Saat Interupsi Terjadi." },
  { "en": "Apa Itu Vektor Interupsi (Interrupt Vector)?", "id": "Tabel Alamat Memori Yang Menunjuk Ke Lokasi ISR." },
  { "en": "Apa Itu Interupsi Eksternal (External Interrupt)?", "id": "Interupsi Dipicu Oleh Perubahan Sinyal Pada Pin Eksternal." },
  { "en": "Apa Itu Interupsi Timer (Timer Interrupt)?", "id": "Interupsi Dipicu Saat Timer Meluap (Overflow)." },
  { "en": "Apa Itu Timer/Counter (Pewaktu/Penghitung)?", "id": "Periferal Menghitung Siklus Clock Atau Pulsa Eksternal." },
  { "en": "Apa Itu Prescaler (Prescaler) Timer?", "id": "Pembagi Frekuensi Memperlambat Laju Hitungan Timer." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Mengatur Daya Analog Dengan Sinyal Digital." },
  { "en": "Apa Itu Siklus Kerja (Duty Cycle) PWM?", "id": "Persentase Waktu Sinyal PWM Berada Di Level Tinggi (On)." },
  { "en": "Apa Itu Frekuensi (Frequency) PWM?", "id": "Seberapa Cepat Siklus PWM Berulang Per Detik." },
  { "en": "Apa Itu Resolusi (Resolution) PWM?", "id": "Jumlah Langkah Siklus Kerja (Contoh: 8-bit, 10-bit)." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Periferal Mengubah Sinyal Analog (Tegangan) Menjadi Nilai Digital." },
  { "en": "Apa Itu Resolusi (Resolution) ADC?", "id": "Jumlah Bit Hasil Konversi Digital (Kehalusan)." },
  { "en": "Apa Arti ADC 10-bit?", "id": "Menghasilkan Nilai Digital Dari 0 Hingga 1023." },
  { "en": "Apa Itu Tegangan Referensi (Reference Voltage) ADC?", "id": "Tegangan Acuan Skala Penuh Pengukuran ADC." },
  { "en": "Apa Itu Laju Sampling (Sample Rate) ADC?", "id": "Seberapa Cepat ADC Mengambil Sampel Analog (SPS)." },
  { "en": "Apa Itu ADC Tipe SAR (Successive Approximation)?", "id": "Tipe ADC Paling Umum (Keseimbangan Kecepatan Resolusi)." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Periferal Mengubah Nilai Digital Menjadi Sinyal Analog." },
  { "en": "Apa Itu Komparator Analog (Analog Comparator)?", "id": "Periferal Membandingkan Dua Tegangan Analog (Output Digital)." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication)?", "id": "Mengirim Data Satu Bit Demi Satu Bit (Satu Jalur)." },
  { "en": "Apa Itu Komunikasi Paralel (Parallel Communication)?", "id": "Mengirim Data Banyak Bit Bersamaan (Banyak Jalur)." },
  { "en": "Apa Itu Komunikasi Sinkron (Synchronous)?", "id": "Komunikasi Serial Membutuhkan Sinyal Clock Bersama." },
  { "en": "Apa Itu Komunikasi Asinkron (Asynchronous)?", "id": "Komunikasi Serial Tidak Butuh Clock (Pakai Start/Stop Bit)." },
  { "en": "Apa Itu UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Protokol Komunikasi Serial Asinkron (TX, RX)." },
  { "en": "Apa Itu Baud Rate (Baud Rate)?", "id": "Kecepatan Transmisi Data Serial (Bit Per Detik)." },
  { "en": "Apa Itu Start Bit (Bit Awal) UART?", "id": "Sinyal Menandakan Awal Transmisi Satu Byte Data." },
  { "en": "Apa Itu Stop Bit (Bit Akhir) UART?", "id": "Sinyal Menandakan Akhir Transmisi Satu Byte Data." },
  { "en": "Apa Itu Parity Bit (Bit Paritas) UART?", "id": "Bit Tambahan Untuk Pengecekan Kesalahan Sederhana." },
  { "en": "Apa Itu Level Tegangan TTL (TTL Level) UART?", "id": "Sinyal UART Menggunakan Tegangan Logika Chip (0V, 5V/3.3V)." },
  { "en": "Apa Itu Level Tegangan RS-232 (RS-232 Level)?", "id": "Sinyal UART Standar Lama (Tegangan Tinggi +/- 12V)." },
  { "en": "Apa Itu Konverter USB-to-UART (USB-to-Serial)?", "id": "Chip (FTDI, CH340) Menjembatani USB Ke Sinyal UART TTL." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Protokol Serial Sinkron Dua Kabel (SDA, SCL) Multi-Perangkat." },
  { "en": "Apa Itu Jalur SDA (Serial Data) I2C?", "id": "Jalur Data Dua Arah Pada Bus I2C." },
  { "en": "Apa Itu Jalur SCL (Serial Clock) I2C?", "id": "Jalur Clock Yang Dibangkitkan Oleh Master I2C." },
  { "en": "Apa Itu Master (Master) I2C?", "id": "Perangkat Menginisiasi Komunikasi Membangkitkan Clock." },
  { "en": "Apa Itu Slave (Slave) I2C?", "id": "Perangkat Merespon Master (Memiliki Alamat Unik)." },
  { "en": "Apa Itu Alamat (Address) I2C?", "id": "Nomor Unik 7-bit (Atau 10-bit) Mengidentifikasi Slave." },
  { "en": "Apa Itu Kondisi Start (Start Condition) I2C?", "id": "Transisi SDA Turun Saat SCL Tinggi." },
  { "en": "Apa Itu Kondisi Stop (Stop Condition) I2C?", "id": "Transisi SDA Naik Saat SCL Tinggi." },
  { "en": "Apa Itu Bit ACK (Acknowledge) I2C?", "id": "Bit Konfirmasi (Ditarik Rendah) Oleh Penerima." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Protokol Serial Sinkron Cepat Empat Kabel (Full-Duplex)." },
  { "en": "Apa Itu Jalur MOSI (Master Out Slave In)?", "id": "Jalur Data Dari Master Ke Slave (SPI)." },
  { "en": "Apa Itu Jalur MISO (Master In Slave Out)?", "id": "Jalur Data Dari Slave Ke Master (SPI)." },
  { "en": "Apa Itu Jalur SCK (Serial Clock) SPI?", "id": "Jalur Clock Yang Dibangkitkan Master (SPI)." },
  { "en": "Apa Itu Jalur SS (Slave Select)?", "id": "Jalur Memilih Slave Mana Yang Aktif (Dikelola Master)." },
  { "en": "Apa Keunggulan SPI (Serial Peripheral Interface) Dibanding I2C (Inter-Integrated Circuit)?", "id": "Lebih Cepat, Full-Duplex, Protokol Lebih Sederhana." },
  { "en": "Apa Keunggulan I2C (Inter-Integrated Circuit) Dibanding SPI (Serial Peripheral Interface)?", "id": "Hanya Dua Kabel, Mendukung Banyak Slave (Dengan Alamat)." },
  { "en": "Apa Itu Protokol One-Wire (1-Wire)?", "id": "Protokol Komunikasi Data Dan Daya Lewat Satu Kabel." },
  { "en": "Apa Itu CAN Bus (Controller Area Network)?", "id": "Protokol Serial Kuat (Dua Kabel Diferensial) Otomotif Industri." },
  { "en": "Apa Keunggulan CAN Bus (Controller Area Network)?", "id": "Sangat Tahan Noise, Deteksi Kesalahan, Multi-Master." },
  { "en": "Apa Itu LIN Bus (Local Interconnect Network)?", "id": "Protokol Serial Biaya Rendah (Satu Kabel) Otomotif." },
  { "en": "Apa Itu Firmware (Firmware)?", "id": "Perangkat Lunak Tertanam Di Hardware (ROM/Flash)." },
  { "en": "Apa Itu Bootloader (Bootloader)?", "id": "Program Kecil Berjalan Saat Startup (Memuat Program Utama)." },
  { "en": "Apa Itu ISP (In-System Programming)?", "id": "Metode Memprogram Chip Langsung Di Papan Sirkuit." },
  { "en": "Apa Itu ICSP (In-Circuit Serial Programming)?", "id": "Nama Lain (Standar Microchip) Untuk ISP (Menggunakan SPI)." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Antarmuka Standar Pengujian PCB (Boundary Scan) Dan Debugging." },
  { "en": "Apa Itu Debugger (Debugger)?", "id": "Alat (Hardware/Software) Membantu Mencari Kesalahan (Bug) Kode." },
  { "en": "Apa Itu Breakpoint (Breakpoint)?", "id": "Titik Henti Program Disengaja Untuk Inspeksi Debug." },
  { "en": "Apa Itu Step Over (Langkahi)?", "id": "Debugging Menjalankan Satu Baris (Melompati Fungsi)." },
  { "en": "Apa Itu Step Into (Masuk Ke)?", "id": "Debugging Menjalankan Satu Baris (Masuk Ke Dalam Fungsi)." },
  { "en": "Apa Itu Step Out (Keluar Dari)?", "id": "Debugging Menjalankan Sisa Fungsi (Berhenti Setelah Keluar)." },
  { "en": "Apa Itu IDE (Integrated Development Environment)?", "id": "Perangkat Lunak Terintegrasi (Editor, Compiler, Debugger)." },
  { "en": "Apa Itu Compiler (Kompilator)?", "id": "Menerjemahkan Kode Sumber (C/C++) Menjadi Kode Mesin Biner." },
  { "en": "Apa Itu Linker (Penaut)?", "id": "Menggabungkan File Objek Dan Pustaka Menjadi File Eksekusi." },
  { "en": "Apa Itu Bahasa Assembly (Assembly)?", "id": "Bahasa Pemrograman Level Rendah Representasi Teks Kode Mesin." },
  { "en": "Apa Itu Variabel Volatile (Volatile)?", "id": "Kata Kunci Mencegah Kompiler Mengoptimasi Variabel (ISR)." },
  { "en": "Apa Itu Stack (Tumpukan) Memori?", "id": "Memori LIFO Otomatis Menyimpan Variabel Lokal Panggilan Fungsi." },
  { "en": "Apa Itu Heap (Heap) Memori?", "id": "Memori Alokasi Dinamis (Malloc/Free)." },
  { "en": "Apa Itu Stack Overflow (Tumpukan Meluap)?", "id": "Error Kehabisan Memori Stack (Rekursi Terlalu Dalam)." },
  { "en": "Apa Itu Kebocoran Memori (Memory Leak)?", "id": "Error Alokasi Memori Heap Tidak Pernah Dibebaskan (Free)." },
  { "en": "Apa Itu Endianness (Endianness)?", "id": "Urutan Penyimpanan Byte Data Multi-Byte Di Memori." },
  { "en": "Apa Itu Big Endian (Big Endian)?", "id": "Byte Paling Signifikan (MSB) Disimpan Di Alamat Terendah." },
  { "en": "Apa Itu Little Endian (Little Endian)?", "id": "Byte Paling Tidak Signifikan (LSB) Disimpan Di Alamat Terendah." },
  { "en": "Apa Itu Operator Bitwise (Bitwise Operator)?", "id": "Operator Logika Bekerja Pada Level Bit (AND, OR, XOR)." },
  { "en": "Apa Itu Bit Masking (Penopeng Bit)?", "id": "Teknik Menggunakan Operator Bitwise Mengatur Membaca Bit." },
  { "en": "Apa Itu RTOS (Real-Time Operating System)?", "id": "Sistem Operasi Respon Deterministik Waktu Nyata." },
  { "en": "Apa Itu Tugas (Task) RTOS?", "id": "Fungsi Independen Yang Dijalankan Oleh Penjadwal RTOS." },
  { "en": "Apa Itu Penjadwal (Scheduler) RTOS?", "id": "Bagian RTOS Menentukan Tugas Mana Yang Berjalan Kapan." },
  { "en": "Apa Itu Penjadwalan Preemptive (Preemptive)?", "id": "Tugas Prioritas Tinggi Dapat Menyela Tugas Prioritas Rendah." },
  { "en": "Apa Itu Mutex (Mutex) RTOS?", "id": "Objek Sinkronisasi Mencegah Akses Bersamaan (Satu Tugas)." },
  { "en": "Apa Itu Semaphore (Semaphore) RTOS?", "id": "Objek Sinkronisasi Mengizinkan Sejumlah Tugas Akses." },
  { "en": "Apa Itu Antrian Pesan (Message Queue) RTOS?", "id": "Mekanisme Komunikasi Antar Tugas (Mengirim Data)." },
  { "en": "Apa Itu Relai Kehilangan Eksitasi (Loss of Excitation)?", "id": "Relai Melindungi Generator Jika Kehilangan Medan Magnet." },
  { "en": "Apa Itu Relai Anti-Motoring (Anti-Motoring)?", "id": "Relai Melindungi Generator Agar Tidak Berfungsi Sebagai Motor." },
  { "en": "Apa Itu Relai Urutan Negatif (Negative Sequence)?", "id": "Relai Melindungi Generator Dari Arus Beban Tidak Seimbang." },
  { "en": "Apa Itu Relai Gangguan Rotor (Rotor Earth Fault)?", "id": "Relai Mendeteksi Kebocoran Arus Lilitan Rotor Ke Tanah." },
  { "en": "Apa Itu Relai Gangguan Stator (Stator Earth Fault)?", "id": "Relai Mendeteksi Kebocoran Arus Lilitan Stator Ke Tanah." },
  { "en": "Apa Itu Relai Impedansi (Impedance Relay)?", "id": "Relai Proteksi (Umumnya) Untuk Saluran Transmisi Jarak Jauh." },
  { "en": "Apa Itu Relai Mho (Mho Relay)?", "id": "Relai Jarak (Bentuk Lingkaran) Sangat Cocok Saluran Jauh." },
  { "en": "Apa Itu Relai Reaktansi (Reactance Relay)?", "id": "Relai Jarak (Hanya Merespon Reaktansi) Saluran Pendek." },
  { "en": "Apa Itu Skema Power Line Carrier (PLC)?", "id": "Komunikasi Data (Proteksi) Menumpang Di Kabel Transmisi." },
  { "en": "Apa Itu Skema Transfer Trip (Transfer Trip)?", "id": "Skema Mengirim Sinyal Trip Dari Satu Gardu Ke Gardu Lain." },
  { "en": "Apa Itu Skema POTT (Permissive Overreach Transfer Trip)?", "id": "Skema (Transfer Trip) Membutuhkan Izin (Overreach) Relai Lokal." },
  { "en": "Apa Itu Skema PUTT (Permissive Underreach Transfer Trip)?", "id": "Skema (Transfer Trip) Membutuhkan Izin (Underreach) Relai Lokal." },
  { "en": "Apa Itu Skema DCB (Directional Comparison Blocking)?", "id": "Skema (Blokir) Menggunakan Sinyal Blokir (Bukan Sinyal Trip)." },
  { "en": "Apa Itu Relai Sinkronisasi (Synchrocheck Relay)?", "id": "Relai Memastikan (Syarat Terpenuhi) Sebelum Paralel Generator." },
  { "en": "Apa Itu Transformator Grounding (Grounding Transformer)?", "id": "Trafo (Khusus) Menyediakan Titik Netral (Sistem Delta)." },
  { "en": "Apa Itu Tipe Trafo Grounding Zig-Zag?", "id": "Tipe (Paling Umum) Trafo Grounding (Impedansi Nol Tinggi)." },
  { "en": "Apa Itu Resistor Pembumian Netral (NGR)?", "id": "Resistor (Membatasi) Arus Gangguan Tanah (Di Titik Netral)." },
  { "en": "Apa Itu Pembumian Solid (Solidly Grounded)?", "id": "Titik Netral Terhubung Langsung Ke Tanah (Tanpa Resistor)." },
  { "en": "Apa Itu Pembumian Impedansi Tinggi (High-Resistance)?", "id": "Pembumian (NGR) Arus Gangguan Tanah Sangat Kecil." },
  { "en": "Apa Itu Pembumian Impedansi Rendah (Low-Resistance)?", "id": "Pembumian (NGR) Arus Gangguan Tanah Dibatasi (Ratusan Amper)." },
  { "en": "Apa Itu Sistem Tidak Dibumikan (Ungrounded System)?", "id": "Sistem (Delta) Tidak Memiliki Koneksi Disengaja Ke Tanah." },
  { "en": "Apa Itu Gangguan Fasa-Ke-Tanah (Phase-to-Ground Fault)?", "id": "Hubung Singkat Antara Satu Fasa Dan Tanah." },
  { "en": "Apa Itu Gangguan Fasa-Ke-Fasa (Phase-to-Phase Fault)?", "id": "Hubung Singkat Antara Dua Fasa Yang Berbeda." },
  { "en": "Apa Itu Gangguan Tiga Fasa (Three-Phase Fault)?", "id": "Hubung Singkat (Simetris) Melibatkan Ketiga Fasa." },
  { "en": "Apa Itu Gangguan Simetris (Symmetrical Fault)?", "id": "Gangguan (Tiga Fasa) Arusnya Seimbang Di Setiap Fasa." },
  { "en": "Apa Itu Gangguan Asimetris (Asymmetrical Fault)?", "id": "Gangguan (Tidak Seimbang) Fasa-Tanah, Fasa-Fasa." },
  { "en": "Apa Itu Komponen Simetris (Symmetrical Components)?", "id": "Metode (Matematis) Analisis Gangguan Asimetris." },
  { "en": "Apa Itu Jaringan Urutan Positif (Positive Sequence)?", "id": "Komponen (Simetris) Urutan Fasa Normal (R-S-T)." },
  { "en": "Apa Itu Jaringan Urutan Negatif (Negative Sequence)?", "id": "Komponen (Simetris) Urutan Fasa Terbalik (R-T-S)." },
  { "en": "Apa Itu Jaringan Urutan Nol (Zero Sequence)?", "id": "Komponen (Simetris) Tiga Fasa Sefasa (Arus Tanah)." },
  { "en": "Arus Urutan Apa Yang Ada Di Gangguan Fasa-Tanah?", "id": "Urutan Positif, Negatif, Dan Nol." },
  { "en": "Arus Urutan Apa Yang Ada Di Gangguan Fasa-Fasa?", "id": "Hanya Urutan Positif Dan Negatif." },
  { "en": "Arus Urutan Apa Yang Ada Di Gangguan Tiga Fasa?", "id": "Hanya Urutan Positif." },
  { "en": "Apa Itu Nilai Per Unit (Per Unit System)?", "id": "Metode (Normalisasi) Menganalisis Sistem Tenaga (Dasar MVA/KV)." },
  { "en": "Apa Keuntungan Sistem Per Unit?", "id": "Menyederhanakan (Perhitungan) (Menghilangkan) Rasio Trafo." },
  { "en": "Apa Itu Studi Aliran Daya (Load Flow Study)?", "id": "Analisis (Menghitung) Aliran (Daya MW, MVAR) Dalam Jaringan." },
  { "en": "Apa Itu Studi Hubung Singkat (Short Circuit Study)?", "id": "Analisis (Menghitung) Arus (Maksimum) Saat Gangguan." },
  { "en": "Apa Itu Studi Stabilitas Transien (Transient Stability)?", "id": "Analisis (Kemampuan) Generator (Tetap Sinkron) Pasca Gangguan." },
  { "en": "Apa Itu Studi Motor Starting (Motor Starting Study)?", "id": "Analisis (Dampak) Penurunan (Tegangan) Saat Motor Besar Start." },
  { "en": "Apa Itu Studi Koordinasi Proteksi?", "id": "Analisis (Mengatur) Setting (Relai) Agar Bekerja (Selektif)." },
  { "en": "Apa Itu Studi Bahaya Arc Flash?", "id": "Analisis (Menghitung) Energi (Insiden) Bahaya (Busur Api)." },
  { "en": "Apa Itu Perangkat Lunak ETAP (ETAP Software)?", "id": "Perangkat (Lunak) Populer (Analisis) Sistem Tenaga Listrik." },
  { "en": "Apa Itu Bus (Bus) Dalam Studi Aliran Daya?", "id": "Titik (Simpul) Pertemuan (Node) Dalam Jaringan Listrik." },
  { "en": "Apa Itu Bus Swing (Swing Bus)?", "id": "Bus (Referensi) Menyeimbangkan (Selisih) Daya (Generator Utama)." },
  { "en": "Apa Itu Bus Beban (Load Bus/PQ Bus)?", "id": "Bus (Diketahui) Daya Aktif (P) Dan Reaktif (Q)." },
  { "en": "Apa Itu Bus Generator (Voltage-Controlled Bus/PV Bus)?", "id": "Bus (Diketahui) Daya Aktif (P) Dan Tegangan (V)." },
  { "en": "Apa Itu Metode Newton-Raphson?", "id": "Metode (Iteratif) Populer (Menyelesaikan) Aliran Daya (Cepat)." },
  { "en": "Apa Itu Metode Gauss-Seidel?", "id": "Metode (Iteratif) Sederhana (Menyelesaikan) Aliran Daya (Lambat)." },
  { "en": "Apa Itu Matriks Admitansi (Y-Bus)?", "id": "Matriks (Merepresentasikan) Koneksi (Admitansi) Antar Bus Jaringan." },
  { "en": "Apa Itu Matriks Impedansi (Z-Bus)?", "id": "Matriks (Merepresentasikan) Impedansi (Antar Bus) Jaringan." },
  { "en": "Apa Itu Kabel Bawah Tanah (Underground Cable)?", "id": "Kabel (Transmisi/Distribusi) Ditanam (Di Dalam) Tanah." },
  { "en": "Apa Keunggulan Kabel Bawah Tanah?", "id": "Estetika (Indah), Tahan Cuaca, Andal." },
  { "en": "Apa Kerugian Kabel Bawah Tanah?", "id": "Biaya (Pemasangan) Mahal, Sulit (Diperbaiki), Kapasitansi Tinggi." },
  { "en": "Apa Itu Kapasitansi (Capacitance) Kabel Bawah Tanah?", "id": "Kabel (Bertindak) Seperti (Kapasitor) (Isolasi Antar Konduktor)." },
  { "en": "Apa Itu Arus Pengisian (Charging Current) Kabel?", "id": "Arus (Kapasitif) Mengisi (Kabel) (Terutama Kabel Panjang)." },
  { "en": "Apa Itu Kabel XLPE (Cross-Linked Polyethylene)?", "id": "Kabel (Isolasi) Populer (Untuk Tegangan) Menengah Tinggi." },
  { "en": "Apa Itu Kabel PILC (Paper Insulated Lead Covered)?", "id": "Kabel (Isolasi Kertas) Tipe (Lama) (Berisi Minyak)." },
  { "en": "Apa Itu Sambungan (Jointing) Kabel?", "id": "Proses (Menyambung) Dua (Ujung) Kabel Bawah Tanah." },
  { "en": "Apa Itu Terminasi (Termination) Kabel?", "id": "Proses (Mengakhiri) Kabel (Menyambung) Ke Peralatan." },
  { "en": "Apa Itu Cone Stres (Stress Cone) Terminasi?", "id": "Mengontrol (Gradien Medan Listrik) Di Ujung (Kabel)." },
  { "en": "Apa Itu Pelindung (Screen/Shield) Kabel TM?", "id": "Lapisan (Konduktif) Mengontrol (Medan Listrik) Internal Kabel." },
  { "en": "Apa Itu Armor (Armor) Kabel?", "id": "Lapisan (Baja) Proteksi (Mekanis) Kabel Bawah Tanah." },
  { "en": "Apa Itu Selubung Luar (Outer Sheath) Kabel?", "id": "Lapisan (Luar) Proteksi (Kelembaban, Korosi) Kabel." },
  { "en": "Apa Itu Tes TDR (Time-Domain Reflectometer) Kabel?", "id": "Mengukur (Jarak) Lokasi (Gangguan) (Putus/Korsleting) Kabel." },
  { "en": "Apa Itu Tes VLF (Very Low Frequency) Kabel?", "id": "Tes (Hipot) Isolasi (Kabel) (Frekuensi 0.1 Hz)." },
  { "en": "Mengapa Tes Kabel Menggunakan VLF (Bukan DC)?", "id": "DC (Dapat Merusak) Isolasi (XLPE) Tipe Lama." },
  { "en": "Apa Itu Tes Tan Delta (Tan Delta Test) Kabel?", "id": "Mengukur (Kualitas) Deteriorasi (Isolasi) Kabel." },
  { "en": "Apa Itu Tes Pelepasan Sebagian (Partial Discharge) Kabel?", "id": "Mendeteksi (Void/Kotoran) Cacat (Kecil) Di Isolasi Kabel." },
  { "en": "Apa Itu Manhole (Manhole) Kabel?", "id": "Lubang (Akses) Bawah Tanah (Tempat Sambungan/Penarikan Kabel)." },
  { "en": "Apa Itu Kabel Tray (Baki Kabel)?", "id": "Struktur (Penopang) Jalur (Instalasi) Kabel Listrik." },
  { "en": "Apa Itu Kabel Ladder (Tangga Kabel)?", "id": "Baki (Kabel) Berbentuk (Tangga) Untuk (Kabel Berat)." },
  { "en": "Apa Itu Kabel Trough (Palung Kabel)?", "id": "Baki (Kabel) Berlubang (Perforated) (Ventilasi Baik)." },
  { "en": "Apa Itu Kabel Channel (Kanal Kabel)?", "id": "Baki (Kabel) Solid (Tertutup) (Proteksi Penuh)." },
  { "en": "Apa Itu Sistem Bus Riser (Bus Riser)?", "id": "Sistem (Distribusi) Busbar (Vertikal) Antar (Lantai Gedung)." },
  { "en": "Apa Itu Tap-Off Unit (Unit Sambungan) Busduct?", "id": "Kotak (Sambungan) Mengambil (Daya) Dari (Jalur Busduct)." },
  { "en": "Apa Itu Isolator Standoff (Standoff Insulator)?", "id": "Isolator (Penyangga) Busbar (Di Dalam) Panel Listrik." },
  { "en": "Apa Itu Kuncian Kabel (Cable Gland)?", "id": "Konektor (Penyekat) Kabel (Masuk Ke) Casing (Panel)." },
  { "en": "Apa Fungsi Kuncian Kabel (Cable Gland)?", "id": "Menjaga (Rating IP) Casing (Mencegah Debu/Air Masuk)." },
  { "en": "Apa Itu Kuncian Kabel (Cable Gland) Tahan Ledakan (Ex)?", "id": "Kuncian (Kabel) Khusus (Untuk Lokasi) Berbahaya (Explosion Proof)." },
  { "en": "Apa Itu Pemanas Ruang (Space Heater) Panel?", "id": "Pemanas (Listrik) Mencegah (Kondensasi/Kelembaban) Dalam Panel." },
  { "en": "Apa Itu Termostat (Thermostat) Pemanas Panel?", "id": "Saklar (Otomatis) Menghidupkan (Pemanas) Berdasarkan (Suhu)." },
  { "en": "Apa Itu Higrostat (Hygrostat) Pemanas Panel?", "id": "Saklar (Otomatis) Menghidupkan (Pemanas) Berdasarkan (Kelembaban)." },
  { "en": "Apa Itu Ventilasi (Ventilation) Paksa Panel?", "id": "Menggunakan (Kipas/Fan) Untuk (Sirkulasi) Udara (Pendingin) Panel." },
  { "en": "Apa Itu Pendingin Udara (Air Conditioner) Panel?", "id": "AC (Khusus) Mendinginkan (Peralatan) Di Dalam (Panel Listrik)." },
  { "en": "Apa Itu Peringkat NEMA (NEMA Rating) Panel?", "id": "Standar (Amerika) Rating (Proteksi) Casing (Enclosure)." },
  { "en": "Apa Itu Peringkat IP (IP Rating) Panel?", "id": "Standd" },
  { "en": "Apa Itu Pembumian (Grounding) Panel?", "id": "Menghubungkan (Bodi Panel) Logam (Ke Sistem) Tanah (Keselamatan)." },
  { "en": "Apa Itu Busbar Pembumian (Grounding Busbar)?", "id": "Batang (Tembaga) Titik (Kumpul) Koneksi (Kabel Ground)." },
  { "en": "Apa Itu Busbar Netral (Neutral Busbar)?", "id": "Batang (Tembaga) Titik (Kumpul) Koneksi (Kabel Netral)." },
  { "en": "Apa Itu Ikatan (Bonding) Netral-Ground?", "id": "Sambungan (Tunggal) Antara (Netral) Dan (Ground) (Di Panel Utama)." },
  { "en": "Apa Itu Label (Label) Peringatan Panel?", "id": "Stiker (Peringatan) Bahaya (Tegangan Tinggi, Arc Flash)." },
  { "en": "Apa Itu Diagram Satu Garis (One-Line Diagram)?", "id": "Gambar (Sederhana) Sistem (Listrik) Menggunakan (Satu Garis)." },
  { "en": "Apa Itu Diagram Pengkabelan (Wiring Diagram)?", "id": "Gambar (Detail) Menunjukkan (Koneksi Kabel) Aktual (Fisik)." },
  { "en": "Apa Itu Diagram Skematik (Schematic Diagram)?", "id": "Gambar (Logika) Rangkaian (Menggunakan) Simbol-Simbol." },
  { "en": "Apa Itu Daftar Material (Bill of Materials/BOM)?", "id": "Daftar (Semua Komponen) Dan (Kuantitas) Untuk (Proyek)." },
  { "en": "Apa Itu Prosedur (Procedure) Pengujian FAT?", "id": "Tes (Penerimaan) Di (Pabrik) Pembuat (Factory Acceptance Test)." },
  { "en": "Apa Itu Prosedur (Procedure) Pengujian SAT?", "id": "Tes (Penerimaan) Di (Lokasi) Pelanggan (Site Acceptance Test)." },
  { "en": "Apa Itu Komisioning (Commissioning)?", "id": "Proses (Memverifikasi) Instalasi (Sesuai Desain) Dan (Siap Operasi)." },
  { "en": "Apa Itu Start-Up (Start-Up)?", "id": "Proses (Menyalakan) Peralatan (Pertama Kali) Setelah (Instalasi)." },
  { "en": "Apa Itu Serah Terima (Handover)?", "id": "Proses (Penyerahan) Proyek (Selesai) Dari (Kontraktor) Ke (Pemilik)." },
  { "en": "Apa Itu Periode Garansi (Warranty Period)?", "id": "Masa (Jaminan) Perbaikan (Gratis) Dari (Pabrikan)." },
  { "en": "Apa Itu Pemeliharaan Preventif (Preventive Maintenance)?", "id": "Perawatan (Terjadwal) Mencegah (Kerusakan) (Berbasis Waktu)." },
  { "en": "Apa Itu Pemeliharaan Prediktif (Predictive Maintenance)?", "id": "Perawatan (Berdasarkan Kondisi) (Memprediksi) Kapan (Akan Rusak)." },
  { "en": "Apa Itu Pemeliharaan Korektif (Corrective Maintenance)?", "id": "Perbaikan (Dilakukan) Setelah (Peralatan) Gagal (Rusak)." },
  { "en": "Apa Itu CMMS (Computerized Maintenance Management System)?", "id": "Perangkat Lunak (Mengelola) Jadwal (Perawatan) Aset (Work Order)." },
  { "en": "Apa Itu Komputasi Kuantum (Quantum Computing)?", "id": "Komputasi Menggunakan Fenomena Mekanika Kuantum." },
  { "en": "Apa Itu Qubit (Quantum Bit)?", "id": "Unit Dasar Informasi Kuantum (Bisa 0, 1, Keduanya)." },
  { "en": "Apa Itu Superposisi (Superposition) Kuantum?", "id": "Kemampuan Qubit Berada Dalam Keadaan 0 Dan 1 Bersamaan." },
  { "en": "Apa Itu Keterkaitan (Entanglement) Kuantum?", "id": "Koneksi Dua Qubit (Status Saling Terkait) Meski Terpisah Jauh." },
  { "en": "Apa Itu Dekoherensi (Decoherence) Kuantum?", "id": "Hilangnya Sifat Kuantum (Superposisi) Akibat Gangguan Lingkungan." },
  { "en": "Apa Tantangan Terbesar Komputasi Kuantum?", "id": "Menjaga Koherensi Qubit (Mengatasi Dekoherensi)." },
  { "en": "Apa Itu Kriptografi Kuantum (Quantum Cryptography)?", "id": "Menggunakan Prinsip Kuantum (Foton) Untuk Keamanan Komunikasi." },
  { "en": "Apa Itu Distribusi Kunci Kuantum (QKD)?", "id": "Metode (Aman) Berbagi Kunci Enkripsi (Berbasis Kuantum)." },
  { "en": "Apa Kepanjangan QKD (Quantum Key Distribution)?", "id": "Distribusi Kunci Kuantum." },
  { "en": "Apa Itu Protokol BB84 (Protokol Kuantum)?", "id": "Protokol (QKD) Pertama Menggunakan Polarisasi Foton." },
  { "en": "Apa Itu Kriptografi Pasca-Kuantum (PQC)?", "id": "Algoritma (Kriptografi Klasik) Yang (Diyakini) Aman (Dari Serangan Kuantum)." },
  { "en": "Apa Kepanjangan PQC (Post-Quantum Cryptography)?", "id": "Kriptografi Pasca-Kuantum." },
  { "en": "Apa Itu Elektronika Kuantum (Quantum Electronics)?", "id": "Studi (Efek Kuantum) Pada (Perilaku) Elektron (Laser, Semikonduktor)." },
  { "en": "Apa Itu Efek Terowongan (Tunneling Effect)?", "id": "Partikel (Kuantum) Menembus (Penghalang) Potensial (Energi)." },
  { "en": "Apa Itu Dioda Terowongan (Tunnel Diode/Esaki Diode)?", "id": "Dioda (Menggunakan) Efek Terowongan (Resistansi Negatif)." },
  { "en": "Apa Itu Mikroskop Terowongan Pindai (STM)?", "id": "Mikroskop (Resolusi Atom) Menggunakan (Arus) Terowongan Kuantum." },
  { "en": "Apa Kepanjangan STM (Scanning Tunneling Microscope)?", "id": "Mikroskop Terowongan Pindai." },
  { "en": "Apa Itu Titik Kuantum (Quantum Dot)?", "id": "Nanokristal (Semikonduktor) Sifat (Optik) Tergantung Ukuran." },
  { "en": "Apa Aplikasi Titik Kuantum (Quantum Dot)?", "id": "Layar Tampilan (TV QLED), Pencitraan Medis, Sel Surya." },
  { "en": "Apa Itu Sumur Kuantum (Quantum Well)?", "id": "Struktur (Semikonduktor Tipis) Menjebak (Partikel) Satu Dimensi." },
  { "en": "Apa Itu Kawat Kuantum (Quantum Wire)?", "id": "Struktur (Semikonduktor) Menjebak (Partikel) Dua Dimensi." },
  { "en": "Apa Itu Efek Hall Kuantum (Quantum Hall Effect)?", "id": "Efek (Kuantum) Resistansi Hall (Terkuantisasi) Di Suhu Dingin." },
  { "en": "Apa Itu Efek Hall Kuantum Spin (Quantum Spin Hall)?", "id": "Material (Isolator Topologis) Konduksi (Spin) Di Tepi." },
  { "en": "Apa Itu Isolator Topologis (Topological Insulator)?", "id": "Material (Isolator) Di Dalam, (Konduktor) Di Permukaan." },
  { "en": "Apa Itu Spintronik (Spintronics)?", "id": "Elektronik (Berbasis) Sifat (Spin) Elektron (Bukan Muatan)." },
  { "en": "Apa Itu GMR (Giant Magnetoresistance)?", "id": "Efek (Perubahan Resistansi) Besar (Akibat Medan Magnet) (Head HDD)." },
  { "en": "Apa Kepanjangan GMR (Giant Magnetoresistance)?", "id": "Magnetoresistansi Raksasa." },
  { "en": "Apa Itu MRAM (Magnetoresistive RAM)?", "id": "Memori (Non-Volatil) Menyimpan (Data) Menggunakan (Orientasi) Spin." },
  { "en": "Apa Itu Sirkuit Logika CMOS (Complementary MOS)?", "id": "Logika (Digital) Menggunakan (Pasangan) Transistor PMOS Dan NMOS." },
  { "en": "Apa Keunggulan Logika CMOS?", "id": "Konsumsi Daya (Statis) Sangat Rendah." },
  { "en": "Apa Itu Inverter CMOS (Inverter CMOS)?", "id": "Gerbang (NOT) Dasar (Satu PMOS, Satu NMOS)." },
  { "en": "Apa Itu Gerbang NAND (NAND Gate) CMOS?", "id": "Dua (PMOS Paralel) Di Atas, Dua (NMOS Seri) Di Bawah." },
  { "en": "Apa Itu Gerbang NOR (NOR Gate) CMOS?", "id": "Dua (PMOS Seri) Di Atas, Dua (NMOS Paralel) Di Bawah." },
  { "en": "Apa Itu Gerbang Transmisi (Transmission Gate) CMOS?", "id": "Saklar (Analog) Elektronik (Satu PMOS, Satu NMOS) Paralel." },
  { "en": "Apa Itu Logika Pass-Transistor (Pass-Transistor Logic)?", "id": "Logika (Menggunakan) Transistor (Hanya NMOS) Sebagai Saklar." },
  { "en": "Apa Kelemahan Logika Pass-Transistor?", "id": "Degradasi (Tegangan) Sinyal (Tidak Bisa Melewatkan '1' Penuh)." },
  { "en": "Apa Itu Logika Dinamis (Dynamic Logic)?", "id": "Logika (CMOS) Menggunakan (Sinyal Clock) Mengurangi Jumlah Transistor." },
  { "en": "Apa Itu Logika Domino (Domino Logic)?", "id": "Tipe (Logika Dinamis) (Output Dihubungkan) Ke Inverter." },
  { "en": "Apa Itu Fan-In (Fan-In) Gerbang Logika?", "id": "Jumlah (Input) Maksimum (Yang Dimiliki) Gerbang Logika." },
  { "en": "Apa Itu Fan-Out (Fan-Out) Gerbang Logika?", "id": "Jumlah (Input Gerbang Lain) Yang (Dapat Digerakkan) Output." },
  { "en": "Apa Itu Margin Derau (Noise Margin)?", "id": "Batas (Tegangan Noise) Yang (Dapat Ditoleransi) Gerbang." },
  { "en": "Apa Itu VIL (Voltage Input Low)?", "id": "Tegangan (Input) Maksimum (Yang Dianggap) Logika '0' (Rendah)." },
  { "en": "Apa Itu VIH (Voltage Input High)?", "id": "Tegangan (Input) Minimum (Yang Dianggap) Logika '1' (Tinggi)." },
  { "en": "Apa Itu VOL (Voltage Output Low)?", "id": "Tegangan (Output) Maksimum (Saat Mengeluarkan) Logika '0' (Rendah)." },
  { "en": "Apa Itu VOH (Voltage Output High)?", "id": "Tegangan (Output) Minimum (Saat Mengeluarkan) Logika '1' (Tinggi)." },
  { "en": "Apa Itu Arus Bocor (Leakage Current)?", "id": "Arus (Kecil) Yang (Tetap Mengalir) Saat Transistor (Off)." },
  { "en": "Apa Itu Disipasi Daya Statis (Static Power)?", "id": "Daya (Hilang) Akibat (Arus Bocor) Saat (Sirkuit) Diam." },
  { "en": "Apa Itu Disipasi Daya Dinamis (Dynamic Power)?", "id": "Daya (Hilang) Akibat (Pengisian Kapasitor) Saat (Switching)." },
  { "en": "Apa Rumus Daya Dinamis (P_dyn)?", "id": "Proporsional (Kapasitansi) C, (Tegangan) V-Kuadrat, (Frekuensi) F." },
  { "en": "Apa Itu Penurunan Skala (Scaling) CMOS?", "id": "Proses (Mengecilkan) Ukuran (Transistor) (Hukum Moore)." },
  { "en": "Apa Itu Efek Saluran Pendek (Short-Channel Effect)?", "id": "Masalah (Transistor) Akibat (Ukuran Channel) Terlalu Pendek." },
  { "en": "Apa Itu FinFET (Fin Field-Effect Transistor)?", "id": "Arsitektur (Transistor 3D) (Gate Mengelilingi) Saluran (Fin)." },
  { "en": "Mengapa Beralih Ke FinFET?", "id": "Mengurangi (Efek Saluran Pendek) Dan (Arus Bocor)." },
  { "en": "Apa Itu GAAFET (Gate-All-Around FET)?", "id": "Arsitektur (Transistor) (Gate Mengelilingi) Saluran (Sepenuhnya)." },
  { "en": "Apa Itu Desain VLSI (Very Large Scale Integration)?", "id": "Proses (Desain) Sirkuit Terpadu (Sangat Kompleks)." },
  { "en": "Apa Itu HDL (Hardware Description Language)?", "id": "Bahasa (Deskripsi) Perangkat Keras (VHDL, Verilog)." },
  { "en": "Apa Itu VHDL (VHSIC Hardware Description Language)?", "id": "Bahasa (Standar IEEE) Deskripsi Perangkat Keras." },
  { "en": "Apa Itu Verilog (Verilog)?", "id": "Bahasa (Alternatif) Deskripsi Perangkat Keras (Mirip C)." },
  { "en": "Apa Itu RTL (Register-Transfer Level)?", "id": "Level (Abstraksi Desain) Menggambarkan (Aliran Data) Antar Register." },
  { "en": "Apa Itu Sintesis (Synthesis) Logika?", "id": "Proses (Kompilasi) Kode (RTL/HDL) Menjadi (Gerbang Logika)." },
  { "en": "Apa Itu Penempatan Dan Perutean (Place and Route)?", "id": "Proses (Desain Fisik) Menempatkan (Gerbang) Merutekan (Kabel)." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "Chip (Dapat Diprogram) (Berisi Blok Logika) Dan (Interkoneksi)." },
  { "en": "Apa Itu Blok Logika (Logic Block) FPGA?", "id": "Unit (Dasar) FPGA (Berisi LUT Dan Flip-Flop)." },
  { "en": "Apa Itu LUT (Look-Up Table) FPGA?", "id": "Memori (Kecil) Mengimplementasikan (Fungsi Logika) Kombinasional." },
  { "en": "Apa Itu CPLD (Complex Programmable Logic Device)?", "id": "Perangkat (Logika Terprogram) Lebih (Sederhana) Dari FPGA." },
  { "en": "Apa Itu ASIC (Application-Specific Integrated Circuit)?", "id": "Chip (Didesain) Khusus (Hanya Untuk) Satu Aplikasi (Kinerja Tinggi)." },
  { "en": "Apa Beda FPGA (Field-Programmable Gate Array) Dan ASIC (Application-Specific Integrated Circuit)?", "id": "FPGA (Dapat Diprogram Ulang), ASIC (Permanen, Cepat, Murah Massal)." },
  { "en": "Apa Itu Verifikasi (Verification) Desain?", "id": "Proses (Memastikan) Desain (Chip) Bekerja (Sesuai Spesifikasi)." },
  { "en": "Apa Itu Simulasi (Simulation) Desain?", "id": "Menguji (Fungsionalitas) Desain (HDL) Menggunakan (Testbench)." },
  { "en": "Apa Itu Testbench (Testbench)?", "id": "Kode (HDL) Yang (Menghasilkan Sinyal) Uji (Untuk Simulasi)." },
  { "en": "Apa Itu Emulasi (Emulation) Perangkat Keras?", "id": "Menggunakan (FPGA) Untuk (Mensimulasikan) Perilaku (ASIC) (Cepat)." },
  { "en": "Apa Itu Verifikasi Formal (Formal Verification)?", "id": "Menggunakan (Metode Matematika) Membuktikan (Kebenaran) Desain." },
  { "en": "Apa Itu Analisis Waktu Statis (Static Timing Analysis/STA)?", "id": "Analisis (Memeriksa) Waktu (Tundaan Sinyal) Tanpa (Simulasi)." },
  { "en": "Apa Itu Batasan Waktu (Timing Constraints)?", "id": "Aturan (Setup, Hold) Yang (Harus Dipenuhi) Desain (Digital)." },
  { "en": "Apa Itu Waktu Setup (Setup Time)?", "id": "Waktu (Data) Harus (Stabil) Sebelum (Tepi Clock) Aktif." },
  { "en": "Apa Itu Waktu Tahan (Hold Time)?", "id": "Waktu (Data) Harus (Stabil) Setelah (Tepi Clock) Aktif." },
  { "en": "Apa Itu Pelanggaran Waktu (Timing Violation)?", "id": "Kondisi (Sinyal) Gagal (Memenuhi) Waktu (Setup/Hold)." },
  { "en": "Apa Itu Kemiringan Jam (Clock Skew)?", "id": "Perbedaan (Waktu Tiba) Sinyal (Clock) Di (Flip-Flop) Berbeda." },
  { "en": "Apa Itu Jitter Jam (Clock Jitter)?", "id": "Variasi (Waktu) Tepi (Sinyal Clock) Dari (Posisi) Ideal." },
  { "en": "Apa Itu Pohon Jam (Clock Tree) Desain?", "id": "Jaringan (Distribusi Sinyal Clock) Ke (Semua Flip-Flop)." },
  { "en": "Apa Itu Desain Untuk Pengujian (Design for Test/DFT)?", "id": "Teknik (Desain) Memudahkan (Pengujian) Chip (Setelah Produksi)." },
  { "en": "Apa Itu Pindai Batas (Boundary Scan/JTAG)?", "id": "Teknik (DFT) Menguji (Koneksi Pin) Chip (Ke PCB)." },
  { "en": "Apa Itu ATPG (Automatic Test Pattern Generation)?", "id": "Perangkat Lunak (Otomatis) Menghasilkan (Pola Uji) Chip." },
  { "en": "Apa Itu BIST (Built-In Self-Test)?", "id": "Sirkuit (Logika Uji) Yang (Tertanam) Di Dalam (Chip)." },
  { "en": "Apa Itu Hasil (Yield) Produksi Chip?", "id": "Persentase (Chip) Yang (Berfungsi Baik) Dari (Total Wafer)." },
  { "en": "Apa Itu Fab (Fab/Foundry)?", "id": "Pabrik (Manufaktur) Semikonduktor (Membuat Wafer Chip)." },
  { "en": "Apa Itu Perusahaan Fabless (Fabless Company)?", "id": "Perusahaan (Hanya Mendesain) Chip (Tidak Punya Fabrikasi)." },
  { "en": "Apa Itu Noda (Node) Proses (Contoh: 7nm, 5nm)?", "id": "Ukuran (Teknologi) Manufaktur (Menunjukkan) Kepadatan (Transistor)." },
  { "en": "Apa Itu Litografi (Lithography)?", "id": "Proses (Pencetakan) Pola (Sirkuit) Ke (Wafer Silikon)." },
  { "en": "Apa Itu Litografi EUV (Extreme Ultraviolet)?", "id": "Litografi (Menggunakan Sinar UV Ekstrem) Untuk (Noda Tercanggih)." },
  { "en": "Apa Itu Wafer (Wafer) Silikon?", "id": "Kepingan (Tipis, Bulat) Silikon (Murni) (Bahan Dasar) Chip." },
  { "en": "Apa Itu Die (Die) Chip?", "id": "Satu (Potongan) Chip (Individual) Dari (Wafer)." },
  { "en": "Apa Itu Pengemasan (Packaging) Chip?", "id": "Proses (Melindungi Die) Dan (Menyambung Pin) Ke (Dunia Luar)." },
  { "en": "Apa Itu Ikatan Kawat (Wire Bonding)?", "id": "Menyambung (Die) Ke (Kaki Kemasan) Menggunakan (Kawat Emas) Halus." },
  { "en": "Apa Itu Flip-Chip (Flip-Chip)?", "id": "Metode (Pemasangan Die) (Terbalik) (Bola Solder) Langsung (Ke Substrat)." },
  { "en": "Apa Itu Interposer (Interposer)?", "id": "Lapisan (Perantara) (Menghubungkan Die) Ke (Substrat Kemasan) (HBM)." },
  { "en": "Apa Itu HBM (High Bandwidth Memory)?", "id": "Memori (Tumpuk 3D) (Bandwidth Sangat Tinggi) (GPU)." },
  { "en": "Apa Itu Chiplet (Chiplet)?", "id": "Desain (Modular) Menggabungkan (Beberapa Die Kecil) Dalam (Satu Kemasan)." },
  { "en": "Apa Itu Desain Sistem-On-Chip (SoC)?", "id": "Integrasi (Semua Komponen) Sistem (CPU, GPU, Memori) (Dalam Satu Chip)." },
  { "en": "Apa Itu IP Core (Intellectual Property Core)?", "id": "Blok (Desain) Fungsional (Siap Pakai) (Contoh: ARM CPU)." },
  { "en": "Apa Itu Analisis Kegagalan (Failure Analysis)?", "id": "Proses Investigasi Mencari Akar Penyebab Kegagalan." },
  { "en": "Apa Itu Dekapsulasi (Decapsulation) IC?", "id": "Membuka Mengupas Kemasan IC Melihat Chip." },
  { "en": "Apa Itu Etsa Basah (Wet Etching) FA?", "id": "Menggunakan Asam Kimia Membuka Lapisan Chip." },
  { "en": "Apa Itu Etsa Kering (Dry Etching/RIE) FA?", "id": "Menggunakan Plasma Gas Membuka Lapisan Chip Presisi." },
  { "en": "Apa Itu SEM (Scanning Electron Microscope)?", "id": "Mikroskop Elektron Menampilkan Gambar Resolusi Sangat Tinggi." },
  { "en": "Apa Fungsi SEM (Scanning Electron Microscope) Dalam FA?", "id": "Melihat Struktur Kerusakan Fisik Skala Nano Mikro." },
  { "en": "Apa Itu EDX (Energy-Dispersive X-ray Spectroscopy)?", "id": "Analisis Menentukan Komposisi Unsur Material Di SEM." },
  { "en": "Apa Itu FIB (Focused Ion Beam)?", "id": "Alat Memotong Menggambar Material Skala Nano Chip." },
  { "en": "Apa Fungsi FIB (Focused Ion Beam) Dalam FA?", "id": "Membuat Potongan Lintang Cross-Section Chip Presisi." },
  { "en": "Apa Itu Emisi Foton (Photon Emission/PEM)?", "id": "Mendeteksi Cahaya Sangat Redup Dari Kebocoran Transistor." },
  { "en": "Apa Itu Mikroskopi Termal (Thermal Microscopy)?", "id": "Mendeteksi Titik Panas Hot Spot Skala Mikro." },
  { "en": "Apa Itu Uji Latch-Up (Latch-Up Test)?", "id": "Menguji Ketahanan Sirkuit CMOS Terhadap Kondisi Latch-Up." },
  { "en": "Apa Itu Latch-Up (Latch-Up) CMOS?", "id": "Kondisi Hubung Singkat Internal SCR Parasitik CMOS." },
  { "en": "Apa Itu Uji HBM (Human-Body Model) ESD?", "id": "Simulasi ESD Pelepasan Muatan Listrik Tubuh Manusia." },
  { "en": "Apa Itu Uji CDM (Charged-Device Model) ESD?", "id": "Simulasi ESD Pelepasan Muatan Dari Perangkat Sendiri." },
  { "en": "Apa Itu Uji MM (Machine Model) ESD?", "id": "Simulasi ESD Pelepasan Muatan Dari Benda Logam Mesin." },
  { "en": "Apa Itu Uji Keandalan (Reliability Testing)?", "id": "Menguji Batas Produk Memastikan Umur Pakai Lifetime." },
  { "en": "Apa Itu Uji HALT (Highly Accelerated Life Test)?", "id": "Uji Stres Ekstrem Mencari Titik Lemah Desain." },
  { "en": "Apa Itu Uji HASS (Highly Accelerated Stress Screen)?", "id": "Uji Stres Produksi Menyaring Produk Cacat Awal." },
  { "en": "Apa Itu Uji Siklus Termal (Thermal Cycling)?", "id": "Uji Suhu Naik-Turun Berulang Tes Ketahanan Sambungan." },
  { "en": "Apa Itu Uji Kejut Termal (Thermal Shock)?", "id": "Uji Perpindahan Suhu Sangat Cepat Panas-Dingin." },
  { "en": "Apa Itu Uji THB (Temperature Humidity Bias)?", "id": "Uji Suhu Tinggi Lembab Tinggi Tegangan Korosi." },
  { "en": "Apa Itu Uji HAST (Highly Accelerated Stress Test)?", "id": "Uji THB Dipercepat Menggunakan Tekanan Uap Tinggi." },
  { "en": "Apa Itu Uji Getaran (Vibration Testing)?", "id": "Uji Ketahanan Mekanis Terhadap Getaran Frekuensi Acak Sinus." },
  { "en": "Apa Itu Uji Jatuh (Drop Test)?", "id": "Uji Ketahanan Mekanis Produk Dijatuhkan Ketinggian Tertentu." },
  { "en": "Apa Itu Uji Kabut Garam (Salt Spray Test)?", "id": "Uji Ketahanan Korosi Material Disemprot Kabut Garam." },
  { "en": "Apa Itu Uji Paparan UV (UV Exposure Test)?", "id": "Uji Ketahanan Material Terhadap Radiasi Sinar UV." },
  { "en": "Apa Itu Uji Peringkat IP (IP Rating Test)?", "id": "Uji Ketahanan Casing Terhadap Debu Dan Air." },
  { "en": "Apa Itu MTBF (Mean Time Between Failures)?", "id": "Waktu Rata-Rata Antar Kegagalan Produk Dapat Diperbaiki." },
  { "en": "Apa Itu MTTF (Mean Time To Failure)?", "id": "Waktu Rata-Rata Hingga Gagal Produk Sekali Pakai." },
  { "en": "Apa Itu Kurva Bak Mandi (Bathtub Curve)?", "id": "Grafik Tingkat Kegagalan Awal, Normal, Akhir Aus." },
  { "en": "Apa Itu Periode Infant Mortality (Kematian Bayi)?", "id": "Tingkat Kegagalan Tinggi Di Awal Akibat Cacat Produksi." },
  { "en": "Apa Itu Uji Burn-In (Burn-In Test)?", "id": "Menjalankan Produk Baru Intensif Menyaring Infant Mortality." },
  { "en": "Apa Itu FMEA (Failure Mode and Effects Analysis)?", "id": "Analisis Sistematis Mode Kegagalan Dan Dampaknya Bottom-Up." },
  { "en": "Apa Itu FMECA (Failure Mode, Effects, and Criticality Analysis)?", "id": "FMEA Ditambah Analisis Tingkat Kekritisan Severity." },
  { "en": "Apa Itu FTA (Fault Tree Analysis)?", "id": "Analisis Pohon Logika Mencari Penyebab Kegagalan Top-Down." },
  { "en": "Apa Itu Angka Prioritas Risiko (RPN)?", "id": "Angka Prioritas Risiko Severity X Occurrence X Detection." },
  { "en": "Apa Kepanjangan RPN (Risk Priority Number)?", "id": "Angka Prioritas Risiko." },
  { "en": "Apa Itu HAZOP (Hazard and Operability Study)?", "id": "Studi Bahaya Dan Operabilitas Proses Industri Kimia." },
  { "en": "Apa Itu Kata Kunci (Guide Word) HAZOP?", "id": "Kata Pemicu Analisis HAZOP Contoh: No, More, Less." },
  { "en": "Apa Itu SIL (Safety Integrity Level)?", "id": "Tingkat Keandalan Fungsi Keselamatan Industri Proses." },
  { "en": "Apa Itu ASIL (Automotive Safety Integrity Level)?", "id": "Tingkat Keandalan Fungsi Keselamatan Industri Otomotif." },
  { "en": "Apa Itu Standar ISO 26262?", "id": "Standar Internasional Keamanan Fungsional Otomotif." },
  { "en": "Apa Itu Konverter Ćuk (Cuk Converter)?", "id": "Konverter DC-DC Inverting Output Negatif Naik Turun." },
  { "en": "Apa Itu Konverter SEPIC (SEPIC Converter)?", "id": "Konverter DC-DC Non-Inverting Output Positif Naik Turun." },
  { "en": "Apa Itu Konverter Zeta (Zeta Converter)?", "id": "Konverter DC-DC Non-Inverting Mirip SEPIC." },
  { "en": "Apa Itu Konverter Interleaved (Interleaved Converter)?", "id": "Dua Atau Lebih Konverter Paralel Beda Fasa." },
  { "en": "Apa Keuntungan Konverter Interleaved?", "id": "Mengurangi Riak Ripple Arus Input Dan Output." },
  { "en": "Apa Itu Jembatan Setengah (Half-Bridge) Inverter?", "id": "Inverter Dua Transistor Menghasilkan Output AC." },
  { "en": "Apa Itu Jembatan Penuh (Full-Bridge) Inverter?", "id": "Inverter Empat Transistor Mengontrol Beban AC." },
  { "en": "Apa Itu Inverter Multilevel (Multilevel Inverter)?", "id": "Inverter Menghasilkan Bentuk Gelombang Tangga Mendekati Sinus." },
  { "en": "Apa Itu Dioda Netral Terjepit (NPC)?", "id": "Topologi Umum Inverter Multilevel Tiga Level." },
  { "en": "Apa Kepanjangan NPC (Neutral Point Clamped)?", "id": "Titik Netral Terjepit." },
  { "en": "Apa Keuntungan Inverter Multilevel?", "id": "Harmonisa THD Rendah, Tegangan Transistor Rendah." },
  { "en": "Apa Itu Modulasi Lebar Pulsa (PWM)?", "id": "Teknik Switching Menghasilkan Sinyal Analog Dari Digital." },
  { "en": "Apa Itu SPWM (Sinusoidal Pulse Width Modulation)?", "id": "PWM Lebar Pulsa Dimodulasi Mengikuti Bentuk Sinus." },
  { "en": "Apa Itu Modulasi Vektor Ruang (Space Vector Modulation/SVM)?", "id": "Teknik PWM Digital Canggih Kontrol Motor Tiga Fasa." },
  { "en": "Apa Itu Jaringan Listrik Cerdas (Smart Grid)?", "id": "Jaringan Listrik Modern Komunikasi Dua Arah Otomatis." },
  { "en": "Apa Itu Meter Cerdas (Smart Meter)?", "id": "Meter Listrik Digital Mencatat Konsumsi Real-Time." },
  { "en": "Apa Kepanjangan AMI (Advanced Metering Infrastructure)?", "id": "Infrastruktur Pengukuran Lanjutan Jaringan Smart Meter." },
  { "en": "Apa Itu Respon Permintaan (Demand Response)?", "id": "Program Konsumen Mengurangi Beban Saat Puncak." },
  { "en": "Apa Itu Sumber Energi Terdistribusi (DER)?", "id": "Pembangkit Listrik Skala Kecil Tersebar PLTS Atap." },
  { "en": "Apa Kepanjangan DER (Distributed Energy Resources)?", "id": "Sumber Energi Terdistribusi." },
  { "en": "Apa Itu Jaringan Mikro (Microgrid)?", "id": "Jaringan Listrik Lokal Bisa Mandiri Islanding." },
  { "en": "Apa Itu V2G (Vehicle-to-Grid)?", "id": "Mobil Listrik Menyuplai Daya Kembali Ke Jaringan Grid." },
  { "en": "Apa Itu PMU (Phasor Measurement Unit)?", "id": "Perangkat Presisi Mengukur Fasa Tegangan Arus Sinkron." },
  { "en": "Apa Fungsi PMU (Phasor Measurement Unit)?", "id": "Monitor Stabilitas Jaringan Grid Area Luas Real-Time." },
  { "en": "Apa Itu WAMS (Wide Area Monitoring System)?", "id": "Sistem Monitoring Jaringan Grid Menggunakan Data PMU." },
  { "en": "Apa Itu Efek Fotovoltaik (Photovoltaic Effect)?", "id": "Fenomena Bahan Semikonduktor Menghasilkan Listrik Dari Cahaya." },
  { "en": "Apa Itu Sel Surya Monokristalin (Monocrystalline)?", "id": "Sel Surya Efisiensi Tinggi Terbuat Dari Kristal Tunggal." },
  { "en": "Apa Itu Sel Surya Polikristalin (Polycrystalline)?", "id": "Sel Surya Lebih Murah Terbuat Dari Banyak Kristal." },
  { "en": "Apa Itu Sel Surya Film Tipis (Thin-Film)?", "id": "Sel Surya Sangat Tipis Fleksibel Efisiensi Rendah." },
  { "en": "Apa Itu Sel Surya Perovskite?", "id": "Teknologi Sel Surya Baru Potensi Efisiensi Tinggi." },
  { "en": "Apa Itu Efisiensi (Efficiency) Sel Surya?", "id": "Persentase Energi Cahaya Diubah Menjadi Energi Listrik." },
  { "en": "Apa Itu Dioda Bypass (Bypass Diode) Panel Surya?", "id": "Dioda Paralel Sel Melindungi Sel Ternaungi Bayangan." },
  { "en": "Apa Itu Hot Spot (Hot Spot) Panel Surya?", "id": "Sel Ternaungi Menjadi Panas Resistif Merusak Panel." },
  { "en": "Apa Itu Dioda Blokir (Blocking Diode) Panel Surya?", "id": "Dioda Seri Rangkaian Mencegah Arus Balik Malam Hari." },
  { "en": "Apa Itu Pengontrol Muatan (Charge Controller) Surya?", "id": "Mengatur Pengisian Baterai Mencegah Overcharge." },
  { "en": "Apa Itu Kontroler PWM (Pulse Width Modulation) Surya?", "id": "Kontroler Sederhana Murah Menggunakan Switching PWM." },
  { "en": "Apa Itu Kontroler MPPT (Maximum Power Point Tracking)?", "id": "Kontroler Canggih Efisien Mencari Titik Daya Maksimal." },
  { "en": "Apa Itu Inverter Surya (Solar Inverter)?", "id": "Mengubah Listrik DC Panel Surya Menjadi Listrik AC." },
  { "en": "Apa Itu Inverter On-Grid (Grid-Tied)?", "id": "Inverter Sinkron Jaringan PLN Tanpa Baterai." },
  { "en": "Apa Itu Inverter Off-Grid (Stand-Alone)?", "id": "Inverter Mandiri Mengisi Baterai Perlu Penyimpanan." },
  { "en": "Apa Itu Inverter Hibrida (Hybrid Inverter)?", "id": "Inverter Gabungan On-Grid Dan Off-Grid Baterai + PLN." },
  { "en": "Apa Itu Proteksi Anti-Islanding (Anti-Islanding)?", "id": "Fitur Inverter On-Grid Mati Saat PLN Padam." },
  { "en": "Apa Itu Inverter Rangkai (String Inverter)?", "id": "Satu Inverter Besar Melayani Banyak Panel Surya." },
  { "en": "Apa Itu Inverter Mikro (Microinverter)?", "id": "Inverter Kecil Terpasang Langsung Di Setiap Panel." },
  { "en": "Apa Itu Pengoptimal Daya (Power Optimizer)?", "id": "Perangkat DC-DC MPPT Di Setiap Panel Bekerja String Inverter." },
  { "en": "Apa Itu Sistem Pemasangan (Mounting System) Surya?", "id": "Struktur Rangka Penyangga Panel Surya Di Atap." },
  { "en": "Apa Itu Pemasangan Atap (Rooftop Mounting)?", "id": "Memasang Panel Surya Di Atas Atap Bangunan." },
  { "en": "Apa Itu Pemasangan Darat (Ground Mounting)?", "id": "Memasang Panel Surya Di Struktur Rangka Tanah." },
  { "en": "Apa Itu Pelacak Surya (Solar Tracker)?", "id": "Struktur Pemasangan Bergerak Mengikuti Arah Matahari." },
  { "en": "Apa Itu Pelacak Sumbu Tunggal (Single-Axis Tracker)?", "id": "Pelacak Bergerak Satu Sumbu Timur-Barat." },
  { "en": "Apa Itu Pelacak Sumbu Ganda (Dual-Axis Tracker)?", "id": "Pelacak Bergerak Dua Sumbu Timur-Barat, Utara-Selatan." },
  { "en": "Apa Itu Sudut Azimut (Azimuth Angle) Surya?", "id": "Sudut Kompas Arah Panel Menghadap Utara Selatan." },
  { "en": "Apa Itu Sudut Kemiringan (Tilt Angle) Surya?", "id": "Sudut Kemiringan Panel Relatif Terhadap Horizontal." },
  { "en": "Apa Itu Irradiance (Iradiasi) Surya?", "id": "Kekuatan Daya Radiasi Matahari Watt Per Meter Persegi." },
  { "en": "Apa Itu Insolasi (Insolation) Surya?", "id": "Energi Total Radiasi Matahari KWh Per Meter Persegi Per Hari." },
  { "en": "Apa Itu Kondisi Uji Standar (Standard Test Conditions/STC)?", "id": "Kondisi Lab Standar Pengujian Panel Surya." },
  { "en": "Apa Itu RF (Radio Frequency)?", "id": "Frekuensi Elektromagnetik Dalam Rentang Spektrum Radio." },
  { "en": "Apa Rentang Frekuensi RF (Radio Frequency)?", "id": "Kira-Kira 3 KHz Hingga 300 GHz." },
  { "en": "Apa Itu Gelombang Radio (Radio Wave)?", "id": "Gelombang Elektromagnetik Digunakan Transmisi Data Nirkabel." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Proses Menumpangkan Sinyal Informasi Ke Gelombang Pembawa." },
  { "en": "Apa Itu Gelombang Pembawa (Carrier Wave)?", "id": "Gelombang RF Frekuensi Tinggi Membawa Sinyal Informasi." },
  { "en": "Apa Itu Sinyal Baseband (Baseband Signal)?", "id": "Sinyal Informasi Asli Frekuensi Rendah (Audio, Video)." },
  { "en": "Mengapa Perlu Modulasi (Modulation)?", "id": "Mengirim Sinyal Jarak Jauh, Efisiensi Antena, Multiplexing." },
  { "en": "Apa Itu Modulasi Amplitudo (Amplitude Modulation/AM)?", "id": "Modulasi Amplitudo Gelombang Pembawa Diubah Sesuai Sinyal Info." },
  { "en": "Apa Itu Modulasi Frekuensi (Frequency Modulation/FM)?", "id": "Modulasi Frekuensi Gelombang Pembawa Diubah Sesuai Sinyal Info." },
  { "en": "Apa Itu Modulasi Fasa (Phase Modulation/PM)?", "id": "Modulasi Fasa Gelombang Pembawa Diubah Sesuai Sinyal Info." },
  { "en": "Apa Keunggulan FM (Frequency Modulation) Dibanding AM (Amplitude Modulation)?", "id": "Kualitas Suara Lebih Baik, Jauh Lebih Tahan Terhadap Noise." },
  { "en": "Apa Itu Pita Sisi (Sideband) AM?", "id": "Komponen Frekuensi Baru (Atas Bawah) Dihasilkan Modulasi AM." },
  { "en": "Apa Itu DSB-SC (Double-Sideband Suppressed-Carrier)?", "id": "Modulasi AM (Kedua Pita Sisi) Tanpa (Gelombang Pembawa)." },
  { "en": "Apa Itu SSB (Single-Sideband)?", "id": "Modulasi AM Hanya Mengirimkan Satu Pita Sisi (Hemat Bandwidth)." },
  { "en": "Apa Itu Demodulasi (Demodulation/Deteksi)?", "id": "Proses Mengambil Kembali Sinyal Informasi Dari Gelombang Pembawa." },
  { "en": "Apa Itu Detektor Selubung (Envelope Detector)?", "id": "Rangkaian (Dioda, RC) Sederhana Demodulasi Sinyal AM." },
  { "en": "Apa Itu Modulasi Sudut (Angle Modulation)?", "id": "Kategori Modulasi Yang Mencakup FM Dan PM." },
  { "en": "Apa Itu Indeks Modulasi (Modulation Index) FM?", "id": "Ukuran Seberapa Banyak Frekuensi Pembawa Berubah (Deviasi)." },
  { "en": "Apa Itu Deviasi Frekuensi (Frequency Deviation) FM?", "id": "Pergeseran Frekuensi Maksimum Gelombang Pembawa FM." },
  { "en": "Apa Itu Aturan Carson (Carson's Rule) FM?", "id": "Estimasi Bandwidth Yang Dibutuhkan Sinyal FM." },
  { "en": "Apa Itu Modulasi Digital (Digital Modulation)?", "id": "Modulasi Menggunakan Sinyal Informasi Digital (Bit 0 Dan 1)." },
  { "en": "Apa Itu ASK (Amplitude-Shift Keying)?", "id": "Modulasi Digital Mengubah Amplitudo (On-Off Keying)." },
  { "en": "Apa Itu FSK (Frequency-Shift Keying)?", "id": "Modulasi Digital Mengubah Frekuensi (Frekuensi Beda 0/1)." },
  { "en": "Apa Itu PSK (Phase-Shift Keying)?", "id": "Modulasi Digital Mengubah Fasa (Fasa Beda 0/1)." },
  { "en": "Apa Itu BPSK (Binary Phase-Shift Keying)?", "id": "Modulasi PSK Menggunakan Dua Fasa (0 Dan 180 Derajat)." },
  { "en": "Apa Itu QPSK (Quadrature Phase-Shift Keying)?", "id": "Modulasi PSK Menggunakan Empat Fasa (Mengirim 2 Bit Sekaligus)." },
  { "en": "Apa Itu QAM (Quadrature Amplitude Modulation)?", "id": "Modulasi Digital Menggabungkan Perubahan Amplitudo Dan Fasa." },
  { "en": "Apa Itu Diagram Konstelasi (Constellation Diagram)?", "id": "Diagram Plot Titik Simbol Modulasi Digital (QPSK, QAM)." },
  { "en": "Apa Itu Simbol (Symbol) Modulasi Digital?", "id": "Satu Keadaan Sinyal (Titik Konstelasi) Mewakili Satu/Lebih Bit." },
  { "en": "Apa Itu Laju Bit (Bit Rate)?", "id": "Jumlah Bit Data Aktual Yang Dikirim Per Detik." },
  { "en": "Apa Itu Laju Simbol (Symbol Rate/Baud Rate)?", "id": "Jumlah Simbol (Perubahan Sinyal) Yang Dikirim Per Detik." },
  { "en": "Apa Itu Efisiensi Spektral (Spectral Efficiency)?", "id": "Ukuran Bit Per Detik Per Hertz (Seberapa Efisien Bandwidth)." },
  { "en": "Apa Itu BER (Bit Error Rate)?", "id": "Rasio Jumlah Bit Error Dibanding Total Bit Terkirim." },
  { "en": "Apa Itu SNR (Signal-to-Noise Ratio) RF?", "id": "Rasio Kekuatan Sinyal Yang Diinginkan Terhadap Noise Latar." },
  { "en": "Apa Itu OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Teknik Modulasi Digital (Banyak Subcarrier) Tahan Multipath." },
  { "en": "Apa Aplikasi OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Wi-Fi, 4G LTE, 5G, Televisi Digital (DVB-T2)." },
  { "en": "Apa Itu Multipath (Multipath Fading)?", "id": "Sinyal Diterima Dari Banyak Jalur Pantulan (Interferensi)." },
  { "en": "Apa Itu Spektrum Tersebar (Spread Spectrum)?", "id": "Teknik Menyebarkan Sinyal (Bandwidth Lebar) Tahan Interferensi." },
  { "en": "Apa Itu FHSS (Frequency-Hopping Spread Spectrum)?", "id": "Spread Spectrum (Frekuensi) Sinyal Melompat-Lompat Cepat." },
  { "en": "Apa Itu DSSS (Direct-Sequence Spread Spectrum)?", "id": "Spread Spectrum (Sinyal) Dikali (Kode Chip) Cepat." },
  { "en": "Apa Itu Kode Chip (Chipping Code/PN Code)?", "id": "Kode Pseudo-Random Digunakan Dalam DSSS (GPS, CDMA)." },
  { "en": "Apa Itu CDMA (Code-Division Multiple Access)?", "id": "Akses Ganda (Setiap Pengguna) Diberi (Kode Unik)." },
  { "en": "Apa Itu TDMA (Time-Division Multiple Access)?", "id": "Akses Ganda (Setiap Pengguna) Diberi (Slot Waktu) Bergantian." },
  { "en": "Apa Itu FDMA (Frequency-Division Multiple Access)?", "id": "Akses Ganda (Setiap Pengguna) Diberi (Saluran Frekuensi) Berbeda." },
  { "en": "Apa Itu Penerima (Receiver) RF?", "id": "Perangkat Elektronik Menerima Memproses Sinyal Radio." },
  { "en": "Apa Itu Pemancar (Transmitter) RF?", "id": "Perangkat Elektronik Menghasilkan Memancarkan Sinyal Radio." },
  { "en": "Apa Itu Transceiver (Transceiver)?", "id": "Perangkat Gabungan Pemancar (Transmitter) Dan Penerima (Receiver)." },
  { "en": "Apa Itu Penerima Superheterodyne (Superheterodyne)?", "id": "Arsitektur (Penerima) Paling Umum (Menggeser RF) Ke (IF)." },
  { "en": "Apa Itu Frekuensi Menengah (Intermediate Frequency/IF)?", "id": "Frekuensi (Tetap, Lebih Rendah) Hasil (Pencampuran) Sinyal RF." },
  { "en": "Apa Itu Osilator Lokal (Local Oscillator/LO)?", "id": "Osilator (Internal Penerima) Sinyal (Dicampur) Dengan RF." },
  { "en": "Apa Itu Pencampur (Mixer) RF?", "id": "Rangkaian (Non-Linear) Mencampur (Dua Frekuensi) (Menghasilkan IF)." },
  { "en": "Apa Itu Frekuensi Bayangan (Image Frequency)?", "id": "Frekuensi (Tidak Diinginkan) (Juga Menghasilkan) IF (Yang Sama)." },
  { "en": "Apa Itu Filter Bayangan (Image Rejection Filter)?", "id": "Filter (Bandpass RF) (Menolak) Frekuensi Bayangan." },
  { "en": "Apa Itu LNA (Low-Noise Amplifier)?", "id": "Penguat (RF) Tahap (Awal) (Sangat Rendah Noise)." },
  { "en": "Apa Itu Angka Derau (Noise Figure/NF)?", "id": "Ukuran (Tambahan Noise) Yang (Dihasilkan) Penguat (dB)." },
  { "en": "Apa Itu Penerima Direct-Conversion (Zero-IF)?", "id": "Arsitektur (Penerima) (Langsung Menggeser) RF (Ke Baseband) (IF = 0)." },
  { "en": "Apa Itu Kebocoran LO (LO Leakage)?", "id": "Masalah (Penerima Direct-Conversion) (Sinyal LO) Bocor (Ke Input)." },
  { "en": "Apa Itu Penguat Daya (Power Amplifier/PA) RF?", "id": "Penguat (Tahap Akhir) (Pemancar) (Daya Output Tinggi)." },
  { "en": "Apa Itu Efisiensi (Efficiency) PA (Power Amplifier) RF?", "id": "Rasio (Daya Output RF) Dibanding (Daya Input DC)." },
  { "en": "Apa Itu P1dB (1 dB Compression Point)?", "id": "Titik (Daya Input) (Gain Penguat) Turun (1 dB) (Non-Linear)." },
  { "en": "Apa Itu Titik Intersep Orde Ketiga (IP3)?", "id": "Ukuran (Linearitas) Penguat (Intermodulasi)." },
  { "en": "Apa Itu Distorsi Intermodulasi (Intermodulation Distortion/IMD)?", "id": "Produk (Frekuensi Palsu) (Akibat) Non-Linearitas (Dua Sinyal)." },
  { "en": "Apa Itu Filter (Filter) RF?", "id": "Komponen (Melewatkan) Frekuensi (Diinginkan) (Memblokir) Lainnya." },
  { "en": "Apa Itu Filter SAW (Surface Acoustic Wave)?", "id": "Filter (RF) (Ukuran Kecil) (Selektivitas Tinggi) (Ponsel)." },
  { "en": "Apa Itu Filter BAW (Bulk Acoustic Wave)?", "id": "Filter (RF) (Frekuensi Lebih Tinggi) Dari SAW (Ponsel)." },
  { "en": "Apa Itu Filter Rongga (Cavity Filter)?", "id": "Filter (Logam Besar) (Daya Tinggi, Rugi Rendah) (BTS)." },
  { "en": "Apa Itu Filter LC (LC Filter) RF?", "id": "Filter (Sederhana) Menggunakan (Induktor) Dan (Kapasitor)." },
  { "en": "Apa Itu Osilator (Oscillator) RF?", "id": "Rangkaian (Menghasilkan) Sinyal (Gelombang Pembawa) RF (Stabil)." },
  { "en": "Apa Itu Osilator Kristal (Crystal Oscillator/XO)?", "id": "Osilator (Sangat Stabil) Menggunakan (Resonansi) Kristal Kuarsa." },
  { "en": "Apa Itu TCXO (Temperature-Compensated XO)?", "id": "Osilator (Kristal) (Stabilitas) Ditingkatkan (Terhadap Suhu)." },
  { "en": "Apa Itu OCXO (Oven-Controlled XO)?", "id": "Osilator (Kristal) (Sangat Stabil) (Kristal) Diletakkan (Di Oven)." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator (Frekuensi) Dapat (Diatur) Oleh (Tegangan) Input." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem (Umpan Balik) (Mengunci) Fasa (VCO) (Ke Referensi Kristal)." },
  { "en": "Apa Itu Penyintesis Frekuensi (Frequency Synthesizer)?", "id": "Rangkaian (PLL) (Menghasilkan) Berbagai (Frekuensi) Presisi." },
  { "en": "Apa Itu Sirkulator (Circulator) RF?", "id": "Perangkat (Tiga Port) (Sinyal) Hanya (Mengalir) Satu Arah (Port 1->2, 2->3)." },
  { "en": "Apa Itu Isolator (Isolator) RF?", "id": "Sirkulator (Satu Port) Diterminasi (Sinyal) Hanya (Satu Arah)." },
  { "en": "Apa Itu Duplekser (Duplexer) RF?", "id": "Filter (Memungkinkan) (Pemancar, Penerima) (Berbagi) Satu Antena." },
  { "en": "Apa Itu Kopler Direksional (Directional Coupler)?", "id": "Perangkat (Mencuplik) Sebagian (Kecil) Sinyal (RF) (Mengukur Daya)." },
  { "en": "Apa Itu Atenuator (Attenuator) RF?", "id": "Komponen (Resistif) (Melemahkan) Level (Sinyal) RF (Tetap/Variabel)." },
  { "en": "Apa Itu Saklar (Switch) RF?", "id": "Saklar (Elektronik) (Memilih) Jalur (Sinyal) RF (PIN Diode, FET)." },
  { "en": "Apa Itu Dioda PIN (PIN Diode)?", "id": "Dioda (Lapisan Intrinsik) (Digunakan) Sebagai (Saklar/Atenuator) RF." },
  { "en": "Apa Itu Dioda Varaktor (Varactor Diode)?", "id": "Dioda (Kapasitansi) Berubah (Sesuai) Tegangan (Mundur) (VCO)." },
  { "en": "Apa Itu Rantai RF (RF Chain)?", "id": "Urutan (Semua Komponen) Dari (Antena) Ke (Baseband)." },
  { "en": "Apa Itu Papan Smith (Smith Chart)?", "id": "Grafik (Kompleks) (Membantu) Perhitungan (Pencocokan Impedansi) RF." },
  { "en": "Apa Itu Pencocokan Jaringan (Matching Network)?", "id": "Rangkaian (LC) (Mengubah) Satu (Impedansi) Ke (Impedansi Lain)." },
  { "en": "Apa Itu Stub (Stub) Saluran Transmisi?", "id": "Potongan (Saluran Transmisi) (Hubung Singkat/Terbuka) (Matching)." },
  { "en": "Apa Itu Propagasi (Propagation) Gelombang Radio?", "id": "Cara (Gelombang Radio) Merambat (Melalui Ruang)." },
  { "en": "Apa Itu Propagasi Garis Pandang (Line-of-Sight/LOS)?", "id": "Propagasi (Frekuensi Tinggi) (Lurus) (Antena Harus Terlihat)." },
  { "en": "Apa Itu Propagasi Gelombang Tanah (Ground Wave)?", "id": "Propagasi (Frekuensi Rendah) (Mengikuti) Kontur (Permukaan Bumi)." },
  { "en": "Apa Itu Propagasi Gelombang Langit (Skywave)?", "id": "Propagasi (Frekuensi HF) (Dipantulkan) Oleh (Ionosfer)." },
  { "en": "Apa Itu Ionosfer (Ionosphere)?", "id": "Lapisan (Atmosfer) (Mengandung Ion) (Memantulkan) Gelombang HF." },
  { "en": "Apa Itu Fading (Fading)?", "id": "Fluktuasi (Kekuatan Sinyal) (Akibat) Interferensi (Multipath)." },
  { "en": "Apa Itu Zona Fresnel (Fresnel Zone)?", "id": "Area (Berbentuk Elips) (Antara Pemancar-Penerima) (Harus Bebas)." },
  { "en": "Apa Itu Rugi-Rugi Ruang Bebas (Free-Space Path Loss)?", "id": "Pelemahan (Sinyal) (Alami) (Saat Merambat) Di Ruang Hampa." },
  { "en": "Apa Itu Taut Nirkabel (Wireless Link Budget)?", "id": "Perhitungan (Semua Gain Dan Loss) (Memastikan) Sinyal (Cukup)." },
  { "en": "Apa Itu EIRP (Effective Isotropic Radiated Power)?", "id": "Daya (Total) (Dipancarkan) Antena (Relatif) Ke Isotropik." },
  { "en": "Apa Itu Sensitivitas (Sensitivity) Penerima?", "id": "Level (Sinyal Input) (Minimum) (Yang Dapat) Dideteksi (Penerima)." },
  { "en": "Apa Itu SDR (Software-Defined Radio)?", "id": "Radio (Fungsi Modulasi/Demodulasi) (Dilakukan) Perangkat Lunak." },
  { "en": "Apa Keunggulan SDR (Software-Defined Radio)?", "id": "Sangat (Fleksibel) (Dapat Berubah) Protokol (Hanya) Update Software." },
  { "en": "Apa Itu Radio Kognitif (Cognitive Radio)?", "id": "SDR (Cerdas) (Mendeteksi) Spektrum (Kosong) (Menggunakannya)." },
  { "en": "Apa Itu Manajemen Spektrum (Spectrum Management)?", "id": "Proses (Alokasi) (Regulasi) Frekuensi (Radio) (Oleh Pemerintah)." },
  { "en": "Apa Itu Antena Cerdas (Smart Antenna)?", "id": "Antena Menggunakan Pemrosesan Sinyal Digital Mengubah Pola Radiasi." },
  { "en": "Apa Itu Pembentukan Berkas (Beamforming)?", "id": "Teknik Antena Memfokuskan Sinyal Ke Arah Pengguna Tertentu." },
  { "en": "Apa Itu MIMO (Multiple-Input Multiple-Output)?", "id": "Sistem Nirkabel Menggunakan Banyak Antena Pemancar Penerima." },
  { "en": "Apa Keunggulan MIMO (Multiple-Input Multiple-Output)?", "id": "Meningkatkan Kecepatan Data (Throughput) Dan Keandalan." },
  { "en": "Apa Itu Keanekaragaman Spasial (Spatial Diversity)?", "id": "Mengirim Sinyal Sama Lewat Antena Berbeda (Mengurangi Fading)." },
  { "en": "Apa Itu Multiplexing Spasial (Spatial Multiplexing)?", "id": "Mengirim Sinyal Berbeda Lewat Antena Berbeda (Menambah Kecepatan)." },
  { "en": "Apa Itu Massive MIMO (MIMO Masif)?", "id": "Sistem MIMO Menggunakan Ratusan Antena Di Base Station (5G)." },
  { "en": "Apa Itu Jaringan Ad Hoc (Ad Hoc Network)?", "id": "Jaringan Nirkabel (Peer-to-Peer) Terdesentralisasi Tanpa Infrastruktur." },
  { "en": "Apa Itu Jaringan Mesh (Mesh Network)?", "id": "Jaringan (Node) Saling Terhubung (Data Melompat) Antar Node." },
  { "en": "Apa Itu Perutean (Routing) Jaringan Mesh?", "id": "Proses Node Menemukan Jalur Terbaik Mengirim Data." },
  { "en": "Apa Itu Jaringan Sensor Nirkabel (WSN)?", "id": "Jaringan (Node Sensor) Kecil Terdistribusi (Monitoring Lingkungan)." },
  { "en": "Apa Itu Protokol Zigbee (Zigbee)?", "id": "Standar Nirkabel (Daya Rendah, Jaringan Mesh) Untuk WSN/IoT." },
  { "en": "Apa Itu Protokol Z-Wave (Z-Wave)?", "id": "Standar Nirkabel (Daya Rendah, Mesh) Populer (Otomasi Rumah)." },
  { "en": "Apa Itu Bluetooth (Bluetooth)?", "id": "Standar Nirkabel (Jarak Pendek) (Audio, Transfer Data)." },
  { "en": "Apa Itu Bluetooth Low Energy (BLE)?", "id": "Versi (Bluetooth) Hemat Daya (Untuk Perangkat) IoT (Beacon)." },
  { "en": "Apa Itu Beacon (Beacon) BLE?", "id": "Pemancar (BLE) Satu Arah (Menyiarkan) ID (Lokasi Indoor)." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Standar (Jaringan Area Lokal) Nirkabel (WLAN) (IEEE 802.11)." },
  { "en": "Apa Itu Standar 802.11ax (Wi-Fi 6)?", "id": "Standar Wi-Fi (Generasi Keenam) (Efisiensi Tinggi) Area Padat." },
  { "en": "Apa Itu OFDMA (Orthogonal Frequency-Division Multiple Access)?", "id": "Fitur (Wi-Fi 6) (Membagi Saluran) Untuk (Banyak Pengguna) Efisien." },
  { "en": "Apa Itu TWT (Target Wake Time)?", "id": "Fitur (Wi-Fi 6) (Mengatur Jadwal Tidur) Perangkat (Hemat Baterai)." },
  { "en": "Apa Itu Wi-Fi Direct (Wi-Fi Direct)?", "id": "Standar (Wi-Fi) (Koneksi Langsung) Antar Perangkat (Tanpa Router)." },
  { "en": "Apa Itu WPS (Wi-Fi Protected Setup)?", "id": "Metode (Mudah) Menghubungkan (Perangkat) Ke (Wi-Fi) (Tombol Tekan)." },
  { "en": "Apa Itu NFC (Near Field Communication)?", "id": "Standar (Nirkabel) (Jarak Sangat Dekat) (4 Cm) (Pembayaran)." },
  { "en": "Apa Itu RFID (Radio-Frequency Identification)?", "id": "Teknologi (Identifikasi) Menggunakan (Tag Pasif) Dan (Pembaca Aktif)." },
  { "en": "Apa Itu Tag RFID (RFID Tag) Pasif?", "id": "Tag (Tanpa Baterai) (Daya) Didapat (Dari Sinyal) Pembaca." },
  { "en": "Apa Itu Tag RFID (RFID Tag) Aktif?", "id": "Tag (Memiliki Baterai) (Dapat Memancar) Sinyal (Jarak Jauh)." },
  { "en": "Apa Itu Backscatter (Hamburan Balik) RFID?", "id": "Cara (Tag Pasif) (Memantulkan) Sinyal (Pembaca) (Membawa Data)." },
  { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem (Navigasi Satelit) (Menentukan) Lokasi (Koordinat) Global." },
  { "en": "Bagaimana GPS (Global Positioning System) Bekerja?", "id": "Menerima (Sinyal Waktu) Dari (Minimal 4) Satelit (Trilaterasi)." },
  { "en": "Apa Itu GNSS (Global Navigation Satellite System)?", "id": "Istilah (Umum) (Sistem Navigasi) Satelit (GPS, GLONASS, Galileo)." },
  { "en": "Apa Itu GLONASS (GLONASS)?", "id": "Sistem (Navigasi Satelit) GNSS (Milik Rusia)." },
  { "en": "Apa Itu Galileo (Galileo)?", "id": "Sistem (Navigasi Satelit) GNSS (Milik Uni Eropa)." },
  { "en": "Apa Itu BeiDou (BeiDou)?", "id": "Sistem (Navigasi Satelit) GNSS (Milik Tiongkok)." },
  { "en": "Apa Itu GPS Diferensial (Differential GPS/DGPS)?", "id": "Teknik (Meningkatkan) Akurasi (GPS) (Menggunakan Stasiun Referensi)." },
  { "en": "Apa Itu RTK (Real-Time Kinematic) GPS?", "id": "Teknik (DGPS) (Akurasi Sangat Tinggi) (Level Sentimeter)." },
  { "en": "Apa Itu Komunikasi Satelit (Satellite Communication)?", "id": "Komunikasi (RF) Menggunakan (Satelit) Di Orbit (Sebagai Relay)." },
  { "en": "Apa Itu Orbit Geostasioner (GEO)?", "id": "Orbit (Satelit) Tampak (Diam) Di Atas (Ekuator) (35.786 Km)." },
  { "en": "Apa Itu Orbit Bumi Rendah (LEO)?", "id": "Orbit (Satelit) Rendah (Cepat) (Latensi Rendah) (Starlink)." },
  { "en": "Apa Itu Orbit Bumi Menengah (MEO)?", "id": "Orbit (Antara LEO Dan GEO) (Satelit Navigasi GPS)." },
  { "en": "Apa Itu Uplink (Uplink) Satelit?", "id": "Transmisi (Sinyal) Dari (Stasiun Bumi) Ke (Satelit)." },
  { "en": "Apa Itu Downlink (Downlink) Satelit?", "id": "Transmisi (Sinyal) Dari (Satelit) Ke (Stasiun Bumi)." },
  { "en": "Apa Itu Transponder (Transponder) Satelit?", "id": "Perangkat (Relay) (Menerima, Menguatkan, Mengubah Frekuensi) Sinyal." },
  { "en": "Apa Itu Stasiun Bumi (Earth Station/Gateway)?", "id": "Fasilitas (Darat) (Antena Parabola Besar) (Komunikasi Satelit)." },
  { "en": "Apa Itu VSAT (Very Small Aperture Terminal)?", "id": "Terminal (Satelit) Kecil (Dua Arah) (Data, Internet Jauh)." },
  { "en": "Apa Itu Pita-C (C-Band)?", "id": "Frekuensi (Satelit) (4-8 GHz) (Tahan Hujan) (Antena Besar)." },
  { "en": "Apa Itu Pita-Ku (Ku-Band)?", "id": "Frekuensi (Satelit) (12-18 GHz) (Rentan Hujan) (Antena Kecil/TV)." },
  { "en": "Apa Itu Pita-Ka (Ka-Band)?", "id": "Frekuensi (Satelit) (26-40 GHz) (Kapasitas Besar) (Internet Cepat)." },
  { "en": "Apa Itu Fading Hujan (Rain Fade)?", "id": "Pelemahan (Sinyal Satelit) (Frekuensi Tinggi) (Akibat Hujan)." },
  { "en": "Apa Itu Jaringan Seluler (Cellular Network)?", "id": "Jaringan (Nirkabel) (Area Terbagi) Menjadi (Sel-Sel Kecil)." },
  { "en": "Apa Itu BTS (Base Transceiver Station)?", "id": "Peralatan (Pemancar/Penerima) Radio (Di Menara) Seluler (2G)." },
  { "en": "Apa Itu NodeB (NodeB)?", "id": "Nama (Lain) BTS (Untuk Jaringan) Seluler (3G)." },
  { "en": "Apa Itu ENodeB (Evolved NodeB)?", "id": "Nama (Lain) BTS (Untuk Jaringan) Seluler (4G LTE)." },
  { "en": "Apa Itu GNodeB (Next Generation NodeB)?", "id": "Nama (Lain) BTS (Untuk Jaringan) Seluler (5G)." },
  { "en": "Apa Itu Handoff (Handover) Seluler?", "id": "Proses (Perpindahan) Koneksi (Otomatis) Antar (Sel/BTS)." },
  { "en": "Apa Itu Kartu SIM (Subscriber Identity Module)?", "id": "Kartu (Pintar) Menyimpan (Identitas) Pelanggan (Jaringan Seluler)." },
  { "en": "Apa Itu IMEI (International Mobile Equipment Identity)?", "id": "Nomor (Identitas) Unik (Perangkat Keras) Ponsel." },
  { "en": "Apa Itu 1G (Generasi Pertama)?", "id": "Jaringan (Seluler) Analog (Hanya Suara) (Contoh: AMPS, NMT)." },
  { "en": "Apa Itu 2G (Generasi Kedua)?", "id": "Jaringan (Seluler) Digital (Suara Dan SMS) (Contoh: GSM, CDMA)." },
  { "en": "Apa Itu GSM (Global System for Mobile Communications)?", "id": "Standar (2G) Paling (Populer) Di Dunia (Menggunakan SIM Card)." },
  { "en": "Apa Itu GPRS (General Packet Radio Service)?", "id": "Upgrade (2.5G) (Layanan Data) Paket (Pada Jaringan) GSM." },
  { "en": "Apa Itu EDGE (Enhanced Data rates for GSM Evolution)?", "id": "Upgrade (2.75G) (Kecepatan Data) Lebih (Cepat) Dari GPRS." },
  { "en": "Apa Itu 3G (Generasi Ketiga)?", "id": "Jaringan (Seluler) (Data Cepat) (Internet Mobile) (Contoh: UMTS/HSPA)." },
  { "en": "Apa Itu 4G LTE (Fourth Generation Long-Term Evolution)?", "id": "Jaringan (Seluler) (Data Sangat Cepat) (Berbasis IP) (OFDM)." },
  { "en": "Apa Itu 5G (Generasi Kelima)?", "id": "Jaringan (Seluler) (Terbaru) (Sangat Cepat, Latensi Rendah, IoT)." },
  { "en": "Apa Itu MmWave (Milimeter Wave) 5G?", "id": "Frekuensi (5G) (Sangat Tinggi) (Super Cepat, Jarak Sangat Pendek)." },
  { "en": "Apa Itu Sub-6 GHz (Sub-6 GHz) 5G?", "id": "Frekuensi (5G) (Di Bawah 6 GHz) (Cakupan Luas, Kecepatan Baik)." },
  { "en": "Apa Itu Pembelahan Jaringan (Network Slicing) 5G?", "id": "Membuat (Jaringan Virtual) (Independen) (Di Atas Infrastruktur) Fisik (5G)." },
  { "en": "Apa Itu Komunikasi Optik (Optical Communication)?", "id": "Transmisi (Data) Menggunakan (Sinyal) Cahaya (Melalui Serat Optik)." },
  { "en": "Apa Itu Serat Optik (Optical Fiber)?", "id": "Media (Kaca/Plastik) Tipis (Pemandu) Gelombang (Cahaya)." },
  { "en": "Apa Itu Inti (Core) Serat Optik?", "id": "Bagian (Tengah) Serat (Tempat) Cahaya (Merambat)." },
  { "en": "Apa Itu Pelapis (Cladding) Serat Optik?", "id": "Lapisan (Luar) Inti (Indeks Bias Lebih Rendah) (Memantulkan Cahaya)." },
  { "en": "Apa Itu Pantulan Internal Total (Total Internal Reflection)?", "id": "Prinsip (Fisika) (Cahaya Terjebak) Merambat (Dalam Inti Serat)." },
  { "en": "Apa Itu Serat Mode Tunggal (Single-Mode Fiber/SMF)?", "id": "Serat (Inti Sangat Kecil) (Satu Jalur Cahaya) (Jarak Jauh)." },
  { "en": "Apa Itu Serat Mode Jamak (Multi-Mode Fiber/MMF)?", "id": "Serat (Inti Lebih Besar) (Banyak Jalur Cahaya) (Jarak Pendek)." },
  { "en": "Apa Itu Dispersi (Dispersion) Serat Optik?", "id": "Fenomena (Penyebaran) Pulsa (Cahaya) (Seiring Jarak) (Membatasi Kecepatan)." },
  { "en": "Apa Itu Dispersi Kromatik (Chromatic Dispersion)?", "id": "Dispersi (Akibat) Kecepatan (Warna/Panjang Gelombang) Cahaya (Berbeda)." },
  { "en": "Apa Itu Dispersi Modal (Modal Dispersion)?", "id": "Dispersi (Hanya Di Multi-Mode) (Akibat) Waktu (Tempuh Jalur) Berbeda." },
  { "en": "Apa Itu Redaman (Attenuation) Serat Optik?", "id": "Pelemahan (Kekuatan Sinyal) Cahaya (Sepanjang Serat) (Satuan dB/km)." },
  { "en": "Apa Itu Jendela (Window) Transmisi Optik?", "id": "Rentang (Panjang Gelombang) (Dimana) Redaman (Serat) Paling Rendah." },
  { "en": "Berapa Jendela (Window) Transmisi Serat Optik?", "id": "Sekitar 850nm (MMF), 1310nm, Dan 1550nm (SMF)." },
  { "en": "Apa Itu Sumber Cahaya (Light Source) Optik?", "id": "Perangkat (Pemancar) Sinyal Cahaya (LED Atau Laser)." },
  { "en": "Apa Itu Dioda Laser (Laser Diode)?", "id": "Sumber (Cahaya Koheren) (Daya Tinggi, Cepat) (Untuk Single-Mode)." },
  { "en": "Apa Itu LED (Light Emitting Diode) Optik?", "id": "Sumber (Cahaya Inkoheren) (Murah, Lambat) (Untuk Multi-Mode)." },
  { "en": "Apa Itu Detektor Foto (Photodetector) Optik?", "id": "Perangkat (Penerima) (Mengubah Sinyal) Cahaya (Menjadi Listrik)." },
  { "en": "Apa Itu Fotodioda (Photodiode) PIN?", "id": "Detektor (Foto) Umum (Kecepatan Sedang, Sensitivitas Baik)." },
  { "en": "Apa Itu Fotodioda Longsor (APD)?", "id": "Detektor (Foto) (Sangat Sensitif) (Memiliki Penguatan Internal)." },
  { "en": "Apa Kepanjangan APD (Avalanche Photodiode)?", "id": "Fotodioda Longsor." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Teknik (Mengirim) Banyak (Sinyal/Warna) (Dalam Satu) Serat." },
  { "en": "Apa Itu DWDM (Dense WDM)?", "id": "WDM (Jarak Saluran) Sangat Rapat (Kapasitas Sangat Tinggi)." },
  { "en": "Apa Itu CWDM (Coarse WDM)?", "id": "WDM (Jarak Saluran) Kasar (Lebih Murah, Kapasitas Rendah)." },
  { "en": "Apa Itu Mux/Demux (Multiplexer) Optik?", "id": "Perangkat (Menggabungkan/Memisahkan) Sinyal (Panjang Gelombang) WDM." },
  { "en": "Apa Itu EDFA (Erbium-Doped Fiber Amplifier)?", "id": "Penguat (Optik) (Serat Didoping Erbium) (Menguatkan Sinyal 1550nm)." },
  { "en": "Apa Itu Penguat Optik (Optical Amplifier)?", "id": "Menguatkan (Sinyal Cahaya) Langsung (Tanpa Konversi Listrik)." },
  { "en": "Apa Itu Penyambungan Fusi (Fusion Splicing)?", "id": "Metode (Menyambung) Dua (Serat Optik) (Permanen) (Peleburan/Las)." },
  { "en": "Apa Itu Konektor (Connector) Serat Optik?", "id": "Konektor (Sambungan) Serat Optik (Tidak Permanen) (SC, LC)." },
  { "en": "Apa Itu OTDR (Optical Time-Domain Reflectometer)?", "id": "Alat (Uji) (Mencari) Lokasi (Putus/Redaman) Serat (Pantulan Cahaya)." },
  { "en": "Apa Itu OPM (Optical Power Meter)?", "id": "Alat (Ukur) (Kekuatan) Daya (Sinyal) Optik (dBm)." },
  { "en": "Apa Itu OLS (Optical Light Source)?", "id": "Sumber (Cahaya) (Stabil) (Digunakan) Bersama (OPM) (Mengukur Rugi-Rugi)." },
  { "en": "Apa Itu PON (Passive Optical Network)?", "id": "Jaringan (Serat Ke Rumah) (Menggunakan) Pemecah (Splitter) Pasif." },
  { "en": "Apa Itu OLT (Optical Line Terminal)?", "id": "Perangkat (Sentral) (Provider) Dalam Jaringan PON." },
  { "en": "Apa Itu ONU/ONT (Optical Network Unit/Terminal)?", "id": "Perangkat (Di Sisi) Pelanggan (Rumah) Dalam Jaringan PON." },
  { "en": "Apa Itu Pemecah (Splitter) Optik Pasif?", "id": "Komponen (Membagi) Sinyal (Cahaya) (Satu Ke Banyak) (Tanpa Listrik)." },
  { "en": "Apa Itu Ruang Bersih (Cleanroom)?", "id": "Ruangan Sangat Bersih Untuk Fabrikasi Semikonduktor." },
  { "en": "Mengapa Ruang Bersih (Cleanroom) Digunakan?", "id": "Mencegah Kontaminasi Partikel Debu Pada Chip." },
  { "en": "Apa Itu Standar ISO (International Organization for Standardization) 14644-1?", "id": "Standar Klasifikasi Kebersihan Udara Ruang Bersih." },
  { "en": "Apa Itu Ruang Bersih Kelas 100 (Class 100)?", "id": "Kurang Dari 100 Partikel Per Kaki Kubik Udara." },
  { "en": "Apa Itu Filter HEPA (High-Efficiency Particulate Air)?", "id": "Filter Udara Efisiensi Tinggi Menyaring Partikel Besar." },
  { "en": "Apa Itu Filter ULPA (Ultra-Low Particulate Air)?", "id": "Filter Udara Partikulat Sangat Rendah Lebih Ketat HEPA." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit Ke Wafer (Sinar UV)." },
  { "en": "Apa Itu Photoresist (Photoresist)?", "id": "Bahan Kimia Peka Cahaya Digunakan Fotolitografi." },
  { "en": "Apa Itu Photoresist (Photoresist) Positif?", "id": "Area Terkena UV Larut (Menjadi Pola Masker)." },
  { "en": "Apa Itu Photoresist (Photoresist) Negatif?", "id": "Area Terkena UV Mengeras (Menjadi Pola Masker)." },
  { "en": "Apa Itu Masker (Photomask)?", "id": "Pelat Kaca (Berpola Kromium) Cetakan Pola Sirkuit." },
  { "en": "Apa Itu Stepper (Stepper Lithography)?", "id": "Mesin Litografi Memproyeksikan Pola Masker Ke Wafer." },
  { "en": "Apa Itu Etsa (Etching) Dalam Fabrikasi?", "id": "Proses Menghilangkan Material Yang Tidak Dilindungi Photoresist." },
  { "en": "Apa Itu Etsa Basah (Wet Etching)?", "id": "Proses Etsa Menggunakan Bahan Kimia Cair (Asam)." },
  { "en": "Apa Itu Etsa Kering (Dry Etching)?", "id": "Proses Etsa Menggunakan Gas Reaktif (Plasma)." },
  { "en": "Apa Itu RIE (Reactive Ion Etching)?", "id": "Teknik Etsa Kering (Plasma) Arah Vertikal Presisi." },
  { "en": "Apa Itu Etsa Anisotropik (Anisotropic Etching)?", "id": "Etsa Ke Satu Arah (Vertikal) Jauh Lebih Cepat." },
  { "en": "Apa Itu Etsa Isotropik (Isotropic Etching)?", "id": "Etsa Ke Segala Arah (Sama Cepat) (Membulat)." },
  { "en": "Apa Itu Deposisi (Deposition)?", "id": "Proses Menambahkan Lapisan Material Baru Ke Wafer." },
  { "en": "Apa Itu CVD (Chemical Vapor Deposition)?", "id": "Deposisi Menggunakan Reaksi Gas Kimia Di Permukaan Panas." },
  { "en": "Apa Itu PVD (Physical Vapor Deposition)?", "id": "Deposisi Menggunakan Metode Fisik (Evaporasi, Sputtering)." },
  { "en": "Apa Itu Sputtering (Sputtering)?", "id": "Deposisi PVD (Menembakkan Ion) Ke Target Material." },
  { "en": "Apa Itu Evaporasi (Evaporation) Deposisi?", "id": "Deposisi PVD (Memanaskan) Material (Hingga Menguap) Di Vakum." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Menambahkan (Impuritas) Mengubah Sifat Listrik Semikonduktor." },
  { "en": "Apa Itu Implantasi Ion (Ion Implantation)?", "id": "Metode (Doping) Menembakkan (Ion Dopant) Ke Wafer." },
  { "en": "Apa Itu Difusi (Diffusion) Doping?", "id": "Metode (Doping) (Memanaskan Wafer) Dalam (Gas Dopant)." },
  { "en": "Apa Itu Wafer (Wafer) Semikonduktor?", "id": "Kepingan (Tipis Bulat) Silikon (Bahan Dasar) Chip." },
  { "en": "Apa Bahan Wafer (Wafer) Semikonduktor?", "id": "Silikon (Si) Monokristalin Kemurnian Sangat Tinggi." },
  { "en": "Apa Itu Die (Die) Pada Wafer?", "id": "Satu (Potongan Chip) Individual (Di Atas) Wafer." },
  { "en": "Apa Itu Pemotongan (Dicing) Wafer?", "id": "Proses (Memotong) Wafer (Menjadi) Kepingan Die Individual." },
  { "en": "Apa Itu Ikatan (Bonding) Die?", "id": "Proses (Melekatkan) Die (Chip) (Ke Substrat) Kemasan." },
  { "en": "Apa Itu Ikatan Kawat (Wire Bonding)?", "id": "Menyambung (Pad Die) Ke (Kaki Kemasan) (Kawat Emas)." },
  { "en": "Apa Bahan Kawat (Wire Bonding)?", "id": "Umumnya Emas (Au) Atau Aluminium (Al)." },
  { "en": "Apa Itu Ikatan Flip-Chip (Flip-Chip)?", "id": "Metode (Memasang Die) (Terbalik) (Menggunakan Bola Solder)." },
  { "en": "Apa Itu Bola Solder (Solder Bumps)?", "id": "Bola (Timah) Kecil (Koneksi) Pada (Ikatan Flip-Chip)." },
  { "en": "Apa Itu Pengemasan IC (Integrated Circuit)?", "id": "Proses (Melindungi Die) Dan (Memberi Koneksi) Eksternal (Pin)." },
  { "en": "Apa Itu Rangka Timah (Leadframe)?", "id": "Rangka (Logam) (Internal) Kemasan (Menjadi Kaki Pin)." },
  { "en": "Apa Itu Enkapsulasi (Encapsulation) IC?", "id": "Proses (Membungkus) Die (Dengan Plastik/Epoxy) (Molding)." },
  { "en": "Apa Itu Kemasan DIP (Dual In-Line Package)?", "id": "Kemasan (THT) (Dua Baris) Kaki (Pin) Paralel." },
  { "en": "Apa Itu Kemasan SOIC (Small Outline Integrated Circuit)?", "id": "Kemasan (SMD) (Bentuk Mirip DIP) (Kaki Sayap Camar)." },
  { "en": "Apa Itu Kemasan QFP (Quad Flat Package)?", "id": "Kemasan (SMD) (Kaki) Di (Keempat Sisi) (Sayap Camar)." },
  { "en": "Apa Itu Kemasan BGA (Ball Grid Array)?", "id": "Kemasan (SMD) (Koneksi) (Bola Solder) Di (Bawah)." },
  { "en": "Apa Itu Kemasan CSP (Chip-Scale Package)?", "id": "Kemasan (Sangat Kecil) (Ukuran) Hampir (Sama Dengan) Die." },
  { "en": "Apa Itu Kemasan IC (Integrated Circuit) 3D?", "id": "Menumpuk (Beberapa Die) (Secara Vertikal) (Dalam Satu Kemasan)." },
  { "en": "Apa Itu TSV (Through-Silicon Via)?", "id": "Koneksi (Vertikal) (Menembus) Wafer (Silikon) (Untuk Tumpukan 3D)." },
  { "en": "Apa Itu Sistem-Dalam-Kemasan (System-in-Package/SiP)?", "id": "Menggabungkan (Beberapa Die) (Berbeda Fungsi) (Satu Kemasan)." },
  { "en": "Apa Itu Sistem-Pada-Chip (System-on-Chip/SoC)?", "id": "Integrasi (Semua Fungsi) Sistem (CPU, GPU, RAM) (Satu Die)." },
  { "en": "Apa Itu MEMS (Micro-Electro-Mechanical System)?", "id": "Perangkat (Mekanis) (Sangat Kecil) (Dibuat) Di (Chip Silikon)." },
  { "en": "Apa Itu NEMS (Nano-Electro-Mechanical System)?", "id": "MEMS (Dalam Skala) Nanometer." },
  { "en": "Apa Itu Giroskop (Gyroscope) MEMS?", "id": "Sensor (Getar) MEMS (Mengukur) Kecepatan (Sudut Rotasi)." },
  { "en": "Apa Itu Akselerometer (Accelerometer) MEMS?", "id": "Sensor (MEMS) (Mengukur) Percepatan (Linear) (Getaran)." },
  { "en": "Apa Itu Mikrofon (Microphone) MEMS?", "id": "Mikrofon (Sangat Kecil) (Berbasis) (Kapasitansi) Di (Chip Silikon)." },
  { "en": "Apa Itu Spintronik (Spintronics)?", "id": "Elektronik (Berbasis) (Sifat Spin) Elektron (Bukan Muatan)." },
  { "en": "Apa Itu MRAM (Magnetoresistive Random-Access Memory)?", "id": "Memori (Non-Volatil) (Menyimpan Data) (Orientasi Magnetik)." },
  { "en": "Apa Itu GMR (Giant Magnetoresistance)?", "id": "Efek (Perubahan Resistansi) (Besar) (Dalam Medan Magnet)." },
  { "en": "Apa Itu TMR (Tunnel Magnetoresistance)?", "id": "Efek (GMR) (Menggunakan) (Lapisan Terowongan) Isolator (Tipis)." },
  { "en": "Apa Itu Fotonik (Photonics)?", "id": "Ilmu (Teknologi) (Membangkitkan, Mengontrol, Mendeteksi) Foton (Cahaya)." },
  { "en": "Apa Itu Fotonik Silikon (Silicon Photonics)?", "id": "Integrasi (Komponen Optik) (Langsung) Di (Atas Chip) Silikon." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide) Optik?", "id": "Struktur (Pemandu Cahaya) (Di Dalam) Chip (Mirip Serat Optik)." },
  { "en": "Apa Itu Modulator (Modulator) Optik?", "id": "Perangkat (Mengubah) Sinyal (Listrik) Menjadi (Sinyal Optik) (Data)." },
  { "en": "Apa Itu Detektor Foto (Photodetector)?", "id": "Perangkat (Mengubah) Sinyal (Optik) Menjadi (Sinyal Listrik)." },
  { "en": "Apa Itu Transceiver (Transceiver) Optik?", "id": "Modul (Gabungan) (Pemancar Optik) Dan (Penerima Optik)." },
  { "en": "Apa Itu Laser (Laser)?", "id": "Sumber (Cahaya) (Koheren) Dan (Monokromatik)." },
  { "en": "Apa Itu VCSEL (Vertical-Cavity Surface-Emitting Laser)?", "id": "Laser (Semikonduktor) (Memancar) (Vertikal) Dari (Permukaan Chip)." },
  { "en": "Apa Itu Sirkuit Terpadu Fotonik (PIC)?", "id": "Sirkuit (Terpadu) (Menggunakan) Foton (Bukan Elektron) (Di Chip)." },
  { "en": "Apa Itu Plasmonik (Plasmonics)?", "id": "Studi (Interaksi) (Cahaya) Dengan (Elektron) Di (Permukaan Logam)." },
  { "en": "Apa Itu Metamaterial (Metamaterial)?", "id": "Bahan (Buatan) (Struktur) (Sifat Unik) (Tidak Ada Di Alam)." },
  { "en": "Apa Itu Indeks Bias (Refractive Index) Negatif?", "id": "Sifat (Metamaterial) (Membelokkan) Cahaya (Arah Berlawanan)." },
  { "en": "Apa Itu Komputasi Kuantum (Quantum Computing)?", "id": "Komputasi (Menggunakan) (Fenomena) Mekanika (Kuantum)." },
  { "en": "Apa Itu Qubit (Quantum Bit)?", "id": "Unit (Dasar) (Informasi Kuantum) (0, 1, Keduanya)." },
  { "en": "Apa Itu Superposisi (Superposition)?", "id": "Kemampuan (Qubit) (Berada) (Dalam Keadaan) 0 Dan 1 (Bersamaan)." },
  { "en": "Apa Itu Keterkaitan (Entanglement)?", "id": "Koneksi (Kuantum) (Dua Qubit) (Saling Terkait) (Meski Jauh)." },
  { "en": "Apa Itu Dekoherensi (Decoherence)?", "id": "Hilangnya (Sifat Kuantum) (Qubit) (Akibat) (Gangguan Lingkungan)." },
  { "en": "Apa Itu Sambungan Josephson (Josephson Junction)?", "id": "Sambungan (Dua Superkonduktor) (Dipisah) (Isolator Tipis)." },
  { "en": "Apa Itu Qubit (Qubit) Superkonduktor?", "id": "Qubit (Dibuat) (Menggunakan) (Sirkuit) Sambungan (Josephson)." },
  { "en": "Apa Itu Kriptografi Kuantum (Quantum Cryptography)?", "id": "Keamanan (Komunikasi) (Menggunakan) (Prinsip) Fisika (Kuantum)." },
  { "en": "Apa Itu QKD (Quantum Key Distribution)?", "id": "Metode (Berbagi) Kunci (Enkripsi) (Aman) (Berbasis Kuantum)." },
  { "en": "Apa Itu Kriptografi Pasca-Kuantum (PQC)?", "id": "Algoritma (Klasik) (Diyakini) (Aman) (Dari Serangan) (Komputer Kuantum)." },
  { "en": "Apa Itu Manajemen Termal (Thermal Management)?", "id": "Mengelola (Pembuangan) Panas (Dari Komponen) Elektronik." },
  { "en": "Apa Itu Heatsink (Heatsink/Pendingin)?", "id": "Komponen (Logam) (Pasif) (Menyebarkan) Panas (Ke Udara)." },
  { "en": "Apa Itu Resistansi Termal (Thermal Resistance)?", "id": "Ukuran (Hambatan) (Aliran Panas) (Antar Material)." },
  { "en": "Apa Itu Pipa Panas (Heat Pipe)?", "id": "Perangkat (Transfer Panas) (Dua Fasa) (Sangat Efisien)." },
  { "en": "Apa Itu Ruang Uap (Vapor Chamber)?", "id": "Pipa (Panas) (Berbentuk) (Datar Tipis) (Pendingin Laptop)." },
  { "en": "Apa Itu Pendinginan Cair (Liquid Cooling)?", "id": "Menggunakan (Cairan) (Air) (Untuk Mengangkut) Panas." },
  { "en": "Apa Itu Pelat Dingin (Cold Plate)?", "id": "Platform (Logam) (Didinginkan) (Oleh Aliran) Cairan." },
  { "en": "Apa Itu Pendingin Termoelektrik (TEC)?", "id": "Pendingin (Efek Peltier) (Menggunakan Listrik) (Memindahkan Panas)." },
  { "en": "Apa Itu Efek Peltier (Peltier Effect)?", "id": "Arus (Listrik) (Menghasilkan) (Perbedaan Suhu) (Pendinginan)." },
  { "en": "Apa Itu Bahan Antarmuka Termal (TIM)?", "id": "Material (Pasta/Pad) (Mengisi Celah) (Udara) (Antar Permukaan)." },
  { "en": "Apa Itu Gemuk Termal (Thermal Grease/Paste)?", "id": "Pasta (Konduktif Panas) (TIM) (Paling Umum)." },
  { "en": "Apa Itu Pad Termal (Thermal Pad)?", "id": "Bantalan (Silikon) (Konduktif Panas) (TIM) (Pengganti Pasta)." },
  { "en": "Apa Itu Rekayasa Keandalan (Reliability Engineering)?", "id": "Disiplin (Teknik) (Memastikan) (Produk) (Berfungsi) (Sesuai Waktu)." },
  { "en": "Apa Itu MTBF (Mean Time Between Failures)?", "id": "Waktu (Rata-Rata) (Antar Kegagalan) (Produk Dapat Diperbaiki)." },
  { "en": "Apa Itu MTTF (Mean Time To Failure)?", "id": "Waktu (Rata-Rata) (Hingga Gagal) (Produk Sekali Pakai)." },
  { "en": "Apa Itu MTTR (Mean Time To Repair)?", "id": "Waktu (Rata-Rata) (Dibutuhkan) (Untuk Memperbaiki) Produk." },
  { "en": "Apa Itu Ketersediaan (Availability)?", "id": "Persentase (Waktu) (Produk) (Siap) (Beroperasi Normal)." },
  { "en": "Apa Itu Kurva Bak Mandi (Bathtub Curve)?", "id": "Grafik (Tingkat Kegagalan) (Awal, Normal, Akhir Aus)." },
  { "en": "Apa Itu Kegagalan Awal (Infant Mortality)?", "id": "Tingkat (Kegagalan) (Tinggi Di Awal) (Akibat Cacat) Produksi." },
  { "en": "Apa Itu Kegagalan Aus (Wear-Out Failure)?", "id": "Tingkat (Kegagalan) (Naik Di Akhir) (Akibat Penuaan)." },
  { "en": "Apa Itu FMEA (Failure Mode and Effects Analysis)?", "id": "Analisis (Sistematis) (Mode Kegagalan) (Dan Dampaknya) (Bottom-Up)." },
  { "en": "Apa Itu FTA (Fault Tree Analysis)?", "id": "Analisis (Pohon Logika) (Mencari Penyebab) Kegagalan (Top-Down)." },
  { "en": "Apa Itu Analisis Fourier (Fourier Analysis)?", "id": "Metode Matematika Memecah Sinyal Menjadi Komponen Frekuensi." },
  { "en": "Apa Itu Deret Fourier (Fourier Series)?", "id": "Representasi Sinyal Periodik Sebagai Jumlah Gelombang Sinus." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Mengubah Sinyal Dari Domain Waktu Ke Domain Frekuensi." },
  { "en": "Apa Itu Domain Waktu (Time Domain)?", "id": "Representasi Sinyal Sebagai Amplitudo Terhadap Waktu." },
  { "en": "Apa Itu Domain Frekuensi (Frequency Domain)?", "id": "Representasi Sinyal Sebagai Amplitudo Terhadap Frekuensi." },
  { "en": "Apa Itu Spektrum (Spectrum) Sinyal?", "id": "Komponen Frekuensi Yang Terkandung Dalam Sinyal." },
  { "en": "Apa Itu Frekuensi Fundamental (Fundamental Frequency)?", "id": "Frekuensi Terendah (Dasar) Dari Sinyal Periodik." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Frekuensi Kelipatan Bulat Dari Frekuensi Fundamental." },
  { "en": "Apa Itu FFT (Fast Fourier Transform)?", "id": "Algoritma Komputasi Cepat Untuk Menghitung Transformasi Fourier." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Algoritma DSP Memodifikasi Komponen Frekuensi Sinyal." },
  { "en": "Apa Itu Filter FIR (Finite Impulse Response)?", "id": "Filter Digital Tanpa Umpan Balik (Selalu Stabil)." },
  { "en": "Apa Itu Filter IIR (Infinite Impulse Response)?", "id": "Filter Digital Menggunakan Umpan Balik (Lebih Efisien)." },
  { "en": "Apa Itu Respon Impuls (Impulse Response)?", "id": "Output Filter Ketika Diberi Input Impuls Sempurna." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematika Menerapkan Respon Impuls Ke Sinyal." },
  { "en": "Apa Itu Fasa Linear (Linear Phase) Filter?", "id": "Filter Menunda Semua Frekuensi Dengan Jumlah Waktu Sama." },
  { "en": "Mengapa Fasa Linear (Linear Phase) Penting?", "id": "Menjaga Bentuk Gelombang Sinyal (Tidak Ada Distorsi Fasa)." },
  { "en": "Apa Itu Filter Butterworth (Butterworth Filter)?", "id": "Filter Respon Passband Datar Maksimal (Maximally Flat)." },
  { "en": "Apa Itu Filter Chebyshev (Chebyshev Filter)?", "id": "Filter Respon Roll-Off Tajam (Tepi Curam) Dengan Riak." },
  { "en": "Apa Itu Filter Bessel (Bessel Filter)?", "id": "Filter Respon Fasa Linear Terbaik (Tundaan Grup Konstan)." },
  { "en": "Apa Itu Filter Elliptic (Elliptic/Cauer Filter)?", "id": "Filter Roll-Off Paling Tajam (Riak Di Passband Stopband)." },
  { "en": "Apa Itu Sampling (Sampling)?", "id": "Proses Mengubah Sinyal Analog Kontinu Menjadi Diskrit." },
  { "en": "Apa Itu Laju Sampling (Sampling Rate)?", "id": "Jumlah Sampel Yang Diambil Per Detik (Hertz)." },
  { "en": "Apa Itu Teorema Sampling Nyquist?", "id": "Laju Sampling Harus Minimal Dua Kali Frekuensi Sinyal." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Distorsi Sinyal Akibat Laju Sampling Terlalu Rendah." },
  { "en": "Apa Itu Filter Anti-Aliasing (Anti-Aliasing Filter)?", "id": "Filter Low-Pass Analog Sebelum ADC Mencegah Aliasing." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Proses Memetakan Nilai Sampel Analog Ke Nilai Digital." },
  { "en": "Apa Itu Derau Kuantisasi (Quantization Noise)?", "id": "Kesalahan Pembulatan (Error) Akibat Proses Kuantisasi." },
  { "en": "Apa Itu Resolusi (Resolution) Kuantisasi?", "id": "Jumlah Bit Yang Digunakan (Menentukan Level Diskrit)." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Sirkuit Mengubah Sinyal Analog Menjadi Data Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Sirkuit Mengubah Data Digital Menjadi Sinyal Analog." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) Tipe Flash?", "id": "ADC Paralel Sangat Cepat Menggunakan Banyak Komparator." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) Tipe SAR (Successive Approximation)?", "id": "ADC Umum Keseimbangan Baik Kecepatan Dan Resolusi." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) Tipe Sigma-Delta?", "id": "ADC Resolusi Sangat Tinggi Menggunakan Oversampling." },
  { "en": "Apa Itu Oversampling (Oversampling)?", "id": "Mengambil Sampel Jauh Lebih Cepat Dari Laju Nyquist." },
  { "en": "Apa Itu Pembentukan Derau (Noise Shaping)?", "id": "Teknik ADC Sigma-Delta Memindahkan Noise Ke Frekuensi Tinggi." },
  { "en": "Apa Itu DNL (Differential Non-Linearity)?", "id": "Ukuran Kesalahan Lebar Langkah Antar Kode ADC/DAC." },
  { "en": "Apa Itu INL (Integral Non-Linearity)?", "id": "Ukuran Penyimpangan Total Fungsi Transfer Dari Garis Lurus." },
  { "en": "Apa Itu Rangkaian Sampel Dan Tahan (Sample-and-Hold)?", "id": "Rangkaian Menjaga Tegangan Input Konstan Selama Konversi ADC." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) R-2R Ladder?", "id": "DAC Menggunakan Jaringan Resistor Presisi R Dan 2R." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) String?", "id": "DAC Menggunakan Rangkaian Pembagi Tegangan Resistor Seri." },
  { "en": "Apa Itu Prosesor DSP (Digital Signal Processor)?", "id": "Mikroprosesor Khusus Dioptimalkan Operasi Matematika DSP." },
  { "en": "Apa Itu Arsitektur MAC (Multiply-Accumulate)?", "id": "Instruksi Hardware (Perkalian Penjumlahan) Cepat DSP." },
  { "en": "Apa Itu Aritmetika Titik Tetap (Fixed-Point)?", "id": "Representasi Angka Pecahan Menggunakan Integer Cepat." },
  { "en": "Apa Itu Aritmetika Titik Mengambang (Floating-Point)?", "id": "Representasi Angka Pecahan Menggunakan Mantissa Eksponen." },
  { "en": "Apa Itu Antrian Melingkar (Circular Buffer)?", "id": "Struktur Data Buffer (Streaming) Menulis Ulang Dari Awal." },
  { "en": "Apa Itu Filter Adaptif (Adaptive Filter)?", "id": "Filter Koefisiennya Dapat Berubah Menyesuaikan Sinyal." },
  { "en": "Apa Itu Peredam Gema (Echo Cancellation)?", "id": "Aplikasi Filter Adaptif Menghilangkan Gema Suara." },
  { "en": "Apa Itu Peredam Derau Aktif (Active Noise Control/ANC)?", "id": "Sistem Menghasilkan Sinyal Anti-Fasa Meredam Noise." },
  { "en": "Apa Itu Transformasi Z (Z-Transform)?", "id": "Alat Matematika Analisis Sistem Diskrit Analog Laplace." },
  { "en": "Apa Itu Kutub (Pole) Sistem Diskrit?", "id": "Akar Penyebut Fungsi Alih Domain-Z." },
  { "en": "Apa Itu Nol (Zero) Sistem Diskrit?", "id": "Akar Pembilang Fungsi Alih Domain-Z." },
  { "en": "Di Mana Kutub (Pole) Stabil Sistem Diskrit?", "id": "Berada Di Dalam Lingkaran Satuan (Unit Circle) Bidang-Z." },
  { "en": "Apa Itu Transformasi Bilinear (Bilinear Transform)?", "id": "Metode Mendesain Filter Digital Dari Filter Analog." },
  { "en": "Apa Itu Warping Frekuensi (Frequency Warping)?", "id": "Distorsi Frekuensi Akibat Transformasi Bilinear." },
  { "en": "Apa Itu Spektrogram (Spectrogram)?", "id": "Visualisasi Grafik Spektrum Frekuensi Sinyal Terhadap Waktu." },
  { "en": "Apa Itu Transformasi Wavelet (Wavelet Transform)?", "id": "Analisis Sinyal (Frekuensi Dan Waktu) Sinyal Non-Stasioner." },
  { "en": "Apa Beda Fourier Dan Wavelet?", "id": "Fourier (Frekuensi Saja), Wavelet (Waktu Dan Frekuensi Lokal)." },
  { "en": "Apa Itu Pemrosesan Citra (Image Processing)?", "id": "Pemrosesan Sinyal Digital (DSP) Pada Data Gambar 2D." },
  { "en": "Apa Itu Piksel (Pixel)?", "id": "Elemen Titik Terkecil Dalam Gambar Digital." },
  { "en": "Apa Itu Histogram (Histogram) Gambar?", "id": "Grafik Distribusi Intensitas Kecerahan Piksel." },
  { "en": "Apa Itu Ekualisasi Histogram (Histogram Equalization)?", "id": "Metode Otomatis Meningkatkan Kontras Gambar." },
  { "en": "Apa Itu Filter Spasial (Spatial Filter)?", "id": "Filter Bekerja Langsung Pada Nilai Piksel Tetangga." },
  { "en": "Apa Itu Konvolusi (Convolution) Gambar?", "id": "Operasi Menerapkan Kernel Filter Spasial Ke Gambar." },
  { "en": "Apa Itu Kernel (Kernel) Konvolusi?", "id": "Matriks Bobot Kecil Mendefinisikan Operasi Filter Spasial." },
  { "en": "Apa Itu Filter Rata-Rata (Mean/Average Filter)?", "id": "Filter Spasial Menghaluskan Gambar Mengaburkan Noise." },
  { "en": "Apa Itu Filter Gaussian (Gaussian Blur)?", "id": "Filter Spasial Menghaluskan Gambar (Bobot Kernel Gaussian)." },
  { "en": "Apa Itu Filter Median (Median Filter)?", "id": "Filter Spasial Efektif Menghilangkan Noise Garam Merica." },
  { "en": "Apa Itu Filter Penajaman (Sharpening Filter)?", "id": "Filter Spasial Menekankan Perbedaan Tepi (Detail)." },
  { "en": "Apa Itu Deteksi Tepi (Edge Detection)?", "id": "Algoritma Mencari Batas Objek (Perubahan Intensitas Tajam)." },
  { "en": "Apa Itu Operator Sobel (Sobel Operator)?", "id": "Kernel Konvolusi Umum Untuk Deteksi Tepi (Gradien)." },
  { "en": "Apa Itu Operator Canny (Canny Edge Detector)?", "id": "Algoritma Deteksi Tepi Multi-Tahap (Akurasi Tinggi)." },
  { "en": "Apa Itu Transformasi Hough (Hough Transform)?", "id": "Algoritma Mendeteksi Bentuk Geometris (Garis, Lingkaran)." },
  { "en": "Apa Itu Segmentasi (Segmentation) Gambar?", "id": "Proses Mempartisi Gambar Menjadi Wilayah Bermakna." },
  { "en": "Apa Itu Thresholding (Ambang Batas) Gambar?", "id": "Segmentasi Sederhana Memisahkan Piksel (Cerah/Gelap)." },
  { "en": "Apa Itu Ruang Warna (Color Space)?", "id": "Model Matematika Merepresentasikan Warna (RGB, HSV, CMYK)." },
  { "en": "Apa Itu Ruang Warna RGB (Red Green Blue)?", "id": "Model Warna Aditif Merah Hijau Biru (Monitor, Kamera)." },
  { "en": "Apa Itu Ruang Warna CMYK (Cyan Magenta Yellow Black)?", "id": "Model Warna Subtraktif Cyan Magenta Kuning Hitam (Printer)." },
  { "en": "Apa Itu Ruang Warna HSV (Hue Saturation Value)?", "id": "Model Warna Intuitif Corak Kejenuhan Kecerahan." },
  { "en": "Apa Itu Ruang Warna Grayscale (Skala Abu-Abu)?", "id": "Gambar Hanya Memiliki Informasi Intensitas Kecerahan." },
  { "en": "Apa Itu Operasi Morfologi (Morphological Operations)?", "id": "Operasi (Gambar Biner) Mengubah Bentuk (Dilasi, Erosi)." },
  { "en": "Apa Itu Erosi (Erosion) Morfologi?", "id": "Operasi Mengikis Mengecilkan Batas Objek Putih." },
  { "en": "Apa Itu Dilasi (Dilation) Morfologi?", "id": "Operasi Memperluas Menebalkan Batas Objek Putih." },
  { "en": "Apa Itu Pembukaan (Opening) Morfologi?", "id": "Operasi Erosi Diikuti Dilasi (Menghilangkan Noise Titik Putih)." },
  { "en": "Apa Itu Penutupan (Closing) Morfologi?", "id": "Operasi Dilasi Diikuti Erosi (Menutup Lubang Hitam Kecil)." },
  { "en": "Apa Itu Pengenalan Karakter Optik (OCR)?", "id": "Proses Komputer Mengenali Teks Karakter Dalam Gambar." },
  { "en": "Apa Kepanjangan OCR (Optical Character Recognition)?", "id": "Pengenalan Karakter Optik." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Bidang Ilmu Membuat Komputer Memahami Data Visual." },
  { "en": "Apa Itu Pembelajaran Mendalam (Deep Learning)?", "id": "Cabang AI Menggunakan Jaringan Saraf Tiruan Multi-Lapisan." },
  { "en": "Apa Itu Jaringan Saraf Konvolusional (CNN)?", "id": "Arsitektur Deep Learning Unggul Untuk Visi Komputer." },
  { "en": "Apa Kepanjangan CNN (Convolutional Neural Network)?", "id": "Jaringan Saraf Konvolusional." },
  { "en": "Apa Itu Lapisan Konvolusi (Convolutional Layer)?", "id": "Lapisan CNN Melakukan Operasi Konvolusi Ekstraksi Fitur." },
  { "en": "Apa Itu Lapisan Pooling (Pooling Layer)?", "id": "Lapisan CNN Melakukan Downsampling Mengurangi Dimensi." },
  { "en": "Apa Itu Fungsi Aktivasi (Activation Function)?", "id": "Fungsi Non-Linear (Contoh: ReLU) Dalam Jaringan Saraf." },
  { "en": "Apa Itu ReLU (Rectified Linear Unit)?", "id": "Fungsi Aktivasi Populer (Output X Jika X Positif, Lainnya 0)." },
  { "en": "Apa Itu Backpropagation (Propagasi Balik)?", "id": "Algoritma Melatih Jaringan Saraf (Menghitung Gradien Error)." },
  { "en": "Apa Itu Penurunan Gradien (Gradient Descent)?", "id": "Algoritma Optimasi (Mencari Bobot) Minimal (Fungsi Kerugian)." },
  { "en": "Apa Itu Overfitting (Overfitting)?", "id": "Model Terlalu Hafal Data Latih, Performa Buruk Data Baru." },
  { "en": "Apa Itu Regularisasi (Regularization)?", "id": "Teknik Mencegah Overfitting (Menambah Penalti) Pada Bobot." },
  { "en": "Apa Itu Dropout (Dropout)?", "id": "Teknik Regularisasi (Mematikan Neuron) Acak (Selama Pelatihan)." },
  { "en": "Apa Itu Augmentasi Data (Data Augmentation)?", "id": "Memperbanyak (Data Latih) (Memutar, Memotong) Gambar (Cegah Overfitting)." },
  { "en": "Apa Itu Pembelajaran Transfer (Transfer Learning)?", "id": "Menggunakan (Model AI) Yang (Sudah Dilatih) (Tugas Serupa)." },
  { "en": "Apa Itu Inferensi (Inference) AI?", "id": "Proses (Menggunakan) Model AI (Yang Sudah Dilatih) (Prediksi)." },
  { "en": "Apa Itu Jaringan Saraf Berulang (RNN)?", "id": "Arsitektur (Deep Learning) (Memiliki Memori) (Data Sekuensial)." },
  { "en": "Apa Kepanjangan RNN (Recurrent Neural Network)?", "id": "Jaringan Saraf Berulang." },
  { "en": "Apa Itu LSTM (Long Short-Term Memory)?", "id": "Tipe (RNN) Canggih (Mengatasi) Masalah (Gradien Hilang)." },
  { "en": "Apa Itu Model Transformer (Transformer Model)?", "id": "Arsitektur (Deep Learning) (Berbasis Perhatian) (Sangat Populer)." },
  { "en": "Apa Itu Model Bahasa Besar (LLM)?", "id": "Model (Transformer) (Sangat Besar) (Dilatih) (Data Teks Masif)." },
  { "en": "Apa Itu Pemrosesan Bahasa Alami (NLP)?", "id": "Cabang AI Memahami Dan Memproses Bahasa Manusia." },
  { "en": "Apa Kepanjangan NLP (Natural Language Processing)?", "id": "Pemrosesan Bahasa Alami." },
  { "en": "Apa Itu Tokenisasi (Tokenization) NLP?", "id": "Memecah Teks Menjadi Unit Kecil Kata Atau Kalimat." },
  { "en": "Apa Itu Stemming (Stemming) NLP?", "id": "Mengubah Kata Menjadi Bentuk Dasar Memotong Awalan Akhiran." },
  { "en": "Apa Itu Lematisasi (Lemmatization) NLP?", "id": "Mengubah Kata Menjadi Bentuk Dasar Kamus Lemma." },
  { "en": "Apa Itu Stop Words (Stop Words) NLP?", "id": "Kata-Kata Umum Yang Sering Dihilangkan (Contoh: Dan, Di, Yang)." },
  { "en": "Apa Itu Bag-of-Words (BoW)?", "id": "Model Representasi Teks Menghitung Frekuensi Kata." },
  { "en": "Apa Itu TF-IDF (Term Frequency-Inverse Document Frequency)?", "id": "Ukuran Statistik Bobot Pentingnya Kata Dalam Dokumen." },
  { "en": "Apa Itu Embedding Kata (Word Embedding)?", "id": "Representasi Kata Dalam Ruang Vektor Numerik." },
  { "en": "Apa Itu Word2Vec (Word2Vec)?", "id": "Model Populer Membuat Word Embedding (Prediksi Konteks)." },
  { "en": "Apa Itu Model Sekuensial (Sequence Model) NLP?", "id": "Model Memproses Data Berurutan (Contoh: RNN, LSTM)." },
  { "en": "Apa Itu Model Transformer (Transformer Model) NLP?", "id": "Arsitektur Model Berbasis Mekanisme Perhatian (Attention)." },
  { "en": "Apa Itu Mekanisme Perhatian (Attention Mechanism)?", "id": "Fitur Model Memfokuskan Pada Bagian Input Relevan." },
  { "en": "Apa Itu BERT (Bidirectional Encoder Representations from Transformers)?", "id": "Model Transformer (Google) Populer Untuk Pemahaman Konteks." },
  { "en": "Apa Itu GPT (Generative Pre-trained Transformer)?", "id": "Model Transformer (OpenAI) Populer Untuk Menghasilkan Teks." },
  { "en": "Apa Itu Model Bahasa Besar (LLM)?", "id": "Model AI Sangat Besar (Transformer) Dilatih Data Teks Masif." },
  { "en": "Apa Itu Fine-Tuning (Penyesuaian Halus) LLM?", "id": "Melatih Model Pra-Terlatih Dengan Data Khusus Tugas Baru." },
  { "en": "Apa Itu Prompt (Prompt) Engineering?", "id": "Seni Merancang Input Teks Optimal (Instruksi) Untuk LLM." },
  { "en": "Apa Itu Analisis Sentimen (Sentiment Analysis)?", "id": "NLP Mengklasifikasikan Opini Teks (Positif, Negatif, Netral)." },
  { "en": "Apa Itu Pengenalan Entitas Bernama (NER)?", "id": "NLP Menemukan Dan Mengkategorikan Entitas (Nama, Lokasi, Organisasi)." },
  { "en": "Apa Kepanjangan NER (Named Entity Recognition)?", "id": "Pengenalan Entitas Bernama." },
  { "en": "Apa Itu Terjemahan Mesin (Machine Translation)?", "id": "NLP Menerjemahkan Teks Otomatis Antar Bahasa." },
  { "en": "Apa Itu Peringkasan Teks (Text Summarization)?", "id": "NLP Membuat Ringkasan Pendek Dari Dokumen Panjang." },
  { "en": "Apa Itu Respon Pertanyaan (Question Answering)?", "id": "NLP Sistem Menjawab Pertanyaan Berdasarkan Konteks Teks." },
  { "en": "Apa Itu Pengenalan Suara (Speech Recognition/ASR)?", "id": "Mengubah Bahasa Lisan (Suara) Menjadi Teks Tertulis." },
  { "en": "Apa Itu Sintesis Suara (Text-to-Speech/TTS)?", "id": "Mengubah Teks Tertulis Menjadi Bahasa Lisan (Suara Buatan)." },
  { "en": "Apa Itu Diagram Blok (Block Diagram)?", "id": "Representasi Grafis Sistem Menggunakan Blok Fungsional Panah." },
  { "en": "Apa Itu Diagram Alir (Flowchart)?", "id": "Representasi Grafis Langkah-Langkah Logika Algoritma (Bentuk Geometris)." },
  { "en": "Apa Itu Pseudocode (Kode Semu)?", "id": "Deskripsi Teks Informal Algoritma (Mirip Kode, Bukan Bahasa Baku)." },
  { "en": "Apa Itu Mesin Keadaan (State Machine)?", "id": "Model Komputasi (Perilaku) Berdasarkan Keadaan (State) Transisi." },
  { "en": "Apa Itu Diagram Keadaan (State Diagram)?", "id": "Grafik Visualisasi Mesin Keadaan (Lingkaran Dan Panah)." },
  { "en": "Apa Itu Keadaan (State)?", "id": "Kondisi Operasi Spesifik Dalam Mesin Keadaan." },
  { "en": "Apa Itu Transisi (Transition)?", "id": "Perpindahan Dari Satu Keadaan Ke Keadaan Lain (Dipicu Peristiwa)." },
  { "en": "Apa Itu Peristiwa (Event)?", "id": "Pemicu Yang Menyebabkan Transisi Antar Keadaan (Input)." },
  { "en": "Apa Itu Tindakan (Action)?", "id": "Aktivitas Yang Dilakukan (Saat Transisi) Atau (Saat Dalam Keadaan)." },
  { "en": "Apa Itu Mesin Keadaan Mealy (Mealy Machine)?", "id": "Mesin Keadaan (Output) Bergantung (Keadaan Saat Ini) Dan (Input)." },
  { "en": "Apa Itu Mesin Keadaan Moore (Moore Machine)?", "id": "Mesin Keadaan (Output) Bergantung (Hanya) Pada (Keadaan Saat Ini)." },
  { "en": "Apa Itu Petri Net (Petri Net)?", "id": "Model (Grafis Matematis) (Sistem Terdistribusi) (Tempat, Transisi, Token)." },
  { "en": "Apa Itu UML (Unified Modeling Language)?", "id": "Bahasa (Pemodelan Visual) Standar (Desain Perangkat Lunak)." },
  { "en": "Apa Itu Diagram Kasus Penggunaan (Use Case Diagram)?", "id": "Diagram (UML) (Interaksi) Aktor (Dengan Sistem)." },
  { "en": "Apa Itu Diagram Kelas (Class Diagram)?", "id": "Diagram (UML) (Struktur Statis) (Kelas, Atribut, Metode) OOP." },
  { "en": "Apa Itu Diagram Urutan (Sequence Diagram)?", "id": "Diagram (UML) (Interaksi) Objek (Urutan Waktu)." },
  { "en": "Apa Itu Diagram Aktivitas (Activity Diagram)?", "id": "Diagram (UML) (Aliran Kerja) (Mirip Flowchart) (Aktivitas, Keputusan)." },
  { "en": "Apa Itu Diagram Komponen (Component Diagram)?", "id": "Diagram (UML) (Organisasi) (Ketergantungan) Komponen (Fisik)." },
  { "en": "Apa Itu Diagram Penerapan (Deployment Diagram)?", "id": "Diagram (UML) (Arsitektur Fisik) (Perangkat Keras, Perangkat Lunak)." },
  { "en": "Apa Itu Manajemen Rantai Pasokan (SCM)?", "id": "Manajemen (Aliran) Barang (Jasa, Informasi) (Bahan Baku Ke Konsumen)." },
  { "en": "Apa Kepanjangan SCM (Supply Chain Management)?", "id": "Manajemen Rantai Pasokan." },
  { "en": "Apa Itu Logistik (Logistics)?", "id": "Proses (Merencanakan, Melaksanakan) Penyimpanan (Aliran) Barang." },
  { "en": "Apa Itu Inventaris (Inventory)?", "id": "Stok (Bahan Baku, Barang Setengah Jadi, Barang Jadi)." },
  { "en": "Apa Itu Pergudangan (Warehousing)?", "id": "Fungsi (Penyimpanan) (Penerimaan, Penyimpanan, Pengambilan) Barang." },
  { "en": "Apa Itu Just-In-Time (JIT)?", "id": "Strategi (Manufaktur) (Mengurangi Inventaris) (Barang Datang Tepat Waktu)." },
  { "en": "Apa Itu Waktu Tunggu (Lead Time)?", "id": "Total (Waktu) (Dari Pemesanan) Hingga (Barang Diterima)." },
  { "en": "Apa Itu Titik Pemesanan Ulang (Reorder Point/ROP)?", "id": "Level (Inventaris) (Memicu) Pemesanan (Barang) Baru." },
  { "en": "Apa Itu Stok Pengaman (Safety Stock)?", "id": "Inventaris (Ekstra) (Disimpan) (Mencegah Kehabisan Stok)." },
  { "en": "Apa Itu Efek Cambuk Banteng (Bullwhip Effect)?", "id": "Fluktuasi (Permintaan) (Semakin Membesar) Di (Hulu) Rantai Pasok." },
  { "en": "Apa Itu Logistik Pihak Ketiga (3PL)?", "id": "Menggunakan (Perusahaan Luar) (Mengelola) (Fungsi Logistik)." },
  { "en": "Apa Itu RFID (Radio-Frequency Identification)?", "id": "Teknologi (Pelacakan) (Menggunakan Tag) Radio (Menggantikan Barcode)." },
  { "en": "Apa Itu WMS (Warehouse Management System)?", "id": "Perangkat Lunak (Mengelola) (Operasi) (Penerimaan, Penyimpanan) Gudang." },
  { "en": "Apa Itu TMS (Transportation Management System)?", "id": "Perangkat Lunak (Mengelola) (Perencanaan, Eksekusi) Transportasi (Pengiriman)." },
  { "en": "Apa Itu Freight Forwarder (Freight Forwarder)?", "id": "Perusahaan (Jasa) (Mengatur) (Pengiriman Barang) (Internasional)." },
  { "en": "Apa Itu Incoterms (Incoterms)?", "id": "Istilah (Perdagangan Internasional) (Menentukan Tanggung Jawab) Penjual Pembeli." },
  { "en": "Apa Itu FOB (Free On Board)?", "id": "Incoterm (Penjual) (Tanggung Jawab) (Sampai Barang) Di Atas Kapal." },
  { "en": "Apa Itu CIF (Cost, Insurance, and Freight)?", "id": "Incoterm (Penjual) (Menanggung Biaya) (Asuransi, Kargo) (Sampai Pelabuhan Tujuan)." },
  { "en": "Apa Itu Letter of Credit (L/C)?", "id": "Jaminan (Bank) (Pembayaran) (Kepada Penjual) (Jika Dokumen Sesuai)." },
  { "en": "Apa Itu Bill of Lading (B/L)?", "id": "Dokumen (Resmi) (Bukti Kepemilikan) (Barang) (Dalam Pengiriman Laut)." },
  { "en": "Apa Itu Air Waybill (AWB)?", "id": "Dokumen (Resmi) (Bukti Kontrak) (Pengiriman Udara)." },
  { "en": "Apa Itu Bea Cukai (Customs)?", "id": "Instansi (Pemerintah) (Mengawasi) (Barang Masuk/Keluar) (Negara)." },
  { "en": "Apa Itu Bea Masuk (Import Duty)?", "id": "Pajak (Dikenakan) (Pada Barang) Impor." },
  { "en": "Apa Itu Bea Keluar (Export Duty)?", "id": "Pajak (Dikenakan) (Pada Barang) Ekspor." },
  { "en": "Apa Itu Zona Perdagangan Bebas (Free Trade Zone)?", "id": "Area (Khusus) (Barang) (Dapat Masuk) (Tanpa Bea Masuk)." },
  { "en": "Apa Itu Kontainer (Container) Pengiriman?", "id": "Kotak (Logam) (Standar) (Mengangkut Barang) (Kapal, Truk, Kereta)." },
  { "en": "Apa Itu TEU (Twenty-foot Equivalent Unit)?", "id": "Satuan (Ukuran) (Kapasitas Kargo) (Setara Kontainer 20 Kaki)." },
  { "en": "Apa Itu Kapal Kargo (Cargo Ship)?", "id": "Kapal (Laut) (Dirancang) (Mengangkut Barang) Kargo." },
  { "en": "Apa Itu Kapal Kontainer (Container Ship)?", "id": "Kapal (Kargo) (Khusus) (Mengangkut Kontainer) Standar." },
  { "en": "Apa Itu Kapal Tanker (Tanker)?", "id": "Kapal (Kargo) (Khusus) (Mengangkut Cairan) (Minyak, Gas, Kimia)." },
  { "en": "Apa Itu Kapal Curah (Bulk Carrier)?", "id": "Kapal (Kargo) (Mengangkut) (Muatan Curah) (Batu Bara, Biji-Bijian)." },
  { "en": "Apa Itu Logistik Rantai Dingin (Cold Chain)?", "id": "Logistik (Barang) (Sensitif Suhu) (Obat, Makanan Beku)." },
  { "en": "Apa Itu Reefer (Refrigerated) Container?", "id": "Kontainer (Dilengkapi) (Mesin Pendingin) (Untuk Rantai Dingin)." },
  { "en": "Apa Itu Cross-Docking (Cross-Docking)?", "id": "Proses (Logistik) (Bongkar Muat) (Langsung) (Tanpa Penyimpanan Gudang)." },
  { "en": "Apa Itu Last Mile Delivery (Pengiriman Mil Terakhir)?", "id": "Tahap (Akhir) (Proses Pengiriman) (Dari Hub) (Ke Pelanggan)." },
  { "en": "Apa Itu Ekonomi (Economics)?", "id": "Ilmu (Sosial) (Studi) (Produksi, Distribusi, Konsumsi) Barang Jasa." },
  { "en": "Apa Itu Ekonomi Mikro (Microeconomics)?", "id": "Ekonomi (Studi) (Perilaku) (Individu, Rumah Tangga, Perusahaan)." },
  { "en": "Apa Itu Ekonomi Makro (Macroeconomics)?", "id": "Ekonomi (Studi) (Perekonomian) (Agregat, Keseluruhan) (Inflasi, PDB)." },
  { "en": "Apa Itu Permintaan (Demand)?", "id": "Jumlah (Barang/Jasa) (Yang Diinginkan) (Konsumen) (Pada Berbagai Harga)." },
  { "en": "Apa Itu Penawaran (Supply)?", "id": "Jumlah (Barang/Jasa) (Yang Ditawarkan) (Produsen) (Pada Berbagai Harga)." },
  { "en": "Apa Itu Hukum Permintaan (Law of Demand)?", "id": "Harga (Naik) (Permintaan Turun), (Harga Turun) (Permintaan Naik)." },
  { "en": "Apa Itu Hukum Penawaran (Law of Supply)?", "id": "Harga (Naik) (Penawaran Naik), (Harga Turun) (Penawaran Turun)." },
  { "en": "Apa Itu Harga Ekuilibrium (Equilibrium Price)?", "id": "Harga (Dimana) (Jumlah Permintaan) (Sama Dengan) (Jumlah Penawaran)." },
  { "en": "Apa Itu Elastisitas (Elasticity)?", "id": "Ukuran (Sensitivitas) (Permintaan/Penawaran) (Terhadap Perubahan) Harga." },
  { "en": "Apa Itu Permintaan Elastis (Elastic Demand)?", "id": "Perubahan (Harga) (Menyebabkan Perubahan Besar) Permintaan (Barang Mewah)." },
  { "en": "Apa Itu Permintaan Inelastis (Inelastic Demand)?", "id": "Perubahan (Harga) (Hanya Sedikit) (Mempengaruhi Permintaan) (Kebutuhan Pokok)." },
  { "en": "Apa Itu PDB (Produk Domestik Bruto)?", "id": "Total (Nilai Pasar) (Barang Jasa) (Final) (Diproduksi) (Dalam Negara)." },
  { "en": "Apa Kepanjangan PDB (Produk Domestik Bruto)?", "id": "Produk Domestik Bruto (Gross Domestic Product/GDP)." },
  { "en": "Apa Itu Inflasi (Inflation)?", "id": "Kenaikan (Harga) (Barang Jasa) (Secara Umum) (Terus Menerus)." },
  { "en": "Apa Itu Deflasi (Deflation)?", "id": "Penurunan (Harga) (Barang Jasa) (Secara Umum) (Terus Menerus)." },
  { "en": "Apa Itu Indeks Harga Konsumen (IHK)?", "id": "Ukuran (Rata-Rata) (Perubahan Harga) (Keranjang Belanja) Konsumen." },
  { "en": "Apa Itu Bank Sentral (Central Bank)?", "id": "Institusi (Mengelola) (Mata Uang) (Kebijakan Moneter) (Negara)." },
  { "en": "Apa Itu Kebijakan Moneter (Monetary Policy)?", "id": "Kebijakan (Bank Sentral) (Mengontrol) (Pasokan Uang) (Suku Bunga)." },
  { "en": "Apa Itu Kebijakan Fiskal (Fiscal Policy)?", "id": "Kebijakan (Pemerintah) (Menggunakan) (Anggaran, Pajak) (Ekonomi)." },
  { "en": "Apa Itu Suku Bunga (Interest Rate)?", "id": "Biaya (Meminjam) Uang (Atau Imbalan) (Menabung)." },
  { "en": "Apa Itu Resesi (Recession)?", "id": "Penurunan (Aktivitas Ekonomi) (Signifikan) (Selama Beberapa Bulan)." },
  { "en": "Apa Itu Pengangguran (Unemployment)?", "id": "Kondisi (Orang) (Aktif Mencari Kerja) (Tapi Tidak Mendapat) Pekerjaan." },
  { "en": "Apa Itu Oligopoli (Oligopoly)?", "id": "Struktur Pasar Dikuasai Oleh Sejumlah Kecil Perusahaan Besar." },
  { "en": "Apa Itu Monopoli (Monopoly)?", "id": "Struktur Pasar Dikuasai Oleh Satu Penjual Tunggal." },
  { "en": "Apa Itu Persaingan Sempurna (Perfect Competition)?", "id": "Struktur Pasar Banyak Penjual Pembeli Produk Identik." },
  { "en": "Apa Itu Persaingan Monopolistik (Monopolistic Competition)?", "id": "Pasar Banyak Penjual Produk Serupa Tapi Terdiferensiasi." },
  { "en": "Apa Itu Biaya Peluang (Opportunity Cost)?", "id": "Nilai Alternatif Terbaik Yang Dikorbankan Saat Memilih." },
  { "en": "Apa Itu Keunggulan Komparatif (Comparative Advantage)?", "id": "Kemampuan Produksi Biaya Peluang Lebih Rendah." },
  { "en": "Apa Itu Keunggulan Absolut (Absolute Advantage)?", "id": "Kemampuan Produksi Lebih Efisien (Output Lebih Banyak)." },
  { "en": "Apa Itu Proteksionisme (Protectionism)?", "id": "Kebijakan Pemerintah Melindungi Industri Domestik." },
  { "en": "Apa Itu Tarif (Tariff)?", "id": "Pajak Yang Dikenakan Pada Barang Impor." },
  { "en": "Apa Itu Kuota (Quota)?", "id": "Batasan Kuantitas Barang Yang Boleh Diimpor." },
  { "en": "Apa Itu Embargo (Embargo)?", "id": "Larangan Total Perdagangan Dengan Negara Tertentu." },
  { "en": "Apa Itu Neraca Perdagangan (Balance of Trade)?", "id": "Selisih Antara Nilai Ekspor Dan Nilai Impor." },
  { "en": "Apa Itu Surplus (Surplus) Perdagangan?", "id": "Kondisi Nilai Ekspor Lebih Besar Dari Impor." },
  { "en": "Apa Itu Defisit (Deficit) Perdagangan?", "id": "Kondisi Nilai Impor Lebih Besar Dari Ekspor." },
  { "en": "Apa Itu Kurs (Exchange Rate)?", "id": "Nilai Tukar Satu Mata Uang Terhadap Mata Uang Lain." },
  { "en": "Apa Itu Depresiasi (Depreciation) Mata Uang?", "id": "Penurunan Nilai Mata Uang Relatif Terhadap Mata Uang Lain." },
  { "en": "Apa Itu Apresiasi (Appreciation) Mata Uang?", "id": "Kenaikan Nilai Mata Uang Relatif Terhadap Mata Uang Lain." },
  { "en": "Apa Itu Pasar Saham (Stock Market)?", "id": "Pasar Tempat Perdagangan Saham (Ekuitas) Perusahaan Publik." },
  { "en": "Apa Itu Saham (Stock)?", "id": "Bukti Kepemilikan Sebagian (Ekuitas) Suatu Perusahaan." },
  { "en": "Apa Itu Obligasi (Bond)?", "id": "Surat Hutang (Penerbit Berjanji Membayar Bunga Kupon)." },
  { "en": "Apa Itu Dividen (Dividend)?", "id": "Bagian Laba Perusahaan Yang Dibagikan Kepada Pemegang Saham." },
  { "en": "Apa Itu Pasar Bullish (Bull Market)?", "id": "Kondisi Pasar Saham Cenderung Naik Optimistis." },
  { "en": "Apa Itu Pasar Bearish (Bear Market)?", "id": "Kondisi Pasar Saham Cenderung Turun Pesimistis." },
  { "en": "Apa Itu Indeks (Index) Pasar Saham?", "id": "Ukuran Statistik (Rata-Rata) Pergerakan Harga Saham (IHSG)." },
  { "en": "Apa Itu Kapitalisasi Pasar (Market Capitalization)?", "id": "Total Nilai Pasar Perusahaan (Harga Saham Kali Jumlah Saham)." },
  { "en": "Apa Itu IPO (Initial Public Offering)?", "id": "Penawaran Umum Perdana (Perusahaan Menjual Saham Ke Publik)." },
  { "en": "Apa Itu Pialang (Broker) Saham?", "id": "Perantara (Perusahaan Sekuritas) Transaksi Jual Beli Saham." },
  { "en": "Apa Itu Reksadana (Mutual Fund)?", "id": "Wadah Investasi Kolektif (Dikelola Manajer Investasi)." },
  { "en": "Apa Itu Manajer Investasi (Investment Manager)?", "id": "Profesional Mengelola Portofolio Investasi Reksadana." },
  { "en": "Apa Itu Diversifikasi (Diversification)?", "id": "Strategi Investasi Menyebar Risiko Ke Berbagai Aset." },
  { "en": "Apa Itu Aset (Asset)?", "id": "Sumber Daya Ekonomi (Dimiliki) (Tunai, Saham, Properti)." },
  { "en": "Apa Itu Liabilitas (Liability)?", "id": "Kewajiban Finansial (Hutang) Yang Dimiliki." },
  { "en": "Apa Itu Kekayaan Bersih (Net Worth)?", "id": "Total Aset Dikurangi Total Liabilitas." },
  { "en": "Apa Itu Laporan Arus Kas (Cash Flow Statement)?", "id": "Laporan Keuangan (Aliran) Uang Tunai (Masuk Keluar)." },
  { "en": "Apa Itu Laporan Laba Rugi (Income Statement)?", "id": "Laporan Keuangan (Pendapatan, Biaya, Laba) Suatu Periode." },
  { "en": "Apa Itu Neraca (Balance Sheet)?", "id": "Laporan Keuangan (Aset, Liabilitas, Ekuitas) Satu Titik Waktu." },
  { "en": "Apa Itu Akuntansi (Accounting)?", "id": "Proses (Pencatatan, Pengukuran, Pelaporan) Informasi Keuangan." },
  { "en": "Apa Itu Debit (Debit) Akuntansi?", "id": "Catatan (Sisi Kiri) Akun (Penambahan Aset, Pengurangan Liabilitas)." },
  { "en": "Apa Itu Kredit (Credit) Akuntansi?", "id": "Catatan (Sisi Kanan) Akun (Pengurangan Aset, Penambahan Liabilitas)." },
  { "en": "Apa Itu Jurnal (Journal) Akuntansi?", "id": "Buku (Catatan) Transaksi (Kronologis) (Debit Kredit)." },
  { "en": "Apa Itu Buku Besar (Ledger) Akuntansi?", "id": "Kumpulan (Akun) (Meringkas) Transaksi (Dari Jurnal)." },
  { "en": "Apa Itu Aset Lancar (Current Asset)?", "id": "Aset (Mudah) Dicairkan (Menjadi Uang Tunai) (Satu Tahun)." },
  { "en": "Apa Itu Aset Tetap (Fixed Asset)?", "id": "Aset (Jangka Panjang) (Gedung, Mesin) (Digunakan Operasi)." },
  { "en": "Apa Itu Liabilitas Lancar (Current Liability)?", "id": "Hutang (Jangka Pendek) (Harus Dibayar) (Satu Tahun)." },
  { "en": "Apa Itu Liabilitas Jangka Panjang (Long-Term Liability)?", "id": "Hutang (Jangka Panjang) (Jatuh Tempo) (Lebih Satu Tahun)." },
  { "en": "Apa Itu Ekuitas (Equity)?", "id": "Modal (Pemilik) (Aset Dikurangi Liabilitas)." },
  { "en": "Apa Itu Pendapatan (Revenue)?", "id": "Total (Uang Masuk) (Hasil) Penjualan (Barang Jasa)." },
  { "en": "Apa Itu Beban (Expense)?", "id": "Biaya (Dikeluarkan) (Untuk Menghasilkan) Pendapatan (Gaji, Sewa)." },
  { "en": "Apa Itu Laba Bersih (Net Income/Profit)?", "id": "Sisa (Pendapatan) (Setelah Dikurangi) (Semua Beban)." },
  { "en": "Apa Itu Titik Impas (Break-Even Point)?", "id": "Titik (Dimana) (Total Pendapatan) (Sama Dengan) (Total Biaya)." },
  { "en": "Apa Itu Biaya Tetap (Fixed Cost)?", "id": "Biaya (Tidak Berubah) (Meskipun) (Volume Produksi) (Berubah)." },
  { "en": "Apa Itu Biaya Variabel (Variable Cost)?", "id": "Biaya (Berubah) (Seiring) (Perubahan) (Volume Produksi)." },
  { "en": "Apa Itu Pemasaran (Marketing)?", "id": "Aktivitas (Mempromosikan) (Menjual) (Produk Jasa) (Ke Konsumen)." },
  { "en": "Apa Itu Bauran Pemasaran (Marketing Mix/4P)?", "id": "Empat (Elemen) (Product, Price, Place, Promotion)." },
  { "en": "Apa Itu Produk (Product)?", "id": "Barang (Atau Jasa) (Yang Ditawarkan) (Ke Pasar)." },
  { "en": "Apa Itu Harga (Price)?", "id": "Jumlah (Uang) (Yang Dikenakan) (Untuk Suatu) Produk." },
  { "en": "Apa Itu Tempat (Place/Distribution)?", "id": "Saluran (Distribusi) (Membuat Produk) (Tersedia) (Bagi Konsumen)." },
  { "en": "Apa Itu Promosi (Promotion)?", "id": "Aktivitas (Komunikasi) (Membujuk) (Konsumen) (Membeli Produk)." },
  { "en": "Apa Itu Segmentasi (Segmentation) Pasar?", "id": "Membagi (Pasar Heterogen) (Menjadi Kelompok) (Homogen)." },
  { "en": "Apa Itu Penargetan (Targeting) Pasar?", "id": "Memilih (Satu Atau Lebih) (Segmen Pasar) (Untuk Dilayani)." },
  { "en": "Apa Itu Pemosisian (Positioning) Pasar?", "id": "Menciptakan (Persepsi) (Jelas Dan Berbeda) (Di Benak) Konsumen." },
  { "en": "Apa Itu Merek (Brand)?", "id": "Nama (Simbol, Desain) (Mengidentifikasi) (Produk) (Dari Pesaing)." },
  { "en": "Apa Itu Citra Merek (Brand Image)?", "id": "Persepsi (Konsumen) (Terhadap) (Suatu Merek)." },
  { "en": "Apa Itu Loyalitas Merek (Brand Loyalty)?", "id": "Komitmen (Konsumen) (Membeli Kembali) (Merek Tertentu)." },
  { "en": "Apa Itu Ekuitas Merek (Brand Equity)?", "id": "Nilai (Tambah) (Yang Diberikan) (Merek) (Kepada Produk)." },
  { "en": "Apa Itu Riset Pasar (Market Research)?", "id": "Proses (Mengumpulkan) (Menganalisis) (Data) (Tentang Pasar)." },
  { "en": "Apa Itu Data Primer (Primary Data)?", "id": "Data (Dikumpulkan) (Langsung) (Untuk Riset) (Survei, Wawancara)." },
  { "en": "Apa Itu Data Sekunder (Secondary Data)?", "id": "Data (Yang Sudah Ada) (Dikumpulkan) (Pihak Lain) (Laporan, Statistik)." },
  { "en": "Apa Itu Survei (Survey)?", "id": "Metode (Riset) (Mengajukan Pertanyaan) (Kepada Responden)." },
  { "en": "Apa Itu Kelompok Fokus (Focus Group)?", "id": "Diskusi (Kelompok Kecil) (Dipandu) (Moderator) (Riset Kualitatif)." },
  { "en": "Apa Itu Pemasaran Digital (Digital Marketing)?", "id": "Pemasaran (Menggunakan) (Saluran Digital) (Internet, Media Sosial)." },
  { "en": "Apa Itu SEO (Search Engine Optimization)?", "id": "Optimasi (Situs Web) (Peringkat Tinggi) (Di Mesin Pencari)." },
  { "en": "Apa Itu SEM (Search Engine Marketing)?", "id": "Pemasaran (Mesin Pencari) (Termasuk SEO) (Dan Iklan Berbayar/PPC)." },
  { "en": "Apa Itu PPC (Pay-Per-Click)?", "id": "Model (Iklan Digital) (Pengiklan Membayar) (Setiap Kali) (Iklan Diklik)." },
  { "en": "Apa Itu Pemasaran Media Sosial (Social Media Marketing)?", "id": "Menggunakan (Platform) (Media Sosial) (Membangun Merek)." },
  { "en": "Apa Itu Pemasaran Konten (Content Marketing)?", "id": "Membuat (Mendistribusikan) (Konten Bernilai) (Menarik Audiens)." },
  { "en": "Apa Itu Pemasaran Email (Email Marketing)?", "id": "Mengirim (Pesan Komersial) (Melalui Email) (Ke Daftar Pelanggan)." },
  { "en": "Apa Itu Pemasaran Afiliasi (Affiliate Marketing)?", "id": "Mendapat (Komisi) (Mempromosikan) (Produk Orang Lain)." },
  { "en": "Apa Itu Analitik (Analytics) Web?", "id": "Mengukur (Menganalisis) (Lalu Lintas) (Situs Web) (Google Analytics)." },
  { "en": "Apa Itu Tingkat Konversi (Conversion Rate)?", "id": "Persentase (Pengunjung) (Yang Melakukan) (Tindakan Diinginkan)." },
  { "en": "Apa Itu Rasio Klik-Tayang (Click-Through Rate/CTR)?", "id": "Persentase (Tayangan Iklan) (Yang Menghasilkan) (Klik)." },
  { "en": "Apa Itu Biaya Per Akuisisi (Cost Per Acquisition/CPA)?", "id": "Biaya (Iklan) (Untuk Mendapatkan) (Satu Pelanggan) (Konversi)." },
  { "en": "Apa Itu Manajemen (Management)?", "id": "Proses (Merencanakan, Mengorganisasi, Memimpin, Mengendalikan) Sumber Daya." },
  { "en": "Apa Itu Perencanaan (Planning)?", "id": "Fungsi (Manajemen) (Menetapkan Tujuan) (Menentukan Cara Mencapai)." },
  { "en": "Apa Itu Pengorganisasian (Organizing)?", "id": "Fungsi (Manajemen) (Mengatur Sumber Daya) (Struktur Tugas)." },
  { "en": "Apa Itu Kepemimpinan (Leading)?", "id": "Fungsi (Manajemen) (Memotivasi, Mengarahkan) (Orang) (Mencapai Tujuan)." },
  { "en": "Apa Itu Pengendalian (Controlling)?", "id": "Fungsi (Manajemen) (Memantau Kinerja) (Membandingkan Dengan Standar)." },
  { "en": "Apa Itu Struktur Organisasi (Organizational Structure)?", "id": "Sistem (Formal) (Tugas, Wewenang) (Dalam Organisasi)." },
  { "en": "Apa Itu Struktur Fungsional (Functional Structure)?", "id": "Mengelompokkan (Pekerjaan) (Berdasarkan Fungsi) (Pemasaran, Keuangan)." },
  { "en": "Apa Itu Struktur Divisional (Divisional Structure)?", "id": "Mengelompokkan (Pekerjaan) (Berdasarkan Produk, Geografis, Pelanggan)." },
  { "en": "Apa Itu Struktur Matriks (Matrix Structure)?", "id": "Struktur (Hibrida) (Menggabungkan) (Fungsional Dan Divisional)." },
  { "en": "Apa Itu Rentang Kendali (Span of Control)?", "id": "Jumlah (Bawahan) (Yang Dapat Diawasi) (Manajer) (Efektif)." },
  { "en": "Apa Itu Sentralisasi (Centralization)?", "id": "Pengambilan (Keputusan) (Terkonsentrasi) (Di Manajemen Puncak)." },
  { "en": "Apa Itu Desentralisasi (Decentralization)?", "id": "Pengambilan (Keputusan) (Didistribusikan) (Ke Manajer Level Bawah)." },
  { "en": "Apa Itu Budaya Organisasi (Organizational Culture)?", "id": "Nilai (Keyakinan, Norma) (Bersama) (Dalam Organisasi)." },
  { "en": "Apa Itu Manajemen Sumber Daya Manusia (HRM)?", "id": "Manajemen (Fungsi) (Perekrutan, Pelatihan, Kompensasi) Karyawan." },
  { "en": "Apa Itu Perekrutan (Recruitment)?", "id": "Proses (Menarik) (Mencari) (Kandidat) (Pekerjaan)." },
  { "en": "Apa Itu Seleksi (Selection)?", "id": "Proses (Memilih) (Kandidat Terbaik) (Dari Pelamar)." },
  { "en": "Apa Itu Pelatihan (Training)?", "id": "Proses (Mengajarkan) (Keterampilan Teknis) (Spesifik) (Pekerjaan)." },
  { "en": "Apa Itu Pengembangan (Development)?", "id": "Proses (Meningkatkan) (Kemampuan) (Karyawan) (Jangka Panjang)." },
  { "en": "Apa Itu Penilaian Kinerja (Performance Appraisal)?", "id": "Proses (Evaluasi) (Kinerja) (Karyawan) (Secara Periodik)." },
  { "en": "Apa Itu Kompensasi (Compensation)?", "id": "Total (Imbalan) (Finansial, Non-Finansial) (Diterima Karyawan)." },
  { "en": "Apa Itu Gaji (Salary)?", "id": "Kompensasi (Tetap) (Dibayar) (Secara Periodik) (Bulanan)." },
  { "en": "Apa Itu Upah (Wage)?", "id": "Kompensasi (Berdasarkan) (Jam Kerja) (Atau Unit Produksi)." },
  { "en": "Apa Itu Listrik Statis (Static Electricity)?", "id": "Ketidakseimbangan Muatan Listrik Pada Permukaan Benda." },
  { "en": "Apa Kepanjangan ESD (Electrostatic Discharge)?", "id": "Pelepasan Muatan Listrik Statis." },
  { "en": "Mengapa ESD (Electrostatic Discharge) Berbahaya Bagi Elektronik?", "id": "Dapat Merusak Komponen Internal Yang Sensitif." },
  { "en": "Apa Fungsi Gelang Anti-Statis (ESD Strap)?", "id": "Menyalurkan Muatan Statis Tubuh Ke Ground." },
  { "en": "Apa Fungsi Matras Anti-Statis (ESD Mat)?", "id": "Menyalurkan Muatan Statis Peralatan Di Meja Kerja." },
  { "en": "Apa Itu Kantong Anti-Statis (ESD Bag)?", "id": "Kantong Khusus Melindungi Komponen Dari ESD." },
  { "en": "Apa Itu Ionizer (Ion Blower)?", "id": "Alat Meniupkan Ion Menetralkan Muatan Statis." },
  { "en": "Apa Itu APD (Alat Pelindung Diri)?", "id": "Peralatan Wajib Untuk Keselamatan Pekerja." },
  { "en": "Apa Itu Sepatu Pengaman (Safety Shoes) Listrik?", "id": "Sepatu Isolasi Mencegah Sengatan Listrik Dari Tanah." },
  { "en": "Apa Itu Sarung Tangan Isolasi (Insulating Gloves)?", "id": "Sarung Tangan Karet Tahan Tegangan Listrik Tinggi." },
  { "en": "Apa Itu Pelindung Kulit (Leather Protector) Sarung Tangan?", "id": "Sarung Tangan Kulit Pelindung Mekanis Sarung Tangan Karet." },
  { "en": "Apa Itu Tongkat Isolasi (Hot Stick)?", "id": "Tongkat Isolasi Bekerja Jarak Jauh Tegangan Tinggi." },
  { "en": "Apa Itu Detektor Tegangan (Voltage Detector)?", "id": "Alat Memastikan Rangkaian Listrik Sudah Mati Total." },
  { "en": "Apa Itu Prosedur Tes Tiga Titik (Live-Dead-Live)?", "id": "Memverifikasi Alat Ukur Bekerja Sebelum Menguji Rangkaian Mati." },
  { "en": "Apa Itu Prosedur LOTO (Lockout Tagout)?", "id": "Prosedur Penguncian Isolasi Energi Saat Perawatan." },
  { "en": "Apa Itu Gembok (Lock) LOTO?", "id": "Gembok Pengunci Pribadi Untuk Mengisolasi Sumber Energi." },
  { "en": "Apa Itu Label (Tag) LOTO?", "id": "Label Peringatan Bahaya Jangan Operasikan." },
  { "en": "Apa Itu Hasp (Hasp) LOTO?", "id": "Pengait Multi Gembok Untuk Isolasi Kelompok Kerja." },
  { "en": "Apa Itu Energi Tersimpan (Stored Energy)?", "id": "Energi Sisa Kapasitor Pegas Setelah Isolasi." },
  { "en": "Apa Itu Batas Arc Flash (Arc Flash Boundary)?", "id": "Jarak Aman Minimal Dari Bahaya Ledakan Busur Api." },
  { "en": "Apa Itu Energi Insiden (Incident Energy)?", "id": "Ukuran Energi Panas Ledakan Arc Flash Kalori/Cm²." },
  { "en": "Apa Itu Pakaian FR (Flame Resistant)?", "id": "Pakaian Tahan Api Tidak Meleleh Melindungi Arc Flash." },
  { "en": "Apa Itu Kategori APD (PPE Category) Arc Flash?", "id": "Tingkat Level Proteksi Pakaian Tahan Api." },
  { "en": "Apa Itu Grounding (Pembumian) Temporer?", "id": "Menghubungkan Konduktor Ke Tanah Saat Perawatan." },
  { "en": "Apa Itu Kawat Jumper (Jumper Lead)?", "id": "Kabel Penghubung Sementara Arus Tinggi." },
  { "en": "Apa Itu Klem (Clamp) Grounding?", "id": "Penjepit Logam Kuat Koneksi Ke Sistem Ground." },
  { "en": "Apa Itu Panel Listrik (Electrical Panel)?", "id": "Kotak Distribusi Pembagi Dan Proteksi Sirkuit Listrik." },
  { "en": "Apa Itu Busbar (Busbar)?", "id": "Batang Konduktor Tembaga Distribusi Daya Di Panel." },
  { "en": "Apa Itu Isolator Standoff (Standoff Insulator)?", "id": "Isolator Penyangga Busbar Di Dalam Panel Listrik." },
  { "en": "Apa Itu Kuncian Kabel (Cable Gland)?", "id": "Konektor Penyekat Kabel Masuk Ke Casing Panel." },
  { "en": "Apa Fungsi Kuncian Kabel (Cable Gland)?", "id": "Menjaga Rating IP Panel Mencegah Debu Air." },
  { "en": "Apa Itu Pemanas Ruang (Space Heater) Panel?", "id": "Pemanas Listrik Mencegah Kondensasi Kelembaban Panel." },
  { "en": "Apa Itu Termostat (Thermostat) Panel?", "id": "Saklar Otomatis Mengontrol Pemanas Ruang Panel." },
  { "en": "Apa Itu Diagram Satu Garis (One-Line Diagram)?", "id": "Gambar Sederhana Sistem Listrik Menggunakan Satu Garis." },
  { "en": "Apa Itu Diagram Pengkabelan (Wiring Diagram)?", "id": "Gambar Detail Koneksi Fisik Kabel Sebenarnya." },
  { "en": "Apa Itu Diagram Skematik (Schematic Diagram)?", "id": "Gambar Logika Rangkaian Menggunakan Simbol Elektronik." },
  { "en": "Apa Itu Daftar Material (Bill of Materials/BOM)?", "id": "Daftar Semua Komponen Dan Kuantitas Proyek." },
  { "en": "Apa Itu Panel MDP (Main Distribution Panel)?", "id": "Panel Distribusi Utama Setelah Sumber Daya Utama." },
  { "en": "Apa Itu Panel SDP (Sub Distribution Panel)?", "id": "Panel Distribusi Cabang Setelah MDP Menuju Beban." },
  { "en": "Apa Itu Panel MCC (Motor Control Center)?", "id": "Panel Khusus Kumpulan Starter Motor Industri." },
  { "en": "Apa Itu Starter DOL (Direct On-Line)?", "id": "Starter Motor Langsung Ke Jaringan Arus Start Tinggi." },
  { "en": "Apa Itu Starter Bintang-Delta (Star-Delta)?", "id": "Starter Mengurangi Arus Start Motor Tiga Fasa." },
  { "en": "Apa Itu Soft Starter (Soft Starter)?", "id": "Starter Elektronik Mengurangi Tegangan Start Motor Bertahap." },
  { "en": "Apa Itu VFD (Variable Frequency Drive)?", "id": "Mengatur Kecepatan Motor AC Mengubah Frekuensi." },
  { "en": "Apa Itu Kontak Latching (Seal-In) Starter?", "id": "Kontak Bantu NO Mengunci Kontaktor Tetap On." },
  { "en": "Apa Itu Kontak Interlock (Interlock) Starter?", "id": "Kontak Bantu NC Mencegah Dua Kontaktor Bekerja Bersamaan." },
  { "en": "Apa Itu Uji Injeksi Primer (Primary Injection)?", "id": "Pengujian Breaker Relai Menggunakan Arus Aktual Tinggi." },
  { "en": "Apa Itu Uji Injeksi Sekunder (Secondary Injection)?", "id": "Pengujian Relai Saja Menggunakan Sinyal Kecil." },
  { "en": "Apa Itu Uji DLRO (Digital Low Resistance Ohmmeter)?", "id": "Mengukur Resistansi Sangat Rendah Kontak Breaker Busbar." },
  { "en": "Apa Itu Uji TTR (Transformer Turns Ratio)?", "id": "Menguji Rasio Perbandingan Lilitan Transformator." },
  { "en": "Apa Itu Uji Resistansi Lilitan (Winding Resistance)?", "id": "Mengukur Resistansi DC Lilitan Trafo Motor." },
  { "en": "Apa Itu Uji Faktor Daya (Power Factor) Trafo?", "id": "Mengukur Kualitas Dielektrik Isolasi Trafo." },
  { "en": "Apa Itu Uji Arus Eksitasi (Excitation Current) Trafo?", "id": "Mengukur Arus Tanpa Beban Mendeteksi Masalah Inti." },
  { "en": "Apa Itu Uji SFRA (Sweep Frequency Response Analysis)?", "id": "Mendeteksi Pergeseran Mekanis Lilitan Internal Trafo." },
  { "en": "Apa Itu Tes TDR (Time-Domain Reflectometer)?", "id": "Mencari Lokasi Jarak Putus Atau Hubung Singkat Kabel." },
  { "en": "Apa Itu Tes VLF (Very Low Frequency) Kabel?", "id": "Tes Hipot Kabel Tegangan Menengah Frekuensi Sangat Rendah." },
  { "en": "Apa Itu Tes Tan Delta (Tan Delta Test) Kabel?", "id": "Mengukur Kualitas Deteriorasi Isolasi Kabel." },
  { "en": "Apa Itu Tes PD (Partial Discharge) Kabel?", "id": "Mendeteksi Cacat Kecil Void Rongga Udara Isolasi Kabel." },
  { "en": "Apa Itu Termografi (Thermography) Inframerah?", "id": "Inspeksi Tanpa Kontak Menggunakan Kamera Panas Inframerah." },
  { "en": "Apa Itu Hot Spot (Titik Panas) Listrik?", "id": "Area Suhu Tinggi Abnormal Koneksi Longgar Beban Lebih." },
  { "en": "Apa Itu Emisivitas (Emissivity)?", "id": "Kemampuan Permukaan Benda Memancarkan Panas Radiasi." },
  { "en": "Apa Itu Analisis Getaran (Vibration Analysis)?", "id": "Mendeteksi Kerusakan Mekanis Mesin Berputar Bearing Unbalance." },
  { "en": "Apa Itu Akselerometer (Accelerometer)?", "id": "Sensor Piezoelektrik Mengukur Getaran Percepatan." },
  { "en": "Apa Itu Spektrum (Spectrum) FFT Getaran?", "id": "Grafik Amplitudo Getaran Terhadap Frekuensi." },
  { "en": "Apa Itu Ketidakseimbangan (Unbalance) Mekanis?", "id": "Penyebab Getaran Massa Rotor Tidak Seimbang (Di 1x RPM)." },
  { "en": "Apa Itu Ketidaklurusan (Misalignment) Poros?", "id": "Penyebab Getaran Poros Tidak Segaris (Di 2x RPM)." },
  { "en": "Apa Itu Kelonggaran (Looseness) Mekanis?", "id": "Penyebab Getaran Baut Longgar Atau Fondasi Retak." },
  { "en": "Apa Itu Kerusakan Bantalan (Bearing Failure)?", "id": "Penyebab Getaran Frekuensi Tinggi Khas Cacat Bearing." },
  { "en": "Apa Itu Pelurusan (Alignment) Poros?", "id": "Proses Meluruskan Sumbu Poros Motor Dan Beban." },
  { "en": "Apa Itu Pelurusan Laser (Laser Alignment)?", "id": "Metode Pelurusan Poros Akurat Menggunakan Laser." },
  { "en": "Apa Itu Kaki Lunak (Soft Foot) Motor?", "id": "Kondisi Kaki Dudukan Motor Tidak Menapak Rata Miring." },
  { "en": "Apa Itu Shim (Shim) Kaki Motor?", "id": "Pelat Logam Tipis Presisi Pengganjal Kaki Motor." },
  { "en": "Apa Itu Keseimbangan (Balancing) Rotor?", "id": "Proses Menyeimbangkan Distribusi Massa Rotor Mengurangi Getaran." },
  { "en": "Apa Itu Analisis Pelumas (Oil Analysis)?", "id": "Menganalisis Sampel Oli Mendeteksi Keausan Internal Mesin." },
  { "en": "Apa Itu Spektrometri (Spectrometry) Oli?", "id": "Mendeteksi Kandungan Logam Keausan Besi Tembaga." },
  { "en": "Apa Itu Viskositas (Viscosity) Oli?", "id": "Mengukur Kekentalan Oli Degradasi Pelumas." },
  { "en": "Apa Itu Analisis Partikel (Particle Count) Oli?", "id": "Menghitung Jumlah Partikel Kontaminasi Kotoran." },
  { "en": "Apa Itu Analisis Ultrasonik (Ultrasound Analysis)?", "id": "Mendeteksi Suara Frekuensi Tinggi Kebocoran Friksi." },
  { "en": "Apa Itu Pemeliharaan Prediktif (PdM)?", "id": "Memprediksi Kapan Kerusakan Terjadi Berbasis Kondisi." },
  { "en": "Apa Itu Pemeliharaan Preventif (PM)?", "id": "Pemeliharaan Terjadwal Berbasis Waktu Atau Jam Pakai." },
  { "en": "Apa Itu Pemeliharaan Korektif (Corrective/Reactive)?", "id": "Perbaikan Dilakukan Setelah Terjadi Kerusakan." },
  { "en": "Apa Itu RCM (Reliability Centered Maintenance)?", "id": "Metodologi Menentukan Strategi Pemeliharaan Optimal." },
  { "en": "Apa Itu MTBF (Mean Time Between Failures)?", "id": "Waktu Rata-Rata Antara Satu Kegagalan Ke Berikutnya." },
  { "en": "Apa Itu MTTR (Mean Time To Repair)?", "id": "Waktu Rata-Rata Dibutuhkan Untuk Memperbaiki Kerusakan." },
  { "en": "Apa Itu Ketersediaan (Availability)?", "id": "Persentase Waktu Peralatan Siap Beroperasi." },
  { "en": "Apa Itu CMMS (Computerized Maintenance Management System)?", "id": "Perangkat Lunak Manajemen Perintah Kerja Aset Inventaris." },
  { "en": "Apa Itu Perintah Kerja (Work Order)?", "id": "Dokumen Perintah Resmi Melakukan Pekerjaan Pemeliharaan." },
  { "en": "Apa Itu SOP (Standard Operating Procedure)?", "id": "Instruksi Langkah-Langkah Standar Melakukan Pekerjaan." },
  { "en": "Apa Itu Analisis Akar Masalah (RCA)?", "id": "Metode Mencari Penyebab Paling Dasar Kegagalan." },
  { "en": "Apa Kepanjangan RCA (Root Cause Analysis)?", "id": "Analisis Akar Masalah." },
  { "en": "Apa Itu Diagram Pareto (Pareto Chart)?", "id": "Grafik Batang Garis Menunjukkan Prinsip 80/20 Masalah." },
  { "en": "Apa Itu Diagram Tulang Ikan (Ishikawa/Fishbone)?", "id": "Diagram Sebab-Akibat Menganalisis Penyebab Masalah." },
  { "en": "Apa Itu Lima Mengapa (5 Whys)?", "id": "Teknik RCA Bertanya 'Mengapa' Berulang Mencari Akar." },
  { "en": "Apa Itu Audit (Audit) Keselamatan?", "id": "Inspeksi Sistematis Memastikan Kepatuhan Prosedur Keselamatan." },
  { "en": "Apa Itu Izin Kerja (Permit To Work)?", "id": "Izin Tertulis Resmi Melakukan Pekerjaan Berisiko." },
  { "en": "Apa Itu Izin Kerja Panas (Hot Work Permit)?", "id": "Izin Kerja Melibatkan Api Las Gerinda." },
  { "en": "Apa Itu Izin Kerja Ruang Terbatas (Confined Space Permit)?", "id": "Izin Kerja Masuk Ruang Terbatas Tangki Pipa." },
  { "en": "Apa Itu Izin Kerja Listrik (Electrical Work Permit)?", "id": "Izin Kerja Pada Peralatan Listrik Bertegangan." },
  { "en": "Apa Itu JSA (Job Safety Analysis)?", "id": "Analisis Keselamatan Langkah Kerja Identifikasi Bahaya." },
  { "en": "Apa Kepanjangan JSA (Job Safety Analysis)?", "id": "Analisis Keselamatan Pekerjaan." },
  { "en": "Apa Itu Grounding (Pembumian) Temporer?", "id": "Menghubungkan Kabel Konduktor Ke Tanah Saat Perawatan." },
  { "en": "Mengapa Perlu Grounding (Pembumian) Temporer?", "id": "Melindungi Pekerja Dari Tegangan Induksi Atau Kesalahan." },
  { "en": "Apa Itu APAR (Alat Pemadam Api Ringan)?", "id": "Tabung Pemadam Api Portabel." },
  { "en": "Apa Itu APAR (Alat Pemadam Api Ringan) Kelas C (Class C)?", "id": "Pemadam Khusus Kebakaran Akibat Listrik Non-Konduktif." },
  { "en": "Jenis Media Apa Untuk APAR (Alat Pemadam Api Ringan) Kelas C?", "id": "Karbon Dioksida CO2 Atau Dry Chemical Powder." },
  { "en": "Mengapa Air Tidak Boleh Untuk Kebakaran Listrik?", "id": "Air Menghantarkan Listrik Bahaya Sengatan Listrik." },
  { "en": "Apa Itu Kumparan Rogowski (Rogowski Coil)?", "id": "Sensor Arus Fleksibel Mengukur Arus AC Tanpa Inti." },
  { "en": "Bagaimana Kumparan Rogowski (Rogowski Coil) Bekerja?", "id": "Mengukur Laju Perubahan Medan Magnet (Membutuhkan Integrator)." },
  { "en": "Apa Keunggulan Kumparan Rogowski?", "id": "Tidak Ada Saturasi Magnetik, Linear, Fleksibel." },
  { "en": "Apa Itu Shunt (Shunt) Arus?", "id": "Resistor Presisi Nilai Sangat Rendah Untuk Mengukur Arus DC." },
  { "en": "Apa Itu FOCS (Fiber Optic Current Sensor)?", "id": "Sensor Arus Menggunakan Serat Optik Dan Efek Faraday." },
  { "en": "Apa Itu Efek Faraday (Faraday Effect)?", "id": "Rotasi Polarisasi Cahaya Akibat Medan Magnet." },
  { "en": "Apa Itu Anemometer Kawat Panas (Hot-Wire Anemometer)?", "id": "Sensor Mengukur Aliran Udara Berdasarkan Pendinginan Kawat." },
  { "en": "Apa Itu Sinkronoskop (Synchroscope)?", "id": "Alat Visual Membantu Sinkronisasi Paralel Generator." },
  { "en": "Apa Itu Pengukur Urutan Fasa (Phase Sequence Meter)?", "id": "Alat Memverifikasi Urutan Fasa R-S-T Sudah Benar." },
  { "en": "Apa Itu Uji Tahan Potensial Tinggi (Hipot Test)?", "id": "Tes Menekankan Isolasi Tegangan Tinggi Mendeteksi Kebocoran." },
  { "en": "Apa Tujuan Tes Hipot (Hipot Test)?", "id": "Memastikan Isolasi Kuat Tidak Tembus Breakdown." },
  { "en": "Apa Itu Tes VLF (Very Low Frequency)?", "id": "Tes Hipot Kabel Tegangan Menengah Menggunakan Frekuensi Rendah." },
  { "en": "Apa Itu Tes Tan Delta (Tan Delta Test)?", "id": "Tes Mengukur Faktor Disipasi Kualitas Isolasi." },
  { "en": "Apa Itu Tes Pelepasan Sebagian (Partial Discharge/PD)?", "id": "Tes Mendeteksi Lucutan Listrik Kecil Di Dalam Isolasi." },
  { "en": "Apa Itu SFRA (Sweep Frequency Response Analysis)?", "id": "Tes Mendiagnosis Integritas Mekanis Internal Trafo." },
  { "en": "Apa Itu DGA (Dissolved Gas Analysis)?", "id": "Analisis Gas Terlarut Dalam Minyak Trafo Mendeteksi Gangguan." },
  { "en": "Gas Apa Menandakan Arcing (Arcing) Di Trafo?", "id": "Asetilena C2H2." },
  { "en": "Gas Apa Menandakan Panas Berlebih (Overheating) Di Trafo?", "id": "Etana C2H6 Dan Metana CH4." },
  { "en": "Gas Apa Menandakan Korona (Corona/PD) Di Trafo?", "id": "Hidrogen H2." },
  { "en": "Apa Itu Uji Resistansi Isolasi?", "id": "Mengukur Resistansi Isolasi Menggunakan Megohmmeter." },
  { "en": "Apa Itu Megohmmeter (Megger)?", "id": "Alat Uji Menghasilkan Tegangan Tinggi DC Mengukur Resistansi Tinggi." },
  { "en": "Apa Itu Tes Indeks Polarisasi (PI)?", "id": "Rasio Tes Megger 10 Menit Dibagi 1 Menit." },
  { "en": "Apa Arti Nilai PI (Polarization Index) Rendah?", "id": "Isolasi Kemungkinan Lembab Atau Terkontaminasi." },
  { "en": "Apa Itu Tes Rasio Penyerapan Dielektrik (DAR)?", "id": "Rasio Tes Megger 60 Detik Dibagi 30 Detik." },
  { "en": "Apa Arti Nilai DAR (Dielectric Absorption Ratio) Rendah?", "id": "Isolasi Kemungkinan Menyerap Kelembaban." },
  { "en": "Apa Itu DLRO (Digital Low Resistance Ohmmeter)?", "id": "Alat Ukur Resistansi Sangat Rendah (Mikro Ohm)." },
  { "en": "Mengapa DLRO (Digital Low Resistance Ohmmeter) Menggunakan 4 Kawat (Kelvin)?", "id": "Menghilangkan Resistansi Kabel Pengukuran Dari Hasil Tes." },
  { "en": "Apa Aplikasi Tes DLRO (Digital Low Resistance Ohmmeter)?", "id": "Mengukur Resistansi Kontak Breaker Atau Sambungan Busbar." },
  { "en": "Apa Itu Filtrasi (Filtration) Minyak Trafo?", "id": "Proses Memurnikan Minyak Trafo Dari Air Dan Kotoran." },
  { "en": "Apa Itu Silika Gel (Silica Gel) Pernapasan Trafo?", "id": "Bahan Penyerap Kelembaban Udara Yang Masuk Trafo." },
  { "en": "Apa Warna Silika Gel (Silica Gel) Kering?", "id": "Biru Terang Atau Oranye." },
  { "en": "Apa Warna Silika Gel (Silica Gel) Jenuh?", "id": "Merah Jambu (Pink) Atau Hijau Tua (Tergantung Jenis)." },
  { "en": "Apa Itu Konservator (Conservator) Trafo?", "id": "Tangki Ekspansi Minyak Di Atas Trafo Utama." },
  { "en": "Apa Itu Relai Buchholz (Buchholz Relay)?", "id": "Relai Mekanis Deteksi Gas Akibat Gangguan Internal Trafo." },
  { "en": "Apa Itu PRD (Pressure Relief Device) Trafo?", "id": "Alat Pelepas Tekanan Darurat Jika Tekanan Internal Berlebih." },
  { "en": "Apa Itu OTI (Oil Temperature Indicator)?", "id": "Indikator Pengukur Suhu Minyak Trafo." },
  { "en": "Apa Itu WTI (Winding Temperature Indicator)?", "id": "Indikator Pengukur Estimasi Suhu Lilitan Trafo." },
  { "en": "Apa Itu Pengubah Tap (Tap Changer) Trafo?", "id": "Alat Mengubah Rasio Lilitan Trafo (Mengatur Tegangan)." },
  { "en": "Apa Itu OLTC (On-Load Tap Changer)?", "id": "Pengubah Tap Yang Dapat Beroperasi Saat Trafo Berbeban." },
  { "en": "Apa Itu DETC (De-Energized Tap Changer)?", "id": "Pengubah Tap Yang Harus Dioperasikan Saat Trafo Mati." },
  { "en": "Apa Itu Switchgear (Switchgear)?", "id": "Kumpulan Peralatan Pemutus Penghubung Proteksi Sirkuit Listrik." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker/CB)?", "id": "Alat Memutus Arus Beban Penuh Dan Arus Gangguan." },
  { "en": "Apa Itu Pemisah (Disconnecting Switch/DS)?", "id": "Alat Memisahkan Sirkuit Dalam Keadaan Tanpa Beban." },
  { "en": "Apa Itu Saklar Pembumian (Earthing Switch/ES)?", "id": "Saklar Menghubungkan Sirkuit Ke Tanah Untuk Keamanan Perawatan." },
  { "en": "Apa Itu Saklar Pemutus Beban (Load Break Switch/LBS)?", "id": "Saklar Mampu Memutus Arus Beban Normal (Bukan Gangguan)." },
  { "en": "Apa Itu VCB (Vacuum Circuit Breaker)?", "id": "Pemutus Sirkuit Menggunakan Ruang Hampa (Vakum) Media Isolasi." },
  { "en": "Apa Itu SF6CB (SF6 Circuit Breaker)?", "id": "Pemutus Sirkuit Menggunakan Gas SF6 Media Pemadam Busur." },
  { "en": "Apa Itu ACB (Air Circuit Breaker)?", "id": "Pemutus Sirkuit (Tegangan Rendah, Arus Besar) Media Udara." },
  { "en": "Apa Itu MCCB (Molded Case Circuit Breaker)?", "id": "Pemutus Sirkuit (Tegangan Rendah) Casing Cetak Terisolasi." },
  { "en": "Apa Itu MCB (Miniature Circuit Breaker)?", "id": "Pemutus Sirkuit (Kecil) Proteksi Sirkuit Final (Rumah)." },
  { "en": "Apa Itu Bilik Busur Api (Arc Chute)?", "id": "Komponen Pemutus Sirkuit Memecah Mendinginkan Busur Api." },
  { "en": "Apa Itu Peringkat Interupsi (Interrupting Rating)?", "id": "Arus Gangguan Maksimum Yang Dapat Diputus Breaker." },
  { "en": "Apa Itu CT (Current Transformer)?", "id": "Trafo Instrumen Menurunkan Arus Tinggi Ke Level Pengukuran." },
  { "en": "Apa Itu PT (Potential Transformer) Atau VT (Voltage Transformer)?", "id": "Trafo Instrumen Menurunkan Tegangan Tinggi Ke Level Pengukuran." },
  { "en": "Apa Itu Rasio CT (CT Ratio)?", "id": "Perbandingan Arus Primer Dan Sekunder (Contoh: 100/5 Amper)." },
  { "en": "Apa Itu Beban (Burden) CT?", "id": "Total Impedansi VA Yang Terhubung Ke Sisi Sekunder CT." },
  { "en": "Mengapa Sekunder CT (Current Transformer) Tidak Boleh Terbuka?", "id": "Menghasilkan Tegangan Sangat Tinggi Berbahaya Dan Merusak CT." },
  { "en": "Apa Itu Saturasi (Saturation) CT?", "id": "Kondisi Inti CT Jenuh Output Tidak Linear Lagi." },
  { "en": "Apa Itu Polaritas (Polarity) CT?", "id": "Penanda Titik (Dot) Menunjukkan Arah Aliran Arus Relatif." },
  { "en": "Apa Itu Relai Proteksi (Protection Relay)?", "id": "Perangkat Mendeteksi Kondisi Abnormal Memicu Pemutus Sirkuit." },
  { "en": "Apa Itu Kode ANSI (American National Standards Institute) 50?", "id": "Fungsi Relai Proteksi Arus Lebih Seketika." },
  { "en": "Apa Itu Kode ANSI (American National Standards Institute) 51?", "id": "Fungsi Relai Proteksi Arus Lebih Waktu Tunda." },
  { "en": "Apa Itu Kode ANSI (American National Standards Institute) 87?", "id": "Fungsi Relai Proteksi Diferensial." },
  { "en": "Apa Itu Kode ANSI (American National Standards Institute) 27?", "id": "Fungsi Relai Proteksi Tegangan Kurang." },
  { "en": "Apa Itu Kode ANSI (American National Standards Institute) 59?", "id": "Fungsi Relai Proteksi Tegangan Lebih." },
  { "en": "Apa Itu Kode ANSI (American National Standards Institute) 81?", "id": "Fungsi Relai Proteksi Frekuensi (Naik/Turun)." },
  { "en": "Apa Itu Kode ANSI (American National Standards Institute) 67?", "id": "Fungsi Relai Proteksi Arus Lebih Berarah." },
  { "en": "Apa Itu Kode ANSI (American National Standards Institute) 25?", "id": "Fungsi Relai Pengecek Sinkronisasi (Synchro-check)." },
  { "en": "Apa Itu Kode ANSI (American National Standards Institute) 32?", "id": "Fungsi Relai Proteksi Daya Terbalik (Reverse Power)." },
  { "en": "Apa Itu Koordinasi Proteksi (Protection Coordination)?", "id": "Mengatur Relai Agar Bekerja Selektif (Hilir Trip Duluan)." },
  { "en": "Apa Itu Kurva TCC (Time-Current Curve)?", "id": "Grafik Hubungan Waktu Trip Relai Terhadap Besaran Arus." },
  { "en": "Apa Itu Lightning Arrester (Surge Arrester)?", "id": "Perangkat Proteksi Melindungi Peralatan Dari Surja Petir." },
  { "en": "Apa Itu MCOV (Maximum Continuous Operating Voltage)?", "id": "Tegangan Operasi Kontinu Maksimum Yang Diizinkan Arrester." },
  { "en": "Apa Itu Pencacah Surja (Surge Counter)?", "id": "Alat Mencatat Berapa Kali Arrester Bekerja." },
  { "en": "Apa Itu Menara Transmisi (Transmission Tower)?", "id": "Struktur Tiang Penyangga Kabel Transmisi Tegangan Tinggi." },
  { "en": "Apa Itu Isolator Rantai (String Insulator)?", "id": "Rangkaian Piringan Isolator Keramik Kaca Menggantung Kabel." },
  { "en": "Apa Itu Cincin Korona (Corona Ring)?", "id": "Cincin Logam Mengontrol Medan Listrik Di Ujung Isolator." },
  { "en": "Apa Itu Pelepasan Korona (Corona Discharge)?", "id": "Pelepasan Listrik Sebagian Di Udara Sekitar Konduktor HV." },
  { "en": "Apa Itu Konduktor Berkas (Bundled Conductor)?", "id": "Lebih Dari Satu Konduktor Per Fasa (Mengurangi Korona)." },
  { "en": "Apa Itu Spacer (Spacer) Konduktor?", "id": "Alat Menjaga Jarak Antar Konduktor Berkas." },
  { "en": "Apa Itu Peredam Getaran (Vibration Damper)?", "id": "Alat Meredam Getaran Kabel Transmisi Akibat Angin." },
  { "en": "Apa Itu Kabel Pelindung (Shield Wire/Ground Wire)?", "id": "Kabel Paling Atas Menara (Melindungi Fasa Dari Petir)." },
  { "en": "Apa Itu Stator (Stator) Generator?", "id": "Bagian Generator Yang Diam (Berisi Lilitan Armatur)." },
  { "en": "Apa Itu Rotor (Rotor) Generator?", "id": "Bagian Generator Yang Berputar (Berisi Lilitan Medan)." },
  { "en": "Apa Itu Eksiter (Exciter) Generator?", "id": "Sumber Pembangkit Arus DC Untuk Lilitan Medan Rotor." },
  { "en": "Apa Itu Eksiter Tanpa Sikat (Brushless Exciter)?", "id": "Eksiter Modern (Alternator Kecil Dioda Berputar) Tanpa Sikat." },
  { "en": "Apa Itu AVR (Automatic Voltage Regulator)?", "id": "Alat Mengatur Tegangan Output Generator Tetap Konstan." },
  { "en": "Apa Itu Governor (Governor) Mesin?", "id": "Alat Mengatur Bahan Bakar Menjaga Kecepatan Frekuensi Konstan." },
  { "en": "Apa Itu Sinkronisasi (Synchronization) Generator?", "id": "Proses Menghubungkan Generator Secara Paralel Ke Jaringan." },
  { "en": "Apa Itu Pembagian Beban (Load Sharing)?", "id": "Membagi Beban (Daya Aktif Dan Reaktif) Antar Generator." },
  { "en": "Apa Itu Kontrol Droop (Droop Control)?", "id": "Metode Pembagian Beban (Kecepatan Frekuensi Turun) Saat Beban Naik." },
  { "en": "Apa Itu Kontrol Isochronous (Isochronous Control)?", "id": "Metode Kontrol Menjaga Kecepatan Frekuensi Tepat Konstan." },
  { "en": "Apa Itu Kondensor Sinkron (Synchronous Condenser)?", "id": "Motor Sinkron (Tanpa Beban) Mengatur Daya Reaktif Grid." },
  { "en": "Apa Itu PSS (Power System Stabilizer)?", "id": "Kontrol Tambahan Meredam Osilasi Ayunan Daya Generator." },
  { "en": "Apa Itu Inersia (Inertia) Jaringan?", "id": "Energi Kinetik Tersimpan Di Massa Berputar Generator." },
  { "en": "Mengapa Inersia (Inertia) Jaringan Penting?", "id": "Membantu Menjaga Stabilitas Frekuensi Jaringan." },
  { "en": "Apa Itu Black Start (Black Start)?", "id": "Kemampuan Pembangkit Hidup Tanpa Bantuan Listrik Jaringan Eksternal." },
  { "en": "Apa Itu Jaringan Listrik Cerdas (Smart Grid)?", "id": "Jaringan Listrik Modern Menggunakan Komunikasi Digital Dua Arah." },
  { "en": "Apa Itu Meter Cerdas (Smart Meter)?", "id": "Meter Listrik Digital Komunikasi Dua Arah (Mencatat Real-Time)." },
  { "en": "Apa Itu AMI (Advanced Metering Infrastructure)?", "id": "Infrastruktur Jaringan Komunikasi Untuk Meter Cerdas." },
  { "en": "Apa Itu Respon Permintaan (Demand Response)?", "id": "Program Konsumen Mengurangi Beban Listrik Saat Puncak." },
  { "en": "Apa Itu Pembangkit Terdistribusi (Distributed Generation/DER)?", "id": "Pembangkit Skala Kecil Tersebar Dekat Konsumen (PLTS Atap)." },
  { "en": "Apa Itu Jaringan Mikro (Microgrid)?", "id": "Jaringan Listrik Lokal Skala Kecil (Bisa Lepas Dari Jaringan Utama)." },
  { "en": "Apa Itu V2G (Vehicle-to-Grid)?", "id": "Teknologi Mobil Listrik Mengirim Daya Kembali Ke Jaringan." },
  { "en": "Apa Itu Penyimpanan Energi (Energy Storage)?", "id": "Menyimpan Energi Listrik Untuk Digunakan Nanti." },
  { "en": "Apa Itu BESS (Battery Energy Storage System)?", "id": "Sistem Penyimpanan Energi Menggunakan Baterai Skala Besar." },
  { "en": "Apa Fungsi BESS (Battery Energy Storage System) Di Jaringan?", "id": "Menstabilkan Frekuensi, Cadangan Daya, Menggeser Beban." },
  { "en": "Apa Itu Penggeseran Beban (Load Shifting)?", "id": "Mengisi Baterai Saat Beban Murah, Melepas Saat Puncak." },
  { "en": "Apa Itu Pencukuran Puncak (Peak Shaving)?", "id": "Menggunakan Baterai Mengurangi Beban Puncak Jaringan." },
  { "en": "Apa Itu Penyimpanan Energi Udara Terkompresi (CAES)?", "id": "Menyimpan Energi Memompa Udara Ke Rongga Bawah Tanah." },
  { "en": "Apa Itu Penyimpanan Energi Pompa Air (Pumped Hydro)?", "id": "Menyimpan Energi Memompa Air Ke Reservoir Lebih Tinggi." },
  { "en": "Apa Itu Penyimpanan Energi Roda Gila (Flywheel)?", "id": "Menyimpan Energi Kinetik Dalam Rotor Berputar Cepat." },
  { "en": "Apa Itu Sel Bahan Bakar (Fuel Cell)?", "id": "Perangkat Elektrokimia Mengubah Bahan Bakar Menjadi Listrik." },
  { "en": "Apa Itu Sel Bahan Bakar Hidrogen (Hydrogen Fuel Cell)?", "id": "Mengubah Hidrogen Dan Oksigen Menjadi Listrik Dan Air." },
  { "en": "Apa Itu Elektroliser (Electrolyzer)?", "id": "Perangkat Menggunakan Listrik Memecah Air Menjadi Hidrogen." },
  { "en": "Apa Itu Hidrogen Hijau (Green Hydrogen)?", "id": "Hidrogen Dihasilkan Menggunakan Energi Terbarukan." },
  { "en": "Apa Itu Hidrogen Biru (Blue Hydrogen)?", "id": "Hidrogen Dihasilkan Dari Gas Alam Dengan Penangkapan Karbon." },
  { "en": "Apa Itu Hidrogen Abu-Abu (Grey Hydrogen)?", "id": "Hidrogen Dihasilkan Dari Gas Alam Tanpa Penangkapan Karbon." },
  { "en": "Apa Itu Penangkapan Karbon (Carbon Capture)?", "id": "Teknologi Menangkap Emisi CO2 Mencegah Ke Atmosfer." },
  { "en": "Apa Itu Standar (Standard)?", "id": "Dokumen Aturan Pedoman Spesifikasi Teknis Yang Disepakati." },
  { "en": "Apa Itu IEC (International Electrotechnical Commission)?", "id": "Organisasi Standar Internasional Bidang Elektro." },
  { "en": "Apa Itu IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Organisasi Profesional Teknik Standar (Wi-Fi, Ethernet)." },
  { "en": "Apa Itu ISO (International Organization for Standardization)?", "id": "Organisasi Standar Internasional (Non-Elektro, Manajemen Mutu)." },
  { "en": "Apa Itu Standar ISO 9001?", "id": "Standar Sistem Manajemen Mutu (Quality Management)." },
  { "en": "Apa Itu Standar ISO 14001?", "id": "Standar Sistem Manajemen Lingkungan (Environmental)." },
  { "en": "Apa Itu Standar ISO 45001?", "id": "Standar Sistem Manajemen Keselamatan Kesehatan Kerja (K3)." },
  { "en": "Apa Itu Standar ISO 50001?", "id": "Standar Sistem Manajemen Energi (Energy Management)." },
  { "en": "Apa Itu Standar ISO 27001?", "id": "Standar Sistem Manajemen Keamanan Informasi (ISMS)." },
  { "en": "Apa Itu ANSI (American National Standards Institute)?", "id": "Badan Standar Nasional Amerika Serikat." },
  { "en": "Apa Itu NEMA (National Electrical Manufacturers Association)?", "id": "Asosiasi Pabrikan Listrik Amerika (Standar Enclosure)." },
  { "en": "Apa Itu NFPA (National Fire Protection Association)?", "id": "Organisasi Standar Pencegahan Kebakaran Amerika." },
  { "en": "Apa Itu NFPA 70 (National Electrical Code/NEC)?", "id": "Standar Kode Instalasi Listrik Bangunan Di Amerika." },
  { "en": "Apa Itu NFPA 70E (Standard for Electrical Safety)?", "id": "Standar Keselamatan Listrik Tempat Kerja (Arc Flash)." },
  { "en": "Apa Itu OSHA (Occupational Safety and Health Administration)?", "id": "Badan Pemerintah Amerika Mengatur Keselamatan Kerja." },
  { "en": "Apa Itu UL (Underwriters Laboratories)?", "id": "Organisasi Sertifikasi Pengujian Keamanan Produk Amerika." },
  { "en": "Apa Itu CE Marking (CE Mark)?", "id": "Tanda Konformitas Produk (Aman) Untuk Pasar Uni Eropa." },
  { "en": "Apa Itu RoHS (Restriction of Hazardous Substances)?", "id": "Direktif Uni Eropa Membatasi Bahan Berbahaya Elektronik." },
  { "en": "Apa Itu WEEE (Waste Electrical and Electronic Equipment)?", "id": "Direktif Uni Eropa Mengelola Limbah Peralatan Elektronik." },
  { "en": "Apa Itu ATEX (ATEX Directive)?", "id": "Direktif Uni Eropa Peralatan Di Area Berbahaya (Ledakan)." },
  { "en": "Apa Itu IECEx (IEC System for Certification to Standards)?", "id": "Sistem Sertifikasi Internasional (IEC) Peralatan Area Berbahaya." },
  { "en": "Apa Itu SNI (Standar Nasional Indonesia)?", "id": "Standar Nasional Yang Berlaku Di Indonesia." },
  { "en": "Apa Itu BSN (Badan Standardisasi Nasional)?", "id": "Lembaga Pemerintah Menetapkan Standar SNI." },
  { "en": "Apa Itu PUIL (Persyaratan Umum Instalasi Listrik)?", "id": "Standar (SNI) Acuan Instalasi Listrik Di Indonesia." },
  { "en": "Apa Itu SLD (Single Line Diagram)?", "id": "Diagram Satu Garis Sistem Tenaga Listrik." },
  { "en": "Apa Itu Kontrol Proses (Process Control)?", "id": "Disiplin Teknik Mengelola Variabel Proses Industri." },
  { "en": "Apa Itu Variabel Proses (Process Variable/PV)?", "id": "Parameter Fisik Yang Diukur Dikontrol (Suhu, Tekanan)." },
  { "en": "Apa Itu Setpoint (Setpoint/SP)?", "id": "Nilai Target Yang Diinginkan Untuk Variabel Proses." },
  { "en": "Apa Itu Sinyal Error (Error Signal)?", "id": "Selisih Antara Setpoint (SP) Dan Variabel Proses (PV)." },
  { "en": "Apa Itu Variabel Termanipulasi (Manipulated Variable/MV)?", "id": "Variabel Output Kontroler (Mengatur Elemen Kontrol Final)." },
  { "en": "Apa Itu Elemen Kontrol Final (Final Control Element)?", "id": "Perangkat Fisik Mengubah Proses (Control Valve, Heater)." },
  { "en": "Apa Itu Gangguan (Disturbance) Kontrol Proses?", "id": "Input Tak Diinginkan Mempengaruhi Variabel Proses." },
  { "en": "Apa Itu Kontrol Loop Terbuka (Open-Loop Control)?", "id": "Sistem Kontrol Tanpa Umpan Balik (Output Tidak Diukur)." },
  { "en": "Apa Itu Kontrol Loop Tertutup (Closed-Loop Control)?", "id": "Sistem Kontrol Menggunakan Umpan Balik (Error) Mengoreksi Output." },
  { "en": "Apa Itu Kontrol Umpan Balik (Feedback Control)?", "id": "Nama Lain Kontrol Loop Tertutup (Mengoreksi Setelah Error)." },
  { "en": "Apa Itu Kontrol Umpan Maju (Feedforward Control)?", "id": "Kontrol Mengantisipasi Gangguan Sebelum Mempengaruhi PV." },
  { "en": "Apa Itu Kontrol Kaskade (Cascade Control)?", "id": "Sistem Kontrol Dua Loop (Master Dan Slave)." },
  { "en": "Apa Itu Kontrol Rasio (Ratio Control)?", "id": "Sistem Kontrol Menjaga Rasio Tetap Antara Dua Aliran." },
  { "en": "Apa Itu Kontrol PID (Proportional-Integral-Derivative)?", "id": "Algoritma Kontrol Loop Tertutup Paling Umum." },
  { "en": "Apa Itu Aksi Proporsional (P) PID?", "id": "Output Proporsional Terhadap Sinyal Error Saat Ini." },
  { "en": "Apa Itu Aksi Integral (I) PID?", "id": "Output Proporsional Akumulasi Error Masa Lalu." },
  { "en": "Apa Fungsi Aksi Integral (I)?", "id": "Menghilangkan Kesalahan Keadaan Tunak (Steady-State Error)." },
  { "en": "Apa Itu Aksi Derivatif (D) PID?", "id": "Output Proporsional Laju Perubahan Error (Prediksi)." },
  { "en": "Apa Fungsi Aksi Derivatif (D)?", "id": "Mengurangi Lewatan (Overshoot) Mempercepat Respon." },
  { "en": "Apa Itu Penyetelan (Tuning) PID?", "id": "Proses Mencari Nilai Konstanta Kp, Ki, Kd Optimal." },
  { "en": "Apa Itu Metode Tuning Ziegler-Nichols?", "id": "Metode Klasik Tuning PID (Berdasarkan Osilasi)." },
  { "en": "Apa Itu Lewatan (Overshoot) Respon?", "id": "Kondisi PV Melebihi Setpoint Sebelum Stabil." },
  { "en": "Apa Itu Waktu Naik (Rise Time) Respon?", "id": "Waktu PV Naik Dari 10 Persen Ke 90 Persen Setpoint." },
  { "en": "Apa Itu Waktu Mapag (Settling Time) Respon?", "id": "Waktu PV Masuk Ke Batas Toleransi Stabil Sekitar Setpoint." },
  { "en": "Apa Itu Kesalahan Keadaan Tunak (Steady-State Error)?", "id": "Selisih Antara PV Dan SP Setelah Waktu Lama." },
  { "en": "Apa Itu Kontrol On-Off (On-Off Control)?", "id": "Kontrol Paling Sederhana (Output Hanya On Atau Off)." },
  { "en": "Apa Itu Histeresis (Hysteresis) Kontrol On-Off?", "id": "Zona Mati (Deadband) Mencegah Switching Terlalu Cepat." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Industri Digital Mengontrol Mesin Otomasi." },
  { "en": "Apa Itu DCS (Distributed Control System)?", "id": "Sistem Kontrol Terdistribusi (Skala Besar) Proses Pabrik." },
  { "en": "Apa Beda PLC (Programmable Logic Controller) Dan DCS (Distributed Control System)?", "id": "PLC (Cepat, Logika Diskrit), DCS (Skala Besar, Kontrol Proses Kontinu)." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem (Perangkat Lunak) Memantau Mengendalikan Proses (HMI)." },
  { "en": "Apa Itu HMI (Human-Machine Interface)?", "id": "Antarmuka Grafis Visual Operator Berinteraksi Mesin." },
  { "en": "Apa Itu Diagram Tangga (Ladder Logic)?", "id": "Bahasa Pemrograman Grafis PLC (Mirip Skematik Relai)." },
  { "en": "Apa Itu Kontak (Contact) Ladder Logic?", "id": "Instruksi Input (Normally Open/Normally Closed)." },
  { "en": "Apa Itu Kumparan (Coil) Ladder Logic?", "id": "Instruksi Output (Mengaktifkan Mematikan Bit)." },
  { "en": "Apa Itu Rung (Rung) Ladder Logic?", "id": "Satu Garis Horizontal Logika Dalam Diagram Tangga." },
  { "en": "Apa Itu Siklus Pindai (Scan Cycle) PLC?", "id": "Urutan (Baca Input, Eksekusi Program, Tulis Output) PLC." },
  { "en": "Apa Itu Waktu Pindai (Scan Time) PLC?", "id": "Waktu Dibutuhkan PLC Menyelesaikan Satu Siklus Pindai." },
  { "en": "Apa Itu Modul Input (Input Module) PLC?", "id": "Kartu Antarmuka PLC Menerima Sinyal Dari Sensor." },
  { "en": "Apa Itu Modul Output (Output Module) PLC?", "id": "Kartu Antarmuka PLC Mengirim Sinyal Ke Aktuator." },
  { "en": "Apa Itu Modul Input Diskrit (Digital)?", "id": "Modul Input Menerima Sinyal On/Off (Saklar, Tombol)." },
  { "en": "Apa Itu Modul Input Analog?", "id": "Modul Input Menerima Sinyal Kontinu (4-20mA, 0-10V)." },
  { "en": "Apa Itu Modul Output Diskrit (Digital)?", "id": "Modul Output Mengirim Sinyal On/Off (Relai, Lampu)." },
  { "en": "Apa Itu Modul Output Analog?", "id": "Modul Output Mengirim Sinyal Kontinu (4-20mA, 0-10V)." },
  { "en": "Apa Itu Sinyal 4-20 mA?", "id": "Standar Sinyal Arus Analog Industri (4mA = 0 Persen)." },
  { "en": "Mengapa Menggunakan Sinyal Arus (4-20 mA)?", "id": "Lebih Tahan Noise, Mendeteksi Kabel Putus (Jika 0mA)." },
  { "en": "Apa Itu Sinyal 0-10 V?", "id": "Standar Sinyal Tegangan Analog Industri (Kurang Tahan Noise)." },
  { "en": "Apa Itu Protokol HART (HART Protocol)?", "id": "Sinyal Digital (Frekuensi) Menumpang Di Atas Sinyal 4-20mA." },
  { "en": "Apa Itu Fieldbus (Fieldbus)?", "id": "Jaringan Komunikasi Digital Serial Industri (Menggantikan 4-20mA)." },
  { "en": "Apa Itu Profibus (Profibus)?", "id": "Standar Fieldbus Populer (Siemens) Otomasi Industri." },
  { "en": "Apa Itu Modbus (Modbus)?", "id": "Protokol Komunikasi Serial Sederhana Populer (PLC, HMI)." },
  { "en": "Apa Itu Modbus RTU (Modbus RTU)?", "id": "Varian Modbus Serial (Kompak, Biner) (RS-485, RS-232)." },
  { "en": "Apa Itu Modbus TCP/IP (Modbus TCP/IP)?", "id": "Varian Modbus Berjalan Di Atas Jaringan Ethernet." },
  { "en": "Apa Itu Ethernet Industri (Industrial Ethernet)?", "id": "Ethernet Disesuaikan Lingkungan Industri (Kuat, Deterministik)." },
  { "en": "Apa Itu Profinet (Profinet)?", "id": "Standar Ethernet Industri (Siemens) Berbasis Ethernet TCP/IP." },
  { "en": "Apa Itu EtherNet/IP (EtherNet/IP)?", "id": "Standar Ethernet Industri (Rockwell) Berbasis Ethernet TCP/IP." },
  { "en": "Apa Itu OPC (OLE for Process Control)?", "id": "Standar Interoperabilitas Perangkat Lunak Industri (Server Klien)." },
  { "en": "Apa Itu OPC UA (Unified Architecture)?", "id": "Standar OPC (Generasi Baru) (Platform Independen, Aman)." },
  { "en": "Apa Itu Katup Kontrol (Control Valve)?", "id": "Elemen Kontrol Final Mengatur Laju Aliran Fluida." },
  { "en": "Apa Itu Aktuator (Actuator) Katup?", "id": "Perangkat Menggerakkan Membuka Menutup Katup (Pneumatik, Elektrik)." },
  { "en": "Apa Itu Aktuator Pneumatik (Pneumatic Actuator)?", "id": "Aktuator Menggunakan Tekanan Udara (Diafragma, Piston)." },
  { "en": "Apa Itu Aktuator Elektrik (Electric Actuator)?", "id": "Aktuator Menggunakan Motor Listrik." },
  { "en": "Apa Itu Positioner (Positioner) Katup?", "id": "Perangkat (Kontrol Loop Tertutup) (Memastikan Posisi) Katup (Akurat)." },
  { "en": "Apa Itu Konverter I/P (I-to-P Converter)?", "id": "Mengubah Sinyal Kontrol Arus (4-20mA) Menjadi Tekanan Udara." }



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

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


    { "en": "Apa Cabang Teknik Elektro Pengolahan Daya?", "id": "Elektronika Daya (Power Electronics)." },
    { "en": "Apa Fungsi Utama Elektronika Daya?", "id": "Mengonversi Dan Mengontrol Daya Listrik." },
    { "en": "Apa Komponen Saklar Utama Elektronika Daya?", "id": "Semikonduktor Daya." },
    { "en": "Apa Contoh Semikonduktor Daya?", "id": "Dioda, Thyristor, MOSFET, IGBT." },
    { "en": "Apa Saklar Daya Yang Tidak Terkendali?", "id": "Dioda Daya." },
    { "en": "Apa Saklar Daya Semi-Terkendali?", "id": "Thyristor (SCR)." },
    { "en": "Apa Saklar Daya Terkendali Penuh?", "id": "MOSFET, IGBT, GTO." },
    { "en": "Apa Kepanjangan MOSFET?", "id": "Metal-Oxide-Semiconductor Field-Effect Transistor." },
    { "en": "Apa Kepanjangan IGBT?", "id": "Insulated-Gate Bipolar Transistor." },
    { "en": "Apa Kepanjangan SCR?", "id": "Silicon-Controlled Rectifier." },
    { "en": "Apa Konverter Daya Dari AC Ke DC?", "id": "Penyearah (Rectifier)." },
    { "en": "Apa Konverter Daya Dari DC Ke AC?", "id": "Inverter." },
    { "en": "Apa Konverter Daya Dari DC Ke DC?", "id": "Chopper (Konverter DC-DC)." },
    { "en": "Apa Konverter Daya Dari AC Ke AC?", "id": "Kontroler Tegangan AC / Cycloconverter." },
    { "en": "Apa Penyearah Yang Tidak Terkendali?", "id": "Penyearah Dioda." },
    { "en": "Apa Penyearah Yang Terkendali?", "id": "Penyearah Thyristor." },
    { "en": "Apa Penyearah Setengah Gelombang Satu Fasa?", "id": "Menyearahkan Setengah Siklus AC." },
    { "en": "Apa Penyearah Gelombang Penuh Satu Fasa?", "id": "Menyearahkan Seluruh Siklus AC." },
    { "en": "Apa Penyearah Jembatan?", "id": "Penyearah Gelombang Penuh Empat Dioda." },
    { "en": "Apa Penyearah Tiga Fasa?", "id": "Mengubah AC Tiga Fasa Menjadi DC." },
    { "en": "Apa Variasi AC Kecil Pada Output DC?", "id": "Riak (Ripple)." },
    { "en": "Bagaimana Cara Mengurangi Riak?", "id": "Menggunakan Filter Kapasitor Atau Induktor." },
    { "en": "Apa Itu Dioda Freewheeling?", "id": "Memberi Jalur Arus Beban Induktif." },
    { "en": "Apa Fungsi Dioda Freewheeling?", "id": "Mencegah Lonjakan Tegangan Negatif." },
    { "en": "Apa Itu Sudut Penyalaan (Firing Angle)?", "id": "Sudut Pemicuan Thyristor." },
    { "en": "Apa Itu Sudut Konduksi?", "id": "Durasi Thyristor Menghantar." },
    { "en": "Apa Itu Konverter DC-DC Penurun Tegangan?", "id": "Buck Converter." },
    { "en": "Apa Itu Konverter DC-DC Penaik Tegangan?", "id": "Boost Converter." },
    { "en": "Apa Itu Konverter Penaik-Penurun Tegangan?", "id": "Buck-Boost Converter." },
    { "en": "Apa Konverter Yang Membalik Polaritas?", "id": "Buck-Boost, Cuk, SEPIC." },
    { "en": "Apa Itu Siklus Kerja (Duty Cycle)?", "id": "Rasio Waktu ON Terhadap Periode." },
    { "en": "Bagaimana Buck Converter Mengatur Tegangan?", "id": "Dengan Mengubah Siklus Kerja." },
    { "en": "Apa Itu Mode Konduksi Kontinu?", "id": "CCM (Continuous Conduction Mode)." },
    { "en": "Apa Ciri CCM (Continuous Conduction Mode)?", "id": "Arus Induktor Tidak Pernah Nol." },
    { "en": "Apa Itu Mode Konduksi Dis-kontinu?", "id": "DCM (Discontinuous Conduction Mode)." },
    { "en": "Apa Ciri DCM (Discontinuous Conduction Mode)?", "id": "Arus Induktor Menjadi Nol." },
    { "en": "Apa Inverter Sumber Tegangan?", "id": "VSI (Voltage Source Inverter)." },
    { "en": "Apa Inverter Sumber Arus?", "id": "CSI (Current Source Inverter)." },
    { "en": "Apa Inverter Jembatan Setengah?", "id": "Menggunakan Dua Saklar." },
    { "en": "Apa Inverter Jembatan Penuh?", "id": "Menggunakan Empat Saklar." },
    { "en": "Apa Inverter Tiga Fasa?", "id": "Menghasilkan Output AC Tiga Fasa." },
    { "en": "Apa Itu Modulasi Lebar Pulsa?", "id": "PWM (Pulse-Width Modulation)." },
    { "en": "Apa Tujuan PWM (Pulse-Width Modulation) Di Inverter?", "id": "Mengontrol Tegangan Dan Frekuensi Output." },
    { "en": "Apa Itu Modulasi Lebar Pulsa Sinusoidal?", "id": "SPWM (Sinusoidal Pulse-Width Modulation)." },
    { "en": "Apa Gelombang Referensi Dalam SPWM?", "id": "Gelombang Sinus (Sinyal Modulasi)." },
    { "en": "Apa Gelombang Pembawa Dalam SPWM?", "id": "Gelombang Segitiga (Carrier)." },
    { "en": "Apa Itu Rasio Modulasi Amplitudo?", "id": "Mengontrol Amplitudo Tegangan Output." },
    { "en": "Apa Itu Rasio Modulasi Frekuensi?", "id": "Mengontrol Frekuensi Tegangan Output." },
    { "en": "Apa Itu Distorsi Harmonik Total?", "id": "THD (Total Harmonic Distortion)." },
    { "en": "Apa Itu Inverter Multi-level?", "id": "Menghasilkan Output Mendekati Sinus." },
    { "en": "Apa Keuntungan Inverter Multi-level?", "id": "THD (Total Harmonic Distortion) Rendah." },
    { "en": "Apa Kontroler Tegangan AC Satu Fasa?", "id": "Mengontrol Daya AC Ke Beban." },
    { "en": "Bagaimana Cara Kerja Kontroler Tegangan AC?", "id": "Dengan Kontrol Fasa (Phase Control)." },
    { "en": "Apa Itu Cycloconverter?", "id": "Konverter Frekuensi AC-AC Langsung." },
    { "en": "Apa Kelemahan Cycloconverter?", "id": "Frekuensi Output Terbatas, Harmonik Buruk." },
    { "en": "Apa Itu Saklar Statis?", "id": "Saklar Semikonduktor Tanpa Bagian Bergerak." },
    { "en": "Apa Itu Thyristor?", "id": "Saklar Semikonduktor Empat Lapis (PNPN)." },
    { "en": "Apa Tiga Terminal Thyristor?", "id": "Anoda, Katoda, Gerbang (Gate)." },
    { "en": "Bagaimana Cara Menghidupkan Thyristor?", "id": "Memberi Pulsa Positif Ke Gerbang." },
    { "en": "Bagaimana Cara Mematikan Thyristor?", "id": "Arus Anoda Turun Di Bawah Arus Tahan." },
    { "en": "Apa Itu Arus Latching?", "id": "Arus Minimum Agar Thyristor Tetap ON." },
    { "en": "Apa Itu Arus Holding?", "id": "Arus Minimum Menjaga Thyristor ON." },
    { "en": "Apa Itu GTO (Gate Turn-Off Thyristor)?", "id": "Thyristor Yang Bisa Dimatikan Gerbangnya." },
    { "en": "Apa Itu TRIAC (Triode for AC Switch)?", "id": "Dua Thyristor Anti-Paralel." },
    { "en": "Apa Fungsi TRIAC?", "id": "Mengontrol Daya AC Dua Arah." },
    { "en": "Apa Itu MOSFET Daya?", "id": "Saklar Terkendali Tegangan Kecepatan Tinggi." },
    { "en": "Apa Keuntungan MOSFET Daya?", "id": "Kecepatan Switching Sangat Cepat." },
    { "en": "Apa Itu IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Gabungan MOSFET Dan BJT." },
    { "en": "Apa Keuntungan IGBT?", "id": "Tegangan Tinggi, Arus Tinggi, Cepat." },
    { "en": "Apa Itu Kerugian Konduksi?", "id": "Disipasi Daya Saat Saklar ON." },
    { "en": "Apa Itu Kerugian Pensaklaran?", "id": "Disipasi Daya Saat Transisi ON-OFF." },
    { "en": "Kapan Kerugian Pensaklaran Dominan?", "id": "Pada Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu Heatsink (Pendingin)?", "id": "Membuang Panas Dari Semikonduktor." },
    { "en": "Apa Itu Resistansi Termal?", "id": "Hambatan Terhadap Aliran Panas." },
    { "en": "Apa Itu Rangkaian Snubber?", "id": "Melindungi Saklar Dari Lonjakan Tegangan." },
    { "en": "Apa Rating dV/dt?", "id": "Laju Kenaikan Tegangan Maksimum." },
    { "en": "Apa Rating dI/dt?", "id": "Laju Kenaikan Arus Maksimum." },
    { "en": "Apa Itu SOA (Safe Operating Area)?", "id": "Batas Operasi Aman Saklar." },
    { "en": "Apa Itu Soft Switching?", "id": "Teknik Mengurangi Kerugian Pensaklaran." },
    { "en": "Apa Itu Zero Voltage Switching (ZVS)?", "id": "Switching Saat Tegangan Melintasi Nol." },
    { "en": "Apa Itu Zero Current Switching (ZCS)?", "id": "Switching Saat Arus Melintasi Nol." },
    { "en": "Apa Itu Konverter Resonansi?", "id": "Menggunakan Sirkuit LC Untuk Soft Switching." },
    { "en": "Apa Itu Penggerak Gerbang (Gate Driver)?", "id": "Sirkuit Untuk Mengaktifkan MOSFET/IGBT." },
    { "en": "Apa Itu Isolasi Gerbang?", "id": "Memisahkan Sirkuit Kontrol Dari Daya." },
    { "en": "Apa Itu Waktu Mati (Dead Time)?", "id": "Penundaan Mencegah Hubung Singkat Jembatan." },
    { "en": "Apa Itu Shoot-through?", "id": "Kondisi Hubung Singkat Pada Kaki Jembatan." },
    { "en": "Apa Itu Sistem Loop Terbuka?", "id": "Sistem Kontrol Tanpa Umpan Balik." },
    { "en": "Apa Itu Sistem Loop Tertutup?", "id": "Sistem Kontrol Dengan Umpan Balik." },
    { "en": "Apa Itu Kontroler PID?", "id": "Proportional-Integral-Derivative Controller." },
    { "en": "Apa Itu SMPS (Switch-Mode Power Supply)?", "id": "Catu Daya Berbasis Pensaklaran." },
    { "en": "Apa Keuntungan SMPS?", "id": "Efisiensi Sangat Tinggi, Ukuran Kecil." },
    { "en": "Apa Itu Konverter Terisolasi?", "id": "Input Dan Output Terpisah Galvanik." },
    { "en": "Apa Konverter Terisolasi Paling Umum?", "id": "Flyback Converter." },
    { "en": "Apa Itu Koreksi Faktor Daya?", "id": "PFC (Power Factor Correction)." },
    { "en": "Apa Tujuan PFC (Power Factor Correction)?", "id": "Membuat Beban Terlihat Seperti Resistor." },
    { "en": "Apa Itu Active PFC?", "id": "PFC Menggunakan Konverter Boost." },
    { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Catu Daya Darurat." },
    { "en": "Apa Aplikasi Utama Elektronika Daya?", "id": "Penggerak Motor, Catu Daya, VFD." },
    { "en": "Apa Itu Penggerak Motor (Motor Drive)?", "id": "Sistem Untuk Mengontrol Kecepatan Torsi Motor." },
    { "en": "Apa Itu Penggerak Kecepatan Variabel?", "id": "VSD (Variable Speed Drive)." },
    { "en": "Apa Nama Lain VSD?", "id": "VFD (Variable Frequency Drive)." },
    { "en": "Apa Fungsi VFD (Variable Frequency Drive)?", "id": "Mengontrol Kecepatan Motor Induksi AC." },
    { "en": "Bagaimana VFD Mengontrol Kecepatan?", "id": "Mengubah Frekuensi Dan Tegangan." },
    { "en": "Apa Itu Kontrol V/Hz?", "id": "Menjaga Rasio Tegangan-Frekuensi Konstan." },
    { "en": "Apa Itu Pengereman Regeneratif?", "id": "Mengembalikan Energi Pengereman Ke Sumber." },
    { "en": "Apa Itu Pengereman Dinamis?", "id": "Membuang Energi Pengereman Sebagai Panas." },
    { "en": "Apa Itu Chopper Empat Kuadran?", "id": "Dapat Mengontrol Motor Dua Arah." },
    { "en": "Apa Itu Kuadran Operasi Motor?", "id": "Mode (Forward/Reverse, Motoring/Braking)." },
    { "en": "Apa Itu Sistem Tenaga Listrik Fleksibel?", "id": "FACTS (Flexible AC Transmission Systems)." },
    { "en": "Apa Tujuan FACTS (Flexible AC Transmission Systems)?", "id": "Meningkatkan Stabilitas Dan Kontrol Jaringan." },
    { "en": "Apa Itu Kompensator Sinkron Statis?", "id": "STATCOM (Static Synchronous Compensator)." },
    { "en": "Apa Fungsi STATCOM?", "id": "Menyediakan Atau Menyerap Daya Reaktif." },
    { "en": "Apa Itu Kompensator VAR Statis?", "id": "SVC (Static VAR Compensator)." },
    { "en": "Apa Itu Transmisi HVDC (High-Voltage Direct Current)?", "id": "Transmisi Daya Jarak Jauh DC." },
    { "en": "Apa Keuntungan HVDC?", "id": "Rugi-rugi Rendah, Stabilitas Meningkat." },
    { "en": "Apa Itu Stasiun Konverter?", "id": "Mengubah AC Ke DC Dan Sebaliknya." },
    { "en": "Apa Itu Pemanasan Induksi?", "id": "Memanaskan Logam Dengan Medan Magnet." },
    { "en": "Apa Itu Pengelasan Listrik?", "id": "Menggunakan Busur Listrik Untuk Menyambung Logam." },
    { "en": "Apa Itu Ballast Elektronik?", "id": "Driver Frekuensi Tinggi Untuk Lampu." },
    { "en": "Apa Itu Konverter Matriks?", "id": "Konverter AC-AC Langsung Tanpa DC Link." },
    { "en": "Apa Itu DC Link?", "id": "Kapasitor Penyimpan Energi Antar Konverter." },
    { "en": "Apa Itu Snubber Disipatif?", "id": "Snubber Yang Membuang Energi Panas." },
    { "en": "Apa Itu Snubber Non-Disipatif?", "id": "Snubber Yang Mengembalikan Energi." },
    { "en": "Apa Itu Dioda Pemulihan Cepat?", "id": "Dioda Dengan Waktu Pemulihan Cepat." },
    { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Dengan Jatuh Tegangan Rendah." },
    { "en": "Apa Itu Tegangan Tembus (Breakdown Voltage)?", "id": "Tegangan Maksimum Yang Dapat Ditahan Saklar." },
    { "en": "Apa Itu Arus Bocor (Leakage Current)?", "id": "Arus Saat Saklar Dalam Kondisi OFF." },
    { "en": "Apa Itu Jatuh Tegangan Maju?", "id": "Tegangan Saat Saklar ON." },
    { "en": "Apa Itu Waktu Nyala (Turn-on Time)?", "id": "Waktu Transisi Dari OFF Ke ON." },
    { "en": "Apa Itu Waktu Mati (Turn-off Time)?", "id": "Waktu Transisi Dari ON Ke OFF." },
    { "en": "Apa Itu Muatan Gerbang (Gate Charge)?", "id": "Muatan Untuk Menghidupkan MOSFET." },
    { "en": "Apa Itu Kapasitansi Miller?", "id": "Kapasitansi Antara Drain Dan Gate." },
    { "en": "Apa Itu Konverter Flyback?", "id": "Konverter Terisolasi Berbasis Buck-Boost." },
    { "en": "Apa Itu Konverter Forward?", "id": "Konverter Terisolasi Berbasis Buck." },
    { "en": "Apa Itu Konverter Jembatan Penuh?", "id": "Menggunakan Empat Saklar Sisi Primer." },
    { "en": "Apa Itu Konverter Setengah Jembatan?", "id": "Menggunakan Dua Saklar Sisi Primer." },
    { "en": "Apa Itu Konverter Push-Pull?", "id": "Menggunakan Trafo Center-Tap." },
    { "en": "Apa Itu Penyearah Sinkron?", "id": "Menggunakan MOSFET Sebagai Pengganti Dioda." },
    { "en": "Apa Keuntungan Penyearah Sinkron?", "id": "Efisiensi Sangat Tinggi." },
    { "en": "Apa Itu Kontrol Sisi Primer?", "id": "Umpan Balik Tanpa Optocoupler." },
    { "en": "Apa Itu Kontrol Sisi Sekunder?", "id": "Umpan Balik Menggunakan Optocoupler." },
    { "en": "Apa Itu Mode Arus (Current Mode) Control?", "id": "Loop Umpan Balik Mengontrol Arus Puncak." },
    { "en": "Apa Itu Mode Tegangan (Voltage Mode) Control?", "id": "Loop Umpan Balik Mengontrol Tegangan." },
    { "en": "Apa Itu Inverter Gelombang Persegi?", "id": "Inverter Paling Sederhana Dan Murah." },
    { "en": "Apa Itu Inverter Sinus Termodifikasi?", "id": "Output Mendekati Bentuk Sinus." },
    { "en": "Apa Itu Inverter Sinus Murni?", "id": "Output Gelombang Sinus Berkualitas Tinggi." },
    { "en": "Apa Itu Inverter Terhubung Jaringan (Grid-Tied)?", "id": "Inverter Yang Sinkron Dengan Jaringan PLN." },
    { "en": "Apa Itu Anti-Islanding?", "id": "Proteksi Mematikan Inverter Saat Jaringan Padam." },
    { "en": "Apa Itu MPPT (Maximum Power Point Tracking)?", "id": "Melacak Titik Daya Maksimum Panel Surya." },
    { "en": "Apa Itu Pengisi Daya Baterai?", "id": "Sirkuit Untuk Mengisi Ulang Baterai." },
    { "en": "Apa Itu Sistem Manajemen Baterai?", "id": "BMS (Battery Management System)." },
    { "en": "Apa Fungsi BMS (Battery Management System)?", "id": "Melindungi Dan Memonitor Sel Baterai." },
    { "en": "Apa Itu Penyeimbangan Sel (Cell Balancing)?", "id": "Menjaga Tegangan Sel Baterai Seragam." },
    { "en": "Apa Itu Penggerak Motor DC Tanpa Sikat?", "id": "Driver BLDC (Brushless DC)." },
    { "en": "Apa Itu Komutasi Elektronik?", "id": "Proses Pensaklaran Fasa Motor BLDC." },
    { "en": "Apa Itu Sensor Efek Hall?", "id": "Sensor Untuk Mendeteksi Posisi Rotor." },
    { "en": "Apa Itu Kontrol Tanpa Sensor (Sensorless)?", "id": "Mengontrol Motor Tanpa Sensor Posisi." },
    { "en": "Apa Itu GGL Balik (Back-EMF)?", "id": "Tegangan Terinduksi Di Belitan Motor." },
    { "en": "Apa Itu Kontrol Medan Terorientasi?", "id": "FOC (Field-Oriented Control)." },
    { "en": "Apa Fungsi FOC (Field-Oriented Control)?", "id": "Kontrol Torsi Motor AC Presisi." },
    { "en": "Apa Itu Transformasi Clarke?", "id": "Mengubah Vektor Tiga Fasa Ke Dua Fasa." },
    { "en": "Apa Itu Transformasi Park?", "id": "Mengubah Vektor Statis Ke Berputar." },
    { "en": "Apa Itu Modulasi Vektor Ruang?", "id": "SVM (Space Vector Modulation)." },
    { "en": "Apa Itu Inverter Tiga Tingkat?", "id": "Jenis Inverter Multi-level." },
    { "en": "Apa Itu Diode-Clamped Multilevel Inverter?", "id": "Inverter NPC (Neutral Point Clamped)." },
    { "en": "Apa Itu Flying Capacitor Multilevel Inverter?", "id": "Inverter Berbasis Kapasitor Terbang." },
    { "en": "Apa Itu Cascaded H-Bridge Inverter?", "id": "Inverter Berbasis Jembatan-H Bertingkat." },
    { "en": "Apa Itu Konverter Modular Multi-level?", "id": "MMC (Modular Multilevel Converter)." },
    { "en": "Di Mana MMC (Modular Multilevel Converter) Digunakan?", "id": "Aplikasi HVDC Dan STATCOM." },
    { "en": "Apa Itu Solid-State Transformer (SST)?", "id": "Transformator Berbasis Elektronika Daya." },
    { "en": "Apa Itu Daya Reaktif?", "id": "Daya Yang Disimpan Dan Dikembalikan." },
    { "en": "Apa Itu Daya Nyata (Aktif)?", "id": "Daya Aktual Yang Digunakan." },
    { "en": "Apa Itu Daya Semu?", "id": "Kombinasi Vektor Daya Nyata-Reaktif." },
    { "en": "Apa Itu Saklar Solid-State?", "id": "SSR (Solid-State Relay)." },
    { "en": "Apa Itu Thyristor Pemicu Cahaya?", "id": "LTT (Light-Triggered Thyristor)." },
    { "en": "Apa Itu IGCT (Integrated Gate-Commutated Thyristor)?", "id": "Thyristor Dengan Sirkuit Gate Terintegrasi." },
    { "en": "Apa Itu MCT (MOS-Controlled Thyristor)?", "id": "Thyristor Yang Dikontrol Gerbang MOS." },
    { "en": "Apa Itu Dioda SiC (Silicon Carbide)?", "id": "Dioda Berkinerja Sangat Tinggi." },
    { "en": "Apa Itu MOSFET GaN (Gallium Nitride)?", "id": "MOSFET Dengan Kecepatan Sangat Tinggi." },
    { "en": "Apa Itu Semikonduktor Celah Pita Lebar?", "id": "WBG (Wide-Bandgap) Semiconductor." },
    { "en": "Apa Keuntungan Semikonduktor WBG?", "id": "Suhu Tinggi, Frekuensi Tinggi, Efisiensi." },
    { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Dengan Fungsi Khusus." },
    { "en": "Apa Itu DSP (Digital Signal Processor)?", "id": "Prosesor Untuk Pemrosesan Sinyal." },
    { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "IC Digital Yang Dapat Dikonfigurasi." },
    { "en": "Apa Itu Power Module?", "id": "Beberapa Semikonduktor Daya Dalam Satu Paket." },
    { "en": "Apa Itu Intelligent Power Module (IPM)?", "id": "Power Module Dengan Proteksi Terintegrasi." },
    { "en": "Apa Itu Sistem-pada-Chip?", "id": "SoC (System-on-Chip)." },
    { "en": "Apa Itu EMI (Electromagnetic Interference)?", "id": "Gangguan Elektromagnetik." },
    { "en": "Apa Itu Filter EMI?", "id": "Menekan Gangguan Elektromagnetik." },
    { "en": "Apa Itu Konduksi Mode-Sama (Common-Mode)?", "id": "Derau Mengalir Searah Di Kedua Jalur." },
    { "en": "Apa Itu Konduksi Mode-Diferensial?", "id": "Derau Mengalir Berlawanan Arah." },
    { "en": "Apa Itu Kapasitor Y?", "id": "Kapasitor Untuk Menekan Derau Common-mode." },
    { "en": "Apa Itu Kapasitor X?", "id": "Kapasitor Untuk Menekan Derau Diferensial." },
    { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit)?", "id": "IC (Integrated Circuit)." },
    { "en": "Apa Itu Sirkuit Kontroler PWM?", "id": "Menghasilkan Sinyal PWM Untuk Konverter." },
    { "en": "Apa Itu Komparator?", "id": "Membandingkan Dua Tegangan." },
    { "en": "Apa Itu Penguat Kesalahan (Error Amplifier)?", "id": "Membandingkan Output Dengan Referensi." },
    { "en": "Apa Itu Referensi Tegangan?", "id": "Menghasilkan Tegangan Stabil." },
    { "en": "Apa Itu Osilator Gigi Gergaji?", "id": "Menghasilkan Gelombang Pembawa Segitiga." },
    { "en": "Apa Itu Rangkaian Soft-Start?", "id": "Membatasi Arus Awal Secara Perlahan." },
    { "en": "Apa Itu Proteksi Arus Berlebih?", "id": "OCP (Over-Current Protection)." },
    { "en": "Apa Itu Proteksi Tegangan Berlebih?", "id": "OVP (Over-Voltage Protection)." },
    { "en": "Apa Itu Proteksi Suhu Berlebih?", "id": "OTP (Over-Temperature Protection)." },
    { "en": "Apa Itu Proteksi Tegangan Kurang?", "id": "UVP (Under-Voltage Protection)." },
    { "en": "Apa Itu Pengunci Tegangan Kurang?", "id": "UVLO (Under-Voltage Lockout)." },
    { "en": "Apa Itu Beban Elektronik?", "id": "Perangkat Untuk Menguji Catu Daya." },
    { "en": "Apa Itu Faktor Puncak (Crest Factor)?", "id": "Rasio Nilai Puncak Per RMS." },
    { "en": "Apa Itu Faktor Daya?", "id": "Rasio Daya Nyata Per Daya Semu." },
    { "en": "Apa Itu Distorsi Lintas Batas (Crossover)?", "id": "Distorsi Saat Arus Melintasi Nol." },
    { "en": "Apa Itu Waktu Pemulihan Mundur Dioda?", "id": "Trr (Reverse Recovery Time)." },
    { "en": "Apa Itu Muatan Pemulihan Mundur?", "id": "Qrr (Reverse Recovery Charge)." },
    { "en": "Apa Itu Latch-up?", "id": "Kondisi Hubung Singkat Internal IC." },
    { "en": "Apa Itu Dioda Bodi?", "id": "Dioda Parasitik Internal MOSFET." },
    { "en": "Apa Itu Resistansi Keadaan-ON?", "id": "RDS(on) MOSFET." },
    { "en": "Apa Itu Tegangan Gerbang-Source Ambang?", "id": "VGS(th) MOSFET." },
    { "en": "Apa Itu Kapasitansi Input?", "id": "Ciss MOSFET." },
    { "en": "Apa Itu Kapasitansi Output?", "id": "Coss MOSFET." },
    { "en": "Apa Itu Kapasitansi Umpan Balik Mundur?", "id": "Crss MOSFET." },
    { "en": "Apa Itu Tegangan Saturasi Kolektor-Emitor?", "id": "VCE(sat) IGBT." },
    { "en": "Apa Itu Waktu Tunda Propagasi?", "id": "Waktu Sinyal Merambat Melalui Sirkuit." },
    { "en": "Apa Itu Konverter Kuadran-Tunggal?", "id": "Hanya Bekerja Di Satu Kuadran." },
    { "en": "Apa Itu Konverter Dua-Kuadran?", "id": "Bekerja Di Dua Kuadran." },
    { "en": "Apa Itu Konverter Empat-Kuadran?", "id": "Bekerja Di Empat Kuadran." },
    { "en": "Apa Itu Pensaklaran Sisi-Tinggi (High-Side)?", "id": "Saklar Terletak Antara Catu Daya-Beban." },
    { "en": "Apa Itu Pensaklaran Sisi-Rendah (Low-Side)?", "id": "Saklar Terletak Antara Beban-Ground." },
    { "en": "Apa Itu Catu Daya Pompa Muatan?", "id": "Menghasilkan Tegangan Gate Drive Lebih Tinggi." },
    { "en": "Apa Itu Efek Miller?", "id": "Peningkatan Kapasitansi Efektif Akibat Penguatan." },
    { "en": "Apa Itu Arus Magnetisasi?", "id": "Arus Untuk Membangkitkan Fluks Di Trafo." },
    { "en": "Apa Itu Induktansi Bocor?", "id": "Fluks Magnet Yang Tidak Terhubung." },
    { "en": "Apa Itu Trafo Arus?", "id": "CT (Current Transformer)." },
    { "en": "Apa Itu Trafo Potensial?", "id": "PT (Potential Transformer)." },
    { "en": "Apa Itu Analisis Keadaan Rata-rata?", "id": "Menyederhanakan Analisis Konverter Switching." },
    { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Output Per Input Domain Frekuensi." },
    { "en": "Apa Itu Bode Plot?", "id": "Grafik Penguatan Dan Fasa Vs Frekuensi." },
    { "en": "Apa Itu Margin Penguatan (Gain Margin)?", "id": "Ukuran Stabilitas Sistem." },
    { "en": "Apa Itu Margin Fasa (Phase Margin)?", "id": "Ukuran Stabilitas Sistem Lainnya." },
    { "en": "Apa Itu Frekuensi Crossover?", "id": "Frekuensi Saat Penguatan Loop Adalah 0 dB." },
    { "en": "Apa Itu Pole?", "id": "Frekuensi Yang Menyebabkan Penguatan Turun." },
    { "en": "Apa Itu Zero?", "id": "Frekuensi Yang Menyebabkan Penguatan Naik." },
    { "en": "Apa Itu Zero Sisi Kanan?", "id": "RHPZ (Right-Half Plane Zero)." },
    { "en": "Apa Masalah RHPZ (Right-Half Plane Zero)?", "id": "Membatasi Bandwidth Loop Kontrol." },
    { "en": "Konverter Mana Yang Memiliki RHPZ?", "id": "Boost Dan Buck-Boost." },
    { "en": "Apa Itu Jaringan Kompensasi?", "id": "Sirkuit Untuk Menstabilkan Loop Kontrol." },
    { "en": "Apa Itu Kompensator Tipe I?", "id": "Integrator." },
    { "en": "Apa Itu Kompensator Tipe II?", "id": "Satu Pole Dan Satu Zero." },
    { "en": "Apa Itu Kompensator Tipe III?", "id": "Dua Pole Dan Dua Zero." },
    { "en": "Apa Itu Kontrol Histeresis?", "id": "Pensaklaran Berdasarkan Batas Atas-Bawah." },
    { "en": "Apa Itu Kontrol Frekuensi Konstan?", "id": "Frekuensi Pensaklaran Selalu Tetap." },
    { "en": "Apa Itu Kontrol Frekuensi Variabel?", "id": "Frekuensi Berubah Sesuai Beban." },
    { "en": "Apa Itu Kontrol Mode Puncak Arus?", "id": "Mengontrol Arus Puncak Induktor." },
    { "en": "Apa Itu Slope Compensation?", "id": "Mencegah Sub-harmonic Oscillation." },
    { "en": "Apa Itu Kontrol Mode Lembah Arus?", "id": "Mengontrol Arus Minimum Induktor." },
    { "en": "Apa Itu Kontrol V-Kuadrat (V2 Control)?", "id": "Teknik Kontrol Mode Tegangan." },
    { "en": "Apa Itu Pemanasan Dielektrik?", "id": "Memanaskan Isolator Dengan Medan RF." },
    { "en": "Apa Itu Catu Daya Busur Plasma?", "id": "Menghasilkan Busur Plasma Untuk Pemotongan." },
    { "en": "Apa Itu Penggerak Piezoelektrik?", "id": "Menghasilkan Tegangan Tinggi Untuk Aktuator Piezo." },
    { "en": "Apa Itu Pengendap Elektrostatis?", "id": "ESP (Electrostatic Precipitator)." },
    { "en": "Apa Itu Generator Ozon?", "id": "Menghasilkan Ozon Dengan Pelepasan Korona." },
    { "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Ukuran Kualitas Tegangan Dan Arus." },
    { "en": "Apa Itu Harmonik?", "id": "Frekuensi Kelipatan Dari Frekuensi Dasar." },
    { "en": "Apa Itu Filter Pasif?", "id": "Filter Terbuat Dari R, L, C." },
    { "en": "Apa Itu Filter Aktif?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
    { "en": "Apa Itu Filter Harmonik Aktif?", "id": "Menghilangkan Harmonik Dengan Injeksi Arus." },
    { "en": "Apa Itu Kondisioner Daya?", "id": "Memperbaiki Masalah Kualitas Daya." },
    { "en": "Apa Itu Sag Tegangan?", "id": "Penurunan Tegangan Sesaat." },
    { "en": "Apa Itu Swell Tegangan?", "id": "Kenaikan Tegangan Sesaat." },
    { "en": "Apa Itu Transien?", "id": "Lonjakan Tegangan Sangat Cepat." },
    { "en": "Apa Itu Interupsi?", "id": "Hilangnya Tegangan Sepenuhnya." },
    { "en": "Apa Itu Flicker Tegangan?", "id": "Fluktuasi Amplitudo Tegangan." },
    { "en": "Apa Itu Ketidakseimbangan Tegangan?", "id": "Perbedaan Tegangan Antar Fasa." },
    { "en": "Apa Itu Jaringan Cerdas (Smart Grid)?", "id": "Jaringan Listrik Dengan Komunikasi Digital." },
    { "en": "Apa Itu Energi Terbarukan?", "id": "Energi Dari Sumber Alam (Surya, Angin)." },
    { "en": "Apa Itu Inverter Panel Surya?", "id": "Mengubah DC Dari Panel Surya Ke AC." },
    { "en": "Apa Itu Micro-inverter?", "id": "Satu Inverter Untuk Setiap Panel Surya." },
    { "en": "Apa Itu String Inverter?", "id": "Satu Inverter Untuk Rangkaian Panel." },
    { "en": "Apa Itu Inverter Sentral?", "id": "Inverter Besar Untuk Skala Utilitas." },
    { "en": "Apa Itu Konverter Turbin Angin?", "id": "Mengonversi Daya AC Variabel Ke Jaringan." },
    { "en": "Apa Itu Sistem Penyimpanan Energi Baterai?", "id": "BESS (Battery Energy Storage System)." },
    { "en": "Apa Itu Kendaraan Listrik?", "id": "EV (Electric Vehicle)." },
    { "en": "Apa Itu Pengisi Daya On-Board?", "id": "OBC (On-Board Charger)." },
    { "en": "Apa Itu Pengisian Cepat DC?", "id": "DC Fast Charging." },
    { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Mobil Listrik Mengirim Daya Ke Jaringan." },
    { "en": "Apa Itu Pencahayaan Solid-State?", "id": "SSL (Solid-State Lighting)." },
    { "en": "Apa Itu Driver LED?", "id": "Catu Daya Khusus Untuk LED." },
    { "en": "Apa Itu Peredupan (Dimming)?", "id": "Mengatur Kecerahan Cahaya." },
    { "en": "Apa Itu Peredupan PWM?", "id": "Mengatur Kecerahan Dengan Sinyal PWM." },
    { "en": "Apa Itu Peredupan CCR?", "id": "CCR (Constant Current Reduction)." },
    { "en": "Apa Itu Konverter SEPIC?", "id": "Single-Ended Primary-Inductor Converter." },
    { "en": "Apa Fungsi Konverter SEPIC?", "id": "Buck-Boost Non-inverting." },
    { "en": "Apa Itu Konverter Zeta?", "id": "Konverter Non-inverting Buck-Boost Lainnya." },
    { "en": "Apa Itu Konverter Cuk?", "id": "Konverter Inverting Dengan Ripple Rendah." },
    { "en": "Apa Itu Konverter Interleaved?", "id": "Beberapa Konverter Paralel Beda Fasa." },
    { "en": "Apa Keuntungan Konverter Interleaved?", "id": "Mengurangi Riak Arus Input/Output." },
    { "en": "Apa Itu Konverter Jembatan Geser Fasa?", "id": "PSFB (Phase-Shifted Full-Bridge)." },
    { "en": "Apa Keuntungan PSFB (Phase-Shifted Full-Bridge)?", "id": "Mencapai ZVS (Zero Voltage Switching) Pada Rentang Beban Luas." },
    { "en": "Apa Itu Jembatan Aktif (Active Clamp)?", "id": "Sirkuit Untuk Mereset Trafo Konverter." },
    { "en": "Apa Itu Konverter LLC?", "id": "Konverter Resonansi Efisiensi Sangat Tinggi." },
    { "en": "Apa Tiga Komponen Resonansi LLC?", "id": "Dua Induktor Dan Satu Kapasitor." },
    { "en": "Kapan Konverter LLC Sangat Efisien?", "id": "Di Dekat Frekuensi Resonansinya." },
    { "en": "Apa Itu Penyearah Jembatan Tak Terkendali?", "id": "Penyearah Dioda Tiga Fasa." },
    { "en": "Apa Itu Penyearah Vienna?", "id": "Penyearah PFC (Power Factor Correction) Tiga Fasa." },
    { "en": "Apa Itu Inverter Sumber Impedansi?", "id": "ZSI (Z-Source Inverter)." },
    { "en": "Apa Keuntungan ZSI (Z-Source Inverter)?", "id": "Dapat Menaikkan Tegangan (Boost)." },
    { "en": "Apa Itu Inverter Quasi-Z-Source?", "id": "qZSI (Quasi-Z-Source Inverter)." },
    { "en": "Apa Itu Elektronika Daya Modular?", "id": "Konverter Dibangun Dari Modul-modul Identik." },
    { "en": "Apa Itu Power Electronics Building Block?", "id": "PEBB (Power Electronics Building Block)." },
    { "en": "Apa Itu Isolasi Galvanik?", "id": "Pemisahan Listrik Antara Sirkuit." },
    { "en": "Mengapa Isolasi Galvanik Penting?", "id": "Untuk Keamanan Dan Penekanan Derau." },
    { "en": "Apa Komponen Untuk Isolasi?", "id": "Transformator Dan Optocoupler." },
    { "en": "Apa Itu Jarak Rambat (Creepage)?", "id": "Jarak Terpendek Sepanjang Permukaan Isolator." },
    { "en": "Apa Itu Jarak Bebas (Clearance)?", "id": "Jarak Terpendek Melalui Udara." },
    { "en": "Apa Itu Isolasi Dasar?", "id": "Isolasi Tunggal Untuk Proteksi Dasar." },
    { "en": "Apa Itu Isolasi Ganda?", "id": "Dua Lapis Isolasi Independen." },
    { "en": "Apa Itu Isolasi Diperkuat?", "id": "Isolasi Tunggal Setara Isolasi Ganda." },
    { "en": "Apa Itu Superkapasitor?", "id": "Komponen Penyimpan Energi Kapasitansi Tinggi." },
    { "en": "Apa Itu Penyimpanan Energi Magnetik Superkonduktor?", "id": "SMES (Superconducting Magnetic Energy Storage)." },
    { "en": "Apa Itu Roda Gila (Flywheel)?", "id": "Penyimpanan Energi Kinetik." },
    { "en": "Apa Itu Sel Bahan Bakar?", "id": "Mengubah Energi Kimia Menjadi Listrik." },
    { "en": "Apa Itu Termoelektrik Generator?", "id": "TEG (Thermoelectric Generator)." },
    { "en": "Apa Fungsi TEG (Thermoelectric Generator)?", "id": "Mengubah Panas Langsung Menjadi Listrik." },
    { "en": "Apa Itu Simulasi Elektromagnetik?", "id": "Menganalisis Medan Listrik Dan Magnet." },
    { "en": "Apa Itu Metode Elemen Hingga?", "id": "FEM (Finite Element Method)." },
    { "en": "Apa Itu Simulasi Termal?", "id": "Menganalisis Distribusi Panas." },
    { "en": "Apa Itu Fluidodinamika Komputasi?", "id": "CFD (Computational Fluid Dynamics)." },
    { "en": "Apa Itu Simulasi Level-Sirkuit?", "id": "Simulasi SPICE." },
    { "en": "Apa Itu Simulasi Level-Sistem?", "id": "Menganalisis Interaksi Antar Konverter." },
    { "en": "Apa Itu Hardware-in-the-Loop (HIL)?", "id": "Pengujian Kontroler Dengan Plant Tersimulasi." },
    { "en": "Apa Itu Stabilitas Sistem Tenaga?", "id": "Kemampuan Sistem Kembali Normal." },
    { "en": "Apa Itu Stabilitas Sudut Rotor?", "id": "Sinkronisasi Antar Generator." },
    { "en": "Apa Itu Stabilitas Tegangan?", "id": "Menjaga Tegangan Dalam Batas." },
    { "en": "Apa Itu Stabilitas Frekuensi?", "id": "Menjaga Frekuensi Dalam Batas." },
    { "en": "Apa Itu Inersia Sistem?", "id": "Kemampuan Melawan Perubahan Frekuensi." },
    { "en": "Apa Itu Respon Frekuensi Primer?", "id": "Kontrol Governor Generator." },
    { "en": "Apa Itu Respon Frekuensi Sekunder?", "id": "Kontrol Pembangkitan Otomatis (AGC)." },
    { "en": "Apa Itu Black Start?", "id": "Memulai Ulang Jaringan Tanpa Daya Eksternal." },
    { "en": "Apa Itu Islanding?", "id": "Bagian Jaringan Terisolasi Tapi Beroperasi." },
    { "en": "Apa Itu Beban Kritis?", "id": "Beban Yang Tidak Boleh Kehilangan Daya." },
    { "en": "Apa Itu Motor Sinkron?", "id": "Kecepatan Rotor Sinkron Dengan Frekuensi." },
    { "en": "Apa Itu Motor Asinkron?", "id": "Motor Induksi." },
    { "en": "Apa Itu Slip?", "id": "Perbedaan Kecepatan Antara Rotor-Stator." },
    { "en": "Apa Itu Motor Reluktansi?", "id": "Motor Yang Bekerja Berdasarkan Reluktansi." },
    { "en": "Apa Itu Motor Histeresis?", "id": "Motor Sinkron Menggunakan Kerugian Histeresis." },
    { "en": "Apa Itu Motor Universal?", "id": "Dapat Beroperasi Dengan Daya AC/DC." },
    { "en": "Apa Itu Motor Stepper?", "id": "Motor Yang Bergerak Dalam Langkah Diskret." },
    { "en": "Apa Itu Motor Servo?", "id": "Motor Dengan Kontrol Posisi Loop Tertutup." },
    { "en": "Apa Itu Aktuator Linear?", "id": "Perangkat Yang Menghasilkan Gerakan Lurus." },
    { "en": "Apa Itu Kontrol Skalar?", "id": "Kontrol V/Hz Untuk Motor." },
    { "en": "Apa Itu Kontrol Vektor?", "id": "Kontrol FOC (Field-Oriented Control) Untuk Motor." },
    { "en": "Apa Itu Estimator Kecepatan?", "id": "Memperkirakan Kecepatan Tanpa Sensor." },
    { "en": "Apa Itu Observer?", "id": "Model Matematis Untuk Mengestimasi Keadaan." },
    { "en": "Apa Itu Filter Kalman?", "id": "Filter Optimal Untuk Estimasi." },
    { "en": "Apa Itu Kontrol Prediktif?", "id": "Menggunakan Model Untuk Memprediksi Perilaku." },
    { "en": "Apa Itu Kontrol Logika Fuzzy?", "id": "Kontrol Berbasis Aturan Logika Fuzzy." },
    { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "ANN (Artificial Neural Network)." },
    { "en": "Apa Itu Kontrol Adaptif?", "id": "Kontroler Yang Menyesuaikan Parameternya." },
    { "en": "Apa Itu Kontrol Kuat (Robust Control)?", "id": "Tahan Terhadap Ketidakpastian Model." },
    { "en": "Apa Itu Keandalan (Reliability)?", "id": "Probabilitas Perangkat Bekerja Sesuai Harapan." },
    { "en": "Apa Itu MTBF (Mean Time Between Failures)?", "id": "Waktu Rata-rata Antar Kegagalan." },
    { "en": "Apa Itu MTTF (Mean Time To Failure)?", "id": "Waktu Rata-rata Hingga Kegagalan." },
    { "en": "Apa Itu MTTR (Mean Time To Repair)?", "id": "Waktu Rata-rata Untuk Perbaikan." },
    { "en": "Apa Itu Analisis FMEA (Failure Mode and Effects Analysis)?", "id": "Analisis Mode Dan Efek Kegagalan." },
    { "en": "Apa Itu Analisis Pohon Kesalahan?", "id": "FTA (Fault Tree Analysis)." },
    { "en": "Apa Itu Redundansi?", "id": "Menyediakan Komponen Cadangan." },
    { "en": "Apa Itu Redundansi N+1?", "id": "Satu Unit Cadangan Untuk N Unit." },
    { "en": "Apa Itu Derating Komponen?", "id": "Mengoperasikan Komponen Di Bawah Rating." },
    { "en": "Apa Itu Tegangan Tahan Impuls?", "id": "BIL (Basic Insulation Level)." },
    { "en": "Apa Itu Koordinasi Isolasi?", "id": "Menyesuaikan Kekuatan Isolasi Dengan Tegangan." },
    { "en": "Apa Itu Pembumian (Grounding)?", "id": "Koneksi Konduktif Ke Bumi." },
    { "en": "Apa Itu Ikatan (Bonding)?", "id": "Menghubungkan Semua Bagian Logam." },
    { "en": "Apa Itu Proteksi Petir?", "id": "Melindungi Sistem Dari Sambaran Petir." },
    { "en": "Apa Itu Gelombang Berjalan?", "id": "Transien Yang Merambat Di Jalur." },
    { "en": "Apa Itu Impedansi Gelombang?", "id": "Rasio Tegangan-Arus Gelombang Berjalan." },
    { "en": "Apa Itu Lattice Diagram?", "id": "Menganalisis Pantulan Gelombang Berjalan." },
    { "en": "Apa Itu Gardu Induk?", "id": "Fasilitas Transformasi Dan Switching." },
    { "en": "Apa Itu Switchgear?", "id": "Peralatan Switching, Proteksi, Kontrol." },
    { "en": "Apa Itu Busbar?", "id": "Konduktor Umum Untuk Distribusi Daya." },
    { "en": "Apa Itu Disconnector?", "id": "Saklar Isolasi Tanpa Kemampuan Pemutusan Beban." },
    { "en": "Apa Itu Pemutus Sirkuit?", "id": "Saklar Yang Dapat Memutus Arus Gangguan." },
    { "en": "Apa Itu Relai Proteksi?", "id": "Mendeteksi Gangguan Dan Memberi Perintah Trip." },
    { "en": "Apa Itu Proteksi Diferensial?", "id": "Membandingkan Arus Masuk Dan Keluar." },
    { "en": "Apa Itu Proteksi Jarak?", "id": "Mengukur Impedansi Untuk Menentukan Lokasi Gangguan." },
    { "en": "Apa Itu Proteksi Arus Lebih?", "id": "Bekerja Saat Arus Melebihi Batas." },
    { "en": "Apa Itu Proteksi Cadangan (Backup)?", "id": "Proteksi Yang Bekerja Jika Primer Gagal." },
    { "en": "Apa Itu Skema Proteksi?", "id": "Kombinasi Relai Untuk Melindungi Peralatan." },
    { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan Dan Kontrol Jarak Jauh." },
    { "en": "Apa Itu RTU (Remote Terminal Unit)?", "id": "Perangkat Di Lapangan Yang Mengumpulkan Data." },
    { "en": "Apa Itu HMI (Human-Machine Interface)?", "id": "Antarmuka Grafis Untuk Operator." },
    { "en": "Apa Itu Efisiensi Konverter?", "id": "Rasio Daya Output Per Daya Input." },
    { "en": "Apa Itu Kurva Efisiensi?", "id": "Grafik Efisiensi Vs Beban." },
    { "en": "Apa Itu Disipasi Daya?", "id": "Energi Yang Hilang Sebagai Panas." },
    { "en": "Apa Itu Manajemen Termal?", "id": "Mengelola Panas Yang Dihasilkan." },
    { "en": "Apa Itu Arus RMS (Root Mean Square)?", "id": "Nilai Efektif Arus AC." },
    { "en": "Apa Itu Arus Rata-rata?", "id": "Nilai Rata-rata Arus DC." },
    { "en": "Apa Itu Elektrolit Kapasitor?", "id": "Bahan Konduktif Ionik Dalam Kapasitor." },
    { "en": "Apa Itu ESR (Equivalent Series Resistance)?", "id": "Resistansi Seri Internal Kapasitor." },
    { "en": "Apa Itu ESL (Equivalent Series Inductance)?", "id": "Induktansi Seri Internal Kapasitor." },
    { "en": "Apa Itu Umur Kapasitor Elektrolit?", "id": "Menurun Seiring Suhu Dan Tegangan." },
    { "en": "Apa Itu Daur Ulang Elektronika Daya?", "id": "Mengambil Kembali Material Berharga." },
    { "en": "Apa Itu Keandalan Semikonduktor?", "id": "Probabilitas Berfungsi Tanpa Gagal." },
    { "en": "Apa Itu Migrasi Elektrokimia?", "id": "Pertumbuhan Filamen Logam Akibat Kelembaban." },
    { "en": "Apa Itu Siklus Termal?", "id": "Perubahan Suhu Berulang." },
    { "en": "Apa Itu Kegagalan Akibat Kelelahan (Fatigue)?", "id": "Kegagalan Akibat Stres Berulang." },
    { "en": "Apa Itu Wire Bond Lift-off?", "id": "Kegagalan Sambungan Kawat Akibat Siklus Termal." },
    { "en": "Apa Itu Solder Joint Fatigue?", "id": "Kelelahan Sambungan Solder." },
    { "en": "Apa Itu Whiskering?", "id": "Pertumbuhan Filamen Timah." },
    { "en": "Apa Itu Kepadatan Daya?", "id": "Daya Output Per Satuan Volume." },
    { "en": "Apa Tren Kepadatan Daya?", "id": "Semakin Meningkat Seiring Waktu." },
    { "en": "Apa Itu Integrasi?", "id": "Menggabungkan Banyak Fungsi Dalam Satu Chip." },
    { "en": "Apa Itu Sistem-dalam-Paket?", "id": "SiP (System-in-Package)." },
    { "en": "Apa Itu Paket Tiga Dimensi?", "id": "3D Packaging." },
    { "en": "Apa Itu Pendinginan Cair?", "id": "Menggunakan Cairan Untuk Mendinginkan Komponen." },
    { "en": "Apa Itu Pipa Panas (Heat Pipe)?", "id": "Perangkat Transfer Panas Pasif." },
    { "en": "Apa Itu Ruang Uap (Vapor Chamber)?", "id": "Penyebar Panas Dua Dimensi." },
    { "en": "Apa Itu Material Antarmuka Termal?", "id": "TIM (Thermal Interface Material)." },
    { "en": "Apa Fungsi TIM (Thermal Interface Material)?", "id": "Mengisi Celah Udara Antara Permukaan." },
    { "en": "Apa Itu Kendaraan Listrik Hibrida?", "id": "HEV (Hybrid Electric Vehicle)." },
    { "en": "Apa Itu Kendaraan Listrik Hibrida Plug-in?", "id": "PHEV (Plug-in Hybrid Electric Vehicle)." },
    { "en": "Apa Itu Kendaraan Listrik Baterai?", "id": "BEV (Battery Electric Vehicle)." },
    { "en": "Apa Itu Kendaraan Listrik Sel Bahan Bakar?", "id": "FCEV (Fuel Cell Electric Vehicle)." },
    { "en": "Apa Itu Konverter DC-DC Dua Arah?", "id": "Dapat Mengalirkan Daya Dua Arah." },
    { "en": "Apa Itu Arsitektur 48V?", "id": "Sistem Listrik Otomotif Hibrida Ringan." },
    { "en": "Apa Itu Inverter Traksi?", "id": "Inverter Yang Menggerakkan Motor Utama EV." },
    { "en": "Apa Itu Papan Distribusi Daya?", "id": "PDB (Power Distribution Board)." },
    { "en": "Apa Itu Sistem Manajemen Termal Baterai?", "id": "BTMS (Battery Thermal Management System)." },
    { "en": "Apa Itu State of Charge (SoC)?", "id": "Tingkat Pengisian Baterai." },
    { "en": "Apa Itu State of Health (SoH)?", "id": "Kondisi Kesehatan Baterai." },
    { "en": "Apa Itu Depth of Discharge (DoD)?", "id": "Kedalaman Pelepasan Muatan Baterai." },
    { "en": "Apa Itu Siklus Hidup Baterai?", "id": "Jumlah Siklus Pengisian-Pengosongan." },
    { "en": "Apa Itu Pesawat Lebih Listrik?", "id": "MEA (More Electric Aircraft)." },
    { "en": "Apa Itu Propulsi Listrik Kapal?", "id": "Sistem Penggerak Kapal Berbasis Listrik." },
    { "en": "Apa Itu Jaringan DC Kapal?", "id": "Menggunakan Distribusi DC Di Kapal." },
    { "en": "Apa Itu Kereta Listrik?", "id": "Menggunakan Daya Listrik Dari Jaringan Atas." },
    { "en": "Apa Itu Pantograf?", "id": "Kontak Pengambil Daya Dari Jaringan Atas." },
    { "en": "Apa Itu Rel Ketiga?", "id": "Sumber Daya Listrik Di Samping Rel." },
    { "en": "Apa Itu Pusat Data (Data Center)?", "id": "Fasilitas Komputasi Skala Besar." },
    { "en": "Apa Itu Efektivitas Penggunaan Daya?", "id": "PUE (Power Usage Effectiveness)." },
    { "en": "Apa Itu Catu Daya Tingkat-Papan?", "id": "Board-Level Power Supply." },
    { "en": "Apa Itu Titik Beban (Point-of-Load)?", "id": "POL (Point-of-Load) Converter." },
    { "en": "Apa Itu Arsitektur Daya Terdistribusi?", "id": "DPA (Distributed Power Architecture)." },
    { "en": "Apa Itu Bus Menengah?", "id": "Intermediate Bus Architecture (IBA)." },
    { "en": "Apa Itu Modul Regulator Tegangan?", "id": "VRM (Voltage Regulator Module)." },
    { "en": "Apa Itu Respon Transien Beban?", "id": "Bagaimana Output Merespon Perubahan Beban." },
    { "en": "Apa Itu Droop Tegangan?", "id": "Penurunan Tegangan Saat Beban Meningkat." },
    { "en": "Apa Itu Pembagian Arus (Current Sharing)?", "id": "Membagi Arus Antar Konverter Paralel." },
    { "en": "Apa Itu Hot Swap?", "id": "Memasang/Melepas Papan Saat Sistem Hidup." },
    { "en": "Apa Itu Or-ing?", "id": "Menggabungkan Output Catu Daya Redundan." },
    { "en": "Apa Itu Sistem AC Tiga Fasa?", "id": "Menggunakan Tiga Gelombang AC Terpisah." },
    { "en": "Apa Itu Hubungan Bintang (Wye)?", "id": "Konfigurasi Tiga Fasa Dengan Titik Netral." },
    { "en": "Apa Itu Hubungan Delta?", "id": "Konfigurasi Tiga Fasa Tanpa Netral." },
    { "en": "Apa Itu Tegangan Fasa-ke-Netral?", "id": "Tegangan Fasa." },
    { "en": "Apa Itu Tegangan Fasa-ke-Fasa?", "id": "Tegangan Jalur (Line Voltage)." },
    { "en": "Apa Itu Urutan Fasa?", "id": "Urutan Puncak Tegangan (ABC/ACB)." },
    { "en": "Apa Itu Beban Seimbang?", "id": "Impedansi Sama Di Setiap Fasa." },
    { "en": "Apa Itu Beban Tidak Seimbang?", "id": "Impedansi Berbeda Antar Fasa." },
    { "en": "Apa Itu Komponen Simetris?", "id": "Metode Analisis Sistem Tidak Seimbang." },
    { "en": "Apa Itu Urutan Positif?", "id": "Komponen Urutan Fasa Normal." },
    { "en": "Apa Itu Urutan Negatif?", "id": "Komponen Urutan Fasa Terbalik." },
    { "en": "Apa Itu Urutan Nol?", "id": "Komponen Yang Sefasa." },
    { "en": "Apa Efek Urutan Negatif?", "id": "Pemanasan Berlebih Pada Motor." },
    { "en": "Apa Efek Urutan Nol?", "id": "Arus Mengalir Di Netral." },
    { "en": "Apa Itu Grounding Solid?", "id": "Netral Terhubung Langsung Ke Tanah." },
    { "en": "Apa Itu Grounding Resistansi?", "id": "Netral Terhubung Ke Tanah Lewat Resistor." },
    { "en": "Apa Itu Grounding Reaktansi?", "id": "Netral Terhubung Ke Tanah Lewat Reaktor." },
    { "en": "Apa Itu Sistem Tanpa Ground?", "id": "Sistem Yang Tidak Terhubung Ke Tanah." },
    { "en": "Apa Itu Gangguan Fasa-ke-Tanah?", "id": "Jenis Gangguan Paling Umum." },
    { "en": "Apa Itu Gangguan Fasa-ke-Fasa?", "id": "Hubung Singkat Antara Dua Fasa." },
    { "en": "Apa Itu Gangguan Tiga Fasa?", "id": "Hubung Singkat Antara Tiga Fasa." },
    { "en": "Apa Itu Studi Aliran Daya?", "id": "Menganalisis Aliran Daya Dalam Jaringan." },
    { "en": "Apa Itu Studi Hubung Singkat?", "id": "Menghitung Arus Selama Gangguan." },
    { "en": "Apa Itu Studi Stabilitas?", "id": "Menganalisis Perilaku Dinamis Jaringan." },
    { "en": "Apa Itu Model Per-Unit?", "id": "Normalisasi Kuantitas Listrik." },
    { "en": "Apa Itu Bus Slack?", "id": "Bus Referensi Dalam Analisis Aliran Daya." },
    { "en": "Apa Itu Bus PV?", "id": "Bus Generator (Daya Nyata & Tegangan)." },
    { "en": "Apa Itu Bus PQ?", "id": "Bus Beban (Daya Nyata & Reaktif)." },
    { "en": "Apa Itu Matriks Admitansi Bus?", "id": "Y-Bus." },
    { "en": "Apa Itu Matriks Impedansi Bus?", "id": "Z-Bus." },
    { "en": "Apa Itu Metode Newton-Raphson?", "id": "Metode Iteratif Untuk Analisis Aliran Daya." },
    { "en": "Apa Itu Metode Gauss-Seidel?", "id": "Metode Iteratif Lain Untuk Aliran Daya." },
    { "en": "Apa Itu Aliran Daya DC?", "id": "Aproksimasi Linier Dari Aliran Daya AC." },
    { "en": "Apa Itu Dispatch Ekonomi?", "id": "Menjadwalkan Pembangkit Dengan Biaya Terendah." },
    { "en": "Apa Itu Kurva Biaya Inkremental?", "id": "Biaya Untuk Menghasilkan Megawatt Tambahan." },
    { "en": "Apa Itu Rugi-rugi Jaringan?", "id": "Daya Yang Hilang Dalam Jalur Transmisi." },
    { "en": "Apa Itu Faktor Penalti?", "id": "Memperhitungkan Rugi-rugi Dalam Dispatch." },
    { "en": "Apa Itu Unit Commitment?", "id": "Menentukan Generator Mana Yang Harus Beroperasi." },
    { "en": "Apa Itu Cadangan Berputar (Spinning Reserve)?", "id": "Kapasitas Pembangkit Online Cadangan." },
    { "en": "Apa Itu Cadangan Tidak Berputar?", "id": "Kapasitas Pembangkit Offline Siap Cepat." },
    { "en": "Apa Itu Respon Kebutuhan (Demand Response)?", "id": "Konsumen Mengubah Pemakaian Merespon Sinyal." },
    { "en": "Apa Itu Pasar Listrik?", "id": "Mekanisme Jual Beli Energi Listrik." },
    { "en": "Apa Itu Pasar Energi?", "id": "Perdagangan Energi (MWh)." },
    { "en": "Apa Itu Pasar Kapasitas?", "id": "Perdagangan Kapasitas Pembangkitan (MW)." },
    { "en": "Apa Itu Layanan Ansilari?", "id": "Layanan Pendukung Keandalan Jaringan." },
    { "en": "Contoh Layanan Ansilari?", "id": "Regulasi Frekuensi, Dukungan Tegangan." },
    { "en": "Apa Itu Operator Sistem Independen?", "id": "ISO (Independent System Operator)." },
    { "en": "Apa Fungsi ISO (Independent System Operator)?", "id": "Mengelola Jaringan Transmisi Secara Independen." },
    { "en": "Apa Itu Kestabilan Sinyal Kecil?", "id": "Stabilitas Sistem Terhadap Gangguan Kecil." },
    { "en": "Apa Itu Osilasi Daya?", "id": "Ayunan Daya Antar Generator." },
    { "en": "Apa Itu Peredam Osilasi Daya?", "id": "PSS (Power System Stabilizer)." },
    { "en": "Apa Itu Kestabilan Transien?", "id": "Stabilitas Sistem Terhadap Gangguan Besar." },
    { "en": "Apa Itu Waktu Pemutusan Kritis?", "id": "Waktu Maksimum Gangguan Boleh Terjadi." },
    { "en": "Apa Itu Metode Sudut Sama?", "id": "Analisis Grafis Stabilitas Transien." },
    { "en": "Apa Itu Mesin Sinkron?", "id": "Generator Atau Motor Sinkron." },
    { "en": "Apa Itu Model Klasik Generator?", "id": "Model Sederhana Untuk Analisis Stabilitas." },
    { "en": "Apa Itu Persamaan Ayunan (Swing Equation)?", "id": "Mendeskripsikan Dinamika Rotor Generator." },
    { "en": "Apa Itu Mesin Kutub Menonjol (Salient Pole)?", "id": "Rotor Dengan Kutub Yang Menonjol." },
    { "en": "Apa Itu Mesin Kutub Silindris?", "id": "Rotor Dengan Permukaan Halus." },
    { "en": "Apa Itu Eksitasi?", "id": "Menyediakan Arus Medan DC Ke Generator." },
    { "en": "Apa Itu Sistem Eksitasi?", "id": "Sistem Yang Mengontrol Arus Medan." },
    { "en": "Apa Itu Regulator Tegangan Otomatis?", "id": "AVR (Automatic Voltage Regulator)." },
    { "en": "Apa Itu Turbin Uap?", "id": "Mengubah Energi Panas Uap Menjadi Rotasi." },
    { "en": "Apa Itu Turbin Gas?", "id": "Menggunakan Gas Panas Untuk Memutar Turbin." },
    { "en": "Apa Itu Turbin Air?", "id": "Menggunakan Aliran Air Untuk Memutar Turbin." },
    { "en": "Apa Itu Governor?", "id": "Mengontrol Kecepatan Putaran Turbin." },
    { "en": "Apa Itu Karakteristik Droop?", "id": "Hubungan Antara Kecepatan Dan Daya." },
    { "en": "Apa Itu Kontrol Isokron?", "id": "Menjaga Kecepatan Konstan Terlepas Beban." },
    { "en": "Apa Itu Desain Berbasis Keandalan?", "id": "Mempertimbangkan Probabilitas Kegagalan." },
    { "en": "Apa Itu Analisis Probabilistik?", "id": "Menggunakan Probabilitas Dalam Analisis." },
    { "en": "Apa Itu Waktu-ke-Pulih (Time-to-Restore)?", "id": "Waktu Untuk Memulihkan Sistem." },
    { "en": "Apa Itu Pemeliharaan Berbasis Kondisi?", "id": "CBM (Condition-Based Maintenance)." },
    { "en": "Apa Itu Pemantauan Kondisi Online?", "id": "Memantau Peralatan Secara Real-time." },
    { "en": "Apa Itu Analisis Gas Terlarut?", "id": "DGA (Dissolved Gas Analysis)." },
    { "en": "Untuk Apa DGA (Dissolved Gas Analysis)?", "id": "Mendiagnosis Kesehatan Transformator." },
    { "en": "Apa Itu Pelepasan Sebagian (Partial Discharge)?", "id": "Pelepasan Listrik Kecil Di Isolasi." },
    { "en": "Apa Itu Pencitraan Termal?", "id": "Mendeteksi Titik Panas Dengan Inframerah." },
    { "en": "Apa Itu Analisis Getaran?", "id": "Mendeteksi Masalah Mekanis Mesin." },
    { "en": "Apa Itu Penuaan Isolasi?", "id": "Degradasi Material Isolasi Seiring Waktu." },
    { "en": "Apa Itu Model Umur Arrhenius?", "id": "Menghubungkan Umur Dengan Suhu." },
    { "en": "Apa Itu Pengujian Tegangan Tahan?", "id": "Withstand Voltage Test." },
    { "en": "Apa Itu Pengujian Impuls?", "id": "Menguji Peralatan Dengan Gelombang Petir." },
    { "en": "Apa Itu Pengujian Faktor Daya Isolasi?", "id": "Tan Delta Test." },
    { "en": "Apa Itu Kapasitor Kopling?", "id": "Digunakan Untuk Mengukur Sinyal Di Jalur HV." },
    { "en": "Apa Itu Sensor Serat Optik?", "id": "Sensor Yang Kebal Terhadap EMI." },
    { "en": "Apa Itu Unit Pengukuran Fasor?", "id": "PMU (Phasor Measurement Unit)." },
    { "en": "Apa Fungsi PMU (Phasor Measurement Unit)?", "id": "Mengukur Fasor Tegangan/Arus Sinkron." },
    { "en": "Apa Itu Jaringan Area Luas Sinkrofasor?", "id": "WAMS (Wide Area Measurement Systems)." },
    { "en": "Apa Itu Keterkaitan (Interoperability)?", "id": "Kemampuan Sistem Berbeda Bekerja Sama." },
    { "en": "Apa Itu Standar IEC 61850?", "id": "Standar Komunikasi Gardu Induk." },
    { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Melindungi Sistem Dari Serangan Siber." },
    { "en": "Apa Itu Perangkat Elektronik Cerdas?", "id": "IED (Intelligent Electronic Device)." },
    { "en": "Apa Itu Relai Digital?", "id": "Relai Berbasis Mikroprosesor." },
    { "en": "Apa Itu Penggabung Unit (Merging Unit)?", "id": "Mengubah Sinyal Analog CT/PT Ke Digital." },
    { "en": "Apa Itu Bus Proses?", "id": "Jaringan Komunikasi Dalam Gardu Induk." },
    { "en": "Apa Itu Bus Stasiun?", "id": "Menghubungkan IED Ke Level Stasiun." },
    { "en": "Apa Itu Gerbang (Gateway)?", "id": "Menghubungkan Jaringan Protokol Berbeda." },
    { "en": "Apa Itu Elektronika Daya Tegangan Menengah?", "id": "MVPE (Medium-Voltage Power Electronics)." },
    { "en": "Apa Itu Konverter Sumber Arus?", "id": "CSI (Current-Source Converter)." },
    { "en": "Apa Itu Konverter Sumber Tegangan?", "id": "VSC (Voltage-Source Converter)." },
    { "en": "Apa Itu Modulasi Vektor Ruang?", "id": "SVM (Space Vector Modulation)." },
    { "en": "Apa Itu Modulasi Harmonik Selektif?", "id": "SHE (Selective Harmonic Elimination)." },
    { "en": "Apa Itu Kontrol Prediktif Model?", "id": "MPC (Model Predictive Control)." },
    { "en": "Apa Itu Estimasi Keadaan?", "id": "Memperkirakan Kondisi Internal Sistem." },
    { "en": "Apa Itu Filter Kalman Diperluas?", "id": "EKF (Extended Kalman Filter)." },
    { "en": "Apa Itu Filter Kalman Tanpa Aroma?", "id": "UKF (Unscented Kalman Filter)." },
    { "en": "Apa Itu Pengamat Mode Geser?", "id": "Sliding Mode Observer." },
    { "en": "Apa Itu Identifikasi Sistem?", "id": "Membangun Model Matematis Dari Data." },
    { "en": "Apa Itu Ruang Keadaan (State-Space)?", "id": "Representasi Sistem Dengan Variabel Keadaan." },
    { "en": "Apa Itu Keterkendalian (Controllability)?", "id": "Kemampuan Mengarahkan Keadaan Sistem." },
    { "en": "Apa Itu Keteramatan (Observability)?", "id": "Kemampuan Memperkirakan Keadaan Dari Output." },
    { "en": "Apa Itu Penempatan Pole?", "id": "Teknik Desain Kontroler." },
    { "en": "Apa Itu Regulator Kuadratik Linier?", "id": "LQR (Linear-Quadratic Regulator)." },
    { "en": "Apa Itu Kontrol Optimal?", "id": "Menemukan Sinyal Kontrol Yang Terbaik." },
    { "en": "Apa Itu Sistem Non-linear?", "id": "Sistem Yang Tidak Memenuhi Superposisi." },
    { "en": "Apa Itu Metode Lyapunov?", "id": "Menganalisis Stabilitas Sistem Non-linear." },
    { "en": "Apa Itu Umpan Balik Linearisasi?", "id": "Teknik Kontrol Non-linear." },
    { "en": "Apa Itu Kontrol Mode Geser?", "id": "SMC (Sliding Mode Control)." },
    { "en": "Apa Itu Sistem Hibrida?", "id": "Sistem Dengan Dinamika Kontinu Dan Diskrit." },
    { "en": "Apa Itu Jaringan Petri?", "id": "Model Grafis Untuk Sistem Diskrit." },
    { "en": "Apa Itu Otomata Hingga?", "id": "Model Matematis Komputasi." },
    { "en": "Apa Itu Sistem Waktu-Nyata (Real-Time)?", "id": "Sistem Dengan Batasan Waktu Ketat." },
    { "en": "Apa Itu Penjadwalan?", "id": "Mengatur Urutan Eksekusi Tugas." },
    { "en": "Apa Itu Inversi Prioritas?", "id": "Tugas Prioritas Rendah Menghalangi Tinggi." },
    { "en": "Apa Itu Mutex?", "id": "Mekanisme Untuk Akses Eksklusif." },
    { "en": "Apa Itu Semaphore?", "id": "Variabel Untuk Mengontrol Akses Sumber Daya." },
    { "en": "Apa Itu Deadlock?", "id": "Dua Tugas Atau Lebih Saling Menunggu." },
    { "en": "Apa Itu Sistem Operasi Waktu Nyata?", "id": "RTOS (Real-Time Operating System)." },
    { "en": "Apa Itu Komputasi Tertanam?", "id": "Komputer Khusus Dalam Sistem Lebih Besar." },
    { "en": "Apa Itu Konverter Analog-ke-Digital?", "id": "ADC (Analog-to-Digital Converter)." },
    { "en": "Apa Itu Konverter Digital-ke-Analog?", "id": "DAC (Digital-to-Analog Converter)." },
    { "en": "Apa Itu Pengkondisian Sinyal?", "id": "Mempersiapkan Sinyal Untuk Pemrosesan." },
    { "en": "Apa Itu Aliasing?", "id": "Distorsi Akibat Sampling Terlalu Lambat." },
    { "en": "Apa Itu Filter Anti-Aliasing?", "id": "Filter Low-pass Sebelum Sampling." },
    { "en": "Apa Itu Kuantisasi?", "id": "Pembulatan Nilai Analog Ke Level Digital." },
    { "en": "Apa Itu Kesalahan Kuantisasi?", "id": "Error Akibat Proses Kuantisasi." },
    { "en": "Apa Itu Resolusi?", "id": "Jumlah Bit Dalam Representasi Digital." },
    { "en": "Apa Itu Laju Sampling?", "id": "Seberapa Sering Sinyal Diukur." },
    { "en": "Apa Itu Teorema Nyquist?", "id": "Laju Sampling Minimal Dua Kali." },
    { "en": "Apa Itu Penahanan Orde Nol?", "id": "ZOH (Zero-Order Hold)." },
    { "en": "Apa Itu Filter Rekonstruksi?", "id": "Memuluskan Output Dari DAC." },
    { "en": "Apa Itu Pemrosesan Sinyal Digital?", "id": "DSP (Digital Signal Processing)." },
    { "en": "Apa Itu Transformasi Fourier Cepat?", "id": "FFT (Fast Fourier Transform)." },
    { "en": "Apa Itu Filter FIR (Finite Impulse Response)?", "id": "Filter Digital Tanpa Umpan Balik." },
    { "en": "Apa Itu Filter IIR (Infinite Impulse Response)?", "id": "Filter Digital Dengan Umpan Balik." },
    { "en": "Apa Itu Efek Jendela (Windowing)?", "id": "Mengurangi Kebocoran Spektral." },
    { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Untuk Otomasi Industri." },
    { "en": "Apa Itu Logika Tangga (Ladder Logic)?", "id": "Bahasa Pemrograman Grafis Untuk PLC." },
    { "en": "Apa Itu I/O (Input/Output) Diskrit?", "id": "Sinyal On/Off." },
    { "en": "Apa Itu I/O (Input/Output) Analog?", "id": "Sinyal Dengan Nilai Kontinu." },
    { "en": "Apa Itu Siklus Pindai (Scan Cycle)?", "id": "Proses Eksekusi Program PLC." },
    { "en": "Apa Itu Protokol Komunikasi Industri?", "id": "Modbus, Profibus, Ethernet/IP." },
    { "en": "Apa Itu Jaringan Listrik Mikro (Microgrid)?", "id": "Jaringan Listrik Lokal Terkendali." },
    { "en": "Apa Itu Konverter Pembentuk Jaringan?", "id": "Grid-Forming Converter." },
    { "en": "Apa Itu Konverter Pengikut Jaringan?", "id": "Grid-Following Converter." },
    { "en": "Apa Itu Inersia Sintetis?", "id": "Inersia Disediakan Oleh Konverter." },
    { "en": "Apa Itu Droop Control?", "id": "Metode Pembagian Beban Tanpa Komunikasi." },
    { "en": "Apa Itu Catu Daya Dapat Diprogram?", "id": "Output Dapat Diatur Via Komputer." },
    { "en": "Apa Itu Beban Dapat Diprogram?", "id": "Beban Uji Yang Dapat Diatur." },
    { "en": "Apa Itu Pengujian Kepatuhan (Compliance)?", "id": "Menguji Produk Sesuai Standar." },
    { "en": "Apa Itu Standar IEEE?", "id": "Institute of Electrical and Electronics Engineers." },
    { "en": "Apa Itu Standar IEC?", "id": "International Electrotechnical Commission." },
    { "en": "Apa Itu Tanda CE?", "id": "Menandakan Kepatuhan Dengan Standar Eropa." },
    { "en": "Apa Itu Sertifikasi UL?", "id": "Underwriters Laboratories." },
    { "en": "Apa Itu RoHS (Restriction of Hazardous Substances)?", "id": "Pembatasan Bahan Berbahaya." },
    { "en": "Apa Itu WEEE (Waste Electrical and Electronic Equipment)?", "id": "Direktif Limbah Elektronik." },
    { "en": "Apa Itu Efisiensi Energi?", "id": "Rasio Daya Bermanfaat Per Total Daya." },
    { "en": "Apa Itu Standar Efisiensi 80 Plus?", "id": "Standar Efisiensi Untuk Catu Daya Komputer." },
    { "en": "Apa Itu Energy Star?", "id": "Program Efisiensi Energi." },
    { "en": "Apa Itu Daya Siaga (Standby Power)?", "id": "Daya Yang Dikonsumsi Saat Perangkat Mati." },
    { "en": "Apa Itu Daya Vampir?", "id": "Nama Lain Untuk Daya Siaga." },
    { "en": "Apa Itu Saklar Beban (Load Switch)?", "id": "IC Sederhana Untuk Menghidup-matikan Daya." },
    { "en": "Apa Itu Pengurutan Daya (Power Sequencing)?", "id": "Menghidupkan Beberapa Jalur Daya Berurutan." },
    { "en": "Apa Itu Inverter Gelombang Sinus?", "id": "Menghasilkan Output AC Sinusoidal." },
    { "en": "Apa Itu Konverter Resonansi?", "id": "Menggunakan Resonansi LC Untuk Efisiensi." },
    { "en": "Apa Itu Konverter Quasi-Resonant?", "id": "Konverter Dengan Karakteristik Resonansi." },
    { "en": "Apa Itu Topologi Jembatan Tak Simetris?", "id": "AHB (Asymmetrical Half-Bridge)." },
    { "en": "Apa Itu Topologi LLC?", "id": "Konverter Resonansi (L-L-C)." },
    { "en": "Apa Itu Penggerak Gerbang Sisi-Tinggi?", "id": "Membutuhkan Catu Daya Bootstrap/Terisolasi." },
    { "en": "Apa Itu Dioda Bootstrap?", "id": "Mengisi Kapasitor Bootstrap." },
    { "en": "Apa Itu Kapasitor Bootstrap?", "id": "Menyediakan Catu Daya Mengambang." },
    { "en": "Apa Itu Pergeseran Level?", "id": "Mengubah Level Referensi Sinyal." },
    { "en": "Apa Itu Penguat Indera Arus?", "id": "Current Sense Amplifier." },
    { "en": "Apa Itu Resistor Shunt?", "id": "Resistor Presisi Untuk Mengukur Arus." },
    { "en": "Apa Itu Efek Termoelektrik?", "id": "Kesalahan Pengukuran Akibat Gradien Suhu." },
    { "en": "Apa Itu Koneksi Kelvin?", "id": "Pengukuran Empat-Kawat Untuk Akurasi." },
    { "en": "Apa Itu Sirkuit Terpadu Manajemen Daya?", "id": "PMIC (Power Management Integrated Circuit)." },
    { "en": "Apa Itu Regulator Linear?", "id": "Regulator Yang Bekerja Di Daerah Linear." },
    { "en": "Apa Itu Regulator LDO (Low-Dropout)?", "id": "Regulator Linear Dengan Tegangan Dropout Rendah." },
    { "en": "Apa Itu Bandgap Reference?", "id": "Sumber Referensi Tegangan Stabil." },
    { "en": "Apa Itu PSRR (Power Supply Rejection Ratio)?", "id": "Kemampuan Menolak Derau Catu Daya." },
    { "en": "Apa Itu Respon Transien?", "id": "Respons Terhadap Perubahan Cepat." },
    { "en": "Apa Itu Kapasitansi Keramik Multi-lapis?", "id": "MLCC (Multi-layer Ceramic Capacitor)." },
    { "en": "Apa Itu Efek DC Bias Kapasitor?", "id": "Penurunan Kapasitansi Akibat Tegangan DC." },
    { "en": "Apa Itu Efek Piezoelektrik Kapasitor?", "id": "Kapasitor Menghasilkan Derau Akustik." },
    { "en": "Apa Itu Kapasitor Polimer?", "id": "Kapasitor Dengan ESR Sangat Rendah." },
    { "en": "Apa Itu Induktor Berpelindung (Shielded)?", "id": "Mengurangi Emisi Medan Magnet." },
    { "en": "Apa Itu Induktor Tak Berpelindung?", "id": "Ukuran Lebih Kecil, Biaya Lebih Rendah." },
    { "en": "Apa Itu Arus Saturasi Induktor?", "id": "Arus Yang Menyebabkan Penurunan Induktansi." },
    { "en": "Apa Itu Arus RMS Induktor?", "id": "Batas Arus Berdasarkan Pemanasan." },
    { "en": "Apa Itu Resistansi DC (DCR)?", "id": "Resistansi Belitan Induktor." },
    { "en": "Apa Itu Frekuensi Resonansi Diri (SRF)?", "id": "Frekuensi Resonansi Parasitik." },
    { "en": "Apa Itu Rugi Inti?", "id": "Kerugian Energi Dalam Material Inti." },
    { "en": "Apa Itu Rugi Tembaga?", "id": "Kerugian Akibat Resistansi Belitan." },
    { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus AC Mengalir Di Permukaan." },
    { "en": "Apa Itu Efek Kedekatan (Proximity Effect)?", "id": "Arus Terdistribusi Akibat Konduktor Berdekatan." },
    { "en": "Apa Itu Kawat Litz?", "id": "Kawat Untuk Mengurangi Rugi-rugi AC." },
    { "en": "Apa Itu Layout PCB (Printed Circuit Board)?", "id": "Desain Tata Letak Komponen Dan Jalur." },
    { "en": "Apa Itu Induktansi Parasitik?", "id": "Induktansi Tak Diinginkan Dari Jalur." },
    { "en": "Apa Itu Kapasitansi Parasitik?", "id": "Kapasitansi Tak Diinginkan Antar Jalur." },
    { "en": "Apa Itu Loop Arus Panas (Hot Loop)?", "id": "Loop Dengan Arus Switching Cepat." },
    { "en": "Bagaimana Mendesain Hot Loop?", "id": "Harus Dibuat Sekecil Mungkin." },
    { "en": "Apa Itu Via?", "id": "Koneksi Vertikal Antar Lapisan PCB." },
    { "en": "Apa Itu Bidang Tanah (Ground Plane)?", "id": "Lapisan Tembaga Luas Untuk Ground." },
    { "en": "Apa Itu Bidang Daya (Power Plane)?", "id": "Lapisan Tembaga Untuk Distribusi Daya." },
    { "en": "Apa Itu Penginderaan Jarak Jauh (Remote Sensing)?", "id": "Mengukur Tegangan Langsung Di Beban." },
    { "en": "Apa Itu Pasangan Diferensial?", "id": "Jalur Untuk Sinyal Diferensial." },
    { "en": "Apa Itu Impedansi Terkontrol?", "id": "Jalur Dengan Impedansi Karakteristik Spesifik." },
    { "en": "Apa Itu Radiator Termal?", "id": "Area Tembaga Untuk Membuang Panas." },
    { "en": "Apa Itu Via Termal?", "id": "Via Untuk Membantu Transfer Panas." },
    { "en": "Apa Itu Sirkuit Terpadu Analog?", "id": "IC Untuk Memproses Sinyal Analog." },
    { "en": "Apa Itu Sirkuit Terpadu Sinyal Campuran?", "id": "IC Dengan Sirkuit Analog Dan Digital." },
    { "en": "Apa Itu Sirkuit Terpadu Aplikasi Spesifik?", "id": "ASIC (Application-Specific Integrated Circuit)." },
    { "en": "Apa Itu Elektromagnetisme Komputasi?", "id": "CEM (Computational Electromagnetics)." },
    { "en": "Apa Itu Simulasi Monte Carlo?", "id": "Analisis Statistik Menggunakan Angka Acak." },
    { "en": "Apa Itu Analisis Sensitivitas?", "id": "Mempelajari Pengaruh Variasi Parameter." },
    { "en": "Apa Itu Analisis Kasus Terburuk?", "id": "Menganalisis Skenario Operasi Paling Ekstrem." },
    { "en": "Apa Itu Optimisasi?", "id": "Mencari Solusi Desain Terbaik." },
    { "en": "Apa Itu Algoritma Genetika?", "id": "Metode Optimisasi Terinspirasi Evolusi." },
    { "en": "Apa Itu Pendinginan Termoelektrik?", "id": "TEC (Thermoelectric Cooler)." },
    { "en": "Apa Itu Efek Peltier?", "id": "Prinsip Dasar Pendingin Termoelektrik." },
    { "en": "Apa Itu Efek Seebeck?", "id": "Prinsip Dasar Termokopel." },
    { "en": "Apa Itu Pemanenan Energi?", "id": "Energy Harvesting." },
    { "en": "Apa Itu Pemanenan Energi Getaran?", "id": "Mengubah Getaran Menjadi Listrik." },
    { "en": "Apa Itu Pemanenan Energi RF?", "id": "Mengubah Gelombang Radio Menjadi Listrik." },
    { "en": "Apa Itu Pemanenan Energi Termal?", "id": "Mengubah Perbedaan Panas Menjadi Listrik." },
    { "en": "Apa Itu Pemanenan Energi Surya?", "id": "Mengubah Cahaya Matahari Menjadi Listrik." },
    { "en": "Apa Itu Konverter Piezoelektrik?", "id": "Mengubah Tekanan Mekanis Menjadi Listrik." },
    { "en": "Apa Itu Transfer Daya Nirkabel?", "id": "WPT (Wireless Power Transfer)." },
    { "en": "Apa Itu Kopling Induktif?", "id": "Transfer Daya Lewat Medan Magnet." },
    { "en": "Apa Itu Kopling Resonansi Induktif?", "id": "Transfer Daya Efisien Jarak Menengah." },
    { "en": "Apa Itu Transfer Daya Kapasitif?", "id": "Transfer Daya Lewat Medan Listrik." },
    { "en": "Apa Itu Transfer Daya Gelombang Mikro?", "id": "Mengirim Daya Menggunakan Berkas Microwave." },
    { "en": "Apa Itu Rectenna?", "id": "Antena Penerima Dan Penyearah." },
    { "en": "Apa Itu Sistem Tenaga Listrik DC?", "id": "Menggunakan Distribusi Daya DC." },
    { "en": "Apa Itu Bus DC?", "id": "Jalur Distribusi Daya DC Umum." },
    { "en": "Apa Itu Solid-State Circuit Breaker?", "id": "Pemutus Sirkuit Berbasis Semikonduktor." },
    { "en": "Apa Itu Pembatas Arus Gangguan?", "id": "FCL (Fault Current Limiter)." },
    { "en": "Apa Itu FCL Superkonduktor?", "id": "SFCL (Superconducting Fault Current Limiter)." },
    { "en": "Apa Itu Transformator Solid-State?", "id": "SST (Solid-State Transformer)." },
    { "en": "Apa Itu Konverter AC-AC Matriks?", "id": "Konverter AC-AC Langsung." },
    { "en": "Apa Itu Sistem Penggerak Multi-motor?", "id": "Satu Inverter Mengontrol Beberapa Motor." },
    { "en": "Apa Itu Inverter Kaskade?", "id": "Inverter Multi-level Berbasis Jembatan-H." },
    { "en": "Apa Itu Kontrol Deadbeat?", "id": "Mencapai Respon Dalam Jumlah Langkah Minimum." },
    { "en": "Apa Itu Modulasi Sigma-Delta?", "id": "Teknik Oversampling Di ADC/DAC." },
    { "en": "Apa Itu Noise Shaping?", "id": "Membentuk Spektrum Derau Kuantisasi." },
    { "en": "Apa Itu Dithering?", "id": "Menambahkan Derau Untuk Memperbaiki Linearitas." },
    { "en": "Apa Itu Konverter Jembatan Tiga Fasa?", "id": "Penyearah Atau Inverter Tiga Fasa." },
    { "en": "Apa Itu Penyearah 12-Pulsa?", "id": "Penyearah Dengan Riak Sangat Rendah." },
    { "en": "Apa Itu Penyearah 18-Pulsa?", "id": "Penyearah Dengan Riak Lebih Rendah Lagi." },
    { "en": "Apa Itu Transformator Geser Fasa?", "id": "Digunakan Untuk Penyearah Multi-pulsa." },
    { "en": "Apa Itu Filter LCL?", "id": "Filter Output Inverter (Induktor-Kapasitor-Induktor)." },
    { "en": "Apa Itu Peredaman Pasif?", "id": "Menambahkan Resistor Untuk Meredam Resonansi." },
    { "en": "Apa Itu Peredaman Aktif?", "id": "Menggunakan Loop Kontrol Untuk Meredam Resonansi." },
    { "en": "Apa Itu Stabilitas Sistem Waktu Diskrit?", "id": "Analisis Stabilitas Sistem Digital." },
    { "en": "Apa Itu Lingkaran Satuan (Unit Circle)?", "id": "Referensi Stabilitas Di Domain-Z." },
    { "en": "Apa Itu Metode Transformasi Bilinear?", "id": "Mengubah Sistem Kontinu Ke Diskrit." },
    { "en": "Apa Itu Penahanan Orde Pertama?", "id": "FOH (First-Order Hold)." },
    { "en": "Apa Itu Efek Aliasing Pada Kontrol?", "id": "Dapat Menyebabkan Ketidakstabilan." },
    { "en": "Apa Itu Jaringan Saraf Konvolusional?", "id": "CNN (Convolutional Neural Network)." },
    { "en": "Apa Itu Jaringan Saraf Berulang?", "id": "RNN (Recurrent Neural Network)." },
    { "en": "Apa Itu Pembelajaran Penguatan?", "id": "Reinforcement Learning." },
    { "en": "Apa Itu Kontrol Optimal?", "id": "Mencari Trajektori Kontrol Terbaik." },
    { "en": "Apa Itu Persamaan Hamilton-Jacobi-Bellman?", "id": "Dasar Dari Kontrol Optimal." },
    { "en": "Apa Itu Pemrograman Dinamis?", "id": "Metode Untuk Menyelesaikan Masalah Optimisasi." },
    { "en": "Apa Itu Estimasi Bayesian?", "id": "Estimasi Berbasis Teorema Bayes." },
    { "en": "Apa Itu Filter Partikel?", "id": "Metode Estimasi Non-linear." },
    { "en": "Apa Itu Kembaran Digital (Digital Twin)?", "id": "Model Virtual Dari Aset Fisik." },
    { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Terhubung." },
    { "en": "Apa Itu Komputasi Tepi (Edge Computing)?", "id": "Pemrosesan Data Dekat Sumbernya." },
    { "en": "Apa Itu Komputasi Kabut (Fog Computing)?", "id": "Lapisan Antara Tepi Dan Cloud." },
    { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Sumber Daya Komputasi Via Internet." },
    { "en": "Apa Itu Keamanan Fungsional?", "id": "Melindungi Dari Bahaya Akibat Kegagalan." },
    { "en": "Apa Itu Standar ISO 26262?", "id": "Standar Keamanan Fungsional Otomotif." },
    { "en": "Apa Itu Tingkat Integritas Keamanan Otomotif?", "id": "ASIL (Automotive Safety Integrity Level)." },
    { "en": "Apa Itu Tingkat Integritas Keamanan?", "id": "SIL (Safety Integrity Level)." },
    { "en": "Apa Itu Analisis Bahaya Dan Risiko?", "id": "HARA (Hazard Analysis and Risk Assessment)." },
    { "en": "Apa Itu Ketersediaan (Availability)?", "id": "Probabilitas Sistem Beroperasi." },
    { "en": "Apa Itu Keterpeliharaan (Maintainability)?", "id": "Kemudahan Perbaikan Sistem." },
    { "en": "Apa Itu Kelangsungan (Survivability)?", "id": "Kemampuan Bertahan Dalam Kondisi Keras." },
    { "en": "Apa Itu Ketahanan (Resilience)?", "id": "Kemampuan Pulih Dari Gangguan." },
    { "en": "Apa Itu Analisis Akar Penyebab?", "id": "RCA (Root Cause Analysis)." },
    { "en": "Apa Itu Diagram Tulang Ikan?", "id": "Diagram Ishikawa." },
    { "en": "Apa Itu Analisis 5 Mengapa (5 Whys)?", "id": "Teknik Sederhana Menemukan Akar Penyebab." },
    { "en": "Apa Itu Desain Untuk Keandalan?", "id": "DfR (Design for Reliability)." },
    { "en": "Apa Itu Pengujian Umur Dipercepat?", "id": "ALT (Accelerated Life Testing)." },
    { "en": "Apa Itu Faktor Akselerasi?", "id": "Hubungan Antara Stres Dan Umur." },
    { "en": "Apa Itu Model Stres Arrhenius?", "id": "Model Umur Berbasis Suhu." },
    { "en": "Apa Itu Model Stres Coffin-Manson?", "id": "Model Umur Berbasis Siklus Termal." },
    { "en": "Apa Itu Distribusi Weibull?", "id": "Distribusi Statistik Untuk Analisis Kegagalan." },
    { "en": "Apa Itu Parameter Bentuk (Shape Parameter)?", "id": "Menunjukkan Jenis Mekanisme Kegagalan." },
    { "en": "Apa Itu Parameter Skala (Scale Parameter)?", "id": "Umur Karakteristik." },
    { "en": "Apa Itu Tingkat Kegagalan Bak Mandi?", "id": "Kurva Bathtub." },
    { "en": "Apa Itu Kegagalan Awal (Infant Mortality)?", "id": "Tingkat Kegagalan Tinggi Di Awal." },
    { "en": "Apa Itu Umur Bermanfaat (Useful Life)?", "id": "Tingkat Kegagalan Rendah Dan Konstan." },
    { "en": "Apa Itu Aus (Wear-out)?", "id": "Tingkat Kegagalan Meningkat Di Akhir." },
    { "en": "Apa Itu Pengujian HALT (Highly Accelerated Life Test)?", "id": "Menguji Hingga Titik Kegagalan." },
    { "en": "Apa Itu Pengujian HASS (Highly Accelerated Stress Screen)?", "id": "Menyaring Cacat Produksi." },
    { "en": "Apa Itu Sirkuit Terpadu Daya?", "id": "PIC (Power Integrated Circuit)." },
    { "en": "Apa Itu Proses BCD (Bipolar-CMOS-DMOS)?", "id": "Teknologi Integrasi Daya." },
    { "en": "Apa Itu DMOS (Double-Diffused MOSFET)?", "id": "Struktur MOSFET Daya." },
    { "en": "Apa Itu SOI (Silicon-on-Insulator)?", "id": "Teknologi Substrat Untuk Kinerja Tinggi." },
    { "en": "Apa Itu Penggerak Cerdas (Smart Driver)?", "id": "Driver Dengan Proteksi Dan Diagnostik." },
    { "en": "Apa Itu Saklar Daya Cerdas?", "id": "Smart Power Switch." },
    { "en": "Apa Itu Desain Berbasis Model?", "id": "MBD (Model-Based Design)." },
    { "en": "Apa Itu Pembuatan Kode Otomatis?", "id": "Automatic Code Generation." },
    { "en": "Apa Itu Verifikasi Perangkat Lunak?", "id": "Memastikan Perangkat Lunak Bekerja Benar." },
    { "en": "Apa Itu Validasi Perangkat Lunak?", "id": "Memastikan Perangkat Lunak Memenuhi Kebutuhan." },
    { "en": "Apa Itu Pengujian Perangkat Keras Dalam Loop?", "id": "HIL (Hardware-in-the-Loop) Testing." },
    { "en": "Apa Itu Pengujian Perangkat Lunak Dalam Loop?", "id": "SIL (Software-in-the-Loop) Testing." },
    { "en": "Apa Itu Pengujian Model Dalam Loop?", "id": "MIL (Model-in-the-Loop) Testing." },
    { "en": "Apa Itu Otomasi Desain Elektronik?", "id": "EDA (Electronic Design Automation)." },
    { "en": "Apa Itu Tata Letak Sirkuit Terpadu?", "id": "IC (Integrated Circuit) Layout." },
    { "en": "Apa Itu Pemeriksaan Aturan Desain?", "id": "DRC (Design Rule Check)." },
    { "en": "Apa Itu Tata Letak Versus Skematik?", "id": "LVS (Layout Versus Schematic)." },
    { "en": "Apa Itu Ekstraksi Parasitik?", "id": "Mengekstrak R, L, C Dari Tata Letak." },
    { "en": "Apa Itu Verifikasi Fisik?", "id": "Memastikan Tata Letak Dapat Diproduksi." },
    { "en": "Apa Itu Sintesis Logika?", "id": "Mengubah Kode HDL Menjadi Gerbang Logika." },
    { "en": "Apa Itu Penempatan Dan Perutean?", "id": "Place and Route." },
    { "en": "Apa Itu Analisis Waktu Statis?", "id": "STA (Static Timing Analysis)." },
    { "en": "Apa Itu Analisis Penurunan IR?", "id": "Menganalisis Jatuh Tegangan Di Jaringan Daya." },
    { "en": "Apa Itu Analisis Elektromigrasi?", "id": "Menganalisis Keandalan Interkoneksi Logam." },
    { "en": "Apa Itu Desain Untuk Manufakturabilitas?", "id": "DFM (Design for Manufacturability)." },
    { "en": "Apa Itu Desain Untuk Pengujian?", "id": "DFT (Design for Testability)." },
    { "en": "Apa Itu Pindai Batas (Boundary Scan)?", "id": "Teknik Pengujian JTAG." },
    { "en": "Apa Itu Tes Mandiri Bawaan?", "id": "BIST (Built-In Self-Test)." },
    { "en": "Apa Itu Sel Standar?", "id": "Blok Bangunan Logika Digital Pra-desain." },
    { "en": "Apa Itu Pustaka Sel Standar?", "id": "Kumpulan Gerbang Logika Dan Flip-flop." },
    { "en": "Apa Itu Blok IP (Intellectual Property)?", "id": "Sirkuit Pra-desain Yang Dapat Digunakan Ulang." },
    { "en": "Apa Itu IP Lunak?", "id": "IP Dalam Bentuk Kode HDL." },
    { "en": "Apa Itu IP Keras?", "id": "IP Dalam Bentuk Tata Letak Fisik." },
    { "en": "Apa Itu Node Teknologi?", "id": "Ukuran Fitur Terkecil Dalam Proses Fabrikasi." },
    { "en": "Apa Itu Hukum Moore?", "id": "Jumlah Transistor Berlipat Ganda Setiap Dua Tahun." },
    { "en": "Apa Itu Fotolitografi?", "id": "Proses Pencetakan Pola Sirkuit." },
    { "en": "Apa Itu Wafer Silikon?", "id": "Substrat Dasar Untuk Sirkuit Terpadu." },
    { "en": "Apa Itu Ruang Bersih (Cleanroom)?", "id": "Lingkungan Fabrikasi Dengan Partikel Terkontrol." },
    { "en": "Apa Itu Doping?", "id": "Menambahkan Impuritas Ke Semikonduktor." },
    { "en": "Apa Itu Implantasi Ion?", "id": "Teknik Untuk Doping." },
    { "en": "Apa Itu Etsa (Etching)?", "id": "Menghilangkan Material Secara Selektif." },
    { "en": "Apa Itu Deposisi?", "id": "Menumbuhkan Lapisan Tipis Material." },
    { "en": "Apa Itu Deposisi Uap Kimia?", "id": "CVD (Chemical Vapor Deposition)." },
    { "en": "Apa Itu Deposisi Uap Fisik?", "id": "PVD (Physical Vapor Deposition)." },
    { "en": "Apa Itu Epitaksi?", "id": "Menumbuhkan Lapisan Kristal Tunggal." },
    { "en": "Apa Itu Oksidasi Termal?", "id": "Menumbuhkan Silikon Dioksida." },
    { "en": "Apa Itu Planarisasi Mekanis Kimia?", "id": "CMP (Chemical-Mechanical Planarization)." },
    { "en": "Apa Itu Metalurgi?", "id": "Proses Pembentukan Interkoneksi Logam." },
    { "en": "Apa Itu Pengemasan (Packaging)?", "id": "Melindungi Dan Menghubungkan Chip." },
    { "en": "Apa Itu Die?", "id": "Chip Silikon Individual." },
    { "en": "Apa Itu Pemasangan Permukaan?", "id": "SMT (Surface-Mount Technology)." },
    { "en": "Apa Itu Teknologi Lubang Tembus?", "id": "Through-Hole Technology." },
    { "en": "Apa Itu Ball Grid Array?", "id": "BGA (Ball Grid Array)." },
    { "en": "Apa Itu Flip-Chip?", "id": "Menghubungkan Die Terbalik Ke Substrat." },
    { "en": "Apa Itu Kawat Ikat (Wire Bonding)?", "id": "Menghubungkan Pad Die Ke Leadframe." },
    { "en": "Apa Itu Substrat?", "id": "Papan Sirkuit Kecil Dalam Kemasan IC." },
    { "en": "Apa Itu Leadframe?", "id": "Kerangka Logam Untuk Koneksi Eksternal." },
    { "en": "Apa Itu Senyawa Cetak (Molding Compound)?", "id": "Material Enkapsulasi Plastik." },
    { "en": "Apa Itu Underfill?", "id": "Mengisi Celah Di Bawah Die Flip-Chip." },
    { "en": "Apa Itu Koefisien Ekspansi Termal?", "id": "CTE (Coefficient of Thermal Expansion)." },
    { "en": "Apa Itu Ketidakcocokan CTE?", "id": "Penyebab Stres Mekanis." },
    { "en": "Apa Itu Analisis Elemen Hingga?", "id": "FEA (Finite Element Analysis)." },
    { "en": "Apa Itu Bahan Semikonduktor?", "id": "Silikon (Si), Galium Arsenida (GaAs)." },
    { "en": "Apa Itu Semikonduktor Senyawa?", "id": "Terbuat Dari Dua Elemen Atau Lebih." },
    { "en": "Apa Itu Semikonduktor Celah Pita Langsung?", "id": "Efisien Dalam Memancarkan Cahaya." },
    { "en": "Apa Itu Semikonduktor Celah Pita Tidak Langsung?", "id": "Tidak Efisien Memancarkan Cahaya." },
    { "en": "Apa Itu Rekombinasi?", "id": "Elektron Dan Lubang Bergabung Kembali." },
    { "en": "Apa Itu Generasi?", "id": "Penciptaan Pasangan Elektron-Lubang." },
    { "en": "Apa Itu Pembawa Mayoritas?", "id": "Pembawa Muatan Paling Banyak." },
    { "en": "Apa Itu Pembawa Minoritas?", "id": "Pembawa Muatan Paling Sedikit." },
    { "en": "Apa Itu Tingkat Fermi?", "id": "Tingkat Energi Dengan Probabilitas 50%." },
    { "en": "Apa Itu Sambungan PN?", "id": "Batas Antara Semikonduktor Tipe-P dan Tipe-N." },
    { "en": "Apa Itu Daerah Deplesi?", "id": "Wilayah Tanpa Pembawa Muatan Bebas." },
    { "en": "Apa Itu Potensial Bawaan?", "id": "Tegangan Yang Terbentuk Di Sambungan." },
    { "en": "Apa Itu Bias Maju?", "id": "Mengurangi Potensial Bawaan." },
    { "en": "Apa Itu Bias Mundur?", "id": "Meningkatkan Potensial Bawaan." },
    { "en": "Apa Itu Arus Difusi?", "id": "Aliran Akibat Perbedaan Konsentrasi." },
    { "en": "Apa Itu Arus Drift?", "id": "Aliran Akibat Medan Listrik." },
    { "en": "Apa Itu Persamaan Dioda Shockley?", "id": "Model Matematis Kurva I-V Dioda." },
    { "en": "Apa Itu Kapasitansi Sambungan?", "id": "Kapasitansi Di Daerah Deplesi." },
    { "en": "Apa Itu Kapasitansi Difusi?", "id": "Kapasitansi Akibat Injeksi Pembawa Minoritas." },
    { "en": "Apa Itu Transistor Bipolar?", "id": "BJT (Bipolar Junction Transistor)." },
    { "en": "Apa Itu Transistor Efek Medan?", "id": "FET (Field-Effect Transistor)." },
    { "en": "Apa Itu Daerah Aktif?", "id": "Mode Operasi Untuk Amplifikasi." },
    { "en": "Apa Itu Daerah Saturasi (BJT)?", "id": "Mode Operasi Seperti Saklar Tertutup." },
    { "en": "Apa Itu Daerah Cut-off?", "id": "Mode Operasi Seperti Saklar Terbuka." },
    { "en": "Apa Itu Efek Early?", "id": "Modulasi Lebar Basis." },
    { "en": "Apa Itu JFET (Junction Field-Effect Transistor)?", "id": "FET Dengan Gerbang Sambungan PN." },
    { "en": "Apa Itu Daerah Pinch-off?", "id": "Saat Kanal Tertutup." },
    { "en": "Apa Itu Daerah Triode/Ohmik?", "id": "FET Bekerja Seperti Resistor." },
    { "en": "Apa Itu Daerah Saturasi (FET)?", "id": "Arus Konstan, Bekerja Sebagai Penguat." },
    { "en": "Apa Itu Modulasi Panjang Kanal?", "id": "Efek Mirip Efek Early Pada FET." },
    { "en": "Apa Itu CMOS (Complementary MOS)?", "id": "Menggunakan Transistor NMOS Dan PMOS." },
    { "en": "Apa Keuntungan CMOS?", "id": "Disipasi Daya Statis Sangat Rendah." },
    { "en": "Apa Itu Inverter CMOS?", "id": "Blok Bangunan Dasar Logika CMOS." },
    { "en": "Apa Itu Gerbang Transmisi?", "id": "Saklar Analog Menggunakan CMOS." },
    { "en": "Apa Itu Logika TTL (Transistor-Transistor Logic)?", "id": "Keluarga Logika Berbasis BJT." },
    { "en": "Apa Itu Logika ECL (Emitter-Coupled Logic)?", "id": "Keluarga Logika Kecepatan Sangat Tinggi." },
    { "en": "Apa Itu Pemodelan Perangkat?", "id": "Membuat Model Matematis Perilaku Semikonduktor." },
    { "en": "Apa Itu Fisika Semikonduktor?", "id": "Studi Sifat Fisik Material Semikonduktor." },
    { "en": "Apa Itu Mekanika Kuantum?", "id": "Dasar Dari Perilaku Elektron." },
    { "en": "Apa Itu Struktur Pita?", "id": "Tingkat Energi Yang Diizinkan Untuk Elektron." },
    { "en": "Apa Itu Celah Pita (Bandgap)?", "id": "Energi Pemisah Pita Valensi-Konduksi." },
    { "en": "Apa Itu Pita Valensi?", "id": "Pita Energi Terisi Elektron Valensi." },
    { "en": "Apa Itu Pita Konduksi?", "id": "Pita Energi Untuk Elektron Bergerak Bebas." },
    { "en": "Apa Itu Lubang (Hole)?", "id": "Kekosongan Elektron Di Pita Valensi." },
    { "en": "Apa Itu Massa Efektif?", "id": "Massa Partikel Dalam Kisi Kristal." },
    { "en": "Apa Itu Fonon?", "id": "Kuantum Getaran Kisi." },
    { "en": "Apa Itu Hamburan Fonon?", "id": "Penyebab Utama Resistivitas." },
    { "en": "Apa Itu Semikonduktor Intrinsik?", "id": "Semikonduktor Murni Tanpa Doping." },
    { "en": "Apa Itu Semikonduktor Ekstrinsik?", "id": "Semikonduktor Dengan Doping." },
    { "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Doping Dengan Donor Elektron." },
    { "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Doping Dengan Akseptor Elektron." },
    { "en": "Apa Itu Atom Donor?", "id": "Impuritas Yang Menyumbangkan Elektron." },
    { "en": "Apa Itu Atom Akseptor?", "id": "Impuritas Yang Menerima Elektron." },
    { "en": "Apa Itu Tingkat Energi Donor?", "id": "Tingkat Energi Dekat Pita Konduksi." },
    { "en": "Apa Itu Tingkat Energi Akseptor?", "id": "Tingkat Energi Dekat Pita Valensi." },
    { "en": "Apa Itu Hukum Aksi Massa?", "id": "Perkalian Konsentrasi Elektron-Lubang Konstan." },
    { "en": "Apa Itu Netralitas Muatan?", "id": "Total Muatan Positif Sama Dengan Negatif." },
    { "en": "Apa Itu Pembekuan (Freeze-out)?", "id": "Pada Suhu Rendah, Donor/Akseptor Netral." },
    { "en": "Apa Itu Daerah Saturasi (Doping)?", "id": "Semua Atom Dopan Terionisasi." },
    { "en": "Apa Itu Daerah Intrinsik (Suhu)?", "id": "Pada Suhu Tinggi, Perilaku Intrinsik." },
    { "en": "Apa Itu Persamaan Kontinuitas?", "id": "Menjelaskan Konservasi Pembawa Muatan." },
    { "en": "Apa Itu Waktu Hidup Pembawa Minoritas?", "id": "Waktu Rata-rata Sebelum Rekombinasi." },
    { "en": "Apa Itu Panjang Difusi?", "id": "Jarak Rata-rata Pembawa Sebelum Rekombinasi." },
    { "en": "Apa Itu Kontak Ohmik?", "id": "Sambungan Logam-Semikonduktor Resistansi Rendah." },
    { "en": "Apa Itu Kontak Schottky?", "id": "Sambungan Logam-Semikonduktor Penyearah." },
    { "en": "Apa Itu Fungsi Kerja?", "id": "Energi Untuk Melepaskan Elektron." },
    { "en": "Apa Itu Afinitas Elektron?", "id": "Energi Yang Dilepaskan Saat Menambah Elektron." },
    { "en": "Apa Itu Diagram Pita Energi?", "id": "Representasi Tingkat Energi Dalam Perangkat." },
    { "en": "Apa Itu Pembengkokan Pita?", "id": "Perubahan Tingkat Energi Dekat Sambungan." },
    { "en": "Apa Itu Heterojunction?", "id": "Sambungan Antara Dua Semikonduktor Berbeda." },
    { "en": "Apa Itu Kuantum Well?", "id": "Lapisan Tipis Menjebak Elektron." },
    { "en": "Apa Itu Kuantum Wire?", "id": "Struktur Satu Dimensi." },
    { "en": "Apa Itu Kuantum Dot?", "id": "Struktur Nol Dimensi (Atom Buatan)." },
    { "en": "Apa Itu Superlattice?", "id": "Struktur Periodik Lapisan Tipis." },
    { "en": "Apa Itu Transistor Mobilitas Elektron Tinggi?", "id": "HEMT (High-Electron-Mobility Transistor)." },
    { "en": "Apa Itu Transistor Bipolar Heterojunction?", "id": "HBT (Heterojunction Bipolar Transistor)." },
    { "en": "Apa Itu Efek Fotolistrik?", "id": "Emisi Elektron Akibat Cahaya." },
    { "en": "Apa Itu Sel Surya?", "id": "Mengubah Energi Cahaya Menjadi Listrik." },
    { "en": "Apa Itu Fotodetektor?", "id": "Mendeteksi Cahaya." },
    { "en": "Apa Itu Fotodioda?", "id": "Dioda Yang Sensitif Terhadap Cahaya." },
    { "en": "Apa Itu Fototransistor?", "id": "Transistor Yang Dikontrol Cahaya." },
    { "en": "Apa Itu Dioda Pemancar Cahaya?", "id": "LED (Light-Emitting Diode)." },
    { "en": "Apa Itu Dioda Laser?", "id": "Menghasilkan Cahaya Koheren." },
    { "en": "Apa Itu Emisi Spontan?", "id": "Emisi Foton Secara Acak." },
    { "en": "Apa Itu Emisi Terstimulasi?", "id": "Emisi Foton Dipicu Foton Lain." },
    { "en": "Apa Itu Inversi Populasi?", "id": "Syarat Untuk Terjadinya Lasing." },
    { "en": "Apa Itu Rongga Optik?", "id": "Dua Cermin Untuk Umpan Balik Optik." },
    { "en": "Apa Itu Optoelektronik?", "id": "Studi Perangkat Elektronik Berbasis Cahaya." },
    { "en": "Apa Itu Fotonik?", "id": "Ilmu Dan Teknologi Foton." },
    { "en": "Apa Itu Fotonik Silikon?", "id": "Mengintegrasikan Fotonik Di Chip Silikon." },
    { "en": "Apa Itu MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sistem Mekanis Mikro Di Chip." },
    { "en": "Apa Itu Akselerometer MEMS?", "id": "Sensor Percepatan Berbasis MEMS." },
    { "en": "Apa Itu Giroskop MEMS?", "id": "Sensor Rotasi Berbasis MEMS." },
    { "en": "Apa Itu Mikrofon MEMS?", "id": "Mikrofon Berbasis Silikon." },
    { "en": "Apa Itu Saklar RF MEMS?", "id": "Saklar Frekuensi Radio Mekanis Mikro." },
    { "en": "Apa Itu Cermin Mikro Digital?", "id": "DMD (Digital Micromirror Device)." },
    { "en": "Di Mana DMD Digunakan?", "id": "Proyektor DLP (Digital Light Processing)." },
    { "en": "Apa Itu BioMEMS?", "id": "MEMS Untuk Aplikasi Biologis." },
    { "en": "Apa Itu Lab-on-a-Chip?", "id": "Integrasi Fungsi Laboratorium Di Chip." },
    { "en": "Apa Itu Spintronik?", "id": "Elektronik Berbasis Spin Elektron." },
    { "en": "Apa Itu MRAM (Magnetoresistive RAM)?", "id": "Memori Non-Volatile Berbasis Magnetisme." },
    { "en": "Apa Itu Elektronik Fleksibel?", "id": "Sirkuit Yang Dibangun Di Substrat Lentur." },
    { "en": "Apa Itu Elektronik Cetak?", "id": "Mencetak Sirkuit Menggunakan Tinta Konduktif." },
    { "en": "Apa Itu Elektronik Transparan?", "id": "Sirkuit Yang Tembus Pandang." },
    { "en": "Apa Itu Bahan Feroelektrik?", "id": "Material Dengan Polarisasi Spontan." },
    { "en": "Apa Itu FeRAM (Ferroelectric RAM)?", "id": "Memori Non-Volatile Berbasis Feroelektrik." },
    { "en": "Apa Itu Bahan Piezoelektrik?", "id": "Menghasilkan Tegangan Saat Ditekan." },
    { "en": "Apa Itu Resonator SAW (Surface Acoustic Wave)?", "id": "Filter Frekuensi Berbasis Efek Piezo." },
    { "en": "Apa Itu Resonator BAW (Bulk Acoustic Wave)?", "id": "Filter RF Kinerja Tinggi." },
    { "en": "Apa Itu Bahan Piroelektrik?", "id": "Menghasilkan Tegangan Saat Suhu Berubah." },
    { "en": "Di Mana Bahan Piroelektrik Digunakan?", "id": "Sensor Gerak Inframerah (PIR)." },
    { "en": "Apa Itu Komputasi Neuromorfik?", "id": "Komputasi Terinspirasi Otak Manusia." },
    { "en": "Apa Itu Memristor?", "id": "Resistor Dengan Memori." },
    { "en": "Apa Itu Sirkuit Terpadu Fotonik?", "id": "PIC (Photonic Integrated Circuit)." },
    { "en": "Apa Itu Kriptografi Kuantum?", "id": "Mengamankan Komunikasi Menggunakan Prinsip Kuantum." },
    { "en": "Apa Itu Pendinginan Termionik?", "id": "Mendinginkan Dengan Emisi Elektron." },
    { "en": "Apa Itu Konverter Termofotovoltaik?", "id": "TPV (Thermophotovoltaic) Converter." },
    { "en": "Apa Itu Konverter Magnetohidrodinamik?", "id": "MHD (Magnetohydrodynamic) Generator." },
    { "en": "Apa Itu Efek Elektrokalorik?", "id": "Perubahan Suhu Akibat Medan Listrik." },
    { "en": "Apa Itu Efek Magnetokalorik?", "id": "Perubahan Suhu Akibat Medan Magnet." },
    { "en": "Apa Itu Pendinginan Magnetik?", "id": "Teknologi Pendingin Berbasis Efek Magnetokalorik." },
    { "en": "Apa Itu Elektroluminesensi?", "id": "Emisi Cahaya Akibat Medan Listrik." },
    { "en": "Di Mana Elektroluminesensi Digunakan?", "id": "Layar OLED Dan Lampu Malam." },
    { "en": "Apa Itu Katodoluminesensi?", "id": "Emisi Cahaya Akibat Tumbukan Elektron." },
    { "en": "Di Mana Katodoluminesensi Digunakan?", "id": "Tabung Sinar Katoda (CRT)." },
    { "en": "Apa Itu Termoluminesensi?", "id": "Emisi Cahaya Akibat Pemanasan." },
    { "en": "Di Mana Termoluminesensi Digunakan?", "id": "Dosimetri Radiasi." },
    { "en": "Apa Itu Elektrowetting?", "id": "Mengubah Sudut Kontak Cairan Dengan Tegangan." },
    { "en": "Di Mana Elektrowetting Digunakan?", "id": "Layar E-paper Dan Lensa Cair." },
    { "en": "Apa Itu Elektroforesis?", "id": "Gerakan Partikel Bermuatan Dalam Medan Listrik." },
    { "en": "Di Mana Elektroforesis Digunakan?", "id": "Analisis DNA Dan Layar E-ink." },
    { "en": "Apa Itu Dielektroforesis?", "id": "Gerakan Partikel Netral Dalam Medan Tak Seragam." },
    { "en": "Apa Itu Elektro-osmosis?", "id": "Gerakan Cairan Dalam Medan Listrik." },
    { "en": "Apa Itu Elektrokimia?", "id": "Studi Hubungan Listrik Dan Reaksi Kimia." },
    { "en": "Apa Itu Elektrolisis?", "id": "Menggunakan Listrik Untuk Memicu Reaksi Kimia." },
    { "en": "Apa Itu Sel Galvanik?", "id": "Menghasilkan Listrik Dari Reaksi Kimia." },
    { "en": "Apa Itu Baterai?", "id": "Kombinasi Sel Galvanik." },
    { "en": "Apa Itu Anoda?", "id": "Elektroda Tempat Terjadinya Oksidasi." },
    { "en": "Apa Itu Katoda?", "id": "Elektroda Tempat Terjadinya Reduksi." },
    { "en": "Apa Itu Elektrolit?", "id": "Medium Konduktor Ion." },
    { "en": "Apa Itu Sel Elektrokimia?", "id": "Sistem Penghasil Listrik Dari Reaksi Kimia." },
    { "en": "Apa Itu Potensial Elektroda Standar?", "id": "Ukuran Kecenderungan Terjadinya Reduksi." },
    { "en": "Apa Itu Deret Volta?", "id": "Urutan Logam Berdasarkan Potensial Elektroda." },
    { "en": "Apa Itu Jembatan Garam?", "id": "Menjaga Netralitas Muatan Antar Setengah-sel." },
    { "en": "Apa Itu Persamaan Nernst?", "id": "Menghitung Potensial Sel Non-standar." },
    { "en": "Apa Itu Overpotensial?", "id": "Tegangan Tambahan Untuk Mengatasi Hambatan Reaksi." },
    { "en": "Apa Itu Hukum Faraday Tentang Elektrolisis?", "id": "Menghubungkan Muatan Dengan Jumlah Produk." },
    { "en": "Apa Itu Efisiensi Arus?", "id": "Rasio Produk Aktual Per Teoritis." },
    { "en": "Apa Itu Korosi?", "id": "Degradasi Logam Akibat Reaksi Elektrokimia." },
    { "en": "Apa Itu Proteksi Katodik?", "id": "Metode Mencegah Korosi." },
    { "en": "Apa Itu Anoda Korban?", "id": "Logam Lebih Reaktif Yang Dikorbankan." },
    { "en": "Apa Itu Arus Impres?", "id": "Memberikan Arus DC Eksternal." },
    { "en": "Apa Itu Elektroplating?", "id": "Melapisi Logam Dengan Logam Lain." },
    { "en": "Apa Itu Electropolishing?", "id": "Menghaluskan Permukaan Logam." },
    { "en": "Apa Itu Anodisasi?", "id": "Membentuk Lapisan Oksida Pelindung." },
    { "en": "Apa Itu Baterai Primer?", "id": "Baterai Sekali Pakai." },
    { "en": "Apa Itu Baterai Sekunder?", "id": "Baterai Yang Dapat Diisi Ulang." },
    { "en": "Apa Itu Baterai Timbal-Asam?", "id": "Baterai Aki Mobil." },
    { "en": "Apa Itu Baterai Nikel-Kadmium?", "id": "NiCd (Nickel-Cadmium)." },
    { "en": "Apa Itu Baterai Nikel-Metal Hidrida?", "id": "NiMH (Nickel-Metal Hydride)." },
    { "en": "Apa Itu Baterai Litium-Ion?", "id": "Li-ion." },
    { "en": "Apa Itu Baterai Litium-Polimer?", "id": "LiPo (Lithium-Polymer)." },
    { "en": "Apa Itu Baterai Aliran?", "id": "Flow Battery." },
    { "en": "Apa Itu Baterai Logam-Udara?", "id": "Metal-Air Battery." },
    { "en": "Apa Itu Baterai Solid-State?", "id": "Menggunakan Elektrolit Padat." },
    { "en": "Apa Itu Kapasitas Baterai?", "id": "Diukur Dalam Ampere-jam (Ah)." },
    { "en": "Apa Itu Tingkat Pelepasan Muatan Sendiri?", "id": "Self-Discharge Rate." },
    { "en": "Apa Itu Memori Efek (Baterai)?", "id": "Masalah Pada Baterai NiCd Lama." },
    { "en": "Apa Itu Impedansi Internal?", "id": "Resistansi Internal Baterai." },
    { "en": "Apa Itu Kurva Pelepasan Muatan?", "id": "Grafik Tegangan Vs Kapasitas." },
    { "en": "Apa Itu Pengisian Tetes (Trickle Charging)?", "id": "Pengisian Arus Sangat Rendah." },
    { "en": "Apa Itu Pengisian Cepat?", "id": "Mengisi Baterai Dengan Arus Tinggi." },
    { "en": "Apa Itu Pengisian CC/CV?", "id": "Constant Current/Constant Voltage." },
    { "en": "Apa Itu Sensor?", "id": "Mengubah Besaran Fisik Menjadi Sinyal Listrik." },
    { "en": "Apa Itu Transduser?", "id": "Mengubah Satu Bentuk Energi Ke Lainnya." },
    { "en": "Apa Itu Aktuator?", "id": "Mengubah Sinyal Listrik Menjadi Aksi Fisik." },
    { "en": "Apa Itu Akurasi?", "id": "Kedekatan Pengukuran Dengan Nilai Sebenarnya." },
    { "en": "Apa Itu Presisi?", "id": "Pengulangan Pengukuran Yang Sama." },
    { "en": "Apa Itu Linearitas Sensor?", "id": "Seberapa Lurus Respon Sensor." },
    { "en": "Apa Itu Histeresis Sensor?", "id": "Perbedaan Output Saat Naik Dan Turun." },
    { "en": "Apa Itu Waktu Respon?", "id": "Waktu Sensor Merespon Perubahan." },
    { "en": "Apa Itu Sensitivitas Sensor?", "id": "Rasio Perubahan Output Per Input." },
    { "en": "Apa Itu Rentang Dinamis?", "id": "Rentang Input Yang Dapat Diukur." },
    { "en": "Apa Itu Drift?", "id": "Perubahan Output Seiring Waktu." },
    { "en": "Apa Itu Sinyal-ke-Derau Rasio?", "id": "SNR (Signal-to-Noise Ratio)." },
    { "en": "Apa Itu Kalibrasi?", "id": "Menyesuaikan Sensor Dengan Standar." },
    { "en": "Apa Itu Kompensasi?", "id": "Mengoreksi Efek Eksternal (Suhu)." },
    { "en": "Apa Itu Jaringan Sensor?", "id": "Sekelompok Sensor Yang Terhubung." },
    { "en": "Apa Itu Fusi Sensor?", "id": "Menggabungkan Data Dari Beberapa Sensor." },
    { "en": "Apa Itu Sensor Cerdas?", "id": "Sensor Dengan Pemrosesan Terintegrasi." },
    { "en": "Apa Itu Sensor Nirkabel?", "id": "Sensor Yang Berkomunikasi Tanpa Kabel." },
    { "en": "Apa Itu Potensiometer?", "id": "Sensor Posisi Sudut Resistif." },
    { "en": "Apa Itu Strain Gauge?", "id": "Mengukur Regangan Mekanis." },
    { "en": "Apa Itu Load Cell?", "id": "Mengukur Gaya Atau Berat." },
    { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu Berbasis Efek Seebeck." },
    { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Berbasis Resistansi." },
    { "en": "Apa Itu Termistor?", "id": "Resistor Yang Peka Terhadap Suhu." },
    { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Yang Peka Terhadap Cahaya." },
    { "en": "Apa Itu Encoder Optik?", "id": "Sensor Posisi Atau Kecepatan Optik." },
    { "en": "Apa Itu Encoder Inkremental?", "id": "Menghasilkan Pulsa Relatif Terhadap Gerakan." },
    { "en": "Apa Itu Encoder Absolut?", "id": "Memberikan Posisi Sudut Unik." },
    { "en": "Apa Itu Tachometer?", "id": "Mengukur Kecepatan Rotasi." },
    { "en": "Apa Itu Efek Hall?", "id": "Menghasilkan Tegangan Akibat Medan Magnet." },
    { "en": "Apa Itu Sensor Proximity Induktif?", "id": "Mendeteksi Benda Logam." },
    { "en": "Apa Itu Sensor Proximity Kapasitif?", "id": "Mendeteksi Berbagai Jenis Benda." },
    { "en": "Apa Itu Sensor Proximity Ultrasonik?", "id": "Menggunakan Suara Untuk Mengukur Jarak." },
    { "en": "Apa Itu Sensor Proximity Optik?", "id": "Menggunakan Cahaya Untuk Mendeteksi Objek." },
    { "en": "Apa Itu LiDAR (Light Detection and Ranging)?", "id": "Mengukur Jarak Dengan Laser." },
    { "en": "Apa Itu RADAR (Radio Detection and Ranging)?", "id": "Menggunakan Gelombang Radio." },
    { "en": "Apa Itu SONAR (Sound Navigation and Ranging)?", "id": "Menggunakan Gelombang Suara." },
    { "en": "Apa Itu Sensor Gambar CMOS?", "id": "CIS (CMOS Image Sensor)." },
    { "en": "Apa Itu Perangkat Terkopel Muatan?", "id": "CCD (Charge-Coupled Device)." },
    { "en": "Apa Itu Piksel?", "id": "Elemen Individual Dalam Sensor Gambar." },
    { "en": "Apa Itu Filter Warna Bayer?", "id": "Pola Filter Merah, Hijau, Biru." },
    { "en": "Apa Itu Demosaicing?", "id": "Merekonstruksi Gambar Warna Penuh." },
    { "en": "Apa Itu White Balance?", "id": "Menyesuaikan Warna Berdasarkan Iluminasi." },
    { "en": "Apa Itu Autofokus?", "id": "Menyesuaikan Fokus Lensa Secara Otomatis." },
    { "en": "Apa Itu Deteksi Fasa?", "id": "Metode Autofokus Cepat." },
    { "en": "Apa Itu Deteksi Kontras?", "id": "Metode Autofokus Berbasis Kontras." },
    { "en": "Apa Itu Shutter Global?", "id": "Membaca Semua Piksel Sekaligus." },
    { "en": "Apa Itu Rolling Shutter?", "id": "Membaca Piksel Baris Demi Baris." },
    { "en": "Apa Efek Rolling Shutter?", "id": "Distorsi Pada Objek Bergerak Cepat." },
    { "en": "Apa Itu Rentang Dinamis Sensor?", "id": "Rasio Antara Sinyal Terterang-Tergelap." },
    { "en": "Apa Itu Kuantum Efisiensi?", "id": "Efisiensi Konversi Foton-Elektron." },
    { "en": "Apa Itu Arus Gelap?", "id": "Derau Yang Dihasilkan Tanpa Cahaya." },
    { "en": "Apa Itu Derau Baca?", "id": "Derau Dari Proses Pembacaan Piksel." },
    { "en": "Apa Itu Binning Piksel?", "id": "Menggabungkan Piksel Untuk Sensitivitas." },
    { "en": "Apa Itu Kontrol Paparan Otomatis?", "id": "AEC (Automatic Exposure Control)." },
    { "en": "Apa Itu Kontrol Penguatan Otomatis?", "id": "AGC (Automatic Gain Control)." },
    { "en": "Apa Itu Stabilisasi Gambar Optik?", "id": "OIS (Optical Image Stabilization)." },
    { "en": "Apa Itu Stabilisasi Gambar Elektronik?", "id": "EIS (Electronic Image Stabilization)." },
    { "en": "Apa Itu HDR (High Dynamic Range) Imaging?", "id": "Menggabungkan Beberapa Paparan." },
    { "en": "Apa Itu Sensor Time-of-Flight (ToF)?", "id": "Mengukur Jarak Dengan Waktu Tempuh Cahaya." },
    { "en": "Apa Itu Kamera Stereoskopik?", "id": "Menggunakan Dua Lensa Untuk Persepsi Kedalaman." },
    { "en": "Apa Itu Pencitraan Terstruktur Cahaya?", "id": "Structured Light Imaging." },
    { "en": "Apa Itu Pembaca Kode Batang?", "id": "Barcode Scanner." },
    { "en": "Apa Itu Kode Batang Satu Dimensi?", "id": "1D Barcode (UPC, EAN)." },
    { "en": "Apa Itu Kode Batang Dua Dimensi?", "id": "2D Barcode (QR Code, Data Matrix)." },
    { "en": "Apa Itu Sistem Pengenalan Karakter Optik?", "id": "OCR (Optical Character Recognition)." },
    { "en": "Apa Itu Sistem Pengenalan Wajah?", "id": "Facial Recognition System." },
    { "en": "Apa Itu Sistem Pengenalan Sidik Jari?", "id": "Fingerprint Recognition System." },
    { "en": "Apa Itu Sistem Pengenalan Iris?", "id": "Iris Recognition System." },
    { "en": "Apa Itu Sistem Pengenalan Suara?", "id": "Voice Recognition System." },
    { "en": "Apa Itu Biometrik?", "id": "Pengenalan Berdasarkan Karakteristik Fisik." },
    { "en": "Apa Itu Sistem Catu Daya?", "id": "Menyediakan Daya Listrik Ke Sirkuit." },
    { "en": "Apa Itu Catu Daya Linear?", "id": "Menggunakan Trafo Dan Regulator Linear." },
    { "en": "Apa Itu Catu Daya Switching?", "id": "SMPS (Switch-Mode Power Supply)." },
    { "en": "Apa Kelemahan Catu Daya Linear?", "id": "Efisiensi Rendah, Ukuran Besar." },
    { "en": "Apa Kelemahan Catu Daya Switching?", "id": "Kompleks, Menghasilkan Derau (EMI)." },
    { "en": "Apa Itu Arus Inrush?", "id": "Arus Puncak Saat Catu Daya Dinyalakan." },
    { "en": "Apa Itu Pengurutan Catu Daya?", "id": "Menghidupkan Beberapa Tegangan Berurutan." },
    { "en": "Apa Itu Catu Daya Redundan?", "id": "Memiliki Catu Daya Cadangan." },
    { "en": "Apa Itu Catu Daya Terdistribusi?", "id": "Beberapa Konverter Lokal Dekat Beban." },
    { "en": "Apa Itu Manajemen Daya?", "id": "Mengoptimalkan Penggunaan Daya." },
    { "en": "Apa Itu Mode Tidur (Sleep Mode)?", "id": "Mode Operasi Daya Sangat Rendah." },
    { "en": "Apa Itu Mode Hibernasi?", "id": "Menyimpan Keadaan Ke Memori Non-volatile." },
    { "en": "Apa Itu Penskalaan Frekuensi Dinamis?", "id": "DFS (Dynamic Frequency Scaling)." },
    { "en": "Apa Itu Penskalaan Tegangan Dinamis?", "id": "DVS (Dynamic Voltage Scaling)." },
    { "en": "Apa Itu Penskalaan Tegangan Dan Frekuensi Dinamis?", "id": "DVFS (Dynamic Voltage and Frequency Scaling)." },
    { "en": "Apa Itu Gerbang Clock (Clock Gating)?", "id": "Mematikan Clock Ke Blok Tidak Aktif." },
    { "en": "Apa Itu Gerbang Daya (Power Gating)?", "id": "Mematikan Catu Daya Ke Blok Tidak Aktif." },
    { "en": "Apa Itu Baterai Koin?", "id": "Baterai Kecil Untuk RTC Atau Memori." },
    { "en": "Apa Itu Pemanenan Energi RF?", "id": "Mengambil Energi Dari Gelombang Radio." },
    { "en": "Apa Itu Motor DC?", "id": "Motor Arus Searah." },
    { "en": "Apa Itu Stator?", "id": "Bagian Diam Dari Motor." },
    { "en": "Apa Itu Rotor?", "id": "Bagian Berputar Dari Motor." },
    { "en": "Apa Itu Komutator?", "id": "Saklar Putar Mekanis." },
    { "en": "Apa Itu Sikat (Brush)?", "id": "Kontak Geser Penghantar Arus." },
    { "en": "Apa Itu Motor DC Dengan Sikat?", "id": "Brushed DC Motor." },
    { "en": "Apa Itu Motor DC Tanpa Sikat?", "id": "BLDC (Brushless DC) Motor." },
    { "en": "Apa Itu Motor AC?", "id": "Motor Arus Bolak-balik." },
    { "en": "Apa Itu Motor Induksi?", "id": "Motor AC Paling Umum." },
    { "en": "Apa Itu Rotor Sangkar Tupai?", "id": "Squirrel Cage Rotor." },
    { "en": "Apa Itu Rotor Belitan?", "id": "Wound Rotor." },
    { "en": "Apa Itu Motor Sinkron?", "id": "Kecepatan Rotor Sama Dengan Medan Putar." },
    { "en": "Apa Itu Medan Magnet Putar?", "id": "Dihasilkan Oleh Catu Daya Tiga Fasa." },
    { "en": "Apa Itu Motor Servo?", "id": "Motor Dengan Umpan Balik Posisi." },
    { "en": "Apa Itu Motor Stepper?", "id": "Motor Yang Bergerak Dalam Langkah." },
    { "en": "Apa Itu Motor Linear?", "id": "Menghasilkan Gerakan Lurus." },
    { "en": "Apa Itu Aktuator Suara Koil?", "id": "VCM (Voice Coil Motor)." },
    { "en": "Apa Itu Papan Sirkuit Cetak?", "id": "PCB (Printed Circuit Board)." },
    { "en": "Apa Itu Bahan Substrat PCB?", "id": "FR-4 (Flame Retardant 4)." },
    { "en": "Apa Itu PCB Satu Sisi?", "id": "Jalur Tembaga Di Satu Sisi." },
    { "en": "Apa Itu PCB Dua Sisi?", "id": "Jalur Tembaga Di Kedua Sisi." },
    { "en": "Apa Itu PCB Multi-lapis?", "id": "Memiliki Lapisan Internal." },
    { "en": "Apa Itu Jalur (Trace)?", "id": "Garis Tembaga Penghubung." },
    { "en": "Apa Itu Pad?", "id": "Area Tembaga Untuk Menyolder Komponen." },
    { "en": "Apa Itu Via?", "id": "Lubang Konduktif Antar Lapisan." },
    { "en": "Apa Itu Via Buta (Blind Via)?", "id": "Menghubungkan Lapisan Luar-Dalam." },
    { "en": "Apa Itu Via Terkubur (Buried Via)?", "id": "Hanya Menghubungkan Lapisan Internal." },
    { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Berwarna." },
    { "en": "Apa Itu Silkscreen?", "id": "Tanda Putih Untuk Identifikasi Komponen." },
    { "en": "Apa Itu Komponen Pemasangan Permukaan?", "id": "SMD (Surface-Mount Device)." },
    { "en": "Apa Itu Komponen Lubang Tembus?", "id": "Through-Hole Component." },
    { "en": "Apa Itu Desain Skematik?", "id": "Diagram Logis Rangkaian." },
    { "en": "Apa Itu Desain Tata Letak (Layout)?", "id": "Desain Fisik Papan PCB." },
    { "en": "Apa Itu File Gerber?", "id": "Format Standar Untuk Manufaktur PCB." },
    { "en": "Apa Itu File Bor?", "id": "Informasi Lokasi Dan Ukuran Lubang." },
    { "en": "Apa Itu Panelisasi?", "id": "Menggabungkan Beberapa Papan Dalam Satu Panel." },
    { "en": "Apa Itu Papan Sirkuit Fleksibel?", "id": "Flex PCB." },
    { "en": "Apa Itu Papan Sirkuit Kaku-Fleksibel?", "id": "Rigid-Flex PCB." },
    { "en": "Apa Itu PCB Inti Logam?", "id": "MCPCB (Metal Core PCB)." },
    { "en": "Di Mana MCPCB Digunakan?", "id": "Aplikasi LED Berdaya Tinggi." },
    { "en": "Apa Itu Penyolderan Reflow?", "id": "Metode Penyolderan Untuk SMD." },
    { "en": "Apa Itu Pasta Solder?", "id": "Campuran Bola Timah Dan Fluks." },
    { "en": "Apa Itu Penyolderan Gelombang?", "id": "Metode Penyolderan Untuk Through-Hole." },
    { "en": "Apa Itu Fluks?", "id": "Bahan Kimia Pembersih Oksida." },
    { "en": "Apa Itu Solder Tanpa Timbal?", "id": "Solder Bebas Unsur Timbal (Pb)." },
    { "en": "Apa Itu Paduan Solder?", "id": "Campuran Logam (Timah, Perak, Tembaga)." },
    { "en": "Apa Itu Jembatan Solder?", "id": "Hubung Singkat Tak Diinginkan." },
    { "en": "Apa Itu Sambungan Solder Dingin?", "id": "Sambungan Solder Yang Buruk." },
    { "en": "Apa Itu Pengerjaan Ulang (Rework)?", "id": "Memperbaiki Kesalahan Perakitan." },
    { "en": "Apa Itu Stasiun Udara Panas?", "id": "Alat Untuk Melepas Komponen SMD." },
    { "en": "Apa Itu Pelapisan Konformal?", "id": "Lapisan Pelindung Tipis Untuk PCB." },
    { "en": "Apa Itu Potting?", "id": "Mengisi Selungkup Elektronik Dengan Resin." },
    { "en": "Apa Itu Pengujian Dalam Sirkuit?", "id": "ICT (In-Circuit Testing)." },
    { "en": "Apa Itu Pengujian Fungsional?", "id": "FCT (Functional Testing)." },
    { "en": "Apa Itu Flying Probe Test?", "id": "Pengujian PCB Otomatis Tanpa Jig." },
    { "en": "Apa Itu Inspeksi Optik Otomatis?", "id": "AOI (Automated Optical Inspection)." },
    { "en": "Apa Itu Inspeksi Sinar-X Otomatis?", "id": "AXI (Automated X-ray Inspection)." },
    { "en": "Apa Itu Daftar Material?", "id": "BOM (Bill of Materials)." },
    { "en": "Apa Itu Desain Untuk Perakitan?", "id": "DFA (Design for Assembly)." },
    { "en": "Apa Itu Desain Untuk Manufaktur?", "id": "DFM (Design for Manufacturing)." },
    { "en": "Apa Itu Desain Untuk Pengujian?", "id": "DFT (Design for Testability)." },
    { "en": "Apa Itu Analisis Integritas Sinyal?", "id": "Memastikan Kualitas Sinyal Listrik." },
    { "en": "Apa Itu Analisis Integritas Daya?", "id": "Memastikan Kualitas Distribusi Daya." },
    { "en": "Apa Itu Analisis Termal?", "id": "Menganalisis Distribusi Panas." },
    { "en": "Apa Itu Analisis EMI/EMC?", "id": "Analisis Gangguan Elektromagnetik." },
    { "en": "Apa Itu Perangkat Keras Sumber Terbuka?", "id": "Open-Source Hardware (OSHW)." },
    { "en": "Apa Itu Mikroskop USB?", "id": "Digunakan Untuk Inspeksi Papan Sirkuit." },
    { "en": "Apa Itu Osiloskop Penyimpanan Digital?", "id": "DSO (Digital Storage Oscilloscope)." },
    { "en": "Apa Itu Osiloskop Sinyal Campuran?", "id": "MSO (Mixed-Signal Oscilloscope)." },
    { "en": "Apa Itu Probe Diferensial?", "id": "Mengukur Selisih Tegangan Antara Dua Titik." },
    { "en": "Apa Itu Probe Arus?", "id": "Mengukur Arus Tanpa Memutus Sirkuit." },
    { "en": "Apa Itu Probe Tegangan Tinggi?", "id": "Mengukur Tegangan Ribuan Volt." },
    { "en": "Apa Itu Penganalisis Logika?", "id": "Menganalisis Banyak Saluran Sinyal Digital." },
    { "en": "Apa Itu Penganalisis Spektrum?", "id": "Menampilkan Sinyal Dalam Domain Frekuensi." },
    { "en": "Apa Itu Penganalisis Jaringan Vektor?", "id": "VNA (Vector Network Analyzer)." },
    { "en": "Apa Fungsi VNA (Vector Network Analyzer)?", "id": "Mengukur Parameter-S Sirkuit RF." },
    { "en": "Apa Itu LCR Meter?", "id": "Mengukur Induktansi, Kapasitansi, Resistansi." },
    { "en": "Apa Itu Multimeter Digital?", "id": "DMM (Digital Multimeter)." },
    { "en": "Apa Itu Sumber Unit Ukur?", "id": "SMU (Source Measure Unit)." },
    { "en": "Apa Itu Beban Elektronik DC?", "id": "Beban Uji Yang Dapat Diprogram." },
    { "en": "Apa Itu Generator Sinyal Arbitrer?", "id": "AWG (Arbitrary Waveform Generator)." },
    { "en": "Apa Itu Generator Pulsa?", "id": "Menghasilkan Pulsa Dengan Parameter Terkontrol." },
    { "en": "Apa Itu Pencacah Frekuensi?", "id": "Mengukur Frekuensi Sinyal Dengan Presisi." },
    { "en": "Apa Itu Kamera Termal?", "id": "Mengambil Gambar Berdasarkan Radiasi Inframerah." },
    { "en": "Apa Itu Sistem Akuisisi Data?", "id": "DAQ (Data Acquisition System)." },
    { "en": "Apa Itu Antarmuka Bus Instrumen Umum?", "id": "GPIB (General Purpose Interface Bus)." },
    { "en": "Apa Itu LAN Untuk Ekstensi Instrumen?", "id": "LXI (LAN eXtensions for Instrumentation)." },
    { "en": "Apa Itu PXI (PCI eXtensions for Instrumentation)?", "id": "Platform Instrumentasi Modular." },
    { "en": "Apa Itu SCPI (Standard Commands for Programmable Instruments)?", "id": "Bahasa Perintah Untuk Instrumen." },
    { "en": "Apa Itu LabVIEW?", "id": "Lingkungan Pemrograman Grafis Untuk Instrumentasi." },
    { "en": "Apa Itu Selungkup (Enclosure)?", "id": "Kotak Pelindung Peralatan Elektronik." },
    { "en": "Apa Itu Peringkat IP (Ingress Protection)?", "id": "Tingkat Proteksi Terhadap Debu Dan Air." },
    { "en": "Apa Itu Peringkat NEMA?", "id": "Standar Selungkup Di Amerika Utara." },
    { "en": "Apa Itu Selungkup Tahan Ledakan?", "id": "Mencegah Ledakan Internal Menyebar." },
    { "en": "Apa Itu Kipas Pendingin?", "id": "Mengalirkan Udara Untuk Mendinginkan Komponen." },
    { "en": "Apa Itu Pendingin Peltier?", "id": "Pendingin Solid-State Berbasis Efek Peltier." },
    { "en": "Apa Itu Penukar Panas (Heat Exchanger)?", "id": "Memindahkan Panas Antara Dua Medium." },
    { "en": "Apa Itu Manajemen Kabel?", "id": "Mengatur Dan Merapikan Kabel." },
    { "en": "Apa Itu Pengikat Kabel (Cable Tie)?", "id": "Tali Plastik Untuk Mengikat Kabel." },
    { "en": "Apa Itu Selongsong Kabel?", "id": "Melindungi Dan Merapikan Kumpulan Kabel." },
    { "en": "Apa Itu Kelenjar Kabel (Cable Gland)?", "id": "Menyegel Kabel Yang Masuk Ke Selungkup." },
    { "en": "Apa Itu Blok Terminal?", "id": "Titik Koneksi Untuk Kabel." },
    { "en": "Apa Itu Rel DIN?", "id": "Rel Standar Untuk Memasang Komponen." },
    { "en": "Apa Itu Ferrul?", "id": "Selongsong Logam Di Ujung Kabel Serabut." },
    { "en": "Apa Itu Lug Kabel?", "id": "Terminal Koneksi Untuk Kabel Besar." },
    { "en": "Apa Itu Alat Pengeriting (Crimping Tool)?", "id": "Memasang Terminal Ke Kabel." },
    { "en": "Apa Itu Alat Pengupas Kabel?", "id": "Wire Stripper." },
    { "en": "Apa Itu Pistol Panas (Heat Gun)?", "id": "Digunakan Untuk Heat Shrink Tubing." },
    { "en": "Apa Itu Tabung Susut Panas?", "id": "Heat Shrink Tubing." },
    { "en": "Apa Itu Konduktor?", "id": "Bahan Yang Menghantarkan Listrik." },
    { "en": "Apa Itu Isolator?", "id": "Bahan Yang Menghambat Listrik." },
    { "en": "Apa Itu Semikonduktor?", "id": "Bahan Antara Konduktor Dan Isolator." },
    { "en": "Apa Itu Superkonduktor?", "id": "Bahan Dengan Resistansi Listrik Nol." },
    { "en": "Apa Itu Resistivitas?", "id": "Ukuran Intrinsik Hambatan Bahan." },
    { "en": "Apa Itu Konduktivitas?", "id": "Ukuran Intrinsik Kemampuan Hantar Bahan." },
    { "en": "Apa Itu Ampacity?", "id": "Kapasitas Hantar Arus Maksimum Konduktor." },
    { "en": "Apa Itu Jatuh Tegangan?", "id": "Penurunan Tegangan Sepanjang Kabel." },
    { "en": "Apa Itu Kode Warna Kabel?", "id": "Standar Warna Untuk Identifikasi Kabel." },
    { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel Dengan Konduktor Pusat Dan Pelindung." },
    { "en": "Apa Itu Kabel Pasangan Terpilin?", "id": "Twisted Pair Cable." },
    { "en": "Apa Itu Kabel Berpelindung?", "id": "Kabel Dengan Lapisan Pelindung EMI." },
    { "en": "Apa Itu Kabel Pita (Ribbon Cable)?", "id": "Kabel Datar Dengan Banyak Konduktor." },
    { "en": "Apa Itu Serat Optik?", "id": "Media Transmisi Menggunakan Cahaya." },
    { "en": "Apa Itu Konektor BNC?", "id": "Konektor Koaksial Tipe Bayonet." },
    { "en": "Apa Itu Konektor SMA?", "id": "Konektor Koaksial Ulir Frekuensi Tinggi." },
    { "en": "Apa Itu Konektor RJ45?", "id": "Konektor Untuk Jaringan Ethernet." },
    { "en": "Apa Itu Konektor D-Sub?", "id": "Konektor Multi-pin Bentuk-D." },
    { "en": "Apa Itu Konektor USB (Universal Serial Bus)?", "id": "Standar Koneksi Periferal." },
    { "en": "Apa Itu Konektor HDMI (High-Definition Multimedia Interface)?", "id": "Standar Koneksi Audio/Video Digital." },
    { "en": "Apa Itu Elektromagnet?", "id": "Magnet Yang Dihasilkan Oleh Arus Listrik." },
    { "en": "Apa Itu Solenoida?", "id": "Kumparan Kawat Berbentuk Tabung." },
    { "en": "Apa Itu Relai?", "id": "Saklar Yang Dioperasikan Elektromagnetik." },
    { "en": "Apa Itu Kontaktor?", "id": "Relai Berdaya Besar." },
    { "en": "Apa Itu Katup Solenoida?", "id": "Katup Yang Dioperasikan Elektromagnetik." },
    { "en": "Apa Itu Bel Listrik?", "id": "Menggunakan Elektromagnet Untuk Menghasilkan Suara." },
    { "en": "Apa Itu Pengeras Suara (Loudspeaker)?", "id": "Mengubah Sinyal Listrik Menjadi Suara." },
    { "en": "Apa Itu Mikrofon Dinamis?", "id": "Menggunakan Induksi Elektromagnetik." },
    { "en": "Apa Itu Pickup Gitar?", "id": "Transduser Getaran Senar Menjadi Listrik." },
    { "en": "Apa Itu Pengereman Arus Eddy?", "id": "Pengereman Menggunakan Medan Magnet." },
    { "en": "Apa Itu Kopling Magnetik?", "id": "Mentransfer Torsi Melalui Medan Magnet." },
    { "en": "Apa Itu Bantalan Magnetik?", "id": "Menopang Poros Dengan Medan Magnet." },
    { "en": "Apa Itu Levitasi Magnetik?", "id": "Mengambangkan Objek Dengan Medan Magnet." },
    { "en": "Apa Itu Hukum Ampere?", "id": "Menghubungkan Arus Dengan Medan Magnet." },
    { "en": "Apa Itu Hukum Biot-Savart?", "id": "Menghitung Medan Magnet Dari Arus." },
    { "en": "Apa Itu Hukum Faraday Tentang Induksi?", "id": "Perubahan Fluks Magnet Menghasilkan GGL." },
    { "en": "Apa Itu Hukum Lenz?", "id": "Arah Arus Induksi Melawan Perubahan." },
    { "en": "Apa Itu Persamaan Maxwell?", "id": "Kumpulan Persamaan Dasar Elektromagnetisme." },
    { "en": "Apa Itu Gelombang Elektromagnetik?", "id": "Getaran Medan Listrik Dan Magnet." },
    { "en": "Apa Itu Spektrum Elektromagnetik?", "id": "Rentang Semua Frekuensi Gelombang EM." },
    { "en": "Apa Itu Antena?", "id": "Mengubah Gelombang Terpandu Menjadi Ruang Bebas." },
    { "en": "Apa Itu Polarisasi?", "id": "Orientasi Medan Listrik Gelombang." },
    { "en": "Apa Itu Antena Dipol?", "id": "Antena Dasar Setengah Panjang Gelombang." },
    { "en": "Apa Itu Antena Monopol?", "id": "Antena Seperempat Panjang Gelombang." },
    { "en": "Apa Itu Antena Yagi-Uda?", "id": "Antena Direktif Dengan Banyak Elemen." },
    { "en": "Apa Itu Antena Parabola?", "id": "Antena Reflektor Untuk Gain Tinggi." },
    { "en": "Apa Itu Antena Patch?", "id": "Antena Planar Di Papan PCB." },
    { "en": "Apa Itu Array Antena?", "id": "Sekelompok Antena Bekerja Bersama." },
    { "en": "Apa Itu Beamforming?", "id": "Mengarahkan Pola Radiasi Secara Elektronik." },
    { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Pipa Logam Untuk Gelombang Mikro." },
    { "en": "Apa Itu Rongga Resonansi?", "id": "Resonator Frekuensi Tinggi." },
    { "en": "Apa Itu Magnetron?", "id": "Sumber Gelombang Mikro Berdaya Tinggi." },
    { "en": "Apa Itu Klystron?", "id": "Penguat Gelombang Mikro Kecepatan Termodulasi." },
    { "en": "Apa Itu Tabung Gelombang Berjalan?", "id": "TWT (Traveling-Wave Tube)." },
    { "en": "Apa Itu Gyrotron?", "id": "Sumber Gelombang Milimeter Berdaya Tinggi." },
    { "en": "Apa Itu Sirkulator Ferit?", "id": "Perangkat Non-resiprokal." },
    { "en": "Apa Itu Isolator Ferit?", "id": "Memungkinkan Gelombang Lewat Satu Arah." },
    { "en": "Apa Itu Geseran Fasa Faraday?", "id": "Rotasi Polarisasi Dalam Material Magnet." },
    { "en": "Apa Itu Bahan Penyerap Radar?", "id": "RAM (Radar-Absorbing Material)." },
    { "en": "Apa Itu Metamaterial?", "id": "Material Buatan Dengan Sifat Elektromagnetik Unik." },
    { "en": "Apa Itu Selubung Tembus Pandang (Invisibility Cloak)?", "id": "Aplikasi Hipotetis Metamaterial." },
    { "en": "Apa Itu Permukaan Selektif Frekuensi?", "id": "FSS (Frequency-Selective Surface)." },
    { "en": "Apa Itu Lensa Luneburg?", "id": "Lensa Untuk Memfokuskan Gelombang Radio." },
    { "en": "Apa Itu Reflektor Sudut?", "id": "Target Pasif Yang Memantulkan Sinyal Kuat." },
    { "en": "Apa Itu Radome?", "id": "Penutup Pelindung Antena Tahan Cuaca." },
    { "en": "Apa Itu Sistem Radar Pasif?", "id": "Mendeteksi Target Menggunakan Sinyal Eksternal." },
    { "en": "Apa Itu Radar Bistatik?", "id": "Pemancar Dan Penerima Terpisah Jauh." },
    { "en": "Apa Itu Radar Monostatik?", "id": "Pemancar Dan Penerima Di Lokasi Sama." },
    { "en": "Apa Itu Radar Gelombang Kontinu?", "id": "CW (Continuous-Wave) Radar." },
    { "en": "Apa Itu Radar FMCW (Frequency-Modulated Continuous-Wave)?", "id": "Mengukur Jarak Dengan Perubahan Frekuensi." },
    { "en": "Apa Itu Radar Pulsa-Doppler?", "id": "Mengukur Jarak Dan Kecepatan." },
    { "en": "Apa Itu Pemrosesan Sinyal Radar?", "id": "Mengekstrak Informasi Dari Gema Radar." },
    { "en": "Apa Itu Indikasi Target Bergerak?", "id": "MTI (Moving Target Indication)." },
    { "en": "Apa Itu Indikasi Target Bergerak Udara?", "id": "AMTI (Airborne Moving Target Indication)." },
    { "en": "Apa Itu Clutter?", "id": "Gema Tak Diinginkan (Tanah, Hujan)." },
    { "en": "Apa Itu Penekanan Clutter?", "id": "Mengurangi Efek Gema Tak Diinginkan." },
    { "en": "Apa Itu Peta Clutter?", "id": "Menyimpan Data Clutter Untuk Pengurangan." },
    { "en": "Apa Itu Constant False Alarm Rate?", "id": "CFAR (Constant False Alarm Rate)." },
    { "en": "Apa Itu Integrasi Pulsa?", "id": "Menjumlahkan Beberapa Gema Untuk Meningkatkan SNR." },
    { "en": "Apa Itu Integrasi Koheren?", "id": "Integrasi Dengan Informasi Fasa." },
    { "en": "Apa Itu Integrasi Non-koheren?", "id": "Integrasi Tanpa Informasi Fasa." },
    { "en": "Apa Itu Kompresi Pulsa?", "id": "Meningkatkan Resolusi Jarak Tanpa Mengurangi Energi." },
    { "en": "Apa Itu Sinyal Chirp?", "id": "Pulsa Dengan Frekuensi Yang Berubah." },
    { "en": "Apa Itu Kode Barker?", "id": "Kode Biner Dengan Sifat Autokorelasi Baik." },
    { "en": "Apa Itu Kode Polifase?", "id": "Kode Dengan Banyak Fasa." },
    { "en": "Apa Itu Fungsi Ambiguitas?", "id": "Menggambarkan Resolusi Radar Dalam Jarak-Doppler." },
    { "en": "Apa Itu Radar Array Bertahap?", "id": "Phased Array Radar." },
    { "en": "Apa Itu Pemindaian Elektronik?", "id": "Menggerakkan Berkas Radar Tanpa Gerakan Mekanis." },
    { "en": "Apa Itu Modul Transmit/Receive?", "id": "T/R (Transmit/Receive) Module." },
    { "en": "Apa Itu Radar Apertur Sintetis?", "id": "SAR (Synthetic Aperture Radar)." },
    { "en": "Apa Fungsi SAR (Synthetic Aperture Radar)?", "id": "Menghasilkan Gambar Permukaan Resolusi Tinggi." },
    { "en": "Apa Itu Radar Apertur Sintetis Terbalik?", "id": "ISAR (Inverse Synthetic Aperture Radar)." },
    { "en": "Apa Itu Radar Penembus Tanah?", "id": "GPR (Ground-Penetrating Radar)." },
    { "en": "Apa Itu Radar Cuaca?", "id": "Mendeteksi Presipitasi Dan Angin." },
    { "en": "Apa Itu Reflektivitas (Radar Cuaca)?", "id": "Ukuran Intensitas Hujan." },
    { "en": "Apa Itu Radar Doppler?", "id": "Mengukur Kecepatan Radial." },
    { "en": "Apa Itu Radar Polarisasi Ganda?", "id": "Memancarkan Gelombang Polarisasi Horisontal-Vertikal." },
    { "en": "Apa Itu Profil Angin?", "id": "Mengukur Kecepatan Dan Arah Angin." },
    { "en": "Apa Itu Lidar (Light Detection and Ranging)?", "id": "Radar Menggunakan Cahaya (Laser)." },
    { "en": "Apa Itu Sonar (Sound Navigation and Ranging)?", "id": "Sistem Akustik Bawah Air." },
    { "en": "Apa Itu Sonar Aktif?", "id": "Memancarkan Pulsa Suara (Ping)." },
    { "en": "Apa Itu Sonar Pasif?", "id": "Hanya Mendengarkan Suara." },
    { "en": "Apa Itu Akustik Bawah Air?", "id": "Studi Propagasi Suara Di Air." },
    { "en": "Apa Itu Saluran Suara SOFAR?", "id": "Lapisan Di Laut Memandu Suara Jarak Jauh." },
    { "en": "Apa Itu Reverberasi (Sonar)?", "id": "Gema Akustik Tak Diinginkan." },
    { "en": "Apa Itu Kekuatan Target (Sonar)?", "id": "Ukuran Seberapa Baik Target Memantulkan Suara." },
    { "en": "Apa Itu Tingkat Kebisingan Sekitar?", "id": "Derau Latar Belakang Di Laut." },
    { "en": "Apa Itu Persamaan Sonar?", "id": "Analisis Anggaran Daya Untuk Sonar." },
    { "en": "Apa Itu Side-Scan Sonar?", "id": "Mencitrakan Dasar Laut." },
    { "en": "Apa Itu Multibeam Echosounder?", "id": "Memetakan Batimetri Dasar Laut." },
    { "en": "Apa Itu Profil Arus Doppler Akustik?", "id": "ADCP (Acoustic Doppler Current Profiler)." },
    { "en": "Apa Itu Peperangan Elektronik?", "id": "EW (Electronic Warfare)." },
    { "en": "Apa Itu Dukungan Elektronik?", "id": "ESM (Electronic Support Measures)." },
    { "en": "Apa Itu Serangan Elektronik?", "id": "EA (Electronic Attack)." },
    { "en": "Apa Itu Proteksi Elektronik?", "id": "EP (Electronic Protection)." },
    { "en": "Apa Itu Jamming?", "id": "Mengganggu Sinyal Komunikasi Atau Radar." },
    { "en": "Apa Itu Deception Jamming?", "id": "Mengirim Sinyal Palsu Untuk Menipu." },
    { "en": "Apa Itu Noise Jamming?", "id": "Menutupi Sinyal Dengan Derau." },
    { "en": "Apa Itu Chaff?", "id": "Material Reflektif Untuk Membingungkan Radar." },
    { "en": "Apa Itu Flare?", "id": "Sumber Panas Untuk Mengelabui Rudal Inframerah." },
    { "en": "Apa Itu Teknologi Siluman (Stealth)?", "id": "Mengurangi Penampang Lintang Radar (RCS)." },
    { "en": "Apa Itu Intelijen Sinyal?", "id": "SIGINT (Signals Intelligence)." },
    { "en": "Apa Itu Intelijen Komunikasi?", "id": "COMINT (Communications Intelligence)." },
    { "en": "Apa Itu Intelijen Elektronik?", "id": "ELINT (Electronic Intelligence)." },
    { "en": "Apa Itu Pencarian Arah?", "id": "DF (Direction Finding)." },
    { "en": "Apa Itu Navigasi?", "id": "Proses Menentukan Posisi Dan Arah." },
    { "en": "Apa Itu Dead Reckoning?", "id": "Navigasi Berdasarkan Kecepatan Dan Arah." },
    { "en": "Apa Itu Navigasi Inersia?", "id": "Menggunakan Akselerometer Dan Giroskop." },
    { "en": "Apa Itu Sistem Navigasi Satelit Global?", "id": "GNSS (Global Navigation Satellite System)." },
    { "en": "Contoh GNSS?", "id": "GPS, GLONASS, Galileo, BeiDou." },
    { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem Navigasi Milik Amerika Serikat." },
    { "en": "Apa Itu Trilaterasi?", "id": "Menentukan Posisi Dari Jarak Ke Tiga Titik." },
    { "en": "Apa Itu Pseudorange?", "id": "Ukuran Jarak Semu Ke Satelit." },
    { "en": "Apa Itu Kesalahan Jam Satelit?", "id": "Ketidakakuratan Jam Di Satelit." },
    { "en": "Apa Itu Kesalahan Jam Penerima?", "id": "Ketidakakuratan Jam Di Penerima." },
    { "en": "Apa Itu Penundaan Ionosfer?", "id": "Kesalahan Akibat Sinyal Melewati Ionosfer." },
    { "en": "Apa Itu Penundaan Troposfer?", "id": "Kesalahan Akibat Sinyal Melewati Troposfer." },
    { "en": "Apa Itu Kesalahan Multipath?", "id": "Kesalahan Akibat Sinyal Pantulan." },
    { "en": "Apa Itu Dilution of Precision?", "id": "DOP (Dilution of Precision)." },
    { "en": "Apa Itu GPS Diferensial?", "id": "DGPS (Differential GPS)." },
    { "en": "Apa Fungsi DGPS?", "id": "Meningkatkan Akurasi Posisi." },
    { "en": "Apa Itu Real-Time Kinematic?", "id": "RTK (Real-Time Kinematic)." },
    { "en": "Apa Akurasi RTK?", "id": "Akurasi Tingkat Sentimeter." },
    { "en": "Apa Itu Sistem Augmentasi Berbasis Satelit?", "id": "SBAS (Satellite-Based Augmentation System)." },
    { "en": "Contoh SBAS?", "id": "WAAS (Wide Area Augmentation System)." },
    { "en": "Apa Itu VOR (VHF Omnidirectional Range)?", "id": "Sistem Navigasi Radio Penerbangan." },
    { "en": "Apa Itu DME (Distance Measuring Equipment)?", "id": "Mengukur Jarak Ke Stasiun." },
    { "en": "Apa Itu TACAN (Tactical Air Navigation)?", "id": "Sistem Navigasi Militer." },
    { "en": "Apa Itu LORAN-C (Long Range Navigation)?", "id": "Sistem Navigasi Radio Frekuensi Rendah." },
    { "en": "Apa Itu Sistem Pendaratan Instrumen?", "id": "ILS (Instrument Landing System)." },
    { "en": "Apa Itu Localizer?", "id": "Memberikan Panduan Horizontal." },
    { "en": "Apa Itu Glide Slope?", "id": "Memberikan Panduan Vertikal." },
    { "en": "Apa Itu Penanda Suar (Marker Beacon)?", "id": "Memberikan Indikasi Jarak Ke Landasan." },
    { "en": "Apa Itu Pencari Arah Otomatis?", "id": "ADF (Automatic Direction Finder)." },
    { "en": "Apa Itu Pemancar Suar Non-direksional?", "id": "NDB (Non-Directional Beacon)." },
    { "en": "Apa Itu Sistem Pencegahan Tabrakan Lalu Lintas?", "id": "TCAS (Traffic Collision Avoidance System)." },
    { "en": "Apa Itu Pengawasan Dependen Otomatis–Siaran?", "id": "ADS-B (Automatic Dependent Surveillance–Broadcast)." },
    { "en": "Apa Itu Sistem Manajemen Penerbangan?", "id": "FMS (Flight Management System)." },
    { "en": "Apa Itu Transponder (Penerbangan)?", "id": "Merespon Sinyal Radar Sekunder." },
    { "en": "Apa Itu Radar Sekunder?", "id": "SSR (Secondary Surveillance Radar)." },
    { "en": "Apa Itu Mode A?", "id": "Kode Identifikasi Pesawat (Squawk)." },
    { "en": "Apa Itu Mode C?", "id": "Melaporkan Ketinggian Tekanan." },
    { "en": "Apa Itu Mode S?", "id": "Transponder Dengan Komunikasi Data Dua Arah." },
    { "en": "Apa Itu Fisika Medis?", "id": "Aplikasi Fisika Dalam Kedokteran." },
    { "en": "Apa Itu Radioterapi?", "id": "Penggunaan Radiasi Untuk Mengobati Kanker." },
    { "en": "Apa Itu Akselerator Linear Medis?", "id": "LINAC (Linear Accelerator)." },
    { "en": "Apa Itu Brakiterapi?", "id": "Menempatkan Sumber Radioaktif Dekat Tumor." },
    { "en": "Apa Itu Terapi Proton?", "id": "Menggunakan Berkas Proton Untuk Radioterapi." },
    { "en": "Apa Itu Kedokteran Nuklir?", "id": "Penggunaan Radioisotop Untuk Diagnosis/Terapi." },
    { "en": "Apa Itu Radiofarmaka?", "id": "Obat Radioaktif." },
    { "en": "Apa Itu Kamera Gamma?", "id": "Mencitrakan Distribusi Radiofarmaka." },
    { "en": "Apa Itu Tomografi Emisi Positron?", "id": "PET (Positron Emission Tomography)." },
    { "en": "Apa Itu Tomografi Komputasi Emisi Foton Tunggal?", "id": "SPECT (Single-Photon Emission Computed Tomography)." },
    { "en": "Apa Itu Pencitraan Resonansi Magnetik?", "id": "MRI (Magnetic Resonance Imaging)." },
    { "en": "Apa Itu Urutan Pulsa (MRI)?", "id": "Urutan Pulsa RF Dan Gradien." },
    { "en": "Apa Itu Waktu Relaksasi T1?", "id": "Pemulihan Magnetisasi Longitudinal." },
    { "en": "Apa Itu Waktu Relaksasi T2?", "id": "Peluruhan Magnetisasi Transversal." },
    { "en": "Apa Itu Pencitraan Tertimbang Difusi?", "id": "DWI (Diffusion-Weighted Imaging)." },
    { "en": "Apa Itu MRI Fungsional?", "id": "fMRI (Functional MRI)." },
    { "en": "Apa Fungsi fMRI?", "id": "Memetakan Aktivitas Otak." },
    { "en": "Apa Itu Gradien Medan Magnet?", "id": "Variasi Spasial Medan Magnet." },
    { "en": "Apa Itu Koil RF?", "id": "Memancarkan Dan Menerima Sinyal Radio." },
    { "en": "Apa Itu Pencitraan Ultrasonik?", "id": "Menggunakan Gelombang Suara Frekuensi Tinggi." },
    { "en": "Apa Itu Transduser Ultrasonik?", "id": "Mengubah Listrik Menjadi Suara." },
    { "en": "Apa Itu Efek Piezoelektrik?", "id": "Dasar Dari Transduser Ultrasonik." },
    { "en": "Apa Itu Mode-A (Amplitudo)?", "id": "Menampilkan Amplitudo Gema." },
    { "en": "Apa Itu Mode-B (Kecerahan)?", "id": "Menampilkan Gema Sebagai Titik Kecerahan." },
    { "en": "Apa Itu Mode-M (Gerakan)?", "id": "Menampilkan Gerakan Struktur." },
    { "en": "Apa Itu Pencitraan Doppler?", "id": "Memvisualisasikan Aliran Darah." },
    { "en": "Apa Itu Elastografi?", "id": "Memetakan Kekakuan Jaringan." },
    { "en": "Apa Itu Agen Kontras Ultrasonik?", "id": "Gelembung Mikro Untuk Meningkatkan Gema." },
    { "en": "Apa Itu Dosimetri?", "id": "Pengukuran Dosis Radiasi." },
    { "en": "Apa Itu Gray (Gy)?", "id": "Satuan Dosis Serap." },
    { "en": "Apa Itu Sievert (Sv)?", "id": "Satuan Dosis Ekuivalen." },
    { "en": "Apa Itu Kamar Ionisasi?", "id": "Detektor Radiasi Berisi Gas." },
    { "en": "Apa Itu Termoluminesen Dosimeter?", "id": "TLD (Thermoluminescent Dosimeter)." },
    { "en": "Apa Itu Rencana Perawatan (Radioterapi)?", "id": "Perencanaan Distribusi Dosis Radiasi." },
    { "en": "Apa Itu Kolimator Multi-daun?", "id": "MLC (Multi-Leaf Collimator)." },
    { "en": "Apa Fungsi MLC?", "id": "Membentuk Berkas Radiasi." },
    { "en": "Apa Itu Radiografi Komputasi?", "id": "CR (Computed Radiography)." },
    { "en": "Apa Itu Radiografi Digital?", "id": "DR (Digital Radiography)." },
    { "en": "Apa Itu Mamografi?", "id": "Pencitraan Sinar-X Payudara." },
    { "en": "Apa Itu Fluoroskopi?", "id": "Pencitraan Sinar-X Real-time." },
    { "en": "Apa Itu Angiografi?", "id": "Pencitraan Pembuluh Darah." },
    { "en": "Apa Itu Agen Kontras?", "id": "Bahan Untuk Meningkatkan Visibilitas." },
    { "en": "Apa Itu Sistem Akuisisi Gambar Dan Komunikasi?", "id": "PACS (Picture Archiving and Communication System)." },
    { "en": "Apa Itu DICOM (Digital Imaging and Communications in Medicine)?", "id": "Standar Untuk Gambar Medis." },
    { "en": "Apa Itu Bioelektromagnetisme?", "id": "Studi Interaksi Medan EM Dengan Sistem Biologis." },
    { "en": "Apa Itu Elektroensefalografi?", "id": "EEG (Electroencephalography)." },
    { "en": "Apa Itu Elektrokardiografi?", "id": "ECG (Electrocardiography)." },
    { "en": "Apa Itu Elektromiografi?", "id": "EMG (Electromyography)." },
    { "en": "Apa Itu Elektrookulografi?", "id": "EOG (Electrooculography)." },
    { "en": "Apa Itu Elektroretinografi?", "id": "ERG (Electroretinography)." },
    { "en": "Apa Itu Magnetoensefalografi?", "id": "MEG (Magnetoencephalography)." },
    { "en": "Apa Itu Stimulasi Magnetik Transkranial?", "id": "TMS (Transcranial Magnetic Stimulation)." },
    { "en": "Apa Itu Terapi Kejut Listrik?", "id": "ECT (Electroconvulsive Therapy)." },
    { "en": "Apa Itu Ablasi Frekuensi Radio?", "id": "Menggunakan Panas RF Untuk Merusak Jaringan." },
    { "en": "Apa Itu Diatermi?", "id": "Pemanasan Jaringan Dalam Dengan Frekuensi Tinggi." },
    { "en": "Apa Itu Impedansi Bioelektrik?", "id": "Mengukur Komposisi Tubuh." },
    { "en": "Apa Itu Teknik Biomedis?", "id": "Aplikasi Teknik Dalam Kedokteran." },
    { "en": "Apa Itu Rekayasa Klinis?", "id": "Manajemen Teknologi Medis Di Rumah Sakit." },
    { "en": "Apa Itu Informatika Medis?", "id": "Manajemen Informasi Kesehatan." },
    { "en": "Apa Itu Rekayasa Jaringan?", "id": "Menumbuhkan Jaringan Biologis." },
    { "en": "Apa Itu Biomaterial?", "id": "Material Yang Berinteraksi Dengan Sistem Biologis." },
    { "en": "Apa Itu Biomekanika?", "id": "Studi Mekanika Sistem Biologis." },
    { "en": "Apa Itu Implan?", "id": "Perangkat Buatan Yang Ditanam Di Tubuh." },
    { "en": "Apa Itu Prostesis?", "id": "Pengganti Bagian Tubuh Yang Hilang." },
    { "en": "Apa Itu Alat Pacu Jantung?", "id": "Menghasilkan Impuls Listrik Untuk Jantung." },
    { "en": "Apa Itu Defibrilator Implan?", "id": "ICD (Implantable Cardioverter-Defibrillator)." },
    { "en": "Apa Itu Implan Koklea?", "id": "Alat Bantu Dengar Elektronik." },
    { "en": "Apa Itu Stimulator Saraf Vagus?", "id": "VNS (Vagus Nerve Stimulation)." },
    { "en": "Apa Itu Stimulasi Otak Dalam?", "id": "DBS (Deep Brain Stimulation)." },
    { "en": "Apa Itu Jantung Buatan?", "id": "Perangkat Yang Menggantikan Fungsi Jantung." },
    { "en": "Apa Itu Mesin Dialisis?", "id": "Membersihkan Darah Secara Buatan." },
    { "en": "Apa Itu Pompa Infus?", "id": "Mengirimkan Cairan Ke Tubuh Pasien." },
    { "en": "Apa Itu Ventilator?", "id": "Alat Bantu Pernapasan Mekanis." },
    { "en": "Apa Itu Mesin Anestesi?", "id": "Memberikan Gas Anestesi." },
    { "en": "Apa Itu Unit Bedah Listrik?", "id": "ESU (Electrosurgical Unit)." },
    { "en": "Apa Itu Pemantauan Pasien?", "id": "Mengukur Tanda-tanda Vital." },
    { "en": "Apa Itu Oksimetri Nadi?", "id": "Mengukur Saturasi Oksigen." },
    { "en": "Apa Itu Kapnografi?", "id": "Mengukur Karbon Dioksida." },
    { "en": "Apa Itu Telemedis?", "id": "Pemberian Layanan Kesehatan Jarak Jauh." },
    { "en": "Apa Itu Rekam Medis Elektronik?", "id": "EMR (Electronic Medical Record)." },
    { "en": "Apa Itu Robot Bedah?", "id": "Membantu Ahli Bedah Dalam Operasi." },
    { "en": "Apa Itu Pencetakan 3D Medis?", "id": "Membuat Model Anatomi Dan Implan." },
    { "en": "Apa Itu Rekayasa Rehabilitasi?", "id": "Teknologi Untuk Penyandang Disabilitas." },
    { "en": "Apa Itu Antarmuka Otak-Komputer?", "id": "BCI (Brain-Computer Interface)." },
    { "en": "Apa Itu Teknologi Bantu?", "id": "Perangkat Untuk Membantu Aktivitas Sehari-hari." },
    { "en": "Apa Itu Robotika Medis?", "id": "Penggunaan Robot Dalam Perawatan Kesehatan." },
    { "en": "Apa Itu Otomasi Laboratorium?", "id": "Menggunakan Robot Untuk Tugas Laboratorium." },
    { "en": "Apa Itu Sistem Pengiriman Obat?", "id": "Mengontrol Pelepasan Obat Di Tubuh." },
    { "en": "Apa Itu Biosensor?", "id": "Sensor Yang Menggunakan Komponen Biologis." },
    { "en": "Apa Itu BioMEMS?", "id": "Sistem Mikro-Elektro-Mekanis Untuk Biologi." },
    { "en": "Apa Itu Mikrofluida?", "id": "Manipulasi Cairan Dalam Saluran Mikro." },
    { "en": "Apa Itu Lab-on-a-Chip?", "id": "Integrasi Fungsi Lab Dalam Satu Chip." },
    { "en": "Apa Itu Bioinformatika?", "id": "Analisis Data Biologis Dengan Komputasi." },
    { "en": "Apa Itu Pemodelan Fisiologis?", "id": "Simulasi Matematis Sistem Biologis." },
    { "en": "Apa Itu Pemrosesan Sinyal Biomedis?", "id": "Analisis Sinyal (EEG, ECG)." },
    { "en": "Apa Itu Pemrosesan Gambar Medis?", "id": "Analisis Gambar (CT, MRI)." },
    { "en": "Apa Itu Standar Keamanan Peralatan Medis?", "id": "IEC 60601." },
    { "en": "Apa Itu Kompatibilitas Biologis?", "id": "Material Tidak Menyebabkan Reaksi Buruk." },
    { "en": "Apa Itu Sterilisasi?", "id": "Menghilangkan Semua Mikroorganisme." },
    { "en": "Apa Itu Disinfeksi?", "id": "Mengurangi Jumlah Mikroorganisme." },
    { "en": "Apa Itu Faktor Manusia?", "id": "Studi Interaksi Manusia-Mesin." },
    { "en": "Apa Itu Ergonomi?", "id": "Desain Untuk Kemudahan Dan Kenyamanan Penggunaan." },
    { "en": "Apa Itu Kegunaan (Usability)?", "id": "Kemudahan Penggunaan Suatu Perangkat." },
    { "en": "Apa Itu Uji Klinis?", "id": "Pengujian Perangkat Medis Pada Manusia." },
    { "en": "Apa Itu Persetujuan Regulasi?", "id": "Izin Dari Otoritas (FDA, CE)." },
    { "en": "Apa Itu Perangkat Lunak Sebagai Alat Medis?", "id": "SaMD (Software as a Medical Device)." },
    { "en": "Apa Itu Keamanan Siber Medis?", "id": "Melindungi Perangkat Medis Dari Peretasan." },
    { "en": "Apa Itu Interoperabilitas?", "id": "Kemampuan Perangkat Berbeda Berkomunikasi." },
    { "en": "Apa Itu Standar HL7 (Health Level Seven)?", "id": "Standar Pertukaran Data Kesehatan." },
    { "en": "Apa Itu Sistem Informasi Rumah Sakit?", "id": "HIS (Hospital Information System)." },
    { "en": "Apa Itu Sistem Informasi Radiologi?", "id": "RIS (Radiology Information System)." },
    { "en": "Apa Itu Sistem Informasi Laboratorium?", "id": "LIS (Laboratory Information System)." },
    { "en": "Apa Itu Wearable Technology?", "id": "Teknologi Yang Dapat Dikenakan." },
    { "en": "Apa Itu Sensor Dapat Dikenakan?", "id": "Memantau Tanda Vital Secara Terus Menerus." },
    { "en": "Apa Itu Elektronik Kulit?", "id": "Sirkuit Fleksibel Yang Menempel Di Kulit." },
    { "en": "Apa Itu Implan Cerdas?", "id": "Implan Dengan Kemampuan Sensor Dan Komunikasi." },
    { "en": "Apa Itu Pil Cerdas?", "id": "Kapsul Yang Dapat Ditelan Dengan Sensor." },
    { "en": "Apa Itu Terapi Gen?", "id": "Memodifikasi Gen Untuk Mengobati Penyakit." },
    { "en": "Apa Itu CRISPR?", "id": "Teknologi Penyuntingan Gen." },
    { "en": "Apa Itu Organ-on-a-Chip?", "id": "Mensimulasikan Fungsi Organ Manusia." },
    { "en": "Apa Itu Kecerdasan Buatan Dalam Kedokteran?", "id": "AI (Artificial Intelligence) Untuk Diagnosis/Pengobatan." },
    { "en": "Apa Itu Pembelajaran Mesin?", "id": "Machine Learning." },
    { "en": "Apa Itu Pembelajaran Mendalam?", "id": "Deep Learning." },
    { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "ANN (Artificial Neural Network)." },
    { "en": "Apa Itu Visi Komputer?", "id": "Computer Vision." },
    { "en": "Apa Itu Pemrosesan Bahasa Alami?", "id": "NLP (Natural Language Processing)." },
    { "en": "Apa Itu Analitik Prediktif?", "id": "Memprediksi Hasil Kesehatan." },
    { "en": "Apa Itu Sistem Pendukung Keputusan Klinis?", "id": "CDSS (Clinical Decision Support System)." },
    { "en": "Apa Itu Robotika Rehabilitasi?", "id": "Robot Untuk Membantu Terapi Fisik." },
    { "en": "Apa Itu Exoskeleton?", "id": "Rangka Luar Robotik." },
    { "en": "Apa Itu Bionik?", "id": "Integrasi Biologi Dan Elektronik." },
    { "en": "Apa Itu Optogenetik?", "id": "Mengontrol Neuron Dengan Cahaya." },
    { "en": "Apa Itu Sonogenetik?", "id": "Mengontrol Neuron Dengan Suara." },
    { "en": "Apa Itu Terapi Fotodinamik?", "id": "PDT (Photodynamic Therapy)." },
    { "en": "Apa Itu Litotripsi?", "id": "Menghancurkan Batu Ginjal Dengan Gelombang Kejut." },
    { "en": "Apa Itu Kauterisasi?", "id": "Menggunakan Panas Untuk Menghentikan Pendarahan." },
    { "en": "Apa Itu Cryosurgery?", "id": "Menggunakan Suhu Sangat Dingin." },
    { "en": "Apa Itu Elektroporasi?", "id": "Menggunakan Medan Listrik Untuk Membuka Membran Sel." },
    { "en": "Apa Itu Iontoforesis?", "id": "Mengirim Obat Melalui Kulit Dengan Arus." },
    { "en": "Apa Itu Fonoforesis?", "id": "Mengirim Obat Dengan Gelombang Ultrasonik." },
    { "en": "Apa Itu Terapi Hipertermia?", "id": "Menggunakan Panas Untuk Mengobati Kanker." },
    { "en": "Apa Itu Ablasi Laser?", "id": "Menggunakan Laser Untuk Menghilangkan Jaringan." },
    { "en": "Apa Itu Pisau Gamma?", "id": "Alat Bedah Radio Stereotaktik." },
    { "en": "Apa Itu CyberKnife?", "id": "Sistem Bedah Radio Robotik." },
    { "en": "Apa Itu Sistem Navigasi Bedah?", "id": "Membantu Pemanduan Selama Operasi." },
    { "en": "Apa Itu Pencitraan Terpandu?", "id": "Menggunakan Pencitraan Real-time Selama Prosedur." },
    { "en": "Apa Itu Ruang Operasi Hibrida?", "id": "Menggabungkan Ruang Operasi Dengan Pencitraan." },
    { "en": "Apa Itu Realitas Tertambah Dalam Bedah?", "id": "Menampilkan Informasi Digital Di Atas Pasien." },
    { "en": "Apa Itu Realitas Virtual Dalam Pelatihan Medis?", "id": "Simulasi Prosedur Bedah." },
    { "en": "Apa Itu Pencetakan 3D Anatomi?", "id": "Membuat Model Organ Untuk Perencanaan Bedah." },
    { "en": "Apa Itu Instrumen Bedah Cerdas?", "id": "Instrumen Dengan Sensor Terintegrasi." },
    { "en": "Apa Itu Kapsul Endoskopi?", "id": "Kamera Nirkabel Yang Dapat Ditelan." },
    { "en": "Apa Itu Laboratorium Kateterisasi Jantung?", "id": "Cath Lab." },
    { "en": "Apa Itu Angioplasti?", "id": "Membuka Pembuluh Darah Yang Tersumbat." },
    { "en": "Apa Itu Stent?", "id": "Tabung Jala Untuk Menjaga Pembuluh Tetap Terbuka." },
    { "en": "Apa Itu Mesin Jantung-Paru?", "id": "Mengambil Alih Fungsi Jantung-Paru." },
    { "en": "Apa Itu Oksigenator Membran?", "id": "Paru-paru Buatan." },
    { "en": "Apa Itu Pompa Darah?", "id": "Menggantikan Fungsi Pompa Jantung." },
    { "en": "Apa Itu Alat Bantu Ventrikel?", "id": "VAD (Ventricular Assist Device)." },
    { "en": "Apa Itu Kontrol Loop Tertutup Dalam Medis?", "id": "Contoh: Pompa Insulin Otomatis." },
    { "en": "Apa Itu Pankreas Buatan?", "id": "Sistem Loop Tertutup Untuk Diabetes." },
    { "en": "Apa Itu Pemantauan Glukosa Berkelanjutan?", "id": "CGM (Continuous Glucose Monitoring)." },
    { "en": "Apa Itu Pompa Insulin?", "id": "Memberikan Insulin Secara Terus Menerus." },
    { "en": "Apa Itu Elektrokardiogram Holter?", "id": "Perekaman EKG Jangka Panjang." },
    { "en": "Apa Itu Pemantau Tekanan Darah Ambulatori?", "id": "Perekaman Tekanan Darah 24 Jam." },
    { "en": "Apa Itu Analisis Variabilitas Detak Jantung?", "id": "HRV (Heart Rate Variability)." },
    { "en": "Apa Itu Analisis Gait?", "id": "Mempelajari Pola Berjalan." },
    { "en": "Apa Itu Platform Gaya?", "id": "Mengukur Gaya Reaksi Tanah." },
    { "en": "Apa Itu Sistem Penangkap Gerak?", "id": "Motion Capture (MoCap)." },
    { "en": "Apa Itu Elektrogoniometer?", "id": "Mengukur Sudut Sendi." },
    { "en": "Apa Itu Dinamometer?", "id": "Mengukur Kekuatan Otot." },
    { "en": "Apa Itu Spirometer?", "id": "Mengukur Fungsi Paru-paru." },
    { "en": "Apa Itu Audiometer?", "id": "Menguji Kemampuan Pendengaran." },
    { "en": "Apa Itu Timpanometer?", "id": "Menguji Kondisi Telinga Tengah." },
    { "en": "Apa Itu Tonometer?", "id": "Mengukur Tekanan Intraokular." },
    { "en": "Apa Itu Keratometer?", "id": "Mengukur Kelengkungan Kornea." },
    { "en": "Apa Itu Funduskopi?", "id": "Melihat Bagian Dalam Mata." },
    { "en": "Apa Itu Dermatoskopi?", "id": "Memeriksa Lesi Kulit." },
    { "en": "Apa Itu Kolposkopi?", "id": "Memeriksa Serviks." }



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

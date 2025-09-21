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


  { "en": "Apa Ide Utama Fourier?", "id": "Semua Sinyal Adalah Jumlahan Sinusoid." },
  { "en": "Siapa Penemu Analisis Fourier?", "id": "Jean-Baptiste Joseph Fourier." },
  { "en": "Apa Itu Domain Waktu?", "id": "Representasi Sinyal Sebagai Amplitudo Versus Waktu." },
  { "en": "Apa Itu Domain Frekuensi?", "id": "Representasi Sinyal Sebagai Amplitudo Versus Frekuensi." },
  { "en": "Jembatan Antar Dua Domain?", "id": "Transformasi Fourier Dan Inversnya." },
  { "en": "Apa Itu Sinyal Periodik?", "id": "Sinyal Yang Berulang Dalam Pola Tetap." },
  { "en": "Alat Untuk Sinyal Periodik?", "id": "Deret Fourier (Fourier Series)." },
  { "en": "Apa Itu Sinyal Aperiodik?", "id": "Sinyal Yang Tidak Berulang." },
  { "en": "Alat Untuk Sinyal Aperiodik?", "id": "Transformasi Fourier (Fourier Transform)." },
  { "en": "Apa Komponen Dasar Sinyal?", "id": "Gelombang Sinus Dan Kosinus Sederhana." },
  { "en": "Apa Itu Frekuensi Dasar (F0)?", "id": "Frekuensi Terendah Dari Sinyal Periodik." },
  { "en": "Apa Itu Harmonik?", "id": "Kelipatan Bulat Dari Frekuensi Dasar." },
  { "en": "Harmonik Kedua Berarti?", "id": "Dua Kali Frekuensi Dasar." },
  { "en": "Deret Fourier Merepresentasikan Sinyal Sebagai?", "id": "Jumlah Frekuensi Dasar Dan Harmoniknya." },
  { "en": "Apa Itu Koefisien Fourier?", "id": "Amplitudo Dan Fasa Setiap Komponen Harmonik." },
  { "en": "Koefisien 'a0' Mewakili Apa?", "id": "Komponen DC Atau Nilai Rata-rata Sinyal." },
  { "en": "Koefisien 'an' Mewakili Apa?", "id": "Amplitudo Dari Komponen Kosinus." },
  { "en": "Koefisien 'bn' Mewakili Apa?", "id": "Amplitudo Dari Komponen Sinus." },
  { "en": "Apa Itu Spektrum?", "id": "Plot Koefisien Fourier Terhadap Frekuensi." },
  { "en": "Apa Itu Spektrum Amplitudo?", "id": "Plot Amplitudo Terhadap Frekuensi." },
  { "en": "Apa Itu Spektrum Fasa?", "id": "Plot Fasa Terhadap Frekuensi." },
  { "en": "Spektrum Sinyal Periodik Berbentuk?", "id": "Garis-Garis Diskrit Pada Frekuensi Harmonik." },
  { "en": "Spektrum Sinyal Aperiodik Berbentuk?", "id": "Kurva Kontinu Di Domain Frekuensi." },
  { "en": "Apa Bentuk Eksponensial Deret Fourier?", "id": "Menggunakan Eksponensial Kompleks (e^jωt)." },
  { "en": "Koefisien Bentuk Eksponensial?", "id": "Koefisien Kompleks 'Cn'." },
  { "en": "Apa Keuntungan Bentuk Eksponensial?", "id": "Lebih Ringkas Dan Mudah Dimanipulasi." },
  { "en": "Apa Hubungan 'Cn' Dengan 'an' 'bn'?", "id": "Cn Mengandung Informasi Amplitudo Dan Fasa." },
  { "en": "Apa Itu Frekuensi Negatif?", "id": "Konstruk Matematis Dari Bentuk Eksponensial." },
  { "en": "Apa Itu Simetri Hermite?", "id": "Sifat Spektrum Untuk Sinyal Bernilai Riil." },
  { "en": "Magnitudo Spektrum Sinyal Riil?", "id": "Selalu Simetris Genap." },
  { "en": "Fasa Spektrum Sinyal Riil?", "id": "Selalu Simetris Ganjil." },
  { "en": "Apa Itu Transformasi Fourier?", "id": "Deret Fourier Untuk Sinyal Dengan Periode Tak Hingga." },
  { "en": "Hasil Transformasi Fourier?", "id": "Fungsi Kontinu Di Domain Frekuensi." },
  { "en": "Apa Itu Sifat Linearitas?", "id": "Transformasi Jumlah Adalah Jumlah Transformasi." },
  { "en": "Apa Itu Sifat Pergeseran Waktu?", "id": "Penundaan Di Waktu Menjadi Pergeseran Fasa." },
  { "en": "Apa Itu Sifat Penskalaan Waktu?", "id": "Kompresi Di Waktu Menjadi Ekspansi Di Frekuensi." },
  { "en": "Apa Itu Sifat Dualitas?", "id": "Bentuk Transformasi Mirip Dengan Inversnya." },
  { "en": "Apa Itu Teorema Konvolusi?", "id": "Konvolusi Di Waktu Menjadi Perkalian Di Frekuensi." },
  { "en": "Kenapa Teorema Konvolusi Penting?", "id": "Sangat Menyederhanakan Analisis Sistem LTI." },
  { "en": "Apa Itu Sistem LTI?", "id": "Sistem Linier Invarian Waktu." },
  { "en": "Output Sistem LTI Di Waktu?", "id": "Konvolusi Input Dengan Respon Impuls." },
  { "en": "Output Sistem LTI Di Frekuensi?", "id": "Perkalian Spektrum Input Dengan Respon Frekuensi." },
  { "en": "Apa Itu Respon Impuls?", "id": "Deskripsi Sistem Di Domain Waktu." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Deskripsi Sistem Di Domain Frekuensi." },
  { "en": "Keduanya Adalah Pasangan Transformasi?", "id": "Ya Pasangan Transformasi Fourier." },
  { "en": "Sinyal Kotak Di Waktu?", "id": "Spektrumnya Berbentuk Fungsi Sinc." },
  { "en": "Sinyal Impuls Di Waktu?", "id": "Spektrumnya Datar Di Semua Frekuensi." },
  { "en": "Sinyal Sinus Di Waktu?", "id": "Spektrumnya Dua Impuls Di Frekuensinya." },
  { "en": "Apa Itu Fungsi Sinc?", "id": "Fungsi Sin(x) Dibagi Dengan x." },
  { "en": "Sinyal Gaussian Di Waktu?", "id": "Spektrumnya Juga Berbentuk Gaussian." },
  { "en": "Apa Itu Teorema Parseval?", "id": "Energi Sinyal Sama Di Kedua Domain." },
  { "en": "Apa Itu Energi Sinyal?", "id": "Integral Kuadrat Dari Amplitudo Sinyal." },
  { "en": "Apa Itu Daya Sinyal?", "id": "Energi Rata-rata Sinyal Per Satuan Waktu." },
  { "en": "Apa Itu Kepadatan Spektral Energi (ESD)?", "id": "Distribusi Energi Sinyal Di Domain Frekuensi." },
  { "en": "Apa Itu Kepadatan Spektral Daya (PSD)?", "id": "Distribusi Daya Sinyal Di Domain Frekuensi." },
  { "en": "ESD Untuk Sinyal?", "id": "Sinyal Energi (Aperiodik)." },
  { "en": "PSD Untuk Sinyal?", "id": "Sinyal Daya (Periodik Atau Acak)." },
  { "en": "Apa Itu Sinyal Genap?", "id": "Sinyal Simetris Terhadap Sumbu Y." },
  { "en": "Transformasi Fourier Sinyal Genap?", "id": "Menghasilkan Spektrum Yang Bernilai Riil." },
  { "en": "Apa Itu Sinyal Ganjil?", "id": "Sinyal Anti-Simetris Terhadap Titik Asal." },
  { "en": "Transformasi Fourier Sinyal Ganjil?", "id": "Menghasilkan Spektrum Yang Bernilai Imajiner." },
  { "en": "Sinyal Kosinus Adalah Sinyal?", "id": "Sinyal Genap." },
  { "en": "Sinyal Sinus Adalah Sinyal?", "id": "Sinyal Ganjil." },
  { "en": "Apa Itu Sinyal Waktu Diskrit?", "id": "Sinyal Yang Didefinisikan Hanya Pada Sampel." },
  { "en": "Transformasi Untuk Sinyal Diskrit?", "id": "Transformasi Fourier Waktu Diskrit (DTFT)." },
  { "en": "Spektrum Hasil DTFT?", "id": "Kontinu Dan Periodik." },
  { "en": "Apa Itu Transformasi Fourier Diskrit (DFT)?", "id": "Versi Komputasi Dari Analisis Fourier." },
  { "en": "DFT Bekerja Pada Sinyal?", "id": "Sinyal Diskrit Dan Terbatas (Satu Blok)." },
  { "en": "Hasil DFT Adalah?", "id": "Spektrum Diskrit Dan Terbatas." },
  { "en": "Apa Itu FFT?", "id": "Algoritma Cepat Untuk Menghitung DFT." },
  { "en": "Kenapa FFT Sangat Penting?", "id": "Membuat Analisis Frekuensi Praktis Di Komputer." },
  { "en": "Apa Itu Bandwidth?", "id": "Lebar Pita Frekuensi Yang Digunakan Sinyal." },
  { "en": "Hubungan Durasi Dan Bandwidth?", "id": "Berbanding Terbalik (Prinsip Ketidakpastian)." },
  { "en": "Sinyal Pendek Di Waktu?", "id": "Memiliki Bandwidth Lebar Di Frekuensi." },
  { "en": "Sinyal Panjang Di Waktu?", "id": "Memiliki Bandwidth Sempit Di Frekuensi." },
  { "en": "Aplikasi Fourier Di Elektro?", "id": "Analisis Rangkaian Komunikasi Dan Pemrosesan Sinyal." },
  { "en": "Apa Itu Filter?", "id": "Sirkuit Yang Melewatkan Frekuensi Tertentu." },
  { "en": "Filter Ideal Dideskripsikan Di?", "id": "Domain Frekuensi (Bentuk Kotak)." },
  { "en": "Respon Impuls Filter Ideal?", "id": "Berbentuk Sinc Dan Non-Kausal." },
  { "en": "Apa Itu Filter Low-Pass?", "id": "Melewatkan Frekuensi Rendah." },
  { "en": "Apa Itu Filter High-Pass?", "id": "Melewatkan Frekuensi Tinggi." },
  { "en": "Apa Itu Filter Band-Pass?", "id": "Melewatkan Pita Frekuensi Tertentu." },
  { "en": "Apa Itu Modulasi?", "id": "Menggeser Spektrum Sinyal Ke Frekuensi Lain." },
  { "en": "Modulasi Adalah Perkalian Di?", "id": "Domain Waktu." },
  { "en": "Efeknya Di Domain Frekuensi?", "id": "Konvolusi Yang Menggeser Spektrum." },
  { "en": "Apa Itu Amplitudo Modulasi (AM)?", "id": "Informasi Ada Di Amplitudo Pembawa." },
  { "en": "Apa Itu Frekuensi Modulasi (FM)?", "id": "Informasi Ada Di Frekuensi Pembawa." },
  { "en": "Apa Itu Sampling?", "id": "Mengubah Sinyal Kontinu Menjadi Diskrit." },
  { "en": "Efek Sampling Di Frekuensi?", "id": "Membuat Spektrum Asli Berulang." },
  { "en": "Apa Itu Teorema Sampling Nyquist?", "id": "Frekuensi Sampling Harus Dua Kali Bandwidth." },
  { "en": "Jika Dilanggar Terjadi Apa?", "id": "Distorsi Aliasing." },
  { "en": "Aliasing Adalah?", "id": "Tumpang Tindih Spektrum Yang Berulang." },
  { "en": "Filter Anti-Aliasing Adalah?", "id": "Filter Low-Pass Sebelum Proses Sampling." },
  { "en": "Tujuannya Untuk?", "id": "Membatasi Bandwidth Sinyal Sebelum Sampling." },
  { "en": "Apa Itu Distorsi Harmonik?", "id": "Munculnya Harmonik Akibat Non-Linearitas." },
  { "en": "Distorsi Harmonik Dianalisis Di?", "id": "Domain Frekuensi." },
  { "en": "Apa Beda Deret Dan Transformasi Fourier?", "id": "Deret Untuk Periodik Transformasi Untuk Aperiodik." },
  { "en": "Spektrum Deret Fourier Berbentuk?", "id": "Garis-Garis Diskrit Yang Terpisah Jelas." },
  { "en": "Spektrum Transformasi Fourier Berbentuk?", "id": "Kurva Kontinu Yang Mulus." },
  { "en": "Apa Itu Sinyal Waktu Kontinu?", "id": "Sinyal Didefinisikan Di Setiap Momen Waktu." },
  { "en": "Apa Itu Sinyal Waktu Diskrit?", "id": "Sinyal Didefinisikan Hanya Pada Titik Waktu Tertentu." },
  { "en": "Deret Fourier Untuk Sinyal?", "id": "Sinyal Waktu Kontinu Dan Periodik." },
  { "en": "Transformasi Fourier Untuk Sinyal?", "id": "Sinyal Waktu Kontinu Dan Aperiodik." },
  { "en": "DTFT Untuk Sinyal?", "id": "Sinyal Waktu Diskrit Dan Aperiodik." },
  { "en": "DFT Untuk Sinyal?", "id": "Sinyal Waktu Diskrit Dan Periodik (Terbatas)." },
  { "en": "Mana Yang Digunakan Komputer?", "id": "DFT Dan Algoritma Cepatnya FFT." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Untuk Melihat Sinyal Di Domain Waktu." },
  { "en": "Apa Itu Spectrum Analyzer?", "id": "Alat Untuk Melihat Sinyal Di Domain Frekuensi." },
  { "en": "Operasi Konvolusi Di Waktu?", "id": "Menjadi Perkalian Sederhana Di Domain Frekuensi." },
  { "en": "Operasi Perkalian Di Waktu?", "id": "Menjadi Konvolusi Di Domain Frekuensi." },
  { "en": "Filtering Lebih Mudah Dilakukan Di?", "id": "Domain Frekuensi Dengan Operasi Perkalian." },
  { "en": "Sinyal Dirac Delta Di Waktu?", "id": "Mengandung Semua Frekuensi Dengan Amplitudo Sama." },
  { "en": "Sinyal DC Konstan Di Waktu?", "id": "Hanya Punya Energi Di Frekuensi Nol." },
  { "en": "Pergeseran Waktu Mempengaruhi Amplitudo?", "id": "Tidak Sama Sekali." },
  { "en": "Pergeseran Waktu Mempengaruhi Fasa?", "id": "Ya Mengubah Fasa Secara Linier." },
  { "en": "Mempercepat Sinyal Di Waktu?", "id": "Melebarkan Spektrumnya Di Domain Frekuensi." },
  { "en": "Memperlambat Sinyal Di Waktu?", "id": "Menyempitkan Spektrumnya Di Domain Frekuensi." },
  { "en": "Apa Arti Fisis Frekuensi?", "id": "Ukuran Seberapa Cepat Sinyal Berosilasi." },
  { "en": "Apa Arti Fisis Fasa?", "id": "Posisi Awal Gelombang Pada Waktu Nol." },
  { "en": "Apa Itu Koefisien DC?", "id": "Nilai Rata-rata Sinyal Di Domain Waktu." },
  { "en": "Gelombang Kotak Simetris Hanya Punya?", "id": "Komponen Harmonik Ganjil." },
  { "en": "Kenapa Harmonik Genap Hilang?", "id": "Karena Adanya Simetri Setengah Gelombang." },
  { "en": "Apa Itu Simetri Genap?", "id": "Bentuk Sinyal Dicerminkan Sumbu Vertikal." },
  { "en": "Spektrum Sinyal Genap?", "id": "Bernilai Riil Dan Tidak Punya Fasa." },
  { "en": "Apa Itu Simetri Ganjil?", "id": "Bentuk Sinyal Dicerminkan Terhadap Titik Asal." },
  { "en": "Spektrum Sinyal Ganjil?", "id": "Bernilai Imajiner Murni." },
  { "en": "Sinyal Riil Di Waktu Punya Spektrum?", "id": "Dengan Simetri Konjugat Kompleks." },
  { "en": "Artinya Magnitudo Selalu?", "id": "Simetris Genap." },
  { "en": "Artinya Fasa Selalu?", "id": "Simetris Ganjil." },
  { "en": "Apa Itu Respon Frekuensi Sistem?", "id": "Transformasi Fourier Dari Respon Impuls Sistem." },
  { "en": "Magnitudo Respon Frekuensi Menunjukkan?", "id": "Penguatan (Gain) Sistem Pada Tiap Frekuensi." },
  { "en": "Fasa Respon Frekuensi Menunjukkan?", "id": "Pergeseran Fasa Sistem Pada Tiap Frekuensi." },
  { "en": "Sistem LTI Tidak Akan Pernah?", "id": "Menciptakan Frekuensi Baru Yang Tidak Ada." },
  { "en": "Sistem Non-Linier Akan?", "id": "Menciptakan Harmonik Baru (Distorsi)." },
  { "en": "THD Adalah Ukuran?", "id": "Total Harmonic Distortion." },
  { "en": "THD Diukur Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Filter Ideal Di Frekuensi?", "id": "Berbentuk Kotak Sempurna (Brickwall)." },
  { "en": "Respon Impulsnya Di Waktu?", "id": "Berbentuk Sinc Tak Terhingga." },
  { "en": "Kenapa Filter Ideal Tidak Bisa Dibuat?", "id": "Karena Non-Kausal (Merespon Sebelum Input)." },
  { "en": "Apa Itu Efek Gibbs?", "id": "Riak Kecil Dekat Tepi Tajam." },
  { "en": "Efek Gibbs Terjadi Di?", "id": "Domain Waktu." },
  { "en": "Penyebabnya Adalah Pemotongan?", "id": "Deret Fourier Di Domain Frekuensi." },
  { "en": "Apa Itu Jendela (Windowing)?", "id": "Mengalikan Sinyal Dengan Fungsi Jendela." },
  { "en": "Tujuan Jendela Di Analisis Spektral?", "id": "Mengurangi Bocoran Spektral (Spectral Leakage)." },
  { "en": "Apa Itu Bocoran Spektral?", "id": "Energi Bocor Ke Frekuensi Tetangga." },
  { "en": "Apa Itu Sinyal Waktu-Terbatas?", "id": "Sinyal Dengan Durasi Waktu Terbatas." },
  { "en": "Spektrumnya Akan Berlangsung?", "id": "Selamanya (Tidak Terbatas)." },
  { "en": "Apa Itu Sinyal Pita-Terbatas?", "id": "Sinyal Dengan Bandwidth Frekuensi Terbatas." },
  { "en": "Bentuk Gelombangnya Akan Berlangsung?", "id": "Selamanya Di Domain Waktu." },
  { "en": "Sinyal Bisa Terbatas Di Keduanya?", "id": "Tidak Mungkin Secara Teori." },
  { "en": "Ini Disebut Prinsip?", "id": "Prinsip Ketidakpastian Fourier." },
  { "en": "Apa Itu Derau Putih (White Noise)?", "id": "Spektrum Datar Di Semua Frekuensi." },
  { "en": "Autokorelasi Derau Putih?", "id": "Fungsi Impuls Di Domain Waktu." },
  { "en": "Apa Itu Sinyal Baseband?", "id": "Sinyal Dengan Spektrum Di Sekitar Nol." },
  { "en": "Apa Itu Sinyal Passband?", "id": "Sinyal Dengan Spektrum Di Sekitar Frekuensi Pembawa." },
  { "en": "Radio Adalah Sinyal?", "id": "Passband." },
  { "en": "Audio Adalah Sinyal?", "id": "Baseband." },
  { "en": "Modulasi Mengubah Sinyal?", "id": "Baseband Menjadi Sinyal Passband." },
  { "en": "Demodulasi Mengubah Sinyal?", "id": "Passband Kembali Menjadi Baseband." },
  { "en": "Apa Itu FDM?", "id": "Frequency Division Multiplexing." },
  { "en": "FDM Bekerja Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu TDM?", "id": "Time Division Multiplexing." },
  { "en": "TDM Bekerja Di Domain?", "id": "Domain Waktu." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Generalisasi Fourier Untuk Menganalisis Stabilitas." },
  { "en": "Laplace Menggunakan Frekuensi?", "id": "Frekuensi Kompleks (s = σ + jω)." },
  { "en": "Fourier Adalah Kasus Khusus?", "id": "Saat Bagian Riil σ Sama Dengan Nol." },
  { "en": "Apa Itu Aliasing?", "id": "Frekuensi Tinggi Menyamar Sebagai Frekuensi Rendah." },
  { "en": "Penyebab Aliasing?", "id": "Laju Sampling Terlalu Lambat." },
  { "en": "Bagaimana Mencegah Aliasing?", "id": "Sampling Di Atas Laju Nyquist." },
  { "en": "Laju Nyquist Adalah?", "id": "Dua Kali Frekuensi Maksimum Sinyal." },
  { "en": "Apa Itu Kepadatan Spektral?", "id": "Ukuran Bagaimana Energi Atau Daya Terdistribusi." },
  { "en": "Unit Kepadatan Spektral Energi?", "id": "Joule Per Hertz." },
  { "en": "Unit Kepadatan Spektral Daya?", "id": "Watt Per Hertz." },
  { "en": "Apa Itu Koherensi?", "id": "Ukuran Korelasi Antara Dua Sinyal." },
  { "en": "Koherensi Dianalisis Di?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Orthogonalitas?", "id": "Dua Sinyal Tidak Tumpang Tindih." },
  { "en": "Fungsi Basis Fourier?", "id": "Saling Ortogonal Satu Sama Lain." },
  { "en": "Sifat Ini Memungkinkan?", "id": "Dekomposisi Sinyal Menjadi Komponen Unik." },
  { "en": "Apa Itu Spektogram?", "id": "Representasi Waktu-Frekuensi Dari Sinyal." },
  { "en": "Spektogram Berguna Untuk Sinyal?", "id": "Non-Stasioner Seperti Ucapan Dan Musik." },
  { "en": "Apa Itu Resolusi Waktu?", "id": "Kemampuan Membedakan Peristiwa Di Waktu." },
  { "en": "Apa Itu Resolusi Frekuensi?", "id": "Kemampuan Membedakan Komponen Di Frekuensi." },
  { "en": "Keduanya Berbanding Terbalik?", "id": "Ya Ini Adalah Trade-off Fundamental." },
  { "en": "Apa Itu Sinyal Analitik?", "id": "Sinyal Kompleks Dengan Spektrum Satu Sisi." },
  { "en": "Apa Itu Transformasi Hilbert?", "id": "Filter Yang Menggeser Fasa 90 Derajat." },
  { "en": "Sinyal Analitik Dibuat Dengan?", "id": "Sinyal Asli Dan Transformasi Hilbertnya." },
  { "en": "Apa Itu Amplop Sesaat?", "id": "Amplitudo Sinyal Di Domain Waktu." },
  { "en": "Apa Itu Fasa Sesaat?", "id": "Fasa Sinyal Di Domain Waktu." },
  { "en": "Apa Itu Frekuensi Sesaat?", "id": "Turunan Dari Fasa Sesaat." },
  { "en": "Ketiganya Didapat Dari?", "id": "Sinyal Analitik." },
  { "en": "Apa Itu Dispersi?", "id": "Kecepatan Fase Bergantung Pada Frekuensi." },
  { "en": "Efek Dispersi Di Domain Waktu?", "id": "Pulsa Sinyal Menjadi Melebar." },
  { "en": "Contoh Dispersi?", "id": "Pelangi (Cahaya Putih Terurai)." },
  { "en": "Apa Efek Sistem LTI Pada Frekuensi?", "id": "Mengubah Amplitudo Dan Fasa Sinyal." },
  { "en": "Apakah Sistem LTI Menciptakan Frekuensi Baru?", "id": "Tidak Hanya Memodifikasi Frekuensi Yang Ada." },
  { "en": "Transformasi Fourier Dari Sinyal Riil?", "id": "Menghasilkan Spektrum Dengan Simetri Hermite." },
  { "en": "Magnitudo Spektrumnya Akan Selalu?", "id": "Genap Dan Simetris." },
  { "en": "Fasa Spektrumnya Akan Selalu?", "id": "Ganjil Dan Anti-Simetris." },
  { "en": "Apa Itu Sinyal Kausal?", "id": "Sinyal Bernilai Nol Untuk Waktu Negatif." },
  { "en": "Apa Itu Sistem Kausal?", "id": "Output Tidak Mendahului Input." },
  { "en": "Respon Impuls Sistem Kausal?", "id": "Harus Bernilai Nol Untuk Waktu Negatif." },
  { "en": "Apa Itu Efek Gibbs?", "id": "Overshoot Dekat Diskontinuitas Akibat Pemotongan." },
  { "en": "Ini Terjadi Di Domain Mana?", "id": "Domain Waktu." },
  { "en": "Penyebabnya Adalah Keterbatasan Di?", "id": "Domain Frekuensi (Bandwidth Terbatas)." },
  { "en": "Apa Itu Resolusi Frekuensi DFT?", "id": "Jarak Antara Dua Titik Frekuensi Berdekatan." },
  { "en": "Resolusi Frekuensi Bergantung Pada?", "id": "Laju Sampling Dan Panjang DFT." },
  { "en": "Untuk Resolusi Lebih Baik Kita Perlu?", "id": "Mengamati Sinyal Dalam Waktu Lebih Lama." },
  { "en": "Apa Itu Zero-Padding?", "id": "Menambahkan Nol Pada Sinyal Di Domain Waktu." },
  { "en": "Apa Efek Zero-Padding?", "id": "Meningkatkan Tampilan Resolusi Frekuensi (Interpolasi)." },
  { "en": "Apakah Zero-Padding Menambah Informasi?", "id": "Tidak Hanya Menghaluskan Tampilan Spektrum." },
  { "en": "Apa Itu Jendela Persegi (Rectangular)?", "id": "Memotong Sinyal Secara Tiba-tiba." },
  { "en": "Efek Jendela Persegi?", "id": "Menyebabkan Banyak Bocoran Spektral." },
  { "en": "Apa Itu Jendela Hamming Atau Hanning?", "id": "Jendela Halus Untuk Mengurangi Bocoran Spektral." },
  { "en": "Konsekuensi Jendela Halus?", "id": "Resolusi Frekuensi Menjadi Sedikit Lebih Buruk." },
  { "en": "Apa Itu Sinyal Stasioner?", "id": "Sifat Statistiknya Tidak Berubah Seiring Waktu." },
  { "en": "Apa Itu Sinyal Non-Stasioner?", "id": "Sifat Statistiknya Berubah Seiring Waktu." },
  { "en": "Musik Atau Ucapan Adalah Sinyal?", "id": "Non-Stasioner." },
  { "en": "Analisis Sinyal Non-Stasioner Menggunakan?", "id": "Transformasi Fourier Jangka Pendek (STFT)." },
  { "en": "STFT Adalah Dasar Dari?", "id": "Pembuatan Spektogram Atau Plot Air Terjun." },
  { "en": "Apa Itu Bank Filter?", "id": "Kumpulan Filter Untuk Memisahkan Pita Frekuensi." },
  { "en": "Analisis Fourier Bisa Dilihat Sebagai?", "id": "Bank Filter Dengan Pita Sangat Sempit." },
  { "en": "Apa Itu Fasa Linier?", "id": "Fasa Berubah Linier Terhadap Frekuensi." },
  { "en": "Sistem Dengan Fasa Linier Menyebabkan?", "id": "Penundaan Waktu Konstan Tanpa Distorsi." },
  { "en": "Apa Itu Sinyal Orde-nol-hold?", "id": "Hasil Dari DAC (Tangga)." },
  { "en": "Spektrum Sinyal Ini?", "id": "Memiliki Atentuasi Berbentuk Fungsi Sinc." },
  { "en": "Apa Itu Konvolusi Sirkular?", "id": "Hasil Dari Perkalian DFT." },
  { "en": "Untuk Menghindari Konvolusi Sirkular?", "id": "Gunakan Zero-Padding Yang Cukup." },
  { "en": "Apa Itu Sinyal Pita Sempit?", "id": "Bandwidth Jauh Lebih Kecil Dari Frekuensi Pembawa." },
  { "en": "Apa Itu Sinyal Pita Lebar?", "id": "Bandwidth Sebanding Dengan Frekuensi Pembawa." },
  { "en": "Apa Itu Teorema Dualitas?", "id": "Bentuk Operasi Di Satu Domain Mirip Lainnya." },
  { "en": "Transformasi Dari Sinc Adalah?", "id": "Fungsi Kotak (Rect)." },
  { "en": "Transformasi Dari Kotak Adalah?", "id": "Fungsi Sinc." },
  { "en": "Apa Itu Autokorelasi?", "id": "Mengukur Kemiripan Sinyal Dengan Versi Gesernya." },
  { "en": "Transformasi Fourier Dari Autokorelasi?", "id": "Adalah Kepadatan Spektral Daya (PSD)." },
  { "en": "Teorema Ini Disebut?", "id": "Teorema Wiener-Khinchin." },
  { "en": "Apa Itu Cross-Korelasi?", "id": "Mengukur Kemiripan Antara Dua Sinyal Berbeda." },
  { "en": "Aplikasi Cross-Korelasi?", "id": "Deteksi Sinyal Radar Dan Pengenalan Pola." },
  { "en": "Apa Itu Decimation In Time?", "id": "Salah Satu Jenis Algoritma FFT." },
  { "en": "Apa Itu Decimation In Frequency?", "id": "Jenis Lain Dari Algoritma FFT." },
  { "en": "Apa Itu Orde Filter?", "id": "Menentukan Ketajaman Transisi Filter." },
  { "en": "Semakin Tinggi Orde Filter?", "id": "Semakin Mendekati Filter Ideal." },
  { "en": "Kekurangannya Adalah?", "id": "Kompleksitas Dan Penundaan Sistem Meningkat." },
  { "en": "Apa Itu Fasa Minimum?", "id": "Sistem Stabil Yang Inversnya Juga Stabil." },
  { "en": "Apa Itu Sistem All-Pass?", "id": "Sistem Yang Hanya Mengubah Fasa Sinyal." },
  { "en": "Magnitudo Respon Frekuensinya?", "id": "Selalu Satu (Datar)." },
  { "en": "Apa Itu Group Delay?", "id": "Turunan Negatif Dari Fasa Terhadap Frekuensi." },
  { "en": "Group Delay Mengukur?", "id": "Penundaan Amplop Sinyal." },
  { "en": "Apa Itu Sinyal Analitik?", "id": "Sinyal Kompleks Dengan Spektrum Satu Sisi." },
  { "en": "Bagian Imajiner Sinyal Analitik?", "id": "Adalah Transformasi Hilbert Dari Bagian Riil." },
  { "en": "Sinyal Analitik Berguna Untuk?", "id": "Menghitung Amplop Dan Frekuensi Sesaat." },
  { "en": "Apa Itu Efek Doppler?", "id": "Pergeseran Frekuensi Akibat Gerakan Relatif." },
  { "en": "Ini Adalah Fenomena Domain?", "id": "Frekuensi Yang Disebabkan Gerakan Di Waktu." },
  { "en": "Apa Itu Sinyal Chirp?", "id": "Sinyal Sinusoidal Dengan Frekuensi Yang Berubah." },
  { "en": "Analisis Sinyal Chirp Menggunakan?", "id": "Spektogram Atau Transformasi Wigner-Ville." },
  { "en": "Apa Itu Kestabilan BIBO?", "id": "Bounded-Input Bounded-Output." },
  { "en": "Syarat Stabilitas BIBO?", "id": "Respon Impuls Harus Terintegralkan Secara Mutlak." },
  { "en": "Apa Implikasinya Di Domain Frekuensi?", "id": "Respon Frekuensi Harus Ada Dan Terbatas." },
  { "en": "Apa Itu Kebisingan Termal?", "id": "Noise Acak Akibat Gerakan Elektron." },
  { "en": "Spektrum Kebisingan Termal?", "id": "Hampir Datar (Derau Putih)." },
  { "en": "Apa Itu Derau 1/f (Pink Noise)?", "id": "Spektrum Daya Menurun Seiring Frekuensi." },
  { "en": "Banyak Ditemukan Di?", "id": "Sistem Biologis Dan Elektronik." },
  { "en": "Apa Itu Modulasi Pita Sisi Tunggal (SSB)?", "id": "Modulasi AM Yang Lebih Efisien." },
  { "en": "SSB Mengirimkan Apa?", "id": "Hanya Salah Satu Pita Sisi Saja." },
  { "en": "Ini Menghemat Apa?", "id": "Menghemat Daya Dan Bandwidth Kanal." },
  { "en": "Demodulasi SSB Membutuhkan?", "id": "Rekonstruksi Frekuensi Pembawa Yang Tepat." },
  { "en": "Apa Itu Transformasi Kosinus Diskrit (DCT)?", "id": "Varian Fourier Yang Hanya Menggunakan Kosinus." },
  { "en": "DCT Digunakan Di Kompresi?", "id": "Kompresi Gambar JPEG Dan Video MPEG." },
  { "en": "Kenapa DCT Dipakai?", "id": "Sangat Baik Memadatkan Energi Sinyal Gambar." },
  { "en": "Apa Itu Koefisien DC Dalam DCT?", "id": "Mewakili Kecerahan Rata-rata Blok Gambar." },
  { "en": "Apa Itu Koefisien AC Dalam DCT?", "id": "Mewakili Detail Dan Tekstur Blok Gambar." },
  { "en": "Kompresi Bekerja Dengan?", "id": "Membuang Koefisien AC Bernilai Kecil." },
  { "en": "Apa Itu Cepstrum?", "id": "Transformasi Fourier Dari Logaritma Spektrum." },
  { "en": "Cepstrum Berguna Untuk?", "id": "Mendeteksi Pitch Dan Gema Dalam Sinyal." },
  { "en": "Sumbu X Cepstrum Disebut?", "id": "Quefrency (Memiliki Satuan Waktu)." },
  { "en": "Apa Itu Homomorphic Filtering?", "id": "Filtering Yang Dilakukan Di Domain Cepstral." },
  { "en": "Apa Itu OFDM?", "id": "Orthogonal Frequency Division Multiplexing." },
  { "en": "OFDM Mengirim Data Di?", "id": "Banyak Sub-pembawa Ortogonal Secara Paralel." },
  { "en": "Implementasi Praktis OFDM Menggunakan?", "id": "IFFT Di Pemancar Dan FFT Di Penerima." },
  { "en": "Keuntungan OFDM?", "id": "Sangat Tahan Terhadap Efek Multipath." },
  { "en": "Apa Itu Cyclic Prefix?", "id": "Salinan Akhir Simbol Ditambahkan Ke Awal." },
  { "en": "Tujuan Cyclic Prefix?", "id": "Mengatasi Interferensi Antar Simbol." },
  { "en": "Apa Itu Intercarrier Interference?", "id": "Gangguan Antar Sub-pembawa Akibat Pergeseran Doppler." },
  { "en": "Apa Itu Kepadatan?", "id": "Seberapa Padat Sinyal Di Suatu Domain." },
  { "en": "Sinyal Padat Di Waktu?", "id": "Sinyal Yang Selalu Aktif." },
  { "en": "Sinyal Jarang Di Waktu?", "id": "Sinyal Yang Aktif Sesekali (Pulsa)." },
  { "en": "Sinyal Jarang Di Frekuensi?", "id": "Hanya Terdiri Dari Beberapa Komponen Sinusoid." },
  { "en": "Compressed Sensing Bekerja Untuk Sinyal?", "id": "Sinyal Yang Jarang Di Domain Tertentu." },
  { "en": "Apa Esensi Fourier?", "id": "Mengubah Sudut Pandang Dari Waktu Ke Frekuensi." },
  { "en": "Apa Itu Sifat Diferensiasi Waktu?", "id": "Turunan Di Waktu Menjadi Perkalian Dengan jω." },
  { "en": "Apa Itu Sifat Integrasi Waktu?", "id": "Integral Di Waktu Menjadi Pembagian Dengan jω." },
  { "en": "Diferensiasi Di Waktu Menguatkan Komponen?", "id": "Frekuensi Tinggi Dari Sinyal." },
  { "en": "Integrasi Di Waktu Meredam Komponen?", "id": "Frekuensi Tinggi Dari Sinyal." },
  { "en": "Apa Itu Sifat Perkalian Di Waktu?", "id": "Perkalian Di Waktu Menjadi Konvolusi Di Frekuensi." },
  { "en": "Operasi Ini Disebut Juga?", "id": "Modulasi Amplitudo Atau Pencampuran (Mixing)." },
  { "en": "Efeknya Pada Spektrum?", "id": "Menggeser Spektrum Asli Ke Frekuensi Pembawa." },
  { "en": "Apa Itu Teorema Simetri Atau Dualitas?", "id": "Bentuk Operasi Di Satu Domain Mirip Lainnya." },
  { "en": "Jika x(t) Menghasilkan X(ω)?", "id": "Maka X(t) Menghasilkan 2πx(-ω)." },
  { "en": "Pulsa Kotak Di Waktu Menghasilkan?", "id": "Fungsi Sinc Di Domain Frekuensi." },
  { "en": "Sesuai Dualitas Sinc Di Waktu Menghasilkan?", "id": "Fungsi Kotak Di Domain Frekuensi." },
  { "en": "Apa Itu Sinyal Real?", "id": "Sinyal Yang Nilainya Selalu Bilangan Riil." },
  { "en": "Spektrum Sinyal Real?", "id": "Memiliki Simetri Konjugat (Hermitian)." },
  { "en": "Apa Itu Sinyal Imajiner Murni?", "id": "Sinyal Yang Nilainya Selalu Bilangan Imajiner." },
  { "en": "Spektrum Sinyal Imajiner Murni?", "id": "Memiliki Simetri Konjugat Ganjil." },
  { "en": "Apa Itu Bagian Genap Sinyal?", "id": "Komponen Sinyal Yang Simetris Genap." },
  { "en": "Transformasi Bagian Genap?", "id": "Bagian Riil Dari Transformasi Fourier." },
  { "en": "Apa Itu Bagian Ganjil Sinyal?", "id": "Komponen Sinyal Yang Simetris Ganjil." },
  { "en": "Transformasi Bagian Ganjil?", "id": "Bagian Imajiner Dari Transformasi Fourier." },
  { "en": "Apa Itu Kondisi Dirichlet?", "id": "Syarat Agar Deret Fourier Konvergen." },
  { "en": "Sebutkan Satu Kondisi Dirichlet?", "id": "Sinyal Harus Terintegralkan Secara Mutlak." },
  { "en": "Sebutkan Kondisi Dirichlet Lain?", "id": "Jumlah Maksima Dan Minima Terbatas." },
  { "en": "Sebutkan Kondisi Terakhir Dirichlet?", "id": "Jumlah Diskontinuitas Terbatas." },
  { "en": "Sinyal Teknik Biasanya Memenuhi?", "id": "Ya Hampir Semua Sinyal Praktis Memenuhi." },
  { "en": "Apa Itu Konvergensi Deret Fourier?", "id": "Deret Mendekati Sinyal Asli Saat N Tak Hingga." },
  { "en": "Di Titik Kontinu Konvergensi?", "id": "Menuju Nilai Sinyal Asli." },
  { "en": "Di Titik Diskontinu Konvergensi?", "id": "Menuju Nilai Rata-rata Dari Lompatan." },
  { "en": "Fenomena Gibbs Terjadi Dimana?", "id": "Di Sekitar Titik Diskontinuitas (Lompatan)." },
  { "en": "Besar Overshoot Fenomena Gibbs?", "id": "Sekitar Sembilan Persen Dari Tinggi Lompatan." },
  { "en": "Apakah Overshoot Hilang Dengan N Besar?", "id": "Tidak Overshoot Tetap Ada Tapi Menyempit." },
  { "en": "Apa Itu Sistem Stabil?", "id": "Output Terbatas Untuk Input Yang Terbatas." },
  { "en": "Apa Syarat Stabilitas?", "id": "Respon Impuls Harus Terintegralkan Secara Mutlak." },
  { "en": "Apa Artinya Di Domain Frekuensi?", "id": "Respon Frekuensi Harus Ada Dan Terdefinisi." },
  { "en": "Apa Itu Distorsi Amplitudo?", "id": "Saat Magnitudo Respon Frekuensi Tidak Datar." },
  { "en": "Apa Itu Distorsi Fasa?", "id": "Saat Fasa Respon Frekuensi Tidak Linier." },
  { "en": "Sistem Bebas Distorsi Memiliki?", "id": "Gain Konstan Dan Fasa Linier." },
  { "en": "Respon Impuls Sistem Bebas Distorsi?", "id": "Impuls Yang Tertunda Dan Diskala." },
  { "en": "Sinyal Impuls Ideal Punya Bandwidth?", "id": "Tak Terhingga." },
  { "en": "Sinyal DC Ideal Punya Bandwidth?", "id": "Nol." },
  { "en": "Panjang DFT (N) Menentukan?", "id": "Jumlah Titik Spektrum Dan Resolusi." },
  { "en": "Resolusi Frekuensi DFT Adalah?", "id": "Fs Dibagi Dengan N (Fs/N)." },
  { "en": "Fs Adalah Apa?", "id": "Frekuensi Sampling Sinyal." },
  { "en": "Apa Itu Kebocoran Spektral?", "id": "Energi Sinyal Bocor Ke Bin Frekuensi Lain." },
  { "en": "Penyebab Utama Kebocoran?", "id": "Sinyal Tidak Periodik Sempurna Dalam Jendela." },
  { "en": "Bagaimana Menguranginya?", "id": "Menggunakan Fungsi Jendela (Windowing)." },
  { "en": "Jendela Mengubah Sinyal Di?", "id": "Domain Waktu (Ujung Dibuat Nol)." },
  { "en": "Efeknya Di Domain Frekuensi?", "id": "Mengurangi Sidelobe Spektrum Jendela." },
  { "en": "Apa Itu Teorema Modulasi?", "id": "Sama Dengan Sifat Pergeseran Frekuensi." },
  { "en": "Apa Itu Sideband?", "id": "Pita Sisi Yang Muncul Akibat Modulasi." },
  { "en": "Modulasi AM Menghasilkan?", "id": "Satu Frekuensi Pembawa Dan Dua Sideband." },
  { "en": "Bandwidth Sinyal AM?", "id": "Dua Kali Bandwidth Sinyal Informasi." },
  { "en": "Apa Itu Demodulasi?", "id": "Proses Mendapatkan Kembali Sinyal Informasi." },
  { "en": "Demodulasi Koheren Membutuhkan?", "id": "Sinkronisasi Fasa Dengan Frekuensi Pembawa." },
  { "en": "Demodulasi Non-Koheren Menggunakan?", "id": "Detektor Amplop (Envelope Detector)." },
  { "en": "Apa Itu Sinyal Acak?", "id": "Sinyal Yang Tidak Dapat Diprediksi." },
  { "en": "Analisisnya Menggunakan?", "id": "Metode Statistik (Autokorelasi Dan PSD)." },
  { "en": "Fungsi Autokorelasi Diukur Di?", "id": "Domain Waktu (Lag τ)." },
  { "en": "Kepadatan Spektral Daya Diukur Di?", "id": "Domain Frekuensi (f)." },
  { "en": "Apa Hubungan Keduanya?", "id": "Pasangan Transformasi Fourier (Teorema Wiener-Khinchin)." },
  { "en": "PSD Dari Derau Putih?", "id": "Konstan Di Semua Frekuensi." },
  { "en": "PSD Sistem LTI Output?", "id": "PSD Input Dikalikan Kuadrat Magnitudo H(f)." },
  { "en": "Apa Itu Matched Filter?", "id": "Filter Optimal Untuk Mendeteksi Sinyal." },
  { "en": "Respon Impuls Matched Filter?", "id": "Bentuk Sinyal Dicari Yang Dibalik Waktu." },
  { "en": "Tujuannya Memaksimalkan Apa?", "id": "Rasio Sinyal Terhadap Derau (SNR)." },
  { "en": "Apa Itu Orthogonalitas?", "id": "Dua Fungsi Tidak Tumpang Tindih." },
  { "en": "Integral Perkalian Keduanya?", "id": "Sama Dengan Nol." },
  { "en": "Contoh Fungsi Ortogonal?", "id": "Sinus Dan Kosinus Dengan Frekuensi Sama." },
  { "en": "Fungsi Basis Fourier?", "id": "Adalah Kumpulan Fungsi Yang Ortogonal." },
  { "en": "Sifat Ini Memungkinkan Apa?", "id": "Menghitung Koefisien Fourier Secara Independen." },
  { "en": "Apa Itu Transformasi Hilbert?", "id": "Filter Ideal Yang Menggeser Fasa -90 Derajat." },
  { "en": "Hilbert Digunakan Untuk Membuat?", "id": "Sinyal Analitik." },
  { "en": "Apa Itu Sinyal Analitik?", "id": "Sinyal Kompleks Dengan Spektrum Satu Sisi." },
  { "en": "Apa Itu Fasa Sesaat?", "id": "Fasa Sinyal Sebagai Fungsi Waktu." },
  { "en": "Apa Itu Frekuensi Sesaat?", "id": "Turunan Dari Fasa Sesaat." },
  { "en": "Keduanya Dihitung Dari?", "id": "Sinyal Analitik." },
  { "en": "Apa Itu Sistem Fasa Minimum?", "id": "Sistem Stabil Dan Inversnya Juga Stabil." },
  { "en": "Zero Sistem Fasa Minimum?", "id": "Semua Di Setengah Kiri Bidang-s." },
  { "en": "Apa Itu Sistem All-pass?", "id": "Sistem Dengan Magnitudo Respon Frekuensi Datar." },
  { "en": "Sistem Ini Hanya Mengubah?", "id": "Fasa Dari Sinyal." },
  { "en": "Setiap Sistem Bisa Dipecah Menjadi?", "id": "Bagian Fasa Minimum Dan Bagian All-pass." },
  { "en": "Apa Itu Plot Bode?", "id": "Plot Logaritmik Dari Respon Frekuensi." },
  { "en": "Sumbu Y Plot Magnitudo?", "id": "Dalam Satuan Desibel (dB)." },
  { "en": "Sumbu X Adalah?", "id": "Frekuensi Dalam Skala Logaritmik." },
  { "en": "Apa Keuntungan Plot Bode?", "id": "Perkalian Respon Frekuensi Menjadi Penjumlahan." },
  { "en": "Apa Itu Asimtot?", "id": "Garis Lurus Aproksimasi Plot Bode." },
  { "en": "Kemiringan Asimtot Ditentukan Oleh?", "id": "Jumlah Pole Dan Zero Di Origin." },
  { "en": "Apa Itu Frekuensi Pojok (Corner Frequency)?", "id": "Frekuensi Dimana Asimtot Bertemu." },
  { "en": "Frekuensi Pojok Ditentukan Oleh?", "id": "Lokasi Pole Dan Zero." },
  { "en": "Apa Itu Decimation?", "id": "Proses Mengurangi Laju Sampling." },
  { "en": "Apa Itu Interpolasi?", "id": "Proses Meningkatkan Laju Sampling." },
  { "en": "Keduanya Adalah Inti Dari?", "id": "Pemrosesan Sinyal Multirate." },
  { "en": "Apa Itu Filter FIR?", "id": "Finite Impulse Response." },
  { "en": "Respon Impulsnya Berdurasi?", "id": "Terbatas." },
  { "en": "Filter FIR Selalu?", "id": "Stabil." },
  { "en": "Apa Itu Filter IIR?", "id": "Infinite Impulse Response." },
  { "en": "Respon Impulsnya Berdurasi?", "id": "Tak Terhingga." },
  { "en": "Filter IIR Bisa?", "id": "Tidak Stabil Jika Desainnya Salah." },
  { "en": "Apa Itu DFT?", "id": "Discrete Fourier Transform." },
  { "en": "DFT Adalah Versi Diskrit Dari?", "id": "Transformasi Fourier Untuk Sinyal Diskrit." },
  { "en": "Input DFT Adalah?", "id": "Blok Sampel Sinyal Di Domain Waktu." },
  { "en": "Output DFT Adalah?", "id": "Blok Sampel Spektrum Di Domain Frekuensi." },
  { "en": "Jumlah Sampel Input Dan Output?", "id": "Selalu Sama (N Titik)." },
  { "en": "Apa Itu FFT?", "id": "Fast Fourier Transform." },
  { "en": "FFT Adalah Algoritma Untuk?", "id": "Menghitung DFT Dengan Sangat Cepat." },
  { "en": "Apa Itu 'Bin' DFT?", "id": "Satu Sampel Frekuensi Hasil DFT." },
  { "en": "Setiap Bin Mewakili Amplitudo Di?", "id": "Frekuensi Tertentu." },
  { "en": "Apa Itu Resolusi Frekuensi DFT?", "id": "Jarak Frekuensi Antara Dua Bin Berdekatan." },
  { "en": "Resolusi Frekuensi Dihitung Dengan?", "id": "Laju Sampling Dibagi Dengan Panjang DFT." },
  { "en": "Untuk Resolusi Lebih Baik Diperlukan?", "id": "Panjang DFT (N) Yang Lebih Besar." },
  { "en": "N Lebih Besar Artinya Mengamati Sinyal?", "id": "Dalam Durasi Waktu Yang Lebih Lama." },
  { "en": "Apa Itu DTFT?", "id": "Discrete-Time Fourier Transform." },
  { "en": "Apa Beda DFT Dan DTFT?", "id": "DFT Diskrit DTFT Spektrumnya Kontinu." },
  { "en": "Spektrum DTFT Periodik?", "id": "Ya Periodik Dengan Periode Laju Sampling." },
  { "en": "DFT Adalah Sampel Dari?", "id": "Satu Periode Spektrum DTFT." },
  { "en": "Apa Itu Zero-Padding?", "id": "Menambahkan Sampel Nol Ke Sinyal Waktu." },
  { "en": "Tujuan Zero-Padding?", "id": "Mendapatkan Tampilan Spektrum Yang Lebih Halus." },
  { "en": "Zero-Padding Sama Dengan Interpolasi Di?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Teorema Parseval Untuk DFT?", "id": "Energi Sinyal Sama Di Kedua Domain." },
  { "en": "Energi Di Waktu Dihitung Dengan?", "id": "Jumlah Kuadrat Amplitudo Sampel." },
  { "en": "Energi Di Frekuensi Dihitung Dengan?", "id": "Jumlah Kuadrat Magnitudo Koefisien DFT." },
  { "en": "Apa Itu Sifat Periodisitas DFT?", "id": "Baik Sinyal Waktu Dan Frekuensi Adalah Periodik." },
  { "en": "Periode Di Waktu Adalah?", "id": "N Sampel." },
  { "en": "Periode Di Frekuensi Adalah?", "id": "Juga N Sampel." },
  { "en": "Apa Itu Konvolusi Linier?", "id": "Konvolusi Standar Untuk Sinyal Aperiodik." },
  { "en": "Apa Itu Konvolusi Sirkular?", "id": "Konvolusi Untuk Sinyal Periodik." },
  { "en": "Perkalian DFT Di Frekuensi Menghasilkan?", "id": "Konvolusi Sirkular Di Domain Waktu." },
  { "en": "Bagaimana Melakukan Konvolusi Linier Dengan DFT?", "id": "Menggunakan Zero-Padding Yang Cukup." },
  { "en": "Panjang Sinyal Hasil Konvolusi Linier?", "id": "L + M - 1." },
  { "en": "L Dan M Adalah?", "id": "Panjang Dari Dua Sinyal Asli." },
  { "en": "Panjang DFT Minimal Adalah?", "id": "L + M - 1." },
  { "en": "Apa Itu Sifat Simetri DFT?", "id": "Mirip Dengan Simetri Hermite Untuk Sinyal Riil." },
  { "en": "Koefisien DFT Sinyal Riil?", "id": "Memiliki Simetri Konjugat." },
  { "en": "Apa Implikasinya?", "id": "Setengah Koefisien Saja Yang Perlu Disimpan." },
  { "en": "Apa Itu Laju Nyquist?", "id": "Laju Sampling Minimum Yang Dibutuhkan." },
  { "en": "Nilainya Adalah?", "id": "Dua Kali Frekuensi Maksimum Sinyal." },
  { "en": "Apa Itu Frekuensi Nyquist?", "id": "Setengah Dari Laju Sampling." },
  { "en": "Frekuensi Maksimum Yang Bisa Diwakili?", "id": "Adalah Frekuensi Nyquist." },
  { "en": "Apa Itu Filter Digital?", "id": "Sistem Diskrit Yang Memodifikasi Spektrum." },
  { "en": "Implementasi Filter Digital?", "id": "Persamaan Beda Di Domain Waktu." },
  { "en": "Deskripsi Filter Digital?", "id": "Respon Frekuensi Di Domain Frekuensi." },
  { "en": "Apa Itu Filter FIR?", "id": "Finite Impulse Response." },
  { "en": "Filter FIR Tidak Memiliki?", "id": "Umpan Balik (Feedback)." },
  { "en": "Filter FIR Selalu?", "id": "Stabil." },
  { "en": "Filter FIR Bisa Punya Fasa?", "id": "Fasa Linier Sempurna." },
  { "en": "Apa Itu Filter IIR?", "id": "Infinite Impulse Response." },
  { "en": "Filter IIR Memiliki?", "id": "Umpan Balik (Feedback)." },
  { "en": "Filter IIR Bisa?", "id": "Tidak Stabil." },
  { "en": "Keuntungan Filter IIR?", "id": "Jauh Lebih Efisien Secara Komputasi." },
  { "en": "Desain Filter IIR Seringkali?", "id": "Berdasarkan Prototipe Filter Analog." },
  { "en": "Apa Itu Transformasi Bilinear?", "id": "Metode Untuk Mengubah Filter Analog Ke Digital." },
  { "en": "Apa Itu Efek Warping Frekuensi?", "id": "Distorsi Sumbu Frekuensi Akibat Transformasi Bilinear." },
  { "en": "Apa Itu Pre-warping?", "id": "Kompensasi Frekuensi Sebelum Melakukan Transformasi." },
  { "en": "Apa Itu Jendela Kaiser?", "id": "Fungsi Jendela Dengan Parameter Beta." },
  { "en": "Parameter Beta Mengontrol?", "id": "Trade-off Antara Mainlobe Dan Sidelobe." },
  { "en": "Metode Desain FIR?", "id": "Metode Jendela Dan Metode Parks-McClellan." },
  { "en": "Metode Parks-McClellan Menghasilkan?", "id": "Filter Optimal Dengan Riak Sama (Equiripple)." },
  { "en": "Apa Itu Sinyal Kuasa?", "id": "Sinyal Dengan Daya Rata-rata Terbatas." },
  { "en": "Contoh Sinyal Kuasa?", "id": "Sinusoid Dan Sinyal Acak Stasioner." },
  { "en": "Apa Itu Sinyal Energi?", "id": "Sinyal Dengan Energi Total Terbatas." },
  { "en": "Contoh Sinyal Energi?", "id": "Pulsa Tunggal Atau Eksponensial Meluruh." },
  { "en": "Apa Itu Sinyal Real?", "id": "Nilai Amplitudonya Selalu Bilangan Riil." },
  { "en": "Apa Itu Sinyal Kompleks?", "id": "Nilai Amplitudonya Bisa Bilangan Kompleks." },
  { "en": "Sinyal Fisik Selalu?", "id": "Bernilai Riil." },
  { "en": "Sinyal Kompleks Digunakan Untuk?", "id": "Kenyamanan Dan Efisiensi Analisis Matematis." },
  { "en": "Apa Itu Eksponensial Kompleks?", "id": "e Pangkat jωt." },
  { "en": "Menurut Rumus Euler Terdiri Dari?", "id": "Bagian Riil Kosinus Dan Imajiner Sinus." },
  { "en": "Eksponensial Kompleks Adalah Basis?", "id": "Untuk Semua Analisis Fourier." },
  { "en": "Kenapa Eksponensial Kompleks Basis Yang Baik?", "id": "Merupakan Fungsi Eigen Dari Sistem LTI." },
  { "en": "Artinya Bentuknya Tidak Berubah?", "id": "Benar Hanya Amplitudo Dan Fasanya Berubah." },
  { "en": "Perubahan Ini Dideskripsikan Oleh?", "id": "Respon Frekuensi Sistem H(ω)." },
  { "en": "Apa Itu Sistem Fasa Nol?", "id": "Sistem Yang Tidak Menggeser Fasa Sinyal." },
  { "en": "Respon Impuls Sistem Fasa Nol?", "id": "Simetris Sempurna Di Sekitar Waktu Nol." },
  { "en": "Sistem Fasa Nol Adalah?", "id": "Sistem Non-Kausal." },
  { "en": "Bisa Diimplementasikan Secara Real-Time?", "id": "Tidak Tapi Bisa Untuk Pemrosesan Offline." },
  { "en": "Apa Itu Kausalitas?", "id": "Output Tidak Bisa Mendahului Input." },
  { "en": "Apa Itu Stabilitas?", "id": "Output Terbatas Untuk Input Yang Terbatas." },
  { "en": "Apa Itu Linearitas?", "id": "Prinsip Superposisi Berlaku." },
  { "en": "Apa Itu Invariansi Waktu?", "id": "Perilaku Sistem Tidak Berubah Seiring Waktu." },
  { "en": "Keempat Sifat Ini Adalah Dasar?", "id": "Analisis Sistem Menggunakan Fourier." },
  { 'en': 'Apa Itu Superposisi?', 'id': 'Respon Terhadap Jumlah Input Adalah Jumlah Respon.' },
  { 'en': 'Apa Itu Time Reversal?', 'id': 'Membalik Sumbu Waktu Sinyal (x[-n]).' },
  { 'en': 'Efek Time Reversal Pada Spektrum?', 'id': 'Membalik Sumbu Frekuensi (X[-k]).' },
  { 'en': 'Apa Itu Scaling?', 'id': 'Mengompres Atau Meregangkan Sumbu Waktu.' },
  { 'en': 'Efek Scaling Pada Spektrum?', 'id': 'Efek Penskalaan Terbalik Pada Sumbu Frekuensi.' },
  { 'en': 'Apa Itu Decimation In Time FFT?', 'id': 'Memecah Sinyal Waktu Menjadi Genap Ganjil.' },
  { 'en': 'Apa Itu Decimation In Frequency FFT?', 'id': 'Memecah Spektrum Frekuensi Menjadi Genap Ganjil.' },
  { 'en': 'Keduanya Adalah Algoritma?', 'id': 'Rekursif Yang Sangat Efisien.' },
  { 'en': 'Apa Itu Butterfly Diagram?', 'id': 'Diagram Aliran Dasar Dalam Komputasi FFT.' },
  { 'en': 'Apa Itu Twiddle Factor?', 'id': 'Koefisien Eksponensial Kompleks Dalam Algoritma FFT.' },
  { 'en': 'Apa Itu Transformasi Wavelet?', 'id': 'Analisis Sinyal Menggunakan Fungsi Basis Wavelet.' },
  { 'en': 'Kelebihan Wavelet Dibanding Fourier?', 'id': 'Memberikan Lokalisasi Di Waktu Dan Frekuensi.' },
  { 'en': 'Fourier Hanya Memberi Lokalisasi Di?', 'id': 'Domain Frekuensi Saja.' },
  { 'en': 'Wavelet Baik Untuk Sinyal?', 'id': 'Non-Stasioner Dengan Transien.' },
  { "en": "Apa Itu Sifat Diferensiasi Frekuensi?", "id": "Perkalian Dengan 'jt' Di Domain Waktu." },
  { "en": "Apa Itu Sifat Konjugasi?", "id": "Konjugat Di Waktu Menjadi Konjugat Terbalik Di Frekuensi." },
  { "en": "Energi Sinyal Dihitung Di Domain?", "id": "Domain Waktu Atau Domain Frekuensi." },
  { "en": "Hasilnya Akan Selalu Sama?", "id": "Ya Menurut Teorema Parseval." },
  { "en": "Apa Itu Respon Frekuensi Nol (DC)?", "id": "Gain Sistem Pada Frekuensi Nol." },
  { "en": "Bagaimana Menghitungnya Dari Respon Impuls?", "id": "Integral Total Dari Respon Impuls h(t)." },
  { "en": "Apa Itu Nilai Rata-rata Sinyal?", "id": "Koefisien Fourier 'a0' Atau 'c0'." },
  { "en": "Nilai Ini Terletak Di Frekuensi?", "id": "Nol Atau DC." },
  { "en": "Apa Itu Sistem Fasa Linier?", "id": "Menunda Semua Frekuensi Dengan Waktu Yang Sama." },
  { "en": "Respon Impuls Sistem Fasa Linier?", "id": "Memiliki Simetri Konjugat Genap." },
  { "en": "Apa Itu Sistem Fasa Nol?", "id": "Kasus Khusus Fasa Linier Tanpa Penundaan." },
  { "en": "Respon Impuls Sistem Fasa Nol?", "id": "Bernilai Riil Dan Simetris Genap." },
  { "en": "Sistem Fasa Nol Kausal?", "id": "Tidak Karena Harus Mulai Sebelum Waktu Nol." },
  { "en": "Apa Itu Aliasing?", "id": "Frekuensi Tinggi Menyamar Sebagai Frekuensi Rendah." },
  { "en": "Penyebab Aliasing?", "id": "Laju Sampling Kurang Dari Laju Nyquist." },
  { "en": "Cara Menghindari Aliasing?", "id": "Gunakan Filter Anti-Aliasing Sebelum Sampling." },
  { "en": "Apa Itu Filter Anti-Aliasing?", "id": "Filter Low-Pass Untuk Membatasi Bandwidth Sinyal." },
  { "en": "Perkalian DFT Sama Dengan?", "id": "Konvolusi Sirkular Di Domain Waktu." },
  { "en": "Panjang Hasil Konvolusi Sirkular?", "id": "Sama Dengan Panjang DFT (N)." },
  { "en": "Panjang Hasil Konvolusi Linier?", "id": "N1 + N2 - 1." },
  { "en": "Apa Itu Metode Overlap-Add?", "id": "Teknik Melakukan Konvolusi Linier Menggunakan FFT." },
  { "en": "Apa Itu Metode Overlap-Save?", "id": "Teknik Lain Untuk Konvolusi Linier Cepat." },
  { "en": "Apa Itu Sinyal Kuasa?", "id": "Sinyal Dengan Daya Terbatas Energi Tak Terbatas." },
  { "en": "Contoh Sinyal Kuasa?", "id": "Sinusoid Sinyal Periodik Dan Sinyal Acak." },
  { "en": "Analisisnya Menggunakan?", "id": "Kepadatan Spektral Daya (PSD)." },
  { "en": "Apa Itu Sinyal Energi?", "id": "Sinyal Dengan Energi Terbatas Daya Nol." },
  { "en": "Contoh Sinyal Energi?", "id": "Pulsa Tunggal Atau Sinyal Yang Meluruh." },
  { "en": "Analisisnya Menggunakan?", "id": "Kepadatan Spektral Energi (ESD)." },
  { "en": "Apa Hubungan ESD Dan Transformasi Fourier?", "id": "ESD Adalah Kuadrat Magnitudo Transformasi Fourier." },
  { "en": "Apa Itu Proses Stokastik?", "id": "Proses Acak Atau Sinyal Acak." },
  { "en": "Apa Itu Sinyal Stasioner?", "id": "Sifat Statistiknya Tidak Berubah Seiring Waktu." },
  { "en": "PSD Dari Sinyal Stasioner?", "id": "Adalah Transformasi Fourier Fungsi Autokorelasi." },
  { "en": "Apa Itu Fungsi Jendela?", "id": "Fungsi Untuk Memilih Segmen Sinyal." },
  { "en": "Tujuan Jendela?", "id": "Mengurangi Efek Pemotongan (Bocoran Spektral)." },
  { "en": "Jendela Persegi Punya Sidelobe?", "id": "Sangat Tinggi Menyebabkan Banyak Bocoran." },
  { "en": "Jendela Hanning Punya Sidelobe?", "id": "Jauh Lebih Rendah Dari Jendela Persegi." },
  { "en": "Kekurangan Jendela Hanning?", "id": "Mainlobe Sedikit Lebih Lebar." },
  { "en": "Trade-off Antara Apa?", "id": "Resolusi Frekuensi Dan Bocoran Spektral." },
  { "en": "Apa Itu 'Scalloping Loss'?", "id": "Kehilangan Amplitudo Jika Sinyal Di Antara Bin." },
  { "en": "Jendela Dengan Scalloping Loss Terburuk?", "id": "Jendela Persegi." },
  { "en": "Apa Itu Interpolasi DFT?", "id": "Meningkatkan Tampilan Spektrum Dengan Zero-Padding." },
  { "en": "Apakah Ini Meningkatkan Resolusi Sebenarnya?", "id": "Tidak Hanya Tampilan Visualnya Saja." },
  { "en": "Resolusi Sebenarnya Ditingkatkan Dengan?", "id": "Menambah Durasi Pengamatan Sinyal." },
  { "en": "Apa Itu DTMF?", "id": "Dual-Tone Multi-Frequency." },
  { "en": "Sinyal DTMF Digunakan Di?", "id": "Sistem Telepon Tombol Tekan." },
  { "en": "Setiap Tombol Adalah Jumlahan?", "id": "Dua Sinyal Sinusoidal Murni." },
  { "en": "Deteksi Tombol Dilakukan Di?", "id": "Domain Frekuensi Dengan Menganalisis Spektrum." },
  { "en": "Apa Itu Spektrum Satu Sisi?", "id": "Plot Spektrum Hanya Untuk Frekuensi Positif." },
  { "en": "Apa Itu Spektrum Dua Sisi?", "id": "Plot Spektrum Untuk Frekuensi Positif Negatif." },
  { "en": "Untuk Sinyal Riil Kita Cukup Melihat?", "id": "Spektrum Satu Sisi Karena Simetri." },
  { "en": "Apa Itu Frekuensi Citra (Image Frequency)?", "id": "Frekuensi Tak Diinginkan Dalam Penerima Superheterodyne." },
  { "en": "Bagaimana Menghilangkan Frekuensi Citra?", "id": "Menggunakan Filter Image Rejection Sebelum Pencampuran." },
  { "en": "Apa Itu Pencampuran (Mixing)?", "id": "Perkalian Sinyal Dengan Osilator Lokal." },
  { "en": "Operasi Ini Adalah?", "id": "Modulasi Yang Menggeser Spektrum." },
  { "en": "Apa Itu Frekuensi Intermediate (IF)?", "id": "Frekuensi Tetap Hasil Dari Proses Pencampuran." },
  { "en": "Keuntungan Arsitektur Superheterodyne?", "id": "Filter IF Bisa Didesain Sangat Optimal." },
  { "en": "Apa Itu Fasa Noise?", "id": "Fluktuasi Acak Fasa Osilator." },
  { "en": "Fasa Noise Adalah Masalah Domain?", "id": "Waktu (Jitter) Yang Terlihat Di Frekuensi." },
  { "en": "Efeknya Pada Spektrum?", "id": "Menyebarkan Energi Sinyal Ke Frekuensi Tetangga." },
  { "en": "Apa Itu Spur?", "id": "Puncak Frekuensi Tak Diinginkan Di Spektrum." },
  { "en": "Penyebab Spur?", "id": "Non-Linearitas Atau Kebocoran Sinyal." },
  { "en": "Apa Itu Dynamic Range?", "id": "Rasio Antara Sinyal Terkuat Dan Terlemah." },
  { "en": "SFDR Adalah Ukuran?", "id": "Spurious-Free Dynamic Range." },
  { "en": "SFDR Diukur Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Deret Fourier Geometri?", "id": "Sama Dengan Bentuk Eksponensial Deret Fourier." },
  { "en": "Basisnya Adalah Fungsi?", "id": "Eksponensial Kompleks e^jnω₀t." },
  { "en": "Apa Itu Transformasi Fourier Invers?", "id": "Proses Kembali Dari Frekuensi Ke Waktu." },
  { "en": "Sintesis Fourier Adalah Nama Lain?", "id": "Transformasi Fourier Invers." },
  { "en": "Analisis Fourier Adalah Nama Lain?", "id": "Transformasi Fourier Maju." },
  { "en": "Apa Itu Transformasi Fourier Sudut-Pendek?", "id": "Nama Lain Untuk STFT." },
  { "en": "STFT Memberikan Resolusi?", "id": "Waktu-Frekuensi Yang Tetap." },
  { "en": "Transformasi Wavelet Memberikan Resolusi?", "id": "Waktu-Frekuensi Yang Adaptif." },
  { "en": "Apa Itu Resolusi Multirate?", "id": "Konsep Dibalik Transformasi Wavelet." },
  { "en": "Apa Itu Filter QMF?", "id": "Quadrature Mirror Filter." },
  { "en": "QMF Digunakan Dalam?", "id": "Bank Filter Dan Transformasi Wavelet." },
  { "en": "Apa Itu DCT?", "id": "Discrete Cosine Transform." },
  { "en": "Basis DCT Adalah?", "id": "Hanya Fungsi Kosinus." },
  { "en": "Kenapa DCT Baik Untuk Kompresi?", "id": "Sangat Efisien Memadatkan Energi Sinyal Tipikal." },
  { "en": "Apa Itu Efek Blok?", "id": "Artefak Visual Dalam Kompresi JPEG." },
  { "en": "Penyebab Efek Blok?", "id": "Kuantisasi Koefisien DCT Secara Independen." },
  { "en": "Efek Ini Terlihat Di Domain?", "id": "Domain Spasial (Gambar)." },
  { "en": "Apa Itu Sinyal Ergodi?", "id": "Sinyal Acak Dimana Rata-rata Waktu Sama Statistik." },
  { "en": "Asumsi Ergodisitas Menyederhanakan?", "id": "Pengukuran Statistik Dari Satu Realisasi Sinyal." },
  { "en": "Apa Itu Bandwidth 3dB?", "id": "Lebar Pita Dimana Daya Turun Setengah." },
  { "en": "Apa Itu Bandwidth Noise Ekuivalen?", "id": "Bandwidth Filter Ideal Dengan Daya Noise Sama." },
  { "en": "Apa Itu Faktor Puncak (Crest Factor)?", "id": "Rasio Amplitudo Puncak Terhadap Nilai RMS." },
  { "en": "Faktor Puncak Adalah Ukuran Di?", "id": "Domain Waktu." },
  { "en": "OFDM Memiliki Faktor Puncak?", "id": "Sangat Tinggi Yang Menjadi Tantangan." },
  { "en": "Apa Itu Sistem Spread Spectrum?", "id": "Sistem Yang Menyebarkan Sinyal Di Pita Lebar." },
  { "en": "Tujuannya Untuk?", "id": "Anti-Jamming Dan Komunikasi Aman." },
  { "en": "Contoh Sistem Spread Spectrum?", "id": "GPS Dan CDMA." },
  { "en": "Analisisnya Dilakukan Di?", "id": "Kedua Domain Waktu Dan Frekuensi." },
  { "en": "Apa Itu Kode Pseudo-Noise (PN)?", "id": "Kode Biner Acak Semu." },
  { "en": "Fungsi Autokorelasinya?", "id": "Mirip Impuls Di Domain Waktu." },
  { "en": "Spektrumnya Mirip Apa?", "id": "Spektrum Derau Putih Di Domain Frekuensi." },
  { "en": "Kode Ini Digunakan Untuk?", "id": "Menyebarkan Spektrum Sinyal Informasi." },
  { "en": "Esensi Fourier Adalah Dekomposisi?", "id": "Sinyal Menjadi Komponen-Komponen Ortogonal." },
  { "en": "Apa Itu Sifat Parseval?", "id": "Energi Sinyal Sama Di Domain Waktu Frekuensi." },
  { "en": "Apa Itu DFT?", "id": "Discrete Fourier Transform." },
  { "en": "DFT Adalah Alat Untuk Analisis?", "id": "Analisis Spektral Sinyal Digital." },
  { "en": "Apa Itu 'Bin' Frekuensi?", "id": "Satu Titik Sampel Di Domain Frekuensi." },
  { "en": "Resolusi Frekuensi DFT Adalah?", "id": "Jarak Antara Dua Bin Berurutan." },
  { "en": "Bagaimana Meningkatkan Resolusi Frekuensi?", "id": "Mengamati Sinyal Dalam Jendela Waktu Lebih Lama." },
  { "en": "Apa Itu Zero Padding?", "id": "Menambahkan Sampel Nol Ke Sinyal." },
  { "en": "Apa Efek Zero Padding Pada Spektrum?", "id": "Menginterpolasi Tampilan Spektrum Menjadi Lebih Halus." },
  { "en": "Apakah Zero Padding Meningkatkan Resolusi?", "id": "Tidak Hanya Tampilan Visualnya Saja." },
  { "en": "Apa Itu Bocoran Spektral (Spectral Leakage)?", "id": "Energi Sinyal Bocor Ke Bin Tetangga." },
  { "en": "Penyebab Bocoran Spektral?", "id": "Pemotongan Sinyal Tiba-tiba Di Domain Waktu." },
  { "en": "Apa Itu Fungsi Jendela (Windowing)?", "id": "Mengalikan Sinyal Dengan Fungsi Jendela." },
  { "en": "Tujuan Fungsi Jendela?", "id": "Mengurangi Efek Bocoran Spektral." },
  { "en": "Jendela Persegi (Rectangular) Punya Bocoran?", "id": "Sangat Buruk." },
  { "en": "Jendela Hanning Atau Hamming Punya Bocoran?", "id": "Jauh Lebih Baik Daripada Jendela Persegi." },
  { "en": "Konsekuensi Menggunakan Jendela Halus?", "id": "Resolusi Frekuensi Sedikit Menurun." },
  { "en": "Apa Itu Scalloping Loss?", "id": "Kesalahan Amplitudo Jika Sinyal Di Antara Bin." },
  { "en": "Jendela Mana Dengan Scalloping Loss Terburuk?", "id":- "Jendela Persegi." },
  { "en": "Apa Itu Konvolusi Sirkular?", "id": "Hasil Dari Perkalian Langsung Dua DFT." },
  { "en": "Bagaimana Melakukan Konvolusi Linier Dengan FFT?", "id": "Menggunakan Zero-Padding Yang Cukup." },
  { "en": "Apa Itu DTFT?", "id": "Discrete-Time Fourier Transform." },
  { "en": "Apa Beda DTFT Dan DFT?", "id": "Spektrum DTFT Kontinu DFT Diskrit." },
  { "en": "Spektrum DTFT Selalu?", "id": "Periodik Dengan Periode Frekuensi Sampling." },
  { "en": "DFT Adalah Sampel Dari?", "id": "Satu Periode Spektrum DTFT." },
  { "en": "Apa Itu Aliasing?", "id": "Frekuensi Tinggi Muncul Sebagai Frekuensi Rendah." },
  { "en": "Penyebab Aliasing?", "id": "Sampling Di Bawah Laju Nyquist." },
  { "en": "Apa Itu Laju Nyquist?", "id": "Dua Kali Frekuensi Maksimum Sinyal." },
  { "en": "Apa Itu Filter Anti-Aliasing?", "id": "Filter Low-Pass Sebelum Proses Sampling." },
  { "en": "Apa Itu Sifat Dualitas?", "id": "Bentuk Operasi Di Satu Domain Mirip Lainnya." },
  { "en": "Dualitas Sinyal Kotak?", "id": "Adalah Fungsi Sinc." },
  { "en": "Dualitas Sinyal Impuls?", "id": "Adalah Konstanta Datar." },
  { "en": "Dualitas Sinyal Gaussian?", "id": "Adalah Sinyal Gaussian Lainnya." },
  { "en": "Apa Itu Autokorelasi?", "id": "Korelasi Sinyal Dengan Dirinya Sendiri." },
  { "en": "Apa Itu Cross-Korelasi?", "id": "Korelasi Antara Dua Sinyal Berbeda." },
  { "en": "Transformasi Fourier Dari Autokorelasi?", "id": "Adalah Kepadatan Spektral Daya (PSD)." },
  { "en": "Teorema Ini Bernama?", "id": "Teorema Wiener-Khinchin." },
  { "en": "PSD Berguna Untuk Sinyal?", "id": "Sinyal Acak Stasioner." },
  { "en": "Apa Itu Sinyal Stasioner?", "id": "Sifat Statistiknya Tidak Berubah Seiring Waktu." },
  { "en": "Apa Itu Sistem Kausal?", "id": "Output Bergantung Pada Input Saat Ini Lalu." },
  { "en": "Sistem Fisik Selalu?", "id": "Kausal." },
  { "en": "Apa Itu Filter FIR?", "id": "Finite Impulse Response." },
  { "en": "Apa Itu Filter IIR?", "id": "Infinite Impulse Response." },
  { "en": "Filter FIR Selalu Stabil?", "id": "Ya Selalu Stabil." },
  { "en": "Filter IIR Bisa Tidak Stabil?", "id": "Ya Jika Pole Di Luar Lingkaran Satuan." },
  { "en": "Filter FIR Bisa Punya Fasa Linier?", "id": "Ya Dengan Koefisien Simetris." },
  { "en": "Filter IIR Bisa Punya Fasa Linier?", "id": "Tidak Mungkin Secara Sempurna." },
  { "en": "Mana Yang Lebih Efisien?", "id": "Filter IIR Untuk Respon Tajam." },
  { "en": "Implementasi FIR Di Waktu?", "id": "Konvolusi." },
  { "en": "Implementasi IIR Di Waktu?", "id": "Persamaan Beda Rekursif." },
  { "en": "Apa Itu Fasa Minimum?", "id": "Sistem Stabil Yang Inversnya Juga Stabil." },
  { "en": "Sistem Fasa Minimum Punya Zero?", "id": "Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu Fasa Berlebih (Excess Phase)?", "id": "Fasa Tambahan Dari Komponen All-Pass." },
  { "en": "Apa Itu Sistem All-Pass?", "id": "Sistem Dengan Magnitudo Respon Frekuensi Datar." },
  { "en": "Apa Itu Group Delay?", "id": "Ukuran Penundaan Amplop Sinyal." },
  { "en": "Filter Fasa Linier Punya Group Delay?", "id": "Konstan." },
  { "en": "Apa Itu Distorsi Fasa?", "id": "Group Delay Tidak Konstan." },
  { "en": "Efek Distorsi Fasa?", "id": "Mengubah Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu STFT?", "id": "Short-Time Fourier Transform." },
  { "en": "STFT Digunakan Untuk Sinyal?", "id": "Non-Stasioner." },
  { "en": "STFT Adalah Dasar Dari?", "id": "Pembuatan Spektogram." },
  { "en": "Apa Itu Spektogram?", "id": "Plot Frekuensi Terhadap Waktu." },
  { "en": "Kecerahan Spektogram Menunjukkan?", "id": "Kekuatan Sinyal." },
  { "en": "Trade-off Dalam STFT?", "id": "Antara Resolusi Waktu Dan Resolusi Frekuensi." },
  { "en": "Jendela Waktu Pendek Memberi?", "id": "Resolusi Waktu Baik Resolusi Frekuensi Buruk." },
  { "en": "Jendela Waktu Panjang Memberi?", "id": "Resolusi Frekuensi Baik Resolusi Waktu Buruk." },
  { "en": "Apa Itu Transformasi Wavelet?", "id": "Analisis Waktu-Frekuensi Dengan Resolusi Adaptif." },
  { "en": "Wavelet Menggunakan Fungsi Basis?", "id": "Berbentuk Gelombang Kecil (Wavelet)." },
  { "en": "Kelebihan Wavelet?", "id": "Baik Menganalisis Transien Dan Frekuensi Rendah." },
  { "en": "Apa Itu DCT?", "id": "Discrete Cosine Transform." },
  { "en": "DCT Adalah Transformasi Bernilai?", "id": "Riil." },
  { "en": "DCT Sangat Baik Untuk?", "id": "Memadatkan Energi Sinyal Gambar Dan Suara." },
  { "en": "Kompresi JPEG Menggunakan?", "id": "DCT Dua Dimensi." },
  { "en": "Apa Itu Kuantisasi?", "id": "Pembulatan Nilai Ke Level Diskrit." },
  { "en": "Kompresi Terjadi Saat?", "id": "Koefisien DCT Dikuantisasi." },
  { "en": "Apa Itu Cepstrum?", "id": "Transformasi Fourier Dari Logaritma Spektrum." },
  { "en": "Sumbu X Cepstrum Disebut?", "id": "Quefrency." },
  { "en": "Cepstrum Berguna Untuk Deteksi?", "id": "Pitch Suara Dan Gema." },
  { "en": "Apa Itu Modulasi?", "id": "Menggeser Spektrum Sinyal Ke Frekuensi Pembawa." },
  { "en": "Apa Itu Demodulasi?", "id": "Mengembalikan Spektrum Sinyal Ke Frekuensi Asal." },
  { "en": "Apa Itu Superheterodyne?", "id": "Arsitektur Penerima Radio Umum." },
  { "en": "Prinsipnya Adalah Mengubah RF Ke?", "id": "Frekuensi Menengah (IF) Yang Tetap." },
  { "en": "Keuntungannya Adalah?", "id": "Filter Dan Amplifier IF Sangat Optimal." },
  { "en": "Apa Itu Sinyal Analitik?", "id": "Sinyal Kompleks Dengan Spektrum Satu Sisi." },
  { "en": "Bagian Imajiner Adalah?", "id": "Transformasi Hilbert Dari Bagian Riil." },
  { "en": "Apa Itu Transformasi Hilbert?", "id": "Filter All-Pass Penggeser Fasa -90 Derajat." },
  { "en": "Sinyal Analitik Berguna Untuk?", "id": "Menghitung Amplop Dan Frekuensi Sesaat." },
  { "en": "Apa Itu OFDM?", "id": "Orthogonal Frequency Division Multiplexing." },
  { "en": "OFDM Menggunakan IFFT Untuk?", "id": "Modulasi." },
  { "en": "OFDM Menggunakan FFT Untuk?", "id":- "Demodulasi." },
  { "en": "Keuntungan OFDM?", "id": "Sangat Kuat Melawan Distorsi Multipath." },
  { "en": "Apa Itu Cyclic Prefix?", "id": "Salinan Akhir Simbol Ditambahkan Ke Awal." },
  { "en": "Tujuannya Untuk?", "id": "Mengatasi Interferensi Antar Simbol." },
  { "en": "Apa Itu PAPR?", "id": "Peak-to-Average Power Ratio." },
  { "en": "PAPR Tinggi Adalah Masalah?", "id": "Dalam Sinyal OFDM." },
  { "en": "Sifat Konjugasi Di Waktu?", "id": "Menjadi Konjugasi Dan Pembalikan Di Frekuensi." },
  { "en": "Sifat Diferensiasi Di Frekuensi?", "id": "Perkalian Dengan -jt Di Domain Waktu." },
  { "en": "Apa Itu Spektrum Daya Kumulatif?", "id": "Integral Dari PSD." },
  { "en": "Nilai Akhirnya Adalah?", "id": "Total Daya Rata-rata Sinyal." },
  { "en": "Apa Itu Sifat Pergeseran Sirkular?", "id": "Pergeseran Periodik Di Waktu." },
  { "en": "Efeknya Pada Spektrum DFT?", "id": "Perkalian Dengan Eksponensial Kompleks." },
  { "en": "Apa Itu Sifat Perkalian Sirkular?", "id": "Perkalian Titik Demi Titik Di Waktu." },
  { "en": "Efeknya Pada Spektrum DFT?", "id": "Konvolusi Sirkular Di Domain Frekuensi." },
  { "en": "Apa Itu Sinyal Real Dan Genap?", "id": "Spektrumnya Real Dan Genap." },
  { "en": "Apa Itu Sinyal Real Dan Ganjil?", "id": "Spektrumnya Imajiner Dan Ganjil." },
  { "en": "Apa Itu Bagian Riil Spektrum?", "id": "Transformasi Dari Bagian Genap Sinyal." },
  { "en": "Apa Itu Bagian Imajiner Spektrum?", "id": "Transformasi Dari Bagian Ganjil Sinyal." },
  { "en": "Spektrum Daya Adalah Transformasi Dari?", "id": "Fungsi Autokorelasi." },
  { "en": "Fungsi Autokorelasi Adalah Operasi?", "id": "Domain Waktu." },
  { "en": "Spektrum Daya Adalah Representasi?", "id": "Domain Frekuensi." },
  { "en": "Hubungan Ini Disebut Teorema?", "id": "Teorema Wiener-Khinchin." },
  { "en": "Apa Itu Derau Putih (White Noise)?", "id": "Sinyal Acak Dengan Spektrum Datar." },
  { "en": "Autokorelasi Derau Putih?", "id": "Fungsi Impuls Sempurna Di Waktu." },
  { "en": "Derau Putih Tidak Terkorelasi?", "id": "Ya Antar Sampel Waktu Yang Berbeda." },
  { "en": "Apa Itu Filter Wiener?", "id": "Filter Optimal Untuk Mengurangi Derau." },
  { "en": "Desain Filter Wiener Membutuhkan?", "id": "Informasi Spektral Sinyal Dan Derau." },
  { "en": "Apa Itu Equalizer?", "id": "Filter Untuk Mengkompensasi Distorsi Kanal." },
  { "en": "Equalizer Bekerja Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Distorsi Kanal?", "id": "Respon Frekuensi Kanal Yang Tidak Datar." },
  { "en": "Efeknya Di Domain Waktu?", "id": "Interferensi Antar Simbol (ISI)." },
  { "en": "Apa Itu Zero-Forcing Equalizer?", "id": "Mencoba Membuat Respon Total Menjadi Impuls." },
  { "en": "Kelemahannya Adalah?", "id": "Bisa Sangat Menguatkan Derau." },
  { "en": "Apa Itu MMSE Equalizer?", "id": "Minimum Mean Square Error Equalizer." },
  { "en": "Tujuannya Adalah Meminimalkan?", "id": "Error Kuadrat Rata-rata Di Output." },
  { "en": "Apa Itu Channel Sounding?", "id": "Proses Mengukur Respon Frekuensi Kanal." },
  { "en": "Bagaimana Caranya?", "id": "Mengirim Sinyal Pilot Yang Diketahui." },
  { "en": "Apa Itu Sinyal Pilot?", "id": "Sinyal Dengan Sifat Spektral Yang Dikenal." },
  { "en": "Apa Itu Spektrum Tersebar (Spread Spectrum)?", "id": "Menyebarkan Energi Sinyal Di Pita Lebar." },
  { "en": "Keuntungan Spread Spectrum?", "id": "Tahan Terhadap Jamming Dan Interferensi." },
  { "en": "Apa Itu Direct Sequence Spread Spectrum (DSSS)?", "id": "Mengalikan Sinyal Dengan Kode PN Cepat." },
  { "en": "Operasi Ini Dilakukan Di?", "id": "Domain Waktu." },
  { "en": "Efeknya Di Domain Frekuensi?", "id": "Menyebarkan Spektrum Sinyal." },
  { "en": "Apa Itu Frequency Hopping Spread Spectrum (FHSS)?", "id": "Mengubah Frekuensi Pembawa Secara Cepat." },
  { "en": "Pola Lompatan Ditentukan Oleh?", "id": "Kode Pseudo-Noise (PN)." },
  { "en": "Bluetooth Menggunakan Teknik?", "id": "Frequency Hopping Spread Spectrum (FHSS)." },
  { "en": "Wifi (802.11b/g) Menggunakan Teknik?", "id": "Direct Sequence Spread Spectrum (DSSS)." },
  { "en": "GPS Menggunakan Teknik?", "id": "DSSS Untuk Kekuatan Sinyal." },
  { "en": "Apa Itu Sinyal Waktu-Singkat?", "id": "Sinyal Dengan Durasi Waktu Sangat Pendek." },
  { "en": "Spektrumnya Akan Sangat?", "id": "Sangat Lebar Di Domain Frekuensi." },
  { "en": "Apa Itu Sinyal Frekuensi-Singkat?", "id": "Sinyal Dengan Bandwidth Sangat Sempit." },
  { "en": "Bentuknya Di Domain Waktu?", "id": "Berlangsung Sangat Lama." },
  { "en": "Hubungan Ini Adalah Inti?", "id": "Prinsip Ketidakpastian Fourier." },
  { "en": "Apa Itu Radix-2 FFT?", "id": "Algoritma FFT Untuk Panjang Data Pangkat Dua." },
  { "en": "Apa Itu Mixed-Radix FFT?", "id": "Algoritma FFT Untuk Panjang Data Komposit." },
  { "en": "Apa Itu Prime-Factor Algorithm?", "id": "Algoritma FFT Lainnya." },
  { "en": "Tujuan Semuanya Adalah?", "id": "Mengurangi Jumlah Operasi Komputasi." },
  { "en": "DFT Membutuhkan Operasi?", "id": "Sekitar N Kuadrat." },
  { "en": "FFT Membutuhkan Operasi?", "id": "Sekitar N Log N." },
  { "en": "Untuk N Besar Perbedaannya?", "id": "Sangat Dramatis Dan Signifikan." },
  { "en": "Apa Itu Transformasi Hadamard?", "id": "Transformasi Berbasis Fungsi Kotak (Walsh-Hadamard)." },
  { "en": "Basisnya Bukan Sinusoid?", "id": "Bukan Basisnya Adalah Fungsi Walsh." },
  { "en": "Keuntungannya Adalah?", "id": "Tidak Membutuhkan Perkalian Hanya Penjumlahan." },
  { "en": "Apa Itu Transformasi Slant?", "id": "Varian Lain Dari Transformasi Diskrit." },
  { "en": "Apa Itu Filter Digital?", "id": "Operasi Matematis Pada Sinyal Digital." },
  { "en": "Tujuannya Memodifikasi Apa?", "id": "Spektrum Sinyal Di Domain Frekuensi." },
  { "en": "Implementasinya Di Domain?", "id": "Waktu (Sebagai Persamaan Beda)." },
  { "en": "Apa Itu Lingkaran Satuan (Unit Circle)?", "id": "Lingkaran Jari-jari Satu Di Bidang Kompleks." },
  { "en": "Hubungannya Dengan DTFT?", "id": "Spektrum DTFT Adalah Evaluasi Di Lingkaran Satuan." },
  { "en": "Ini Terkait Dengan Transformasi?", "id": "Transformasi-Z." },
  { "en": "Transformasi-Z Adalah Versi Diskrit?", "id": "Dari Transformasi Laplace." },
  { "en": "Apa Itu Lingkaran Konvergensi (ROC)?", "id": "Daerah Konvergensi Untuk Transformasi-Z." },
  { "en": "Stabilitas Sistem Diskrit Ditentukan Oleh?", "id": "Posisi Pole Relatif Terhadap Lingkaran Satuan." },
  { "en": "Sistem Stabil Jika Semua Pole?", "id": "Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu Sinyal Multidimensional?", "id": "Sinyal Yang Bergantung Pada Beberapa Variabel." },
  { "en": "Contoh Sinyal 2D?", "id": "Gambar Diam." },
  { "en": "Contoh Sinyal 3D?", "id": "Video Atau Data Medis (MRI)." },
  { "en": "Analisisnya Menggunakan?", "id": "Transformasi Fourier Multidimensional." },
  { "en": "Apa Itu Frekuensi Spasial?", "id": "Ukuran Pengulangan Pola Dalam Ruang." },
  { "en": "Frekuensi Spasial Rendah Adalah?", "id": "Area Halus Dan Berubah Lambat." },
  { "en": "Frekuensi Spasial Tinggi Adalah?", "id": "Tepi Tajam Dan Tekstur Detail." },
  { "en": "Operasi 'Blur' Pada Gambar?", "id": "Filter Low-Pass Spasial." },
  { "en": "Operasi 'Sharpen' Pada Gambar?", "id":- "Filter High-Pass Spasial." },
  { "en": "Apa Itu Sinyal Seismik?", "id": "Getaran Yang Merambat Melalui Bumi." },
  { "en": "Analisis Frekuensinya Untuk?", "id": "Menentukan Struktur Lapisan Bawah Permukaan." },
  { "en": "Apa Itu Interpolasi Frekuensi?", "id": "Sama Dengan Zero-Padding Di Domain Waktu." },
  { "en": "Apa Itu Sinyal Ucapan (Speech)?", "id": "Sinyal Non-Stasioner Yang Kompleks." },
  { "en": "Terdiri Dari Bunyi?", "id": "Vokal (Bersuara) Dan Konsonan (Tak Bersuara)." },
  { "en": "Bunyi Vokal Di Spektrum?", "id": "Memiliki Struktur Harmonik Yang Jelas." },
  { "en": "Bunyi Konsonan Di Spektrum?", "id": "Lebih Mirip Derau (Noise-like)." },
  { "en": "Apa Itu Formant?", "id": "Puncak Resonansi Dalam Spektrum Vokal." },
  { "en": "Formant Ditentukan Oleh?", "id": "Bentuk Saluran Vokal Manusia." },
  { "en": "Pengenalan Ucapan Bekerja Di?", "id": "Domain Waktu-Frekuensi." },
  { "en": "Apa Itu MFCC?", "id": "Mel-Frequency Cepstral Coefficients." },
  { "en": "MFCC Adalah Fitur Dari?", "id": "Spektrum Sinyal Ucapan." },
  { "en": "Digunakan Secara Luas Di?", "id": "Sistem Pengenalan Ucapan Otomatis." },
  { "en": "Skala Mel Adalah?", "id": "Skala Frekuensi Non-Linier Mirip Pendengaran." },
  { "en": "Apa Itu Koherensi Spektral?", "id": "Ukuran Korelasi Linier Di Domain Frekuensi." },
  { "en": "Nilai Satu Berarti?", "id": "Dua Sinyal Sangat Terkorelasi Linier." },
  { "en": "Nilai Nol Berarti?", "id": "Dua Sinyal Tidak Terkorelasi Linier." },
  { "en": "Apa Itu Sinyal Elektrokardiogram (EKG)?", "id": "Sinyal Listrik Dari Aktivitas Jantung." },
  { "en": "Analisis Waktunya Untuk?", "id": "Melihat Ritme Dan Interval (P-QRS-T)." },
  { "en": "Analisis Frekuensinya Untuk?", "id": "Mendeteksi Variabilitas Denyut Jantung (HRV)." },
  { "en": "Apa Itu Sinyal Elektroensefalogram (EEG)?", "id": "Sinyal Listrik Dari Aktivitas Otak." },
  { "en": "Analisis Frekuensinya Untuk?", "id": "Melihat Pita Gelombang Otak (Alpha Beta)." },
  { "en": "Pita Frekuensi Ini Berhubungan Dengan?", "id": "Kondisi Mental (Rileks Fokus Tidur)." },
  { "en": "Apa Itu 'Papar' Sinyal OFDM?", "id": "Sama Dengan PAPR (Peak-to-Average Power Ratio)." },
  { "en": "Apa Ide Mendasar Analisis Fourier?", "id": "Memecah Sesuatu Yang Kompleks Menjadi Sederhana." },
  { "en": "Komponen Sederhananya Adalah?", "id": "Gelombang Sinus Dan Kosinus Murni." },
  { "en": "Apa Itu Spektrum Diskrit?", "id": "Energi Hanya Ada Di Frekuensi Tertentu." },
  { "en": "Sinyal Apa Yang Punya Spektrum Diskrit?", "id": "Sinyal Periodik Seperti Gelombang Kotak." },
  { "en": "Apa Itu Spektrum Kontinu?", "id": "Energi Ada Di Semua Frekuensi." },
  { "en": "Sinyal Apa Yang Punya Spektrum Kontinu?", "id": "Sinyal Aperiodik Seperti Pulsa Tunggal." },
  { "en": "Transformasi Fourier Adalah Operator Linier?", "id": "Ya Memenuhi Sifat Superposisi." },
  { "en": "Apa Arti Fisis Dari Fasa?", "id": "Pergeseran Awal Sinyal Relatif Terhadap Waktu." },
  { "en": "Fasa Nol Berarti Sinyal?", "id": "Dimulai Pada Puncak Maksimumnya." },
  { "en": "Dua Sinyal Dengan Spektrum Magnitudo Sama?", "id": "Bisa Terdengar Sama Tapi Berbentuk Berbeda." },
  { "en": "Perbedaan Bentuknya Ditentukan Oleh?", "id": "Spektrum Fasa Yang Berbeda." },
  { "en": "Apa Itu Sinyal Baseband?", "id": "Spektrum Terpusat Di Sekitar Frekuensi Nol." },
  { "en": "Apa Itu Sinyal Passband?", "id": "Spektrum Terpusat Di Sekitar Frekuensi Pembawa." },
  { "en": "Proses Mengubah Baseband Ke Passband?", "id": "Modulasi." },
  { "en": "Proses Mengubah Passband Ke Baseband?", "id": "Demodulasi." },
  { "en": "Apa Itu Prinsip Ketidakpastian?", "id": "Tidak Bisa Tahu Waktu Frekuensi Tepat." },
  { "en": "Sinyal Terlokalisasi Di Waktu?", "id": "Spektrumnya Tersebar Luas Di Frekuensi." },
  { "en": "Sinyal Terlokalisasi Di Frekuensi?", "id": "Bentuknya Tersebar Luas Di Waktu." },
  { "en": "Sinyal Gaussian Adalah Kompromi Terbaik?", "id": "Ya Meminimalkan Produk Ketidakpastian Waktu-Frekuensi." },
  { "en": "Apa Efek Konvolusi Di Waktu?", "id": "Efek Filtering Atau Penghalusan." },
  { "en": "Di Domain Frekuensi Ini Setara?", "id": "Perkalian Spektrum Input Dengan Respon Frekuensi." },
  { "en": "Respon Impuls Adalah 'DNA' Sistem Di?", "id": "Domain Waktu." },
  { "en": "Respon Frekuensi Adalah 'DNA' Sistem Di?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Sistem Invarian Waktu?", "id": "Karakteristiknya Tidak Berubah Seiring Waktu." },
  { "en": "Apa Itu Sistem Linier?", "id": "Output Proporsional Terhadap Input." },
  { "en": "Fourier Menyederhanakan Analisis Sistem?", "id": "Sistem Linier Invarian Waktu (LTI)." },
  { "en": "Apa Itu Sampling?", "id": "Mengukur Sinyal Kontinu Secara Periodik." },
  { "en": "Efek Sampling Di Domain Frekuensi?", "id": "Membuat Spektrum Asli Berulang Kembali." },
  { "en": "Apa Itu Aliasing?", "id": "Tumpang Tindih Antara Spektrum Yang Berulang." },
  { "en": "Bagaimana Menghindari Aliasing?", "id": "Frekuensi Sampling Harus Cukup Tinggi." },
  { "en": "Aturan Ini Dikenal Sebagai?", "id": "Kriteria Sampling Nyquist." },
  { "en": "Apa Itu Laju Nyquist?", "id": "Dua Kali Frekuensi Maksimum Dalam Sinyal." },
  { "en": "Apa Itu Filter Anti-Aliasing?", "id": "Filter Low-Pass Sebelum Proses Sampling." },
  { "en": "Tujuannya Untuk?", "id": "Memastikan Sinyal Menjadi Pita-Terbatas." },
  { "en": "Apa Itu DTFT?", "id": "Discrete-Time Fourier Transform." },
  { "en": "DTFT Untuk Sinyal Apa?", "id": "Sinyal Waktu-Diskrit Aperiodik." },
  { "en": "Spektrum DTFT Berbentuk?", "id": "Kontinu Dan Periodik." },
  { "en": "Periode Spektrum DTFT?", "id": "Sama Dengan Frekuensi Sampling." },
  { "en": "Apa Itu DFT?", "id": "Discrete Fourier Transform." },
  { "en": "DFT Adalah Versi Komputasi?", "id": "Dari Analisis Fourier." },
  { "en": "DFT Adalah Sampel Dari?", "id": "Spektrum DTFT." },
  { "en": "Apa Itu FFT?", "id": "Algoritma Cepat Untuk Menghitung DFT." },
  { "en": "Apa Itu Jendela Waktu?", "id": "Durasi Sinyal Yang Dianalisis Oleh DFT." },
  { "en": "Jendela Lebih Panjang Memberi Resolusi?", "id": "Resolusi Frekuensi Lebih Baik." },
  { "en": "Jendela Lebih Pendek Memberi Resolusi?", "id": "Resolusi Waktu Lebih Baik." },
  { "en": "Apa Itu Bocoran Spektral?", "id": "Energi Sinyal Bocor Ke Frekuensi Lain." },
  { "en": "Penyebabnya Adalah?", "id": "Pemotongan Sinyal Di Domain Waktu." },
  { "en": "Solusinya Adalah Menggunakan?", "id": "Fungsi Jendela (Windowing)." },
  { "en": "Apa Itu Sinyal Stasioner?", "id": "Sifat Statistiknya Konstan Seiring Waktu." },
  { "en": "Apa Itu Sinyal Non-Stasioner?", "id": "Sifat Statistiknya Berubah Seiring Waktu." },
  { "en": "Fourier Klasik Untuk Sinyal?", "id": "Sinyal Stasioner." },
  { "en": "Analisis Sinyal Non-Stasioner?", "id": "Menggunakan STFT Atau Transformasi Wavelet." },
  { "en": "STFT Adalah Deretan?", "id": "FFT Dari Blok Sinyal Yang Tumpang Tindih." },
  { "en": "Hasilnya Adalah?", "id": "Spektogram (Plot Waktu-Frekuensi)." },
  { "en": "Apa Itu Respon Frekuensi Datar?", "id": "Sistem Melewatkan Semua Frekuensi Dengan Sama." },
  { "en": "Respon Impulsnya Berbentuk?", "id": "Impuls Dirac." },
  { "en": "Apa Itu Fasa Linier?", "id": "Fasa Berubah Linier Dengan Frekuensi." },
  { "en": "Efeknya Pada Sinyal Waktu?", "id": "Hanya Penundaan Murni Tanpa Distorsi." },
  { "en": "Respon Impuls Sistem Fasa Linier?", "id": "Simetris Terhadap Pusatnya." },
  { "en": "Apa Itu Distorsi?", "id": "Perubahan Bentuk Sinyal Yang Tidak Diinginkan." },
  { "en": "Distorsi Amplitudo Disebabkan Oleh?", "id": "Respon Frekuensi Yang Tidak Datar." },
  { "en": "Distorsi Fasa Disebabkan Oleh?", "id": "Respon Fasa Yang Tidak Linier." },
  { "en": "Apa Itu Sistem Kausal?", "id": "Output Tidak Mendahului Input." },
  { "en": "Sistem Dunia Nyata Selalu?", "id": "Kausal." },
  { "en": "Respon Impuls Sistem Kausal?", "id": "Nol Untuk Waktu Negatif." },
  { "en": "Hubungan Kramers-Kronig Menghubungkan?", "id": "Bagian Riil Dan Imajiner Respon Frekuensi." },
  { "en": "Hubungan Ini Berlaku Untuk?", "id": "Sistem Kausal." },
  { "en": "Artinya Magnitudo Dan Fasa?", "id": "Tidak Sepenuhnya Independen." },
  { "en": "Apa Itu Sinyal Analitik?", "id": "Sinyal Kompleks Dengan Spektrum Satu Sisi." },
  { "en": "Bagian Imajiner Adalah?", "id": "Transformasi Hilbert Dari Bagian Riil." },
  { "en": "Sinyal Analitik Berguna Untuk?", "id": "Menghitung Amplop Dan Frekuensi Sesaat." },
  { "en": "Apa Itu Amplop?", "id": "Garis Batas Amplitudo Sinyal." },
  { "en": "Apa Itu Frekuensi Sesaat?", "id": "Frekuensi Sinyal Pada Setiap Momen Waktu." },
  { "en": "Keduanya Adalah Properti Domain?", "id": "Domain Waktu." },
  { "en": "Apa Itu Autokorelasi?", "id": "Ukuran Kemiripan Sinyal Dengan Versi Gesernya." },
  { "en": "Transformasi Fourier Dari Autokorelasi?", "id": "Kepadatan Spektral Daya." },
  { "en": "Teorema Ini Disebut?", "id": "Teorema Wiener-Khinchin." },
  { "en": "PSD Berguna Untuk Sinyal?", "id": "Acak Dan Stasioner." },
  { "en": "Spektrum Gelombang Kotak?", "id": "Mengandung Harmonik Ganjil." },
  { "en": "Amplitudo Harmonik Meluruh Sebanding?", "id": "Satu Dibagi n (1/n)." },
  { "en": "Spektrum Gelombang Segitiga?", "id": "Juga Mengandung Harmonik Ganjil." },
  { "en": "Amplitudo Harmonik Meluruh Sebanding?", "id": "Satu Dibagi n Kuadrat (1/n²)." },
  { "en": "Sinyal Lebih Halus Di Waktu?", "id": "Spektrumnya Meluruh Lebih Cepat Di Frekuensi." },
  { "en": "Apa Itu Kompresi Data?", "id": "Mengurangi Jumlah Bit Untuk Representasi Sinyal." },
  { "en": "Kompresi Lossy Bekerja Di?", "id": "Domain Frekuensi (JPEG MP3)." },
  { "en": "Prinsipnya Adalah Membuang?", "id": "Koefisien Frekuensi Yang Dianggap Tidak Penting." },
  { "en": "Koefisien Tidak Penting Biasanya?", "id": "Koefisien Bernilai Kecil." },
  { "en": "Telinga Dan Mata Manusia?", "id": "Kurang Sensitif Terhadap Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Derau Putih?", "id": "Spektrum Daya Datar Di Semua Frekuensi." },
  { "en": "Apa Itu Derau Merah Jambu?", "id": "Spektrum Daya Menurun 3dB Per Oktaf." },
  { "en": "Apa Itu OFDM?", "id": "Teknik Modulasi Untuk Komunikasi Digital Modern." },
  { "en": "OFDM Menggunakan IFFT Dan FFT?", "id": "Ya Untuk Modulasi Dan Demodulasi." },
  { "en": "Apa Itu Simbol OFDM?", "id": "Satu Blok Data Di Domain Waktu." },
  { "en": "Setiap Simbol Dibangun Di?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Equalization?", "id": "Mengkompensasi Efek Kanal Komunikasi." },
  { "en": "Equalization Paling Mudah Dilakukan Di?", "id": "Domain Frekuensi." },
  { "en": "Bagaimana Caranya?", "id": "Membagi Spektrum Diterima Dengan Respon Frekuensi Kanal." },
  { "en": "Apa Ide Mendasar Di Balik Fourier?", "id": "Sinyal Kompleks Adalah Jumlahan Sinus Sederhana." },
  { "en": "Domain Waktu Menunjukkan Sinyal Vs?", "id": "Waktu." },
  { "en": "Domain Frekuensi Menunjukkan Sinyal Vs?", "id": "Frekuensi." },
  { "en": "Alat Matematisnya Adalah?", "id": "Deret Dan Transformasi Fourier." },
  { "en": "Deret Fourier Untuk Sinyal?", "id": "Sinyal Periodik." },
  { "en": "Transformasi Fourier Untuk Sinyal?", "id": "Sinyal Aperiodik." },
  { "en": "Apa Itu Harmonik?", "id": "Kelipatan Bulat Dari Frekuensi Dasar." },
  { "en": "Spektrum Sinyal Periodik Selalu?", "id": "Diskrit." },
  { "en": "Spektrum Sinyal Aperiodik Selalu?", "id": "Kontinu." },
  { "en": "Apa Itu Komponen DC?", "id": "Nilai Rata-rata Sinyal (Frekuensi Nol)." },
  { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Di Domain Waktu." },
  { "en": "Di Domain Frekuensi Konvolusi Menjadi?", "id": "Perkalian." },
  { "en": "Sifat Ini Sangat Berguna Untuk?", "id": "Analisis Sistem Linier Invarian Waktu." },
  { "en": "Output Sistem LTI Adalah?", "id": "Konvolusi Input Dan Respon Impuls." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Transformasi Fourier Dari Respon Impuls." },
  { "en": "Sinyal Impuls Di Waktu?", "id": "Spektrum Datar Di Frekuensi." },
  { "en": "Sinyal DC Di Waktu?", "id": "Spektrum Impuls Di Frekuensi Nol." },
  { "en": "Pergeseran Di Waktu Menyebabkan?", "id": "Pergeseran Fasa Di Frekuensi." },
  { "en": "Kompresi Di Waktu Menyebabkan?", "id": "Ekspansi Di Frekuensi." },
  { "en": "Apa Itu Prinsip Dualitas?", "id": "Simetri Antara Operasi Waktu Dan Frekuensi." },
  { "en": "Apa Itu Teorema Parseval?", "id": "Energi Sinyal Sama Di Kedua Domain." },
  { "en": "Apa Itu PSD?", "id": "Power Spectral Density (Untuk Sinyal Acak)." },
  { "en": "Apa Itu ESD?", "id": "Energy Spectral Density (Untuk Sinyal Deterministik)." },
  { "en": "Spektrum Sinyal Riil Selalu?", "id": "Simetris Konjugat (Hermitian)." },
  { "en": "Magnitudonya Selalu?", "id": "Genap." },
  { "en": "Fasanya Selalu?", "id": "Ganjil." },
  { "en": "Apa Itu DTFT?", "id": "Transformasi Fourier Untuk Sinyal Diskrit." },
  { "en": "Hasil Spektrum DTFT?", "id": "Kontinu Dan Periodik." },
  { "en": "Apa Itu DFT?", "id": "Versi Diskrit Dari DTFT." },
  { "en": "Hasil Spektrum DFT?", "id": "Diskrit Dan Periodik." },
  { "en": "Apa Itu FFT?", "id": "Algoritma Cepat Untuk Menghitung DFT." },
  { "en": "Apa Itu Aliasing?", "id": "Kesalahan Akibat Sampling Terlalu Lambat." },
  { "en": "Syarat Menghindari Aliasing?", "id": "Teorema Sampling Nyquist." },
  { "en": "Apa Itu Bocoran Spektral?", "id": "Penyebaran Energi Ke Bin Frekuensi Salah." },
  { "en": "Solusinya Adalah Menggunakan?", "id": "Fungsi Jendela (Windowing)." },
  { "en": "Apa Itu Filter Digital?", "id": "Sistem Diskrit Untuk Memodifikasi Spektrum." },
  { "en": "Apa Itu FIR Dan IIR?", "id": "Dua Jenis Utama Filter Digital." },
  { "en": "Filter FIR Selalu Stabil?", "id": "Ya Benar." },
  { "en": "Filter IIR Bisa Tidak Stabil?", "id": "Ya Benar." },
  { "en": "Apa Itu Fasa Linier?", "id": "Menyebabkan Penundaan Konstan Tanpa Distorsi." },
  { "en": "Hanya Filter FIR Yang Bisa?", "id": "Memiliki Fasa Linier Sempurna." },
  { "en": "Apa Itu Spektogram?", "id": "Representasi Sinyal Di Waktu Dan Frekuensi." },
  { "en": "Berguna Untuk Sinyal?", "id": "Non-Stasioner." },
  { "en": "Apa Itu Modulasi?", "id": "Proses Menggeser Spektrum Sinyal." },
  { "en": "Apa Itu Demodulasi?", "id": "Proses Mengembalikan Spektrum Sinyal." },
  { "en": "Apa Itu FDM?", "id": "Membagi Kanal Berdasarkan Frekuensi." },
  { "en": "Apa Itu TDM?", "id": "Membagi Kanal Berdasarkan Waktu." },
  { "en": "Apa Itu Sistem Kausal?", "id": "Output Tidak Mendahului Input." },
  { "en": "Respon Impuls Sistem Kausal?", "id": "Nol Untuk Waktu Negatif." },
  { "en": "Apa Itu Autokorelasi?", "id": "Transformasi Fouriernya Adalah PSD." },
  { "en": "Apa Itu DCT?", "id": "Transformasi Kosinus Yang Digunakan Di JPEG." },
  { "en": "Apa Itu OFDM?", "id": "Modulasi Yang Menggunakan IFFT Dan FFT." },
  { "en": "Apa Itu Cyclic Prefix?", "id": "Mengatasi Masalah Multipath Di OFDM." },
  { "en": "Apa Itu Sinyal Analitik?", "id": "Dibuat Menggunakan Transformasi Hilbert." },
  { "en": "Berguna Untuk Menghitung?", "id": "Amplop Dan Frekuensi Sesaat." },
  { "en": "Apa Itu Group Delay?", "id": "Ukuran Distorsi Fasa." },
  { "en": "Group Delay Konstan Berarti?", "id": "Tidak Ada Distorsi Fasa." },
  { "en": "Apa Itu Decimation?", "id": "Mengurangi Laju Sampling." },
  { "en": "Apa Itu Interpolasi?", "id": "Meningkatkan Laju Sampling." },
  { "en": "Apa Itu Sinyal Genap?", "id": "Simetris Terhadap Sumbu Vertikal." },
  { "en": "Transformasinya Adalah?", "id": "Riil Dan Genap." },
  { "en": "Apa Itu Sinyal Ganjil?", "id": "Anti-Simetris Terhadap Titik Asal." },
  { "en": "Transformasinya Adalah?", "id": "Imajiner Dan Ganjil." },
  { "en": "Resolusi Frekuensi Bergantung Pada?", "id": "Durasi Observasi Sinyal." },
  { "en": "Observasi Lebih Lama?", "id": "Resolusi Frekuensi Lebih Baik." },
  { "en": "Apa Itu Zero Padding?", "id": "Menambah Resolusi Tampilan Spektrum." },
  { "en": "Apa Itu Efek Gibbs?", "id": "Terjadi Dekat Diskontinuitas Di Domain Waktu." },
  { "en": "Apa Itu Sinyal Stasioner?", "id": "Statistiknya Konstan Seiring Waktu." },
  { "en": "Fourier Cocok Untuk Sinyal?", "id": "Stasioner." },
  { "en": "Apa Itu Sinyal Non-Stasioner?", "id": "Statistiknya Berubah Seiring Waktu." },
  { "en": "Wavelet Cocok Untuk Sinyal?", "id": "Non-Stasioner." },
  { "en": "Apa Itu Fasa Noise?", "id": "Jitter Di Domain Waktu." },
  { "en": "Apa Itu Spur?", "id": "Harmonik Palsu Akibat Non-Linearitas." },
  { "en": "Apa Itu SFDR?", "id": "Ukuran Linearitas Di Domain Frekuensi." },
  { "en": "Apa Itu Matched Filter?", "id": "Memaksimalkan SNR Di Domain Waktu." },
  { "en": "Respon Impulsnya Adalah?", "id": "Sinyal Dicari Yang Dibalik Waktu." },
  { "en": "Apa Itu Koherensi?", "id": "Ukuran Hubungan Linier Antar Sinyal." },
  { "en": "Koherensi Diukur Di Domain?", "id": "Frekuensi." },
  { "en": "Sinyal Riil Dan Genap?", "id": "Punya Spektrum Riil Dan Genap." },
  { "en": "Sinyal Riil Dan Ganjil?", "id": "Punya Spektrum Imajiner Dan Ganjil." },
  { "en": "Apa Itu Sistem Fasa Minimum?", "id": "Stabil Dan Inversnya Stabil." },
  { "en": "Apa Itu Sistem All-Pass?", "id": "Hanya Mengubah Fasa Saja." },
  { "en": "Apa Itu Plot Bode?", "id": "Plot Logaritmik Respon Frekuensi." },
  { "en": "Kemiringan Plot Bode Terkait?", "id": "Jumlah Pole Dan Zero." },
  { "en": "Apa Itu Frekuensi Pojok?", "id": "Frekuensi Dimana Kemiringan Berubah." },
  { "en": "Ditentukan Oleh Lokasi?", "id": "Pole Dan Zero." },
  { "en": "Sinyal Kotak Di Waktu?", "id": "Punya Spektrum Berbentuk Sinc." },
  { "en": "Sinyal Sinc Di Waktu?", "id": "Punya Spektrum Berbentuk Kotak." },
  { "en": "Ini Adalah Contoh Sifat?", "id": "Dualitas." },
  { "en": "Apa Itu Cepstrum?", "id": "Alat Untuk Mendeteksi Periodisitas Spektrum." },
  { "en": "Berguna Untuk Analisis?", "id": "Pitch Suara Dan Gema." },
  { "en": "Dunia Waktu Itu Lokal?", "id": "Benar." },
  { "en": "Dunia Frekuensi Itu Global?", "id": "Benar." },
  { "en": "Mengubah Satu Sampel Waktu?", "id": "Mengubah Seluruh Spektrum." },
  { "en": "Mengubah Satu Koefisien Frekuensi?", "id": "Mengubah Seluruh Sinyal Waktu." },
  { "en": "Transformasi Fourier Adalah Jembatan?", "id": "Antara Perspektif Lokal Dan Global." },
  { "en": "Apa Ide Paling Mendasar Fourier?", "id": "Setiap Sinyal Adalah Superposisi Sinusoid." },
  { "en": "Domain Waktu Menjelaskan?", "id": "'Kapan' Sesuatu Terjadi." },
  { "en": "Domain Frekuensi Menjelaskan?", "id": "'Dari Apa' Sesuatu Terbuat." },
  { "en": "Jembatan Antar Keduanya?", "id": "Transformasi Fourier." },
  { "en": "Deret Fourier Untuk Sinyal?", "id": "Periodik Di Domain Waktu." },
  { "en": "Hasil Spektrumnya?", "id": "Diskrit Di Domain Frekuensi." },
  { "en": "Transformasi Fourier Untuk Sinyal?", "id": "Aperiodik Di Domain Waktu." },
  { "en": "Hasil Spektrumnya?", "id": "Kontinu Di Domain Frekuensi." },
  { "en": "Konvolusi Di Waktu Menjadi?", "id": "Perkalian Di Frekuensi." },
  { "en": "Sifat Ini Penting Untuk?", "id": "Analisis Sistem LTI." },
  { "en": "Pergeseran Waktu Menyebabkan Apa?", "id": "Pergeseran Fasa Di Frekuensi." },
  { "en": "Penskalaan Waktu Menyebabkan Apa?", "id": "Penskalaan Terbalik Di Frekuensi." },
  { "en": "Apa Itu Dualitas?", "id": "Simetri Antara Domain Waktu Dan Frekuensi." },
  { "en": "Sinyal Kotak Dual Dengan?", "id": "Sinyal Sinc." },
  { "en": "Sinyal Impuls Dual Dengan?", "id": "Sinyal Konstanta." },
  { "en": "Energi Sinyal Sama Di Kedua Domain?", "id": "Ya Menurut Teorema Parseval." },
  { "en": "Spektrum Sinyal Riil Punya Simetri?", "id": "Simetri Konjugat (Hermitian)." },
  { "en": "Magnitudonya Selalu?", "id": "Fungsi Genap." },
  { "en": "Fasanya Selalu?", "id": "Fungsi Ganjil." },
  { "en": "Apa Itu DFT?", "id": "Alat Komputasi Untuk Analisis Fourier." },
  { "en": "DFT Adalah Sampel Dari?", "id": "Spektrum DTFT." },
  { "en": "Apa Itu FFT?", "id": "Algoritma Cepat Untuk Menghitung DFT." },
  { "en": "Apa Itu Aliasing?", "id": "Terjadi Jika Laju Sampling Terlalu Rendah." },
  { "en": "Solusinya Adalah?", "id": "Filter Anti-Aliasing Dan Laju Nyquist." },
  { "en": "Apa Itu Bocoran Spektral?", "id": "Disebabkan Oleh Pemotongan Sinyal." },
  { "en": "Solusinya Adalah?", "id": "Menggunakan Fungsi Jendela (Windowing)." },
  { "en": "Apa Itu Filter FIR?", "id": "Selalu Stabil Bisa Punya Fasa Linier." },
  { "en": "Apa Itu Filter IIR?", "id": "Efisien Tapi Bisa Tidak Stabil." },
  { "en": "Apa Itu Fasa Linier?", "id": "Menyebabkan Penundaan Murni Tanpa Distorsi." },
  { "en": "Apa Itu Spektogram?", "id": "Menampilkan Spektrum Yang Berubah Seiring Waktu." },
  { "en": "Berguna Untuk Sinyal?", "id": "Non-Stasioner." },
  { "en": "Modulasi Menggeser Spektrum?", "id": "Ya Ke Frekuensi Pembawa." },
  { "en": "Demodulasi Mengembalikan Spektrum?", "id": "Ya Kembali Ke Pita Dasar." },
  { "en": "Sistem LTI Tidak Bisa?", "id": "Menciptakan Frekuensi Baru." },
  { "en": "Sistem Non-Linier Bisa?", "id": "Menciptakan Harmonik Dan Distorsi." },
  { "en": "Apa Itu Autokorelasi?", "id": "Transformasi Fouriernya Adalah PSD." },
  { "en": "Apa Itu PSD?", "id": "Deskripsi Spektral Untuk Sinyal Acak." },
  { "en": "Apa Itu DCT?", "id": "Inti Dari Kompresi JPEG Dan MPEG." },
  { "en": "Apa Itu OFDM?", "id": "Menggunakan IFFT/FFT Untuk Komunikasi Efisien." },
  { "en": "Apa Itu Sinyal Analitik?", "id": "Memungkinkan Perhitungan Amplop Dan Frekuensi Sesaat." },
  { "en": "Apa Itu Group Delay?", "id": "Ukuran Distorsi Fasa." },
  { "en": "Resolusi Frekuensi Bergantung Pada?", "id": "Jendela Waktu Observasi." },
  { "en": "Resolusi Waktu Bergantung Pada?", "id": "Bandwidth Sinyal." },
  { "en": "Keduanya Saling Terikat?", "id": "Ya Oleh Prinsip Ketidakpastian." },
  { "en": "Apa Itu Sinyal Genap?", "id": "Spektrumnya Bernilai Riil." },
  { "en": "Apa Itu Sinyal Ganjil?", "id": "Spektrumnya Bernilai Imajiner." },
  { "en": "Apa Itu Koefisien DC?", "id": "Nilai Rata-rata Sinyal." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "DNA Sistem Di Domain Frekuensi." },
  { "en": "Apa Itu Respon Impuls?", "id": "DNA Sistem Di Domain Waktu." },
  { "en": "Keduanya Adalah Pasangan?", "id": "Transformasi Fourier." },
  { "en": "Sinyal Gaussian Di Waktu?", "id": "Juga Gaussian Di Frekuensi." },
  { "en": "Sifat Ini Unik?", "id": "Ya Hanya Untuk Fungsi Gaussian." },
  { "en": "Apa Itu Zero Padding?", "id": "Menginterpolasi Tampilan Spektrum DFT." },
  { "en": "Perkalian DFT Adalah Konvolusi?", "id": "Sirkular." },
  { "en": "Untuk Konvolusi Linier Perlu?", "id": "Zero Padding." },
  { "en": "Apa Itu Fasa Minimum?", "id": "Sistem Dengan Penundaan Energi Terkecil." },
  { "en": "Apa Itu Sistem All-Pass?", "id": "Hanya Mengubah Fasa Tidak Mengubah Magnitudo." },
  { "en": "Apa Itu Plot Bode?", "id": "Cara Visualisasi Respon Frekuensi." },
  { "en": "Apa Itu Frekuensi Pojok?", "id": "Titik Perubahan Perilaku Di Plot Bode." },
  { "en": "Apa Itu Spread Spectrum?", "id": "Teknik Untuk Komunikasi Yang Kuat." },
  { "en": "Apa Itu DSSS Dan FHSS?", "id": "Dua Jenis Utama Spread Spectrum." },
  { "en": "Apa Itu Lingkaran Satuan?", "id": "Domain Frekuensi Untuk Sinyal Diskrit." },
  { "en": "Apa Itu Transformasi-Z?", "id": "Generalisasi DTFT Untuk Analisis Stabilitas Diskrit." },
  { "en": "Stabilitas Diskrit Dilihat Dari?", "id": "Posisi Pole Relatif Terhadap Lingkaran Satuan." },
  { "en": "Apa Itu Frekuensi Spasial?", "id": "Konsep Frekuensi Untuk Gambar." },
  { "en": "Operasi Blur Adalah?", "id": "Filter Low-Pass Spasial." },
  { "en": "Operasi Sharpen Adalah?", "id": "Filter High-Pass Spasial." },
  { "en": "Apa Itu Sinyal Chirp?", "id": "Frekuensinya Berubah Seiring Waktu." },
  { "en": "Analisisnya Membutuhkan?", "id": "Tampilan Waktu-Frekuensi." },
  { "en": "Apa Itu Cepstrum?", "id": "Berguna Untuk Analisis Pitch." },
  { "en": "Sumbu X Cepstrum Adalah?", "id": "Quefrency." },
  { "en": "Kompresi Lossy Membuang Informasi Di?", "id": "Domain Frekuensi." },
  { "en": "Dasar Pemikirannya Adalah?", "id": "Keterbatasan Persepsi Manusia." },
  { "en": "Telinga Kurang Sensitif Terhadap?", "id": "Frekuensi Sangat Tinggi Dan Fasa." },
  { "en": "Apa Itu Efek Doppler?", "id": "Pergeseran Frekuensi Akibat Gerakan." },
  { "en": "Analisisnya Dilakukan Di?", "id": "Domain Frekuensi." },
  { "en": "Sinyal Riil Dan Genap?", "id": "Punya Spektrum Riil Dan Genap." },
  { "en": "Sinyal Riil Dan Ganjil?", "id": "Punya Spektrum Imajiner Dan Ganjil." },
  { "en": "Apa Itu Sistem Kausal?", "id": "Respons Impuls Nol Untuk Waktu Negatif." },
  { "en": "Sifat Ini Menghubungkan?", "id": "Bagian Riil Dan Imajiner Respon Frekuensi." },
  { "en": "Hubungan Ini Disebut?", "id": "Hubungan Kramers-Kronig." },
  { "en": "Apa Itu PAPR?", "id": "Rasio Daya Puncak Dan Rata-rata." },
  { "en": "PAPR Adalah Masalah Domain?", "id": "Waktu." },
  { "en": "Sinyal OFDM Punya PAPR?", "id": "Tinggi." },
  { "en": "Apa Itu Sifat Ortogonalitas?", "id": "Kunci Untuk Memisahkan Sinyal." },
  { "en": "Basis Fourier Adalah?", "id": "Ortogonal." },
  { "en": "OFDM Menggunakan Basis?", "id": "Ortogonal." },
  { "en": "Apa Itu Sinyal Stasioner?", "id": "Karakteristik Statistiknya Konstan." },
  { "en": "Apa Itu Sinyal Non-Stasioner?", "id": "Karakteristik Statistiknya Berubah." },
  { "en": "Dunia Waktu Itu Lokal?", "id": "Ya." },
  { "en": "Dunia Frekuensi Itu Global?", "id": "Ya." },
  { "en": "Mengubah Satu Sampel Waktu?", "id": "Mengubah Seluruh Spektrum." },
  { "en": "Mengubah Satu Koefisien Frekuensi?", "id": "Mengubah Seluruh Sinyal Waktu." },
  { "en": "Keduanya Adalah Dua Sisi?", "id": "Dari Koin Yang Sama." },
  { "en": "Fourier Adalah Bahasa Untuk?", "id": "Menerjemahkan Antara Dua Sisi Ini." },
  { "en": "Pemahaman Fourier Adalah Kunci?", "id": "Untuk Rekayasa Elektro Modern." }



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

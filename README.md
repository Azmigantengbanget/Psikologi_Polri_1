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


  { "en": "Apa yang Anda lakukan jika melihat rekan kerja melanggar aturan ringan demi kenyamanan pribadi?", "id": "Saya akan menegurnya secara pribadi dengan sopan dan mengingatkan pentingnya menaati aturan sebagai anggota Polri." },
  { "en": "Bagaimana sikap Anda ketika menerima kritik tajam dari atasan mengenai kinerja Anda?", "id": "Saya akan mendengarkan dengan saksama, menjadikannya bahan evaluasi untuk perbaikan diri, dan tidak menanggapinya secara personal." },
  { "en": "Anda diberikan tugas yang sangat mendadak dengan tenggat waktu yang ketat. Apa tindakan pertama Anda?", "id": "Saya akan segera mempelajari tugas tersebut, membuat rencana kerja cepat, dan mulai mengerjakannya dengan fokus dan efisien." },
  { "en": "Seorang warga datang meminta bantuan di saat Anda sedang sibuk dengan pekerjaan lain yang juga penting. Apa yang Anda lakukan?", "id": "Saya akan memberikan perhatian sejenak, melayani warga tersebut dengan cepat dan efektif, baru kemudian melanjutkan pekerjaan saya." },
  { "en": "Jika Anda menemukan dompet berisi uang dan KTP di jalan, tindakan apa yang akan Anda ambil?", "id": "Saya akan segera mengamankannya dan berusaha mencari cara untuk mengembalikannya kepada pemilik, misalnya dengan menghubungi alamat di KTP atau menyerahkannya ke kantor polisi." },
  { "en": "Bagaimana pandangan Anda tentang perintah atasan?", "id": "Perintah atasan adalah kewajiban yang harus dilaksanakan dengan penuh tanggung jawab, selama tidak bertentangan dengan hukum dan etika." },
  { "en": "Apa yang Anda lakukan jika tim Anda gagal mencapai target yang telah ditetapkan?", "id": "Saya akan ikut bertanggung jawab, mengajak tim untuk melakukan evaluasi bersama mencari penyebab kegagalan, dan menyusun strategi perbaikan." },
  { "en": "Jika Anda ditawari sejumlah uang oleh seseorang untuk 'memperlancar' sebuah urusan, bagaimana Anda bersikap?", "id": "Saya akan menolak dengan tegas dan menjelaskan bahwa semua urusan harus diselesaikan sesuai prosedur yang berlaku tanpa ada imbalan." },
  { "en": "Menurut Anda, apa kualitas terpenting yang harus dimiliki seorang anggota Polri?", "id": "Integritas, yaitu kesesuaian antara perkataan dan perbuatan, serta ketaatan pada hukum dan aturan." },
  { "en": "Bagaimana Anda menghadapi situasi di mana Anda harus bekerja sama dengan orang yang tidak Anda sukai?", "id": "Saya akan tetap profesional, fokus pada tujuan pekerjaan, dan menjaga komunikasi yang baik demi keberhasilan tim." },
  { "en": "Apa motivasi utama Anda ingin menjadi anggota Polri?", "id": "Motivasi utama saya adalah untuk mengabdi kepada negara dan masyarakat, serta menegakkan hukum dengan adil." },
  { "en": "Lengkapi analogi: BELAJAR : PANDAI = MAKAN : ...", "id": "KENYANG." },
  { "en": "Lengkapi analogi: PESAWAT : AVTUR = MOBIL : ...", "id": "BENSIN." },
  { "en": "Lengkapi analogi: MATA : MELIHAT = TELINGA : ...", "id": "MENDENGAR." },
  { "en": "Lengkapi analogi: PANAS : API = TERANG : ...", "id": "MATAHARI." },
  { "en": "Lengkapi analogi: INDONESIA : RUPIAH = JEPANG : ...", "id": "YEN." },
  { "en": "Lengkapi analogi: GURU : SEKOLAH = DOKTER : ...", "id": "RUMAH SAKIT." },
  { "en": "Lengkapi analogi: AIR : HAUS = NASI : ...", "id": "LAPAR." },
  { "en": "Lengkapi analogi: UMUR : TAHUN = BERAT : ...", "id": "KILOGRAM." },
  { "en": "Lengkapi analogi: BENANG : KAIN = KAWAT : ...", "id": "JARING." },
  { "en": "Lengkapi analogi: ULAT : KUPU-KUPU = BERUDU : ...", "id": "KATAK." },
  { "en": "Lanjutkan deret angka: 3, 6, 12, 24, ...", "id": "48 (pola: dikali 2)." },
  { "en": "Lanjutkan deret angka: 99, 96, 93, 90, ...", "id": "87 (pola: dikurang 3)." },
  { "en": "Lanjutkan deret angka: 5, 7, 10, 14, 19, ...", "id": "25 (pola: +2, +3, +4, +5)." },
  { "en": "Lanjutkan deret angka: 2, 8, 4, 16, 8, 32, ...", "id": "16 (pola dua larik: 2,4,8,... dan 8,16,32,...)." },
  { "en": "Lanjutkan deret angka: 9, 16, 25, 36, ...", "id": "49 (pola: bilangan kuadrat 3², 4², 5², 6², 7²)." },
  { "en": "Lanjutkan deret huruf: A, C, F, J, O, ...", "id": "U (pola loncat: +2, +3, +4, +5, +6 huruf)." },
  { "en": "Lanjutkan deret angka: 100, 10, 90, 20, 80, ...", "id": "30 (pola dua larik: 100,90,80,... dan 10,20,30,...)." },
  { "en": "Lanjutkan deret angka: 1, 1, 2, 3, 5, 8, ...", "id": "13 (pola: deret Fibonacci)." },
  { "en": "Lanjutkan deret huruf: Z, Y, A, X, W, B, ...", "id": "V (pola: 2 huruf mundur dari abjad akhir, 1 huruf maju dari abjad awal)." },
  { "en": "Lanjutkan deret angka: 3, 9, 13, 8, 24, 28, ...", "id": "23 (pola berulang: x3, +4, -5)." },
  { "en": "Apa sinonim dari kata 'VIRTUAL'?", "id": "Maya." },
  { "en": "Apa sinonim dari kata 'KREDIBILITAS'?", "id": "Dapat dipercaya." },
  { "en": "Apa sinonim dari kata 'INSOMNIA'?", "id": "Sulit tidur." },
  { "en": "Apa sinonim dari kata 'FRIKSI'?", "id": "Perpecahan atau gesekan." },
  { "en": "Apa sinonim dari kata 'PROMOSI' dalam konteks pekerjaan?", "id": "Kenaikan pangkat." },
  { "en": "Apa antonim dari kata 'PASCA'?", "id": "Pra." },
  { "en": "Apa antonim dari kata 'NOMADEN'?", "id": "Menetap." },
  { "en": "Apa antonim dari kata 'HIRAU'?", "id": "Acuh atau abai." },
  { "en": "Apa antonim dari kata 'DEPENDEN'?", "id": "Mandiri atau independen." },
  { "en": "Apa antonim dari kata 'SURAM'?", "id": "Cerah." },
  { "en": "Jika semua anggota Polri berintegritas dan Adi adalah anggota Polri, maka kesimpulannya adalah...", "id": "Adi berintegritas." },
  { "en": "Jika turun hujan lebat, maka terjadi banjir. Saat ini tidak terjadi banjir. Kesimpulannya adalah...", "id": "Saat ini tidak turun hujan lebat." },
  { "en": "Semua kandidat yang lolos seleksi akan mengikuti pendidikan. Budi tidak mengikuti pendidikan. Kesimpulannya adalah...", "id": "Budi tidak lolos seleksi." },
  { "en": "Ikan mas hidup di air tawar. Ikan tuna hidup di air asin. Kesimpulan apa yang bisa ditarik dari kedua pernyataan?", "id": "Tidak semua ikan hidup di habitat yang sama." },
  { "en": "Motor A lebih cepat dari motor B. Motor C lebih lambat dari motor B. Urutan dari yang paling cepat adalah...", "id": "Motor A, Motor B, Motor C." },
  { "en": "Berapakah nilai dari 15% dari 200.000?", "id": "30.000." },
  { "en": "Sebuah mobil menempuh jarak 180 km dalam 3 jam. Berapa kecepatan rata-rata mobil tersebut?", "id": "60 km/jam." },
  { "en": "Harga sebuah jaket setelah diskon 25% adalah Rp 150.000. Berapa harga asli jaket tersebut?", "id": "Rp 200.000." },
  { "en": "Jika 4x + 7 = 35, maka nilai x adalah...", "id": "7." },
  { "en": "Perbandingan uang Tono dan Tini adalah 3:5. Jika jumlah uang mereka Rp 80.000, berapa uang Tini?", "id": "Rp 50.000." },
  { "en": "Bagaimana Anda memastikan pekerjaan Anda selalu teliti dan minim kesalahan?", "id": "Saya akan memeriksa kembali pekerjaan saya beberapa kali sebelum menyerahkannya dan membuat checklist jika diperlukan." },
  { "en": "Apa yang Anda lakukan jika merasa jenuh dan lelah akibat tekanan pekerjaan yang tinggi?", "id": "Saya akan mengambil istirahat sejenak untuk menenangkan pikiran, lalu kembali bekerja dengan energi baru. Saya juga mengelola stres di luar jam kerja." },
  { "en": "Bagaimana Anda menyikapi perubahan mendadak dalam prosedur kerja?", "id": "Saya akan segera mempelajari prosedur baru tersebut dan beradaptasi secepat mungkin untuk menjaga kelancaran pekerjaan." },
  { "en": "Pentingkah menjaga penampilan fisik (kerapian) bagi seorang anggota Polri?", "id": "Sangat penting, karena penampilan yang rapi mencerminkan disiplin, kewibawaan, dan citra positif institusi Polri." },
  { "en": "Apa yang akan Anda lakukan untuk terus mengembangkan kemampuan diri setelah menjadi anggota Polri?", "id": "Saya akan aktif mengikuti pelatihan, membaca, dan belajar dari senior untuk meningkatkan wawasan dan keterampilan saya." },
  { "en": "Bagaimana Anda menangani seorang warga yang sedang dalam keadaan panik atau emosional?", "id": "Saya akan berbicara dengan tenang dan sabar, mencoba menenangkannya terlebih dahulu sebelum menggali informasi atau memberikan bantuan." },
  { "en": "Menurut Anda, apakah loyalitas kepada atasan berarti harus menuruti semua perintahnya tanpa terkecuali?", "id": "Loyalitas berarti mendukung dan melaksanakan perintah yang benar, namun harus berani memberikan masukan jika perintah tersebut berpotensi salah atau melanggar aturan." },
  { "en": "Di antara tugas individu dan tugas kelompok, mana yang lebih Anda sukai?", "id": "Saya siap mengerjakan keduanya. Tugas individu melatih kemandirian, sementara tugas kelompok melatih kerja sama dan komunikasi." },
  { "en": "Bagaimana Anda menjaga kerahasiaan informasi dinas yang sensitif?", "id": "Saya tidak akan pernah membahas informasi sensitif di luar lingkungan kerja atau kepada pihak yang tidak berwenang, termasuk keluarga." },
  { "en": "Apa tindakan Anda jika melihat ada ketidakadilan di lingkungan kerja Anda?", "id": "Saya akan mencoba melaporkannya melalui saluran yang benar dan etis, dengan didukung oleh bukti, demi tegaknya keadilan." },
  { "en": "Lengkapi analogi: JARUM : TAJAM = PISAU : ...", "id": "TAJAM (Hubungan benda dan sifat utamanya)." },
  { "en": "Lengkapi analogi: SISWA : BELAJAR = PETANI : ...", "id": "MENANAM (Hubungan subjek dan aktivitas utamanya)." },
  { "en": "Lengkapi analogi: SIANG : MATAHARI = MALAM : ...", "id": "BULAN." },
  { "en": "Lengkapi analogi: RAMBUT : KEPALA = BULU : ...", "id": "KULIT." },
  { "en": "Lengkapi analogi: HAUS : MINUM = GATAL : ...", "id": "MENGGARUK." },
  { "en": "Lanjutkan deret angka: 4, 8, 7, 14, 13, ...", "id": "26 (Pola: x2, -1, diulang)." },
  { "en": "Lanjutkan deret angka: 1, 4, 9, 16, 25, ...", "id": "36 (pola: bilangan kuadrat)." },
  { "en": "Lanjutkan deret angka: 50, 45, 41, 38, 36, ...", "id": "35 (pola: -5, -4, -3, -2, -1)." },
  { "en": "Lanjutkan deret huruf: B, E, H, K, N, ...", "id": "Q (pola loncat 2 huruf)." },
  { "en": "Lanjutkan deret angka: 1, 2, 4, 8, 16, ...", "id": "32 (pola: dikali 2)." },
  { "en": "Apa sinonim dari kata 'RANDOM'?", "id": "Acak." },
  { "en": "Apa sinonim dari kata 'KONTEMPORER'?", "id": "Masa kini atau modern." },
  { "en": "Apa sinonim dari kata 'ESENSIAL'?", "id": "Penting atau mendasar." },
  { "en": "Apa antonim dari kata 'KOLEKTIF'?", "id": "Individu." },
  { "en": "Apa antonim dari kata 'ABSTRAK'?", "id": "Konkret atau nyata." },
  { "en": "Jika p=2 dan q=3, berapakah nilai dari (p+q)²?", "id": "25." },
  { "en": "Sebuah tim terdiri dari 6 orang dapat menyelesaikan pekerjaan dalam 4 hari. Jika orangnya ditambah 2, berapa hari pekerjaan akan selesai?", "id": "3 hari (perbandingan terbalik)." },
  { "en": "Berapakah 2/5 jika diubah menjadi persen?", "id": "40%." },
  { "en": "Jika sebuah acara dimulai pukul 09:45 dan berakhir pukul 11:15, berapa lama acara tersebut berlangsung?", "id": "1 jam 30 menit." },
  { "en": "Urutkan pecahan berikut dari yang terkecil: 1/2, 3/4, 2/5.", "id": "2/5, 1/2, 3/4." },
  { "en": "Bagaimana sikap Anda terhadap kegagalan?", "id": "Saya melihat kegagalan sebagai sebuah kesempatan belajar yang berharga untuk menjadi lebih baik dan lebih kuat." },
  { "en": "Apa yang lebih penting bagi Anda: hasil akhir atau proses?", "id": "Keduanya penting, namun proses yang benar, jujur, dan sesuai aturan adalah fondasi untuk mencapai hasil akhir yang baik." },
  { "en": "Dalam situasi darurat yang kacau, bagaimana Anda bisa tetap tenang?", "id": "Saya akan fokus pada pernapasan untuk mengontrol detak jantung, kemudian memprioritaskan tindakan yang paling krusial untuk dilakukan." },
  { "en": "Apakah Anda orang yang mudah bergaul?", "id": "Ya, saya menikmati bertemu orang baru dan dapat dengan cepat membangun hubungan komunikasi yang baik." },
  { "en": "Bagaimana Anda memandang kekuasaan atau wewenang?", "id": "Sebagai amanah dan alat untuk melayani masyarakat, bukan untuk kepentingan pribadi." },
  { "en": "Semua mamalia menyusui. Kucing adalah mamalia. Kesimpulannya adalah...", "id": "Kucing menyusui." },
  { "en": "Jika hari ini hari Selasa, maka 3 hari yang lalu adalah hari...", "id": "Sabtu." },
  { "en": "Lengkapi analogi: PUTIH : BERSIH = HITAM : ...", "id": "KOTOR (Hubungan warna dengan konotasi umum)." },
  { "en": "Lanjutkan deret: 10, 30, 32, 16, 48, 50, ...", "id": "25 (pola: x3, +2, :2, diulang)." },
  { "en": "Apa sinonim dari 'TENDENSI'?", "id": "Kecenderungan." },
  { "en": "Apa antonim dari 'PERMANEN'?", "id": "Sementara." },
  { "en": "Apa yang Anda lakukan jika pendapat Anda dalam sebuah rapat tidak diterima oleh mayoritas?", "id": "Saya akan menghargai keputusan bersama dan mendukung pelaksanaannya, meskipun pendapat saya berbeda." },
  { "en": "Berapakah akar kuadrat dari 625?", "id": "25." },
  { "en": "Bagaimana Anda menjaga kesehatan fisik untuk menunjang tugas sebagai anggota Polri?", "id": "Dengan berolahraga secara teratur, menjaga pola makan yang sehat, dan istirahat yang cukup." },
  { "en": "Jika Anda merasa atasan memberikan tugas yang tidak adil (pilih kasih), apa yang Anda lakukan?", "id": "Saya akan tetap mengerjakan tugas tersebut dengan sebaik-baiknya sambil mencari waktu yang tepat untuk berkomunikasi secara profesional dengan atasan mengenai beban kerja." },
  { "en": "Apakah Anda siap mengorbankan waktu pribadi demi kepentingan dinas?", "id": "Ya, saya memahami bahwa tugas sebagai abdi negara menuntut dedikasi dan siap sedia kapan pun dibutuhkan." },
  { "en": "Lengkapi analogi: KOMPOR : MEMASAK = SENJATA : ...", "id": "MELINDUNGI (Hubungan alat dan fungsi utamanya dalam konteks profesi)." },
  { "en": "Lanjutkan deret: 3, 4, 7, 11, 18, ...", "id": "29 (Pola: Deret Fibonacci yang dimulai dari 3 dan 4)." },
  { "en": "Apa tindakan pertama yang harus dilakukan saat tiba di Tempat Kejadian Perkara (TKP)?", "id": "Mengamankan TKP untuk menjaga status quo dan mencegah kerusakan barang bukti." },
  { "en": "Apa yang Anda lakukan jika rekan satu tim Anda terus menerus membuat kesalahan yang sama?", "id": "Saya akan mendekatinya secara pribadi, menawarkan bantuan untuk mencari tahu letak kesulitannya, dan bersama-sama mencari solusi agar tidak terulang." },
  { "en": "Bagaimana Anda menanggapi rumor atau gosip negatif tentang Anda di lingkungan kerja?", "id": "Saya akan mengabaikannya dan tetap fokus pada pekerjaan saya, biarkan kinerja dan integritas saya yang membuktikan kebenarannya." },
  { "en": "Anda melihat seorang atasan menggunakan fasilitas dinas untuk kepentingan pribadi. Apa sikap Anda?", "id": "Saya akan mencari cara yang etis untuk melaporkan hal tersebut kepada pihak yang berwenang atau melalui saluran pengaduan internal, tanpa menimbulkan konfrontasi langsung." },
  { "en": "Dalam sebuah diskusi, ide cemerlang Anda diakui sebagai ide rekan kerja lain oleh atasan. Bagaimana reaksi Anda?", "id": "Saya akan tetap tenang. Prioritas utama adalah keberhasilan tim. Di lain kesempatan, saya akan mencari cara yang sopan untuk menunjukkan kontribusi saya secara lebih jelas." },
  { "en": "Apa arti 'disiplin' menurut Anda?", "id": "Disiplin adalah ketaatan pada aturan dan standar yang didasari oleh kesadaran diri dan rasa tanggung jawab, bukan karena paksaan atau rasa takut." },
  { "en": "Anda merasa lelah secara mental dan fisik, namun tugas masih banyak. Apa yang Anda prioritaskan?", "id": "Saya akan mengambil jeda singkat untuk memulihkan fokus, lalu membuat skala prioritas untuk menyelesaikan tugas yang paling mendesak terlebih dahulu." },
  { "en": "Bagaimana cara Anda membangun kepercayaan dengan masyarakat?", "id": "Dengan menunjukkan sikap yang humanis, melayani dengan tulus, transparan, dan bertindak profesional sesuai hukum yang berlaku." },
  { "en": "Anda ditugaskan ke sebuah daerah baru yang memiliki adat dan budaya yang sangat berbeda. Langkah pertama Anda adalah?", "id": "Mempelajari dan berusaha memahami adat serta budaya setempat agar dapat beradaptasi dan diterima oleh masyarakat." },
  { "en": "Apakah Anda bersedia mengakui ketidaktahuan Anda tentang suatu hal di depan rekan atau atasan?", "id": "Ya, saya bersedia mengakuinya dan akan segera berusaha untuk belajar dan mencari tahu hal tersebut. Lebih baik jujur daripada berpura-pura tahu." },
  { "en": "Bagaimana Anda memastikan bahwa emosi pribadi Anda tidak mempengaruhi keputusan dinas?", "id": "Saya selalu berusaha memisahkan urusan pribadi dan dinas. Setiap keputusan akan saya dasarkan pada fakta, data, dan peraturan yang berlaku, bukan pada perasaan." },
  { "en": "Lengkapi analogi: OBAT : APOTEKER = MAKANAN : ...", "id": "KOKI." },
  { "en": "Lengkapi analogi: BAJA : KUAT = KARET : ...", "id": "LENTUR." },
  { "en": "Lengkapi analogi: BENSIN : TERBAKAR = AIR : ...", "id": "MENGUAP." },
  { "en": "Lengkapi analogi: GELOMBANG : PANTAI = ANGIN : ...", "id": "LEMBAH (atau GUNUNG)." },
  { "en": "Lengkapi analogi: PIDATO : PANGGUNG = KHOTBAH : ...", "id": "MIMBAR." },
  { "en": "Lengkapi analogi: KOMANDAN : PASUKAN = DIRIGEN : ...", "id": "ORKESTRA." },
  { "en": "Lengkapi analogi: KERINGAT : OLAHRAGA = ASAP : ...", "id": "PEMBAKARAN." },
  { "en": "Lengkapi analogi: METEOROLOGI : CUACA = GEOLOGI : ...", "id": "BATUAN (atau BUMI)." },
  { "en": "Lengkapi analogi: SKRIPSI : SARJANA = DISERTASI : ...", "id": "DOKTOR." },
  { "en": "Lengkapi analogi: KAKI : SEPATU = TANGAN : ...", "id": "SARUNG TANGAN." },
  { "en": "Lanjutkan deret angka: 1, 3, 6, 10, 15, ...", "id": "21 (pola: +2, +3, +4, +5, +6)." },
  { "en": "Lanjutkan deret angka: 81, 27, 9, 3, ...", "id": "1 (pola: dibagi 3)." },
  { "en": "Lanjutkan deret angka: 2, 5, 11, 23, 47, ...", "id": "95 (pola: x2 + 1)." },
  { "en": "Lanjutkan deret huruf: A, Z, B, Y, C, ...", "id": "X (pola: maju dari awal, mundur dari akhir)." },
  { "en": "Lanjutkan deret angka: 3, 5, 8, 13, 21, ...", "id": "34 (pola: deret Fibonacci)." },
  { "en": "Lanjutkan deret angka: 1, 8, 27, 64, ...", "id": "125 (pola: bilangan pangkat tiga: 1³, 2³, 3³, 4³, 5³)." },
  { "en": "Lanjutkan deret huruf: C, G, K, O, S, ...", "id": "W (pola loncat 3 huruf)." },
  { "en": "Lanjutkan deret angka: 4, 5, 7, 10, 14, 19, ...", "id": "25 (pola: +1, +2, +3, +4, +5, +6)." },
  { "en": "Lanjutkan deret angka: 5, 3, 6, 4, 7, 5, ...", "id": "8 (pola dua larik: 5,6,7,8,... dan 3,4,5,...)." },
  { "en": "Lanjutkan deret angka: 2, 6, 12, 20, 30, ...", "id": "42 (pola: 1x2, 2x3, 3x4, 4x5, 5x6, 6x7)." },
  { "en": "Apa sinonim dari kata 'AKURAT'?", "id": "Tepat atau saksama." },
  { "en": "Apa sinonim dari kata 'PARADOKS'?", "id": "Bertentangan." },
  { "en": "Apa sinonim dari kata 'KRUSIAL'?", "id": "Penting atau gawat." },
  { "en": "Apa sinonim dari kata 'IMPLISIT'?", "id": "Tersirat." },
  { "en": "Apa sinonim dari kata 'KOMPETEN'?", "id": "Cakap atau mampu." },
  { "en": "Apa antonim dari kata 'OTORITER'?", "id": "Demokratis." },
  { "en": "Apa antonim dari kata 'BONGSOR'?", "id": "Kerdil." },
  { "en": "Apa antonim dari kata 'UNIVERSAL'?", "id": "Lokal atau parsial." },
  { "en": "Apa antonim dari kata 'PROMINEN'?", "id": "Biasa atau tidak menonjol." },
  { "en": "Apa antonim dari kata 'OPTIMIS'?", "id": "Pesimis." },
  { "en": "Jika Tono lebih tinggi dari Adi, dan Adi lebih tinggi dari Candra, maka siapa yang paling pendek?", "id": "Candra." },
  { "en": "Semua burung bisa terbang. Penguin adalah burung. Apakah kesimpulan 'Penguin bisa terbang' benar?", "id": "Tidak benar menurut fakta (exception), meskipun secara logis deduktif dari premis yang salah tersebut, kesimpulannya benar." },
  { "en": "Jika memakai seragam, siswa boleh masuk sekolah. Hari ini Budi tidak memakai seragam. Kesimpulannya adalah...", "id": "Budi tidak boleh masuk sekolah." },
  { "en": "Semua A adalah B. Sebagian C adalah A. Kesimpulannya adalah...", "id": "Sebagian C adalah B." },
  { "en": "Di sebuah rak, buku Sejarah ada di sebelah kiri buku Matematika. Buku Fisika ada di sebelah kanan buku Sejarah. Buku mana yang ada di tengah?", "id": "Tidak dapat dipastikan, bisa Sejarah atau Matematika, tergantung posisi buku Fisika." },
  { "en": "Berapakah 3/4 dari 120?", "id": "90." },
  { "en": "Jika harga 5 buah pensil adalah Rp 7.500, berapakah harga 12 buah pensil?", "id": "Rp 18.000." },
  { "en": "Seorang pelari menempuh 5 km dalam 30 menit. Berapa waktu yang ia butuhkan untuk menempuh 7 km dengan kecepatan yang sama?", "id": "42 menit." },
  { "en": "Jika a = 5 dan b = -2, berapakah nilai dari a² - b²?", "id": "21 (25 - 4)." },
  { "en": "Sebuah wadah terisi 2/3 bagian. Jika ditambahkan 5 liter air, wadah tersebut menjadi penuh. Berapa kapasitas total wadah tersebut?", "id": "15 liter." },
  { "en": "Bagaimana Anda menyikapi kegagalan komunikasi dalam tim?", "id": "Saya akan menginisiasi sebuah diskusi terbuka untuk mengidentifikasi akar masalah komunikasi dan menyepakati cara berkomunikasi yang lebih baik ke depannya." },
  { "en": "Apa yang akan Anda lakukan jika Anda merasa instruksi dari atasan kurang jelas?", "id": "Saya akan bertanya kembali dengan sopan untuk meminta klarifikasi agar tidak terjadi kesalahan dalam pelaksanaan tugas." },
  { "en": "Bagaimana Anda menjaga semangat kerja saat melakukan tugas yang monoton dan berulang?", "id": "Saya akan mencoba mencari cara untuk membuat tugas tersebut lebih efisien atau menetapkan target-target kecil untuk menjaga motivasi." },
  { "en": "Menurut Anda, seberapa penting peran teknologi dalam tugas kepolisian saat ini?", "id": "Sangat penting. Teknologi membantu meningkatkan efisiensi, akurasi, dan jangkauan dalam pelayanan serta penegakan hukum." },
  { "en": "Bagaimana Anda membedakan antara 'tegas' dan 'kasar' dalam bertugas?", "id": "Tegas berarti teguh pada aturan dan prinsip tanpa kompromi, sementara kasar melibatkan emosi negatif dan tindakan yang tidak profesional. Tegas itu perlu, kasar itu tidak." },
  { "en": "Seorang rekan meminta Anda untuk berbohong demi 'melindunginya' dari kesalahan kecil. Apa yang Anda lakukan?", "id": "Saya akan menolak karena kebohongan sekecil apapun adalah pelanggaran integritas. Saya akan menyarankannya untuk mengakui kesalahan secara jujur." },
  { "en": "Anda lebih suka lingkungan kerja yang seperti apa?", "id": "Lingkungan kerja yang solid, di mana ada kerja sama yang baik, saling mendukung, dan menjunjung tinggi profesionalitas serta integritas." },
  { "en": "Bagaimana Anda mengelola waktu antara pekerjaan, keluarga, dan pengembangan diri?", "id": "Dengan membuat perencanaan dan menetapkan prioritas. Saya akan mendedikasikan waktu khusus untuk setiap aspek tersebut secara seimbang." },
  { "en": "Apa yang membuat Anda berbeda atau lebih unggul dari kandidat lainnya?", "id": "Saya memiliki komitmen yang kuat terhadap integritas, kemampuan beradaptasi yang cepat, dan keinginan yang besar untuk terus belajar dan berkontribusi." },
  { "en": "Apa arti 'pengabdian' bagi Anda?", "id": "Pengabdian adalah memberikan yang terbaik dari diri saya untuk kepentingan negara dan masyarakat, bahkan ketika tidak ada yang melihat atau mengharapkan imbalan." },
  { "en": "Lengkapi analogi: JARUM : BENANG = KUAS : ...", "id": "CAT." },
  { "en": "Lengkapi analogi: LAPAR : MAKAN = MENGANTUK : ...", "id": "TIDUR." },
  { "en": "Lengkapi analogi: ES : DINGIN = GURUN : ...", "id": "PANAS (atau KERING)." },
  { "en": "Lengkapi analogi: BINTANG : ASTRONOMI = BATUAN : ...", "id": "GEOLOGI." },
  { "en": "Lengkapi analogi: JANTUNG : MEMOMPA = PARU-PARU : ...", "id": "BERNAPAS." },
  { "en": "Lanjutkan deret angka: 1, 5, 2, 10, 3, 15, ...", "id": "4 (pola dua larik: 1,2,3,4,... dan 5,10,15,...)." },
  { "en": "Lanjutkan deret angka: 2, 3, 5, 7, 11, 13, ...", "id": "17 (pola: deret bilangan prima)." },
  { "en": "Lanjutkan deret huruf: A, B, D, G, K, ...", "id": "P (pola loncat: +1, +2, +3, +4, +5)." },
  { "en": "Lanjutkan deret angka: 36, 34, 30, 28, 24, ...", "id": "22 (pola: -2, -4, -2, -4)." },
  { "en": "Lanjutkan deret angka: 90, 85, 79, 72, 64, ...", "id": "55 (pola: -5, -6, -7, -8, -9)." },
  { "en": "Apa sinonim dari kata 'REGULASI'?", "id": "Peraturan." },
  { "en": "Apa sinonim dari kata 'FLEKSIBEL'?", "id": "Luwes atau mudah disesuaikan." },
  { "en": "Apa sinonim dari kata 'KORELASI'?", "id": "Hubungan timbal balik." },
  { "en": "Apa antonim dari kata 'KEBETULAN'?", "id": "Sengaja." },
  { "en": "Apa antonim dari kata 'MAYOR'?", "id": "Minor." },
  { "en": "Jika X adalah 25% dari 80, dan Y adalah 80% dari 25, maka...", "id": "X = Y (keduanya bernilai 20)." },
  { "en": "Berapakah 30% dari 150?", "id": "45." },
  { "en": "Jika 6 pekerja membangun jembatan dalam 10 hari, berapa hari yang dibutuhkan jika hanya 5 pekerja yang tersedia?", "id": "12 hari." },
  { "en": "Berapakah nilai dari (1/4 + 1/5)?", "id": "9/20." },
  { "en": "Seorang pedagang membeli barang seharga Rp 50.000 dan menjualnya dengan keuntungan 20%. Berapa harga jualnya?", "id": "Rp 60.000." },
  { "en": "Apa yang akan Anda lakukan jika melihat seorang anak tersesat dan menangis di keramaian?", "id": "Saya akan segera menghampirinya, menenangkannya, dan membawanya ke pusat informasi atau pos keamanan terdekat untuk mencari orang tuanya." },
  { "en": "Bagaimana Anda menjelaskan prosedur yang rumit kepada masyarakat awam?", "id": "Saya akan menggunakan bahasa yang sederhana, tidak teknis, dan memberikan contoh nyata agar mudah dipahami." },
  { "en": "Apakah Anda tipe orang yang merencanakan segala sesuatu secara detail atau lebih spontan?", "id": "Saya cenderung merencanakan hal-hal penting untuk memastikan tujuan tercapai, namun tetap fleksibel untuk beradaptasi dengan situasi tak terduga." },
  { "en": "Bagaimana cara Anda memulihkan diri setelah menghadapi hari yang sangat berat dan penuh tekanan?", "id": "Dengan melakukan hobi yang saya sukai, berolahraga, atau menghabiskan waktu berkualitas bersama keluarga agar energi kembali positif." },
  { "en": "Apa arti 'kehormatan' bagi seorang anggota Polri?", "id": "Kehormatan adalah menjaga nama baik diri sendiri dan institusi dengan selalu berpegang pada kebenaran, keadilan, dan integritas dalam setiap tindakan." },
  { "en": "Semua yang ada di perpustakaan adalah buku. Meja ada di perpustakaan. Apakah kesimpulan 'Meja adalah buku' benar?", "id": "Tidak benar. Premis kedua 'Meja ada di perpustakaan' tidak menyatakan bahwa meja termasuk dalam kategori 'semua yang ada di perpustakaan adalah buku'." },
  { "en": "Jika kemarin adalah hari Jumat, maka lusa adalah hari...", "id": "Senin." },
  { "en": "Lengkapi analogi: SUARA : TELEPON = GAMBAR : ...", "id": "TELEVISI." },
  { "en": "Lanjutkan deret: 1, 10, 2, 9, 3, 8, ...", "id": "4 (pola dua larik: 1,2,3,4,... dan 10,9,8,...)." },
  { "en": "Apa sinonim dari kata 'VALID'?", "id": "Sah atau berlaku." },
  { "en": "Apa antonim dari kata 'INTERNAL'?", "id": "Eksternal." },
  { "en": "Bagaimana Anda memprioritaskan tugas jika ada beberapa pekerjaan penting yang datang bersamaan?", "id": "Saya akan mengklasifikasikannya berdasarkan urgensi (mana yang harus segera) dan importansi (mana yang dampaknya paling besar)." },
  { "en": "Berapa jam, menit, dan detik dalam 9000 detik?", "id": "2 jam, 30 menit, 0 detik." },
  { "en": "Bagaimana Anda menunjukkan sikap hormat kepada senior tanpa menjadi penjilat?", "id": "Dengan mendengarkan nasihat mereka, menunjukkan etos kerja yang baik, dan berbicara dengan sopan, bukan dengan pujian yang berlebihan." },
  { "en": "Jika sebuah peraturan baru terasa merugikan masyarakat, apa yang bisa Anda lakukan sebagai pelaksana di lapangan?", "id": "Saya tetap wajib menjalankan peraturan tersebut, namun saya bisa memberikan masukan yang konstruktif tentang dampaknya di lapangan kepada atasan sebagai bahan evaluasi." },
  { "en": "Apa pencapaian terbesar dalam hidup Anda sejauh ini dan mengapa?", "id": "Jawaban untuk pertanyaan ini bersifat pribadi, namun harus mencerminkan nilai-nilai seperti kerja keras, mengatasi tantangan, atau membantu orang lain." },
  { "en": "Anda mengetahui seorang senior yang Anda hormati memiliki masalah pribadi yang mulai mempengaruhi kinerjanya. Apa sikap Anda?", "id": "Saya akan mendekatinya dengan penuh empati dan menawarkan diri untuk mendengarkan, serta menyarankan agar ia berkonsultasi dengan unit pembinaan atau psikologi dinas jika diperlukan." },
  { "en": "Bagaimana Anda menyikapi pujian yang berlebihan dari atasan atau rekan kerja?", "id": "Saya akan berterima kasih dengan rendah hati, namun tidak akan membiarkan pujian tersebut membuat saya lengah. Saya akan tetap fokus pada tugas dan perbaikan diri." },
  { "en": "Dalam sebuah patroli, Anda tidak sengaja menyebabkan kerusakan kecil pada properti warga (misal: menyerempet pot bunga). Apa yang Anda lakukan?", "id": "Saya akan segera berhenti, mencari pemiliknya, meminta maaf dengan tulus, dan menawarkan ganti rugi yang pantas atas kerusakan tersebut." },
  { "en": "Apa yang Anda lakukan jika Anda merasa jenuh dengan rutinitas pekerjaan Anda?", "id": "Saya akan mencoba mencari tantangan baru dalam pekerjaan, mengusulkan ide-ide perbaikan, atau mengambil inisiatif untuk belajar keterampilan baru yang relevan dengan tugas saya." },
  { "en": "Bagaimana pandangan Anda tentang penggunaan media sosial sebagai anggota Polri?", "id": "Media sosial harus digunakan secara bijaksana, menjaga citra institusi, tidak membagikan informasi dinas, dan menghindari perilaku yang dapat menimbulkan kontroversi." },
  { "en": "Anda diminta untuk menjadi pemimpin sebuah tim kecil. Apa langkah pertama yang Anda lakukan?", "id": "Saya akan mengadakan pertemuan awal untuk menyamakan persepsi, menetapkan tujuan bersama yang jelas, dan membuka jalur komunikasi yang baik antar anggota." },
  { "en": "Bagaimana Anda menjelaskan perbedaan antara 'hukum' dan 'keadilan'?", "id": "Hukum adalah seperangkat aturan formal, sedangkan keadilan adalah prinsip moral yang lebih luas. Tugas Polri adalah menegakkan hukum dengan cara yang seadil-adilnya." },
  { "en": "Anda melihat ada sistem atau prosedur kerja yang tidak efisien. Apa tindakan Anda?", "id": "Saya akan mempelajari sistem tersebut, merumuskan usulan perbaikan yang konkret beserta alasannya, lalu menyampaikannya kepada atasan melalui jalur yang tepat." },
  { "en": "Bagaimana Anda menanggapi permintaan tolong dari warga di luar jam dinas?", "id": "Sebagai anggota Polri, kewajiban melayani melekat 24 jam. Saya akan tetap memberikan bantuan semampu saya atau mengarahkannya ke petugas yang berwenang." },
  { "en": "Apa arti 'kolegialitas' dalam lingkungan kerja Polri?", "id": "Kolegialitas berarti menjaga semangat kebersamaan, saling menghormati, dan mendukung sesama rekan dalam menjalankan tugas demi nama baik kesatuan." },
  { "en": "Lengkapi analogi: FOTOSINTESIS : CAHAYA = PENGUAPAN : ...", "id": "PANAS." },
  { "en": "Lengkapi analogi: PENULIS : NOVEL = KOMPONIS : ...", "id": "SIMFONI (atau LAGU)." },
  { "en": "Lengkapi analogi: KORAN : EDITOR = FILM : ...", "id": "SUTRADARA." },
  { "en": "Lengkapi analogi: KECEWA : HATI = LUKA : ...", "id": "KULIT." },
  { "en": "Lengkapi analogi: BIBIT : POHON = TELUR : ...", "id": "INDUK." },
  { "en": "Lengkapi analogi: DIAMETER : LINGKARAN = DIAGONAL : ...", "id": "PERSEGI (atau BUJUR SANGKAR)." },
  { "en": "Lengkapi analogi: LAPTOP : LISTRIK = MANUSIA : ...", "id": "MAKANAN." },
  { "en": "Lengkapi analogi: STETOSKOP : DOKTER = PALU : ...", "id": "HAKIM (atau TUKANG)." },
  { "en": "Lengkapi analogi: API : ARANG = LISTRIK : ...", "id": "BATERAI." },
  { "en": "Lengkapi analogi: SAYAP : TERBANG = SIRIP : ...", "id": "BERENANG." },
  { "en": "Lanjutkan deret angka: 2, 2, 4, 6, 10, 16, ...", "id": "26 (Pola: deret Fibonacci yang dimodifikasi, U(n) = U(n-1) + U(n-2))." },
  { "en": "Lanjutkan deret angka: 100, 98, 94, 88, 80, ...", "id": "70 (Pola: -2, -4, -6, -8, -10)." },
  { "en": "Lanjutkan deret angka: 1, 4, 2, 8, 3, 12, ...", "id": "4 (Pola dua larik: 1,2,3,4,... dan 4,8,12,...)." },
  { "en": "Lanjutkan deret huruf: A, D, I, P, ...", "id": "Y (Pola: berdasarkan urutan kuadrat 1, 4, 9, 16, 25)." },
  { "en": "Lanjutkan deret angka: 5, 6, 8, 11, 15, ...", "id": "20 (Pola: +1, +2, +3, +4, +5)." },
  { "en": "Lanjutkan deret angka: 2, 4, 16, 256, ...", "id": "65536 (Pola: n² -> 2²=4, 4²=16, 16²=256, 256²=65536)." },
  { "en": "Lanjutkan deret huruf: R, U, X, A, D, ...", "id": "G (Pola loncat 2 huruf, berputar setelah Z)." },
  { "en": "Lanjutkan deret angka: 1/9, 1/3, 1, 3, ...", "id": "9 (Pola: dikali 3)." },
  { "en": "Lanjutkan deret angka: 4, 7, 13, 25, 49, ...", "id": "97 (Pola: x2 - 1)." },
  { "en": "Lanjutkan deret angka: 7, 8, 6, 9, 5, 10, ...", "id": "4 (Pola dua larik: 7,6,5,4,... dan 8,9,10,...)." },
  { "en": "Apa sinonim dari kata 'OTENTIK'?", "id": "Asli atau asli." },
  { "en": "Apa sinonim dari kata 'PROSEDURAL'?", "id": "Sesuai tata cara." },
  { "en": "Apa sinonim dari kata 'KONSEKUENSI'?", "id": "Akibat atau dampak." },
  { "en": "Apa sinonim dari kata 'SIGNIFIKAN'?", "id": "Berarti atau penting." },
  { "en": "Apa sinonim dari kata 'ADAPTIF'?", "id": "Mudah menyesuaikan diri." },
  { "en": "Apa antonim dari kata 'PREMATUR'?", "id": "Terlambat." },
  { "en": "Apa antonim dari kata 'CHAOS'?", "id": "Tertib atau teratur." },
  { "en": "Apa antonim dari kata 'LOGIS'?", "id": "Tidak masuk akal (ilogis)." },
  { "en": "Apa antonim dari kata 'ELASTIS'?", "id": "Kaku." },
  { "en": "Apa antonim dari kata 'KOMPATIBEL'?", "id": "Tidak serasi atau tidak cocok." },
  { "en": "Jika besok adalah hari Minggu, maka 3 hari sebelum hari ini adalah hari apa?", "id": "Rabu." },
  { "en": "Tidak ada penjahat yang bijaksana. Semua politisi adalah bijaksana. Kesimpulannya adalah...", "id": "Tidak ada politisi yang merupakan penjahat." },
  { "en": "Beberapa siswa membawa bekal. Setiap yang membawa bekal juga membawa botol minum. Kesimpulannya adalah...", "id": "Beberapa siswa membawa botol minum." },
  { "en": "Jika lampu merah, semua kendaraan berhenti. Andi tidak berhenti. Kesimpulannya adalah...", "id": "Saat itu lampu tidak sedang merah." },
  { "en": "A berlari lebih cepat dari B. C berlari lebih cepat dari A. D berlari lebih lambat dari B. Siapa yang paling cepat?", "id": "C." },
  { "en": "Berapakah nilai dari 3³ + 4² - 5¹?", "id": "40 (27 + 16 - 5)." },
  { "en": "Gaji seorang pegawai adalah Rp 4.000.000. Jika 1/8 gajinya untuk transportasi dan 1/4 untuk makan, berapa sisa gajinya?", "id": "Rp 2.500.000 (Sisa = 4jt - 500rb - 1jt)." },
  { "en": "Berapa menit dalam 2.5 jam?", "id": "150 menit." },
  { "en": "Jika 2a/5 = 4, maka nilai 'a' adalah...", "id": "10." },
  { "en": "Suhu pagi hari adalah 25°C. Siang hari naik 7°C dan malam hari turun 10°C. Berapa suhu pada malam hari?", "id": "22°C." },
  { "en": "Atasan Anda memberikan perintah yang secara teknis tidak melanggar hukum, tetapi Anda merasa perintah itu tidak etis. Apa yang Anda lakukan?", "id": "Saya akan menjalankan perintah tersebut jika tidak melanggar hukum, namun saya akan mencari waktu yang baik untuk berdiskusi dengan atasan mengenai dampak etisnya sebagai bahan pertimbangan beliau di masa depan." },
  { "en": "Bagaimana Anda memotivasi rekan kerja yang sedang kehilangan semangat?", "id": "Saya akan mengajaknya bicara, mengingatkannya pada tujuan bersama dan kontribusi pentingnya dalam tim, serta menawarkan bantuan jika ia menghadapi kesulitan." },
  { "en": "Menurut Anda, mana yang lebih sulit: memimpin atau dipimpin?", "id": "Keduanya memiliki tantangan tersendiri. Memimpin membutuhkan visi dan tanggung jawab besar, sementara dipimpin membutuhkan loyalitas dan kemampuan untuk bekerja sama secara efektif." },
  { "en": "Apa yang Anda lakukan untuk menjaga kondisi fisik tetap prima untuk tugas-tugas lapangan?", "id": "Saya berkomitmen pada rutinitas olahraga yang teratur, menjaga pola makan bergizi, dan memastikan istirahat yang cukup setiap hari." },
  { "en": "Dalam situasi mendesak yang membutuhkan keputusan cepat, apa dasar utama Anda dalam mengambil keputusan?", "id": "Dasar utama saya adalah keselamatan jiwa, diikuti oleh peraturan yang berlaku dan logika yang paling masuk akal dalam situasi tersebut." },
  { "en": "Bagaimana Anda menyikapi informasi hoaks yang menyudutkan institusi Polri?", "id": "Saya tidak akan menyebarkannya. Sebaliknya, saya akan membantu memberikan klarifikasi berdasarkan fakta yang benar melalui saluran komunikasi resmi atau kepada lingkungan terdekat saya." },
  { "en": "Apa bedanya 'inisiatif' dan 'indisipliner'?", "id": "Inisiatif adalah melakukan tindakan positif yang relevan tanpa menunggu perintah. Indisipliner adalah bertindak di luar atau melanggar aturan yang sudah ada." },
  { "en": "Anda diminta memilih antara tugas di kantor yang nyaman atau tugas lapangan yang penuh tantangan. Mana yang Anda pilih?", "id": "Saya siap menerima tugas manapun sesuai kebutuhan institusi. Keduanya memiliki nilai dan tantangan yang akan mengembangkan kemampuan saya." },
  { "en": "Menurut Anda, seberapa penting kerja sama antara Polri dengan institusi lain (TNI, Pemda)?", "id": "Sangat penting. Sinergi dan kerja sama yang baik antar institusi adalah kunci untuk menciptakan keamanan dan ketertiban masyarakat yang komprehensif." },
  { "en": "Bagaimana Anda menghadapi warga yang tidak kooperatif saat diperiksa?", "id": "Saya akan tetap tenang dan profesional, menjelaskan dasar hukum dan tujuan pemeriksaan dengan sabar, dan menggunakan pendekatan persuasif sebelum mengambil tindakan yang lebih tegas." },
  { "en": "Lengkapi analogi: WASIT : PERTANDINGAN = JURI : ...", "id": "LOMBA (atau KOMPETISI)." },
  { "en": "Lengkapi analogi: ILMUWAN : PENELITIAN = WARTAWAN : ...", "id": "LIPUTAN." },
  { "en": "Lengkapi analogi: REL : KERETA API = KABEL : ...", "id": "LISTRIK (atau TELEPON)." },
  { "en": "Lengkapi analogi: AKAR : MENYERAP = DAUN : ...", "id": "BERFOTOSINTESIS." },
  { "en": "Lengkapi analogi: PETUNJUK : PETA = ATURAN : ...", "id": "UNDANG-UNDANG." },
  { "en": "Lanjutkan deret angka: 2, 6, 18, 54, ...", "id": "162 (pola: dikali 3)." },
  { "en": "Lanjutkan deret angka: 1, 2, 5, 10, 17, 26, ...", "id": "37 (pola: +1, +3, +5, +7, +9, +11)." },
  { "en": "Lanjutkan deret huruf: A, E, I, M, Q, ...", "id": "U (pola loncat 3 huruf)." },
  { "en": "Lanjutkan deret angka: 64, 32, 16, 8, 4, ...", "id": "2 (pola: dibagi 2)." },
  { "en": "Lanjutkan deret angka: 123, 234, 345, 456, ...", "id": "567 (pola: urutan angka)." },
  { "en": "Apa sinonim dari kata 'NARASI'?", "id": "Kisah atau cerita." },
  { "en": "Apa sinonim dari kata 'FRAGMEN'?", "id": "Pecahan atau bagian." },
  { "en": "Apa sinonim dari kata 'PREDIKSI'?", "id": "Perkiraan atau ramalan." },
  { "en": "Apa antonim dari kata 'DEFISIT'?", "id": "Surplus." },
  { "en": "Apa antonim dari kata 'DINAMIS'?", "id": "Statis." },
  { "en": "Jika p > q dan q < r, hubungan antara p dan r adalah...", "id": "Tidak dapat ditentukan." },
  { "en": "Sebuah ruangan berukuran 8m x 6m. Jika 1 meter persegi butuh 25 ubin, berapa total ubin yang dibutuhkan?", "id": "1200 ubin (Luas = 48 m², Total ubin = 48 x 25)." },
  { "en": "Berapakah 12.5% dari 240?", "id": "30." },
  { "en": "Jika 10 mesin dapat memproduksi 500 barang dalam 1 jam, berapa barang yang diproduksi 2 mesin dalam 2 jam?", "id": "200 barang (1 mesin = 50 barang/jam. 2 mesin = 100 barang/jam. Dalam 2 jam = 200)." },
  { "en": "Sebuah roda berputar 10 kali untuk menempuh jarak 22 meter. Berapa keliling roda tersebut?", "id": "2.2 meter." },
  { "en": "Apa arti 'empati' dan mengapa itu penting bagi seorang polisi?", "id": "Empati adalah kemampuan untuk memahami dan merasakan perasaan orang lain. Penting agar polisi bisa melayani dengan lebih manusiawi dan tidak sewenang-wenang." },
  { "en": "Bagaimana Anda menjaga objektivitas saat menangani kasus yang melibatkan teman atau kerabat?", "id": "Saya akan segera melaporkan potensi konflik kepentingan tersebut kepada atasan dan meminta agar kasus dialihkan ke petugas lain untuk menjamin objektivitas." },
  { "en": "Apa yang Anda lakukan jika diminta melakukan tugas di luar keahlian Anda?", "id": "Saya akan menerima tugas tersebut sebagai tantangan, jujur mengenai keterbatasan saya, dan segera mencari panduan atau belajar dari yang lebih ahli." },
  { "en": "Bagaimana Anda memandang hierarki dan sistem komando di Polri?", "id": "Sebagai sistem yang esensial untuk menjaga disiplin, ketertiban, dan efektivitas organisasi dalam menjalankan tugas negara." },
  { "en": "Seberapa penting integritas data dalam pekerjaan kepolisian?", "id": "Sangat penting. Integritas data adalah dasar dari penyelidikan yang adil, pengambilan keputusan yang tepat, dan kepercayaan publik terhadap institusi." },
  { "en": "Semua bunga mawar berwarna merah. Bunga ini tidak berwarna merah. Kesimpulannya adalah...", "id": "Bunga ini bukan bunga mawar." },
  { "en": "Berapa jumlah sudut dalam sebuah segitiga?", "id": "180 derajat." },
  { "en": "Lengkapi analogi: KAPAL : NAHKODA = BUS : ...", "id": "SOPIR." },
  { "en": "Lanjutkan deret: 5, 8, 12, 17, 23, ...", "id": "30 (pola: +3, +4, +5, +6, +7)." },
  { "en": "Apa sinonim dari 'KULTIVASI'?", "id": "Pengolahan atau budidaya." },
  { "en": "Apa antonim dari 'INFERIOR'?", "id": "Superior." },
  { "en": "Apa yang Anda lakukan jika seorang warga memberikan informasi yang Anda tahu itu keliru?", "id": "Saya akan berterima kasih atas informasinya, lalu dengan sopan memberikan koreksi atau penjelasan berdasarkan fakta yang benar tanpa merendahkannya." },
  { "en": "Berapakah nilai dari 5! (5 faktorial)?", "id": "120 (5x4x3x2x1)." },
  { "en": "Bagaimana Anda akan menggunakan wewenang yang Anda miliki?", "id": "Saya akan menggunakannya secara bertanggung jawab, adil, dan terukur, semata-mata untuk tujuan penegakan hukum dan pelayanan masyarakat." },
  { "en": "Apa ketakutan terbesar Anda dalam menjalankan tugas sebagai polisi?", "id": "Jawaban bersifat pribadi, namun yang ideal adalah yang berkaitan dengan kegagalan dalam melayani atau melindungi masyarakat, bukan ketakutan pribadi akan bahaya fisik." },
  { "en": "Anda sedang menjalankan rencana kerja yang sudah tersusun, tiba-tiba atasan memberikan perintah prioritas baru yang mengubah total rencana tersebut. Bagaimana reaksi Anda?", "id": "Saya akan segera menghentikan rencana lama, memahami prioritas baru tersebut, dan dengan cepat menyusun ulang rencana kerja untuk mengakomodasi perintah baru dari atasan." },
  { "en": "Dalam membuat laporan, Anda menyadari ada kesalahan kecil yang Anda buat, namun sepertinya tidak akan ada yang menyadarinya. Apa yang Anda lakukan?", "id": "Saya akan segera memperbaiki kesalahan tersebut dan memberitahu atasan jika perlu. Kejujuran dan akurasi data lebih penting daripada menutupi kesalahan kecil." },
  { "en": "Anda melihat seorang lansia kesulitan menyeberang jalan di tengah lalu lintas yang ramai, meskipun Anda sedang tidak dalam jam dinas. Apa tindakan Anda?", "id": "Saya akan segera menghampiri dan menawarkan bantuan untuk menyeberangkannya dengan aman. Jiwa melayani tidak terbatas pada jam dinas." },
  { "en": "Baru saja berhasil menyelesaikan kasus yang sangat berat, Anda langsung diberi tugas berat lainnya tanpa jeda. Bagaimana Anda mengelola emosi dan energi?", "id": "Saya akan mengambil napas sejenak untuk me-reset fokus, menerima tugas baru tersebut sebagai tantangan dan kepercayaan, lalu mengorganisir energi saya untuk memulainya." },
  { "en": "Menurut Anda, apa perbedaan mendasar antara 'percaya diri' dan 'sombong'?", "id": "Percaya diri adalah keyakinan pada kemampuan diri yang didasari oleh kompetensi. Sombong adalah merasa lebih unggul dari orang lain dan cenderung meremehkan." },
  { "en": "Bagaimana cara Anda memberikan umpan balik (feedback) yang membangun kepada rekan kerja yang lebih senior?", "id": "Saya akan mencari waktu yang tepat, menyampaikannya secara pribadi dengan bahasa yang santun, fokus pada masalahnya (bukan orangnya), dan menyajikannya sebagai saran atau sudut pandang." },
  { "en": "Apa yang Anda lakukan jika diminta untuk menjaga sebuah rahasia, namun rahasia itu ternyata merupakan sebuah rencana pelanggaran?", "id": "Saya tidak akan menjaga rahasia tersebut. Saya memiliki kewajiban moral dan hukum untuk mencegah terjadinya pelanggaran dengan melaporkannya kepada pihak yang berwenang." },
  { "en": "Bagaimana Anda menyikapi kegagalan orang lain yang berada di bawah tanggung jawab Anda sebagai ketua tim?", "id": "Saya akan mengambil tanggung jawab penuh sebagai pemimpin, tidak menyalahkan individu, dan fokus pada evaluasi untuk perbaikan tim secara keseluruhan." },
  { "en": "Pentingkah bagi seorang anggota Polri untuk memahami isu-isu sosial yang sedang berkembang di masyarakat?", "id": "Sangat penting, karena pemahaman terhadap isu sosial membantu dalam melakukan pendekatan yang lebih relevan, humanis, dan prediktif dalam menjaga kamtibmas." },
  { "en": "Dalam sebuah keadaan darurat, Anda harus memilih antara menyelamatkan barang bukti penting atau menolong korban luka ringan. Mana yang Anda dahulukan?", "id": "Menyelamatkan nyawa atau menolong korban luka adalah prioritas tertinggi (asas Salus Populi Suprema Lex Esto), baru setelah itu mengamankan barang bukti semaksimal mungkin." },
  { "en": "Lengkapi analogi: RESEP : KOKI = PARTITUR : ...", "id": "MUSISI (atau KOMPONIS)." },
  { "en": "Lengkapi analogi: SIDIK JARI : IDENTITAS = TANDA TANGAN : ...", "id": "PERSETUJUAN (atau VALIDASI)." },
  { "en": "Lengkapi analogi: FONDASI : BANGUNAN = AKAR : ...", "id": "POHON." },
  { "en": "Lengkapi analogi: KATA : KALIMAT = NOT : ...", "id": "MELODI." },
  { "en": "Lengkapi analogi: IRIGASI : SAWAH = VENTILASI : ...", "id": "RUANGAN." },
  { "en": "Lengkapi analogi: NELAYAN : LAUT = ASTRONOT : ...", "id": "LUAR ANGKASA." },
  { "en": "Lengkapi analogi: KEHAUSAN : DEHIDRASI = KEPANASAN : ...", "id": "HIPERTERMIA." },
  { "en": "Lengkapi analogi: BUKU : ILMU = KOMPAS : ...", "id": "ARAH." },
  { "en": "Lengkapi analogi: PROSESOR : KOMPUTER = OTAK : ...", "id": "MANUSIA." },
  { "en": "Lengkapi analogi: KULIT : MELINDUNGI = MATA : ...", "id": "MELIHAT." },
  { "en": "Lanjutkan deret angka: 2, 5, 10, 17, 26, 37, ...", "id": "50 (pola: n²+1 -> 1²+1, 2²+1, 3²+1, dst)." },
  { "en": "Lanjutkan deret angka: 8, 4, 2, 1, 1/2, ...", "id": "1/4 (pola: dibagi 2)." },
  { "en": "Lanjutkan deret huruf: B, C, E, G, K, M, ...", "id": "Q (pola: deret huruf prima)." },
  { "en": "Lanjutkan deret angka: 4, 12, 6, 18, 9, 27, ...", "id": "13.5 (pola: x3, :2, diulang)." },
  { "en": "Lanjutkan deret angka: 2, 8, 26, 80, ...", "id": "242 (pola: x3 + 2)." },
  { "en": "Lanjutkan deret angka: 5, 7, 12, 19, 31, ...", "id": "50 (pola: deret Fibonacci yang dimodifikasi)." },
  { "en": "Lanjutkan deret huruf: A, D, G, J, M, ...", "id": "P (pola loncat 2 huruf)." },
  { "en": "Lanjutkan deret angka: 100, 99, 97, 94, 90, ...", "id": "85 (pola: -1, -2, -3, -4, -5)." },
  { "en": "Lanjutkan deret angka: 3, 30, 6, 60, 9, 90, ...", "id": "12 (pola dua larik: 3,6,9,12,... dan 30,60,90,...)." },
  { "en": "Lanjutkan deret angka: 1, 1, 2, 4, 7, 13, ...", "id": "24 (pola: jumlah 3 angka sebelumnya)." },
  { "en": "Apa sinonim dari kata 'STAGNAN'?", "id": "Macet atau mandek." },
  { "en": "Apa sinonim dari kata 'EKSPLISIT'?", "id": "Tersurat atau gamblang." },
  { "en": "Apa sinonim dari kata 'INHEREN'?", "id": "Melekat." },
  { "en": "Apa sinonim dari kata 'PARSIAL'?", "id": "Sebagian." },
  { "en": "Apa sinonim dari kata 'YURISDIKSI'?", "id": "Kewenangan hukum." },
  { "en": "Apa antonim dari kata 'KONSTRUKTIF'?", "id": "Destruktif." },
  { "en": "Apa antonim dari kata 'VOKAL'?", "id": "Pendiam." },
  { "en": "Apa antonim dari kata 'SIMPATI'?", "id": "Antipati." },
  { "en": "Apa antonim dari kata 'CANGGIH'?", "id": "Sederhana atau kuno." },
  { "en": "Apa antonim dari kata 'RIVAL'?", "id": "Kawan atau mitra." },
  { "en": "Jika tidak ada Bunga, maka Pesta batal. Pesta tidak batal. Kesimpulannya adalah...", "id": "Bunga ada di pesta." },
  { "en": "Beberapa dosen adalah peneliti. Semua peneliti adalah orang cerdas. Kesimpulannya adalah...", "id": "Beberapa dosen adalah orang cerdas." },
  { "en": "Gajah lebih besar dari Kucing. Semut lebih kecil dari Kucing. Pernyataan mana yang pasti salah?", "id": "Semut lebih besar dari Gajah." },
  { "en": "Jika saya belajar, saya lulus. Jika saya lulus, ayah senang. Kesimpulannya adalah...", "id": "Jika saya belajar, ayah senang." },
  { "en": "Semua pohon di kebun A berbuah manis. Pohon ini tidak berbuah manis. Kesimpulannya adalah...", "id": "Pohon ini bukan berasal dari kebun A." },
  { "en": "Berapakah 2.5% dari 1.000.000?", "id": "25.000." },
  { "en": "Sebuah peta memiliki skala 1:500.000. Jika jarak dua kota di peta adalah 4 cm, berapa jarak sebenarnya?", "id": "20 km." },
  { "en": "Jika x adalah bilangan ganjil antara 6 dan 10, dan y adalah bilangan prima antara 5 dan 9, maka nilai x+y adalah...", "id": "14 atau 16 (x bisa 7 atau 9, y = 7)." },
  { "en": "Sebuah rapat berlangsung selama 150 menit. Jika rapat dimulai pukul 13:45, pukul berapa rapat selesai?", "id": "Pukul 16:15." },
  { "en": "Berapakah nilai dari (1/2) / (1/4)?", "id": "2." },
  { "en": "Bagaimana Anda menjelaskan kepada keluarga tentang risiko pekerjaan Anda tanpa membuat mereka cemas berlebihan?", "id": "Saya akan menjelaskan bahwa setiap pekerjaan memiliki risiko, namun institusi telah membekali saya dengan pelatihan dan prosedur untuk meminimalisir risiko tersebut." },
  { "en": "Apa yang Anda lakukan jika ada warga yang merekam tindakan Anda saat bertugas secara provokatif?", "id": "Saya akan tetap tenang, melanjutkan tugas sesuai prosedur, dan tidak terpancing emosi. Tindakan yang profesional adalah jawaban terbaik." },
  { "en": "Bagaimana Anda menyeimbangkan antara kecepatan dan ketelitian dalam bekerja?", "id": "Saya akan fokus mengerjakan tugas dengan teliti terlebih dahulu. Seiring bertambahnya pengalaman, kecepatan akan meningkat tanpa mengorbankan ketelitian." },
  { "en": "Menurut Anda, apa tantangan terbesar yang dihadapi Polri di era digital?", "id": "Kejahatan siber (cybercrime) yang semakin canggih dan penyebaran hoaks yang dapat mengganggu ketertiban umum." },
  { "en": "Bagaimana Anda memandang seorang pemimpin yang baik?", "id": "Pemimpin yang baik adalah yang memberi teladan (bukan hanya perintah), melindungi anak buahnya, dan mampu mengembangkan potensi timnya." },
  { "en": "Anda diberikan wewenang lebih besar dari sebelumnya. Apa yang pertama kali Anda perhatikan?", "id": "Tanggung jawab yang menyertai wewenang tersebut. Saya akan memastikan saya memahami batasan dan cara menggunakannya dengan bijak." },
  { "en": "Bagaimana Anda membangun hubungan baik dengan tokoh masyarakat di tempat Anda bertugas?", "id": "Dengan melakukan silaturahmi secara rutin, mendengarkan aspirasi mereka, dan melibatkan mereka dalam kegiatan-kegiatan kamtibmas." },
  { "en": "Apa tindakan Anda jika merasa keputusan mayoritas dalam tim Anda salah secara fundamental?", "id": "Saya akan menyampaikan data atau argumen logis yang mendukung pandangan saya secara baik-baik. Jika tetap tidak diterima, saya akan mengikuti keputusan namun mencatatkan keberatan saya (dissenting opinion) jika dimungkinkan." },
  { "en": "Apa arti 'siap ditempatkan di seluruh wilayah NKRI' bagi Anda?", "id": "Itu adalah bentuk komitmen dan kesetiaan penuh kepada negara, di mana saya siap mengabdikan diri di manapun negara membutuhkan, tanpa memandang kondisi daerah." },
  { "en": "Mengapa seorang polisi harus memiliki kondisi psikologis yang stabil?", "id": "Karena tugasnya penuh tekanan, berisiko tinggi, dan seringkali harus membuat keputusan krusial dengan cepat. Psikologis yang stabil adalah fondasi profesionalisme." },
  { "en": "Lengkapi analogi: PEDAS : CABAI = MANIS : ...", "id": "GULA." },
  { "en": "Lengkapi analogi: BOR : LUBANG = GUNTING : ...", "id": "POTONGAN." },
  { "en": "Lengkapi analogi: DRAMA : PLOT = LAGU : ...", "id": "IRAMA." },
  { "en": "Lengkapi analogi: PENGACARA : KLIEN = DOKTER : ...", "id": "PASIEN." },
  { "en": "Lengkapi analogi: BAIT : PUISI = PARAGRAF : ...", "id": "ESA." },
  { "en": "Lanjutkan deret angka: 2, 3, 6, 7, 14, 15, ...", "id": "30 (pola: +1, x2, diulang)." },
  { "en": "Lanjutkan deret angka: 4, 9, 16, 25, 36, ...", "id": "49 (pola: kuadrat dari 2,3,4,5,6,7)." },
  { "en": "Lanjutkan deret huruf: A, B, C, E, F, G, I, J, K, ...", "id": "M (pola: 3 huruf urut, loncat 1 huruf)." },
  { "en": "Lanjutkan deret angka: 10, 12, 16, 22, 30, ...", "id": "40 (pola: +2, +4, +6, +8, +10)." },
  { "en": "Lanjutkan deret angka: 500, 400, 310, 230, 160, ...", "id": "100 (pola: -100, -90, -80, -70, -60)." },
  { "en": "Apa sinonim dari kata 'ASUMSI'?", "id": "Anggapan dasar." },
  { "en": "Apa sinonim dari kata 'SIMULTAN'?", "id": "Serentak atau bersamaan." },
  { "en": "Apa sinonim dari kata 'RELEVAN'?", "id": "Berkaitan atau berhubungan." },
  { "en": "Apa antonim dari kata 'MAKAR'?", "id": "Setia atau loyal." },
  { "en": "Apa antonim dari kata 'KRITIK'?", "id": "Pujian." },
  { "en": "Jika 4 pulpen harganya sama dengan 2 buku, dan 3 buku harganya Rp 18.000. Berapa harga 1 pulpen?", "id": "Rp 3.000." },
  { "en": "Kecepatan suara di udara adalah sekitar 340 m/s. Jika Anda mendengar petir 5 detik setelah melihat kilat, berapa jauh petir tersebut?", "id": "1.700 meter atau 1.7 km." },
  { "en": "Dalam sebuah kandang ada 20 hewan, terdiri dari ayam dan kambing. Jika jumlah total kakinya ada 56, berapa jumlah ayam?", "id": "12 ayam (dan 8 kambing)." },
  { "en": "Berapakah 1/5 ditambah 3/4?", "id": "19/20." },
  { "en": "Ayah berusia tiga kali usia anaknya. Jika selisih usia mereka 24 tahun, berapa usia sang anak?", "id": "12 tahun." },
  { "en": "Bagaimana Anda menanggapi keberhasilan rekan Anda?", "id": "Saya akan memberikan ucapan selamat dengan tulus dan turut senang atas pencapaiannya, serta menjadikannya motivasi bagi saya." },
  { "en": "Apa yang akan Anda lakukan di waktu luang Anda?", "id": "Jawaban yang baik adalah yang menunjukkan kegiatan positif seperti berolahraga, membaca, belajar hal baru, atau menghabiskan waktu dengan keluarga." },
  { "en": "Mengapa penting untuk tidak membawa masalah pekerjaan ke rumah?", "id": "Agar dapat menjaga keseimbangan hidup (work-life balance), memulihkan energi secara optimal, dan menjaga keharmonisan dalam keluarga." },
  { "en": "Bagaimana Anda menghadapi situasi yang tidak pasti dan kurangnya informasi?", "id": "Saya akan berusaha mengumpulkan informasi sebanyak mungkin dari sumber yang kredibel, menganalisis situasi berdasarkan data yang ada, dan tidak membuat keputusan terburu-buru." },
  { "en": "Apa yang lebih penting: menjadi anggota tim yang baik atau menjadi seorang bintang individu?", "id": "Menjadi anggota tim yang baik. Keberhasilan institusi dibangun di atas kerja sama tim yang solid, bukan kehebatan individu semata." },
  { "en": "Jika ada seekor sapi menghadap ke Utara, ke arah mana ekornya menghadap?", "id": "Ke bawah." },
  { "en": "Semua peserta ujian memakai kemeja putih. Anton tidak memakai kemeja putih. Kesimpulannya adalah...", "id": "Anton bukan peserta ujian." },
  { "en": "Lengkapi analogi: KERINGAT : TUBUH = GETAH : ...", "id": "POHON." },
  { "en": "Lanjutkan deret: 1, 3, 7, 15, 31, ...", "id": "63 (pola: x2 + 1)." },
  { "en": "Apa sinonim dari 'KAPITAL'?", "id": "Modal." },
  { "en": "Apa antonim dari 'LEGAL'?", "id": "Ilegal." },
  { "en": "Anda melihat ada potensi konflik antara dua kelompok warga di wilayah tugas Anda. Apa langkah preventif Anda?", "id": "Saya akan segera melakukan pendekatan kepada tokoh dari kedua kelompok, memfasilitasi dialog, dan mencari akar permasalahan untuk mencegah konflik pecah." },
  { "en": "Jika 2x + 3y = 12 dan x = 3, berapa nilai y?", "id": "2." },
  { "en": "Bagaimana Anda memandang tugas administrasi atau 'paperwork'?", "id": "Sebagai bagian penting dari pekerjaan yang menjamin akuntabilitas, ketertiban, dan menjadi dasar bagi tindakan operasional di lapangan." },
  { "en": "Jika Anda harus memilih, Anda lebih suka dihormati atau ditakuti oleh masyarakat?", "id": "Dihormati. Rasa hormat datang dari kepercayaan dan profesionalisme, sementara rasa takut bersifat sementara dan menciptakan jarak." },
  { "en": "Anda dihadapkan pada dua laporan intelijen yang saling bertentangan mengenai sebuah potensi ancaman. Langkah analitis apa yang pertama kali Anda lakukan?", "id": "Saya akan membandingkan kedua sumber laporan tersebut untuk menilai kredibilitasnya, mencari data pendukung atau pembanding lain, dan tidak langsung mengambil kesimpulan dari salah satunya." },
  { "en": "Atasan Anda memerintahkan Anda untuk membuat laporan yang datanya sedikit 'diperindah' agar terlihat baik. Apa yang Anda lakukan?", "id": "Saya akan menolak dengan sopan dan menjelaskan bahwa saya harus menyajikan data apa adanya demi integritas laporan. Saya bisa menawarkan untuk menambahkan kolom analisis atau rekomendasi untuk menjelaskan konteks data tersebut." },
  { "en": "Setelah sebuah operasi yang gagal dan mendapat sorotan negatif media, bagaimana Anda memprosesnya dan menjaga mental tim Anda?", "id": "Secara pribadi, saya melakukan introspeksi untuk belajar dari kesalahan. Kepada tim, saya akan menekankan bahwa kegagalan adalah bagian dari proses, mengajak evaluasi bersama secara konstruktif, dan membangkitkan kembali semangat untuk tugas berikutnya." },
  { "en": "Anda harus menyampaikan berita duka kepada keluarga korban. Bagaimana pendekatan komunikasi Anda?", "id": "Saya akan menyampaikannya secara langsung, dengan tenang, penuh empati, dan menggunakan bahasa yang sehalus mungkin. Saya juga akan menawarkan dukungan atau informasi mengenai langkah selanjutnya yang bisa mereka ambil." },
  { "en": "Apa perbedaan antara 'simpati' dan 'empati' dalam konteks pelayanan?", "id": "Simpati adalah merasa kasihan pada kondisi orang lain. Empati adalah kemampuan untuk memahami dan merasakan apa yang orang lain rasakan, yang mendorong kita untuk bertindak dan membantu secara lebih efektif." },
  { "en": "Anda menemukan sebuah metode baru yang bisa membuat pekerjaan Anda jauh lebih efisien, tetapi metode itu belum menjadi prosedur standar. Apa yang Anda lakukan?", "id": "Saya akan menguji coba metode tersebut dalam skala kecil, mendokumentasikan hasilnya, lalu mengajukannya sebagai usulan perbaikan prosedur kepada atasan saya." },
  { "en": "Bagaimana Anda menyikapi seorang rekan yang terlihat sangat ambisius dan terkadang menggunakan 'sikut kanan-kiri'?", "id": "Saya akan menjaga jarak profesional, tidak ikut dalam cara-caranya yang tidak etis, dan fokus untuk menunjukkan kinerja terbaik saya melalui cara yang benar dan berintegritas." },
  { "en": "Menurut Anda, apakah seorang polisi boleh menunjukkan emosi (seperti menangis) saat bertugas?", "id": "Polisi adalah manusia, namun di situasi genting, pengendalian emosi adalah kunci profesionalisme. Menunjukkan emosi secara terkendali di waktu yang tepat (misal saat berempati pada korban) bisa jadi manusiawi, tapi tidak boleh mengganggu pengambilan keputusan." },
  { "en": "Seberapa penting kemampuan berbahasa asing atau bahasa daerah bagi anggota Polri?", "id": "Sangat penting. Bahasa daerah membantu komunikasi dan kedekatan dengan masyarakat lokal, sementara bahasa asing penting dalam menghadapi kejahatan transnasional atau melayani turis." },
  { "en": "Apa yang Anda lakukan jika Anda merasa jenuh bukan karena pekerjaan, tetapi karena lingkungan kerja yang kurang mendukung?", "id": "Saya akan mencoba menjadi agen perubahan positif dengan membangun hubungan baik, tidak terlibat dalam dinamika negatif, dan fokus pada kontribusi saya, sambil tetap membuka opsi untuk berdiskusi dengan atasan mengenai suasana kerja." },
  { "en": "Lengkapi analogi: MERDEKA : PENJAJAHAN = LULUS : ...", "id": "UJIAN." },
  { "en": "Lengkapi analogi: RUMAH : RUANGAN = BUKU : ...", "id": "BAB (atau HALAMAN)." },
  { "en": "Lengkapi analogi: PETIR : GURUH = KILAT : ...", "id": "CAHAYA." },
  { "en": "Lengkapi analogi: FAKTA : OPINI = OBJEKTIF : ...", "id": "SUBJEKTIF." },
  { "en": "Lengkapi analogi: HUJAN : BANJIR = KEMARAU : ...", "id": "KEKERINGAN." },
  { "en": "Lengkapi analogi: TUNA : IKAN = BAYAM : ...", "id": "SAYURAN." },
  { "en": "Lengkapi analogi: PASIR : KACA = KAYU : ...", "id": "KERTAS." },
  { "en": "Lengkapi analogi: DESIBEL : SUARA = VOLT : ...", "id": "LISTRIK (TEGANGAN)." },
  { "en": "Lengkapi analogi: PENA : MENULIS = MIKROSKOP : ...", "id": "MENGAMATI." },
  { "en": "Lengkapi analogi: PASPOR : NEGARA = KARCIS : ...", "id": "BIOSKOP (atau PERTUNJUKAN)." },
  { "en": "Lanjutkan deret angka: 1, 5, 13, 29, 61, ...", "id": "125 (pola: x2 + 3)." },
  { "en": "Lanjutkan deret angka: 1, 4, 27, 256, 3125, ...", "id": "46656 (pola: n pangkat n -> 1¹, 2², 3³, 4⁴, 5⁵, 6⁶)." },
  { "en": "Lanjutkan deret huruf: A, C, B, D, C, E, D, ...", "id": "F (pola: +2, -1, +2, -1, dst)." },
  { "en": "Lanjutkan deret angka: 1/2, 3/4, 1, 5/4, 3/2, ...", "id": "7/4 (pola: ditambah 1/4)." },
  { "en": "Lanjutkan deret angka: 9, 10, 8, 11, 7, 12, ...", "id": "6 (pola dua larik: 9,8,7,6,... dan 10,11,12,...)." },
  { "en": "Lanjutkan deret angka: 3, 6, 11, 20, 37, ...", "id": "70 (pola: x2 - 0, x2 - 1, x2 - 2, x2 - 3, dst)." },
  { "en": "Lanjutkan deret huruf: A, Z, D, W, G, T, ...", "id": "J (pola dua larik: A,D,G,J maju 2 huruf; Z,W,T mundur 2 huruf)." },
  { "en": "Lanjutkan deret angka: 2, 6, 10, 14, 18, ...", "id": "22 (pola: ditambah 4)." },
  { "en": "Lanjutkan deret angka: 88, 81, 74, 67, 60, ...", "id": "53 (pola: dikurang 7)." },
  { "en": "Lanjutkan deret angka: 4, 4, 8, 12, 20, 32, ...", "id": "52 (pola: deret Fibonacci modifikasi)." },
  { "en": "Apa sinonim dari kata 'AGITASI'?", "id": "Hasutan." },
  { "en": "Apa sinonim dari kata 'INSPEKSI'?", "id": "Pemeriksaan." },
  { "en": "Apa sinonim dari kata 'INSENTIF'?", "id": "Bonus atau perangsang." },
  { "en": "Apa sinonim dari kata 'LEGITIMASI'?", "id": "Pengesahan." },
  { "en": "Apa sinonim dari kata 'SANKSI'?", "id": "Hukuman." },
  { "en": "Apa antonim dari kata 'Sporadis'?", "id": "Rutin atau sering." },
  { "en": "Apa antonim dari kata 'PROFESIONAL'?", "id": "Amatir." },
  { "en": "Apa antonim dari kata 'RAPUH'?", "id": "Kuat atau kokoh." },
  { "en": "Apa antonim dari kata 'RESPEK'?", "id": "Hina atau remeh." },
  { "en": "Apa antonim dari kata 'SENIOR'?", "id": "Junior." },
  { "en": "Aturan 1: Jika A hadir, B tidak boleh hadir. Aturan 2: C hanya hadir jika B hadir. Jika A hadir, apa yang bisa dipastikan mengenai C?", "id": "C pasti tidak hadir." },
  { "en": "Semua mamalia berdarah panas. Beberapa hewan di air adalah mamalia. Kesimpulannya adalah...", "id": "Beberapa hewan di air berdarah panas." },
  { "en": "Jika hari tidak hujan, Amir pergi memancing. Hari ini Amir tidak pergi memancing. Kesimpulannya adalah...", "id": "Hari ini hujan (meskipun bisa ada sebab lain, ini kesimpulan logis dari premis)." },
  { "en": "Toko A lebih ramai dari Toko B. Toko C seramai Toko A. Toko D lebih sepi dari Toko B. Mana toko yang paling sepi?", "id": "Toko D." },
  { "en": "Tidak ada pahlawan yang penakut. Beberapa tentara adalah penakut. Kesimpulannya adalah...", "id": "Beberapa tentara bukanlah pahlawan." },
  { "en": "Andi mengecat rumah dalam 4 jam. Budi dapat mengecat rumah yang sama dalam 6 jam. Jika mereka bekerja bersama, berapa lama waktu yang dibutuhkan?", "id": "2.4 jam atau 2 jam 24 menit." },
  { "en": "Sebuah drum berisi air 1/3 bagian. Jika ditambahkan 3 liter, isinya menjadi 1/2 bagian. Berapa kapasitas drum tersebut?", "id": "18 liter." },
  { "en": "Berapakah median dari data berikut: 7, 8, 5, 9, 6, 8, 7?", "id": "7 (setelah diurutkan: 5,6,7,7,8,8,9)." },
  { "en": "Sebuah persegi panjang memiliki keliling 30 cm. Jika panjangnya 9 cm, berapa luasnya?", "id": "54 cm² (Lebar = (30/2) - 9 = 6 cm. Luas = 9x6)." },
  { "en": "Jika 3 tahun yang lalu umur A adalah 12 tahun, dan 5 tahun yang akan datang umur B adalah 25 tahun, berapa selisih umur mereka saat ini?", "id": "5 tahun (Umur A sekarang 15, Umur B sekarang 20)." },
  { "en": "Bagaimana Anda menjelaskan pentingnya 'presisi' dalam sebuah laporan investigasi?", "id": "Presisi memastikan setiap detail akurat dan dapat dipertanggungjawabkan, yang menjadi dasar kuat untuk tindakan hukum dan menghindari kesalahan fatal." },
  { "en": "Bagaimana Anda memandang kritik dari masyarakat terhadap kinerja Polri?", "id": "Sebagai masukan yang berharga untuk evaluasi dan perbaikan. Kritik menunjukkan bahwa masyarakat peduli dan memiliki harapan tinggi terhadap Polri." },
  { "en": "Apa yang Anda lakukan jika Anda merasa bawahan Anda lebih kompeten dalam suatu bidang teknis tertentu?", "id": "Saya akan memberdayakannya, memberikan kepercayaan untuk menangani bidang tersebut, dan saya sendiri akan belajar darinya. Itu menunjukkan kepemimpinan yang baik." },
  { "en": "Dalam situasi yang membahayakan, bagaimana Anda menyeimbangkan antara keberanian dan perhitungan risiko?", "id": "Keberanian harus didasari oleh perhitungan risiko yang matang. Saya akan bertindak berani setelah menganalisis situasi dan mengambil langkah yang paling aman dan efektif." },
  { "en": "Apakah Anda orang yang mudah memaafkan kesalahan orang lain?", "id": "Ya, saya percaya setiap orang bisa berbuat salah. Saya mudah memaafkan, namun tetap memberikan catatan agar kesalahan tersebut tidak diulangi, terutama dalam tugas." },
  { "en": "Bagaimana Anda bersikap jika ada peraturan baru yang Anda secara pribadi tidak setujui namun harus Anda tegakkan?", "id": "Saya akan menegakkannya secara profesional karena itu adalah tugas saya. Ketidaksetujuan pribadi tidak boleh menghalangi pelaksanaan kewajiban dinas." },
  { "en": "Apa pentingnya memiliki hobi atau kegiatan di luar pekerjaan?", "id": "Sangat penting untuk menjaga keseimbangan hidup (work-life balance), mengelola stres, dan menyegarkan kembali pikiran agar bisa kembali bekerja dengan optimal." },
  { "en": "Seorang teman dekat meminta bantuan Anda untuk 'mengamankan' saudaranya yang terlibat kasus pidana ringan. Apa sikap Anda?", "id": "Saya akan menolak dengan tegas. Saya akan menjelaskan bahwa proses hukum harus berjalan adil bagi semua orang tanpa pandang bulu." },
  { "en": "Bagaimana Anda memastikan diri Anda terus relevan dengan perkembangan zaman dan tantangan kepolisian modern?", "id": "Dengan memiliki rasa ingin tahu yang tinggi, proaktif belajar hal baru, mengikuti pelatihan, dan terbuka terhadap teknologi serta metode kerja baru." },
  { "en": "Menurut Anda, apa warisan (legacy) yang ingin Anda tinggalkan setelah selesai mengabdi di Kepolisian?", "id": "Jawaban yang baik mencerminkan nilai-nilai luhur, seperti dikenal sebagai polisi yang jujur, adil, profesional, dan dekat dengan masyarakat." },
  { "en": "Lengkapi analogi: GELAP : LAMPU = DINGIN : ...", "id": "SELIMUT (atau JAKET)." },
  { "en": "Lengkapi analogi: SEMEN : DINDING = LEM : ...", "id": "KERTAS." },
  { "en": "Lengkapi analogi: PADI : PETANI = BERITA : ...", "id": "JURNALIS." },
  { "en": "Lengkapi analogi: ATOM : MOLEKUL = SEL : ...", "id": "ORGAN." },
  { "en": "Lengkapi analogi: TERBIT : MATAHARI = PASANG : ...", "id": "LAUT." },
  { "en": "Lanjutkan deret angka: 5, 6, 4, 7, 3, 8, ...", "id": "2 (pola dua larik: 5,4,3,2,... dan 6,7,8,...)." },
  { "en": "Lanjutkan deret angka: 1, 10, 3, 11, 5, 12, ...", "id": "7 (pola dua larik: 1,3,5,7,... dan 10,11,12,...)." },
  { "en": "Lanjutkan deret huruf: A, D, H, M, S, ...", "id": "Z (pola loncat: +3, +4, +5, +6, +7)." },
  { "en": "Lanjutkan deret angka: 2, 8, 18, 32, 50, ...", "id": "72 (pola: 2 x n² -> 2x1², 2x2², 2x3², dst)." },
  { "en": "Lanjutkan deret angka: 10, 20, 25, 35, 40, 50, ...", "id": "55 (pola: +10, +5, +10, +5, diulang)." },
  { "en": "Apa sinonim dari kata 'PREMIS'?", "id": "Dasar pemikiran atau asumsi." },
  { "en": "Apa sinonim dari kata 'MITIGASI'?", "id": "Peringanan atau pengurangan risiko." },
  { "en": "Apa sinonim dari kata 'DISTORSI'?", "id": "Penyimpangan." },
  { "en": "Apa antonim dari kata 'EKSPANSI'?", "id": "Kontraksi atau penyusutan." },
  { "en": "Apa antonim dari kata 'MAYORITAS'?", "id": "Minoritas." },
  { "en": "Jika a > b > 0, manakah yang nilainya paling kecil?", "id": "b." },
  { "en": "Berapakah 20% dari (15% dari 200)?", "id": "6." },
  { "en": "Sebuah mobil berjalan dengan kecepatan 60 km/jam. Berapa jauh mobil itu berjalan dalam 45 menit?", "id": "45 km." },
  { "en": "Jika 5 kucing menangkap 5 tikus dalam 5 menit, berapa waktu yang dibutuhkan 100 kucing untuk menangkap 100 tikus?", "id": "5 menit (Setiap kucing butuh 5 menit untuk menangkap 1 tikus)." },
  { "en": "Berapakah modus dari data berikut: 2,3,4,4,5,5,5,6,7?", "id": "5." },
  { "en": "Bagaimana Anda membangun 'chemistry' atau kekompakan dengan rekan kerja yang baru?", "id": "Dengan memulai obrolan ringan di luar pekerjaan, menunjukkan minat pada pekerjaannya, menawarkan bantuan, dan bersikap terbuka." },
  { "en": "Apa yang Anda lakukan saat menghadapi hari yang benar-benar tidak produktif?", "id": "Saya akan menerimanya sebagai hal yang wajar sesekali terjadi, mengevaluasi penyebabnya, dan bertekad untuk menjadi lebih baik dan lebih fokus esok hari." },
  { "en": "Bagaimana Anda memandang kekayaan materi bagi seorang abdi negara?", "id": "Sebagai penunjang kehidupan yang wajar, bukan sebagai tujuan utama. Tujuan utama adalah pengabdian, dan kekayaan harus diperoleh dengan cara yang halal dan sah." },
  { "en": "Jika Anda harus memilih, Anda lebih ingin menjadi ahli (spesialis) di satu bidang atau bisa banyak hal (generalis)?", "id": "Jawaban yang baik adalah menunjukkan fleksibilitas, misal: 'Saya ingin memiliki satu keahlian mendalam, namun juga memiliki pengetahuan umum yang luas untuk beradaptasi di berbagai situasi'." },
  { "en": "Apa arti 'menjaga kehormatan' di luar jam dinas?", "id": "Artinya adalah tetap berperilaku sesuai norma hukum dan sosial yang berlaku di masyarakat, menjaga tutur kata dan perbuatan karena status sebagai anggota Polri melekat 24 jam." },
  { "en": "Semua persegi adalah belah ketupat. Beberapa belah ketupat memiliki sudut tumpul. Kesimpulannya adalah...", "id": "Tidak dapat disimpulkan apakah persegi memiliki sudut tumpul (karena hanya beberapa belah ketupat)." },
  { "en": "Jika hari ini bukan hari Rabu, maka saya memakai baju biru. Hari ini saya tidak memakai baju biru. Kesimpulannya?", "id": "Hari ini adalah hari Rabu." },
  { "en": "Lengkapi analogi: PENYAIR : PUISI = KARTUNIS : ...", "id": "KARTUN." },
  { "en": "Lanjutkan deret: 4, 5, 8, 10, 16, 20, ...", "id": "32 (pola dua larik: 4,8,16,32 [x2] dan 5,10,20 [x2])." },
  { "en": "Apa sinonim dari 'AMBIGU'?", "id": "Bermakna ganda." },
  { "en": "Apa antonim dari 'GUGUR' (dalam pertempuran)?", "id": "Selamat (atau hidup)." },
  { "en": "Anda melihat seorang anak jalanan yang potensial menjadi korban eksploitasi. Apa yang dapat Anda lakukan?", "id": "Saya akan berkoordinasi dengan unit PPA (Perlindungan Perempuan dan Anak) atau dinas sosial untuk memastikan anak tersebut mendapatkan penanganan dan perlindungan yang tepat." },
  { "en": "Berapakah 2³ + √81?", "id": "17 (8 + 9)." },
  { "en": "Bagaimana Anda memastikan Anda tidak menyalahgunakan wewenang yang Anda miliki?", "id": "Dengan selalu mengingat sumpah jabatan, berpegang teguh pada prosedur, dan memahami bahwa wewenang tersebut adalah amanah untuk melayani, bukan untuk dilayani." },
  { "en": "Mana yang lebih Anda takuti: membuat kesalahan teknis atau melakukan pelanggaran etis?", "id": "Melakukan pelanggaran etis. Kesalahan teknis bisa diperbaiki dan dipelajari, tetapi pelanggaran etis merusak kepercayaan dan integritas yang sulit dibangun kembali." },
  { "en": "Di wilayah tugas Anda, sering terjadi kemacetan di satu titik pada jam tertentu yang belum ada solusinya. Apa inisiatif yang bisa Anda lakukan?", "id": "Saya akan melakukan observasi untuk mengidentifikasi penyebabnya, kemudian berkoordinasi dengan unit lalu lintas atau dinas perhubungan untuk mengusulkan rekayasa lalu lintas sederhana atau penempatan petugas pada jam sibuk." },
  { "en": "Seorang rekan yang Anda percaya mengaku kepada Anda secara pribadi bahwa ia melakukan pelanggaran disiplin ringan karena alasan keluarga yang mendesak. Apa sikap Anda?", "id": "Saya akan berempati pada masalahnya, namun tetap menasihatinya untuk melaporkan dan bertanggung jawab atas pelanggarannya kepada atasan. Menutupi masalah hanya akan memperburuk situasi." },
  { "en": "Anda ditunjuk mendadak untuk memberikan penyuluhan bahaya narkoba di sekolah karena petugas yang seharusnya berhalangan. Apa yang Anda lakukan?", "id": "Saya akan menerima tugas tersebut, dengan cepat menyusun kerangka materi berdasarkan pengetahuan dan pengalaman saya, dan menyampaikannya dengan bahasa yang mudah dipahami serta interaktif bagi para siswa." },
  { "en": "Anda menangani beberapa kasus pencurian dengan modus operandi yang identik di wilayah yang sama. Apa kesimpulan awal dan langkah investigasi Anda?", "id": "Kesimpulan awal saya adalah kemungkinan pelakunya orang atau kelompok yang sama. Langkah selanjutnya adalah memetakan lokasi dan waktu kejadian untuk mencari pola, serta meningkatkan patroli di area rawan tersebut." },
  { "en": "Apa perbedaan antara 'wewenang' dan 'kekuasaan'?", "id": "Wewenang adalah hak formal yang melekat pada jabatan untuk mengambil keputusan dan memberi perintah. Kekuasaan adalah kemampuan untuk mempengaruhi orang lain, yang bisa bersumber dari wewenang, kharisma, atau keahlian." },
  { "en": "Anda merasa pekerjaan Anda tidak lagi memberikan tantangan. Bagaimana cara Anda mengatasinya?", "id": "Saya akan proaktif meminta tugas tambahan yang lebih menantang, menjadi mentor bagi junior, atau mengusulkan sebuah proyek/kegiatan baru yang bermanfaat bagi satuan." },
  { "en": "Bagaimana Anda menjelaskan suatu kegagalan kepada atasan tanpa terkesan mencari alasan atau menyalahkan orang lain?", "id": "Saya akan fokus menjelaskan kronologi kejadian secara objektif, mengakui bagian di mana saya atau tim saya kurang, dan langsung menyajikan analisis serta usulan langkah perbaikan ke depan." },
  { "en": "Menurut Anda, apakah seorang polisi harus selalu terlihat serius dan kaku?", "id": "Tidak. Seorang polisi harus bisa menyesuaikan sikap. Serius dan tegas saat penegakan hukum, namun tetap bisa humanis, ramah, dan komunikatif saat berinteraksi dengan masyarakat." },
  { "en": "Anda mendapat informasi intelijen dari sumber yang belum terverifikasi. Apa yang Anda lakukan dengan informasi tersebut?", "id": "Saya akan menandainya sebagai informasi yang belum matang (raw), tidak akan bertindak gegabah atas informasi itu, dan akan melakukan kroscek (verifikasi) melalui sumber-sumber lain yang kredibel." },
  { "en": "Bagaimana Anda membangun mental 'tahan banting' dalam menghadapi tekanan dan risiko pekerjaan?", "id": "Dengan membangun pola pikir positif, melihat tantangan sebagai peluang bertumbuh, memiliki support system yang baik (keluarga/rekan), dan menjaga keseimbangan antara kerja dan istirahat." },
  { "en": "Lengkapi analogi: LETNAN : PERWIRA = SERSAN : ...", "id": "BINTARA." },
  { "en": "Lengkapi analogi: SAHAM : INVESTOR = PROPERTI : ...", "id": "PENGUSAHA (atau DEVELOPER)." },
  { "en": "Lengkapi analogi: PANTAI : ABRASI = GUNUNG : ...", "id": "EROSI." },
  { "en": "Lengkapi analogi: TESIS : ARGUMEN = PUISI : ...", "id": "MAJAS (atau IMAJI)." },
  { "en": "Lengkapi analogi: DARAH : OKSIGEN = KABEL : ...", "id": "DATA (atau LISTRIK)." },
  { "en": "Lengkapi analogi: PRESIDEN : NEGARA = GUBERNUR : ...", "id": "PROVINSI." },
  { "en": "Lengkapi analogi: PENGGARIS : PANJANG = TERMOMETER : ...", "id": "SUHU." },
  { "en": "Lengkapi analogi: DETEKTIF : PENYELIDIKAN = ARKEOLOG : ...", "id": "EKSKAVASI." },
  { "en": "Lengkapi analogi: BUNGA : RIBA = INVESTASI : ...", "id": "KEUNTUNGAN." },
  { "en": "Lengkapi analogi: JANGKAR : KAPAL = REM : ...", "id": "MOBIL (atau KENDARAAN)." },
  { "en": "Lanjutkan deret angka: 2, 4, 10, 28, 82, ...", "id": "244 (pola: x3 - 2)." },
  { "en": "Lanjutkan deret angka: 1, 1, 2, 3, 5, 8, 13, ...", "id": "21 (pola: deret Fibonacci)." },
  { "en": "Lanjutkan deret huruf: Z, X, U, Q, L, ...", "id": "E (pola: -2, -3, -4, -5, -6 huruf)." },
  { "en": "Lanjutkan deret angka: 3, 4, 6, 9, 13, 18, ...", "id": "24 (pola: +1, +2, +3, +4, +5, +6)." },
  { "en": "Lanjutkan deret angka: 10, 5, 15, 10, 20, 15, ...", "id": "25 (pola dua larik: 10,15,20,25,... dan 5,10,15,...)." },
  { "en": "Lanjutkan deret angka: 1000, 800, 640, 512, ...", "id": "409.6 (pola: dikali 0.8 atau dikurangi 20%)." },
  { "en": "Lanjutkan deret huruf: A, Z, C, X, F, U, ...", "id": "J (pola dua larik: A,C,F,J loncat +2,+3,+4; dan Z,X,U mundur -2,-3)." },
  { "en": "Lanjutkan deret angka: 4, 16, 36, 64, 100, ...", "id": "144 (pola: kuadrat dari bilangan genap: 2², 4², 6², 8², 10², 12²)." },
  { "en": "Lanjutkan deret angka: 6, 12, 14, 28, 30, ...", "id": "60 (pola: x2, +2, diulang)." },
  { "en": "Lanjutkan deret angka: 2, 5, 7, 12, 19, 31, ...", "id": "50 (pola: deret Lucas, U(n) = U(n-1) + U(n-2))." },
  { "en": "Apa sinonim dari kata 'INTERVENSI'?", "id": "Campur tangan." },
  { "en": "Apa sinonim dari kata 'KLAIM'?", "id": "Tuntutan atau pernyataan." },
  { "en": "Apa sinonim dari kata 'VERIFIKASI'?", "id": "Pembuktian atau pemeriksaan." },
  { "en": "Apa sinonim dari kata 'OPORTUNIS'?", "id": "Orang yang mencari keuntungan untuk diri sendiri." },
  { "en": "Apa sinonim dari kata 'PREVENTIF'?", "id": "Pencegahan." },
  { "en": "Apa antonim dari kata 'KRONIS'?", "id": "Akut." },
  { "en": "Apa antonim dari kata 'MANDATORI'?", "id": "Sukarela atau opsional." },
  { "en": "Apa antonim dari kata 'NETRAL'?", "id": "Memihak atau bias." },
  { "en": "Apa antonim dari kata 'APRESIASI'?", "id": "Cemoohan atau kritik negatif." },
  { "en": "Apa antonim dari kata 'KONVERGEN'?", "id": "Divergen." },
  { "en": "Ada 3 kotak (A, B, C), satu berisi emas. Label A: 'Emas di B'. Label B: 'Emas tidak di sini'. Label C: 'Emas tidak di A'. Jika hanya satu label yang benar, di mana emasnya?", "id": "Di Kotak A (Jika emas di A, maka label A salah, label B salah, dan label C benar. Sesuai syarat)." },
  { "en": "Jika Tono bukan seorang pelaut, maka ia adalah seorang petani. Tono bukan seorang petani. Kesimpulannya adalah...", "id": "Tono adalah seorang pelaut." },
  { "en": "Beberapa politisi adalah pengusaha. Setiap pengusaha membayar pajak. Kesimpulannya adalah...", "id": "Beberapa politisi membayar pajak." },
  { "en": "Urutkan kecepatan berikut dari yang paling lambat: Kuda, Siput, Cheetah, Manusia.", "id": "Siput, Manusia, Kuda, Cheetah." },
  { "en": "Semua mamalia bernapas dengan paru-paru. Semua hewan yang bernapas dengan paru-paru butuh oksigen. Kesimpulannya adalah...", "id": "Semua mamalia butuh oksigen." },
  { "en": "Sebuah barang dijual dengan kerugian 10%. Jika harga jualnya Rp 90.000, berapa harga beli barang tersebut?", "id": "Rp 100.000." },
  { "en": "Ayah 4 tahun lebih tua dari Ibu. Jumlah umur mereka sekarang adalah 68 tahun. Berapa umur Ibu sekarang?", "id": "32 tahun (dan Ayah 36 tahun)." },
  { "en": "Berapakah 1/2 dari 3/4?", "id": "3/8." },
  { "en": "Jika 1 dollar = Rp 15.000, berapakah nilai 25 dollar dalam rupiah?", "id": "Rp 375.000." },
  { "en": "Sebuah tes terdiri dari 40 soal. Jawaban benar diberi skor 4, salah -2, dan tidak dijawab -1. Jika Budi menjawab 30 soal benar dan 5 salah, berapa total skornya?", "id": "105 (30x4 - 5x2 - 5x1 = 120 - 10 - 5)." },
  { "en": "Anda diminta untuk memimpin doa dalam sebuah acara resmi mendadak. Apa yang Anda lakukan?", "id": "Saya akan menerimanya sebagai kehormatan dan memimpin doa secara singkat, umum, dan khidmat sesuai dengan keyakinan saya, dengan tetap menghormati keyakinan lain yang hadir." },
  { "en": "Bagaimana Anda menjelaskan kepada anak Anda tentang pekerjaan Anda sebagai polisi?", "id": "Saya akan menjelaskan dengan bahasa sederhana bahwa pekerjaan saya adalah membantu dan melindungi orang-orang agar semua bisa merasa aman." },
  { "en": "Apa yang lebih penting dalam sebuah tim: kesamaan pendapat atau keragaman pendapat?", "id": "Keragaman pendapat lebih penting untuk menghasilkan ide-ide inovatif dan keputusan yang lebih baik, asalkan dikelola dengan baik untuk mencapai tujuan bersama." },
  { "en": "Bagaimana Anda menyikapi rekan yang lebih muda namun memiliki pangkat lebih tinggi?", "id": "Saya akan tetap menunjukkan sikap hormat sesuai hierarki kepangkatan, sambil menjaga hubungan kerja yang profesional dan kolaboratif." },
  { "en": "Apa yang Anda lakukan jika Anda merasa putusan pengadilan terhadap kasus yang Anda tangani tidak adil?", "id": "Sebagai penegak hukum, saya harus menghormati putusan pengadilan yang berkekuatan hukum tetap, sambil melakukan evaluasi internal pada proses penyidikan sebagai pembelajaran." },
  { "en": "Bagaimana Anda membangun kredibilitas di lingkungan kerja yang baru?", "id": "Dengan menunjukkan etos kerja yang tinggi, konsisten antara ucapan dan tindakan, mau belajar, dan proaktif dalam berkontribusi pada tugas-tugas tim." },
  { "en": "Apa arti 'humanis' dalam tindakan kepolisian?", "id": "Artinya, dalam setiap tindakan, polisi harus selalu memanusiakan manusia, menghargai harkat dan martabat, serta menggunakan pendekatan persuasif sebelum represif." },
  { "en": "Jika Anda memiliki waktu luang satu jam di kantor, apa yang akan Anda lakukan?", "id": "Saya akan menggunakannya untuk merapikan berkas, membaca peraturan atau update informasi terbaru terkait pekerjaan, atau berdiskusi hal-hal positif dengan rekan." },
  { "en": "Apa perbedaan antara 'fakta' dan 'data'?", "id": "Data adalah catatan mentah atas suatu kejadian atau pengukuran. Fakta adalah data yang telah diolah, diverifikasi, dan diakui kebenarannya." },
  { "en": "Bagaimana Anda menolak ajakan rekan untuk melakukan hal yang tidak bermanfaat (misal: nongkrong terlalu lama saat jam kerja)?", "id": "Saya akan menolak dengan sopan sambil memberi alasan yang logis, misalnya 'Maaf, saya harus menyelesaikan laporan ini dulu', tanpa terkesan menggurui." },
  { "en": "Lengkapi analogi: TELER : TELEVISI = ... : KORAN", "id": "PEMBACA." },
  { "en": "Lengkapi analogi: PANEN : PETANI = DIVIDEN : ...", "id": "INVESTOR." },
  { "en": "Lengkapi analogi: AIR MATA : KESEDIHAN = KERINGAT : ...", "id": "KELELAHAN (atau USAHA)." },
  { "en": "Lengkapi analogi: KOREOGRAFER : TARI = ARSITEK : ...", "id": "BANGUNAN." },
  { "en": "Lengkapi analogi: KATA PENGANTAR : BUKU = TRAILER : ...", "id": "FILM." },
  { "en": "Lanjutkan deret angka: 1, 2, 6, 24, 120, ...", "id": "720 (pola: faktorial 1!, 2!, 3!, 4!, 5!, 6!)." },
  { "en": "Lanjutkan deret angka: 2, 5, 4, 7, 6, 9, ...", "id": "8 (pola dua larik: 2,4,6,8,... dan 5,7,9,...)." },
  { "en": "Lanjutkan deret huruf: A, F, K, P, U, ...", "id": "Z (pola loncat 4 huruf)." },
  { "en": "Lanjutkan deret angka: 1, 3, 4, 7, 11, 18, ...", "id": "29 (pola: deret Fibonacci modifikasi)." },
  { "en": "Lanjutkan deret angka: 81, 72, 63, 54, 45, ...", "id": "36 (pola: dikurang 9)." },
  { "en": "Apa sinonim dari kata 'ENTITAS'?", "id": "Wujud atau satuan." },
  { "en": "Apa sinonim dari kata 'FUNDAMENTAL'?", "id": "Mendasar." },
  { "en": "Apa sinonim dari kata 'ALIANSI'?", "id": "Persekutuan." },
  { "en": "Apa antonim dari kata 'KEKAL'?", "id": "Fana atau sementara." },
  { "en": "Apa antonim dari kata 'BERAT HATI'?", "id": "Ikhlas." },
  { "en": "Berapakah 30% dari 1/2?", "id": "0.15 atau 3/20." },
  { "en": "Jika x+y=10 dan x-y=4, berapakah nilai x?", "id": "7." },
  { "en": "Sebuah bakteri membelah diri menjadi dua setiap 5 menit. Jika pada awalnya ada 1 bakteri, berapa jumlahnya setelah 20 menit?", "id": "16 bakteri." },
  { "en": "Berapa jumlah sisi, rusuk, dan sudut pada sebuah kubus?", "id": "6 sisi, 12 rusuk, 8 sudut." },
  { "en": "Luas sebuah lingkaran adalah 154 cm². Berapakah jari-jarinya? (Gunakan π=22/7)", "id": "7 cm." },
  { "en": "Bagaimana Anda merespon jika seorang warga memberikan informasi palsu dengan sengaja?", "id": "Saya akan mencatat informasinya, melakukan verifikasi, dan jika terbukti palsu dan berpotensi mengganggu, saya akan mempertimbangkan untuk melakukan tindakan hukum sesuai aturan yang berlaku." },
  { "en": "Apa yang Anda pahami tentang 'Keadilan Restoratif' (Restorative Justice)?", "id": "Sebuah pendekatan penyelesaian perkara pidana yang fokus pada pemulihan korban dan pelaku, serta melibatkan partisipasi masyarakat untuk mencapai kesepakatan, terutama untuk kasus-kasus ringan." },
  { "en": "Apakah Anda lebih suka bekerja dengan target yang jelas atau dengan kebebasan berkreasi?", "id": "Saya suka bekerja dengan target yang jelas sebagai panduan, namun saya juga menghargai kebebasan untuk berkreasi dalam cara mencapai target tersebut secara efektif dan efisien." },
  { "en": "Bagaimana Anda menghadapi perubahan teknologi yang sangat cepat dalam pekerjaan?", "id": "Saya akan bersikap proaktif untuk mempelajarinya, melihatnya sebagai alat untuk mempermudah pekerjaan, dan tidak takut untuk bertanya atau mengikuti pelatihan." },
  { "en": "Menurut Anda, apa dosa terbesar bagi seorang penegak hukum?", "id": "Menyalahgunakan wewenang untuk kepentingan pribadi dan mengkhianati kepercayaan masyarakat, karena itu merusak fondasi keadilan." },
  { "en": "Beberapa A adalah B. Beberapa B adalah C. Apakah pasti ada A yang merupakan C?", "id": "Tidak pasti." },
  { "en": "Jika semua kucing suka ikan, dan Miko tidak suka ikan. Kesimpulannya adalah...", "id": "Miko bukan kucing." },
  { "en": "Lengkapi analogi: BAWANG : SIUNG = PISANG : ...", "id": "SISIR." },
  { "en": "Lanjutkan deret: Z, A, Y, B, X, C, ...", "id": "W (pola dua larik: Z,Y,X,W,... dan A,B,C,...)." },
  { "en": "Apa sinonim dari 'PARADIGMA'?", "id": "Kerangka berpikir." },
  { "en": "Apa antonim dari 'BERHASIL'?", "id": "Gagal." },
  { "en": "Anda ditugaskan di unit yang tidak sesuai dengan minat atau latar belakang pendidikan Anda. Bagaimana sikap Anda?", "id": "Saya akan menerima tugas tersebut sebagai kesempatan untuk belajar hal baru, berusaha memberikan yang terbaik, dan membuktikan bahwa saya bisa beradaptasi di bidang manapun." },
  { "en": "Berapa rata-rata dari angka berikut: 10, 20, 30, 40, 50?", "id": "30." },
  { "en": "Bagaimana Anda mengelola stres setelah menyaksikan kejadian traumatis?", "id": "Saya akan melakukan teknik relaksasi (debriefing), berbicara dengan rekan atau atasan yang saya percaya, dan jika perlu, tidak ragu untuk meminta bantuan dari psikolog dinas." },
  { "en": "Mana yang menjadi prioritas Anda: menyelesaikan banyak tugas dengan kualitas standar atau sedikit tugas dengan kualitas sempurna?", "id": "Memprioritaskan tugas yang paling penting untuk diselesaikan dengan kualitas terbaik, sambil tetap berusaha menyelesaikan semua tugas lain dengan standar kualitas yang baik dan tepat waktu." },
  { "en": "Anda harus membuat keputusan cepat di lapangan dengan informasi yang sangat terbatas. Apa yang menjadi dasar utama pengambilan keputusan Anda?", "id": "Dasar utama saya adalah prinsip keselamatan (jiwa manusia), diikuti oleh perkiraan logis terhadap dampak tindakan (mana yang risikonya terkecil), dan intuisi yang terlatih dari pengalaman." },
  { "en": "Dalam operasi gabungan, terjadi perbedaan pendapat mengenai strategi antara tim Anda (Polri) dan tim dari institusi lain. Bagaimana Anda menyikapinya?", "id": "Saya akan mencari titik temu dengan fokus pada tujuan bersama operasi, bukan ego sektoral. Saya akan menyajikan data dan argumen dari sudut pandang kami secara logis dan mendengarkan masukan mereka untuk mencapai strategi terbaik." },
  { "en": "Apa kelemahan terbesar yang Anda sadari ada pada diri Anda, dan langkah konkret apa yang akan Anda lakukan untuk memperbaikinya?", "id": "Jawaban yang baik adalah yang menunjukkan kesadaran diri (misal: 'Saya terkadang kurang sabar') dan menyajikan solusi konkret (misal: 'Saya belajar untuk lebih banyak mendengar dan tidak cepat memotong pembicaraan')." },
  { "en": "Bagaimana Anda memastikan dalam proses interogasi, Anda tetap menjunjung tinggi HAM tanpa mengurangi efektivitas penggalian informasi?", "id": "Dengan menggunakan teknik interogasi yang cerdas (bukan kekerasan), membangun rapport, dan selalu memastikan hak-hak terduga (seperti didampingi pengacara) terpenuhi sesuai prosedur." },
  { "en": "Apa arti 'netralitas Polri' dalam konteks tahun politik atau Pemilu?", "id": "Artinya Polri tidak memihak pada kontestan atau partai politik manapun, fokus sepenuhnya pada pengamanan penyelenggaraan Pemilu, dan menegakkan hukum secara adil bagi semua pihak." },
  { "en": "Anda melihat rekan kerja yang baik dan jujur, tetapi kinerjanya sangat lambat. Apa yang akan Anda lakukan?", "id": "Saya akan menawarkan bantuan secara tulus, mungkin dengan berbagi tips atau cara kerja yang lebih efisien, tanpa membuatnya merasa dihakimi atau direndahkan." },
  { "en": "Bagaimana Anda membedakan antara 'memiliki pendirian' dengan 'keras kepala'?", "id": "Memiliki pendirian berarti berpegang pada prinsip yang benar berdasarkan data dan logika. Keras kepala adalah menolak untuk menerima masukan atau data baru meskipun pendapat kita terbukti keliru." },
  { "en": "Jika Anda dipindahtugaskan ke unit yang sama sekali tidak Anda minati, bagaimana Anda menemukan motivasi?", "id": "Saya akan melihatnya sebagai tantangan untuk menguasai bidang baru, mencari aspek-aspek positif dari tugas tersebut, dan meyakini bahwa setiap penempatan adalah bagian dari rencana pengembangan karir yang lebih besar." },
  { "en": "Seorang warga mengadu dan terlihat jelas melebih-lebihkan ceritanya. Bagaimana Anda menanggapinya?", "id": "Saya akan mendengarkan seluruh ceritanya dengan sabar dan empati, kemudian melakukan verifikasi fakta secara objektif untuk memisahkan antara kejadian sebenarnya dan opini atau perasaan pelapor." },
  { "en": "Apa makna 'Korps Bhayangkara' bagi Anda secara pribadi?", "id": "Bagi saya, itu adalah sebuah keluarga besar yang diikat oleh semangat pengabdian yang sama, di mana setiap anggota wajib menjaga kehormatan korps dan saling mendukung dalam kebaikan." },
  { "en": "Lengkapi analogi: PEMUPUKAN : SUBUR = VAKSINASI : ...", "id": "KEBAL." },
  { "en": "Lengkapi analogi: KERTAS : BUBUR KAYU = BENANG : ...", "id": "KAPAS." },
  { "en": "Lengkapi analogi: MIKROFON : SUARA = LENSA : ...", "id": "CAHAYA (atau GAMBAR)." },
  { "en": "Lengkapi analogi: DEKLAMASI : PUISI = AKTING : ...", "id": "DRAMA." },
  { "en": "Lengkapi analogi: ES : AIR = AIR : ...", "id": "UAP." },
  { "en": "Lengkapi analogi: KATA : KAMUS = TANGGAL : ...", "id": "KALENDER." },
  { "en": "Lengkapi analogi: EKONOMI : UANG = SOSIOLOGI : ...", "id": "MASYARAKAT." },
  { "en": "Lengkapi analogi: INTAN : KERAS = SPONS : ...", "id": "LUNAK (atau BERPORI)." },
  { "en": "Lengkapi analogi: OTORITAS : PERINTAH = PENGETAHUAN : ...", "id": "PENJELASAN." },
  { "en": "Lengkapi analogi: PELUKIS : STUDIO = ILMUWAN : ...", "id": "LABORATORIUM." },
  { "en": "Lanjutkan deret angka: 2, 4, 8, 14, 22, 32, ...", "id": "44 (pola: +2, +4, +6, +8, +10, +12)." },
  { "en": "Lanjutkan deret angka: 1, 3, 9, 27, 81, ...", "id": "243 (pola: dikali 3)." },
  { "en": "Lanjutkan deret huruf: A, B, D, E, G, H, J, ...", "id": "K (pola: dua huruf urut, loncat satu)." },
  { "en": "Lanjutkan deret angka: 120, 115, 108, 99, 88, ...", "id": "75 (pola: -5, -7, -9, -11, -13)." },
  { "en": "Lanjutkan deret angka: 3, 8, 18, 38, 78, ...", "id": "158 (pola: x2 + 2)." },
  { "en": "Lanjutkan deret angka: 1, 9, 25, 49, 81, ...", "id": "121 (pola: kuadrat dari bilangan ganjil: 1², 3², 5², 7², 9², 11²)." },
  { "en": "Lanjutkan deret huruf: A, C, E, G, I, K, ...", "id": "M (pola: deret huruf ganjil)." },
  { "en": "Lanjutkan deret angka: 2, 1, 4, 2, 6, 3, ...", "id": "8 (pola dua larik: 2,4,6,8,... dan 1,2,3,...)." },
  { "en": "Lanjutkan deret angka: 5, 5, 6, 7, 8, 10, ...", "id": "11 (pola: +0, +1, +1, +1, +2, +1... atau pola kurang jelas, kemungkinan 5,5,6,7,8,10,11,14)." },
  { "en": "Lanjutkan deret angka: 3, 2, 6, 5, 15, 14, ...", "id": "42 (pola: -1, x3, -1, x3, -1, x3)." },
  { "en": "Apa sinonim dari kata 'PROVOKASI'?", "id": "Pancingan atau hasutan." },
  { "en": "Apa sinonim dari kata 'KONSENSUS'?", "id": "Kesepakatan bersama." },
  { "en": "Apa sinonim dari kata 'ANOMALI'?", "id": "Keanehan atau penyimpangan." },
  { "en": "Apa sinonim dari kata 'URGENSI'?", "id": "Keadaan mendesak." },
  { "en": "Apa sinonim dari kata 'EVALUASI'?", "id": "Penilaian." },
  { "en": "Apa antonim dari kata 'KOOPERATIF'?", "id": "Individualistis atau tidak mau bekerja sama." },
  { "en": "Apa antonim dari kata 'RESIKO'?", "id": "Keamanan atau kepastian." },
  { "en": "Apa antonim dari kata 'TRANSPARAN'?", "id": "Tertutup." },
  { "en": "Apa antonim dari kata 'RASIONAL'?", "id": "Irasional." },
  { "en": "Apa antonim dari kata 'TOLERANSI'?", "id": "Intoleransi." },
  { "en": "Ada 4 orang (A, B, C, D). A bukan Dokter. B adalah Guru. C bukan Polisi atau Arsitek. D bukan Dokter. Siapa yang berprofesi sebagai Polisi?", "id": "D (C pasti dokter, karena bukan polisi/arsitek dan B sudah guru. A dan D bukan dokter, maka tersisa profesi polisi dan arsitek untuk A dan D. Cek lagi, C adalah Dokter, B adalah Guru, sisa A dan D untuk Polisi dan Arsitek. Soal ini kurang info, tapi jika harus memilih, butuh satu premis lagi)." },
  { "en": "Jika Tono lebih tua dari Siska, dan Ani lebih muda dari Tono. Siapa yang paling tua?", "id": "Tono." },
  { "en": "Beberapa tanaman hias beracun. Semua yang beracun berbahaya jika dimakan. Kesimpulannya adalah...", "id": "Beberapa tanaman hias berbahaya jika dimakan." },
  { "en": "Jika saya memakai topi, maka saya memakai sepatu. Saya tidak memakai sepatu. Kesimpulannya adalah...", "id": "Saya tidak memakai topi." },
  { "en": "Lima orang duduk di bangku panjang. A duduk di paling kiri. B duduk di antara C dan E. D duduk di sebelah C. Siapa yang duduk di tengah?", "id": "C." },
  { "en": "Sebuah kubus dicat merah di semua sisinya, lalu dipotong menjadi 27 kubus kecil. Berapa banyak kubus kecil yang tidak memiliki sisi berwarna merah sama sekali?", "id": "1 (kubus yang berada di paling tengah)." },
  { "en": "Berapakah nilai dari √144 + 5² ?", "id": "37 (12 + 25)." },
  { "en": "Sebuah acara seminar membutuhkan biaya Rp 5.000.000. Jika sponsor menanggung 45%, berapa yang harus dibayar dari penjualan tiket?", "id": "Rp 2.750.000 (55% dari 5 juta)." },
  { "en": "Ayah bisa membaca 3 halaman koran dalam 5 menit. Berapa halaman yang bisa ia baca dalam 20 menit?", "id": "12 halaman." },
  { "en": "Berapakah jumlah dari semua bilangan prima antara 10 dan 20?", "id": "60 (11 + 13 + 17 + 19)." },
  { "en": "Bagaimana Anda menyikapi seorang rekan yang selalu mengeluh tentang segala hal?", "id": "Saya akan mencoba mendengarkan keluhannya sesekali, tetapi tidak akan larut di dalamnya. Saya akan mencoba mengalihkan pembicaraan ke hal yang lebih positif atau solusi." },
  { "en": "Apa beda antara 'tahu aturan' dengan 'paham aturan'?", "id": "Tahu aturan hanya sebatas menghafal bunyi pasalnya. Paham aturan berarti mengerti maksud, tujuan, dan konteks dari aturan tersebut sehingga bisa menerapkannya dengan bijaksana." },
  { "en": "Bagaimana cara Anda menjaga stamina saat harus berjaga atau patroli semalaman?", "id": "Dengan memastikan istirahat cukup sebelumnya, mengonsumsi makanan berenergi, dan tetap aktif bergerak atau berinteraksi dengan rekan agar tidak mengantuk." },
  { "en": "Apa arti 'kebanggaan' menjadi seorang polisi bagi Anda?", "id": "Kebanggaan karena mendapat kesempatan dan kepercayaan dari negara untuk secara langsung melindungi masyarakat dan menegakkan keadilan, sebuah tugas yang mulia." },
  { "en": "Anda diberikan tugas yang sangat rutin dan membosankan menurut Anda. Bagaimana Anda tetap mengerjakannya dengan baik?", "id": "Saya akan mengingat bahwa setiap tugas, sekecil apapun, adalah bagian penting dari sebuah sistem yang lebih besar. Saya akan mengerjakannya dengan teliti karena itu adalah cerminan tanggung jawab saya." },
  { "en": "Bagaimana Anda merespons jika ada yang bertanya 'Berapa tarif untuk mengurus kasus ini?' kepada Anda?", "id": "Saya akan menjawab dengan tegas bahwa pelayanan Kepolisian tidak dipungut biaya dan semua proses berjalan sesuai aturan. Menawarkan uang kepada petugas adalah tindakan melanggar hukum." },
  { "en": "Menurut Anda, apa aset paling berharga yang dimiliki seorang anggota Polri?", "id": "Kepercayaan. Kepercayaan dari masyarakat, atasan, dan rekan adalah aset yang paling sulit dibangun dan paling mudah hancur, sehingga harus dijaga dengan integritas." },
  { "en": "Bagaimana Anda menanggapi permintaan 'damai di tempat' dari seorang pelanggar lalu lintas?", "id": "Saya akan menolaknya dengan tegas dan melanjutkan proses penilangan sesuai prosedur. 'Damai di tempat' adalah bentuk pungli dan melanggar hukum." },
  { "en": "Apa yang Anda lakukan jika merasa tidak sependapat dengan budaya kerja atau kebiasaan lama di unit Anda yang menurut Anda tidak efisien?", "id": "Saya tidak akan langsung menentang. Saya akan bekerja sesuai aturan yang benar terlebih dahulu untuk membangun kredibilitas, baru kemudian secara bertahap memberikan masukan atau contoh cara kerja yang lebih baik." },
  { "en": "Dalam situasi bahaya, apa yang Anda lakukan untuk mengendalikan rasa takut?", "id": "Saya akan fokus pada pelatihan yang sudah saya terima, menarik napas dalam untuk mengontrol adrenalin, dan berkonsentrasi pada tugas yang harus dilakukan, bukan pada rasa takut itu sendiri." },
  { "en": "Lengkapi analogi: DERMAWAN : HARTA = PANDAI : ...", "id": "ILMU." },
  { "en": "Lengkapi analogi: ULAR : MELATA = KANGURU : ...", "id": "MELOMPAT." },
  { "en": "Lengkapi analogi: AKSI : REAKSI = SEBAB : ...", "id": "AKIBAT." },
  { "en": "Lengkapi analogi: MAJALAH : ARTIKEL = ALBUM : ...", "id": "LAGU." },
  { "en": "Lengkapi analogi: RAMALAN : ASTROLOGI = PENYAKIT : ...", "id": "PATOLOGI." },
  { "en": "Lanjutkan deret angka: 4, 7, 11, 18, 29, 47, ...", "id": "76 (pola: deret Fibonacci modifikasi)." },
  { "en": "Lanjutkan deret angka: 1, 1.5, 2.5, 4, 6, ...", "id": "8.5 (pola: +0.5, +1, +1.5, +2, +2.5)." },
  { "en": "Lanjutkan deret huruf: B, D, G, K, P, ...", "id": "V (pola loncat: +2, +3, +4, +5, +6)." },
  { "en": "Lanjutkan deret angka: 2, 9, 3, 8, 4, 7, ...", "id": "5 (pola dua larik: 2,3,4,5,... dan 9,8,7,...)." },
  { "en": "Lanjutkan deret angka: 5, 10, 8, 16, 14, 28, ...", "id": "26 (pola: x2, -2, diulang)." },
  { "en": "Apa sinonim dari kata 'DOMINAN'?", "id": "Menguasai atau berpengaruh." },
  { "en": "Apa sinonim dari kata 'ISOLASI'?", "id": "Pengasingan." },
  { "en": "Apa sinonim dari kata 'ESTIMASI'?", "id": "Perkiraan." },
  { "en": "Apa antonim dari kata 'PENDAHULU'?", "id": "Penerus." },
  { "en": "Apa antonim dari kata 'ABSURD'?", "id": "Masuk akal atau logis." },
  { "en": "Jika 4 orang bersalaman, berapa banyak jabat tangan yang terjadi?", "id": "6 jabat tangan." },
  { "en": "Berapakah nilai rata-rata dari 4, 5, 6, 7, 8?", "id": "6." },
  { "en": "Sebuah sepeda motor menempuh jarak 15 km dengan 1 liter bensin. Jika di dalam tangki ada 3 liter bensin, berapa sisa jarak yang bisa ditempuh setelah berjalan 20 km?", "id": "25 km (Total jarak 45 km, sisa 45-20=25)." },
  { "en": "Berapakah 25% dari 2 jam?", "id": "30 menit." },
  { "en": "Jika semua A adalah C dan semua B adalah C, maka...", "id": "Tidak dapat disimpulkan hubungan antara A dan B." },
  { "en": "Bagaimana Anda memastikan bahwa tindakan Anda di lapangan selalu dapat dipertanggungjawabkan (akuntabel)?", "id": "Dengan selalu bertindak sesuai Prosedur Operasional Standar (SOP), membuat laporan yang akurat, dan jika memungkinkan, menggunakan body cam sebagai rekaman." },
  { "en": "Apa arti 'menjaga kehormatan' saat menggunakan media sosial?", "id": "Tidak mengunggah foto/video berseragam yang tidak pantas, tidak berdebat secara emosional, tidak memamerkan kemewahan, dan tidak mengomentari isu sensitif yang bukan kewenangan kita." },
  { "en": "Apa yang Anda lakukan jika diperintahkan oleh atasan untuk melakukan sesuatu yang bertentangan dengan hati nurani Anda, meskipun tidak melanggar hukum?", "id": "Saya akan meminta waktu untuk berdiskusi dengan atasan, menjelaskan perspektif dan kegelisahan saya dari sisi hati nurani, dan memohon kebijaksanaannya." },
  { "en": "Mana yang lebih Anda utamakan: loyalitas kepada atasan, loyalitas kepada rekan, atau loyalitas kepada institusi?", "id": "Loyalitas tertinggi adalah kepada institusi dan negara, yang diwujudkan dengan menaati aturan dan etika. Loyalitas kepada atasan dan rekan adalah bagian dari itu, selama sejalan dengan kepentingan institusi." },
  { "en": "Bagaimana Anda mengatasi rasa bosan terhadap pekerjaan yang sama selama bertahun-tahun?", "id": "Dengan mencari inovasi dalam cara bekerja, menjadi mentor bagi yang lebih muda, mengambil peran baru dalam proyek-proyek kecil, dan terus mengingat tujuan mulia dari pekerjaan tersebut." },
  { "en": "Jika sebuah buku selalu memiliki...", "id": "Halaman." },
  { "en": "Jika A lebih berat dari B, dan C lebih ringan dari B. Manakah yang paling ringan?", "id": "C." },
  { "en": "Lengkapi analogi: PENYIAR : RADIO = AKTOR : ...", "id": "PANGGUNG (atau FILM)." },
  { "en": "Lanjutkan deret: 1, 4, 8, 11, 22, 25, ...", "id": "50 (pola: +3, x2, +3, x2... salah, pola yg benar adalah 1,4 (+3), 4,8(x2), 8,11(+3), 11,22(x2). Jadi berikutnya +3 yaitu 25, lalu 25x2=50)." },
  { "en": "Apa sinonim dari 'KONSEPTUAL'?", "id": "Berkenaan dengan gagasan." },
  { "en": "Apa antonim dari kata 'BERGANTUNG'?", "id": "Mandiri." },
  { "en": "Bagaimana Anda menghadapi situasi di mana Anda harus memberi sanksi kepada rekan yang juga teman baik Anda?", "id": "Saya akan menjalankan prosedur secara profesional dan objektif, memisahkan hubungan pertemanan dengan tugas. Saya akan menjelaskan kepadanya bahwa ini adalah konsekuensi dari tindakannya." },
  { "en": "Berapakah 3/5 dari 60 menit?", "id": "36 menit." },
  { "en": "Bagaimana Anda melihat peran keluarga dalam mendukung karir Anda sebagai anggota Polri?", "id": "Keluarga adalah support system utama yang memberikan kekuatan moral, motivasi, dan menjadi tempat untuk kembali memulihkan energi setelah menghadapi tekanan pekerjaan." },
  { "en": "Menurut Anda, apakah polisi yang 'ideal' itu ada? Jelaskan.", "id": "Polisi yang sempurna mungkin tidak ada, tetapi polisi yang 'ideal' adalah dia yang setiap hari berusaha menjadi lebih baik: lebih profesional, lebih adil, lebih humanis, dan selalu belajar dari kesalahannya." },
  { "en": "Anda dihadapkan pada dua solusi: Solusi A cepat namun hanya menutupi masalah, Solusi B lebih lambat tapi menyelesaikan akar masalah. Mana yang Anda pilih?", "id": "Saya akan memilih Solusi B. Menyelesaikan akar masalah secara permanen lebih efektif untuk jangka panjang, meskipun membutuhkan waktu dan usaha lebih." },
  { "en": "Unit Anda diberikan anggaran yang sangat terbatas untuk sebuah program penting. Bagaimana Anda memaksimalkan sumber daya tersebut?", "id": "Saya akan membuat perencanaan yang sangat detail, memprioritaskan pengeluaran yang paling berdampak, dan mencari cara-cara kreatif seperti bekerja sama dengan pihak lain untuk menekan biaya." },
  { "en": "Sebuah video yang diedit menyebar luas, membuat tindakan rekan Anda terlihat brutal padahal konteksnya membela diri. Apa langkah cerdas Anda?", "id": "Saya tidak akan ikut berkomentar secara emosional di media sosial. Saya akan membantu dengan cara mengumpulkan bukti atau saksi untuk mendukung versi lengkap kejadian tersebut dan menyerahkannya kepada unit yang berwenang (Humas/Propam)." },
  { "en": "Anda tahu seorang informan kunci sesekali melakukan pelanggaran hukum ringan untuk bertahan hidup. Bagaimana Anda mengelola hubungan ini?", "id": "Saya akan tetap menjaga hubungan profesional untuk informasi, namun secara terpisah saya akan memberikan peringatan dan pembinaan kepadanya, serta tidak akan pernah melindungi pelanggaran hukum yang ia lakukan." },
  { "en": "Apa perbedaan antara 'efektif' dan 'efisien' dalam bekerja?", "id": "Efektif berarti melakukan pekerjaan yang benar untuk mencapai tujuan. Efisien berarti melakukan pekerjaan dengan cara yang benar, yaitu dengan sumber daya (waktu, tenaga, biaya) yang minimal." },
  { "en": "Anda melihat ada kesenjangan kompetensi antara anggota senior dan junior di unit Anda. Apa inisiatif yang bisa Anda tawarkan?", "id": "Saya akan mengusulkan program mentoring atau 'knowledge sharing session' di mana senior bisa berbagi pengalaman dan junior bisa berbagi pengetahuan tentang teknologi atau metode baru." },
  { "en": "Bagaimana Anda menanggapi permintaan data dari media massa mengenai kasus yang sedang berjalan?", "id": "Saya akan mengarahkannya kepada pejabat Humas atau atasan yang berwenang, karena ada prosedur dan batasan mengenai informasi apa saja yang boleh dipublikasikan saat proses investigasi masih berjalan." },
  { "en": "Menurut Anda, apa risiko terbesar dari sikap 'solidaritas korps yang membabi buta'?", "id": "Risiko terbesarnya adalah menutupi kesalahan dan pelanggaran, yang pada akhirnya akan merusak kepercayaan masyarakat dan meruntuhkan profesionalisme serta integritas institusi itu sendiri." },
  { "en": "Seorang warga yang Anda kenal baik meminta 'bantuan' untuk mempercepat proses pembuatan SKCK. Bagaimana Anda meresponsnya?", "id": "Saya akan menjelaskan dengan ramah bahwa semua proses harus sesuai antrian dan prosedur untuk menjamin keadilan bagi semua pemohon, dan saya tidak bisa memberikan perlakuan khusus." },
  { "en": "Apa arti dari semboyan 'Rastra Sewakottama' (Abdi Utama dari pada Nusa dan Bangsa)?", "id": "Artinya Polri adalah pelayan utama bagi nusa dan bangsa, yang menempatkan kepentingan masyarakat dan negara di atas kepentingan pribadi atau golongan." },
  { "en": "Lengkapi analogi: MERPATI : PERDAMAIAN = MAHKOTA : ...", "id": "KEKUASAAN (atau KERAJAAN)." },
  { "en": "Lengkapi analogi: GELANGGANG : TINJU = ARENA : ...", "id": "GLADIATOR." },
  { "en": "Lengkapi analogi: BENDERA : NEGARA = LAMBANG : ...", "id": "ORGANISASI (atau PERKUMPULAN)." },
  { "en": "Lengkapi analogi: Kuman : Penyakit = Api : ...", "id": "Kebakaran." },
  { "en": "Lengkapi analogi: FIKTIF : KENYATAAN = DONGENG : ...", "id": "SEJARAH." },
  { "en": "Lengkapi analogi: ANTISEPTIK : KUMAN = PESTISIDA : ...", "id": "HAMA." },
  { "en": "Lengkapi analogi: GAMBAR : DINDING = PATUNG : ...", "id": "TAMAN (atau RUANGAN)." },
  { "en": "Lengkapi analogi: TERSEMBUNYI : RAHASIA = TERLIHAT : ...", "id": "JELAS." },
  { "en": "Lengkapi analogi: KATA : ARTI = SIMBOL : ...", "id": "MAKNA." },
  { "en": "Lengkapi analogi: JALAN TOL : HAMBATAN = INTERNET : ...", "id": "JARAK (atau WAKTU)." },
  { "en": "Lanjutkan deret angka: 1, 8, 9, 64, 25, 216, ...", "id": "49 (pola: bilangan kuadrat dan kubik berselang-seling: 1², 2³, 3², 4³, 5², 6³, 7²)." },
  { "en": "Lanjutkan deret angka: 2, 4, 12, 48, 240, ...", "id": "1440 (pola: x2, x3, x4, x5, x6)." },
  { "en": "Lanjutkan deret huruf: A, C, D, F, G, I, J, ...", "id": "L (pola: loncat 1, urut, loncat 1, urut, dst)." },
  { "en": "Lanjutkan deret angka: 2, 3, 8, 6, 14, 9, ...", "id": "20 (pola dua larik: 2,8,14,20 [+6] dan 3,6,9 [+3])." },
  { "en": "Lanjutkan deret angka: 94, 88, 82, 76, 70, ...", "id": "64 (pola: dikurang 6)." },
  { "en": "Lanjutkan deret angka: 1, 1, 2, 4, 8, 16, ...", "id": "32 (pola: x2, tapi dimulai dari 1,1,2... ini adalah deret yg kurang jelas. Jika dimulai dari 1,2,4,8,... maka jawabannya 16. Jika 1+1=2, 1+1+2=4,... dst maka 1+1+2+4+8=16)." },
  { "en": "Lanjutkan deret huruf: A, B, E, J, Q, ...", "id": "B (Pola huruf ke-1, 2, 5, 10, 17, 28 (pola +1,+3,+5,+7,+11), huruf ke-28 adalah B)." },
  { "en": "Lanjutkan deret angka: 5, 8, 16, 19, 38, 41, ...", "id": "82 (pola: +3, x2, diulang)." },
  { "en": "Lanjutkan deret angka: 1, 1, 1, 3, 5, 9, ...", "id": "17 (pola: jumlah 3 angka sebelumnya)." },
  { "en": "Lanjutkan deret angka: 3, 5, 6, 10, 9, 15, ...", "id": "12 (pola dua larik: 3,6,9,12 [+3] dan 5,10,15 [+5])." },
  { "en": "Apa sinonim dari kata 'FLUKTUATIF'?", "id": "Berubah-ubah atau tidak tetap." },
  { "en": "Apa sinonim dari kata 'NORMA'?", "id": "Kaidah atau aturan." },
  { "en": "Apa sinonim dari kata 'RATIFIKASI'?", "id": "Pengesahan perjanjian." },
  { "en": "Apa sinonim dari kata 'SPESIFIK'?", "id": "Khusus." },
  { "en": "Apa sinonim dari kata 'RESIDU'?", "id": "Sisa atau endapan." },
  { "en": "Apa antonim dari kata 'ACAK'?", "id": "Teratur atau sistematis." },
  { "en": "Apa antonim dari kata 'RAWAN'?", "id": "Aman." },
  { "en": "Apa antonim dari kata 'OTODIDAK'?", "id": "Terpelajar (dididik orang lain)." },
  { "en": "Apa antonim dari kata 'DIALOG'?", "id": "Monolog." },
  { "en": "Apa antonim dari kata 'GENERIK'?", "id": "Spesifik atau khusus." },
  { "en": "Rumah A di timur rumah B. Rumah C di utara rumah A. Rumah D di barat rumah C. Posisi rumah D relatif terhadap rumah B adalah...", "id": "Timur Laut." },
  { "en": "Semua perwira pernah menjadi bintara. Beberapa polisi adalah perwira. Kesimpulannya adalah...", "id": "Beberapa polisi pernah menjadi bintara." },
  { "en": "Jika Tio rajin, maka Tio disukai guru. Tio tidak disukai guru. Kesimpulannya adalah...", "id": "Tio tidak rajin." },
  { "en": "Adi bukan kakak Rina. Rina adalah adik dari Soni. Soni adalah kakak dari Adi. Siapakah anak tengah?", "id": "Soni." },
  { "en": "Setiap peserta rapat memakai batik. Beberapa yang memakai batik adalah pejabat. Kesimpulannya adalah...", "id": "Beberapa peserta rapat adalah pejabat." },
  { "en": "Dalam sebuah garasi ada 15 kendaraan terdiri dari mobil (roda 4) dan motor (roda 2). Jika jumlah roda seluruhnya ada 40, berapa jumlah mobil?", "id": "5 mobil (dan 10 motor)." },
  { "en": "Berapakah 2.5 jam dikurangi 90 menit?", "id": "60 menit atau 1 jam." },
  { "en": "Sebuah tiang tingginya 12 meter. Jika pada suatu waktu bayangannya 4 meter, berapa tinggi tiang yang bayangannya 6 meter pada saat yang sama?", "id": "18 meter." },
  { "en": "Berapakah nilai dari 20% dari 5% dari 1.000.000?", "id": "1.000." },
  { "en": "Jika sebuah pekerjaan dapat diselesaikan oleh 8 orang dalam 6 hari, berapa orang yang harus ditambahkan agar pekerjaan selesai dalam 4 hari?", "id": "4 orang (total pekerja menjadi 12 orang)." },
  { "en": "Bagaimana Anda membedakan antara 'ambisi' yang sehat dan 'ambisi' yang merusak?", "id": "Ambisi yang sehat fokus pada pengembangan diri dan kontribusi, serta dicapai dengan cara yang etis. Ambisi yang merusak menghalalkan segala cara dan merugikan orang lain." },
  { "en": "Anda melihat ada sistem 'senioritas' yang tidak sehat di unit Anda. Apa yang Anda lakukan sebagai junior?", "id": "Saya akan tetap menunjukkan rasa hormat kepada senior, namun fokus menjalankan tugas sesuai aturan profesional. Saya tidak akan ikut dalam budaya yang tidak sehat dan berharap kinerja saya bisa menjadi contoh." },
  { "en": "Apa pentingnya membuat Laporan Harta Kekayaan Pejabat Negara (LHKPN) secara jujur?", "id": "Itu adalah bentuk transparansi dan akuntabilitas kepada publik, serta sebagai mekanisme untuk mencegah terjadinya tindak pidana korupsi." },
  { "en": "Bagaimana Anda menjelaskan konsep 'pelayanan prima' dalam konteks kepolisian?", "id": "Melayani dengan cepat, tepat, ramah, transparan, tidak diskriminatif, dan memberikan solusi yang terbaik bagi masyarakat sesuai dengan aturan." },
  { "en": "Anda lelah setelah bertugas, lalu ada tetangga yang meminta bantuan untuk urusan non-darurat. Apa respons Anda?", "id": "Saya akan tetap merespon dengan baik, jika memungkinkan saya bantu sebentar atau saya akan berjanji untuk membantunya setelah saya beristirahat sejenak." },
  { "en": "Apa yang akan Anda lakukan untuk membangun 'jejaring' (networking) yang positif di lingkungan Polri?", "id": "Dengan bersikap aktif dalam kegiatan-kegiatan dinas, menunjukkan sikap yang kolaboratif, mudah membantu, dan menjaga komunikasi yang baik dengan rekan dari berbagai unit." },
  { "en": "Menurut Anda, apa dampak terbesar dari seorang polisi yang menunjukkan gaya hidup mewah di media sosial?", "id": "Dapat menimbulkan persepsi negatif dan kecurigaan dari masyarakat mengenai sumber kekayaannya, serta merusak citra kesederhanaan dan pengabdian institusi." },
  { "en": "Jika Anda harus memilih antara dihargai karena jabatan Anda atau karena kepribadian Anda, mana yang Anda pilih?", "id": "Dihargai karena kepribadian saya. Karena rasa hormat yang tulus datang dari karakter, bukan dari pangkat yang sifatnya sementara." },
  { "en": "Apa arti 'objektivitas' dalam melakukan penyidikan?", "id": "Artinya penyidikan didasarkan murni pada bukti-bukti dan fakta (alat bukti yang sah), bukan berdasarkan asumsi, opini pribadi, atau tekanan dari pihak manapun." },
  { "en": "Bagaimana Anda menanggapi kebijakan baru dari pimpinan yang tidak populer di kalangan rekan-rekan Anda?", "id": "Saya akan mencoba memahami alasan dan tujuan baik di balik kebijakan tersebut dan menjelaskannya kepada rekan-rekan, sambil tetap menjalankan kebijakan itu sebagai kewajiban." },
  { "en": "Lengkapi analogi: LOKOMOTIF : KERETA = MESIN : ...", "id": "MOBIL." },
  { "en": "Lengkapi analogi: SARUNG TANGAN : PETINJU = RAKET : ...", "id": "PEMAIN TENIS (atau BULU TANGKIS)." },
  { "en": "Lengkapi analogi: AIR : MENGALIR = ANGIN : ...", "id": "BERHEMBUS." },
  { "en": "Lengkapi analogi: OMBAK : PANTAI = SALJU : ...", "id": "GUNUNG." },
  { "en": "Lengkapi analogi: GIGI : MENGUNYAH = LAMBUNG : ...", "id": "MENCERNA." },
  { "en": "Lanjutkan deret angka: 2, 2, 3, 4, 5, 7, 8, ...", "id": "11 (pola dua larik: 2,3,5,8,11 [+1,+2,+3] dan 2,4,7,...)." },
  { "en": "Lanjutkan deret angka: 100, 50, 200, 25, 400, ...", "id": "12.5 (pola dua larik: 100,200,400 [x2] dan 50,25,12.5 [:2])." },
  { "en": "Lanjutkan deret huruf: A, D, E, H, I, L, M, ...", "id": "P (pola: +3, +1, +3, +1, +3, +1)." },
  { "en": "Lanjutkan deret angka: 1, 1, 3, 2, 5, 3, 7, ...", "id": "4 (pola dua larik: 1,3,5,7,... dan 1,2,3,4,...)." },
  { "en": "Lanjutkan deret angka: 3, 7, 15, 31, 63, ...", "id": "127 (pola: x2 + 1)." },
  { "en": "Apa sinonim dari kata 'IMUNITAS'?", "id": "Kekebalan." },
  { "en": "Apa sinonim dari kata 'KOLABORASI'?", "id": "Kerja sama." },
  { "en": "Apa sinonim dari kata 'ABSURD'?", "id": "Tidak masuk akal." },
  { "en": "Apa antonim dari kata 'PENTING'?", "id": "Remeh atau sepele." },
  { "en": "Apa antonim dari kata 'HADIR'?", "id": "Absen atau mangkir." },
  { "en": "Jika 4 = 16, 5 = 25, maka 6 = ...", "id": "36 (Pola: kuadrat)." },
  { "en": "Berapakah 15% dari 2/3?", "id": "0.1 atau 1/10." },
  { "en": "Kecepatan sebuah motor adalah 72 km/jam. Berapa meter per detik kecepatan tersebut?", "id": "20 meter/detik." },
  { "en": "Jika 2x - 5 = x + 3, maka nilai x adalah...", "id": "8." },
  { "en": "Berapa banyak angka 9 yang muncul dari 1 sampai 100?", "id": "20 (9, 19, 29, 39, 49, 59, 69, 79, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99)." },
  { "en": "Apa yang akan Anda lakukan jika melihat rekan Anda mulai menunjukkan tanda-tanda stres berat atau depresi?", "id": "Saya akan mengajaknya bicara secara pribadi untuk menunjukkan kepedulian, mendengarkan masalahnya tanpa menghakimi, dan menyarankannya untuk menemui konselor atau psikolog dinas." },
  { "en": "Mengapa polisi dilarang berpolitik praktis?", "id": "Untuk menjaga netralitas dan profesionalisme, memastikan bahwa tindakan penegakan hukum tidak dipengaruhi oleh kepentingan politik, dan untuk menjaga kepercayaan seluruh lapisan masyarakat." },
  { "en": "Bagaimana Anda menjaga motivasi saat karir Anda terasa mandek atau tidak ada kemajuan?", "id": "Saya akan fokus pada pengembangan kompetensi diri, mencari kepuasan dari kualitas pekerjaan itu sendiri, dan percaya bahwa kesempatan akan datang bagi mereka yang terus mempersiapkan diri." },
  { "en": "Bagaimana Anda menyampaikan perintah kepada anggota yang usianya jauh lebih tua dari Anda?", "id": "Dengan menggunakan bahasa yang tetap sopan dan menghargai senioritasnya, fokus pada 'apa' tugasnya, bukan dengan nada memerintah secara kaku." },
  { "en": "Menurut Anda, apa tiga karakter utama yang paling vital bagi seorang anggota Polri?", "id": "Integritas (jujur dan dapat dipercaya), Profesionalisme (kompeten dan taat aturan), dan Humanis (mampu melayani dengan hati)." },
  { "en": "Jika semua A bukan B, dan semua B adalah C. Kesimpulannya adalah...", "id": "Tidak ada kesimpulan pasti antara A dan C." },
  { "en": "Jika dibutuhkan 1 jam untuk merebus 1 telur, berapa waktu yang dibutuhkan untuk merebus 5 telur sekaligus dalam panci yang sama?", "id": "1 jam." },
  { "en": "Lengkapi analogi: ATAP : RUMAH = TUTUP : ...", "id": "BOTOL." },
  { "en": "Lanjutkan deret: 1, 2, 5, 14, 41, ...", "id": "122 (pola: x3 - 1)." },
  { "en": "Apa sinonim dari 'KONTINUITAS'?", "id": "Keberlanjutan." },
  { "en": "Apa antonim dari 'PERMANEN'?", "id": "Sementara." },
  { "en": "Anda mengetahui ada informasi penting untuk sebuah kasus, tetapi informasi itu didapat dari cara yang tidak prosedural. Apa yang Anda lakukan?", "id": "Saya akan menjadikan informasi itu sebagai petunjuk awal (intelijen) untuk mencari bukti-bukti lain melalui cara yang sah dan prosedural. Informasi itu sendiri tidak bisa dijadikan alat bukti." },
  { "en": "Berapakah 5³ - 10²?", "id": "25 (125 - 100)." },
  { "en": "Bagaimana Anda melihat peran media dalam membentuk citra Kepolisian?", "id": "Media memiliki peran yang sangat strategis. Oleh karena itu, penting bagi Polri untuk proaktif memberikan informasi yang akurat dan membangun hubungan yang baik dengan media untuk membentuk citra yang positif dan transparan." },
  { "en": "Menurut Anda, hukuman apa yang paling berat bagi seorang anggota Polri yang melakukan pelanggaran?", "id": "Pemberhentian Tidak Dengan Hormat (PTDH), karena itu bukan hanya kehilangan pekerjaan, tetapi juga kehilangan kehormatan dan pengabdian yang telah dibangun." },
  { "en": "Anda sedang memimpin sebuah tim, dan salah satu anggota yang paling Anda andalkan tiba-tiba menunjukkan penurunan kinerja drastis. Apa langkah pertama Anda?", "id": "Saya akan mengajaknya bicara secara empat mata untuk mencari tahu apakah ada masalah pribadi atau pekerjaan yang membebaninya, dan menawarkan dukungan sebagai pemimpin." },
  { "en": "Bagaimana Anda menyikapi perintah yang menurut Anda kurang efisien, namun harus segera dilaksanakan?", "id": "Saya akan melaksanakannya terlebih dahulu sesuai perintah untuk memenuhi urgensi. Setelah situasi terkendali, saya akan menyusun analisis dan mengusulkan cara yang lebih efisien untuk tugas serupa di masa depan." },
  { "en": "Anda menemukan bahwa sebuah sistem digital baru yang diterapkan di unit Anda memiliki celah keamanan. Apa tindakan prioritas Anda?", "id": "Saya akan segera melaporkan temuan tersebut secara tertulis dan rahasia kepada atasan dan unit IT, beserta potensi risikonya, dan menyarankan peninjauan ulang sebelum digunakan secara luas." },
  { "en": "Seorang warga yang melapor kasusnya kepada Anda terus-menerus menelepon untuk menanyakan perkembangan. Bagaimana Anda mengelolanya?", "id": "Saya akan memberikan pemahaman bahwa setiap kasus butuh proses, memberikan update secara berkala pada waktu yang wajar, dan meyakinkannya bahwa kasusnya sedang ditangani dengan baik." },
  { "en": "Apa perbedaan antara 'bertanggung jawab' dan 'disalahkan'?", "id": "Bertanggung jawab adalah sikap proaktif mengakui peran dan kewajiban atas suatu hasil, baik atau buruk. Disalahkan adalah posisi pasif menerima tuduhan atas suatu kegagalan." },
  { "en": "Anda melihat seorang rekan menggunakan bahasa yang kurang sopan kepada masyarakat saat bertugas. Bagaimana Anda menyikapinya?", "id": "Saya akan mencari momen yang tepat untuk mengingatkannya secara pribadi bahwa sebagai polisi, kita harus selalu menjaga tutur kata dan sikap yang humanis." },
  { "en": "Bagaimana Anda mendefinisikan 'kesuksesan' dalam karir Anda sebagai anggota Polri?", "id": "Kesuksesan bagi saya adalah saat saya bisa memberikan dampak positif, sekecil apapun, pada keamanan dan kenyamanan masyarakat di lingkungan saya bertugas, serta pensiun dengan nama baik." },
  { "en": "Anda merasa ada ketidakcocokan gaya kerja dengan atasan langsung Anda. Apa yang Anda lakukan untuk menjaga hubungan kerja tetap profesional?", "id": "Saya akan fokus untuk memahami ekspektasi atasan dan menyesuaikan cara komunikasi saya, sambil tetap memberikan hasil kerja terbaik sesuai standar profesional." },
  { "en": "Apa pentingnya 'debriefing' atau evaluasi singkat setelah selesai melaksanakan sebuah operasi atau tugas besar?", "id": "Sangat penting untuk mengidentifikasi apa yang berjalan baik dan apa yang kurang, berbagi pembelajaran, serta melepaskan ketegangan emosional agar tim siap untuk tugas berikutnya." },
  { "en": "Bagaimana Anda menyeimbangkan antara menegakkan aturan secara tegas dengan mempertimbangkan kearifan lokal?", "id": "Penegakan hukum harus tetap berjalan, namun pendekatannya bisa disesuaikan dengan mempertimbangkan nilai-nilai sosial dan kearifan lokal, selama tidak bertentangan dengan prinsip hukum itu sendiri." },
  { "en": "Lengkapi analogi: BESI : MEMUAI = KAYU : ...", "id": "LAPUK." },
  { "en": "Lengkapi analogi: BAIT : PENYAIR = SKETSA : ...", "id": "PELUKIS." },
  { "en": "Lengkapi analogi: KERING : LEMBAB = TERANG : ...", "id": "REDU." },
  { "en": "Lengkapi analogi: METAMORFOSIS : BIOLOGI = GRAVITASI : ...", "id": "FISIKA." },
  { "en": "Lengkapi analogi: KEPALA : HELM = JARI : ...", "id": "CINCIN." },
  { "en": "Lengkapi analogi: KAMPUS : REKTOR = PROVINSI : ...", "id": "GUBERNUR." },
  { "en": "Lengkapi analogi: SUKSES : USAHA = PANDAI : ...", "id": "BELAJAR." },
  { "en": "Lengkapi analogi: BUNGLON : MIMIKRI = CICAK : ...", "id": "AUTOTOMI." },
  { "en": "Lengkapi analogi: KAKI : BERJALAN = MULUT : ...", "id": "BERBICARA." },
  { "en": "Lengkapi analogi: NASABAH : BANK = PASIEN : ...", "id": "RUMAH SAKIT." },
  { "en": "Lanjutkan deret angka: 1, 2, 4, 7, 11, 16, ...", "id": "22 (pola: +1, +2, +3, +4, +5, +6)." },
  { "en": "Lanjutkan deret angka: 3, 9, 27, 81, ...", "id": "243 (pola: dikali 3)." },
  { "en": "Lanjutkan deret huruf: A, E, D, H, G, K, J, ...", "id": "N (pola dua larik: A,D,G,J [+3] dan E,H,K,N [+3])." },
  { "en": "Lanjutkan deret angka: 5, 7, 11, 19, 35, ...", "id": "67 (pola: x2 - 3)." },
  { "en": "Lanjutkan deret angka: 2, 8, 3, 12, 4, 16, ...", "id": "5 (pola dua larik: 2,3,4,5,... dan 8,12,16,... [+4])." },
  { "en": "Lanjutkan deret angka: 100, 95, 85, 70, 50, ...", "id": "25 (pola: -5, -10, -15, -20, -25)." },
  { "en": "Lanjutkan deret huruf: B, F, J, N, R, V, ...", "id": "Z (pola: loncat 3 huruf)." },
  { "en": "Lanjutkan deret angka: 1, 2, 3, 5, 8, 13, ...", "id": "21 (pola: deret Fibonacci)." },
  { "en": "Lanjutkan deret angka: 1, 4, 5, 9, 14, 23, ...", "id": "37 (pola: deret Fibonacci modifikasi)." },
  { "en": "Lanjutkan deret angka: 10, 11, 13, 16, 20, 25, ...", "id": "31 (pola: +1, +2, +3, +4, +5, +6)." },
  { "en": "Apa sinonim dari kata 'SKEPTIS'?", "id": "Ragu-ragu." },
  { "en": "Apa sinonim dari kata 'KOMPREHENSIF'?", "id": "Menyeluruh atau luas." },
  { "en": "Apa sinonim dari kata 'IMPLIKASI'?", "id": "Dampak atau akibat tak langsung." },
  { "en": "Apa sinonim dari kata 'RETORIKA'?", "id": "Seni berbicara." },
  { "en": "Apa sinonim dari kata 'HIPOTESIS'?", "id": "Dugaan awal." },
  { "en": "Apa antonim dari kata 'KONSTAN'?", "id": "Berubah-ubah." },
  { "en": "Apa antonim dari kata 'INTERNAL'?", "id": "Eksternal." },
  { "en": "Apa antonim dari kata 'OPTIMAL'?", "id": "Minimal." },
  { "en": "Apa antonim dari kata 'SERAGAM'?", "id": "Beraneka ragam." },
  { "en": "Apa antonim dari kata 'VITAL'?", "id": "Tidak penting atau sepele." },
  { "en": "Jika semua X adalah Y, dan beberapa Y adalah Z, kesimpulan yang paling tepat adalah...", "id": "Beberapa Z mungkin adalah X (tidak ada kesimpulan pasti)." },
  { "en": "Jika jam dinding menunjukkan pukul 03:30, berapa besar sudut terkecil yang dibentuk oleh kedua jarum jam?", "id": "75 derajat." },
  { "en": "Semua dokter memakai jas putih. Ayah saya tidak memakai jas putih. Kesimpulannya adalah...", "id": "Ayah saya bukan seorang dokter." },
  { "en": "Di sebuah antrian, saya berada di urutan ke-12 dari depan dan ke-15 dari belakang. Berapa total orang dalam antrian tersebut?", "id": "26 orang (12 + 15 - 1)." },
  { "en": "Jika hari ini adalah hari Kamis, hari apakah 100 hari dari sekarang?", "id": "Sabtu (100 dibagi 7 sisa 2, jadi Kamis + 2 hari)." },
  { "en": "Berapakah 120% dari 75?", "id": "90." },
  { "en": "Sebuah proyek dijadwalkan selesai dalam 30 hari oleh 15 pekerja. Setelah 6 hari bekerja, proyek dihentikan selama 4 hari. Berapa pekerja tambahan yang dibutuhkan agar proyek selesai tepat waktu?", "id": "5 pekerja." },
  { "en": "Berapakah nilai dari 2⁻³?", "id": "1/8." },
  { "en": "Sebuah roda memiliki diameter 14 cm. Berapa jarak yang ditempuh jika roda berputar sebanyak 50 kali? (π=22/7)", "id": "2200 cm atau 22 meter." },
  { "en": "Berapakah rata-rata dari bilangan kuadrat pertama dari 1 sampai 5?", "id": "11 ( (1+4+9+16+25)/5 )." },
  { "en": "Apa yang Anda lakukan jika menerima informasi intelijen yang berpotensi membahayakan nyawa Anda saat bertugas?", "id": "Saya akan memverifikasi informasi tersebut secepat mungkin, melaporkannya kepada atasan, dan menyesuaikan taktik di lapangan dengan meningkatkan kewaspadaan dan tidak bertindak sendirian." },
  { "en": "Bagaimana Anda menjaga 'kewarasan' atau kesehatan mental di tengah paparan terhadap sisi gelap kehidupan manusia (kejahatan, kekerasan)?", "id": "Dengan membangun batasan emosional (tidak membawa pekerjaan ke rumah), memiliki support system yang kuat, melakukan hobi positif, dan tidak ragu mencari bantuan profesional jika diperlukan." },
  { "en": "Apa arti 'akuntabilitas' bagi Anda?", "id": "Akuntabilitas berarti setiap tindakan dan keputusan yang saya ambil dapat saya pertanggungjawabkan, baik secara hukum, secara dinas kepada atasan, maupun secara moral kepada masyarakat." },
  { "en": "Anda melihat seorang anak muda yang memiliki potensi tapi sering terlibat dalam kenakalan ringan. Bagaimana pendekatan Anda?", "id": "Saya akan melakukan pendekatan personal sebagai pembina, bukan penghukum. Saya akan mencoba mengarahkan energinya ke kegiatan positif dan menunjukkan kepadanya potensi yang ia miliki." },
  { "en": "Bagaimana Anda menanggapi tuduhan 'Polisi hanya tajam ke bawah, tumpul ke atas'?", "id": "Saya akan menjawab bahwa Polri berkomitmen untuk menegakkan hukum secara adil kepada siapa saja, dan saya pribadi akan membuktikannya dengan bekerja secara profesional dan tidak diskriminatif dalam setiap kasus yang saya tangani." },
  { "en": "Apa tiga hal yang tidak akan pernah Anda kompromikan sebagai anggota Polri?", "id": "Kejujuran (integritas), Ketaatan pada hukum, dan Keselamatan masyarakat." },
  { "en": "Bagaimana Anda membedakan antara 'informasi' dan 'pengetahuan'?", "id": "Informasi adalah sekumpulan data yang telah diolah. Pengetahuan adalah pemahaman mendalam yang didapat dari informasi tersebut, yang memungkinkan kita untuk mengambil keputusan atau tindakan." },
  { "en": "Anda diminta untuk memilih anggota untuk tim khusus. Apa kriteria utama yang akan Anda gunakan?", "id": "Saya akan memilih berdasarkan tiga kriteria utama: Kompetensi (keahlian yang relevan), Integritas (dapat dipercaya), dan Kemampuan bekerja sama (team player)." },
  { "en": "Bagaimana Anda menghadapi situasi di mana Anda harus mengakui kesalahan institusi kepada publik?", "id": "Harus dilakukan dengan transparan dan bertanggung jawab melalui juru bicara resmi, mengakui kesalahan yang terjadi, menjelaskan langkah perbaikan, dan menunjukkan komitmen untuk tidak mengulanginya." },
  { "en": "Menurut Anda, apa tantangan terbesar menjadi polisi di masa depan?", "id": "Beradaptasi dengan kejahatan berbasis teknologi yang semakin canggih, mengelola informasi di era digital, dan terus menjaga kepercayaan publik di tengah masyarakat yang semakin kritis." },
  { "en": "Lengkapi analogi: BENUA : NEGARA = GALAKSI : ...", "id": "TATA SURYA (atau BINTANG)." },
  { "en": "Lengkapi analogi: PENSIL : TULISAN = KAMERA : ...", "id": "FOTO." },
  { "en": "Lengkapi analogi: KERING : JEMUR = BASAH : ...", "id": "CELUP (atau SIRAM)." },
  { "en": "Lengkapi analogi: PENGADILAN : HAKIM = RUMAH SAKIT : ...", "id": "DOKTER." },
  { "en": "Lengkapi analogi: IKAN : INSANG = MANUSIA : ...", "id": "PARU-PARU." },
  { "en": "Lanjutkan deret angka: 2, 4, 7, 12, 19, 30, ...", "id": "43 (pola: +2, +3, +5, +7, +11, +13 -> bilangan prima)." },
  { "en": "Lanjutkan deret angka: 8, 12, 11, 15, 14, 18, ...", "id": "17 (pola dua larik: 8,11,14,17 [+3] dan 12,15,18 [+3])." },
  { "en": "Lanjutkan deret huruf: A, C, F, H, K, M, P, ...", "id": "R (pola: +2, +3, +2, +3, +2, +3)." },
  { "en": "Lanjutkan deret angka: 1, 5, 25, 125, ...", "id": "625 (pola: dikali 5)." },
  { "en": "Lanjutkan deret angka: 3, 6, 12, 21, 33, 48, ...", "id": "66 (pola: +3, +6, +9, +12, +15, +18)." },
  { "en": "Apa sinonim dari kata 'PROSEDUR'?", "id": "Tata cara atau mekanisme." },
  { "en": "Apa sinonim dari kata 'KLAUSUL'?", "id": "Pasal atau ketentuan." },
  { "en": "Apa sinonim dari kata 'SIMPATISAN'?", "id": "Pendukung." },
  { "en": "Apa antonim dari kata 'MAJU'?", "id": "Mundur." },
  { "en": "Apa antonim dari kata 'ASLI'?", "id": "Palsu atau tiruan." },
  { "en": "Jika 5A = 3B dan 2B = 4C, maka perbandingan A:C adalah...", "id": "3:5." },
  { "en": "Berapakah 1/4 dari 80%?", "id": "20% atau 0.2." },
  { "en": "Sebuah kotak berisi 12 bola, terdiri dari 5 merah, 4 biru, dan 3 hijau. Berapa peluang terambilnya bola bukan biru?", "id": "8/12 atau 2/3." },
  { "en": "Jika sisi sebuah kubus diperpanjang 2 kali lipat, berapa kali lipat volumenya akan bertambah?", "id": "8 kali lipat." },
  { "en": "Berapakah FPB (Faktor Persekutuan Terbesar) dari 48 dan 60?", "id": "12." },
  { "en": "Bagaimana Anda membedakan antara 'perintah' dan 'arahan'?", "id": "Perintah bersifat wajib dan harus dilaksanakan (bersifat direktif). Arahan lebih bersifat petunjuk atau panduan yang memberikan ruang untuk penyesuaian dalam pelaksanaannya." },
  { "en": "Apa yang Anda lakukan jika Anda merasa keputusan Anda di masa lalu ternyata keliru?", "id": "Saya akan mengakuinya sebagai kekeliruan, segera mengambil langkah untuk memperbaikinya jika memungkinkan, dan menjadikannya pelajaran berharga agar tidak terulang." },
  { "en": "Mengapa 'penampilan' (kerapian seragam, sikap) penting bagi seorang polisi?", "id": "Karena penampilan adalah bentuk komunikasi non-verbal pertama kepada masyarakat. Penampilan yang rapi dan sikap yang baik membangun kesan disiplin, profesional, dan dapat dipercaya." },
  { "en": "Bagaimana Anda menanggapi situasi di mana Anda harus bekerja di bawah pimpinan yang lebih muda dan kurang berpengalaman?", "id": "Saya akan tetap menghormati posisinya sebagai pimpinan, mendukung keputusannya, dan secara proaktif memberikan masukan berdasarkan pengalaman saya dengan cara yang sopan dan konstruktif." },
  { "en": "Menurut Anda, apa hubungan antara tingkat kesejahteraan anggota Polri dengan tingkat profesionalismenya?", "id": "Kesejahteraan yang cukup dapat menjadi faktor pendukung yang mengurangi godaan untuk melakukan penyimpangan, sehingga anggota bisa lebih fokus untuk bekerja secara profesional. Namun, profesionalisme sejati bersumber dari integritas diri." },
  { "en": "Jika sebuah mobil selalu memiliki...", "id": "Mesin (atau Roda)." },
  { "en": "Semua mamalia adalah hewan. Tidak ada hewan yang tumbuhan. Kesimpulannya adalah...", "id": "Tidak ada mamalia yang merupakan tumbuhan." },
  { "en": "Lengkapi analogi: GELOMBANG : SUARA = SPEKTRUM : ...", "id": "WARNA." },
  { "en": "Lanjutkan deret: 1, 10, 100, 2, 20, 200, 3, ...", "id": "30 (pola: n, nx10, nx100, diulang untuk n=1,2,3...)." },
  { "en": "Apa sinonim dari 'FRAGIL'?", "id": "Rapuh atau mudah pecah." },
  { "en": "Apa antonim dari 'TAMPAK'?", "id": "Tersembunyi." },
  { "en": "Bagaimana Anda memastikan objektivitas saat menangani kasus yang melibatkan isu SARA (Suku, Agama, Ras, dan Antargolongan)?", "id": "Dengan berpegang teguh hanya pada fakta hukum dan alat bukti, mengesampingkan semua stereotip atau prasangka pribadi, dan memperlakukan semua pihak secara sama di hadapan hukum." },
  { "en": "Berapakah 15² - (5x15)?", "id": "150 (225 - 75)." },
  { "en": "Apa arti 'Tri Brata' dan 'Catur Prasetya' bagi Anda?", "id": "Tri Brata adalah pedoman hidup dan Catur Prasetya adalah pedoman kerja bagi setiap anggota Polri, yang keduanya merupakan landasan fundamental dalam berpikir, bersikap, dan bertindak." },
  { "en": "Jika Anda memiliki kesempatan, perubahan positif apa yang paling ingin Anda bawa untuk institusi Polri?", "id": "Jawaban yang baik adalah yang realistis dan fokus pada perbaikan internal, misalnya: 'Saya ingin mendorong budaya kerja yang lebih transparan dan berbasis kinerja, di mana setiap anggota dihargai berdasarkan prestasi dan integritasnya'." },
  { "en": "Anda mengetahui ada program beasiswa dari institusi, namun persyaratannya sulit. Rekan Anda mengajak untuk sedikit memanipulasi data agar lolos. Sikap Anda?", "id": "Saya akan menolak dengan tegas. Mendapatkan sesuatu dengan cara curang bertentangan dengan prinsip integritas saya, lebih baik berusaha memenuhi syarat dengan jujur." },
  { "en": "Bagaimana Anda memandang seorang pimpinan yang sangat populer di kalangan anak buah tetapi sering melonggarkan aturan demi 'kenyamanan bersama'?", "id": "Meskipun populer, pimpinan seperti itu berisiko menciptakan budaya permisif yang berbahaya. Saya akan tetap berpegang pada aturan profesional sebagai standar kerja saya." },
  { "en": "Anda sedang tidak bertugas dan melihat perdebatan kecil antara warga yang berpotensi membesar. Apa yang Anda lakukan?", "id": "Saya akan mendekat dengan tenang, memperkenalkan diri jika perlu, dan mencoba menjadi penengah yang netral untuk mendinginkan situasi sebelum menjadi konflik fisik." },
  { "en": "Apa yang Anda lakukan jika Anda merasa ide atau kontribusi Anda dalam tim tidak pernah dihargai oleh atasan?", "id": "Saya akan terus bekerja dengan standar terbaik, mendokumentasikan kontribusi saya, dan mencari waktu yang tepat untuk berkomunikasi secara profesional dengan atasan mengenai kinerja dan harapan saya." },
  { "en": "Menurut Anda, apa bedanya 'melayani' dengan 'meladeni'?", "id": "Melayani berlandaskan pada ketulusan dan prosedur untuk memberikan solusi terbaik. Meladeni cenderung pasif dan hanya menuruti semua kemauan tanpa mempertimbangkan aturan atau efisiensi." },
  { "en": "Anda diberikan tugas yang sangat detail dan membutuhkan ketelitian ekstrem, yang bukan merupakan kekuatan Anda. Bagaimana Anda menyikapinya?", "id": "Saya akan menerima tugas tersebut sebagai tantangan untuk melatih ketelitian. Saya akan menggunakan alat bantu seperti checklist dan meminta rekan lain untuk memeriksa ulang pekerjaan saya." },
  { "en": "Bagaimana Anda menjelaskan kepada publik mengenai penggunaan kekuatan (misalnya gas air mata) dalam pengendalian massa?", "id": "Saya akan menjelaskan (melalui jalur resmi) bahwa tindakan tersebut adalah opsi terakhir yang terpaksa diambil sesuai prosedur untuk mencegah kerusuhan yang lebih besar, dan tujuannya adalah untuk memulihkan ketertiban." },
  { "en": "Apa arti 'menjadi teladan' di lingkungan tempat tinggal Anda?", "id": "Artinya menunjukkan perilaku yang baik dalam kehidupan sehari-hari, seperti taat aturan lalu lintas, ramah dengan tetangga, dan cepat tanggap membantu, sehingga mencerminkan citra positif Polri." },
  { "en": "Anda merasa lelah secara emosional (burnout). Apa tanda-tanda yang Anda kenali pada diri sendiri dan apa yang Anda lakukan?", "id": "Tandanya bisa berupa kehilangan motivasi, mudah marah, atau sulit fokus. Saya akan segera mengambil waktu untuk istirahat, melakukan hobi, dan berbicara dengan orang yang saya percaya untuk memulihkan kondisi." },
  { "en": "Bagaimana Anda memandang kesalahan yang dibuat oleh anak buah Anda?", "id": "Sebagai kesempatan untuk membina dan mengajar. Saya akan fokus pada perbaikan proses agar kesalahan tidak terulang, bukan pada menghukum individu secara berlebihan." },
  { "en": "Lengkapi analogi: BUKU : PERPUSTAKAAN = ARTEFAK : ...", "id": "MUSEUM." },
  { "en": "Lengkapi analogi: KERINGAT : PANAS = UAP : ...", "id": "MENDIDIH." },
  { "en": "Lengkapi analogi: PANGGUNG : AKTOR = RING : ...", "id": "PETINJU." },
  { "en": "Lengkapi analogi: KONTRAK : KESEPAKATAN = UNDANG-UNDANG : ...", "id": "KETERATURAN." },
  { "en": "Lengkapi analogi: JAHIT : BENANG = PAHAT : ...", "id": "KAYU (atau BATU)." },
  { "en": "Lengkapi analogi: MIKROBA : MIKROSKOP = BINTANG : ...", "id": "TELESKOP." },
  { "en": "Lengkapi analogi: GURUN : OASE = KEGELAPAN : ...", "id": "CAHAYA." },
  { "en": "Lengkapi analogi: Koki : Dapur = Petani : ...", "id": "Sawah." },
  { "en": "Lengkapi analogi: JEMBATAN : SUNGAI = TEROWONGAN : ...", "id": "BUKIT (atau GUNUNG)." },
  { "en": "Lengkapi analogi: MARAH : EMOSI = MERAH : ...", "id": "WARNA." },
  { "en": "Lanjutkan deret angka: 2, 6, 14, 30, 62, ...", "id": "126 (pola: x2 + 2)." },
  { "en": "Lanjutkan deret angka: 128, 64, 32, 16, 8, ...", "id": "4 (pola: dibagi 2)." },
  { "en": "Lanjutkan deret huruf: A, D, I, N, S, X, ...", "id": "C (pola loncat: +3,+5,+5,+5,+5... Pola kurang jelas. Alternatif: A(1), D(4), I(9), P(16), Y(25) -> pola kuadrat. Maka soal ini mungkin salah)." },
  { "en": "Lanjutkan deret angka: 1, 10, 2, 20, 3, 30, ...", "id": "4 (pola dua larik: 1,2,3,4,... dan 10,20,30,...)." },
  { "en": "Lanjutkan deret angka: 4, 10, 22, 46, 94, ...", "id": "190 (pola: x2 + 2)." },
  { "en": "Lanjutkan deret angka: 3, 4, 7, 8, 11, 12, ...", "id": "15 (pola: +1, +3, +1, +3, +1, +3)." },
  { "en": "Lanjutkan deret huruf: A, B, D, H, P, ...", "id": "F (pola: x2 posisi hurufnya -> 1,2,4,8,16,32. Huruf ke-32 adalah F)." },
  { "en": "Lanjutkan deret angka: 100, 98, 95, 91, 86, ...", "id": "80 (pola: -2, -3, -4, -5, -6)." },
  { "en": "Lanjutkan deret angka: 1, 3, 7, 13, 21, 31, ...", "id": "43 (pola: +2, +4, +6, +8, +10, +12)." },
  { "en": "Lanjutkan deret angka: 5, 6, 11, 17, 28, 45, ...", "id": "73 (pola: deret Fibonacci)." },
  { "en": "Apa sinonim dari kata 'RELATIF'?", "id": "Nisbi atau tidak mutlak." },
  { "en": "Apa sinonim dari kata 'SEKULER'?", "id": "Duniawi atau tidak bersifat keagamaan." },
  { "en": "Apa sinonim dari kata 'STATIS'?", "id": "Diam atau tidak bergerak." },
  { "en": "Apa sinonim dari kata 'KRITERIA'?", "id": "Ukuran atau patokan." },
  { "en": "Apa sinonim dari kata 'OTORITAS'?", "id": "Wewenang." },
  { "en": "Apa antonim dari kata 'GANDA'?", "id": "Tunggal." },
  { "en": "Apa antonim dari kata 'KONKRET'?", "id": "Abstrak." },
  { "en": "Apa antonim dari kata 'POSITIF'?", "id": "Negatif." },
  { "en": "Apa antonim dari kata 'VERTIKAL'?", "id": "Horizontal." },
  { "en": "Apa antonim dari kata 'INklusif'?", "id": "Eksklusif." },
  { "en": "Jika semua ilmuwan kreatif, dan beberapa orang kreatif adalah seniman. Kesimpulannya adalah...", "id": "Tidak dapat disimpulkan apakah beberapa ilmuwan adalah seniman." },
  { "en": "Jika perlu 5 menit untuk mengisi 1/3 bak, berapa menit lagi yang dibutuhkan untuk mengisi bak sampai penuh?", "id": "10 menit lagi." },
  { "en": "Semua karyawan mendapat bonus. Tono adalah karyawan. Kesimpulannya adalah...", "id": "Tono mendapat bonus." },
  { "en": "Jika A adalah ayah dari B, dan C adalah anak dari A, tetapi B bukan saudara C. Siapakah B bagi A?", "id": "B adalah anak perempuan dari A (jika B dan C beda jenis kelamin) atau soal ini ambigu." },
  { "en": "Seekor siput memanjat tiang 10 meter. Setiap hari ia naik 3 meter dan malamnya turun 2 meter. Pada hari ke berapa ia sampai di puncak?", "id": "Pada hari ke-8 (Pada hari ke-7, ia mencapai 7 meter. Hari ke-8, ia naik 3 meter dan sampai di puncak)." },
  { "en": "Berapakah KPK (Kelipatan Persekutuan Terkecil) dari 12 dan 15?", "id": "60." },
  { "en": "Jika 2x + 5y = 20 dan x = 5, berapa nilai y?", "id": "2." },
  { "en": "Sebuah buku dijual seharga Rp 75.000 setelah mendapat diskon 25%. Berapa harga awalnya?", "id": "Rp 100.000." },
  { "en": "Berapakah 15% dari 300 ditambah 30% dari 150?", "id": "90 (45 + 45)." },
  { "en": "Jika sebuah dadu dilempar, berapa peluang munculnya angka ganjil?", "id": "3/6 atau 1/2." },
  { "en": "Bagaimana Anda memandang penggunaan kekuatan fisik dalam tugas?", "id": "Sebagai alternatif paling akhir (ultimum remedium) yang digunakan secara terukur, proporsional, dan dapat dipertanggungjawabkan secara hukum ketika cara yang lebih lunak tidak efektif." },
  { "en": "Apa arti 'menjaga jarak' dengan pihak-pihak yang berperkara?", "id": "Artinya tidak membangun kedekatan personal yang dapat menimbulkan konflik kepentingan dan mempengaruhi objektivitas dalam penanganan perkara." },
  { "en": "Bagaimana Anda menyikapi kegagalan dalam tes atau seleksi internal?", "id": "Saya akan menerimanya sebagai bahan introspeksi untuk mengetahui kekurangan saya, lalu belajar lebih giat untuk mempersiapkan diri pada kesempatan berikutnya." },
  { "en": "Apa yang Anda lakukan jika menemukan barang bukti baru yang berpotensi mengubah status tersangka menjadi tidak bersalah?", "id": "Saya akan segera melaporkan temuan tersebut dan memasukkannya ke dalam berkas perkara. Menegakkan kebenaran dan keadilan lebih penting daripada mempertahankan argumen awal." },
  { "en": "Menurut Anda, apa tantangan psikologis terberat menjadi seorang polisi?", "id": "Menjaga empati dan sisi kemanusiaan agar tidak tumpul setelah terus-menerus terpapar pada kekerasan, kejahatan, dan penderitaan." },
  { "en": "Bagaimana Anda memastikan kesejahteraan keluarga tidak terganggu oleh jadwal kerja Anda yang tidak menentu?", "id": "Dengan memaksimalkan kualitas waktu saat bersama keluarga, menjaga komunikasi yang terbuka, dan memberikan pemahaman kepada mereka mengenai tuntutan profesi saya." },
  { "en": "Apa perbedaan antara 'bukti' dan 'petunjuk'?", "id": "Bukti adalah alat bukti yang sah menurut hukum (saksi, surat, ahli, dll) yang dapat digunakan di pengadilan. Petunjuk adalah kesesuaian antara bukti-bukti yang ada yang bisa mengarah pada suatu kesimpulan." },
  { "en": "Anda melihat seorang pejabat tinggi melakukan pelanggaran lalu lintas kecil. Apakah Anda akan menindaknya?", "id": "Ya, saya akan menindaknya sesuai prosedur yang berlaku. Hukum harus ditegakkan tanpa pandang bulu untuk membangun kepercayaan publik." },
  { "en": "Bagaimana Anda membangun ketahanan terhadap godaan suap atau gratifikasi?", "id": "Dengan selalu mengingat sumpah jabatan, memikirkan dampak jangka panjang bagi karir dan keluarga, serta menerapkan gaya hidup sederhana sesuai penghasilan yang sah." },
  { "en": "Jika Anda bisa kembali ke awal karir, nasihat apa yang akan Anda berikan pada diri Anda sendiri?", "id": "Jawaban yang baik adalah yang menunjukkan refleksi dan kebijaksanaan, misal: 'Teruslah belajar, jaga integritas sejak hari pertama, dan jangan pernah lelah melayani'." },
  { "en": "Lengkapi analogi: SAYUR : VITAMIN = NASI : ...", "id": "KARBOHIDRAT." },
  { "en": "Lengkapi analogi: SEMINAR : SERTIFIKAT = PERTANDINGAN : ...", "id": "PIALA (atau MEDALI)." },
  { "en": "Lengkapi analogi: API : PANAS = SALJU : ...", "id": "DINGIN." },
  { "en": "Lengkapi analogi: KULIAH : DOSEN = SEKOLAH : ...", "id": "GURU." },
  { "en": "Lengkapi analogi: BUTA : WARNA = TULI : ...", "id": "SUARA." },
  { "en": "Lanjutkan deret angka: 2, 5, 11, 23, 47, ...", "id": "95 (pola: x2 + 1)." },
  { "en": "Lanjutkan deret angka: 1, 4, 9, 16, 25, 36, ...", "id": "49 (pola: bilangan kuadrat)." },
  { "en": "Lanjutkan deret huruf: A, Z, B, Y, C, X, D, ...", "id": "W (pola: maju dari depan, mundur dari belakang)." },
  { "en": "Lanjutkan deret angka: 4, 8, 10, 20, 22, 44, ...", "id": "46 (pola: x2, +2, diulang)." },
  { "en": "Lanjutkan deret angka: 360, 180, 60, 15, 3, ...", "id": "1 (pola: :2, :3, :4, :5, :6)." },
  { "en": "Apa sinonim dari kata 'VALIDASI'?", "id": "Pengujian kebenaran atau pengesahan." },
  { "en": "Apa sinonim dari kata 'ESKALASI'?", "id": "Peningkatan atau kenaikan." },
  { "en": "Apa sinonim dari kata 'JARGON'?", "id": "Istilah khusus." },
  { "en": "Apa antonim dari kata 'GRATIS'?", "id": "Bayar." },
  { "en": "Apa antonim dari kata 'MANUAL'?", "id": "Otomatis." },
  { "en": "Berapakah 37,5% dari 240?", "id": "90." },
  { "en": "Sebuah mobil menghabiskan 8 liter bensin untuk 96 km. Jika mobil tersebut akan menempuh 144 km, berapa liter bensin yang dibutuhkan?", "id": "12 liter." },
  { "en": "Jika 3x - 7 = 14, maka nilai 2x adalah...", "id": "14 (x=7)." },
  { "en": "Berapa banyak cara 3 orang bisa duduk di 5 kursi yang tersedia?", "id": "60 cara (Permutasi 5P3)." },
  { "en": "Jika A:B = 2:3 dan B:C = 4:5, maka A:C adalah...", "id": "8:15." },
  { "en": "Bagaimana Anda menyikapi rekan yang sering 'mencari muka' di depan atasan?", "id": "Saya akan tetap fokus pada kualitas pekerjaan saya sendiri dan tidak ikut dalam persaingan yang tidak sehat. Biarkan kinerja yang berbicara." },
  { "en": "Apa arti 'proporsional' dalam penggunaan kekuatan?", "id": "Artinya tingkat kekuatan yang digunakan harus seimbang dengan tingkat ancaman yang dihadapi, tidak boleh berlebihan." },
  { "en": "Bagaimana Anda menjaga objektivitas saat media massa sudah 'menghakimi' seorang tersangka sebelum ada putusan pengadilan?", "id": "Saya akan tetap berpegang pada asas praduga tak bersalah dan fokus pada pembuktian fakta hukum di dalam proses penyidikan, bukan pada opini publik." },
  { "en": "Apa yang Anda lakukan jika merasa tidak dihargai di tempat kerja?", "id": "Saya akan melakukan introspeksi terlebih dahulu, kemudian mencoba berkomunikasi dengan atasan mengenai kontribusi dan harapan saya, sambil terus menunjukkan kinerja terbaik." },
  { "en": "Menurut Anda, apa satu kata yang paling menggambarkan seorang anggota Polri yang ideal?", "id": "Mengayomi (atau Melayani, Berintegritas, Profesional)." },
  { "en": "Jika sebuah jam rusak dan berputar dua kali lebih cepat, pukul berapa yang ditunjukkannya saat waktu sebenarnya pukul 14:00 jika keduanya disetel benar pada pukul 12:00?", "id": "Pukul 16:00." },
  { "en": "Semua A adalah B. Tidak ada C yang merupakan B. Kesimpulannya adalah...", "id": "Tidak ada C yang merupakan A." },
  { "en": "Lengkapi analogi: SISIK : IKAN = BULU : ...", "id": "BURUNG." },
  { "en": "Lanjutkan deret: 9, 9, 9, 6, 9, 3, ...", "id": "9 (pola dua larik: 9,9,9,9,... dan 9,6,3,... [-3])." },
  { "en": "Apa sinonim dari 'PROGNOSIS'?", "id": "Prakiraan (biasanya tentang penyakit)." },
  { "en": "Apa antonim dari kata 'KONSERVATIF'?", "id": "Progresif atau liberal." },
  { "en": "Anda menemukan kesalahan fatal dalam laporan yang dibuat oleh senior Anda. Bagaimana Anda menyampaikannya?", "id": "Saya akan menemuinya secara pribadi, menunjukkan data temuan saya dengan sopan, dan menyampaikannya sebagai bahan diskusi atau kroscek, bukan sebagai upaya menyalahkan." },
  { "en": "Berapa nilai dari (2/3) / (4/9)?", "id": "1.5 atau 3/2." },
  { "en": "Apa komitmen pribadi Anda untuk mencegah korupsi di lingkungan Anda?", "id": "Saya berkomitmen untuk tidak pernah meminta, menerima, atau memberi suap/gratifikasi dalam bentuk apapun, serta berani melaporkan jika melihat praktik tersebut sesuai prosedur." },
  { "en": "Jika Anda bisa memiliki satu 'kekuatan super' untuk membantu pekerjaan Anda, apa itu dan mengapa?", "id": "Jawaban yang baik adalah yang simbolis dan terkait tugas, misal: 'Kemampuan untuk mendeteksi kebohongan, agar proses penyidikan bisa lebih cepat dan adil dalam menemukan kebenaran'." }



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

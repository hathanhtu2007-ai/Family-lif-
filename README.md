<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Học Từ Vựng: Family Life</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f7f6;
            text-align: center;
            padding: 20px;
        }
        .container {
            max-width: 500px;
            margin: auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        .word-card {
            font-size: 28px;
            font-weight: bold;
            color: #333;
            margin: 20px 0;
        }
        .meaning-card {
            font-size: 20px;
            color: #666;
            margin-bottom: 20px;
        }
        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            margin: 5px;
            transition: 0.3s;
        }
        button:hover { background-color: #45a049; }
        .btn-speak { background-color: #2196F3; }
        .btn-speak:hover { background-color: #0b7dda; }
        .btn-test { background-color: #ff9800; }
        .btn-test:hover { background-color: #e68900; }
        .result-box {
            margin-top: 15px;
            font-size: 18px;
            font-weight: bold;
            color: #d9534f;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>Chủ đề: Family Life (Cuộc sống gia đình)</h2>
    <hr>
    
    <div id="wordCard" class="word-card">Nhấn "Bắt đầu" để học</div>
    <div id="meaningCard" class="meaning-card"></div>

    <div>
        <button class="btn-speak" onclick="playPronunciation()">🔊 Nghe phát âm</button>
        <button onclick="nextWord()">Từ tiếp theo ➡️</button>
    </div>

    <hr>
    <h3>Kiểm tra: Đọc từ bạn vừa thấy</h3>
    <button class="btn-test" onclick="startRecognition()">🎤 Nói từ tiếng Anh</button>
    <div id="result" class="result-box"></div>
</div>

<script>
    // Danh sách từ vựng chủ đề Family Life
    const vocabList = [
        { en: "Parents", vn: "Cha mẹ" },
        { en: "Siblings", vn: "Anh chị em ruột" },
        { en: "Housework", vn: "Việc nhà" },
        { en: "Breadwinner", vn: "Trụ cột gia đình" },
        { en: "Homemaker", vn: "Người nội trợ" },
        { en: "Supportive", vn: "Ủng hộ, hỗ trợ" },
        { en: "Bond", vn: "Sự gắn kết" }
    ];

    let currentWordIndex = 0;

    // Cập nhật từ vựng lên màn hình
    function showWord() {
        const current = vocabList[currentWordIndex];
        document.getElementById('wordCard').textContent = current.en;
        document.getElementById('meaningCard').textContent = `Nghĩa: ${current.vn}`;
        document.getElementById('result').textContent = ""; 
    }

    // Chuyển sang từ tiếp theo
    function nextWord() {
        currentWordIndex = (currentWordIndex + 1) % vocabList.length;
        showWord();
    }

    // Phát âm chuẩn (Text to Speech)
    function playPronunciation() {
        const word = vocabList[currentWordIndex].en;
        const speech = new SpeechSynthesisUtterance(word);
        speech.lang = 'en-US'; // Giọng Anh - Mỹ
        window.speechSynthesis.speak(speech);
    }

    // Nhận diện giọng nói (Speech Recognition)
    function startRecognition() {
        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        if (!SpeechRecognition) {
            alert("Rất tiếc, trình duyệt của bạn không hỗ trợ tính năng nhận diện giọng nói. Hãy dùng Google Chrome.");
            return;
        }

        const recognition = new SpeechRecognition();
        recognition.lang = 'en-US'; // Kiểm tra phát âm tiếng Anh
        recognition.start();
        document.getElementById('result').textContent = "Đang lắng nghe... Hãy đọc từ đó.";

        recognition.onresult = function(event) {
            const spokenWord = event.results[0][0].transcript.trim().toLowerCase();
            const correctWord = vocabList[currentWordIndex].en.toLowerCase();

            // So sánh từ đọc được và từ chuẩn
            if (spokenWord === correctWord) {
                document.getElementById('result').style.color = "green";
                document.getElementById('result').textContent = `Bạn nói đúng rồi! Bạn đọc là: "${spokenWord}"`;
            } else {
                document.getElementById('result').style.color = "red";
                document.getElementById('result').textContent = `Chưa đúng. Bạn đọc là: "${spokenWord}". Thử lại nhé!`;
            }
        };

        recognition.onerror = function() {
            document.getElementById('result').textContent = "Không nhận diện được giọng nói, vui lòng thử lại.";
        };
    }

    // Hiển thị từ đầu tiên khi load web
    showWord();
</script>

</body>
</html>

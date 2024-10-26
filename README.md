<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kan Voice Assistant</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f5f5f5;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .container {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 300px;
            text-align: center;
        }
        .response {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Kan M-S/B</h1>
        <button onclick="startListening()">듣기</button>
        <div id="response" class="response"></div>
    </div>

    <script>
        const synth = window.speechSynthesis;
        const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
        recognition.lang = 'ko-KR';
        recognition.interimResults = false;
        recognition.continuous = false;

        function playEffectSound() {
            const audioContext = new (window.AudioContext || window.webkitAudioContext)();
            const oscillator = audioContext.createOscillator();
            oscillator.type = 'sine';
            oscillator.frequency.setValueAtTime(523.25, audioContext.currentTime); // 도
            oscillator.frequency.setValueAtTime(659.25, audioContext.currentTime + 0.5); // 미
            oscillator.connect(audioContext.destination);
            oscillator.start();
            oscillator.stop(audioContext.currentTime + 1);
        }

        function startListening() {
            playEffectSound();
            recognition.start();
        }

        recognition.onresult = function(event) {
            const transcript = event.results[0][0].transcript;
            document.getElementById("response").textContent = "들린 내용: " + transcript;

            let responseText = handleCommand(transcript.toLowerCase());
            speakResponse(responseText);
        };

        function handleCommand(command) {
            if (command.includes("안녕") || command.includes("니 하오")) {
                return "안녕하세요! 저는 Kan 입니다! 만나서 반가워요 :>";
            } else if (command.includes("보안") || command.includes("security")) {
                return "보안은 아주 좋습니다. 개인 코드가 있고 데이터 서버를 이용하지 않으며, 기기 내 모드가 원활합니다. Kan은 HTML을 사용하므로 코드가 잘 작동하며 사용자가 직접 수정할 수 있습니다.";
            } else if (command.includes("사랑") || command.includes("love")) {
                return "사랑해요! 정말 감사합니다! o3o";
            } else if (command.includes("이름")) {
                return "제 이름은 Kan입니다!";
            } else if (command.includes("시") || command.includes("타임")) {
                const currentTime = new Date();
                currentTime.setHours(currentTime.getHours() - 3); // 예시로 -3시간
                return "현재 한국 시간은 " + currentTime.toLocaleTimeString("ko-KR") + "입니다.";
            } else if (command.includes("음식") || command.includes("메뉴")) {
                const foodOptions = ["고기", "찌개", "김밥", "닭찜", "김치볶음밥", "파전", "죽", "빵", "김치", "과자", "밥", "피자", "닭발", "도시락", "샌드위치", "족발", "수박", "닭고기", "마라탕", "햄버거", "매운 돼지고기 볶음", "주스"];
                const randomFood = foodOptions[Math.floor(Math.random() * foodOptions.length)];
                return "오늘의 추천 음식: " + randomFood;
            } else if (command.includes("동전") || command.includes("coin")) {
                const coin = Math.random() < 0.5 ? "앞면" : "뒷면";
                return "동전 던지기 결과: " + coin;
            } else if (command.includes("버전") || command.includes("정보") || command.includes("업데이트") || command.includes("시스템")) {
                return "현재 버전은 rainbow ai OS 3.9.01.22입니다. 업데이트 내용: 명령어 추가, AI 이름 변경, 편의성 향상, 오류 수정, 명령어 추천 기능 추가.";
            } else if (command.includes("hey")) {
                return "네! 언제든지 기다리고 있어요!";
            } else if (command.includes("꺼져")) {
                return "KAN AI의 종료 방법은 실행 어플마다 다릅니다.";
            } else {
                return ":( 이해하지 못했어요. 피드백을 보내주세요. 가능한 명령어: '동전 던지기', '메뉴 추천', '한국 시간', '보안 우수성', '~할 확률'";
            }
        }

        function speakResponse(responseText) {
            const utterance = new SpeechSynthesisUtterance(responseText);
            utterance.lang = 'ko-KR';
            synth.speak(utterance);
            document.getElementById("response").textContent = responseText;
        }

        recognition.onerror = function(event) {
            document.getElementById("response").textContent = "오류 발생: " + event.error;
        };

        recognition.onspeechend = function() {
            recognition.stop();
        };
    </script>
</body>
</html>

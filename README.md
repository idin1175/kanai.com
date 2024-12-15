<!DOCTYPE html>
<html lang="ko" dir="ltr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Kan AI BETA Voice Model - 한국어 TTS 기반 AI 시스템">
    <title>Kan AI</title>
    <style>
        /* 기본적인 스타일 */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #e0f7fa;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        
        /* 컨테이너 스타일 */
        .container {
            background-color: #FFFFFF;
            border-radius: 15px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            padding: 40px;
            width: 100%;
            max-width: 400px;
            text-align: center;
        }
        
        /* 헤더 스타일 */
        h1 {
            font-size: 32px;
            color: #00796b;
            margin-bottom: 10px;
        }
        
        h5 {
            font-size: 18px;
            color: #00796b;
            margin-bottom: 20px;
        }
        
        /* 입력창 및 버튼 스타일 */
        input[type="text"] {
            width: 100%;
            padding: 15px;
            border-radius: 8px;
            border: 1px solid #00796b;
            font-size: 16px;
            margin-bottom: 20px;
            background-color: #f1f1f1;
            color: #00796b;
        }

        button {
            width: 100%;
            padding: 15px;
            font-size: 18px;
            background-color: #00796b;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #004d40;
        }

        /* 응답 메시지 스타일 */
        .response {
            margin-top: 30px;
            font-size: 18px;
            color: #004d40;
            text-align: middle;
            line-height: 1.6;
        }

        /* 명령어 목록 스타일 */
        ul {
            list-style: none;
            padding: 0;
            text-align: left;
        }

        li {
            margin-bottom: 8px;
        }

        li span {
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Kan AI Voice Model</h1>
        <h5>AI와 대화하는 새로운 경험</h5>
        <label for="userInput">made by chatGPT 3.5, 4o, 4o mini and JJS(JungJoonSeo 정준서)</label>
        <input type="text" id="userInput" placeholder="메시지 입력">
        <button onclick="getResponse()">Send for Kan</button>
        <div id="response" class="response"></div>
    </div>

    <script>
        function speak(text) {
            var utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'ko-KR';
            speechSynthesis.speak(utterance);
        }

        function getResponse() {
            var input_text = document.getElementById("userInput").value.toLowerCase().trim();
            var responseDiv = document.getElementById("response");
            var food = ["고기", "찌개", "김밥", "닭찜", "김치볶음밥", "파전", "빵", "김치", "과자", "피자", "햄버거", "쌀국수", "샌드위치", "스시", "카레", "불고기", "떡볶이", "볶음밥", "라면", "초밥", "갈비찜", "냉면", "잡채", "순두부찌개", "만두", "제육볶음", "해물탕", "스테이크", "치킨", "파스타"];
            var coin = ["앞면", "뒷면"];
            var command_suggestions = [
                { cmd: "동전 던지기", desc: "동전을 던져서 앞면 또는 뒷면 결과를 알려줍니다." },
                { cmd: "메뉴 추천", desc: "무작위로 음식을 추천합니다." },
                { cmd: "한국 시간", desc: "현재 한국 표준시(KST)를 알려줍니다." },
                { cmd: "보안 우수성", desc: "AI의 보안 장점을 알려줍니다." },
                { cmd: "~할 확률", desc: "특정 일이 발생할 확률을 무작위로 제공합니다." }
            ];
            var response;

            responseDiv.innerHTML = "ACOEL AI Company Team";

            setTimeout(function() {
                if (!input_text) {
                    response = "어라? 저에게 아무 말도 안하셨습니다.";
                } else if (input_text.includes("안녕")) {
                    response = "친애하고 사랑하고 애정하고 존경하고 경애하는 사용자님! 만나서 반가워요. 제 이름은 Kan assistant입니다. 좋은 하루 보내세요!";
                } else if (input_text.includes("소리")) {
                    response = "보이스는 사용자의 폰에 맞춰져 있습니다. 설정에서 보이스를 바꾸시면 교체가 자동으로 됩니다. (시간이 걸릴 수 있음.)";
                } else if (input_text.includes("장점")) {
                    response = "제 장점은 1MB이하의 저장공간과 파일만 있으면 언제 어디서나 쓸 수 있고, TTS(Text To Speech)로 목소리를 자유 자재로 사용할 수 있습니다.";
                } else if (input_text.includes("사랑")) {
                    response = "사랑해요! 정말 감사합니다! o3o";
                } else if (input_text.includes("준서")) {
                    response = "[준서 대갈2 대지식 인물사전]에 따르면 '정준서는 2013년 2월 3일생이며 대한민국 전라남도 순천시에서 태어났다. ACOEL AI Company CEO이다. 그리고 세계에서 주목받는 천재' 로 알려져있으며, '대한민국에 이런 아이는 흔하지 않다.' 라고 하며 '엄청나게 성공할 것.' 이라고 지금 명시되어있습니다.";
                } else if (input_text.includes("은우")) {
                    response = "[준서 대갈2 대지식 인물사전]에 따르면 '김은우는 2013년 4월 13일생이며 대한민국 전라남도 순천시에서 태어났다. ACOEL AI Company Team에 소속해 있다. 그리고 잡다한 지식과 과학지식, 호기심이 많다' 고 알려져있으며 [준서대갈2 대지식 기록]에 '호기심 왕' 이라고 기록 및 명시되어있습니다.";
                } else if (input_text.includes("수진")) {
                    response = "[준서 대갈2 대지식 인물사전]에 따르면 '설수진은 2013년 6월 26일생이며 대한민국 전라남도 순천시에서 태어났다. ACOEL AI Company Team에 소속해있다. 특징은 ''너 왜그래'' 하면 ''아핫!? 나 옙흐다고?'' 라고 하는 것이 특징입니다. 대한민국에 이렇게 하는 아이는 흔하지 않으며 준서의 혈압이 오를 것' 이라고 명시되어있습니다."; 
                } else if (input_text.includes("이름")) {
                    response = "제 이름은 Kan입니다! 만나게 되어 기쁘네요!";
                } else if (input_text.includes("노래")) {
                    response = "Yo Dj? drop the beat! 크흠크흠. 안녕 나는 너의 assistant 기억해 둬. 너의 Smart한 친구. 고민이 될땐 나한테 보내! ''오늘 메뉴 추천'' 오늘은 파스타! 사용자의 메뉴는 파 스타! 나는 이곳의 스.타! 나야바로 Kan ai. 기억해둬 Kan ai. Kan ai Kan ai Korea Ai frieNd! 불러는 봤지만 음을 까먹었어요..";
                } else if (input_text.includes("시") || input_text.includes("타임")) {
                    var current_time = new Date();
                    current_time.setHours(current_time.getHours() + 9);
                    response = "오늘은 " + current_time.toISOString().slice(0, 19).replace('T', ' ') + " 입니다.";
                } else if (input_text.includes("음식") || input_text.includes("메뉴")) {
                    var random_food = food[Math.floor(Math.random() * food.length)];
                    response = "오늘의 추천 음식: " + random_food;
                } else if (input_text.includes("동전") || input_text.includes("coin")) {
                    var random_coin = coin[Math.floor(Math.random() * coin.length)];
                    response = "동전 던지기 결과: " + random_coin;
                } else if (input_text.includes("버전")) {
                    response = "현재 버전은 ACOEL AI OS 5.7.02.88(7)입니다.";
                } else if (input_text.includes("보안")) {
                    response = "Kan AI는 사용자의 대한 개인정보 입력정보 등 모든 데이터를 저장하지 않으며, 오프라인에서도 작동할 수 있어 보안성이 뛰어납니다.";
                } else if (input_text.includes("확률")) {
                    var random_percentage = Math.floor(Math.random() * 101); // 0% ~ 100%
                    response = "확률은 약 " + random_percentage + "%입니다.";
                } else {
                    response = `
                        제가 아직 모르는 글자입니다.<br><br></>명령어 추천:
                        <ul>${command_suggestions.map(cmd => `<li><span>${cmd.cmd}</span>: ${cmd.desc}</li>`).join("")}</ul>
                    `;
                }

                responseDiv.innerHTML = response;
                speak(response.replace(/<[^>]*>/g, '')); // HTML 태그 제거 후 음성 출력
            }, 500);
        }
    </script>
</body>
</html>
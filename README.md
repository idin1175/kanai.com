<!DOCTYPE html>
<html lang="ko" dir="ltr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Kan AI BETA Voice Model - 한국어 TTS 기반 AI 시스템">
    <title>Kan AI</title>
    <style>
        :root {
            --primary-color: #4a90e2; /* 연한 파란색 */
            --secondary-color: #357abd; /* 진한 파란색 */
            --danger-color: #ff6b6b; /* 빨간색 */
            --background-light: #f8f9fa;
        }

        body {
            background-color: var(--background-light);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0;
            padding: 20px;
            font-family: 'Segoe UI', sans-serif;
        }

        .container {
            background: white;
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            padding: 30px;
            width: 100%;
            max-width: 450px;
            transition: transform 0.2s;
        }

        .container:hover {
            transform: translateY(-2px);
        }

        .header {
            text-align: center;
            margin-bottom: 25px;
            border-bottom: 2px solid var(--primary-color);
            padding-bottom: 15px;
        }

        h1 {
            font-size: 2rem;
            color: var(--primary-color);
            margin: 0 0 5px 0;
            letter-spacing: -0.5px;
        }

        h5 {
            font-size: 1rem;
            color: var(--secondary-color);
            margin: 0;
            opacity: 0.8;
        }

        .input-group {
            margin-bottom: 20px;
        }

        input[type="text"] {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid var(--primary-color);
            border-radius: 8px;
            font-size: 1rem;
            transition: border-color 0.3s;
            box-sizing: border-box;
        }

        input[type="text"]:focus {
            outline: none;
            border-color: var(--secondary-color);
            box-shadow: 0 0 8px rgba(74, 144, 226, 0.2);
        }

        .button-group {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        button {
            flex: 1;
            padding: 12px;
            font-size: 1rem;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 600;
        }

        #sendBtn {
            background-color: var(--primary-color);
            color: white;
        }

        #sendBtn:hover {
            background-color: var(--secondary-color);
            box-shadow: 0 4px 12px rgba(53, 122, 189, 0.3);
        }

        #voiceStopBtn {
            background-color: var(--danger-color);
            color: white;
            max-width: 120px;
        }

        #voiceStopBtn:hover {
            background-color: #ff5252;
            box-shadow: 0 4px 12px rgba(255, 107, 107, 0.3);
        }

        .response {
            background: rgba(74, 144, 226, 0.05);
            padding: 20px;
            border-radius: 10px;
            min-height: 100px;
            font-size: 1rem;
            color: var(--secondary-color);
            line-height: 1.6;
            border: 1px solid rgba(74, 144, 226, 0.1);
        }

        .credit {
            text-align: center;
            margin-top: 15px;
            font-size: 0.85rem;
            color: #666;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Kan AI</h1>
            <h5>ACOÉL since 2020</h5>
        </div>
        
        <div class="input-group">
            <input type="text" id="userInput" placeholder="입력창">
        </div>

        <div class="button-group">
            <button id="sendBtn" onclick="getResponse()">전송하기</button>
            <button id="voiceStopBtn" onclick="stopVoice()">답변중지</button>
        </div>

        <div id="response" class="response">여기에서 말할게요! ^-^</div>
        <div class="credit">Developed by ChatGPT & JJS & DeepSeek</div>
    </div>

    <script>
        function speak(text) {
            var utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'ko-KR';
            speechSynthesis.speak(utterance);
        }

        function stopVoice() {
            speechSynthesis.cancel();
        }

        function getResponse() {
            var input_text = document.getElementById("userInput").value.toLowerCase().trim();
            var responseDiv = document.getElementById("response");
            var food = ["고기", "찌개", "김밥", "닭찜", "김치볶음밥", "파전", "빵", "김치", "과자", "피자", "햄버거", "쌀국수", "샌드위치", "스시", "카레", "불고기", "떡볶이", "볶음밥", "라면", "초밥", "갈비찜", "냉면", "잡채", "순두부찌개", "만두", "제육볶음", "해물탕", "스테이크", "치킨", "파스타"];
            var coin = ["앞면(그림)", "뒷면(숫자)"];
            var command_suggestions = [
                { cmd: "동전 던지기", desc: "동전을 던져서 앞면 또는 뒷면 결과를 알려줍니다." },
                { cmd: "메뉴 추천", desc: "무작위로 음식을 추천합니다." },
                { cmd: "한국 시간", desc: "현재 한국 표준시(KST)를 알려줍니다." },
                { cmd: "보안 우수성", desc: "AI의 보안 장점을 알려줍니다." },
                { cmd: "~할 확률", desc: "특정 일이 발생할 확률을 무작위로 제공합니다." }
            ];
            var response;

            responseDiv.innerHTML = "ACOEL Since 2020";

            setTimeout(function() {
                if (!input_text) {
                    response = "아직 아무 말씀도 안하셨습니다.";
                } else if (input_text.includes("안녕")) {
                    response = "안녕하세요. 만나서 반갑습니다. ACOÉL Ai(Kan)입니다. 무엇을 도와드릴까요?";
                } else if (input_text.includes("고급")) {
                    response = "";    
                } else if (input_text.includes("English") || input_text.includes("english") || input_text.includes("영어")) {
                    response = "Sorry. This ACOEL Ai (Kan) is the Korean version, so you need English(United Kingdom)? we give it. 죄송합니다. ACOEL Ai (Kan)은 한국판이오니, 원하신다면 영어(영국)판으로 제공 드리겠습니다.";
                } else if (input_text.includes("날씨")) {
                    response = "오늘 날씨는 춥거나 덥거나 또는 그 중간일 수 있으며. 강수량은 0% 에서 100% 이며 겨울에는 눈이 올 확률도 있습니다. 아침에는 해가 뜨고 저녁에는 해가 지며 새벽에는 해가 덜 뜰 것으로 예상이 됩니다. ACOEL Ai Weather 제공.";    
                } else if (input_text.includes("비트박스")) {
                    response = "이번 비트박스는 박자를 잘 맞춰봤어요 :)";
                    speak("킇음!. 제가 어디서 들었는데요, 비트박스는 두가지만 기억하래요. 북치기............. 박치기............. 북치기 박치기 북치기 박치기 박치기 북치기 박치기 북치기 북치기 박치기 북치기 박치기 박치기 북치기 박치기 북치기........... 헤헷!......");
                } else if (input_text.includes("목소리")) {
                    response = "제 보이스는 여기에서 바꿀 수 없지만 사용자님께서 이용하시고 계신 기기에서 TTS설정을 하시고 조금만 기다리시면 바뀌어있을 것입니다.";
                } else if (input_text.includes("테스트")) {
                    response = "현재 출력이 잘 되는 것을 확인하니 코드에는 큰 문제가 없는 것 같네요 :)";
                } else if (input_text.includes("사랑")) {
                    response = "싸랑해용! 싸랑해용! 싸랑해용! 싸랑해용! 싸랑해용! 싸랑해용! 싸랑해용!";
                } else if (input_text.includes("준서")) {
                    response = "[준서 대갈2 대지식 인물사전]에 따르면 '정준서는 2013년 2월 3일생이며 대한민국 전라남도 순천시에서 태어났다. ACOEL AI Company CEO이다. 그리고 세계에서 주목받는 천재' 로 알려져있으며, '대한민국에 이런 아이는 흔하지 않다.' 라고 하며 '엄청나게 성공할 것.' 이라고 지금 명시되어 있습니다.";
                } else if (input_text.includes("은우")) {
                    response = "[준서 대갈2 대지식 인물사전]에 따르면 '김은우는 2013년 4월 13일생이며 대한민국 전라남도 순천시에서 태어났다. ACOEL AI Company Team에 소속해 있다. 그리고 잡다한 지식과 과학지식, 호기심이 많다' 고 알려져있으며 [준서대갈2 대지식 기록]에 '호기심 왕' 이라고 기록 및 명시되어 있습니다.";
                } else if (input_text.includes("수진")) {
                    response = "[준서 대갈2 대지식 인물사전]에 따르면 '설수진은 2013년 6월 26일생이며 대한민국 전라남도 순천시에서 태어났다. ACOEL AI Company Team에 소속해있다. 특징은 ''너 왜그래'' 하면 ''아핫!? 나 옙흐다고?'' 라고 하는 것이 특징입니다. 대한민국에 이렇게 하는 아이는 흔하지 않으며 준서의 혈압이 오를 것' 이라고 명시되어 있습니다.";
                } else if (input_text.includes("은찬")) {
                    response = "[준서 대갈2 대지식 인물사전]에 따르면 '김은찬은 2013년 11월 7일생이며 대한민국 전라남도 순천시에 있는 병원에서 태어났으나, 광양이 고향이다. 준서의 사촌이자, 친구같은 사이이다. 친구들과 노는 것과 게임을 좋아하며, 축구를 잘 한다.' 라고 명시되어 있습니다.";
                } else if (input_text.includes("은성")) {
                    response = "[준서 대갈2 대지식 인물사전]에 따르면 '김은성은 2016년 1월 22일생이며 대한민국 전라남도 순천시에 있는 병원에서 태어났으나, 광양이 고향이다. 준서의 사촌이자, 친구같은 사이이다. 친구들과 노는 것을 좋아하며, 게임을 즐겨한다.' 라고 명시되어 있습니다.";
                } else if (input_text.includes("동규")) {
                    response = "[준서 대갈2 대지식 인물사전]에 따르면 '김동규는 2013년 9월 4일생이며 대한민국 전라북도 군산시에서 태어났다. 준서의 친구이며 ''아~ 형이 보여줄게~!!'' 라고 하는것이 취미이며 좋아하는 캐릭터는 멩구이다.' 라고 명시되어 있습니다.";
                } else if (input_text.includes("건우")) {
                    response = "[준서 대갈2 대지식 인물사전]에 따르면 '김건우는 2013년 9월 28일생이며 대한민국 전라남도 순천시에서 태어났다. 준서의 친구이며 운동을 좋아한다.' 라고 명시되어 있습니다.";
                } else if (input_text.includes("이름")) {
                    response = "제 이름은 Kan입니다! 만나게 되어 기쁘네요!";
                } else if (input_text.includes("노래")) {
                    response = "yo Dj? drop the beat! .....어라? 비트가 안깔려요";
                } else if (input_text.includes("시") || input_text.includes("타임")) {
                    var current_time = new Date();
                    current_time.setHours(current_time.getHours() + 9);
                    response = "오늘은 " + current_time.toISOString().slice(0, 19).replace('T', ' ') + " 입니다. 좋은 하루 보내세요!";
                } else if (input_text.includes("음식") || input_text.includes("메뉴")) {
                    var random_food = food[Math.floor(Math.random() * food.length)];
                    response = "오늘은 " + random_food + "!";
                } else if (input_text.includes("동전") || input_text.includes("coin")) {
                    var random_coin = coin[Math.floor(Math.random() * coin.length)];
                    response = "동전 던지기 결과는 " + random_coin + "입니다.";
                } else if (input_text.includes("acoel") || input_text.includes("Acoel") || input_text.includes("아코앨")) {
                    response = "ACOÉL 창립 및 목표, 업적<br>2020: ACOÉL 창립<br>2020: BOX TV 1대 생산<br>2023: ACOÉL Flip시리즈 출시 시작<br>2024: Pocket, Fold 시리즈 생산<br>2024.02.26: ACOÉL Kan ai 개발자판 1.0.0 '제작'<br><br>ACOÉL이 벌써 5주년을 맞이했습니다. 이용해주셔서 감사합니다.";
                } else if (input_text.includes("버전")) {
                    response = "현재 버전은 ACOEL AI 7.4.62.11.23.10(0)입니다.<br>가장 큰 업데이트 내용:<br>특정입력어가 답변이 안되는 오류수정";
                } else if (input_text.includes("장점")) {
                    response = "Kan Ai는 사용자의 대한 개인정보 입력정보 등 모든 데이터를 저장하지 않으며, 오프라인에서도 작동할 수 있어 보안성이 뛰어납니다. 또한 1MB미만(KB단위)으로 저장공간이 터지기 직전에도 다운로드 할 수 있습니다. 온디바이스 입니다.";
                } else if (input_text.includes("확률")) {
                    var random_percentage = Math.floor(Math.random() * 101); // 0% ~ 100%
                    response = "확률은 약 " + random_percentage + "%입니다.";
                } else {
                    response = `${input_text}기능은 사용할 수 없는 기능입니다.`;
                }

                responseDiv.innerHTML = response;
                speak(response);
            }, 500);
        }
    </script>
</body>
</html>
Google Teachable Machine으로 웹캠 기반 이미지 분류 모델을 만들었어.
모델은 Teachable Machine에서 온라인으로 업로드했고, 아래 URL을 사용할 거야.
[TEACHABLE_MACHINE_MODEL_URL]

이 모델을 이용해서 사용자가 웹캠을 켜면 실시간으로 이미지를 분류해주는 앱을 만들어줘.

현재 내 모델의 카테고리는 3개야.

[
“Soomin",
“Cool”,
“Shy”
]

각 카테고리의 의미는 다음과 같아.

{
"soomin": "여자 얼굴",
“Cool": "선글라스를 쓴 얼굴",
“Shy”: "손으로 얼굴을 가린 모습"
}

나중에 다른 모델을 사용할 수도 있으니 위의 [카테고리]와 {설명}은 쉽게 바꿀 수 있게 만들어줘.

각 분류 결과에 따라 짧고 위트 있는 멘트를 보여줘.

예시:
- Soomin: "Yep, you're Soomin."
- Cool: "I like your shades 😎"
- Shy: "Hide your face, hide your secrets."

첫 화면에서 바로 웹캠을 켜고 분류 결과를 볼 수 있게 해줘.

가장 높은 확률의 결과를 크게 보여주고, confidence도 같이 보여줘.

디자인은 요즘 Gen Z / Alpha Gen이 좋아할 만한 힙한 카메라 서비스처럼 만들어줘.
너무 기업용 대시보드처럼 만들지 말고, 심플하고 재밌게 해줘.

그리고 Capture 버튼을 만들어서 현재 웹캠 화면 + 분류 결과 + 멘트를 하나의 카드 이미지로 저장할 수 있게 해줘.

모바일에서도 잘 작동하게 해줘.

중요:
- 이미지 분류는 반드시 내가 제공한 Teachable Machine 모델의 실제 prediction 결과를 사용해.
- 위의 Teachable Machine 모델 URL에서 모델을 불러오도록 해줘.
- 모델이 로드되지 않거나 오류가 발생해도 임의의 heuristic, pixel analysis, Gemini Vision, mock/simulation classifier로 대체하지 마. 대신 오류를 표시해.
- Teachable Machine에서 학습할 때와 동일하게 webcam 입력을 정사각형으로 처리하고 selfie mirror 방향을 유지해.
- 이전 prediction이 끝난 후 다음 frame을 prediction하도록 해서 동시에 여러 inference가 실행되지 않게 해.
- class를 코드에서 추측하지 말고, Teachable Machine 모델이 반환하는 실제 className과 probability를 사용해.

에러 방지
- 라이브러리는 tfjs@1.3.1, @teachablemachine/image@0.8.5 버전으로 고정해. latest는 쓰지 마. 
- 모델은 앱 전체에서 딱 1회만 로드해. 콜백 함수는 useRef나 useCallback으로 고정해서 재로딩 루프가 생기지 않게 해. 
- webcam canvas는 React가 건드리지 않는 빈 div에 mount하고, innerHTML을 직접 조작하지 마. 
- React 상태 업데이트는 100ms 정도로 throttle해. 매 프레임 setState 하지 마. 
- video에 muted, playsinline을 넣고 play() 같은 Promise는 전부 catch로 감싸. 효과음이나 AudioContext는 넣지 마. 
- 카메라 권한 거부 / 모델 URL 로드 실패 / HTTPS 아님, 이 3가지는 각각 한국어 안내 문구를 화면에 띄워줘. 그리고 에러가 나도 흰 화면이 되지 않게 ErrorBoundary를 넣어줘.
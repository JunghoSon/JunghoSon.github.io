---
layout: post
title: "브라우저 렌더링 방식"
date: 2025-06-04
---

# 브라우저는 HTML/CSS/JS를 어떻게 렌더링하나요?

브라우저는 HTML, CSS, JavaScript를 받아서 사용자가 볼 수 있는 화면으로 변환하기 위해 **렌더링 파이프라인**이라는 일련의 과정을 거칩니다.  
총 6단계로 구성되며, 이 흐름을 이해하면 성능 최적화의 포인트도 자연스럽게 파악할 수 있습니다.

---

## 🧩 브라우저 렌더링 파이프라인

1. **HTML 파싱 → DOM 트리 생성**

   - HTML을 파싱하며 노드 기반의 DOM 트리를 만듭니다.

2. **CSS 파싱 → CSSOM 트리 생성**

   - 내부 스타일 혹은 외부 CSS 파일을 파싱하여 CSSOM 트리를 구성합니다.

3. **DOM + CSSOM → 렌더 트리(Render Tree) 생성**

   - 실제 렌더링될 요소만 포함된 트리 생성 (`display: none` 요소 제외)

4. **레이아웃 (Layout, aka Reflow)**

   - 각 요소의 위치와 크기를 계산해 화면 좌표를 결정합니다.

5. **페인트(Paint)**

   - 시각적 스타일 (색상, 텍스트, 그림자 등)을 픽셀 단위로 그립니다.

6. **컴포지팅(Compositing)**
   - 여러 레이어를 GPU를 통해 합성해 최종 화면을 출력합니다.

---

## 💡 간단한 예시

```html
<body>
  <div style="color: red;">Hello</div>
</body>
```

- <div>를 통해 DOM 생성
- style 속성 파싱 → CSSOM 생성
- DOM + CSSOM → 렌더 트리 생성
- 레이아웃 계산 후 "Hello" 텍스트를 적절한 위치에 빨간색으로 페인트
- 최종적으로 GPU에서 화면에 표시

---

## 🔍 JavaScript의 역할

JavaScript는 DOM과 CSSOM을 동적으로 조작할 수 있습니다.

예를 들어 다음과 같은 코드:

```javascript
element.style.width = "100px";
```

이 경우 렌더 트리가 재계산되고, 레이아웃/페인트 과정이 다시 발생할 수 있습니다.
이는 성능에 영향을 줄 수 있으므로 조심해서 사용해야 합니다.

---

## ⚠️ 성능 최적화 관점에서의 렌더링 이해

렌더링 파이프라인의 각 단계는 비용이 다릅니다:

- Reflow (Layout): 가장 비용이 큼
- Paint: 중간 정도의 비용
- Composite: 상대적으로 저렴함

💡 성능 최적화 팁

- DOM 조작 최소화
- Layout thrashing 방지 (offsetHeight와 같은 레이아웃 속성 반복 접근 자제)
- transform, opacity를 활용해 페인트/레이아웃을 회피한 애니메이션 처리

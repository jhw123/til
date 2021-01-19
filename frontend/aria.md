# ARIA

## ARIA란?
시각적 장애를 가진 사용자가 웹 화면을 이해하기는 매우 어렵다.
시맨틱 마크업을 통해 화면 요소의 기능을 어느 정도 설명할 수는 있지만, 
구조가 복잡하거나 자바스크립트로 수시로 동적으로 변하는 뷰를 가진 요즘의 웹 트렌드에서 시맨틱 마크업을 구성하기는 쉽지 않다.
ARIA (Accessibility Rich Internet Applications)는 시맨틱 마크업 이외에 웹의 접근성을 높이기 위해 정의된 html 태그 속성 값들이다.
다만, 브라우저나 운영체제에 따라 ARIA를 지원하는 정도가 다르므로 공식 문서에서는 가능한한 지원 범위가 넓은 시맨틱 태그를 사용하도록 권장하고 있다.

ARIA에는 컴포넌트를 추가 설명하는 속성부터 두 컴포넌트 간 관계를 설명하는 등 다양한 속성들이 존재한다.
아래는 ARIA가 어떻게 사용되는지 보여주는 간단한 예제이다.
텍스트 표시없이 시각적으로만 진행률을 보여주는 ui의 경우, 스크린 리더기가 진행 상태값을 읽지 못하므로 시각적 장애가 있으면 현재 진행 상황을 알기 어렵다.
따라서 아래와 같이 현재 진행 상태 값을 속성에 추가해 ARIA를 지원하는 기기에서 사용자에게 현 상태값을 알려주도록 할 수 있다.

```html
<div id="percent-loaded" role="progressbar" aria-valuenow="75"
     aria-valuemin="0" aria-valuemax="100">
</div>
```

### 참고
- [ARIA - Accessibility | MDN](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)

## Labels: label, labelledby
`aria-label`은 컴포넌트를 설명할 수 있는 간단한 레이블을 추가할 수 있다.
예를 들어 아래와 같이 창을 닫는 X 버튼이 있을 때, 버튼의 텍스트만으로는 이 컴포넌트의 기능이 충분히 설명되지 않을 수 있다.
이럴 때 `aria-label`을 사용해 추가 설명하도록 한다.

```html
<button aria-label="close-popup">X</button>
```

`aria-labelledby`는 현재 컴포넌트를 설명하는 레이블 컴포넌트를 지정할 수 있다.
`<label>` 태그를 사용해도 비슷하게 접근성을 향상시킬 수 있는데,
레이블 컴포넌트와 레이블할 컴포넌트가 DOM 구조 상 많이 떨어져 있거나 부모-자식 관계가 아닐 경우나,
여러 컴포넌트들이 하나의 레이블을 바라봐야 할 때, `aria-labelledby`를 사용하면 유용하다.

```html
<div id="label">Name</div>
...
<input type="text" aria-labelledby="label"/>
```

### 참고
- [ARIA Labels and Relationships](https://developers.google.com/web/fundamentals/accessibility/semantics-aria/aria-labels-and-relationships)

## Relationships: owns, activedescendant, describedby, posinset & setsize
`aria-owns`는 두 컴포넌트의 부모-자식 관계를 나타낸다.
예를 들어 시각적으로는 하위 구성 요소로 보이나 DOM 구조 상으로는 아닐 때 `aria-owns`로 자식 컴포넌트가 누구인지 명시해줄 수 있다.

```html
<div aria-owns="child">Parent</div>
<div id="child">Child</div>
```
`aria-activedescendant`는 자식 요소 중 focus된 자식 컴포넌트를 명시한다.
예를 들어 `checked`를 사용하지 않고 시각적으로만 선택 상태를 나타내는 커스텀 라디오 버튼 그룹이 있을 경우,
장애를 가진 사용자는 현재 선택된 값을 알기가 힘들다.
이럴 때 해당 라디오 버튼 그룹를 감싸는 컴포넌트에 `aria-activedescendant`를 사용하여 현재 선택된 자식 컴포넌트를 명시할 수 있다.

```html
<div aria-activedescendant="2">
    <div id="1">option 1</div>
    <div id="2">option 2</div>
    <div id="3">option 3</div>
</div>
```

`aria-describedby`는 `aria-label`과 비슷하게 사용하지만, 정보의 중요성에서 차이가 있다.
`aria-label`이 해당 컴포넌트를 이해하는 데에 필요한 레이블을 다는 것이라면,
`aria-describedby`는 컴포넌트 사용 시 부가적으로 또는 특정 조건 하에 필요한 정보를 제공하는 기능이다.
예를 들어, 사용자가 패스워드 설정하는 와중에 패스워드 규칙이 어긋날 경우 규칙을 설명한다고 하자.
패스워드 규칙은 사용자가 규칙이 어긋난 패스워드를 입력했을 때에만 필요한 정보이고,
`aria-describedby`를 사용하므로써 정보의 중요도가 낮다는 걸 표시해 사용자가 패스워드를 입력할 때 입력값과 같이 더 중요한 정보를 놓치지 않게 할 수 있다.

```html
<input type="password" aria-describedby="rule"/>
<div id="rule">password should be at least 9 characters</div>
```

`aria-setsize`와 `aria-posinset`는 함께 쌍으로 사용되는데, 각각 데이터 그룹의 크기와 그룹 내에서의 위치를 나타낸다.
리스트의 길이가 길 때, 랜딩 속도를 빠르게 하기 위해 dynamic loading을 하는 경우가 있는데,
이럴 경우 스크린 리더기는 리스트 아이템이 다 로딩되지 않았음을 인지할 수 없고 사용자는 리스트 전체를 충분히 탐색하지 못할 수 있다.
이런 경우를 방지하기 위해 사전에 전체 리스트의 크기를 알리고 아직 몇개의 아이템이 더 남았는지 표시하여 리스트 탐색을 도울 수 있다.
[리스트 아이템이 모두 처음부터 존재하는 경우는 이 ARIA 기능이 필요하지 않다.](https://www.w3.org/TR/wai-aria-1.1/#aria-posinset)

```html
<div aria-setsize="2" aria-posinset="1">item 1</div>
<div aria-setsize="2" aria-posinset="2">item 1</div>
```

### 참고
- [ARIA Labels and Relationships](https://developers.google.com/web/fundamentals/accessibility/semantics-aria/aria-labels-and-relationships)


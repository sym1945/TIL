## Overview
- 정의된 style을 적용할 대상 HTML element를 특정하기 위한 **Selector**.
- javascript에서 DOM Select 시에도 같은 문법이 사용된다.

## 1. 전체 셀렉터 (Universal Selector)
|selector|Description|
|--|--|
|*|HTML 문서 내의 모든 요소를 선택한다. html 요소를 포함한 모든 요소가 선택된다. (head 요소도 포함된다)|

## 2. 태그 셀렉터 (Type Selector)
|selector|Description|
|--|--|
|TagName|지정된 태그명을 가지는 요소를 선택한다.|

## 3. ID 셀렉터 (ID Selector)
|selector|Description|
|--|--|
|#id|id 어트리뷰트 값을 지정하여 일치하는 요소를 선택한다. id 어트리뷰트 값은 중복될 수 없는 유일한 값이다.|

## 4. 클래스 셀렉터 (Class Selector)
|selector|Description|
|--|--|
|.class|class 어트리뷰트 값을 지정하여 일치하는 요소를 선택한다. class 어트리뷰트 값은 중복될 수 있다.|

## 5. 어트리뷰트 셀렉터 (Attribute Selector)
|selector|Description|
|--|--|
|selector[attribute]|지정된 어트리뷰트를 갖는 모든 요소를 선택한다.|
|selector[attribute="value"]|지정된 어트리뷰트를 가지며 지정된 값과 어트리뷰트의 값이 일치하는 모든 요소를 선택한다.|
|selector[attribute~="value"]|지정된 어트리뷰트의 값이 지정된 값을 (공백으로 분리된) 단어로 포함하는 요소를 선택한다.|
|selector[attribute\|="value"]|지정된 어트리뷰트의 값과 일치하거나 지정 어트리뷰트 값 뒤 연이은 하이픈(```value-```)으로 시작하는 요소를 선택한다.|
|selector[attribute^="value"]|지정된 어트리뷰트 값으로 시작하는 요소를 선택한다.|
|selector[attribute$="value"]|지정된 어트리뷰트 값으로 끝나는 요소를 선택한다.|
|selector[attribute*="value"]|지정된 어트리뷰트 값을 포함하는 요소를 선택한다.|

## 6. 복합 셀렉터 (Combinator)
### 6.1 후손 셀렉터 (Descendant Combinator)
|selector|Description|
|--|--|
|selectorA selectorB|셀렉터A의 모든 후손(하위) 요소 중 셀렉터B와 일치하는 요소를 선택한다.|
### 6.2 자식 셀렉터 (Child Combinator)
|selector|Description|
|--|--|
|selectorA>selectorB|셀렉터A의 모든 자식 요소 중 셀렉터B와 일치하는 요소를 선택한다.|
### 6.3 형제(동위) 셀렉터 (Sibling Combinator)
|selector|Description|
|--|--|
|selectorA+selectorB|셀렉터A의 형제 요소 중 셀렉터A 바로 뒤에 위치하는 셀렉터B 요소를 선택한다. A와 B 사이에 다른 요소가 존재하면 선택되지 않는다.|
|selectorA~selectorB|셀렉터A의 형제 요소 중 셀렉터A 뒤에 위치하는 셀렉터B 요소를 모두 선택한다.|

## 7. 가상 클래스 셀렉터 (Pseudo-Class Selector)
- 가상 클래스는 요소의 특정 상태에 따라 스타일을 정의할 때 사용된다.
- 특정 상태에는 원래 클래스가 존재하지 않지만 가상 클래스를 임의로 지정하여 선택하는 방법이다.
- 가상 클래스는 마침표(.) 대신 콜론(:)을 사용한다. CSS 표준에 의해 미리 정의된 이름이 있기 때문에 임의의 이름을 사용할 수 없다.
```css
selector:pseudo-class {
	//...
}
```
### 7.1 링크 셀렉터(Link pseudo-classes), 동적 셀렉터(User action pseudo-classes)
|pseudo-class|Description|
|--|--|
|:link|셀렉터가 방문하지 않은 링크일 때|
|:visited|셀렉터가 방문한 링크일 때|
|:hover|셀렉터에 마우스가 올라와 있을 때|
|:active|  셀렉터가 클릭된 상태일 때|
|:focus|  셀렉터에 포커스가 들어와 있을 때|
### 7.2 UI 요소 상태 셀렉터(UI element states pseudo-classes)
|pseudo-class|Description|
|--|--|
|:checked|셀렉터가 체크 상태일 때|
|:enabled|셀렉터가 사용 가능한 상태일 때|
|:disabled|셀렉터가 사용 불가능한 상태일 때|
### 7.3 구조 가상 클래스 셀렉터(Structural pseudo-classes)
|pseudo-class|Description|
|--|--|
|:first-child|셀렉터에 해당하는 모든 요소 중 첫번째 자식인 요소를 선택한다.|
|:last-child|셀렉터에 해당하는 모든 요소 중 마지막 자식인 요소를 선택한다.|
|:nth-child(n)|셀렉터에 해당하는 모든 요소 중 앞에서 n번째 자식인 요소를 선택한다.|
|:nth-last-child(n)|셀렉터에 해당하는 모든 요소 중 뒤에서 n번째 자식인 요소를 선택한다.|
|:first-of-type|셀렉터에 해당하는 요소의 부모 요소의 자식 요소 중 첫번째 등장하는 요소를 선택한다.|
|:last-of-type|셀렉터에 해당하는 요소의 부모 요소의 자식 요소 중 마지막에 등장하는 요소를 선택한다.|
|:nth-of-type(n)|셀렉터에 해당하는 요소의 부모 요소의 자식 요소 중 앞에서 n번째에 등장하는 요소를 선택한다.|
|:nth-last-of-type(n))|셀렉터에 해당하는 요소의 부모 요소의 자식 요소 중 뒤에서 n번째에 등장하는 요소를 선택한다.|
### 7.4 부정 셀렉터(Negation pseudo-class)
|pseudo-class|Description|
|--|--|
|:not(selector)|셀렉터에 해당하지 않는 모든 요소를 선택한다.|
### 7.5 정합성 체크 셀렉터(validity pseudo-class)
|pseudo-class|Description|
|--|--|
|:valid(셀렉터)|정합성 검증이 성공한 input 요소 또는 form 요소를 선택한다.|
|:invalid(셀렉터)|정합성 검증이 실패한 input 요소 또는 form 요소를 선택한다.|

## 8. 가상 요소 셀렉터 (Pseudo-Element Selector)
- 가상 요소는 요소의 특정 부분에 스타일을 적용하기 위하여 사용된다. 
```ex> 요소 콘텐츠의 첫글자 또는 첫줄, 요소 콘텐츠의 앞 또는 뒤```
- 가상 요소에는 두개의 콜론(::)을 사용한다. CSS 표준에 의해 미리 정의된 이름이 있기 때문에 임의의 이름을 사용할 수 없다.
```css
selector::pseudo-element {
	//...
}
```
|pseudo-element|Description|
|--|--|
|::first-letter|콘텐츠의 첫글자를 선택한다.|
|::first-line|콘텐츠의 첫줄을 선택한다. 블록 요소에만 적용할 수 있다.|
|::after|콘텐츠의 뒤에 위치하는 공간을 선택한다. 일반적으로 content 어트리뷰트와 함께 사용된다.|
|::before|콘텐츠의 앞에 위치하는 공간을 선택한다. 일반적으로 content 어트리뷰트와 함께 사용된다.|
|::selection|드래그한 콘텐츠를 선택한다. iOS Safari 등 일부 브라우저에서 동작 않는다.|


## Reference
[CSS Selectors Reference](https://www.w3schools.com/cssref/css_selectors.asp)<br/>
[CSS3 Selector | PoiemaWeb](https://poiemaweb.com/css3-selector)

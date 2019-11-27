# Overview
- ```Tab```키로 focus를 이동시킬 수 있는 _HTML Element_ 를 정리.
- 이하 **Focusable Element**로 칭함.
# Focusable Elements
### 1. input
### 2. select
### 3. textarea
### 4. button
### 5. a (anchor)
- 조건
   <br/>1\) ```href``` attribute가 있어야 함.
   <br/>2\) ```href``` attribute가 없을경우 ```tabindex``` attribute가 ```0``` 이상으로 설정돼 있으면 됨.
### 5. object
- 조건
   <br/>1\) ```data``` attibute가 있어야 함.

### 7. area
- 조건
   <br/>1\) ```href``` attribute가 있어야 함.
   <br/>2\) ```map``` element 안에 있어야 함.
   <br/>3\) ```image``` element가 ```map```을 참조해야 함.
   
# Disable Focusable
위에 정의된 element 모두 ```tabindex```를 _음수(negative number)_ 로 설정하면 된다.

> ```0``` 이상의 ```tabindex```는 숫자가 작을수록 우선순위가 높다.

# Reference
[:focusable Selector | jQuery UI API Documentation](https://api.jqueryui.com/focusable-selector/)

## Regular Expression
```java
/\B(?=(\d{3})+(?!\d))/g
```

## Analysis
/\B(?=```(\d{3})+```(?!\d))/g
: 연속된 숫자 3개의 묶음(이하 **A묶음**)을 1개 이상 찾는다.
<br/>

/\B(?=```(\d{3})+(?!\d))```/g
: **A묶음**을 1개 이상 찾는데, 뒤에 숫자가 나오지 않을때까지 찾는다. (이하 **B묶음**)
<br/>

/```\B(?=(\d{3})+(?!\d))```/g
: **B묶음** 앞에 문자의 경계를 찾는다. (이하 **C경계**)
<br/>

```/\B(?=(\d{3})+(?!\d))/g```
: **C경계**를 _전역_ 에서 찾는다.

## Example
```javascript
"1234567891234567".replace(/\B(?=(\d{3})+(?!\d))/, ',');

>> "1,234567891234567"
```
(234)(567)(891)(234)(567)
: ( ) = A묶음
<br/>

[(234)(567)(891)(234)(567)]
: [ ] = B묶음
<br/>

1|[(234)(567)(891)(234)(567)]
: | = C경계

C경계(blank)를 comma(' , ')로 replace 했으므로  위와 같은 결과가 출력된다.

위 과정을 g 옵션을 부여해 전역에서 작업한다고 하면 아래의 순서로 콤마가 찍힌다.
```javascript
"1,234567891234567".replace(/\B(?=(\d{3})+(?!\d))/, ',');

>> "1,234,567891234567"
```
```javascript
"1,234,567891234567".replace(/\B(?=(\d{3})+(?!\d))/, ',');

>> "1,234,567,891234567"
```
```javascript
"1,234,567,891234567".replace(/\B(?=(\d{3})+(?!\d))/, ',');

>> "1,234,567,891,234567"
```
```javascript
"1,234,567,891234567".replace(/\B(?=(\d{3})+(?!\d))/, ',');

>> "1,234,567,891,234,567"
```

## Use
```javascript
function numberWithCommas(x) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
}
```
위의 방식은 dot(.)이 붙은 숫자(ex. 소수점 이하 4자리 이상)에서 원하지 않는 결과를 반환한다.
아래는 이를 개선한 방식.
```javascript
function numberWithCommas(x) {
    var parts = x.toString().split(".");
    parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    return parts.join(".");
}
```

## Reference
[formatting - How to print a number with commas as thousands separators in JavaScript - Stack Overflow](https://stackoverflow.com/questions/2901102/how-to-print-a-number-with-commas-as-thousands-separators-in-javascript?page=1&tab=votes#tab-top)

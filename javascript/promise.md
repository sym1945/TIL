
# Description
- A라는 작업이 끝나고 이어서 B라는 작업을 진행하고 싶을 때 유용하다.
- A라는 작업이 비동기 작업이어도 동기 작업처럼 리턴값을 받아 다음 작업을 진행할 수 있다.
- **then**을 호출하여 작업을 대기열에 올리고 작업 결과에 따라 **resolve**, **reject**를 호출하여 다음 작업을 이행, 거부할 수 있다.
- 반환값은 **promise**로 다시 **then**을 호출하여 다음 작업을 수행할 수 있다. (promise chain)
- **reject**된 작업은 **catch**를 호출하여 받을 수 있다.

# Basic
```javascript
function cancelReservation(id) {
  return new Promise((resolve, reject) => {
    const xhr = new  XMLHttpRequest();
    xhr.addEventListener('load', () => {
      if (xhr.readyState === XMLHttpRequest.DONE) {
        if (xhr.status === 200) {
          resolve(JSON.parse(xhr.responseText));
        } else {
          reject(xhr.responseText);
        }
      }
    });
    xhr.open('PUT', `/reservation/api/reservations/${id}`);
    xhr.send();
  });
}


cancelReservation(1)
.then((result) => {
  // ajax 응답이 정상일 경우 진행할 작업
})
.catch((result) => {
  // ajax 응답이 비정상일 경우 진행할 작업
  alert('예약 취소 실패');
});
```
# Error propagation
```javascript
doSomething()
.then(result => doSomethingElse(result))
.then(newResult => doThirdThing(newResult))
.then(finalResult => console.log(`Got the final result: ${finalResult}`))
.catch(failureCallback);
```
기본적으로 promise chain은 예외가 발생하면 멈추고 chain의 아래에서 catch를 찾는다.<br/>
아래는 위 코드가 어떻게 동작하는지 모델링 한 것.
```javascript
try {
  const result = syncDoSomething();
  const newResult = syncDoSomethingElse(result);
  const finalResult = syncDoThirdThing(newResult);
  console.log(`Got the final result: ${finalResult}`);
} catch(error) {
  failureCallback(error);
}
```
# Common mistakes
```javascript
// Bad example! Spot 3 mistakes!

doSomething().then(function(result) {
  doSomethingElse(result) // Forgot to return promise from inner chain + unnecessary nesting
  .then(newResult => doThirdThing(newResult));
}).then(() => doFourthThing());
// Forgot to terminate chain with a catch!
```

 1. 제대로 체인을 하지 않는 것.
 2. 불필요하게 중첩되어 첫 번째 실수를 가능하는 만드는 것.
 3. **catch**로 체인을 종료하지 않는 것.

# Reference
[Promise - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)

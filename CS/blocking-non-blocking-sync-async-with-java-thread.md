# Blocking VS Non-Blocking & Synchronous VS Asynchronous

</br>

```
동기/비동기, 블럭/넌블럭 지금까지 지나가며 많이 들어본 말들이다.

Synchronous은 Blocking과 같고 Asynchronous는 Non-Blocking이랑 같은거 아니야?라고 착각하기 쉽다.

이 4가지 개념이 어떤 의미를 가지는지 정리해보자.
```

</br>

먼저 4가지 조합에 대한 2대2 매트릭스 사진이다.
사진을 먼저 보고 아래 내용을 읽어보자.

![](https://camo.githubusercontent.com/77f436abebf3affc528769a64321895de40ae78565a5416cda2dfdb880c4904b/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d6874747073253341253246253246626c6f672e6b616b616f63646e2e6e6574253246646e25324664613530597a2532466274713044736a65345a562532466c47653848386e5a676442646746766f3749637a5330253246696d672e706e67)

</br>

망나니 개발자님이 Github에 정리해주신 [게시물](https://github.com/MangKyu/tech-interview-for-developer/blob/master/Computer%20Science/Network/%5BNetwork%5D%20Blocking%2CNon-blocking%20%26%20Synchronous%2CAsynchronous.md)에 실려 있는 이미지다. 원본 출처는 [HomoEfficio님의 게시물](http://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/) 이다.

</br>

## Blocking VS Non-Blocking - 제어권의 관점

블럭/논블럭은 간단히 말해서 호출된 함수가 호출한 함수에게 **제어권**을 건네주는 유무의 차이라고 볼 수 있다.

함수 A, B가 있고, A 안에서 B를 호출했다고 가정해보자. 이때 호출한 함수는 A고, 호출된 함수는 B가 된다. 현재 B가 호출되면서 B는 자신의 일을 진행해야 한다. </br>(제어권이 B에게 주어진 상황)

### Blocking

- 함수 B는 내 할 일을 다 마칠 때까지 제어권을 가지고 있는다. A는 B가 다 마칠 때까지 기다려야 한다.

### Non-blocking

- 함수 B는 할 일을 마치지 않았어도 A에게 제어권을 바로 넘겨준다. A는 B를 기다리면서도 다른 일을 진행할 수 있다.

</br>

즉, 호출된 함수에서 일을 시작할 때 바로 제어권을 리턴해주느냐, 할 일을 마치고 리턴해주느냐에 따라 블럭과 논블럭으로 나누어진다고 볼 수 있다.

</br>

## Synchronous VS Asynchronous - 동시성의 관점

동기/비동기는 일을 수행 중인 **동시성**에 주목하자

아까처럼 함수 A와 B라고 똑같이 생각했을 때, B의 **수행 결과나 종료 상태를 A가 신경쓰고 있는 유무**의 차이라고 생각하면 된다.

### Synchronous

- 함수 A는 함수 B가 일을 하는 중에 기다리면서, 현재 상태가 어떤지 계속 체크한다.

### Asynchronous

- 함수 B의 수행 상태를 B 혼자 직접 신경쓰면서 처리한다. (Callback)

</br>

즉, 호출된 함수(B)를 호출한 함수(A)가 신경쓰는지, 호출된 함수(B) 스스로 신경쓰는지를 동기/비동기라고 생각하면 된다.

비동기는 호출시 Callback을 전달하여 작업의 완료 여부를 호출한 함수에게 답하게 된다. (Callback이 오기 전까지 호출한 함수는 신경쓰지 않고 다른 일을 할 수 있음)

</br>

## 예시 상황

</br>

```
상황 : 피자를 포장하러 옴
```

</br>

### 1) Blocking & Synchronous

```
나 : 사장님 불고기피자 한 판 포장해주세요
사장님 : 네 금방 나옵니다. 잠시만요!
나 : 네~
-- 사장님 피자 굽는 중 --
나 : (과정 지켜봄...궁금함...아무것도 안하고 계속 서서 기다림)
```

</br>

### 2) Blocking & Asynchronous

```
나 : 사장님 불고기피자 한 판 포장해주세요
사장님 : 네 금방 나옵니다. 잠시만요!
나 : 네~
-- 사장님 피자 굽는 중 --
나 : (안 궁금함. 그냥 딴생각..못 가고 계속 서있음)
```

</br>

### 3) Non-blocking & Synchronous

```
나 : 사장님 불고기피자 한 판 포장해주세요
사장님 : 네~ 주문 밀려서 시간 좀 걸리니까 볼일 보시다 오세요
나 : 네~
-- 사장님 피자 굽는 중 --
(5분뒤) 나 : 피자 나왔나요?
사장님 : 아직요.
(10분뒤) 나 : 피자 나왔나요?
사장님 : 아직요.
(15분뒤) 나 : 피자 나왔나요?
사장님 : 아직이요~!!ㅠㅠ
```

</br>

### 4) Non-blocking & Asynchronous

```
나 : 사장님 불고기피자 한 판 포장해주세요
사장님 : 네~ 주문 밀려서 시간 좀 걸리니까 볼일 보시다 오세요
나 : 네~
-- 사장님 피자 굽는 중 --
나 : (앉아서 다른 일 하는 중)
...
사장님 : 주문하신 피자 나왔습니다
나 : 잘먹겠습니다~
```

</br>

## Reference

- [https://github.com/MangKyu/tech-interview-for-developer/blob/master/Computer%20Science/Network/%5BNetwork%5D%20Blocking%2CNon-blocking%20%26%20Synchronous%2CAsynchronous.md](https://github.com/MangKyu/tech-interview-for-developer/blob/master/Computer%20Science/Network/%5BNetwork%5D%20Blocking%2CNon-blocking%20%26%20Synchronous%2CAsynchronous.md)
- [https://musma.github.io/2019/04/17/blocking-and-synchronous.html](https://musma.github.io/2019/04/17/blocking-and-synchronous.html)

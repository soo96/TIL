# Apache Tomcat 요청 관리

## 1. 스레드 풀과 역할

### Acceptor 스레드

- 새로운 클라이언트 연결을 받아들이는 점담 스레드이다.

### Poller 스레드 (NIO/NIO2 방식)

- Selector 기반으로 여러 소켓 채널의 I/O 이벤트를 감지한다.
- 데이터가 준비된 연결을 작업(Worker) 스레드에 전달한다.

### 작업(Worker) 스레드

- 실제 HTTP 요청을 처리하고 서블릿을 실행하는 스레드들이며, `maxThreads` 속성에 의해 최대 개수가 제한된다.

## 2. Non-Blocking 동작 원리

NIO(Http11NioProtocol) 또는 NIO2 커넥터의 경우, 네트워크 I/O 계층은 Non-Blocking 방식으로 동작한다.

- 연결이 들어오면 Acceptor 스레드가 이를 수락한다.
- Poller 스레드는 여러 연결 중 데이터가 준비된 소켓 이벤트를 감지한다.
- 요청 데이터(Request Headers 등)가 실제로 도착한 경우에만 작업 스레드에 할당되어 처리가 시작되므로, 작업 스레드가 무의미하게 대기하는 것을 방지한다.

## 3. 단계별 대기열(Queue) 관리 흐름

동시 요청이 많아질 때 요청이 처리되는 단계별 흐름은 다음과 같다.

1. 작업 스레드 할당 (maxThreads)

- 비동기 요청이 아닌 경우, 각 요청은 처리가 끝날 때까지 하나의 스레드를 점유한다.
- 동시에 처리 가능한 요청 수는 `maxThreads`로 제한된다.

2. 커넥터 연결 제한 (maxConnections)

- Tomcat이 동시에 유지할 수 있는 TCP 연결의 최대 개수이다 (Keep-Alive 연결 포함).
- 이 수치가 `maxConnections`에 도달할 때까지 연결을 수락은 하되 처리는 대기한다.

3. 운영체제 연결 대기열 (acceptCount)

- `maxConnections`마저 가득 차면, 요청을 운영체제(OS)가 제공하는 연결 큐로 넘어간다.
- 이 큐의 키기는 `acceptCount`이다.

4. 연결 거절

- OS 대기열까지 가득 차면 추가적인 연결 요청은 거절(Refused)되거나 타임아웃이 발생한다.

## References

- [https://tomcat.apache.org/tomcat-9.0-doc/config/http.html](https://tomcat.apache.org/tomcat-9.0-doc/config/http.html)

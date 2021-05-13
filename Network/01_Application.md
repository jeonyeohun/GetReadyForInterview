# Application Layer

## HTTP

### HTTP는 statless 하다는 것이 어떤 의미인지 설명해보세요.

- HTTP 는 stateles protocol로 서버가 클라이언트의 상태정보를 저장하지 않는 것을 의미합니다.

### HTTP/1.1 에서 업데이트된 주요 기능들에 대해서 설명해보세요.

- HTTP/1.1 에서는 `Keep Alive` 가 기본설정이 되고 `파이프라이닝`이 추가되었습니다. Keep Alive 는 TCP 연결을 특정 시간동안 유지시켜 새로운 요청이 시작될 때 새로운 연결을 설정할 필요없이 사용하던 TCP 연결을 사용하도록 하는 기능입니다. 파이프라이닝은 클라이언트가 요청을 보내고 응답을 받기 전에 다음 요청을 전송할 수 있도록 하는 기능입니다. 

### HTTP/1.1 에서 파이프라이닝을 사용할 때 발생하는 문제는 없을까요?

- 파이프라이닝을 사용하면 `Head of Line Blocking` 문제가 발생합니다. 서버는 클라이언트로부터 연속된 요청을 파이프라이닝을 통해 받았을 때, 순차적으로 처리하기 때문에 첫번째 요청에 대한 처리가 끝나야 다음 요청을 처리할 수 있습니다. 이는 TCP는 응답 순서를 보장해야하기 때문에 그렇습니다. 따라서 만약 첫 요청의 처리시간이 길어지면 쌓여있는 요청들은 첫요청이 끝날때까지 기다려야하는 현상이 발생합니다.

### HTTP/2.0 에서 업데이트된 주요 기능들을 설명해보세요.

- HTTP/2.0 은 응답/요청 메세지를 Plain Text 로 송수신하지 않고 바이너리로 인코딩된 Frame 을 사용합니다. 또한 클라이언트가 요청하지 않았지만 필요로 할 리소스를 송신하는 `Server Push` 도 추가되었습니다. 마지막으로 HTTP/1.1 에서 파이프라이닝의 문제점으로 지적되었던 Head of Line Blocking 문제를 frame 과 stream을 사용하여 Multiplexing 을 지원하는 것으로 해결했습니다. 

### HTTP 메서드들과 의미를 설명해보세요.

- HTTP 메세지는 GET, POST, HEAD, PUT, OPTIONS, PATCH, CONNECT, DELETE 가 있습니다.
- `GET` 방식은 서버로부터 리소스를 읽어오기 위해 사용합니다.
- `POST` 방식은 서버에 데이터를 추가하기 위해 사용합니다.
- `HEAD` 방식은 서버에 리소스를 요청하고 응답 body를 제외하고 응답코드와 헤더정보만 응답으로 받기 위해 사용합니다.
- `PUT` 방식은 서버 리소스를 업데이트 하기 위해 사용합니다.
- `OPTIONS` 방식은 클라이언트가 서버가 지원하는 메서드를 확인하기 위해 사용합니다.
- `PATCH` 방식은 서버 리소스의 일부를 업데이트 하기 위해 사용합니다.
- `CONNECT` 방식은 클라이언트가 HTTP 프록시 서버를 통해 서버와 연결을 요청하기 위해 사용합니다.
- `DELETE` 방식은 서버의 리소스를 삭제하기 위해 사용합니다.

### 프록시 서버에 대해서 설명해보세요

- 프록시 서버는 클라이언트와 서버 사이에 징검다리 역할을 합니다. 프록시 서버는 클라이언트가 프록시 서버에 보냈던 요청을 캐싱해두었다가 캐싱된 리소스에 대한 요청을 들어오면 실제 서버로 요청을 보내지 않고 자신이 가진 리소스를 응답해줍니다. 또한 클라이언트와 서버가 직접 통신하지 않고 경유하기 때문에 보안에도 이점이 있습니다.

### 그럼 프록시 서버는 캐시 정보가 최신데이터인지 어떻게 확인할까요?

- 프록시 서버는 실제 서버에 조건부 GET을 사용해서 마지막으로 수정된 시간을 확인합니다. 프록시 서버는 본 서버에 `If-Modified-Since` 헤더에 자신이 가진 리소스의 수정시간을 포함시켜 요청을 보냅니다. 본 서버는 이 헤더에 있는 시간과 자신이 가진 리소스의 시간을 확인하여 시간이 같다면 응답 상태 메세지만 송신하고, 다르다면 최신 리소스를 담아 보냅니다. 프록시 서버는 이 응답을 확인하고 자신의 리소스를 클라이언트로 송신하거나, 새로 받은 리소스를 업데이트 한 뒤에 클라이언트로 송신합니다.

### HTTP 상태코드를 앞번호 기준으로 분류해서 설명해보세요.

- 100번대 코드는 `조건부 응답` 코드로 서버가 요청을 처리했고 추가적인 작업을 수행한다는 것을 클라이언트에게 알립니다.
- 200번대 코드는 `성공` 코드로 클라이언트의 요청을 성공적으로 처리했다는 것을 알립니다.
- 300번대 코드는 `리다이렉트` 코드로 클라이언트가 응답을 받고 추가적인 동작을 수행해야한다는 것을 알립니다.
- 400번대 코드는 `요청오류` 코드로 클라이언트가 서버에 보낸 요청이 잘못되었음을 알립니다.
- 500번대 코드는 `서버오류` 코드로 서버 측 문제로 인해 요청을 처리하지 못했음을 알립니다.

## DNS

### 브라우저에 웹 URL을 입력했을 때 DNS의 동작과정을 설명해보세요


## DHCP

## SMTP
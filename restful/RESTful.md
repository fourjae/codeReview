## RESTful 하게 통신하기

REST(Representational State Transfer)는 웹 서비스의 설계 모델이나 아키텍처 스타일로 볼 수 있다.
REST는 자원 중심의 설계 원칙을 따르면서, 표준 프로토콜과 규칙을 통해 시스템 간의 상호 작용을 단순화한다.

- **자원** (Resource):
    - 모든 것은 자원으로 간주된다. 각 자원은 유일하게 URI (Uniform Resource Identifier)로 식별한다.

- **행위** (Verb):
    - HTTP 메소드를 사용하여 자원에 대한 행위를 나타낸다.

- **표현** (Representations):
    - 클라이언트와 서버 간의 통신에서 자원의 상태는 "표현(JSON, XML 등)"을 통해 전달한다.

    - 클라이언트는 서버에 자원의 특정 표현을 요청할 수 있고, 서버는 해당 표현을 사용하여 응답한다.
    
    - REST는 상태를 유지하지 않는 (stateless) 통신을 진행한다. 
    
        - 각 요청이 서버에 필요한 모든 정보를 포함 해야 한다.
    
        - 서버는 이전 요청의 상태를 기억하지 않는다.

웹 서비스를 RESTful하게 통신한 다는 것은 REST 아키텍처와 규격에 맞추어 통신한다는 것을 의미한다.

--- 

### 1. HTML FORM은 GET, POST만 지원하고 다른 자원을 사용할 수 없을 때

AJAX 같은 비동기 API 통신을 하면 여러 메서드(GET, POST, DELETE, PUT, PATCH, HEAD 등) 모두 사용 가능하지만, 순수 HTML FORM을 사용해야 할시 혹은 보안 문제로 사용이 불가할시 최대한 restful하게 맞춰준다.

### {GET} - 주로 Read에 사용
- 회원 목록 /members
- 회원 등록 폼 /members/new
    - (여기서 회원 정보를 입력)
- 회원 조회 /members/{id}
    - (회원 상세에 대한 HTML) 
- 회원 수정 폼 /members/{id}/edit

### {POST} - 주로 Create에 사용
- 회원 등록 /members/new, /members
    - (회원 정보 등록 버튼을 누를 때)

### 그 외, {POST} - 다른 Method를 사용하지 못할 때
- 회원 수정 /members/{id}/edit → /members/{id}/edit/update 
- /members/{id} → /members/{id}/update
POST (PUT을 사용하지 못하는 것을 가정할 때, 어쩔 수 없이 /PUT 컨트롤 URI로 작성)

- 회원 삭제 /members/{id}/delete
POST (DELETE를 못쓰는 것을 가정했을 때, 어쩔 수 없이 /delete 컨트롤 URI로 작성)

## Repository

**Repository**는 DB Layer에 접근하기 위한 추상화 인터페이스이다.

Spring Data JPA, MyBatis, NoSQL, inMemory등 다양한 데이터 저장소와 상호 작용이 가능하다.

Repository를 추상화 하여 처리하는 이유는 위와 같이 다른 저장소로 갈아 끼울 때나 쿼리 및 비즈니스 로직 처리시 영향을 최소화 하기 위해,
테스트 코드(Mock 객체의 프록시를 활용한 통합 테스트), DI(Dependency Inversion)를 지키기 위해서이다.

Repository를 통해 값을 넘겨주어 DB와 상호작용한다.

다음 규칙을 적절한 상황, 팀 컨벤션에 따라 잘 적용하자.
---

1. 만약, DB의 값에서 반환해야 할 때, Exception이 발생해야 할 경우 Optional을 사용해 처리한다.

```Java
public List<User> getAllUsers() {
    List<User> users = userRepository.findAll();
    if (users.size() == 0) {
        throw new NoDataFoundException("No users found in the database.");
    }
    return users;
}
```

```Java
public List<User> getAllUsers() {
    return userRepository.findAll().orElseThrow(
        () -> throw new NoDataFoundException("No users found in the database.");
    )
}
```

---

2. Create, Update, Delete 작업의 경우 굳이 return을 진행할 필요 없다.
    - 그러나 이러한 부분은 클라이언트와 의논 후 최선의 방법으로 진행하는 것이 좋다. 아래는 예시이다.
        - return true, return false 필요 -> 예외 철ㄹ
        - return Key 혹은 PK 필요 -> 유저 객체 리턴


void 리턴시

```Java
public void createUserInfo(UserRequest request) {
    //Create 후 return 없음
    userRepository.save(request);
}
```

PK 리턴시 
```Java
@Transactional
public Long update(UserRequest request) {
    User existingUser = userRepository.findById(request.getId()).orElseThrow(() -> new EntityNotFoundException("User not found with id: " + request.getId()));

    existingUser.setName(request.getName());

    return existingUser.getId();  // 업데이트된 유저 ID 반환
}
```


---

2. 유저 정보를 가져오는 작업이 비즈니스 로직 내에서만 활용된다면, 메서드명에 'get' 접두어를 붙일 필요는 없다.

반환 값이 없는 메서드의 경우, 예를 들어 'getMemberInfo' 대신 'memberInfo'로 표현하는 것이 더 적합하다고 볼 수 있다.
    - getMemberInfo -> memberInfo

메서드가 'validateCheck' 와 같이 특정 동작을 수행하기 위함일 수 있기 때문이므로, 명칭에서 해당 동작의 본질을 직접적으로 반영하는 것이 바람직하다.


```Java
```
## Repository

**Repository**는 DB Layer에 접근하기 위한 추상화 인터페이스이다.

Spring Data JPA, MyBatis, NoSQL, inMemory등 다양한 데이터 저장소와 상호 작용이 가능하다.

Repository를 추상화 하여 처리하는 이유는 위와 같이 다른 저장소로 갈아 끼울 때나 쿼리 및 비즈니스 로직 처리시 영향을 최소화 하기 위해,
테스트 코드(Mock 객체의 프록시를 활용한 통합 테스트), DI(Dependency Inversion)를 지키기 위해서이다.

Repository를 통해 값을 넘겨주어 DB와 상호작용한다.

다음 규칙을 적절한 상황, 팀 컨벤션에 따라 잘 적용하자.
---

1. 만약, Exception이 발생해야 할 경우 Optional을 사용해 처리한다.

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



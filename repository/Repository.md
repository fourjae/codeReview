## Repository

**Repository**는 DB Layer에 접근하기 위한 추상화 인터페이스이다.

Spring Data JPA, MyBatis, NoSQL, inMemory등 다양한 데이터 저장소와 상호 작용이 가능하다.

Repository를 통해 DAO(Data Access Object) 값을 넘겨주 DB와 통신한다.

다음 규칙을 잘 적용하자.
---
1. 만약, Exception이 발생해야할 경우 Optional을 사용해 처리한다.

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

2.


## ErrorCode

**ErrorCode**는 클라이언트에게 어떠한 에러가 발생했는지 알려주기 위해 만들어둔 에러 코드 집합소이다.

유용하고 빠르게 어떤 에러인지 알기 위해 ERROR 코드는 명확하게 작성한다.

---

```Java
public class ErrorCode(){

    private ErrorCodeMessage(String code, String message) {
        this.code=code;
        this.message=message;
    }

    ERROR_1404(1404, "User not found")  // Not Recommend
    USER_NOT_FOUND(1404, "User not found") // Recommend

    public String code() {
        return code;
    }

    public String message() {
        return message;
    }
}

```


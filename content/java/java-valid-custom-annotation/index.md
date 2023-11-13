---
emoji: 🐌
title: 커스텀 어노테이션으로 회원가입 유효성 검사하기
date: '2023-11-13 00:00:00'
author: 화나
tags: 자바
categories: 자바
---

## @Valid

자바에서 사용되는 어노테이션 중 하나로, HTTP 요청 데이터를 객체로 반환할 때 유효성 검사를 수행하는 어노테이션이다. 

### **LocalValidatorFactoryBean**

스프링에서 제공하는 Validator 인터페이스의 구현체 중 하나이다. 스프링 어플리케이션 컨텍스트 내에서 사용되며 @Valid 어노테이션을 통한 데이터 유효성 검사를 처리한다. 스프링에서는 validation 의존성만 추가해주면 자동으로 빈 등록이 되어 사용할 수 있다.

### 사용법

```java
public class SignupRequestDto {
	@NotBlank
	private String username;
	@NotBlank
	private String password;
}

@PostMapping("/user/signup")
	public ResponseEntity<Void> signup(@Valid @RequestBody SignupRequestDto requestDto) {
		return ResponseEntity.ok().build();
}
```

유효성을 검증할 데이터를 받아올 DTO를 생성하고, 각 필드에 검증하고 싶은 유효성 어노테이션을 붙여준다. 해당 DTO에 @Valid 어노테이션을 붙이면 유효성 검증을 할 수 있다.

### 검증 실패

검증에 실패할 경우 MethodArgumentNotValidException 예외를 던지게 된다.

## 커스텀 어노테이션

### 회원가입 데이터의 조건

- 사용자 이름 : 최소 4자 이상, 10자 이하이며 알파벳 소문자, 숫자로 구성되어야 한다.
- 비밀번호 : 최소 8자 이상, 15자 이하이며 알파벳 대소문자, 숫자로 구성되어야 한다.

### SignupRequestDto

```java
@Getter
@NoArgsConstructor
@AllArgsConstructor
public class SignupRequestDto {
	@Username
	private String username;
	@Password
	private String password;
}
```

### @Username, @Password

```java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = UsernameValidator.class)
@Documented
public @interface Username {

	String message() default "사용자 이름은 최소 4자 이상, 10자 이하이며 알파벳 소문자, 숫자로 구성되어야 합니다.";

	Class<?>[] groups() default { };

	Class<? extends Payload>[] payload() default { };

}

@Documented
@Constraint(validatedBy = PasswordValidator.class)
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Password {

		String message() default "비밀번호는 최소 8자 이상, 15자 이하이며 알파벳 대, 소문자, 숫자로 구성되어야 합니다.";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};
}
```

- @Target(ElementType.FIELD)
    - 이 어노테이션을 필드에만 적용할 수 있도록 지정
- @Retention(RetentionPolicy.RUNTIME)
    - 런타임시에도 이 어노테이션 정보를 유지할 수 있도록 저장
- @Constraint(validatedBy = UsernameValidator.class)
    - 이 어노테이션의 실제 유효성 검사를 수행하는 클래스를 지정
- @Documented
    - 이 어노테이션의 정보가 Javadoc에 포함되도록 지정
- String message() default
    - 어노테이션이 유효성 검사에 실패할 경우 사용할 디폴트 에러 메시지를 정의
- Class<?>[] groups() default {};
    - Bean Validation에서 그룹화를 지원하기 위한 설정, 사용하지 않으면 비어있는 배열 사용
- Class<? extends Payload>[] payload() default {};
    - 추가적인 정보를 전달할 때 사용, 사용하지 않으면 비어있는 배열 사용

### UsernameValidator, PasswordValidator

```java
@Component
public class UsernameValidator implements ConstraintValidator<Username, String> {

    private static final int MIN_SIZE = 4;
    private static final int MAX_SIZE = 10;
    private static final String regexUsername = "^[a-z0-9]+[a-z0-9]{" + MIN_SIZE + "," + MAX_SIZE + "}$";

    @Override
    public boolean isValid(String username, ConstraintValidatorContext context) {
        boolean isValidUsername = username.matches(regexUsername);
        if (!isValidUsername) {
            context.disableDefaultConstraintViolation();
            context.buildConstraintViolationWithTemplate(
                    MessageFormat.format("{0}자 이상의 {1}자 이하의 숫자, 영문자를 포함한 이름을 입력해주세요.",
                            MIN_SIZE,
                            MAX_SIZE)).addConstraintViolation();
        }
        return isValidUsername;
    }
}

@Component
public class PasswordValidator implements ConstraintValidator<Password, String> {

    private static final int MIN_SIZE = 8;
    private static final int MAX_SIZE = 15;
    private static final String regexPassword = "^(?=.*[A-Za-z])(?=.*[0-9])[A-Za-z0-9]{" + MIN_SIZE + "," + MAX_SIZE + "}$";

    @Override
    public boolean isValid(String password, ConstraintValidatorContext context) {
        boolean isValidPassword = password.matches(regexPassword);
        if (!isValidPassword) {
            context.disableDefaultConstraintViolation();
            context.buildConstraintViolationWithTemplate(
                            MessageFormat.format("{0}자 이상의 {1}자 이하의 숫자, 대,소문자를 포함한 비밀번호를 입력해주세요", MIN_SIZE, MAX_SIZE))
                    .addConstraintViolation();
        }
        return isValidPassword;
    }
}
```

- isValid() : 유효성 검사를 진행하는 로직 작성
- context.disableDefaultConstraintViolation() : 기본 에러 메시지를 사용하지 않도록 설정
- context.buildConstraintViolationWithTemplate() : 사용자 정의 에러 메시지를 추가
- addConstraintViolation() : 에러가 발생하게 되면 에러 정보를 ConstraintValidatorContext에 추가하여 클라이언트에게 전달한다. 전달된 정보는 에러 메시지를 생성하는데 사용된다.

[참고1](https://mangkyu.tistory.com/206)
[참고2](https://velog.io/@livenow/Java-%EC%BB%A4%EC%8A%A4%ED%85%80-%EC%95%A0%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98%EC%9C%BC%EB%A1%9C-Password%EA%B7%9C%EC%B9%99-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)

```toc

```

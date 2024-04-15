---
title: Snorose_Signin
author: s
date: 2024-04-13
categories: [Snorose, Spring]
published : false
---

# 스노로즈 회원가입 기능 

1. 우선 간단한 회원가입 기능은 실행했음
2. 앞으로 만들어야 하는 기능들
   - [ ] basetime entity랑 합치기 -> X 추후에 필요하면 합치기
   - [x] 아이디, 이메일, 학번 같으면 안됨
   - [x] 비밀번호 암호화
   - [x] 만들어진거랑 연결
   - [x] 비밀번호 두번 확인 기능
   - [ ] 잘못 쓰면 아래에 빨간 글씨 나타나게->프론트?
   - [ ] 스크랩 기능, 내가 쓴글 댓글 어케할건지 -> 유저 엔티티, 스크랩 엔티티만들어야할듯
   - [ ] 리팩토링
   - [ ] unique 키 처리 할 것인가? (디비 값들)
   - [ ] 예외 처리(숫자만, 특수문자 등)
   - [ ] 로그인과 합체


- [ ] set 메소드는 사용 의도를 파악하기 힘들고, 무분별하게 쓸 수 있어서 지양하는 것 같아
userRegisterRequestDto.toEntity()에서 다 처리하도록!
  - [ ] DB 수정
  - [ ] userService 수정 
    - [ ] setter는 되도록이면 사용안하기 
    - toEntity 메서드는 객체를 엔티티로 변환하는 메서드입니다. 두 가지 방법은 비슷하게 동작하지만 약간의 차이가 있습니다.
      Builder를 사용한 방법:
      ```java
      public User toEntity() {
      return User.builder()
      .loginId(loginId)
      .password(password)
      .userName(userName)
      .email(email)
      .nickname(nickname)
      .studentNumber(studentNumber)
      .major(major)
      .birthday(birthday)
      .build();
      }
      ```   
      이 방법은 Lombok의 @Builder 어노테이션을 사용하여 빌더 패턴을 자동으로 생성합니다. 이 패턴은 객체를 생성할 때 매개변수가 많을 때 유용합니다. 이 코드는 객체를 생성하고 필드를 설정하는 과정을 단순화합니다.

      일반적인 Setter를 사용한 방법:
      ```java
      public User toEntity(String encodedPassword) {
      User user = new User();
      user.setLoginId(this.loginId);
      user.setPassword(encodedPassword);
      user.setUserName(this.userName);
      user.setEmail(this.email);
      user.setNickname(this.nickname);
      user.setStudentNumber(this.studentNumber);
      user.setMajor(this.major);
      user.setBirthday(this.birthday);
      user.setLastLoginAt(null); // 기본값으로 설정
      user.setUserProfile(null); // 기본값으로 설정
      return user;
      }
      ```   
      
      주요 차이점:
  
      Builder를 사용한 경우에는 모든 필드에 대해 한 줄로 설정할 수 있습니다.
      일반적인 Setter를 사용한 경우에는 각 필드를 개별적으로 설정해야 합니다.
      Builder를 사용한 경우에는 빌더 패턴의 이점을 누릴 수 있습니다. 예를 들어, 선택적인 필드를 쉽게 처리할 수 있습니다.
      일반적인 Setter를 사용한 경우에는 코드가 더 명시적이며 각 필드가 어떻게 설정되는지 명확히 볼 수 있습니다.
      어떤 방법을 사용할지는 코드의 가독성과 유지보수성에 따라 다를 수 있습니다. 일반적으로는 Builder 패턴을 사용하는 것이 더 깔끔하고 가독성이 좋습니다.

- [ ] 주로 columndefault 값으로 db 값 설정하기
  - -> 엔티티에서 userRole은 FK로 ManyToOne annotation 사용 중이라 columndefault값 설정해도 안되는 것 같음
  - `@ColumnDefault("1")`은 해당 열의 기본값을 설정하는데 사용됩니다. 그러나 `@ManyToOne` 관계에서는 `@ColumnDefault`를 사용하여 외래 키(FK)의 기본값을 설정하는 것은 의미가 없습니다.
외래 키는 다른 테이블의 기본 키를 참조하기 때문에 자체적으로 값을 설정하는 것이 아니라 연결된 엔티티의 인스턴스를 참조합니다. 따라서 `@ColumnDefault`를 사용하여 외래 키의 기본값을 설정하더라도 이는 영향을 주지 않습니다.

참고자료
왜 Entity에 setter를 사용하지 말아야 할까?
<https://velog.io/@langoustine/setter-%EC%A7%80%EC%96%91-%EC%9D%B4%EC%9C%A0>

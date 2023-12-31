![header](https://capsule-render.vercel.app/api?type=transparent&fontColor=703ee5&height=120&section=header&text=To%20Do%20list&fontSize=70&desc=using%20Spring%20Boot&descAlignY=75&descAlign=60)

## 사용 기술
- Spring Boot 3.1.5
- Thymeleaf
- JPA

## 클래스 다이어그램
<img width="491" alt="class diagram" src="https://github.com/Gyeongsu1997/To-do-app/assets/97381683/d8745a44-2bb9-4857-b5de-83db37c628c2">

## 요구사항 분석
### 기능 목록
- 회원 기능
  <details>
  <summary>회원 등록</summary>
    
    - 회원 등록 버튼을 누르면 createMemberForm이라는 자바스크립트 함수가 실행되어 동적으로 form과 input, button을 만든다. input 태그에는 required 속성이 true로 설정되어 있어 값을 입력하지 않고 버튼을 누르면 '이 입력란을 작성하세요'라는 알림이 나타난다.
  </details>
- 할 일 기능
  - 할 일 등록
  - 할 일 조회
  - 할 일 수정
- 기타 요구사항
  <details>
  <summary>Job 엔티티의 description 필드를 데이터베이스 테이블에 VARCHAR(255) 타입으로 설정하고, 사용자가 255바이트 이하의 글자를 입력하도록 제한한다.</summary>

    -  <details>
       <summary>만약 사용자가 255바이트가 넘어가는 입력을 보내면 어떻게 처리되는가?</summary>

       - JdbcSQLDataException이 발생하였다.
       </details>
    - <details>
      <summary>HTML의 input field에서 입력 글자수를 제한할 수 있는가?</summary>
      
      - input 태그 대신 textarea 태그를 사용하였다. 문자가 입력될때마다 checkByte라는 자바스크립트 함수가 실행되어 현재까지 입력된 바이트를 계산하고 입력된 바이트가 255바이트를 초과하면 substr 메서드로 문자열의 끝부분을 잘라내었다.
      - 참고 1: [textarea 글자수 제한 / 바이트(Byte) 제한](https://hellcoding.tistory.com/entry/textarea-%EA%B8%80%EC%9E%90%EC%88%98-%EC%A0%9C%ED%95%9C-%EB%B0%94%EC%9D%B4%ED%8A%B8Byte-%EC%A0%9C%ED%95%9C)
      - 참고 2: [textarea 입력한 한/영 byte 자르기](https://hansoul.tistory.com/115#google_vignette)
      </details>
  </details>
  <details>
  <summary>할 일의 마감일은 오늘 날짜 이후로 설정 가능하다.</summary>
    
    - LocalDate.now()를 today라는 이름으로 model에 담아 input 태그의 min 속성에 적용함으로써 손쉽게 해결하였다.
  </details>
  <details>
  <summary>할 일 목록은 마감일을 기준으로 오름차순으로 정렬하여 표시한다.</summary>
    
    - repository 계층에서 Job을 조회하는 select query에 order by 옵션을 추가함으로써 해결하였다.
  </details>
  <details>
  <summary>마감일이 지났거나 마감일 당일에 해당하는 할 일은 빨간색으로 표시한다.</summary>
    
    - LocalDate.now()를 today라는 이름으로 model에 담아 th:class="${job.expiryDate <= today} ? 'card expired' : 'card'"의 형태로 job 엔티티의       expiryDate 속성이 today보다 작거나 같으면 추가적인 클래스가 적용되도록 하였다.
  </details>
  <details>
  <summary>특정 회원의 할 일만을 조회할 수 있다.</summary>
    
    - String 타입의 memberName 필드를 갖는 JobSearch라는 클래스를 만들어 memberName에 값이 있으면 select query에 where절을 추가함으로써 해결하였다.
  </details>

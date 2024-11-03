# 프리코스 3주차 미션 - 로또

우아한테크코스 7기 프리코스 3주차 미션, 로또를 구현한 저장소입니다.

## 프로그래밍 요구 사항

- [x] 3항 연산자, else 예약어, switch/case 는 쓰지 않는다.
- [x] indent(인덴트, 들여쓰기)는 2까지만 허용한다. (while문 안에 if문이 있으면 들여쓰기는 2이다.)
- [ ] 함수(또는 메서드)의 길이가 15라인을 넘어가지 않도록 구현한다.
- [x] 함수(또는 메서드)가 한 가지 일만 잘 하도록 구현한다.
- [x] Java Enum을 적용하여 프로그램을 구현한다.
- [x] 제공된 Lotto 클래스를 사용하여 구현해야 한다.
- [x] 구현한 기능에 대한 단위 테스트를 작성한다. 단, UI(System.out, System.in, Scanner) 로직은 제외한다.

## 패키지 목록

- 자주 처리해야하는 예외사항은 common 패키지를 만들어 분리시켰습니다.

```
lotto
├───Application.java                      // 실행파일
└───custom                                
    ├───common                      
    │   ├───ErrorMessages.java            // 공통 예외메시지 상수 처리
    │   └───Exceptions.java               // 공통 예외처리
    ├───constants
    │   ├───NumberConstants.java          // 숫자 상수 처리
    │   └───RegexConstants.java           // 정규표현식 상수 처리
    ├───controller
    │   └───LottoController.java          // 컨트롤러
    ├───model
    │   ├───Lotto.java                    // 로또 관리
    │   ├───LottoMaker.java               // 로또 발행 기능
    │   ├───LottoPrize.java               // 로또 상금 정의 열거형 클래스
    │   ├───Lottos.java                   // 여러 장의 로또 관리
    │   ├───LottoWinningChecker.java      // 로또 당첨 확인 기능
    │   └───LottoYieldCalculator.java     // 로또 수익률 계산 기능
    ├───service
    │   ├───CalculateYieldService.java    // 수익률 관련 서비스
    │   ├───LottoDrawingService.java      // 번호 추첨 관련 서비스
    │   ├───LottoPurchaseService.java     // 구입 관련 서비스
    │   └───LottoResultCheckerService.java// 당첨 여부 확인 관련 서비스
    ├───validator
    │   ├───CustomErrorMessages.java      // 예외 메시지 상수 처리
    │   └───InputValidator.java           // 유효성 검사 기능
    └───view
        ├───InputView.java                // 입력 기능
        ├───OutputView.java               // 출력 기능
        └───PromptMessages.java           // 입출력 관련 메시지 상수 처리
```

## 기능 목록

### 1. 로또 구입 금액 입력

- 프로그래밍 요구사항) 사용자가 입력하는 값은 `camp.nextstep.edu.missionutils.Console`의 `readLine()`을 활용한다.

- **입력예시**:

```입력예시
구입금액을 입력해 주세요.
8000
```

### 2. 로또 발행 수 확인

- 기능 요구사항 1) 구입 금액이 1,000원으로 나누어 떨어지지 않는 경우 예외 처리한다.
- 기능 요구사항 2) 로또 구입 금액을 입력하면 구입 금액에 해당하는 만큼 로또를 발행해야 한다.
- 기능 요구사항 3) 로또 1장의 가격은 1,000원이다.

<details>
<summary>**토글) 구입 금액 문자열 유효성 검사**</summary>

> - 입력된 문자열이 NULL 이거나, 빈 문자열이거나 공백으로만 구성되어 있을 때
>
> - 입력된 문자열에 숫자와 공백 외의 문자가 있을때
>
> - 입력된 문자열의 숫자들 사이에 공백이 존재할 때
>
> - (커스텀) 입력된 숫자가 int 타입의 범위를 벗어날 때
>
> - 입력된 숫자가 1000원으로 나누어 떨어지지 않을 때

</details>

### 3. 로또 발행

- 기능 요구사항 1) 로또 번호의 숫자 범위는 1 ~ 45까지이다.
- 기능 요구사항 2) 1개의 로또를 발행할 때 중복되지 않는 6개의 숫자를 뽑는다.
- 기능 요구사항 3) 발행한 로또 수량 및 번호를 출력한다. 로또 번호는 오름차순으로 정렬하여 보여준다.
- 프로그래밍 요구사항) Random 값 추출은 `camp.nextstep.edu.missionutils.Randoms`의 `pickUniqueNumbersInRange()`를 활용한다.

``` 프로그래밍 요구사항 활용 예시
1에서 45 사이의 중복되지 않은 정수 6개 반환
Randoms.pickUniqueNumbersInRange(1, 45, 6);
```

- **출력예시**:

```출력 예시
8개를 구매했습니다.
[8, 21, 23, 41, 42, 43]
[3, 5, 11, 16, 32, 38]
[7, 11, 16, 35, 36, 44]
[1, 8, 11, 31, 41, 42]
[13, 14, 16, 38, 42, 45]
[7, 11, 30, 40, 42, 43]
[2, 13, 22, 32, 38, 45]
[1, 3, 5, 14, 22, 45]
```

### 4. 당첨 번호 입력

- 기능 요구사항 1) 번호는 쉼표(,)를 기준으로 구분한다.
- 기능 요구사항 2) 로또 번호의 숫자 범위는 1 ~ 45까지이다.
- 기능 요구사항 3) 당첨 번호 추첨 시 중복되지 않는 숫자 6개와 보너스 번호 1개를 뽑는다.
- 프로그래밍 요구사항) 사용자가 입력하는 값은 `camp.nextstep.edu.missionutils.Console`의 `readLine()`을 활용한다.

- **입력예시**:

```입력예시
당첨 번호를 입력해 주세요.
1,2,3,4,5,6
```

<details>
<summary>**토글) 당첨 번호 문자열 유효성 검사**</summary>

> - 입력된 문자열이 NULL 이거나, 빈 문자열이거나 공백으로만 구성되어 있을 때
>
> - 입력된 문자열에 쉼표, 공백, 숫자를 제외한 문자가 존재할 때
>
> - 숫자와 숫자 사이에 공백이 존재할 때
>
> - 숫자가 6개가 아닐 때
>
> - 숫자가 중복될 때
>
> - 숫자가 1 ~ 45의 값이 아닐 때

</details>

### 5. 보너스 번호 입력

- 기능 요구사항 1) 로또 번호의 숫자 범위는 1 ~ 45까지이다.
- 기능 요구사항 2) 당첨 번호 추첨 시 중복되지 않는 숫자 6개와 보너스 번호 1개를 뽑는다.
- 프로그래밍 요구사항) 사용자가 입력하는 값은 `camp.nextstep.edu.missionutils.Console`의 `readLine()`을 활용한다.

- **입력예시**:

```입력예시
보너스 번호를 입력해 주세요.
7
```

<details>
<summary>**토글) 보너스 번호 문자열 유효성 검사**</summary>

> - 입력된 문자열이 NULL 이거나, 빈 문자열이거나 공백으로만 구성되어 있을 때
>
> - 입력된 문자열에 숫자와 공백을 제외한 문자가 존재할 때
>
> - 숫자와 숫자 사이에 공백이 존재할 때
>
> - 보너스 번호가 당첨 번호와 같을 때
>
> - 숫자가 1 ~ 45의 값이 아닐 때

</details>

### 6. 당첨여부 판독

- 기능 요구사항 1) 당첨 내역을 출력한다.
    - 3개 일치 (5,000원) - N개
    - 4개 일치 (50,000원) - N개
    - 5개 일치 (1,500,000원) - N개
    - 5개 일치, 보너스 볼 일치 (30,000,000원) - N개
    - 6개 일치 (2,000,000,000원) - N개

- **출력예시**:

```출력예시
당첨 통계
---
3개 일치 (5,000원) - 1개
4개 일치 (50,000원) - 0개
5개 일치 (1,500,000원) - 0개
5개 일치, 보너스 볼 일치 (30,000,000원) - 0개
6개 일치 (2,000,000,000원) - 0개
```

### 7. 수익률 판독: 당첨에 따른 수익률을 판독한다.

- 기능 요구사항 1) 수익률은 소수점 둘째 자리에서 반올림한다. (ex. 100.0%, 51.5%, 1,000,000.0%)
- 기능 요구사항 2) 수익률을 출력한다.

- **출력예시**:

```출력예시
총 수익률은 62.5%입니다.
```

- 출력 예시를 보아, `수익률은 당첨 금액 / 구입 금액 * 100` 으로 계산한다.

---

## 예외 처리

- 사용자가 잘못된 값을 입력할 경우 IllegalArgumentException을 발생시키고, "[ERROR]"로 시작하는 에러 메시지를 출력 후 그 부분부터 입력을 다시 받는다.
- Exception 이 아닌 IllegalArgumentException, IllegalStateException 등과 같은 명확한 유형을 처리한다.
- 예외 상황 시 에러 문구를 출력해야 한다. 단, 에러 문구는 "[ERROR]"로 시작해야 한다.
    - [ERROR] 로또 번호는 1부터 45 사이의 숫자여야 합니다.

## 테스트 케이스

-**Test Case**:

```테스트케이스

```
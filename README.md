# online-shopping-sequence
"# 온라인 쇼핑몰 상품 구매 시퀀스 다이어그램 및 샘플 코드"

## 1. 개요
온라인 쇼핑몰에서 상품을 구매하는 과정을 시퀀스 다이어그램으로 모델링하고, 이를 기반으로 샘플 코드를 구현한 예시입니다.
또한 구현한 코드의 모듈 구조를 응집도와 결합도 관점에서 평가하였습니다.

## 2. 시퀀스 다이어그램 (Mermaid)
  
    sequenceDiagram
        participant User
        participant Web as WebSite
        participant Server
        participant DB as Database
        participant Pay as PaymentGateway

    User->>Web: 상품 목록 조회 요청
    Web->>Server: 상품 목록 API 요청
    Server->>DB: 상품 목록 쿼리
    DB-->>Server: 상품 목록 반환
    Server-->>Web: 상품 목록 응답
    Web-->>User: 상품 목록 렌더링

    User->>Web: 상품 장바구니 담기
    Web->>Server: 장바구니 추가 요청
    Server->>DB: 장바구니 데이터 저장
    DB-->>Server: 저장 결과 반환
    Server-->>Web: 장바구니 추가 응답
    Web-->>User: 장바구니 상태 갱신

    User->>Web: 주문하기(체크아웃)
    Web->>Server: 주문 요청
    Server->>DB: 주문 정보 저장
    Server->>Pay: 결제 요청
    Pay-->>Server: 결제 결과(성공/실패)
    Server->>DB: 결제 상태 업데이트
    Server-->>Web: 주문/결제 결과 응답
    Web-->>User: 주문/결제 결과 표시
    
![image](https://github.com/user-attachments/assets/ca4332fb-bfaf-49d8-b8c0-e3f845bf9cdb)

## 3. 샘플 코드 구조
- 각 모듈 역할 설명

## 4. 모듈 평가 (응집도, 결합도)
응집도(Cohesion)
각 모듈은 단일 책임 원칙(SRP)에 따라 하나의 역할에 집중되어 있다.
기능적 응집도가 높아 유지보수 및 확장에 용이하다.

결합도(Coupling)
각 모듈은 인터페이스(메서드)로만 상호작용하며, 내부 구현에는 의존하지 않는다.
자료 결합도 수준으로 결합도가 낮아, 각 모듈 변경 시 영향이 최소화된다.


## 5. Python 환경에서 실행하는 방법 안내
1. 각 코드를 user.py, website.py, server.py, database.py, payment_gateway.py, main.py로 저장한다.
2. Python 3.x 환경에서 main.py를 실행한다.

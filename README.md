# online-shopping-sequence
"온라인 쇼핑몰 주문 시나리오의 시퀀스 다이어그램과 Python 샘플 코드 구현"

User->>WebApp: 상품 검색
WebApp->>User: 검색 결과 표시
User->>WebApp: 상품 선택/장바구니 담기
User->>WebApp: 주문 및 결제 요청
WebApp->>Payment: 결제 정보 전달
Payment-->>WebApp: 결제 성공/실패 응답
alt 결제 성공
    WebApp->>Delivery: 배송 요청
    Delivery-->>WebApp: 배송 접수 확인
    WebApp->>User: 주문/배송 확인 알림
else 결제 실패
    WebApp->>User: 결제 실패 알림
end

## 2. 샘플 코드

- Python Flask로 간단한 주문/결제/배송 시나리오 구현

## 3. 모듈 평가

- **응집도:** 각 함수는 단일 기능에 집중 (기능적 응집도)
- **결합도:** 모듈 간 데이터만 주고받는 데이터 결합도, 의존성 낮음

## 4. 실행 방법

- `pip install flask`
- `python app.py` 실행 후, Postman 또는 curl로 API 테스트

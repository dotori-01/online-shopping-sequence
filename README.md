# 개인 실습 과제 1


      주제 " 온라인 쇼핑몰 상품 구매 시스템 " (online-shopping-sequence) 
      학번/이름: 20210015 도현명

## 목차
1. [개요](#1-개요)
2. [시퀀스 다이어그램 (Mermaid.live 활용)](#2-시퀀스-다이어그램-mermaidlive-활용)
3. [샘플 코드 구조](#3-샘플-코드-구조)
4. [모듈 평가 결과(응집도-결합도)](#4-모듈-평가-결과응집도-결합도)
5. [샘플 코드 실행 방법](#5-샘플-코드-실행-방법)
   
## 1. 개요
온라인 쇼핑몰에서 상품을 구매하는 과정을 시퀀스 다이어그램으로 모델링하고, 이를 기반으로 샘플 코드를 구현합니다. 
또한 구현한 코드의 모듈 구조를 응집도와 결합도 관점에서 평가하고, 샘플 코드 실행도 함께 해봤습니다.

## 2. 시퀀스 다이어그램 (Mermaid.live 활용)
  
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
![image](https://github.com/user-attachments/assets/5e41fee6-f753-4650-bdbd-5ae258df6260)


## 3. 샘플 코드 구조
3.1. user.py

    class User:
        def __init__(self, name):
            self.name = name
            self.cart = []

        def browse_products(self, website):
            return website.show_products()
    
        def add_to_cart(self, website, product):
            website.add_to_cart(self, product)
    
        def checkout(self, website):
            return website.checkout(self)
3.2. website.py

    class Website:
          def __init__(self, server):
              self.server = server
      
          def show_products(self):
              return self.server.get_product_list()
      
          def add_to_cart(self, user, product):
              self.server.add_cart_item(user, product)
      
          def checkout(self, user):
              return self.server.process_order(user)
3.3.  server.py
        
    class Server:
         def __init__(self, database, payment_gateway):
             self.database = database
             self.payment_gateway = payment_gateway

         def get_product_list(self):
             return self.database.query_products()
    
         def add_cart_item(self, user, product):
             self.database.save_cart(user, product)
     
         def process_order(self, user):
             order = self.database.save_order(user)
             result = self.payment_gateway.pay(order)
             self.database.update_order_status(order, result)
             return result
3.4. database.py

    class Database:
        def query_products(self):
            return ["상품A", "상품B", "상품C"]

        def save_cart(self, user, product):
            user.cart.append(product)
    
        def save_order(self, user):
            return {"user": user.name, "items": user.cart}
    
        def update_order_status(self, order, payment_result):
            order["status"] = "성공" if payment_result else "실패"
3.5. payment_gateway.py

    class PaymentGateway:
        def pay(self, order):
            # 실제 결제 로직 대신 무조건 성공 처리
            return True

3.6. main.py          

    from user import User
    from website import Website
    from server import Server
    from database import Database
    from payment_gateway import PaymentGateway
    
    db = Database()
    pg = PaymentGateway()
    server = Server(db, pg)
    website = Website(server)
    user = User("홍길동")
    
    print("상품 목록:", user.browse_products(website))
    user.add_to_cart(website, "상품A")
    result = user.checkout(website)
    print("주문 결과:", "성공" if result else "실패")
    
## 4. 모듈 평가 결과(응집도, 결합도)
응집도(Cohesion)


    각 모듈은 단일 책임 원칙(SRP)에 따라 하나의 역할에 집중되어 있다.
    예시: Database는 데이터 저장/조회만 담당, PaymentGateway는 결제만 담당한다.
    기능적 응집도가 높아 유지보수 및 확장에 용이합니다.




결합도(Coupling)


    각 모듈은 인터페이스(메서드)로만 상호작용하며, 내부 구현에는 의존하지 않습니다.
    예시: Server는 PaymentGateway의 pay 메서드만 호출, 내부 결제 방식은 알 필요 없다.
    자료 결합도 수준으로 결합도가 낮아, 각 모듈 변경 시 영향이 최소화된다.




## 5. 샘플 코드 실행 방법 
  5.1. Python 설치

 
      Python 3.xx 버전을 다운로드


5.2. 코드 파일 준비
   
   
         각 모듈(user.py, website.py, server.py, database.py, payment_gateway.py, main.py)
         파일을 같은 폴더에 저장
  ![image](https://github.com/user-attachments/assets/b3185d02-dc8f-4c7b-a825-de59af52c61b)


5.3. 코드 실행
  
  
       터미널(명령 프롬프트)을 열고 해당 폴더로 이동하고, python main.py 명령어를 입력하여 실행


5.4. 실행 화면


  ![image](https://github.com/user-attachments/assets/108d8492-0618-4f87-9e8b-06b97950d8c2)



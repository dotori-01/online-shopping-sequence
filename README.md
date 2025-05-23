# online-shopping-sequence
"온라인 쇼핑몰 주문 시나리오의 시퀀스 다이어그램과 Python 샘플 코드 구현"

![image](https://github.com/user-attachments/assets/5c90dcb1-5792-495c-aa76-a3d960e38ed2)


## 2. 샘플 코드

- Python Flask로 간단한 주문/결제/배송 시나리오 구현
  
# app.py
from flask import Flask, request, jsonify

app = Flask(__name__)

# 상품 데이터 예시
products = [
    {"id": 1, "name": "노트북", "price": 1000000},
    {"id": 2, "name": "마우스", "price": 20000}
]

@app.route('/search', methods=['GET'])
def search():
    # 모든 상품 리스트 반환
    return jsonify({"results": products})

@app.route('/order', methods=['POST'])
def order():
    data = request.json
    product_id = data.get('product_id')
    payment_status = data.get('payment_status')

    # 결제 처리 시뮬레이션
    if payment_status == 'success':
        # 배송 요청 시뮬레이션
        return jsonify({
            "status": "success",
            "message": f"상품 {product_id} 주문 및 배송 요청 완료"
        })
    else:
        return jsonify({
            "status": "fail",
            "message": "결제 실패"
        })

if __name__ == '__main__':
    app.run(debug=True)
    
## 3. 모듈 평가

- *응집도* : 각 함수(search, order)는 각각 상품 검색, 주문/결제/배송이라는 단일 기능에 집중되어 있어 기능적 응집도가 높다.
- *결합도* : 함수 간 직접적인 의존성이 없고, 데이터(HTTP 요청/응답)만 주고받으므로 데이터 결합도에 해당합니다. 이는 모듈 간 독립성을 높여 유지보수와 확장에 유리하다.

## 4. 실행 방법

- `pip install flask`
- `python app.py` 실행 후, Postman 또는 curl로 API 테스트

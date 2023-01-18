# Project : Auto-Farming
1. 개요 : 기후 악화, 농업 인력 부족을 3세대 스마트 팜으로 해소
2. 목표 : 비전 시스템과 로봇팔을 활용한 묘삼 자동 식재 장치 개발
3. 팀원 : 박진우(하드웨어, 프론트엔드), 윤승욱(백엔드), 이범기(로봇팔), 문영철(컨베이어) 외 5인
4. 기간 : 22. 11. 7 ~ 22. 12. 14

# My Role : DeepLearning, Database
1. 나의 역할 : 딥러닝, 데이터베이스, 기획/발표

2. 내가 사용한 기술 : Yolo v5, OpenCV, MySQL

3. 객체탐지 모델 학습
    - 데이터 수집 : 새싹삼 재배 농장에서 묘삼을 촬영
    - 데이터 라벨링 : roboflow 사이트를 사용하여 이미지 라벨링
    - 모델 전이 학습 : Colab 환경에서 pretrianed된 Yolo v5s 모델에 학습
    - mAP50 : 0.99 / mAP50-95 : 0.58 

4. 객체 수와 객체 뭉침 파악하여 컨베이어베트와 플렉싱 피더 제어 
    - 카메라로부터 받은 영상을 Yolo v5 모델을 사용하여 객체탐지
    - 객체 탐지된 객체가 3개 이하이면 수평 컨베이어 벨트 동작 신호 발생
    - 묘상이 뭉쳐 있는 경우 바운딩 박스가 일반적인 객체보다 더 큰 것을 활용  
      정상 객체보다 큰 바운딩 박스가 형성되면 플랙싱 비더 동작 신호 발생

5. 최근접 객체 도출 및 해당 객체 각도 예측
    - 묘삼의 바운딩 박스안에 하나의 뇌두가 들어있는 객체를 정상 객체로 상정,  
      y절편과 가장 가까운 뇌두의 좌표를 최근접 객체로 판단
    - 각도 도출을 위해 묘삼의 바운딩 박스 네 꼭짓점과 뇌두의 중심좌표간의 유클리드 거리 계산,  
      가장 가까운 바운딩 박스에 따라 묘삼의 각도를 예측
    - 0도 90도 180도 270도의 직각인 경우 최근접점과 차근접점의 유클리더 거리차가  
      일정거리 이하가 되면 직각으로 예측
    - 위 알고리즘을 적용하여 0도부터 315도까지 8까지 각도로 예측 가능

6. 개선사항
    - 카메라와 컨베이어 벨트사이의 좌표 맵핑
    - 45도 단위보다 정교한 각도 도출 알고리즘 개발
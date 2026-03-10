--- 

creation date: 23-08-07 
modification date: 23-08-07 
tag: 

---
## 요약
- 도커 컴포즈는 여러 도커 컨테이너를 정의하고 관리하는 도구
- 복잡한 애플리케이션을 쉽게 정의하고 실행 가능. 
- 여러 컨테이너 간의 상호작용 자동화


도커 명령어가 깔끔하지 못하고 불편함. 

```
docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --network=app-network ...
```

위와 같은 명령어들을 편하게 해주는것

yml 파일 형식으로 도커컴포즈 파일 만드는건데
yml은 띄어쓰기로 데이터를 표현하는 얘임

```yml
version: '3' 
services: 
  web: 
    image: my_web_app 
    build: . 
    ports: 
      - "5000:5000"
    depends_on: 
      - db 
```


Compose는 멀티 컨테이너 Docker 애플리케이션을 정의, 실행하기 위한 도구
- 해당 디렉터리에서 docker-compose up
- docker-compose up -d 
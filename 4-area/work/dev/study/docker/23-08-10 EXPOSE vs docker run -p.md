--- 

creation date: 23-08-10 
modification date: 23-08-10 
tag: 

---

### -p 

- 호스트 OS와 컨테이너 환경 포트를 매핑시키는 것
- docker run -p 8080:80 webserver
	- 호스트 OS의 8080포트를 컨테이너의 80번 포트로 연결

### Dockerfile의 Expose

- EXPOSE 80
- 이 도커 이미지는 80번 포트를 외부에 공개할 것이다.
- 이 포트는 호스트 OS에 포워딩이 안됨.

















```mermaid
sequenceDiagram  
    participant user  
    participant bff
    participant web
    user->>web: app/login
    loop 인증 쿠키 굽기 (age 포함)
		
	end
    web-->>user: {age, stoken ...}





```
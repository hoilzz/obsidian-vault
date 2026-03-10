---
created: 25-02-21
modified: 25-02-21
tags:
---
ISR의 가이드문서는 time-based다. 이는 불필요한 API 요청 (변경이 없음에도) 혹은 바로 업데이트 되어야하는 케이스에서 문제가 있는 구현 방식이다.

### 1. res.revalidate()를 이용한 on-demand revalidation
getStaticProps와 함께 사용시 여기서 revalidate 설정 불필요

```tsx
export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  // 1. 보안 토큰 확인 (비인가 요청 차단)
  if (req.query.secret !== process.env.MY_SECRET_TOKEN) {
    return res.status(401).json({ message: 'Invalid token' })
  }
 
  try {
    // 2. 특정 경로를 리빌리데이션 ("/posts/1" 페이지 갱신)
    await res.revalidate('/posts/1')
    return res.json({ revalidated: true })
  }
}
```

### 3. 캐시 저장 위치 커스터마이징

자체 서버에서 배포할 경우 ISR 캐시는 파일디스크에 저장됨. 캐시를 여러 컨테이너에서 공유하고 싶다면 캐시 저장 위치를 직접 설정 가능.


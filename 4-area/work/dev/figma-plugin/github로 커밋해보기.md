https://dev.to/lucis/how-to-push-files-programatically-to-a-repository-using-octokit-with-typescript-1nj0


https://docs.github.com/en/rest/guides/using-the-rest-api-to-interact-with-your-git-database?apiVersion=2022-11-28


### 파일 변경점 커밋하기 aka Git official

- 현재 커밋 객체 가져오기
- 1번을 가리키는 트리 가져오기
- 특정 파일 경로에 대해 트리에 있는 블롭 개체의 콘텐츠를 검색
- 컨텐츠를 변경하고 새 콘텐츠로 새 블롭 객체를 post하고, 블롭 SHA를 다시 받음
- 해당 파일 경로 포인터가 새 블롭 SHA로 대체된 새 트리 오브젝트를 post하여 트리 SHA를 다시 가져옴
- 현재 커밋 SHA를 부모로 하고 새 트리 SHA를 사용하여 새 커밋 개체를 만들어 커밋 SHA를 가져옴
- 브랜치의 레퍼런스가 새 커밋 SHA를 가리키도록 업데이트.



---


1. 마지막 commit SHA 가져오기
2. commit SHA로 해당 커밋 tree 가져오기. 사실상 파일을 가진 데이터 구조임.
3. 업로드할 file list 가져오기. 
   - 이건 path, contents를 포함해야함
   - 이걸 통해 blob을 만들어야함.
4. blobs의 모든 SHA는 Step 3에서 만들어짐. 이 파일들 가지고 **새로운 tree** 를 만들거임. 
   - 여기에서 blob을 path에 연결하고 모든 blob을 tree에 연결할 수 있따.
   - mode는 100644
   - Step 2에서 얻은 SHA를 이 트리의 parent tree로 설정해야함.  
5. 모든 파일을 포함하는 트리로 (그것의 SHA와), 그 트리를 가리키는 new commit을 만들어야함.
   - 레포에 대한 모든 변경점을 가진 커밋이 될 것이다.
   - Step 1에서 얻은 SHA 커밋을 부모 커밋으로 가지고 있어야한다.
6. branch ref를 마지막 스텝에 생성된 커밋을 가리키도록 설정할거고 



---
1. /git/blobs
   - binary large object (blob)
   - 저장소 내부 각 파일 컨텐츠를 저장하는데 사용되는 object type
   - git blob
      - git blob은 object type임. 각 파일 컨텐츠를 저장하는데 사용됨. 
      - file의 SHA-1 해시가 계산되어 blob 객체에 저장된다. 
      - 이 엔드포인트를 통해 blob object를 git DB에 읽고 쓸 수 있다.
      - blob의 미디어 유형으로는 [참고](https://docs.github.com/en/rest/using-the-rest-api/media-types?apiVersion=2022-11-28)
      - content: svg 코드, 와 encoding만 전달하면 sha를 받음.
2. imageBlobsMap 객체 만들기
   - 위 blobs로 배열을 만들건데 `[path]: {path, mode, type, sha}` 형태임
   - 이 때 path는 전체 path가 아닌 파일이름임. 
   - 위 형태가 imageGitBlob 형태
3. 여기까지는 일단 imageBlob을 만드는 과정. 이후부터는 기존 깃헙 blob tree를 가져올거임.
4. baseRef, headCommit, headTree 를 github api로 가져옴
5. 사용자가 입력한 path를 split하여 path로 가진다.
6. 기존 blobTree를 가져온다 (prevImageBlobsTree)
   - apps/tapas/public/icon 하위 모든 blobTree를 찾는과정.
   - `{path: '이름', mode, tyupe, sha, url}`
   - 이때 parentTrees도 만드는데 위와 비슷한 형태를 가진것들의 2차원 배열
      - 각 path별로 쭉 추가함
7. 기존 github blob tree에 새로운 blob tree를 추가함 (newImageBlobsTree)
   - prevImageBlobsTree를 map돌려서 `imageBlobsMap[blob.path]` 가 있다면 기존꺼를 덮어쓴다.


- newImageBlobsTree
- newGitImageTree 
	- createTree(newImageBlobsTree)
- newRootGitTree
	- splittedPaths.reduceRight()
		- `[apps,tapas,public]` 
		- parentTrees -> root 하위, apps 하위, tapas 하위
		- parentTree -> `parentTrees[index]`
		- targetTree -> parentTree에서 splittedPath의 현재 item과 일치하는 것이 targetTree
		- parentTree에서 현재 path의 것만 제외하고 그대로 할당
		- 





---

### 파일 변경점에 대해서만 반영하기
- how to
	- 파일은 해당 path에 그 파일이 있냐 없냐 여부를 따짐
	- 타입파일의 경우 해당 타입파일에서 해당 타입이 있는지 여부를 따져야함.
		- 코드를 다시 복원시켜서 includes 여부를 찾음.
		- includes (O) -> 아무것도 안함
		- includes (X) -> 코드 변경사항 반영해야함.

- 걍 단순 문자열로 된 코드
- JSON.parse -> 추가
	- 근데 런타입상에서 타입스크립트 구문 분석됨?
	- enum값도 들고 있어서; parse가 될지 미지수;
- 다시 그거 태움.
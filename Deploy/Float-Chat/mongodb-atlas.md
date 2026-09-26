# MongoDB Atlas 내용 정리

## 초기 설정 순서

- 새로운 프로젝트를 추가하고 프로젝트 이름만 설정 (다른 항목은 선택 사항)

## 클러스터 생성

- 클러스터를 추가하고 무료로 설정
- 클러스터 이름 설정과 AWS 그리고 지역은 서울로 구성
- 클러스터 구성 시 Sample Dataset은 사용하지 않음

## DB User 생성

- Atlas에서 생성한 DB 사용자 계정의 Username, Password 따로 복사 후 저장
- 해당 계정은 MongoDB Atlas 웹 사이트 로그인 계정과 별개

## MongoDB 연결 방식 선택

- Connect to your application 항목의 Drivers로 들어가서 애플리케이션 연결 정보 확인
- 언어는 JS
- Client Library는 Node.js 선택
- 백엔드 사용하던 버전 선택 (ex: Node.js 6.7 or later)

## MongoDB Connection String

- `mongodb+srv://<username>:<password>@float-chat.wlelfmx.mongodb.net/?appName=float-chat` 같은 내용에 DB 사용자 정보를 넣고 Render 환경 변수에 넣음
- 깃 허브에 저장하지 않고, .env 파일을 깃 허브에 업로드 하지 않으며 Render 환경 변수에만 등록

## NETWORK ACCESS

- 항목을 추가해 0.0.0.0/0로 구성
- 0.0.0.0/0로 구성하면 모든 IP에서 MongoDB Atlas 접속 허용

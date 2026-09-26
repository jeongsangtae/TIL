# Render 백엔드 배포 내용 정리

## 초기 설정 순서

- Web Service 선택
- 깃 허브에서 선택하려는 프로젝트 보여지도록 설정하고 해당 프로젝트 연결
- 프로젝트 이름과 Language, Branch, Region 설정
- Root Directory와 Build 시에 실행될 명령어, 시작 명령어 설정
- Compute는 무료로 설정
- 환경 변수 설정해도 되고, 추후에 추가해도 문제없음

## Settings

- Name은 프로젝트 이름 + backend
- Region은 싱가포르로 설정
- Source는 프로젝트 깃 허브 경로 (깃 허브에 들어가 프로젝트가 보여지도록 설정 필요)
- Root Directory는 backend
- Build Command는 yarn
- Start Command는 `npm install --production=false && npm start`

## Environment

- ACCESS_TOKEN_KEY는 프로젝트에서 설정한 키-값
- REFRESH_TOKEN_KEY는 프로젝트에서 설정한 키-값
- CORS_URL는 Render에서 배포한 프론트엔드 주소
- MONGODB_URI는 MongoDB Atlas 클러스터 구성 시에 보여주는 URI 내용
- NODE_ENV는 프로젝트 env 파일에서 설정한 내용 반대로 구성

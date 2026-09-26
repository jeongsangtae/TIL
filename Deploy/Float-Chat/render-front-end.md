# Render 프론트엔드 배포 내용 정리

## 초기 설정 순서

- Static Site 선택
- 깃 허브에서 선택하려는 프로젝트 보여지도록 설정하고 해당 프로젝트 연결
- 프로젝트 이름과 Branch, Root Directory 설정
- Build 시에 실행될 명령어 입력
- Publish Directory 설정
- 환경 변수 설정해도 되고, 추후에 추가해도 문제없음

## Settings

- Name은 프로젝트 이름
- Source는 프로젝트 깃 허브 경로 (깃 허브에 들어가 프로젝트가 보여지도록 설정 필요)
- Root Directory는 frontend
- Build Command는 `npm install --production=false && NODE_OPTIONS="--max-old-space-size=4096" npm run build`
- Publish Directory는 dist

## Environment

- VITE_API_URL는 Render에서 배포한 백엔드 주소

## Redirects/Rewrites

- Source는 `/*`
- Destination은 `/index.html`
- Action은 Rewrite

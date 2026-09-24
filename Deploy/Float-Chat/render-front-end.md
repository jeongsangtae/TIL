# Render 프론트엔드 배포 내용 정리

## 초기 순서

- 1

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

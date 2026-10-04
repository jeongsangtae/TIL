# Electron Guide 내용 정리

## Node.js 버전

- v22 이후의 버전

## Electron 관련 라이브러리

- electron (React/Vite 앱을 데스크톱 앱으로 실행)
- electron-builder (Electron 앱을 Windows 설치 파일 .exe로 패키징)
- concurrently (Vite와 Electron 개발 서버를 동시에 실행)
- wait-on (Vite 서버가 실행된 후 Electron을 실행하도록 대기)

## Electron 파일 구조

- electron 폴더 내의 main.cjs, preload.cjs
- electron-builder.yml

## Electron 관련 파일 세부 내용

- main.cjs는 Electron의 메인 프로세서 역할을 담당하고 개발 환경에서 Vite 서버를 엶
- preload.cjs는 특별한 Electron API를 노출할 필요가 없기 때문에 기본 파일 형태로 구성

## package.json에 Electron 관련 스크립트 추가

- `"electron:dev": "concurrently -k \"vite\" \"wait-on http://localhost:5173 && electron .\"",`
- 위 내용으로 인해 개발 환경 실행은 `npm run electron:dev`
- `"electron:build": "npm run build && electron-builder"`
- 위 내용으로 인해 Electron Windows 설치 파일 생성은 `npm run electron:build`

## Electron Builder 설정

- Windows용 설치 파일을 만들기 위해 electron-builder.yml 파일 추가
- `directories: output: release` 내용으로 인해 빌드 결과가 frontend/release/에 생성
- 자세한 구성은 깃 허브 확인

## Electron 실행 방식 변경

- main.cjs에서 개발 환경과 패키징된 앱을 구분

```
if (app.isPackaged) {
  mainWindow.loadURL("https://float-chat-vdc1.onrender.com");
} else {
  mainWindow.loadURL("http://localhost:5173");
}

app.isPackaged의 역할은 Electron이 패키징된 애플리케이션으로 실행되고 있는지 판단

npm run electron:dev면 false로 localhost:5173
FloatChat Setup 1.x.x.exe면 true로 Render FloatChat
```

## Electron 개발 환경과 패키징된 앱 실행

- 개발 환경은 프론트엔드 폴더로 들어가 `npm run electron:dev` 명령어 입력
- 패키징된 앱은 깃 허브 release에 배포된 .exe 파일을 다운 받아 설치 후 실행

## 이 외 추가 내용

- loadFile()과 loadURL() 차이는 로컬 HTML 파일을 열 때 사용하는 것과 Render 같은 웹 사이트 주소를 넣는 용도
- 개발 환경 실행 방식은 `npm run electron:dev`로 실행하면 localhost:5173
- 빌드된 내용 실행 방식은 `npm run electron:build` 후 ex) FloatChat Setup 1.0.1.exe 같은 파일 실행 후 Electron 실행
- Electron 관련 내용을 추가로 수정할 때만 다시 build 해야 하며, 내용 수정은 main.cjs 파일을 통해 수정

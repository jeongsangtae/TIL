# Electron Architecture

## 1. Electron의 기본 구조

- FloatChat은 React 기반 웹 애플리케이션을 Electron으로 감싸 데스크톱 앱으로 실행
- 현재 Electron은 배포된 FloatChat 웹 페이지를 불러오는 방식으로 동작

### 주요 구성 요소

| 구성 요소        | 역할                                               |
| ---------------- | -------------------------------------------------- |
| Main Process     | Electron 앱과 데스크톱 창 관리                     |
| Renderer Process | React UI와 웹 페이지 실행                          |
| Preload Script   | 렌더러에 필요한 제한된 Electron 기능 공개          |
| IPC              | Main Process와 Renderer Process 사이의 메시지 전달 |

### 전체 구조

```text
Electron App
├── Main Process
│   └── main.cjs
│       ├── BrowserWindow 생성
│       ├── 웹 페이지 로드
│       ├── 창 최소화 / 최대화 / 닫기
│       └── F5 새로고침 처리
│
├── Preload Script
│   └── preload.cjs
│       └── window.electronAPI 공개
│
└── Renderer Process
    └── React
        ├── FloatChat UI
        └── window.electronAPI 호출
```

## 2. Main Process — `main.cjs`

Electron 앱의 실행과 데스크톱 창을 관리하는 영역

### 주요 API

| 코드              | 역할                                 |
| ----------------- | ------------------------------------ |
| `app`             | 앱 실행 및 생명주기 관리             |
| `BrowserWindow`   | 데스크톱 창 생성 및 관리             |
| `ipcMain`         | 렌더러에서 전달한 IPC 메시지 수신    |
| `webContents`     | 창에 표시되는 웹 페이지 제어         |
| `app.isPackaged`  | 개발 환경과 패키징된 앱 구분         |
| `app.whenReady()` | Electron 앱이 초기화될 때까지 기다림 |
| `app.quit()`      | Electron 앱 종료                     |

### `createWindow()`

메인 창을 생성하고 창의 크기, 최소 크기, 프레임, 투명도, preload 파일 등을 설정

### 웹 페이지 로드

```js
if (app.isPackaged) {
  mainWindow.loadURL("https://float-chat-vdc1.onrender.com");
} else {
  mainWindow.loadURL("http://localhost:5173");
}
```

- **개발 환경:** 로컬 Vite 서버 실행
- **패키징 환경:** 배포된 FloatChat 웹 페이지 실행

현재 구조에서는 React UI 변경 사항을 배포된 웹 페이지에 반영하면 Electron에서도 해당 변경 사항을 확인할 수 있다. 단, 캐시나 서비스 상태 등에 따라 즉시 반영되지 않을 수 있음

### 창 제어

`ipcMain.on()`을 사용해 렌더러에서 전달한 창 제어 요청을 처리

| 이벤트            | 동작                         |
| ----------------- | ---------------------------- |
| `window-minimize` | 창 최소화                    |
| `window-maximize` | 최대화 또는 원래 크기로 복원 |
| `window-close`    | 창 닫기                      |

### F5 새로고침

```js
mainWindow.webContents.on("before-input-event", (event, input) => {
  if (input.key === "F5" && input.type === "keyDown") {
    event.preventDefault();
    mainWindow.webContents.reload();
  }
});
```

| 코드                     | 역할                    |
| ------------------------ | ----------------------- |
| `before-input-event`     | 키보드 입력 이벤트 처리 |
| `input.key`              | 입력된 키 확인          |
| `input.type`             | 키 입력 동작 유형 확인  |
| `event.preventDefault()` | 기본 동작 취소          |
| `webContents.reload()`   | 현재 페이지 새로고침    |

현재는 F5 입력을 감지해 새로고침하도록 구현

## 3. Preload Script — `preload.cjs`

Preload는 Main Process와 Renderer Process 사이에서 렌더러에 필요한 제한된 API를 공개

### 주요 API

| 코드                  | 역할                       |
| --------------------- | -------------------------- |
| `contextBridge`       | 격리된 렌더러에 API 공개   |
| `ipcRenderer`         | Main Process로 메시지 전송 |
| `exposeInMainWorld()` | `window` 객체에 API 공개   |
| `ipcRenderer.send()`  | IPC 이벤트 전송            |

### API 공개

```js
contextBridge.exposeInMainWorld("electronAPI", {
  isElectron: true,
  minimize: () => ipcRenderer.send("window-minimize"),
  maximize: () => ipcRenderer.send("window-maximize"),
  close: () => ipcRenderer.send("window-close"),
});
```

React에서는 `window.electronAPI`를 통해 공개된 기능을 호출

`contextIsolation: true`와 `nodeIntegration: false` 설정을 유지하면서 필요한 기능만 노출하는 방식

## 4. IPC 통신 흐름

IPC(Inter-Process Communication)는 프로세스 사이에서 메시지를 주고받는 방식

### 예시: 창 최소화

```text
1. React Renderer
   사용자가 최소화 버튼 클릭
          ↓
2. Preload
   ipcRenderer.send("window-minimize")
          ↓
3. Main Process
   ipcMain.on("window-minimize", ...)
          ↓
4. BrowserWindow
   창 최소화 실행
```

### 각 코드의 역할

**Renderer Process**

사용자의 클릭을 감지하고 공개된 API를 호출

```ts
window.electronAPI?.minimize();
```

**Preload Script**

메인 프로세스에 IPC 메시지를 전달

```js
minimize: () => ipcRenderer.send("window-minimize");
```

**Main Process**

메시지를 수신하고 실제 창 제어 기능을 실행

```js
ipcMain.on("window-minimize", () => {
  BrowserWindow.getFocusedWindow()?.minimize();
});
```

### 중요한 점

- Renderer는 운영체제 창을 직접 제어하지 않는다.
- Preload는 렌더러에 필요한 기능만 공개한다.
- Main Process는 전달받은 요청에 따라 실제 창 제어를 수행한다.
- IPC 이벤트 이름은 송신하는 쪽과 수신하는 쪽에서 일치해야 한다.

## 5. BrowserWindow 보안 설정

현재 `main.cjs`의 설정

```js
webPreferences: {
  preload: path.join(__dirname, "preload.cjs"),
  contextIsolation: true,
  nodeIntegration: false,
}
```

| 설정                     | 역할                                                  |
| ------------------------ | ----------------------------------------------------- |
| `preload`                | Preload 스크립트 경로 지정                            |
| `contextIsolation: true` | 렌더러의 JavaScript 환경과 preload 환경 격리          |
| `nodeIntegration: false` | 렌더러에서 Node.js 기능에 직접 접근하지 못하도록 제한 |

이 설정은 렌더러에 Electron 내부 기능을 무분별하게 노출하지 않기 위한 기본

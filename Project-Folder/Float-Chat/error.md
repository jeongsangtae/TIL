# Float Chat 에러 내용

### 프로젝트 구성 중 발생한 에러

- React
  - 에러
    - `@types/react`는 TypeScript 설정에 따라 CommonJS 또는 ES Module 방식을 따르는데, 현재 설정에 `allowSyntheticDefaultImports` 플래그가 빠져 있어서 오류 발생
  - 해결 방법
    - tsconfig.json에 `"allowSyntheticDefaultImports": true`, `"esModuleInterop": true`를 추가해서 해결

<br />

- ReactDOM 관련 에러
  - 에러
    - `react-dom/client`는 ESM(ES Module) 스타일로 내보내기를 사용하는데, TypeScript는 이런 스타일을 올바르게 가져오려면 `esModuleInterop`와 `allowSyntheticDefaultImports` 플래그가 필요하지만 빠져있어서 에러 발생
  - 해결 방법
    - React 에러를 해결할 때 추가한 `allowSyntheticDefaultImports`, `esModuleInterop` 내용으로 해당 에러도 함께 해결

<br />

- React Router DOM 관련 에러
  - 에러
    - `moduleResolution` 옵션이 bundler로 설정되어 있어서 node_modules 내 모듈을 올바르게 찾지 못해 에러 발생
  - 해결 방법
    - tsconfig.json 내용에서 `"moduleResolution": "bundler"` 내용을 `"moduleResolution": "node"`로 변경해서 해결

<br />

- allowImportingTsExtensions 에러
  - 에러
    - tsconfig.json 내용에서 `allowImportingTsExtensions` 옵션이 빨간 물결로 에러가 발생하는데, `moduleResolution` 옵션이 `"bundler"`모드와 함께 사용되어야 하지만 `"node"`로 설정되었기 때문에 발생하는 에러
  - 해결 방법
    - tsconfig.json 내용에서 `allowImportingTsExtensions` 옵션을 삭제해 에러를 해결
    - `"moduleResolution": "node"`로 사용을 하기 때문에 `allowImportingTsExtensions` 옵션은 불필요하므로 삭제해 해결

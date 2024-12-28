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

<br />

- useLoaderData 훅 타입 정의 에러
  - 에러
    - React + TypeScript 프로젝트에서 useLoaderData 훅을 사용하는데, 이 내용에 타입 정의를 추가해도 타입 관련 에러가 발생
    - 해당 에러는 현재 사용하고 있는 react router dom 버전이 TypeScript 제네릭 타입 지원을 제대로 포함하지 않아서 발생하는 에러
  - 시도한 방법
    - 초기에 사용하고 있는 react router dom 버전은 6.28.0으로 TypeScript 제네릭 타입 지원을 제대로 포함하지 않는 건지 에러 발생
    - react router dom 6.4 버전부터 TypeScript 제네릭을 통한 useLoaderData의 타입 정의가 지원되었다고 해서 6.14.2 버전을 사용해 봤으나, 여전히 useLoaderData 타입 정의에서 에러 발생
  - 해결 방법
    - 최신 버전인 7.1.1 버전으로 업데이트한 후에, 에러가 사라지고 useLoaderData 내용에서 정상적으로 타입 정의가 적용되는 것을 확인
    - react router dom 7.1.1 버전은 TypeScript 5.x 대 버전과 호환에도 문제가 없고, TypeScript 제네릭 타입 지원을 하기 때문에 에러없이 사용 가능
    - `npx tsc` 명령어를 통해 TypeScript 빌드를 실행해 타입 에러가 발생하는지 확인한 결과, 빌드 중 타입 에러가 없는 것을 확인
      - 6.28.0, 6.14.2 버전에서는 `npx tsc` 명령어를 치면 타입 에러가 있다고 확인이 됐었음

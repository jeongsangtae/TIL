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

<br />

- children 속성이 없다는 에러
  - 에러
    - React.FC를 사용하면 children 속성을 기본적으로 포함하므로, `{ onToggle: () => void }`로 직접 타입을 정의했을 때는 children이 명시적으로 포함되지 않아 TypeScript가 children 속성이 없다고 판단하는 에러
    - React.FC 정의된 형태
      - `type React.FC<P = {}> = FunctionComponent<P & { children?: ReactNode }>;`
    - React.FC를 사용하면 children 속성이 암묵적으로 추가되지만, `React.FC<{ onToggle: () => void }>`로 작성한 경우, children 속성이 타입 정의에 없다고 간주되면서 onToggle만 포함된 타입으로 처리되어 children이 누락되었다는 오류가 발생
  - 해결 방법
    - React.FC를 유지하면서 타입 정의 수정
    - children을 명시적으로 포함하도록 타입 정의를 확장
      - `const AuthModal: React.FC<{ onToggle: () => void; children?: React.ReactNode }> = ({ children, onToggle }) => {...}`

<br />

- filter 관련 타입 불일치(확장) 에러
  - 에러
    - filter를 추가해 반환하는 새로운 배열에서 기존에 구성한 타입 정의 내용과 호환되지 않는 에러 발생
      - filter를 통해 반환되는 새로운 배열에서는 null 값이 넘어올 수 없으나, 구성된 타입 정의 내용은 null을 포함하고 있어서 에러가 발생
    - filter로 인해 타입이 확장되어, TypeScript는 배열의 filter 함수에서 반환된 데이터의 타입을 정확히 추론하지 못하는 에러
      - filter 조건에서 특정 타입을 확인하지 않으면 "type" 속성이 기본적으로 string으로 간주될 수 있음
  - 에러 원인 추가 내용
    - filter 메서드로 인해 타입 확장이 발생
      - TypeScript는 filter를 적용하면 결과 배열의 타입을 명시적으로 좁히지 않는 한, 배열의 원소 타입을 기본적으로 넓게 설정
      - filter로 반환된 결과가 `type: ModalType`으로 유지되지 않고, `type: string`으로 확장되어 타입 호환 문제가 발생
    - 타입 명시와 타입 추론의 차이
      - `const modals: { ... }[]`로 타입을 명시했지만, filter 이후의 결과가 TypeScript에 의해 자동으로 string 타입으로 추론
      - 결과 배열의 type 속성이 ModalType 대신 string으로 간주되었고, 타입 불일치가 발생
  - 해결 방법
    - 타입 정의를 분리
      - 기존의 타입 정의에서 구성된 null 내용은 따로 분리
      - 기존 타입 정의
        - `type ModalType = "login" | "signup" | "createGroupChat" | null;`
      - 수정 후 타입 정의
        - `type ModalType = "login" | "signup" | "createGroupChat";`
        - `type ActiveModalType = ModalType | null;`
    - filter 내용을 분리해 타입 추론의 안정성 확보
      - 이전 내용 구성
        - 필터링 로직을 분리하지 않고, modals에 바로 연결해 구성
        ```
        const modals: {
          type: ModalType;
          label: string;
          component: React.ComponentType<ModalProps>;
        }[] = [
          { type: "login", label: "로그인", component: Login },
          { type: "signup", label: "회원가입", component: Signup },
          { type: "createGroupChat", label: "+", component: CreateGroupChat },
        ].filter(({ type }) => {
          if (isLoggedIn) {
            return type !== "login" && type !== "signup";
          }
          return type !== "createGroupChat";
        });
        ```
      - 수정 후 내용 구성
        - 필터링 로직을 별도의 변수로 분리하고, 타입을 명확히 지정하면 타입 호환 문제가 발생하지 않음
        ```
        const modals: {
          type: ModalType;
          label: string;
          component: React.ComponentType<ModalProps>;
        }[] = [
          { type: "login", label: "로그인", component: Login },
          { type: "signup", label: "회원가입", component: Signup },
          { type: "createGroupChat", label: "+", component: CreateGroupChat },
        ];
        ```
        ```
        const filteredModals = modals.filter(({ type }) => {
          if (isLoggedIn) {
            return type !== "login" && type !== "signup";
          }
          return type !== "createGroupChat";
        });
        ```
      - 또 다른 해결방법
        - filter 이후에도 원래 타입이 유지되도록 명시적으로 선언할 수 있음
        - "as"를 사용해 결과 타입을 강제하면 문제가 해결됨
        ```
        const modals = [
          { type: "login", label: "로그인", component: Login },
          { type: "signup", label: "회원가입", component: Signup },
          { type: "createGroupChat", label: "+", component: CreateGroupChat },
        ].filter(({ type }) => {
          if (isLoggedIn) {
            return type !== "login" && type !== "signup";
          }
          return type !== "createGroupChat";
        }) as { type: ModalType; label: string; component: React.ComponentType<ModalProps>; }[];
        ```

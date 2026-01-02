# 목차

# ~~🚧 이 항목은 보완 예정입니다. (목적)~~

~~기존 dalla 서비스의 frontend 개발환경은 node 12 버전을 기반으로 운영중임. 보다 최신 버전의 개발환경을 구성하고자 함.~~

# 개발 환경

```jsx
"next: ^15.0.1",
"react: ^18.3.1",
"node: ^20.19.6",
"yarn: ^4.5.1"
```

## Node.js

- 현재 버전: v20.19.6
- 업그레이드 가능한 버전들
    ![v20 이후의 LTS 버전: v22, v24](dalla_next_1.png)
## Next.js / React

- 현재 버전 ⇒ **업데이트 필요!** (우측 내용 참고) ****[https://nextjs.org/blog/security-update-2025-12-11](https://nextjs.org/blog/security-update-2025-12-11)
    
    - React: v18.3.1
    - Next.js: v15.0.1
        - next.js 보안패치를 수행하면 react 또한 19 버전으로 넘어가게 될 것으로 보임.
        - 16으로 업데이트해야하는지는 고민할 필요가 있음.
    
- **업그레이드 해야하는 버전**
    
    - Server Component 관련 취약점 해결을 위해, Next.js 의 경우 최소 v15.0.[^2]7 이상으로 업데이트해야 취약점 대응 가능.
    ![현재 버전(v15.0.1)에서 발견된 취약점](dalla_next_2.png)
    ![보안 패치가 적용된 버전들](dalla_next_3.png)

## Yarn

- 현재 버전: v4.5.1
- 업그레이드 가능한 버전: v4.12.0(latest)
    - yarn 의 경우, node v18 이상이면 호환성 문제 없어 업그레이드 해도 무방해보임.

# 코드 컨벤션 유지를 위한 도구

## 도입 의도

코드 작성 시 규칙을 지정해 서로 다른 개발자들이 작성하는 코드를 최대한 동일한 포맷으로 만들어 가독성 / 유지보수성을 높이고자 함.

## 사용하는 도구

1. eslint
2. prettier

## ~~🚧 이 항목은 보완 예정입니다. (결과 비교)~~

`~~src/pages/clip/ClipPage.tsx` 내에서 이전 이후 비교~~

### ~~‘ ’ / “ ”(single quote 와 double quote) 혼재하는 것 통일~~

### ~~자동 줄 넘김(한 줄에 표시할 최대 글자 수 조정)~~

### ~~개행문자 통일~~

- ~~Windows: `\\r\\n`~~
- ~~Linux, Unix / Mac: `\\n`~~
- ~~운영체제가 혼재하는 환경에서, 실제 코드는 같음에도 개행문자 차이 때문에 git 에서 변경이 생긴 것으로 인식하는 경우가 있음.~~
- ~~이때마다 `git restore '파일이름'` 등으로 변경 사항을 제거해줘야 했던 경험이 있음.~~

# 파일 구조

## Router 방식

- Next.js app router

## API 관리

### Route handler

[Getting Started: Route Handlers and Middleware](https://nextjs.org/docs/15/app/getting-started/route-handlers-and-middleware)

- REST API 를 Next.js 의 Route handler 로 관리하는 방식의 기대 효과
    - **중복 코드 제거(hook 으로 빼는 것 보다 편리)**
    - **API 서버 측과 동일한 route로 API 구조화 해서 효율적인 관리 가능(관심사 분리)**
    - 사용 예시
        - 현재는 BJ 랭킹을 조회하기 위해 `/components/rank-list/api.ts` 같은 파일에서 api 로직을 정의한다면, route handler 를 사용하면 프론트엔드에서도 `app/api/ranking/bj/daily` 형식으로 관리를 할 수 있음.
- 프록시 역할 가능
    - 실제 api 서버의 도메인 은닉화
    - JWT 은닉화
- REST 규격 이외의 서버 통신 방식
    - hook 으로 분리 관리
    - 기존 api 관리 방식

### Server Function(Server Action)

- 별도의 Route handler 없이, 기존처럼 바로 fetch 요청을 보낼 수 있어 Form 제출 시 유용함.

### Server Component

- 실제 페이지 중 Server Component 로 구현할 수 있는지 없는지 확인해봐야 적용 여부 판단가능.

### json-server 도입

[GitHub - typicode/json-server at v0](https://github.com/typicode/json-server/tree/v0)

- 도입 의도
    - 현재 단계에서는 migration 작업 시 다른 파트의 도움을 받기 어렵다고 판단해, 실 DB 데이터 구조를 모방한 서버를 구축하고자 함.
- Swagger 명세서를 기반으로 Mock server 구축해서 서버 의존성 줄여 개발 속도 향상 목표.
    - 각각 route 마다 필요한 API 나 데이터가 필요함.
    - 개발 단계에서 type 정의 파일과 함께 사용할 예정

### 각 route 별 api endpoint 관리 도구

- 링크
    - [https://github.com/kybaq/har-tool](https://github.com/kybaq/har-tool)
- 도입 의도
    - migration 작업 시, 어떤 페이지에서 어떤 api 를 호출하고 있는지 파악해야 원활한 기능 구현이 가능하다고 판단.
    - json-server 를 통한 mock server 구축 시 필요.
- 방법
    - 로컬 환경에서 사용 가능한 proxy 를 구현해, 브라우저에서 발생하는 네트워크 트래픽을 가로채 기록함.
- 결과
    
    ![예시 1](attachment:231d25f3-6bd3-41bd-8383-26e62df624be:image.png)
    
    예시 1
    
    ![예시 2](attachment:e52b2c7c-7163-42d6-8e59-122eafa55588:image.png)
    
    예시 2
    

## 상태 관리

### zustand

- agora 로 부터 오는 데이터는 어떻게 관리하는지??
    - saga 에서 사용하는 제너레이터를 반드시 써야하는지, 그렇지 않다면 대체가 가능한지 파악해야함.

### tanstack-query(前 react-query)

- 필요할지 아닐지 판단하기 이르다고 판단.
- Next.js 자체 캐싱 기능 활용해서 커버가 된다면 필요 없음.

## css 소스 관리

- scss(sass)
    - tailwind 로 새로 셋업해서 얻는 장점이 많지 않다고 판단함. 빠른 프로토타이핑엔 도움이 될 수 있지만..?

(향후 작업)

일단 route 마다 사용하는 API 정리.

각 페이지 작업할 때마다 Swagger 명세서 참고해 type 정의 및 Mock Server 구현.

json server 로 데이터 구조에 맞게 mock server 구현해서 페이지 작업 시 api 호출 로직까지 붙여서 구현해나가는 방식.

### 라이브러리 목록





---
layout: post
title: 커스텀 마크다운 에디터 만들기 1
subtitle:
categories: frontend
tags: [frontend, develop]
---

블로그나 깃허브를 이용하면서 마크다운 에디터를 사용한 경험이 많습니다.

이런 마크다운 에디터를 직접 만들고 나만의 규칙을 생성하기 위한 프로젝트를 생성하였고, 해당 글은 커스텀 마크다운 에디터를 만드는 과정을 메모하는 글입니다.

## 프로젝트 세팅

프로젝트 구성

- [React](https://react.dev/learn/start-a-new-react-project)
- [Typescript](https://www.typescriptlang.org/download)
- [Nextjs 14](https://nextjs.org/blog/next-14)
- [Redux Toolkit](https://redux-toolkit.js.org/introduction/getting-started)

프로젝트는 위와 같은 구성으로 세팅을 했습니다.

### Next.js 14

이번 프로젝트에서는 Next.js 14버전을 사용해서 해보려고 합니다.
![nextjs14](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/a23a4fab-4ed1-4e1f-90ab-96696b6a37c4)

Next.js의 슬랙채널을 통해 [Next.js 14버전](https://nextjs.org/blog/next-14)의 소식을 듣고,
한 번 써보고 싶다는 생각이 들어 이번 프로젝트에서 14버전을 선택했습니다.
![slack](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/50286758-dfa5-4751-adf6-70a2787553de)특히 아래의 기능적 업데이트 내용을 직접 경험해보고 싶어 Next.js 14 버전을 선택했습니다.

1. Turbopack
   - 로컬 서버의 시작 속도 53% 향상
   - Fast Refresh로 코드 업데이트 속도 94% 개선
2. Server Actions 기능 안정화
   - 서버에서 실행되는 함수를 정의하고 리액트 컴포넌트에서 직접 호출을 할 수 있는 기능
   - 단순 함수 호출이 가능하며, 폼과도 원활하게 동작

<br/>리액트만을 사용하다가 Nextjs를 사용해보신 분들이라면 파일 시스템 기반 라우팅을 사용하면서 따로 라우팅 설정을 하지 않아도 파일 구조를 이용하여 라우팅을 할 수 있어서 훨씬 편해졌던 것을 느꼈을 것입니다.

특히 13버전부터는 app 라우팅 방식이라고 해서 기존 next에서 pages/ 디렉터리로 라우팅 하던 방식을 app/ 디렉터리 방식으로
라우팅하도록 바뀌었습니다.

14버전에서도 동일하게 app/라우팅 방식으로 사용합니다. 자세한 라우팅 방식에 대해서는
[Next.js 공식문서](https://nextjs.org/docs/app/building-your-application/routing)를 참고하시면 이해가 쉬울 것입니다.

## markdown 에디터 라이브러리 세팅

커스텀 마크다운 에디터를 위해 설치한 라이브러리는 [@uiw/react-md-editor](https://www.npmjs.com/package/@uiw/react-md-editor)와 [react-markdown](https://github.com/remarkjs/react-markdown)입니다.
에디터로는 uiw의 프로젝트 중 react-codemirror를 사용해본 경험이 있어서 **@uiw/react-md-editor**로 선택했고, **react-markdown**은 마크다운으로 작성한 것을 html로 파싱하여 보여주는 화면을 위해 선택했습니다.
react-markdown은 커스텀 태그를 만들어서 보여주기에 용이하다고 생각해서 선택했습니다.

라이브러리 설치를 위해 아래의 명령어를 터미널에 입력합니다.

```
// @uiw/react-md-editor 설치
npm i @uiw/react-md-editor

// react-markdown 설치
npm install react-markdown
```

설치가 완료되면 코드를 작성합니다.

저는 값을 리듀서에 저장을 해주었기때문에 아래와 같이 작성했습니다.

```typescript
const [value, setValue] = useState(`**Hello world!!!** `);

return (
  <>
    <MDEditor height={"50vh"} value={value} preview="edit" />
    <ReactMarkdown>{value}</ReactMarkdown>
  </>
);
```

이렇게 코드를 작성하면
![md](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/6f696bf8-a2c9-451e-b8d1-8977336f2daf)
위와 같은 화면이 나옵니다.
위쪽의 검은색 컴포넌트가 MDEditor로 에디터 컴포넌트입니다.
에디터 컴포넌트 아래에 ReactMarkdown가 보여지면서 에디터에 작성한 마크다운이 html로 보여집니다.

```typescript
<Box data-color-mode="light">
  <MDEditor height={"50vh"} value={value} preview="edit" />
</Box>
```

MDEditor를 감싸는 상위 컴포넌트에
**data-color-mode**를 **light**로 지정하면
![mdLight](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/ebdae2e6-2a78-44a6-af25-3360601adf2e)
위와 같이 흰색 배경으로 바뀌게 됩니다.

또한 **preview** 옵션을 사용하면 에디터와 뷰어를 따로 분리할 수도 있습니다.
preview 옵션을 설정하지 않으면 기본값은 live입니다.

- 에디터 화면

```typescript
<MDEditor height={'10vh'} value={value} preview="edit" />
<MDEditor height={'10vh'} value={value} preview="live" />
<MDEditor height={'10vh'} value={value} preview="preview" />
```

![mdEditor](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/aedc8c2a-68b4-4ec4-9e87-9d7cd58f0968)

- 참고
  - [Next.js 공식문서](https://nextjs.org/docs/app/building-your-application/routing)
  - [@uiw/react-md-editor](https://www.npmjs.com/package/@uiw/react-md-editor)
  - [react-markdown](https://github.com/remarkjs/react-markdown)

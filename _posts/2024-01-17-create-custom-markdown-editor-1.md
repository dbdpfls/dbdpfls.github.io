---
layout: post
title: 커스텀 마크다운 에디터 만들기 1
subtitle:
categories: frontend
banner:
  image: https://github.com/Ragon-Labs/ragon/assets/103565462/6c22935c-0b33-4015-b77f-f1e45c1748c9
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

커스텀 마크다운 에디터를 위해 설치한 라이브러리는 [@uiw/react-md-editor](https://www.npmjs.com/package/@uiw/react-md-editor) 입니다.
uiw의 프로젝트 중 react-codemirror를 사용해본 경험이 있어서 해당 라이브러리로 선택했습니다.

라이브러리 설치를 위해 아래의 명령어를 터미널에 입력합니다.

```
npm i @uiw/react-md-editor
```

설치가 완료되면 코드를 작성합니다.

저는 값을 리듀서에 저장을 해주었기때문에 아래와 같이 작성했습니다.

```typescript
export default function TestView() {
  const dispatch = useAppDispatch();
  const mdContent = useAppSelector((state) => state.mdReducer.content);

  const handleChangeValue = (e: string | undefined) => {
    dispatch(changeContent(e));
  };

  return (
    <Box>
      <MDEditor value={mdContent} onChange={(e) => handleChangeValue(e)} />
    </Box>
  );
}
```

이렇게 코드를 작성하면
![md](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/094ab08f-66ac-47f5-937b-7203dacd2070)
위와 같은 화면이 나옵니다.
기본적으로 왼쪽은 에디터 오른쪽은 뷰어입니다.

```typescript
<Box data-color-mode="light">
  <MDEditor value={mdContent} onChange={(e) => handleChangeValue(e)} />
</Box>
```

**data-color-mode**를 **light**로 지정하면
![mdLight](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/c828eafb-128f-403d-bd49-ea853e603970)
위와 같이 흰색 배경으로 바뀌게 됩니다.

또한 **preview** 옵션을 사용하면 에디터와 뷰어를 따로 분리할 수도 있습니다.
preview 옵션을 설정하지 않으면 기본값은 live입니다.

- 에디터 화면

```typescript
<MDEditor
  height={"50vh"}
  value={mdContent}
  onChange={(e) => handleChangeValue(e)}
  preview="edit"
/>
```

![mdEditor](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/36c51e3c-6e29-4880-88b9-7acd916a880e)

- 뷰어 화면

```typescript
<MDEditor height={"50vh"} value={mdContent} preview="preview" />
```

![mdViewer](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/926029fe-0a7f-419c-97cc-e0426e1eff33)

위에서 말한 옵션들을 사용하면 각각의 화면은 아래와 같이 보여집니다.
![total](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/8f85af61-53f1-4682-8d20-2b386bd45636)

- 참고
  - [Next.js 공식문서](https://nextjs.org/docs/app/building-your-application/routing)
  - [@uiw/react-md-editor](https://www.npmjs.com/package/@uiw/react-md-editor)

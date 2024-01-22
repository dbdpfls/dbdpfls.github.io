---
layout: post
title: 커스텀 마크다운 에디터 만들기 3
subtitle:
categories: frontend
tags: [frontend, develop]
---

드디어 마지막 단계인 커스텀 태그를 만들어 주는 단계입니다.

먼저 제가 지정하는 태그를 분석할 수 있도록 해주는 라이브러리인 [rehype-raw](https://www.npmjs.com/package/rehype-raw)를 사용해야 합니다.
아래의 명령어를 통해 설치를 합니다.

```
npm install rehype-raw
```

설치 후 react-markdown에 적용을 해줍니다.
rehypePlugins에 rehypeRaw를 추가해주면 적용이 됩니다.

```typescript
<ReactMarkdown
  rehypePlugins={[rehypeRaw]}
  disallowedElements={sanitizeTagNames}
>
  {value}
</ReactMarkdown>
```

react-markdown에 커스텀 태그를 만들기 위해서는 components 옵션에 만들고 싶은 태그 이름과 스타일을 지정해주면 됩니다.

저는 test라는 이름의 태그를 만들어서 입력한 값의 색깔이 orange로 보이도록 해줄 것입니다.

```typescript
<ReactMarkdown
  rehypePlugins={[rehypeRaw]}
  disallowedElements={sanitizeTagNames}
  components="{{
    // @ts-ignore
    test: (test) => {
      if (test.children) {
        return (
          <Typography style={{ color: "orange" }}>{test.children}</Typography>
        );
      }
    },
  }}"
>
  {value}
</ReactMarkdown>
```

위 코드처럼 components에 test라는 이름으로 컴포넌트를 설정해주고 return을 해주면 완성입니다.
저는 타입스크립트로 코드를 작성해서 test: (test)에서 타입 관련 에러가 발생하여 // @ts-ignore를 설정해주었습니다.

결과 화면은 아래와 같이 보여집니다.
![image](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/18d4efd6-6a2e-4b78-be8b-1df053aa8ec9)

- 참고
  - [How to use react-markdown to create custom React components](https://www.kristijorgji.com/blog/how-to-use-react-markdown-to-create-custom-react-components/)
  - [rehype-raw](https://github.com/rehypejs/rehype-raw)

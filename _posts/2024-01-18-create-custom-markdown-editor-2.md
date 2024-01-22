---
layout: post
title: 커스텀 마크다운 에디터 만들기 2
subtitle:
categories: frontend
tags: [frontend, develop]
---

커스텀 마크다운 에디터를 만들기에 앞서 markdown이 html로 어떻게 변환되는지 간단히 알아보겠습니다.

## markdown ➡️ html 변환

마크다운을 html로 변환할 때는 AST(Abstract Syntax Tree)가 사용됩니다.
AST는 소스 코드 구조를 트리 형태로 표현한 것입니다.

예시를 보여드리겠습니다.
제가 작성한 마크다운이 AST로 표현하면 아래처럼 보여집니다. 직접 코드를 입력하여 확인해보고 싶으신 분들은 [AST Explorer](https://astexplorer.net/) 확인할 수 있습니다.
![ast_explorer](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/cb10c816-26b1-4a15-b753-7f0a42007804)

소스코드를 AST로 변환하는 과정은 어휘 분석, 구문 분석, AST 생성 단계로 나뉘어집니다.

- 어휘 분석

  어휘 분석 단계는 토큰화 단계라고도 합니다. 코드를 읽고
  공백, 주석 등도 제거하여 소스 코드를 토큰으로 분할합니다.

- 구문 분석

  구문 분석 단계에서는 토큰을 실제 소스코드 구조의 트리로 변환합니다.

- AST 생성

  위 단계를 거친 후 AST가 생성됩니다.

## html 파서 필터 만들기

마크다운에서는 기본적으로 html 태그는 모두 지원합니다. 하지만 보안 이슈 발생 위험이 있기 때문에 일부 태그는 사용하지 못하도록 막아두는 것이 안전합니다.

#### 허용하지 않는 태그

- `<script>`
- `<iframe>, <noembed>, <noframes>`
- `<link>, <style>`
- `<embed>, <object>`
- `<meta>`
- `<input>, <button>, <select>, <textarea>, <form>`
- `<frame>, <frameset>`
- `<textarea>`
- `<xmp>`

위 태그들은 악성 스크립트나 악의적으로 페이지 또는 컨텐츠를 변경할 수 있으므로 사용하지 못하도록 설정할 것입니다.

위 태그가 사용되지 않도록 하려면 html 뷰어 화면에서 설정이 되어야 하기에

react-markdown의 disallowedElements 옵션에 허용하지 않을 태그 목록을 넣어줍니다.

```typescript
  const sanitizeTagNames = [
    'script',
    'iframe',
    'noembed',
    'noframes',
    'style',
    'embed',
    'object',
    'meta',
    'input',
    'button',
    'Button',
    'select',
    'textarea',
    'form',
    'frame',
    'frameset',
    'textarea',
    'xmp',
  ];

  return (
      <ReactMarkdown disallowedElements={sanitizeTagNames}>{value}</ReactMarkdown>
  );
};
```

위와 같이 코드를 작성하고 설정이 잘 되었는지 확인해봅니다.

으로 작성해주면 적용이 됩니다.

적용이 되었는지 테스트를 위해 아래처럼 코드를 작성하여 확인합니다.

```typescript
const [value, setValue] = useState(
  `**Hello world!!!** <IFRAME SRC=\"javascript:javascript:alert(window.origin);\"></IFRAME>`
);
```

결과 화면을 보면 IFRAME 태그를 인식하지 않고 텍스트로 보여주는 것을 확인할 수 있습니다.
![disallowedElements](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/273bfa1f-e75c-4a01-a07e-1d5f7bc8514a)

- 참고
  - [자바스크립트 개발자를 위한 AST(번역)](https://gyujincho.github.io/2018-06-19/AST-for-JS-devlopers)
  - [ASTs - What are they and how to use them](https://www.twilio.com/blog/abstract-syntax-trees)

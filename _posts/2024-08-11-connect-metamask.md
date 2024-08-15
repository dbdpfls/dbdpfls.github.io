---
layout: post
title: 메타마스크 연동 로그인
subtitle:
categories: frontend
tags: [frontend, develop]
---

많은 DApp에서는 메타마스크로 서비스에 로그인을 할 수 있습니다.

<div style="display: flex;">
  <img src="https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/2add24b5-505f-4251-9fa9-365d8a1d681d" alt="OpenSea" style="margin-right: 10px;">
  <img src="https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/618653ef-9059-476e-be83-8798d33a2f3a" alt="UniSwap">
</div>

<br>
이번 글은 리액트에서 메타마스크를 연동하여 로그인하는 방법에 대해 기록합니다.
<br><br>

## 프로젝트 세팅

프로젝트는 아래와 같이 세팅되어 있습니다.

- React
- Typescript
- Nextjs 14
- Redux Toolkit

## wagmi 설치

[wagmi](https://wagmi.sh/)는
이더리움과 상호작용하는 애플리케이션 개발을 더 쉽게 구축할 수 있도록 도와주는 라이브러리입니다.
메타마스크를 연동하기 위해서 ethers 설치해줍니다.

```
npm i wagmi
```

설치가 되었으면 메타마스크와 연동하기 위한 코드를 작성합니다.

## Connect MetaMask

공식 문서에서 [Connectors](https://wagmi.sh/react/api/connectors)를 확인해보면 **injected**와 **metaMask**가 있습니다. 이 두 커넥터를 사용해서 메타마스크를 연결 할 수 있습니다.

- injected: 다양한 EIP-1193 호환 지갑을 지원하며, 설정을 통해 다양한 커스터마이징이 가능합니다.
- metaMask: MetaMask와의 통합을 최적화하고 다양한 SDK 설정 옵션을 제공합니다.

먼저 아래와 같이 이더리움 네트워크 연결을 설정하는 파일을 작성합니다.

```typescript
import { http, cookieStorage, createConfig, createStorage } from "wagmi";
import { mainnet, sepolia } from "wagmi/chains";
import { injected, metaMask } from "wagmi/connectors";

export function getConfig() {
  return createConfig({
    chains: [mainnet, sepolia], // 사용할 체인 목록
    connectors: [injected(), metaMask()], // 커넥터 목록
    storage: createStorage({
      // 저장할 스토리지
      storage: cookieStorage,
    }),
    ssr: true, // 서버 사이드 렌더링 환경 사용 여부
    transports: {
      // JSON-RPC API와의 HTTP 연결을 설정 (예시: Alchemy, Infura, ...)
      [mainnet.id]: http("JSON-RPC_엔드포인트_URL/YOUR_ALCHEMY_KEY"),
      [sepolia.id]: http("JSON-RPC_엔드포인트_URL/YOUR_INFURA_PROJECT_ID"),
    },
  });
}
```

프로바이더도 아래와 같이 작성합니다.

```typescript
"use client";

import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { type ReactNode, useState } from "react";
import { type State, WagmiProvider } from "wagmi";

import { getConfig } from "@/wagmi";

export function Providers(props: {
  children: ReactNode;
  initialState?: State;
}) {
  const [config] = useState(() => getConfig());
  const [queryClient] = useState(() => new QueryClient());

  return (
    <WagmiProvider config={config} initialState={props.initialState}>
      <QueryClientProvider client={queryClient}>
        {props.children}
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```

이제 메타마스크와 연결하고 해제하는 코드를 작성합니다.

```typescript
"use client";

import { useConnect, useDisconnect } from "wagmi";
import { injected, metaMask } from "wagmi/connectors";

export default function ConnectWallet() {
  const { connect } = useConnect(); // 연결
  const { disconnect } = useDisconnect(); //해제

  const handleConnect = () => {
    connect(
      { connector: injected() },
      {
        onSuccess: (data, variables, context) => {
          console.log("Connected!", data);
        },
        onError: (error, variables, context) => {
          console.error("Connection failed", error);
        },
        onSettled: (data, error, variables, context) => {
          console.log("Connection settled", { data, error });
        },
      }
    );
  };

  return (
    <section className="test-section">
      <button className="test-button" onClick={handleConnect}>
        Connect
      </button>
      <button
        className="test-button"
        onClick={() => connect({ connector: metaMask() })}
      >
        MetaMask
      </button>
      <button className="test-button" onClick={() => disconnect()}>
        Disconnect
      </button>
    </section>
  );
}
```

**useConnect**와 **useDisconnect**는 wagmi에서 제공하는 훅(Hook)입니다.

- [useConnect](https://wagmi.sh/react/api/hooks/useConnect): 지갑 연결 기능을 쉽게 구현하기 위한 Hook입니다.
  - connect: 지갑을 특정 체인 및 커넥터와 연결할 때 사용됩니다. 연결 과정에서 발생하는 다양한 상태(성공, 실패, 완료 등)를 처리하기 위한 여러 콜백 함수를 받을 수 있습니다.
- [useDisconnect](https://wagmi.sh/react/api/hooks/useDisconnect): 지갑 연결 해제 기능을 구현하기 위한 Hook입니다.
  - disconnect: 지갑 연결을 해제할 때 사용됩니다. 이 함수도 연결 해제 과정에서 발생하는 다양한 상태(성공, 실패, 완료 등)를 처리하기 위한 여러 콜백 함수를 받을 수 있습니다.

![develop_page](https://github.com/user-attachments/assets/e13eb309-4e5d-4f9f-9d44-90b90b7380c4)

## Error

개발하다가 발생한 에러입니다.

```typescript
Error: (0 , wagmi__WEBPACK_IMPORTED_MODULE_1__.useConnect) is not a function

위처럼 나오는 에러는 "use client"; 를 추가하면 해결
```

---
layout: post
title: 메타마스크 연동 트랜잭션
subtitle:
categories: frontend
tags: [frontend, develop]
---

이전 글에서는 메타마스크 연결 및 해제하는 방법을 기록했습니다.
이번 글에서는 이전 글에 대해 좀 더 발전시키고, 트랜잭션을 발생시키는 방법까지 기록하겠습니다.

## 프로젝트 세팅

프로젝트는 아래와 같이 세팅되어 있습니다.

- React
- Typescript
- Nextjs 14
- Redux Toolkit
- [wagmi](https://wagmi.sh/)

## 현재 Account 정보

<div style="display: flex;">
  <img src="https://github.com/user-attachments/assets/0c86cc21-4448-4d47-8d9e-51180dbd1f07" alt="connect" style="margin-right: 10px;">
  <img src="https://github.com/user-attachments/assets/2e926ee4-464d-4969-b323-63b7c1d1a819" alt="disconnect">
</div>
</br>

메타마스크를 연결 한 후 연결 된 account 정보를 가져오기 위해 **useAccount**를 사용하여 계정을 연결 및 해제를 하고 계정에 대한 정보를 확인하는 코드를 작성합니다.

```typescript
"use client";

import { useAccount, useConnect, useDisconnect } from "wagmi";
import { injected, metaMask } from "wagmi/connectors";

export default function ConnectWallet() {
  const { connect } = useConnect();
  const { isConnected, address, chain, status } = useAccount();
  const { disconnect } = useDisconnect();

  const formattedAddress = formatAddress(address);
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
    <div className="container">
      <div className="title">Connect</div>
      <div className="status">{"status: " + status}</div>

      <div className="content">
        {isConnected ? (
          <>
            {chain && (
              <span>{chain.name + " Account: " + formattedAddress}</span>
            )}
            <button className="button" onClick={() => disconnect()}>
              Disconnect
            </button>
          </>
        ) : (
          <div>
            <button className="button" onClick={handleConnect}>
              Connect
            </button>
            <button
              className="button"
              onClick={() => connect({ connector: metaMask() })}
            >
              MetaMask
            </button>
          </div>
        )}
      </div>
    </div>
  );
}

function formatAddress(address?: string) {
  if (!address) return null;
  return `${address.slice(0, 6)}…${address.slice(38, 42)}`;
}
```

- [useAccount](https://wagmi.sh/react/api/hooks/useAccount): 현재 연결된 계정 정보를 가져오기 위한 Hook입니다.

  - isConnecting, isReconnecting, isConnected, isDisconnected: 계정 상태를 나타내는 boolean 값입니다.
  - address: 연결된 계정의 주소입니다.
  - chain: 연결된 계정의 네트워크 정보입니다.
  - status: 연결된 계정의 상태를 나타내는 문자열입니다. 가능한 값은 다음과 같습니다:
    - connecting: 연결을 시도 중임을 나타냅니다.
    - connected: 연결이 성공적으로 완료되었음을 나타냅니다.
    - reconnecting: 재연결을 시도 중임을 나타냅니다.
    - disconnected: 연결이 끊어졌음을 나타냅니다.

## Transaction

<div style="display: flex;">
  <img src="https://github.com/user-attachments/assets/6db9a8fe-d758-4480-b312-18a9c52c0d37" alt="connect" style="margin-right: 10px;">
  <img src="https://github.com/user-attachments/assets/bd92c78c-8200-4759-98d2-98c3a1b6195b" alt="disconnect">
</div>
</br>

**useSendTransaction** Hook을 사용하여 트랜잭션을 생성하고,
**useWaitForTransactionReceipt** Hook을 사용하여 트랜잭션의 상태를 확인하는 코드를 작성합니다.

```typescript
"use client";

import { parseEther } from "ethers";
import {
  BaseError,
  useSendTransaction,
  useWaitForTransactionReceipt,
} from "wagmi";
import Link from "next/link";

export default function Transaction() {
  const {
    data: hash,
    error,
    isPending,
    sendTransaction,
  } = useSendTransaction();

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const formData = new FormData(e.target as HTMLFormElement);
    const to = formData.get("address") as `0x${string}`;
    const value = formData.get("value") as string;
    sendTransaction({ to, value: parseEther(value) });
  };

  const { isLoading: isConfirming, isSuccess: isConfirmed } =
    useWaitForTransactionReceipt({
      hash,
    });

  return (
    <div className="container">
      <div className="title">Transaction</div>

      <div className="content">
        <form className="form" onSubmit={handleSubmit}>
          <input
            className="input"
            name="address"
            placeholder="enter your address"
            required
          />
          <input className="input" name="value" placeholder="value" required />
          <button className="button" disabled={isPending} type="submit">
            {isPending ? "Confirming..." : "Send"}
          </button>
          {hash && (
            <Link href={`https://sepolia.etherscan.io/tx/${hash}`}>
              <div>Transaction Hash: {hash}</div>
            </Link>
          )}
          <div className="result">
            <span className="title">Result</span>
            {isConfirming && <div>Waiting for confirmation...</div>}
            {isConfirmed && <div>Transaction confirmed.</div>}
            {error && (
              <div className="error">
                Error: {(error as BaseError).shortMessage || error.message}
              </div>
            )}
          </div>
        </form>
      </div>
    </div>
  );
}
```

- [useSendTransaction](https://wagmi.sh/react/api/hooks/useSendTransaction): 트랜잭션을 생성, 서명, 전송하는 Hook입니다.

  - data: 트랜잭션이 성공한 후 반환되는 데이터입니다.
  - error: 트랜잭션 실패 시 반환되는 오류 객체입니다.
  - isError, isIdle, isPending, isSuccess: 트랜잭션 상태를 나타내는 boolean 값입니다.
  - sendTransaction: 트랜잭션을 전송하는 함수입니다. 트랜잭션을 전송할 때 주소와 값을 인자로 받아야 합니다.

- [useWaitForTransactionReceipt](https://wagmi.sh/react/api/hooks/useWaitForTransactionReceipt): 트랜잭션이 블록체인에 포함될 때까지 대기한 후 트랜잭션 영수증을 반환받는 Hook입니다.
  - hash: 영수증을 대기할 트랜잭션의 해시 값입니다.

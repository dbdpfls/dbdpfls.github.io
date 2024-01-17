---
layout: post
title: 니모닉 지갑
subtitle:
categories: blockchain
banner:
  image: https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/4ee113e1-6867-45ea-ba61-cf94218e4642
tags: [blockchain]
---

## 니모닉(Mnemonic)

니모닉(Mnemonic)은 지갑을 복구하기 위한 12개의 단어로 암호화폐 지갑의 개인키를 쉽게 입력할 수 있도록 만들어진 형식입니다.

#### 암호화폐 지갑

암호화폐 지갑은 블록체인 네트워크와 상호 작용하는 수단 중 하나입니다. 지갑은 주소와 개인키로 구성되어 있는데 주소는 암호화폐 거래를 위해 공개해도 괜찮지만 개인키는 유출이 되면 안됩니다.

#### 니모닉 월렛

니모닉 월렛은 니모닉을 사용해서 개인키(비밀키) 관리를 쉽게 해주는 암호화폐 지갑입니다.

### 니모닉 코드로 시드 생성하는 과정

![니모닉1](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/af4f07a5-9bae-4538-b58c-7f7881cfc4db)

1. 128bit 또는 256bit 길이의 난수를 생성합니다.
2. 난수를 SHA-256으로 해싱하고 (시드 키의 길이) / 32 만큼 떼어내어 체크섬으로 만듭니다.
3. 체크섬을 난수 뒤에 붙입니다.
4. 체크섬을 붙인 난수를 11bit 단위로 자릅니다.
5. 11bit로 잘라낸 각각의 단어를 사전에 정의된 단어로 치환합니다.
6. 각 11bit의 순서에 맞게 니모닉 코드를 생성합니다.
7. PBKDF2 키 스트레칭 함수는 에 위에서 생성된 니모닉 코드와 솔트를 연결하여 인자를 구성합니다.
8. PBKDF2 키 스트레칭 함수는 HMAC-SHA512 알고리즘을 사용해서 2048 해시 라운드를 통해 니모닉과 솔트 파라미터를 확장합니다.
9. 512bit의 Seed가 생성되었습니다.

### 니모닉 지갑 개발

eth-lightwallet 모듈을 이용해 간단한 니모닉 지갑을 개발하고 Postman으로 API 테스트를 할 수 있습니다.

- API 테스트를 위한 Postman을 설치합니다.
  <a href="https://www.postman.com/downloads/">Postman 설치</a>

---

- 니모닉 코드 생성을 위한 API입니다.

```javascript
app.post("/getMnemonic", async (req, res) => {
  try {
    const mnemonic = lightwallet.keystore.generateRandomSeed();
    return res.json({ mnemonic });
  } catch (err) {
    console.log(err);
  }
});
```

![니모닉개발1](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/7ba5ae86-73a2-4d78-925f-9238ebecb58b)

---

- 위에서 생성된 니모닉 코드와 패스워드를 이용해 새로운 지갑을 생성하는 API입니다.

```javascript
app.post("/getWallet", async (req, res) => {
  let password = req.body.password;
  let mnemonic = req.body.mnemonic;

  try {
    lightwallet.keystore.createVault(
      {
        password: password,
        seedPhrase: mnemonic,
        hdPathString: "m/0'/0'/0'",
      },
      function (err, ks) {
        ks.keyFromPassword(password, function (err, pwDerivedKey) {
          ks.generateNewAddress(pwDerivedKey, 1);

          const address = ks.getAddresses().toString();
          const keystore = ks.serialize();

          res.json({ keystore: keystore, address: address });
        });
      }
    );
  } catch (err) {
    console.log(err);
  }
});
```

![니모닉개발2](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/09c97f87-3b8a-415d-98e9-776af025463d)
Body에 위에서 생성된 니모닉 코드와 원하는 패스워드를 JSON 형태로 입력한 후 send 버튼을 클릭합니다.

---

- 생성된 keystore를 json 파일로 로컬에 저장하는 API입니다.

```javascript
app.post("/getWallet", async (req, res) => {
  let password = req.body.password;
  let mnemonic = req.body.mnemonic;

  try {
    lightwallet.keystore.createVault(
      {
        password: password,
        seedPhrase: mnemonic,
        hdPathString: "m/0'/0'/0'",
      },
      function (err, ks) {
        ks.keyFromPassword(password, function (err, pwDerivedKey) {
          ks.generateNewAddress(pwDerivedKey, 1);

          const address = ks.getAddresses().toString();
          const keystore = ks.serialize();

          fs.writeFile("wallet.json", keystore, function (err, data) {
            if (err) {
              return res.json({ code: 999, message: "실패" });
            }
            return res.json({ code: 1, message: "성공" });
          });
        });
      }
    );
  } catch (err) {
    console.log(err);
  }
});
```

![니모닉개발3](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/088bf3a0-3245-4903-ba60-4caae18119c3)
위와 같이 니모닉 코드와 원하는 패스워드를 JSON 형태로 입력한 후 send 버튼을 클릭합니다.
![니모닉개발4](https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/3ea0306a-db35-474b-82f9-3a99fc88e390)
로컬에 저장된 파일을 확인할 수 있습니다.

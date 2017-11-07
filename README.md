# FIDO
본 페이지는 FIDO 표준 프로토콜을 정리하는 페이지입니다.



## A. Register Operation

![FIDO_register_operation_diagram](.\img\FIDO_register_operation_diagram.png)

### 1. RegistrationRequest dictionary

```
dictionary RegistrationRequest {
  required OperationHeader header;
  required ServerChallenge challenge;
  required DOMString       username;
  required Policy          policy;
};
```

* 이 때 반드시 Header.op must be"Reg"로 설정되어 있어야 하는데 이 과정이 등록 과정이기 때문이다.

* ServerChallenge는 서버에서 보내는 랜덤한 값으로 나중에 서버쪽으로 답변이 올 때

  * 이미 처리된 요청인지
  * 유효한 기간이 지났는지

  판단하기 위해서 보내는 값이다.

* Username 부분은 사람이 읽을 수 있는 사용자의 이름으로 서버에서 사용자를 구분하기 위해 사용된다.

* Policy 부분은 잘 이해가 안된다.

#### 예시 코드

<pre><code>

```
[{
    "header": {
      "upv": {
        "major": 1,
        "minor": 1
      },
      "op": "Reg",
 "appID": "https://uaf-test-1.noknoktest.com:8443/SampleApp/uaf/facets",
      "serverData": "IjycjPZYiWMaQ1tKLrJROiXQHmYG0tSSYGjP5mgjsDaM17RQgq0
dl3NNDDTx9d-aSR_6hGgclrU2F2Yj-12S67v5VmQHj4eWVseLulHdpk2v_hHtKSvv_DFqL4n
2IiUY6XZWVbOnvg"
    },
    "challenge": "H9iW9yA9aAXF_lelQoi_DhUk514Ad8Tqv0zCnCqKDpo",
    "username": "apa",
    "policy": {
      "accepted": [
        [
          {
            "userVerification": 512,
            "keyProtection": 1,
            "tcDisplay": 1,
            "authenticationAlgorithms": [
              1
            ],
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          }
        ],
        [
          {
            "userVerification": 4,
            "keyProtection": 1,
            "tcDisplay": 1,
            "authenticationAlgorithms": [
              1
            ],
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          }
        ],
        [
          {
            "userVerification": 4,
            "keyProtection": 1,
            "tcDisplay": 1,
            "authenticationAlgorithms": [
              2
            ]
          }
        ],
        [
          {
            "userVerification": 2,
            "keyProtection": 4,
            "tcDisplay": 1,
            "authenticationAlgorithms": [
              2
            ]
          }
        ],
        [
          {
            "userVerification": 4,
            "keyProtection": 2,
            "tcDisplay": 1,
            "authenticationAlgorithms": [
              1,
              3
            ]
          }
        ],
        [
          {
            "userVerification": 2,
            "keyProtection": 2,
            "authenticationAlgorithms": [
              2
            ]
          }
        ],
        [
          {
            "userVerification": 32,
            "keyProtection": 2,
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          },
          {
            "userVerification": 2,
            "authenticationAlgorithms": [
              1,
              3
            ],
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          },
          {
            "userVerification": 2,
            "authenticationAlgorithms": [
              1,
              3
            ],
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          },
          {
            "userVerification": 4,
            "keyProtection": 1,
            "authenticationAlgorithms": [
              1,
              3
            ],
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          }
        ]
      ],
      "disallowed": [
        {
          "userVerification": 512,
          "keyProtection": 16,
          "assertionSchemes": [
            "UAFV1TLV"
          ]
        },
        {
          "userVerification": 256,
          "keyProtection": 16
        },
        {
          "aaid": [
            "ABCD#ABCD"
          ],
          "keyIDs": [
            "RfY_RDhsf4z5PCOhnZExMeVloZZmK0hxaSi10tkY_c4"
          ]
        }
      ]
    }
}]
```

</code></pre>

### 2. RegistrationRequest dictionary

<pre><code>

```
dictionary RegistrationResponse {
    required OperationHeader                      header;
    required DOMString                            fcParams;
    required AuthenticatorRegistrationAssertion[] assertions;
};
```

</code></pre>

* fcParams of type required DOMString은 FacetID의 리스트로 Base64url로 인코딩되어 전달된다. (appID로 확인된 FacetID를 서버에서 확인하여 일치 하는지 확인하는 과정에 사용되는데, 이 과정이 필요한 이유는 FacetID는 Client의 정보이고 appID는 사용자 앱의 정보이기 때문에 이 앱이 서버에서 제공하는 서비스인지 판단하고 FacetID를 통해서 이 서비스가 허용하는 Client 앱인지 판단하기 위해서 사용된다.
* Assertions는 등록된 인증기들의 데이터 리스트로, 사용자는 서버가 보낸 이 인증기 중에서 Client는 인증을 진행하도록 한다.

#### 예시 코드

<pre><code>

```
[{
    "assertions": [
      {
        "assertion": "AT7uAgM-sQALLgkAQUJDRCNBQkNEDi4HAAABAQEAAAEKLiAA9t
BzZC64ecgVQBGSQb5QtEIPC8-Vav4HsHLZDflLaugJLiAAZMCPn92yHv1Ip-iCiBb6i4ADq6
ZOv569KFQCvYSJfNgNLggAAQAAAAEAAAAMLkEABJsvEtUsVKh7tmYHhJ2FBm3kHU-OCdWiUY
VijgYa81MfkjQ1z6UiHbKP9_nRzIN9anprHqDGcR6q7O20q_yctZAHPjUCBi5AACv8L7YlRM
x10gPnszGO6rLFqZFmmRkhtV0TIWuWqYxd1jO0wxam7i5qdEa19u4sfpHFZ9RGI_WHxINkH8
FfvAwFLu0BMIIB6TCCAY8CAQEwCQYHKoZIzj0EATB7MQswCQYDVQQGEwJVUzELMAkGA1UECA
wCQ0ExCzAJBgNVBAcMAlBBMRAwDgYDVQQKDAdOTkwsSW5jMQ0wCwYDVQQLDAREQU4xMRMwEQ
YDVQQDDApOTkwsSW5jIENBMRwwGgYJKoZIhvcNAQkBFg1ubmxAZ21haWwuY29tMB4XDTE0MD
gyODIxMzU0MFoXDTE3MDUyNDIxMzU0MFowgYYxCzAJBgNVBAYTAlVTMQswCQYDVQQIDAJDQT
EWMBQGA1UEBwwNU2FuIEZyYW5jaXNjbzEQMA4GA1UECgwHTk5MLEluYzENMAsGA1UECwwERE
FOMTETMBEGA1UEAwwKTk5MLEluYyBDQTEcMBoGCSqGSIb3DQEJARYNbm5sQGdtYWlsLmNvbT
BZMBMGByqGSM49AgEGCCqGSM49AwEHA0IABCGBt3CIjnDowzSiF68C2aErYXnDUsWXOYxqIP
im0OWg9FFdUYCa6AgKjn1R99Ek2d803sGKROivnavmdVH-SnEwCQYHKoZIzj0EAQNJADBGAi
EAzAQujXnSS9AIAh6lGz6ydypLVTsTnBzqGJ4ypIqy_qUCIQCFsuOEGcRV-o4GHPBph_VMrG
3NpYh2GKPjsAim_cSNmQ",
        "assertionScheme": "UAFV1TLV"
      }
    ],
    "fcParams": "eyJhcHBJRCI6Imh0dHBzOi8vdWFmLXRlc3QtMS5ub2tub2t0ZXN0LmN
vbTo4NDQzL1NhbXBsZUFwcC91YWYvZmFjZXRzIiwiY2hhbGxlbmdlIjoiSDlpVzl5QTlhQVh
GX2xlbFFvaV9EaFVrNTE0QWQ4VHF2MHpDbkNxS0RwbyIsImNoYW5uZWxCaW5kaW5nIjp7fSw
iZmFjZXRJRCI6ImNvbS5ub2tub2suYW5kcm9pZC5zYW1wbGVhcHAifQ",
    "header": {
 "appID": "https://uaf-test-1.noknoktest.com:8443/SampleApp/uaf/facets",
      "op": "Reg",
      "serverData": "IjycjPZYiWMaQ1tKLrJROiXQHmYG0tSSYGjP5mgjsDaM17RQgq0
dl3NNDDTx9d-aSR_6hGgclrU2F2Yj-12S67v5VmQHj4eWVseLulHdpk2v_hHtKSvv_DFqL4n
2IiUY6XZWVbOnvg",
      "upv": {
        "major": 1,
        "minor": 1
      }
    }
}]
```

</code></pre>



## B. Authentication Operation

![FIDO_authentication_operation_diagram](D:\gitProject\FIDO\img\FIDO_authentication_operation_diagram.png)

* **등록 절차와의 차이점으로는 초기 사용자 인증 과정이 생략된다는 것이다.** 
* 초기에 사용자의 생체 인증을 등록하기 이전에는 사용자의 정보를 인증하는 과정을 통해서 생체 인증의 대상이 누구인지 서버에 저장되는데, 이미 서버에 사용자의 정보가 저장된 상태인 인증 과정에서는 생체 인증만으로 서버에 접근해 사용자를 인증 할 수 있기 때문이다.

![FIDO_authentication_operation_cryptographic_data_flow](D:\gitProject\FIDO\img\FIDO_authentication_operation_cryptographic_data_flow.png)

* 위 과정은 서버로부터 전달 받은 appID, Policy 그리고 challenge 값과 함께 Authenticator로 전달되고 되돌아 오는 과정에서 거치는 보안 과정에 대해서 나타낸 것이다.
* 서버에서는 확인을 위한 appID, 유효기간 검사를 위한 challenge와 policy 정보를 client에 넘겨준다. Client는 ASM으로 다시 정보를 넘겨주고 기기로 key handle data, access key, 그리고 기타 정보를 해싱한 fcp를 기기로 넘겨준다.
* 기기에서는 앞선 정보들과 사용자의 생체 인증 정보를 묶어서 ASM을 통해서 client로 넘겨주고 client에서 이 정보를 서버로 넘겨주면 서버에서 일치하는 정보가 있는지 FIDO 서버를 탐색하게 된다.

### 1. Transaction dictionary

<pre><code>

```
dictionary Transaction {
  required DOMString                  contentType;
  required DOMString                  content;
  DisplayPNGCharacteristicsDescriptor tcDisplayPNGCharacteristics;
};
```

</code></pre>

* 각종 요청에서 정보 전달을 위해서 사용되는 profile
* contentType은 content의 데이터 형태를 명시한다. 이 버전에서는 text/plain 혹은 image/png만 지원한다.
* content에는 Base64url로 구성된 사용자에게 전달할 정보를 저장하며 만약, contentType이 text/plain일 때 최대 200자 까지 전달 가능하다.
* tcDisplayPNGCharacteristics는 png 이미지를 전달 할 때 사용하고 이 인자를 사용 할 때에는 contentType은 image/png여야 한다.

### 2. Authentication Request Message

<pre><code>

```
dictionary AuthenticationRequest {
  required OperationHeader header;
  required ServerChallenge challenge;
  Transaction[]            transaction;
  required Policy          policy;
};
```

</code><pre>

* 인증을 요청할 때 보내는 메세지의 형태로, 인증 과정이기 때문에 header의 options 부분은 "Auth"로 지정되어야 한다.
* Challenge는 사용자 요청의 유효기간을 파악하기 위해서 랜덤하게 부여된다.
* 이 때     Transation은 위에 Trantion dictionary를 보면 알 수 있다.
* 안에 코드 구성은 등록때의 요청 코드의 형태와 유사하다.

#### 예시코드

<pre><code>

```
[{
    "header": {
      "upv": {
        "major": 1,
        "minor": 1
      },
      "op": "Auth",
 "appID": "https://uaf-test-1.noknoktest.com:8443/SampleApp/uaf/facets",
      "serverData": "5s7n8-7_LDAtRIKKYqbAtTTOezVKCjl2mPorYzbpxRrZ-_3wWro
MXsF_pLYjNVm_l7bplAx4bkEwK6ibil9EHGfdfKOQ1q0tyEkNJFOgqdjVmLioroxgThlj8Is
tpt7q"
    },
    "challenge": "HQ1VkTUQC1NJDOo6OOWdxewrb9i5WthjfKIehFxpeuU",
    "policy": {
      "accepted": [
        [
          {
            "userVerification": 512,
            "keyProtection": 1,
            "tcDisplay": 1,
            "authenticationAlgorithms": [
              1
            ],
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          }
        ],
        [
          {
            "userVerification": 4,
            "keyProtection": 1,
            "tcDisplay": 1,
            "authenticationAlgorithms": [
              1
            ],
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          }
        ],
        [
          {
            "userVerification": 4,
            "keyProtection": 1,
            "tcDisplay": 1,
            "authenticationAlgorithms": [
              2
            ]
          }
        ],
        [
          {
            "userVerification": 2,
            "keyProtection": 4,
            "tcDisplay": 1,
            "authenticationAlgorithms": [
              2
            ]
          }
        ],
        [
          {
            "userVerification": 4,
            "keyProtection": 2,
            "tcDisplay": 1,
            "authenticationAlgorithms": [
              1,
              3
            ]
          }
        ],
        [
          {
            "userVerification": 2,
            "keyProtection": 2,
            "authenticationAlgorithms": [
              2
            ]
          }
        ],
        [
          {
            "userVerification": 32,
            "keyProtection": 2,
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          },
          {
            "userVerification": 2,
            "authenticationAlgorithms": [
              1,
              3
            ],
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          },
          {
            "userVerification": 2,
            "authenticationAlgorithms": [
              1,
              3
            ],
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          },
          {
            "userVerification": 4,
            "keyProtection": 1,
            "authenticationAlgorithms": [
              1,
              3
            ],
            "assertionSchemes": [
              "UAFV1TLV"
            ]
          }
        ]
      ],
      "disallowed": [
        {
          "userVerification": 512,
          "keyProtection": 16,
          "assertionSchemes": [
            "UAFV1TLV"
          ]
        },
        {
          "userVerification": 256,
          "keyProtection": 16
        }
      ]
    }
}]
```

</code></pre>

### 3. AuthenticatorSignAssertion dictionary

<pre><code>

```
dictionary AuthenticatorSignAssertion {
  required DOMString assertionScheme;
  required DOMString assertion;
  Extension[]        exts;
};
```

</code></pre>

* 특정한 authenticator에 의해 생성된 응답으로 인증에 사용 가능한 기기들이 보내는 반응으로 보인다.
* assertionScheme은 표준의 버전을 나타내는 것으로 보인다. ex) "assertionScheme": "UAFV1TLV"
* assertion은 개인 정보로 생성되는 서명으로 base64url로 인코딩 되어있다.

## 4. AuthenticationResponse dictionary 

<pre><code>

```
dictionary AuthenticationResponse {
  required OperationHeader              header;
  required DOMString                    fcParams;
  required AuthenticatorSignAssertion[] assertions;
};
```

</code></pre>

* 등록 과정과 마찬가지로 client의 정보와 Authenticator를 통해서 얻은 인증 담고 있다.
* RegistrationResponse dictionary와 동일한 프로토콜을 가지고 있다.

#### 예시 코드

<pre><code>

```
[{
    "assertions": [
      {
        "assertion": "Aj7WAAQ-jgALLgkAQUJDRCNBQkNEDi4FAAABAQEADy4gAHwyJA
EX8t1b2wOxbaKOC5ZL7ACqbLo_TtiQfK3DzDsHCi4gAFwCUz-dOuafXKXJLbkUrIzjAU6oDb
P8B9iLQRmCf58fEC4AAAkuIABkwI-f3bIe_Uin6IKIFvqLgAOrpk6_nr0oVAK9hIl82A0uBA
ACAAAABi5AADwDOcBvPslX2bRNy4SvFhAwhEAoBSGUitgMUNChgUSMxss3K3ukekq1paG7Fv
1v5mBmDCZVPt2NCTnjUxrjTp4",
        "assertionScheme": "UAFV1TLV"
      }
    ],
    "fcParams": "eyJhcHBJRCI6Imh0dHBzOi8vdWFmLXRlc3QtMS5ub2tub2t0ZXN0LmN
vbTo4NDQzL1NhbXBsZUFwcC91YWYvZmFjZXRzIiwiY2hhbGxlbmdlIjoiSFExVmtUVVFDMU5
KRE9vNk9PV2R4ZXdyYjlpNVd0aGpmS0llaEZ4cGV1VSIsImNoYW5uZWxCaW5kaW5nIjp7fSw
iZmFjZXRJRCI6ImNvbS5ub2tub2suYW5kcm9pZC5zYW1wbGVhcHAifQ",
    "header": {
 "appID": "https://uaf-test-1.noknoktest.com:8443/SampleApp/uaf/facets",
      "op": "Auth",
      "serverData": "5s7n8-7_LDAtRIKKYqbAtTTOezVKCjl2mPorYzbpxRrZ-_3wWro
MXsF_pLYjNVm_l7bplAx4bkEwK6ibil9EHGfdfKOQ1q0tyEkNJFOgqdjVmLioroxgThlj8Is
tpt7q",
      "upv": {
        "major": 1,
        "minor": 1
      }
    }
}]
```

</code></pre>



## C. Deregistration Operation

### 1. Deregistration Request Message

<pre><code>

```
[{
    "header": {
      "op": "Dereg",
      "upv": {
        "major": 1,
        "minor": 1
      },
  "appID": "https://uaf-test-1.noknoktest.com:8443/SampleApp/uaf/facets"
    },
    "authenticators": [
      {
        "aaid": "ABCD#ABCD",
        "keyID": ""
      }
    ]
}]
```

</code></pre>

* 여기서 header의 option 값으로 "Dereg"를 명시해 삭제를 위한 요청임을 명시하며 다른 요청들과 달리 별도의 Policy 와 Challenge가 없다.

### 2. DeregisterAuthenticator dictionary

<pre><code>

```
dictionary DeregisterAuthenticator {
  required AAID  aaid;
  required KeyID keyID;
};
```

</code></pre>

* Authenticator Attestation ID (AAID) typedef

* - typedef DOMString AAID;
  - AAID =      4(HEXDIG) "#" 4(HEXDIG) ex)[03EF#03ef]
  - V#M으로 구성된 4자리 16진수 두개로 구성되어 있는 Profile로 V는 제조사 코드, M은 모델명으로 구성되어 있다.
  - 이 AAID      profile로 authenticator의 제조사와 모델명을 알 수 있다. 따라서 각각의 기기는 유일한 AAID profile을 가지고 있다.

* KeyID typedef

* - typedef DOMString KeyID;
  - Base64url로 파싱된 유일한 ID로 AAID와 KeyID는 함께 사용자 등록 과정에서 서버에 저장된다.
  - 인증과 삭제 단계에서 서버는 KeyID를 사용자      Authenticator에게 줘서 필요한 동작을 처리 하도록 해야한다.

### 3. DeregistrationRequest dictionary

<pre><code>

```
dictionary DeregistrationRequest {
  required OperationHeader           header;
  required DeregisterAuthenticator[] authenticators;
};
```

</code></pre> 

* 위 DeregisterAuthenticator 프로토콜과 OperationHeader를 가지는 삭제 요청의 Profile은 위와 같다.
  * 사용자가 로컬에서 삭제 하는지 여부는 상관하지 않고 서버에서 삭제하고 끝나기 때문에 별도의 challenge는 필요 없어 보인다.


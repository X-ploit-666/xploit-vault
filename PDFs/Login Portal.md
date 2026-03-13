### Login Steps

```mermaid
flowchart LR

User[User Browser]

FE[Frontend Web App]
API[Auth API]

OTP[(OTP Service)]
DB[(User Account DB)]

User -->|Enter loginId| FE
FE -->|PUT /public/auth/email| API

API -->|Generate OTP| OTP
OTP -->|Send OTP email/SMS| User

API -->|Return verificationId| FE

User -->|Enter otpCode| FE
FE -->|PUT otpCode + verificationId| API

API -->|Validate OTP| OTP
API -->|Fetch account| DB

API -->|Auth success / session| FE
FE -->|Login success| User
```

----
---


```mermaid
sequenceDiagram
participant U as User Browser
participant F as Frontend (id.biltrewards.com)
participant A as Auth API
participant O as OTP Service (Email/SMS)

U->>F: Enter email / phone
F->>A: PUT /public/auth/email\n{loginId}

A->>O: Generate OTP
O-->>U: Send OTP (email/SMS)

A-->>F: Response\n{verificationId, codeLength:6}

U->>F: Enter OTP code
F->>A: PUT /public/auth/email\n{loginId, otpCode, verificationId}

A->>A: Validate OTP + verificationId

alt OTP valid
A-->>F: Auth success\n(auth code / session)
F-->>U: Logged in
else OTP invalid
A-->>F: Error response
F-->>U: Show invalid code
end
```


>Conceptually the backend is doing something like:

```
if (otpCode == storedOTP && verificationId matches session)  
    allow login  
else  
    reject
```
>The **critical binding in this flow** is:

verificationId  → identifies the OTP challenge  
otpCode         → the secret user must provide  
loginId         → account being authenticated

All three must match for authentication to succeed. If any validation between those three objects is weak, authentication logic can break in subtle ways.


---


---
### Data Flows
```text
User → Frontend : loginId
Frontend → Auth API : /public/auth/email
Auth API → OTP Service : generate OTP
OTP Service → User : OTP message
Auth API → Frontend : verificationId
User → Frontend : otpCode
Frontend → Auth API : otpCode + verificationId
Auth API → DB : account lookup
Auth API → Frontend : session/auth code
```
# Single Sign-On (SSO) Endpoint Documentation Empire One

## Overview

This document explains how to integrate with the SSO endpoint provided by Sansoni Management. It covers the authentication flow, endpoint details.

---

## Table of Contents

- [Single Sign-On (SSO) Endpoint Documentation Empire One](#single-sign-on-sso-endpoint-documentation-empire-one)
  - [Overview](#overview)
  - [Table of Contents](#table-of-contents)
  - [1. Introduction](#1-introduction)
  - [2. SSO Flow Overview](#2-sso-flow-overview)
  - [3. Endpoint Details](#3-endpoint-details)
    - [Authorization Endpoint](#authorization-endpoint)
    - [Redirection Uri](#redirection-uri)
    - [User Info Endpoint](#user-info-endpoint)
  - [4. Authentication and Authorization for references](#4-authentication-and-authorization-for-references)
  - [7. Error Handling](#7-error-handling)
  - [8. Security Considerations](#8-security-considerations)
  - [9. Provide a redirect_uri if customization is Required](#9-provide-a-redirect_uri-if-customization-is-required)

---

## 1. Introduction

The SSO endpoint enables third-party applications to authenticate users using Empire One's centralized authentication system. The system is based on AWS Cognito and Managed UI.

---

## 2. SSO Flow Overview

1. **User Initiates Login**: User clicks on the 'Login' button on the Traveller site.
2. **Redirect to Authorization Endpoint**: The user is redirected to the SSO authorization endpoint.
3. **User Authenticates**: User provides credentials (if not already authenticated).
4. **Authorization Code**: User will be redirectd with an authorization code.
5. **Exchange Code for Token**: The authorization code is exchanged for an access token.
6. **Access User Information**: The third-party application uses the access token to request user details from the given endpoint.
7. **User is Authenticated**: The user is logged.

---

## 3. Endpoint Details

### Authorization Endpoint

```
> Current

https://uat.empireone.com.au/api/cognito/generate
https://uat.empireone.com.au/api/generate/signup
https://uat.empireone.com.au/api/cognito/generate/forget-password


> After go prod

https://auth.empireone.com.au/api/cognito/generate

> Request params
  - GET
  - Content-type:application/json
  - Params:
      -- redirect_uri: https://empiretraveller.com.au/cognito/verification-successful/

> Successful request will redirect user to Cognito Managed Login UI.

```

### Redirection Uri

```
https://www.empiretraveller.com.au/cognito/verification-successful/?code=a2683ab8-d383-4263-bb93-f8cf25647f60

queryString "code" will be used for user Info access.

```

### User Info Endpoint

```
> Current

https://uat.empireone.com.au/auth/authenticate/

> After go prod

https://auth.empireone.com.au/auth/authenticate/


> Request params
  - POST
  - Content-type:application/json
  - Params:
      -- code:         a2683ab8-d383-4263-bb93-f8cf25647f60 --- this is a demo code
      -- redirect_uri: https://empiretraveller.com.au/cognito/verification-successful/

> Successful request:
 "status": "success",
  "payload": {
      "access_token": "session_eyJraWQiOiI3bVVRaEFydTZRNjdNZWgrN3VFSFlkR0FlSjhqTTJGa1RGMmZ1SmVRc1dFPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiJhOTllZjQ4OC1kMGIxLTcwYjMtOTBjNS05OGVhNDNkYzlhZjYiLCJjb2duaXRvOmdyb3VwcyI6WyJhcC1zb3V0aGVhc3QtMl9pbmJmdjFhSzlfR29vZ2xlIl0sImlzcyI6Imh0dHBzOlwvXC9jb2duaXRvLWlkcC5hcC1zb3V0aGVhc3QtMi5hbWF6b25hd3MuY29tXC9hcC1zb3V0aGVhc3QtMl9pbmJmdjFhSzkiLCJ2ZXJzaW9uIjoyLCJjbGllbnRfaWQiOiIyNmNvbWM2cGFybmE3NGZuYWllM2x1Mms2ZiIsIm9yaWdpbl9qdGkiOiIyODRiM2M4OC1kN2I2LTQ0ODItYjliZC01M2JjNjljMzViNDEiLCJ0b2tlbl91c2UiOiJhY2Nlc3MiLCJzY29wZSI6Im9wZW5pZCBwcm9maWxlIGVtYWlsIiwiYXV0aF90aW1lIjoxNzQwMDk2OTI0LCJleHAiOjE3NDAxMDA1MjQsImlhdCI6MTc0MDA5NjkyNCwianRpIjoiOWNiODVjOGEtMmQxNy00NzhmLThmZTMtNGQ3MWMyOTVhMTJmIiwidXNlcm5hbWUiOiJnb29nbGVfMTA5MzAxNDQ0NzI2MDk1MDAwMTI4In0.3axd_ePVfO2qCEEW-3lcdoO-i5eY3yT9DGHtch4CDpQBvCQLV_94N3eKpsz0rcfouedc0qf17nfXKQAYBr1i3ALpbYLUIj0dGdzgLmPLCrE36yQD0l5W25LPj83pz2U8AhKAdImGyURz9sXe_ZHyjYGRrofBd_Qr0tf72xzKymAgdOZHWmKoUahRnZHsNY13_E4H3YxFExuZmP1YuhksP7OvIp5NDMhrBl213c5HEFzW0O_bZX2Sbzk-zNt7zbBjLCWpYIrsCvGA5Av40teMtb4qbAD55EXCWRIe3oXlLn0trzDDEnz_2yW14yLsmLd4R_i4TJp6SCgQB4ABnKlO6w",
      "refresh_token": "eyJjdHkiOiJKV1QiLCJlbmMiOiJBMjU2R0NNIiwiYWxnIjoiUlNBLU9BRVAifQ.FdpxZVDyQQTvqj7QeTluXe4BuymlhC4V51DQLC6x3LUFSeTLENAhzp_45-PWEj7TPZTADFV8bOIXQNXQEqe0xz4R_2-5inrpD6r55i-zqMLQ_gyM8YkXTw6cK-H05EuXUkzPcdtCZaki0hkcsfC4NrSj8hFGo7lDiMds1bpvFQH0hS6rCRYTbHZhPPAV0hic63_mh5vZwFq9OCffVO12wuiAebGLy0VDz7lkF-A6gXAB5eUn3M6NGtzsDfl27QEoRSu0RAMsmMqIra4AL78JoXdv4nP6wRiUA1odgzwkdoQfgYBtdq8aa-XbjVgLFvFELZJ0XXQEr1m85TcihJuPuQ.KCSPLir2FgeIqwMv.9tZak2zQ5Vxm1xsUt3fkAoBHPdY8ZorVe9odOG4FiD260hn7Bte65FWX-ycwS27v7xlCS9EqaPUIuzkkzKOonz4ZdTFUQK5rcehtdvuhYu4xoiJISiIHbtakfc8aNUfiwAJu-utGNzRPMb_7b5xCvYQC1_rj5MVjAZP-BlAwNmYTiZajQs-PDiYF2kYNfqFUWvBGbiaJg3Sw6VsHT3ZLcWHmlThwLu1fXHsp4Bx-b3aozc7ZL8CJISm0C-rz12VX9wnjcSPB89bQ0teRt8un1R0gy1nFq49yNYwvSbD5dXDdA7iCdeaO2Pim-5yTnIBRosqqdZWcctbb8cW_rfwHdJE2-4VOh1sSFpFdGZlCtnDs8tT0xnZ91fMrLJq_O1p5EfckeGtSDD4zHHcMDaJcHOwrC5P8lf92xdueGQKigDn1LO1dORZN6YfQRYdwAqCa1IpSCQKjlg_dQjGYoCbFBFMT1kYJYLVzUMpV9mWXfVsB0ceuGTro0UVZVGWBjfkY2fE7GYW_mYCKHigxtJQQNS3ajhhz6X1sijZPQHYCQHCgRSe6eiqqtAxBV8JUHZaiH1bKBj3knlxkTMABmMTPR8OWtehDaSBgsixnIEycd_vZTvwl9vZMg__I0TstM5dWefgi5M4-hVotOydP0hOnuN0m6b-i7FqaCDnuMrQnxRSkwjbcD8xIU5KHLYK1mYDywXPji8YOoQCFczCn_bM4YJZZxMom80277LjMnqtaz3kRFApWiFA3sAkf4aERHncV1ZVNbImVRR-JCFE6f8J4SUPIbF9-v2_D4IaGngVtnG7uOi9rlkKYMM-xNMSp4AkKFdGRaumDn69Deg7wt85U-dEQOLb8GqthkyHzcC1kx7sTQjkccymaBoM1B2PXE-L7DLylpd-5duJBM27ucAgeyEmMJh7b_xI3F10VzRhRFk_NhDkerRa8cOSTDFdorqbaIFrCHO-A7ZiJUcx4eYptABU944A-6VJ5jsVokjUt7Y-Rilg0QqNAdaemYZO4J_TCO_XLcQjd18uPffN9LlWATsvudGpBEhaSmt0gdmy9cmNixv8zd0YcOwsDIQbaVWo_aR-VaugFs8sIJ09oq4UuAB1zwAe99TfvNpEg45PkWFCbtO2dmMv4OKVVp_C_9YyeNBp3CHozbt9Q7xkZudHsNBKmECaJVB61DJkFkGvI9wOi8z2K3Lt7SZ_aBXHRqEl_lauwVICsWHhFFga_5mP6PwOcW7HVY-VpM_999XxZOQ.Ah8tx8Zs8cJ5zKvwYPUGcg",
      "email": "test@gmail.com",
      "user": {
          "id": 12,
          "created_at": "2025-02-18T16:25:07.266Z",
          "updated_at": "2025-02-18T16:25:07.266Z",
          "deleted_at": null,
          "mobile": null,
          "email": "test@gmail.com",
          "email_verified": false,
          "first_name": null,
          "last_name": null,
          "nick_name": null,
          "title": null,
          "postcode": null,
          "address": null,
          "company_name": null,
          "website": null,
          "abn": null,
          "abn_verified": false,
          "metadata": null,
          "user_l_token": 1,
          "preferences": [],
          "tags": [],
          "is_suspended": false
        }
    },
    "count": null,
    "message": "User => test@gmail.com authenticated",
    "tracking_id": "142defb7-8b40-466a-bc17-5c3774641c41",
    "route": "/api/auth/authenticate",
    "timestamp": "2025-02-21T00:15:24.536Z"
```

---

## 4. Authentication and Authorization for references

- **Platform**: AWS Cognito
- **Protocol**: OAuth 2.0
- **Grant Type**: Authorization Code
- **Scopes**:
  - `openid` - Required for OIDC compliance.
  - `profile` - Access to basic user profile information.
  - `email` - Access to user's email address.

---

## 7. Error Handling

Common error responses include:

- `invalid_redirect domain`: Missing or invalid redirect_uri.
- `Invalid user credentials, authentication failed`: Client is not authorized to use this endpoint / Authentication failed.
- `access_denied`: User denied the authorization request.
- `server_error`: Unexpected error on the authorization server.

## 8. Security Considerations

- Always use HTTPS to prevent man-in-the-middle attacks.
- Implement rate limiting to prevent abuse

## 9. Provide a redirect_uri if customization is Required

## 목차

1. [OAuth 2란?](#1-oauth-2)
2. [OAuth roles](#2-oauth-roles)
3. [인증 컴포넌트 상호작용](#3-인증-컴포넌트-상호작용)
4. [Authorization Grant](#4-authorization-grant)
<br>

### 1. OAuth 2

- OAuth 2를 통한 인증은 사용자의 credential을 백엔드 애플리케이션이 직접 관리하지 않는다.
- 사용자가 특정 서비스를 이용하기 위해 신뢰할 만한 벤더에게 인증처리를 위임하는 방식이다.
<br>

### 2. OAuth roles

1. Resource Owner: 애플리케이션에서 사용하고자 하는 리소스의 소유자이다.
2. Client: Resource Owner를 대신해 보호된 리소스에 접근하는 애플리케이션이다.
3. Resource Server: Client의 리소스에 대한 액세스 요청을 수락하고 보호된 리소스를 제공한다.
4. Authorization Server: Resource Owner에 대한 인증처리를 진행하여 Access Token을 Client에게 제공한다.
<br>

### 3. 인증 컴포넌트 상호작용

![oath2](https://user-images.githubusercontent.com/54367532/204139124-4eb78269-ca05-4242-aab0-a540d5a5843e.png)


1. Resource Owner가 Client에게 OAuth 2 인증을 요청한다.
2. Client는 Resource Owner가 인증을 진행할 수 있도록 로그인 페이지로 redirect 한다.
3. Resource Owner가 로그인 인증 정보를 입력해 인증을 진행한다.
4. Authorization Server는 Resource Owner의 로그인 인증이 성공했음을 증명하는 Access Token을 Client에게 전송한다.
5. Client는 Resource Server 에게 Resource Owner의 리소스를 요청한다.
6. Resource Server는 Client가 전송한 Access Token을 검증하여 성공 시 Resource Owner의 리소스를 Cient에게 전송한다.

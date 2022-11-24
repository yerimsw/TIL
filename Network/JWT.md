## 목차

1. [Session vs Token](#1-session-vs-token)
2. [JWT란](#2-jwt란)
3. [JWT 인증 절차](#3-jwt-인증-절차)
4. [JWT 생성 검증](#4-jwt-생성과-검증)
<br>

### 1. Session vs Token

- HTTP 프로토콜은 request를 전송, response를 수신 후 연결을 끊는 비연결성 특징을 갖는다.
- 서버측에서 request가 인증된 사용자로부터 온 것인지 알 수 있는 방법을 알아보자.
<br>

1. Session

   - 사용자 인증 정보를 서버의 세션 저장소에서 관리한다.
   - 클라이언트가 리소스를 요청하면 서버는 세션 저장소에 저장된 세션과 클라이언트로 부터 받은 정보를 비교한다.
   - 사용자의 세션 ID는 클라이언트의 쿠키에 저장되고, request를 전송할 때 인증 수단으로 사용된다.

2. Token

   - 사용자 인증 정보를 서버에서 관리하지 않는다.
   - 클라이언트 측에서 토큰을 헤더에 포함시켜 request를 전송하면 서버가 검증을 진행한다.
   - JWT 발급 이후 사용자의 인증 상태를 서버에서 유지하지 않아도 된다.
<br>

### 2. JWT란

1. Access Token
   - 보호된 정보에 대한 접근 권한을 부여할 때 사용한다.
   - 권한이 탈취되어 악용되는 것을 방지하기 위해 짧은 유효기간을 둔다.
<br>

2. Refresh Token
   - 유효기간이 만료된 Access Token을 다시 발급 받는다.
   - DB에 저장되며 긴 만료시간을 갖는다.
<br>

3. JWT 구조
   - Header : 토큰의 종류(typ)와 알고리즘(alg)을 정의한다.
   - Payload : 토큰 정보를 나타내는 클레임(claim)으로 구성되어 있다. 사용자 정보 등을 담을 수 있다.
   - Signature : 비밀키와 헤더에서 지정한 알고리즘을 사용해 Base64로 인코딩 된 헤더, 페이로드를 단방향 해싱 한다. 암호화된 메시지는 토큰 검증에 사용된다.
<br>

### 3. JWT 인증 절차

1. 클라이언트가 서버에 username과 password 정보를 담아 로그인 요청을 전송한다.
2. 일치한다면 서버는 클라이언트에게 보낼 암호화 된 토큰을 생성한다. (Access Token과 Refresh Token 모두 생성)
3. 클라이언트는 전송 받은 토큰을 저장한다. (Session Storage, Cookie 등에 저장)
4. 클라이언트가 HTTP 헤더 또는 쿠키에 토큰을 담아 request를 전송한다
5. 서버는 토큰 검증을 통해 유효한 토큰이라면, 클라이언트의 요청을 처리하여 응답을 전송한다.
<br>

### 4. JWT 생성과 검증

1. JWT 의존 라이브러리 추가

```
implementation 'io.jsonwebtoken:jjwt-api:0.11.5'
runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.11.5'
runtimeOnly 'io.jsonwebtoken:jjwt-jackson:0.11.5'
```
- jjwt 라이브러리 의존을 build.gradle 파일에 추가한다.
<br>

2. JWT 생성 구현

``` java
public String encodeBase64SecretKey(String secretKey) {
   return Encoders.BASE64.encode(secretKey.getBytes(StandardCharsets.UTF_8));
}
```
- Secret Key를 BASE64 형식의 문자열로 인코딩 하는 메서드이다.
- String 클래스의 getBytes() 메서드를 활용한다.
<br>

``` java
private Key getKey(String secretKey) {
   byte[] keyBytes = secretKey.getBytes(StandardCharsets.UTF_8);
   return Keys.hmacShaKeyFor(keyBytes);
}

private Key getKeyFromBase64EncodedKey(String base64EncodeSecretKey) {
   byte[] keyBytes = Decoders.BASE64.decode(base64EncodedSecretKey);
   Key key = Keys.hmacShaKeyFor(keyBytes);
   return key;
}
```
- 위는 인코딩 되지 않은 secret key에 대해 byte array를 반환한다.
- 아래는 BASE64로 인코딩 된 secret key를 디코딩 한 후 byte array를 반환한다.
- `Keys.hmacShaKeyFor()` 메서드는 파라미터로 주어진 key byte array를 기반으로 HMAC-SHA 알고리즘을 사용해 새로운 SecretKey 인스턴스를 생성한다.
<br>

``` java
public String generateAccessToken(Map<String, Object> claims, String subject, Date expiration, String base64EncodedSecretKey) {
   Key key = getKeyFromBase64EncodeKey(base64EncodedSecretKey);
   
   return Jwts.builder()
            .setClaims(claims) // name/value 쌍으로 이뤄진 맵 데이터를 포함하는 Claims 인스턴스를 리턴.
            .setSubject(subject)
            .setIssueAt(Calendar.getInstance().getTime())
            .setExpiration(expiration)
            .signWith(key) // 생성된 JWT를 key를 이용해 서명한다.
            .compact(); // JWT를 생성하고 url-safe String으로 직렬화한다.
```
- https://javadoc.io/static/io.jsonwebtoken/jjwt/0.9.1/io/jsonwebtoken/JwtBuilder.html
- 인증된 사용자에게 JWT 발행해주는 JWT 생성 메서드이다.
- Url-safe JWT String을 반환한다.
<br>

```java
public String generateRefreshToken(String subject, Date expiration, String base64EncodedSecretKey) {
   Key key = getKeyFromBase64EncodedKey(base64EncodedSecretKey);
   
   return Jwts.builder()
            .setSubject(subject)
            .setIssuedAt(Calendar.getInstance().getTime())
            .setExpiration(expiration)
            .signWith(key)
            .compact();
}
```
- Access Token이 만료되었을 경우, 다시 생성할 수 있게 하는 Refresh Token을 생성하는 메서드다. 
<br>

3. JWT 검증 구현

``` java
public void verifySignature(String jws, String base64EncodedSecretKey) {
    Key key = getKeyFromBase64EncodedKey(base64EncodedSecretKey);

    Jwts.parserBuilder()
        .setSigningKey(key)     // (1)
        .build()
        .parseClaimsJws(jws);   // (2)
        // .getBody() // (3)
}
```
- (1) JWT 서명을 검증할 signing key를 지정한다.
- (2) compact 하게 직렬화된 JWS String 토큰을 JWS 인스턴스로 파싱한다.
- (3) `getBody()` 메서드를 이용해 토큰에 저장된 claims를 얻을 수 있다.

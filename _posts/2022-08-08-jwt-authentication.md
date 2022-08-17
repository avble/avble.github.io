# introduction

The form of encoded string is as below.
``` encode
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ==.VqX1JWqXr8X8lhosCRs-RtACrmUFPV7BySakP9jbccY
```

The encoded string includes three parts which are header, payload, and signature.
A typical use is that the header, payload along with the secret are used to create a new signature.
That will compare with signature in the encoded string.

## Header
``` python
import base64
base64.b64decode('eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9')
# ==> output: b'{"alg":"HS256","typ":"JWT"}'
```

``` json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

## Payload
``` python
import base64
base64.b64decode('eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ==')
# decoded stream: {"sub":"1234567890","name":"John Doe","iat":1516239022}
```

``` json
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
```

## Signature
Signature is signed by either private or public/private key. 


# The use of JWT 
The general flow of using JWT is as below. 

![Authentication](https://cdn2.auth0.com/docs/media/articles/api-auth/client-credentials-grant.png)

1) The application requests authorizations to the authorization server.

2) Once the authorization is granted, it responses an access token.

3) VERIFICATION of the token to check if the application can use the access token to access the protected resource (API).


The VERIFICATION process is very specific application. Continue the above example with assumption that the signature is signed by a private key `12345678`
The verification is processed by computing the signature based on the header, payload, and private key (on server). Then this signature is compared with signature which is along with the access token.

``` python
def jwt_verify(access_token: str, private_key: str):
    (jwt_header, jwt_payload, jwt_signature) = access_token.split(".")

    jwt_header_b64 = bytes(jwt_header.encode())
    jwt_payload_b64 = bytes(jwt_payload.encode())

    message = jwt_header_b64 + b'.' + jwt_payload_b64

    # create the signature
    signature = hmac.new(private_key, message, hashlib.sha256).digest()
    signature = hmac.digest(private_key, message, hashlib.sha256)
    signature = base64.urlsafe_b64encode(signature).rstrip(b'=')

    # Verify the signature
    return signature == bytes(jwt_signature.encode())
  
```

Usage
======
``` python
signature = jwt_verify('eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ==.VqX1JWqXr8X8lhosCRs-RtACrmUFPV7BySakP9jbccY', pri_key)
print (signature)
### Return True
```
# Cloudfront signed cookies and signed url

### 1. generate RSA key:
```
openssl genrsa -out cloudfront_private_key.pem 2048


openssl rsa -pubout -in cloudfront_private_key.pem -out cloudfront_public_key.pem
```

### 2. upload Public RSA key to `Cloudfront`:
```
```

### 3. generate Unix time for `Cloudfront signed cookie`
```
date -d "2024-12-31 00:00:00" +%s
```


### 4. signing data and create signature
```
cat policy.json | openssl dgst -sha1 -sign private_key.pem | openssl base64 -A > signature.txt
```


## 5. get file
```
POLICY=$(cat policy.json | base64 | tr -d '\n')
SIGNATURE=$(cat signature.txt)
KEY_PAIR_ID=xxxx

curl -v -H "Cookie: CloudFront-Policy=$POLICY; CloudFront-Signature=$SIGNATURE; CloudFront-Key-Pair-Id=$KEY_PAIR_ID" "https://cloudfront-url.net/path/2023_10_17_13_32_pkpadmin_-1091-5090-1-CE.doc" --output testing1111.doc
```
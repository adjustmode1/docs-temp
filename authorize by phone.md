### Path: 
  ```
  POST /v2/iam/authorize
  ```
### Header Request:
|Field|Data type| require|Description|
|----|----|----|----|
|device-id|String| True |id của thiết bị|
### Body Request:
|Field|Data type| require|Description|
|----|----|----|----|
|phone|String|True|số điện thoại|
|token|String|False|mã token để thông qua google|
### 1. Success:
```javascript
  { 
    ok: true,
    message: 'Please verify your device.' 
  }
```

### Example:
    curl -X 'POST' \
    'https://api-sb.halome.dev/v2/iam/authorize' \
    -H 'accept: */*' \
    -H 'Content-Type: application/json' \
    -H 'device-id: 111 ' \
    -d '{phone: 84977585797, token:"string"}'

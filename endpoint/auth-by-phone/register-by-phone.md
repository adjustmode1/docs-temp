# Đăng ký
### Path: 
```
POST /v2/iam/register
```
### Header Request:
|Field|Data type| require|Description|
|----|----|----|----|
|device-id|String| True |id của thiết bị|
### Body Request:
|Field|Data type| require|Description|
|----|----|----|----|
|phone|String|số điện thoại đăng ký|True|
|displayName|String|tên người dùng|True|
|code|String|mã otp được gửi về thông qua điện thoại|True|

### 1. Success:
```javascript
  { 
    token: String(mã xác thực cho các lần request),
    refreshToken: String(mã yêu cầu reset token) ,
    isFirstLogin: Boolean(có phải lần đầu đăng nhập),
  }
```

### Example:
    curl -X 'POST' \
    'https://api-sb.halome.dev/v2/iam/register' \
    -H 'accept: */*' \
    -H 'Content-Type: application/json' \
    -H 'device-id: 111 ' \
    -d '{phone: 84977585797,displayName: "nameTemp" code:"000000"}'
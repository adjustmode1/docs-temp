# Đăng ký bằng điện thoại

### Path:

```
POST /v2/iam/register
```

### Header Request:

---

| Field     | Data type | require | Description     |
| --------- | --------- | ------- | --------------- |
| device-id | String    | True    | id của thiết bị |

### Body Request:

---

| Field       | Data type | require | Description                             |
| ----------- | --------- | ------- | --------------------------------------- |
| phone       | String    | True    | số điện thoại của người dùng            |
| displayName | String    | True    | tên người dùng                          |
| code        | String    | True    | mã otp được gửi về thông qua điện thoại |

### Flow:

1. kiểm tra body request:

   - phone:

     ```js
     if not (bắt đầu bằng ['+84'|'+840'|'0'|'84'|'840'] theo sau là 9 ký tự số) {
       lỗi(
          message: 'Only VietNam phone numbers are supported.'
       )
     }

     if not (chuỗi số) {
        lỗi(
          message: 'phone must be a number string'
        )
     }
     ```

   - code:

     ```js
      if not (chuỗi số) {
        lỗi (
          message: 'code must be a number string'
        )
      }

      if code.length < 6 {
        lỗi(
          message: 'code must be longer than or equal to 6 characters'
        )
      }
     ```

2. Main flow:
   1. kiểm tra device-id:
      ```javascript
        if không tồn tại device-id {
          trả về lỗi (
              {
                ok: false,
                message: 'deviceId cannot be null.',
                code: 'deviceId404',
              }
          )
        }
      ```
   2. kiểm tra phiên đăng nhập (session):
      ```js
      if không tồn tại session { // chưa có kích hoạt phiên hoạt động
        trả về lỗi(
          message: 'Invalid session.'
        )
      }else{
        tạo ra tài khoản đăng nhập nặc danh
      }
      ```
   3. trả token về thiết bị:
      ```js
        trả về {
          token: String, //mã token cho các lần reuqest sau
          refreshToken: String, // mã yêu cầu reset token
          isFirstLogin: Boolean, //có phải đăng nhập lần đầu hay không
        }
      ```

### Example:

```sh
curl -X 'POST' 'https://api-sb.halome.dev/v2/iam/register' \
-H 'accept: */*' \
-H 'Content-Type: application/json' \
-H 'device-id: 111 ' \
-d '{phone: 84977585797,displayName: "nameTemp" code:"000000"}'
```

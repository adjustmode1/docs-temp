# xác thực bằng email

### Path:

```
POST /v2/iam/verify
```

### Header Request:

| Field     | Data type | require | Description     |
| --------- | --------- | ------- | --------------- |
| device-id | String    | True    | id của thiết bị |

### Body Request:

| Field | Data type | require                                 | Description |
| ----- | --------- | --------------------------------------- | ----------- |
| email | String    | email dùng để xác thực                  | True        |
| code  | String    | mã otp được gửi về thông qua điện thoại | True        |

### Flow:

1. kiểm tra body request:

   - email:

     ```js
     if not (tồn tại '@' và '.' && sau '.' là 2 tới 4 ký tự) {

       lỗi(
         {
          message: 'Invalid email format. Please try again!'
         }
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

   2. kiểm tra tài khoản:

      ```js
        if nếu tồn tại tài khoản{

          if có phiên hoạt động {

            trả về (
              token: String // chuỗi các ký tự được dùng để cho các request cần chứng thực
            )

          }else{

            trả về lỗi(
              message: 'invalid session.'
            )

          }

        }else{

          if OTP không hợp lệ { // OTP user gửi lên không giống với OTP nhận được

            trả về lỗi(
                  ok: false,
                  message: 'Invalid OTP',
                  code: 'invalidOTP',
            )

          }

          trả về lỗi (
              ok: false,
              message: 'Please register account with this OTP.',
              code: 'account404',
          )

        }
      ```

### Example:

```sh
curl -X 'POST' 'https://api-sb.halome.dev/v2/iam/verify-email' \
-H 'accept: */*' \
-H 'Content-Type: application/json' \
-H 'device-id: 111 ' \
-d '{email: "example@gmail.com", code:"000000"}'
```

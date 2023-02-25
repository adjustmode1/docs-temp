# Đăng ký bằng email

### Path:

```
POST /v2/iam/register-email
```

### Header Request:

| Field     | Data type | require | Description     |
| --------- | --------- | ------- | --------------- |
| device-id | String    | True    | id của thiết bị |

### Body Request:

| Field       | Data type | require                                 | Description |
| ----------- | --------- | --------------------------------------- | ----------- |
| email       | String    | email dùng để đăng ký                   | True        |
| displayName | String    | tên người dùng                          | True        |
| code        | String    | mã otp được gửi về thông qua điện thoại | True        |

### Flow:

1. kiểm tra body request:

   - email:

     ```js
     if not (tồn tại '@' và '.' && sau '.' là 2 tới 4 ký tự) {
       trả về lỗi(
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

      ```js
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

   3. trả về token:

      ```js
        trả về {
          token: String, //mã token cho các lần reuqest sau
          refreshToken: String, // mã yêu cầu reset token
          isFirstLogin: Boolean, //có phải đăng nhập lần đầu hay không
        }
      ```

### Example:

```sh
    curl -X 'POST' 'https://api-sb.halome.dev/v2/iam/register-email' \
    -H 'accept: */*' \
    -H 'Content-Type: application/json' \
    -H 'device-id: 111 ' \
    -d '{email: example@gmail.com, displayName: "nameTemp" code:"000000"}'
```

# kích hoạt phiên hoạt động của thiết bị bằng email

### Path:

```
POST /v2/iam/authorize-email
```

### Header Request:

| Field     | Data type | require | Description     |
| --------- | --------- | ------- | --------------- |
| device-id | String    | True    | id của thiết bị |

### Body Request:

| Field | Data type | require | Description                  |
| ----- | --------- | ------- | ---------------------------- |
| email | String    | True    | email người dùng             |
| token | String    | False   | mã token để thông qua google |

### Flow:

1. kiểm tra body request:
    - phone:

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
      if không tồn tại session {
        tạo ra session
      }
      ```
   3. gửi mã otp về thiết bị:
   
      ```js
        {
          ok: true,
          message: 'Please verify your device.'
        }
      ```

### Example:

```sh
curl -X 'POST' 'https://api-sb.halome.dev/v2/iam/authorize-email' \
-H 'accept: */*' \
-H 'Content-Type: application/json' \
-H 'device-id: 111 ' \
-d '{phone: "example@gmail.com", token:"string"}'
```

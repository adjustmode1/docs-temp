# **kích hoạt phiên hoạt động của thiết bị**

### Path:

```
POST /v2/iam/authorize
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

     ```javascript
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

2. main flow:
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
   2. thay đổi định dạng số điện thoại:

      ```javascript
        if bắt đầu bằng '840' {
          thay bằng '84'
        }
      ```
   3. kiểm tra phiên đăng nhập (session):

      ```javascript
      if không tồn tại session { // chưa có kích hoạt phiên hoạt động
        tạo ra session
      }
      ```
   4. gửi mã otp về thiết bị và trả về kết quả:

      ```javascript
        {
          ok: true,
          message: 'Please verify your device.'
        }
      ```

### Example:

```sh
curl -X 'POST' 'https://api-sb.halome.dev/v2/iam/authorize' \
-H 'accept: */*' \
-H 'Content-Type: application/json' \
-H 'device-id: 111 ' \
-d '{phone: "84977585797", token:"string"}'
```

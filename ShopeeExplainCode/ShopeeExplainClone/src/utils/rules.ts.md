# Dưới đây là phần giải thích chi tiết code trong file rules.ts

```jsx
import type { RegisterOptions, UseFormGetValues } from 'react-hook-form'
import * as yup from 'yup'
import { AnyObject } from 'yup/lib/types'
```

- Dưới đây là giải thích chi tiết cho từng phần trong đoạn mã trên:

- `import type { RegisterOptions, UseFormGetValues } from 'react-hook-form'`: Đoạn này thực hiện việc import các kiểu dữ liệu từ thư viện `react-hook-form`.

- `import * as yup from 'yup'`: Đoạn này import toàn bộ nội dung của thư viện `yup`, một thư viện hỗ trợ việc kiểm tra và xác thực dữ liệu.

- `import { AnyObject } from 'yup/lib/types'`: Đoạn này import kiểu `AnyObject` từ module `types` của thư viện `yup`. `AnyObject` là một kiểu dữ liệu chung trong `yup` được sử dụng cho nhiều mục đích khác nhau.

- Tổng cộng, đoạn mã này thực hiện việc import các kiểu dữ liệu cần thiết từ `react-hook-form` và `yup`, chuẩn bị cho việc sử dụng chúng trong mã tiếp theo.

---

```jsx
type Rules = { [key in 'email' | 'password' | 'confirm_password']?: RegisterOptions }
```

- Trong đoạn mã này, chúng ta định nghĩa một kiểu dữ liệu tên là `Rules`. Kiểu dữ liệu này được sử dụng để định nghĩa các quy tắc xác thực cho các trường dữ liệu cụ thể trong một form. Cụ thể, `Rules` là một đối tượng với các thuộc tính là các trường dữ liệu (như `'email'`, `'password'`, `'confirm_password'`) và giá trị của mỗi thuộc tính là một đối tượng có kiểu là `RegisterOptions`, được import từ thư viện `react-hook-form`.

- Mỗi đối tượng `RegisterOptions` chứa các thông tin quy định về việc xác thực và đăng ký các trường dữ liệu trong form. Sử dụng `?` để làm cho các thuộc tính của `Rules` trở thành tùy chọn, có nghĩa là chúng ta có thể chỉ định quy tắc cho một số trường dữ liệu cụ thể mà không cần quy tắc cho tất cả các trường dữ liệu.

---

```jsx
// eslint-disable-next-line @typescript-eslint/no-explicit-any
export const getRules = (getValues?: UseFormGetValues<any>): Rules => ({
  email: {
    required: {
      value: true,
      message: 'Email là bắt buộc'
    },
    pattern: {
      value: /^\S+@\S+\.\S+$/,
      message: 'Email không đúng định dạng'
    },
    maxLength: {
      value: 160,
      message: 'Độ dài từ 5 - 160 ký tự'
    },
    minLength: {
      value: 5,
      message: 'Độ dài từ 5 - 160 ký tự'
    }
  },
  password: {
    required: {
      value: true,
      message: 'Password là bắt buộc'
    },
    maxLength: {
      value: 160,
      message: 'Độ dài từ 6 - 160 ký tự'
    },
    minLength: {
      value: 6,
      message: 'Độ dài từ 6 - 160 ký tự'
    }
  },
  confirm_password: {
    required: {
      value: true,
      message: 'Nhập lại password là bắt buộc'
    },
    maxLength: {
      value: 160,
      message: 'Độ dài từ 6 - 160 ký tự'
    },
    minLength: {
      value: 6,
      message: 'Độ dài từ 6 - 160 ký tự'
    },
    validate:
      typeof getValues === 'function'
        ? (value) => value === getValues('password') || 'Nhập lại password không khớp'
        : undefined
  }
})
```

- Trong đoạn mã này, chúng ta định nghĩa hàm `getRules` để trả về một đối tượng chứa các quy tắc xác thực cho các trường dữ liệu trong `form`. Hàm này nhận một tham số `getValues` có kiểu `UseFormGetValues<any>`, là một hàm từ thư viện `react-hook-form` để truy cập các giá trị của các trường dữ liệu trong `form`.

- Đối tượng trả về từ hàm `getRules` gồm ba trường dữ liệu: `'email'`, `'password'`, và `'confirm_password'`. Mỗi trường dữ liệu lại chứa một đối tượng chứa các quy tắc xác thực. Cụ thể:

1. `'email'`:

- `required`: Chỉ định trường dữ liệu bắt buộc.

- `pattern`: Xác thực định dạng email bằng biểu thức chính quy.

- `maxLength`: Xác thực độ dài tối đa của chuỗi dữ liệu.

- `minLength`: Xác thực độ dài tối thiểu của chuỗi dữ liệu.

2. `'password'`:

- `required`: Chỉ định trường dữ liệu bắt buộc.

- `maxLength`: Xác thực độ dài tối đa của chuỗi dữ liệu.

- `minLength`: Xác thực độ dài tối thiểu của chuỗi dữ liệu.

3. `'confirm_password'`:

- `required`: Chỉ định trường dữ liệu bắt buộc.

- `maxLength`: Xác thực độ dài tối đa của chuỗi dữ liệu.

- `minLength`: Xác thực độ dài tối thiểu của chuỗi dữ liệu.

- `validate`: Xác thực bổ sung bằng cách so sánh giá trị của trường `'confirm_password'` với giá trị của trường `'password'`. Nếu `getValues` là một hàm và giá trị của `'confirm_password'` không khớp với giá trị của `'password'`, thì thông báo lỗi sẽ được hiển thị.

- Tất cả các quy tắc xác thực này được sử dụng để kiểm tra và xác thực các giá trị của các trường dữ liệu trong `form` để đảm bảo rằng chúng đáp ứng yêu cầu và điều kiện cần thiết.

---

```jsx
function testPriceMinMax(this: yup.TestContext<AnyObject>) {
  const { price_max, price_min } = this.parent as {
    price_min: string
    price_max: string
  }
  if (price_min !== '' && price_max !== '') {
    return Number(price_max) >= Number(price_min)
  }
  return price_min !== '' || price_max !== ''
}
```

- Đoạn code này định nghĩa một hàm tùy chỉnh `testPriceMinMax` dùng để kiểm tra giá trị của trường `price_min` và `price_max` theo một số quy tắc.

- `this: yup.TestContext<AnyObject>`: Hàm này sử dụng context của kiểu `yup.TestContext<AnyObject>`, cho phép truy cập vào các giá trị khác nhau trong schema đang được kiểm tra.

- `const { price_max, price_min } = this.parent as { price_min: string; price_max: string }`: Đoạn này trích xuất các giá trị `price_min` và `price_max` từ đối tượng cha (`this.parent`) và đảm bảo rằng cả hai giá trị này đều có kiểu dữ liệu là chuỗi (string).

- `if (price_min !== '' && price_max !== '') { ... }`: Kiểm tra nếu cả `price_min` và `price_max` đều không trống.

- `return Number(price_max) >= Number(price_min)`: Kiểm tra nếu giá trị số của `price_max` lớn hơn hoặc bằng giá trị số của `price_min`. Nếu đúng, hàm trả về `true`, ngụ ý rằng kiểm tra này đã thành công.

- `return price_min !== '' || price_max !== ''`: Nếu hoặc `price_min` hoặc `price_max` không trống, hàm trả về `true`.

- Tóm lại, hàm `testPriceMinMax` này được sử dụng như một kiểm tra tùy chỉnh trong schema để kiểm tra nếu cả `price_min` và `price_max` đều không trống thì kiểm tra xem `price_max` có lớn hơn hoặc bằng `price_min` không. Nếu một trong hai trường `price_min` hoặc `price_max` không trống, thì kiểm tra sẽ thành công.

---

```jsx
const handleConfirmPasswordYup = (refString: string) => {
  return yup
    .string()
    .required('Nhập lại password là bắt buộc')
    .min(6, 'Độ dài từ 6 - 160 ký tự')
    .max(160, 'Độ dài từ 6 - 160 ký tự')
    .oneOf([yup.ref(refString)], 'Nhập lại password không khớp')
}
```

- Đoạn code này định nghĩa một hàm tạo schema kiểm tra dành cho trường `confirm_password` dựa trên giá trị của trường `password`.

- `const handleConfirmPasswordYup = (refString: string) => { ... }`: Đây là hàm tạo schema kiểm tra trường `confirm_password`. Hàm này có tham số `refString` là chuỗi tham chiếu đến trường `password`.

- `return yup.string()...`: Hàm này bắt đầu bằng việc tạo một schema kiểm tra cho kiểu dữ liệu là chuỗi.

- `.required('Nhập lại password là bắt buộc')`: Đoạn này thêm một kiểm tra bắt buộc, yêu cầu trường `confirm_password` không được trống.

- `.min(6, 'Độ dài từ 6 - 160 ký tự')`: Thêm một kiểm tra độ dài tối thiểu là 6 ký tự cho trường `confirm_password`.

- `.max(160, 'Độ dài từ 6 - 160 ký tự')`: Thêm một kiểm tra độ dài tối đa là 160 ký tự cho trường `confirm_password`.

- `.oneOf([yup.ref(refString)], 'Nhập lại password không khớp')`: Kiểm tra này sử dụng `yup.ref(refString)` để so sánh giá trị của trường `confirm_password` với giá trị của trường `password`. Nếu giá trị của `confirm_password` không khớp với giá trị của `password`, thông báo lỗi sẽ được hiển thị.

- Tóm lại, hàm `handleConfirmPasswordYup` này tạo ra một schema kiểm tra cho trường `confirm_password`, bao gồm kiểm tra bắt buộc, độ dài, và so sánh giá trị của `confirm_password` với giá trị của `password`.

---

```jsx
export const schema = yup.object({
  email: yup
    .string()
    .required('Email là bắt buộc')
    .email('Email không đúng định dạng')
    .min(5, 'Độ dài từ 5 - 160 ký tự')
    .max(160, 'Độ dài từ 5 - 160 ký tự'),
  password: yup
    .string()
    .required('Password là bắt buộc')
    .min(6, 'Độ dài từ 6 - 160 ký tự')
    .max(160, 'Độ dài từ 6 - 160 ký tự'),
  confirm_password: handleConfirmPasswordYup('password'),
  price_min: yup.string().test({
    name: 'price-not-allowed',
    message: 'Giá không phù hợp',
    test: testPriceMinMax
  }),
  price_max: yup.string().test({
    name: 'price-not-allowed',
    message: 'Giá không phù hợp',
    test: testPriceMinMax
  }),
  name: yup.string().trim().required('Tên sản phẩm là bắt buộc')
})
```

- Đoạn code này định nghĩa một schema kiểm tra (validation schema) sử dụng thư viện `Yup` để kiểm tra dữ liệu của các trường trong form.

1. `yup.object({ ... })`: Tạo một schema kiểm tra dữ liệu cho một đối tượng.

2. `email: yup.string().required('Email là bắt buộc')...`: Định nghĩa kiểm tra cho trường `email`:

- `.string()`: Xác định rằng trường `email` là kiểu dữ liệu chuỗi.
- `.required('Email là bắt buộc')`: Yêu cầu trường `email` không được trống.
- `.email('Email không đúng định dạng')`: Kiểm tra định dạng `email` hợp lệ.
- `.min(5, 'Độ dài từ 5 - 160 ký tự')`: Kiểm tra độ dài tối thiểu của trường `email` là 5 ký tự.
- `.max(160, 'Độ dài từ 5 - 160 ký tự')`: Kiểm tra độ dài tối đa của trường `email` là 160 ký tự.

3. `password: yup.string().required('Password là bắt buộc')...`: Định nghĩa kiểm tra cho trường `password`, tương tự như kiểm tra cho trường `email`.

4. `confirm_password: handleConfirmPasswordYup('password')`: Gọi hàm `handleConfirmPasswordYup` để tạo schema kiểm tra cho trường `confirm_password`, kiểm tra bắt buộc, độ dài, và so sánh giá trị với trường `password`.

5. `price_min: yup.string().test({ ... })`: Định nghĩa kiểm tra tùy chỉnh cho trường `price_min` bằng cách sử dụng `.test()`. Kiểm tra này sử dụng hàm `testPriceMinMax` để kiểm tra xem giá trị của `price_max` có lớn hơn hoặc bằng `price_min` hay không.

6. `price_max: yup.string().test({ ... })`: Định nghĩa kiểm tra tùy chỉnh cho trường `price_max`, tương tự như trường `price_min`.

7. `name: yup.string().trim().required('Tên sản phẩm là bắt buộc')`: Định nghĩa kiểm tra cho trường `name`:

- `.trim()`: Loại bỏ khoảng trắng ở đầu và cuối chuỗi.
- `.required('Tên sản phẩm là bắt buộc')`: Kiểm tra yêu cầu trường `name` không được trống.

- Tóm lại, đoạn code này định nghĩa một schema kiểm tra dữ liệu cho các trường trong form, bao gồm kiểm tra định dạng email, độ dài, so sánh giá trị của `confirm_password` và `password`, và kiểm tra tùy chỉnh cho các trường `price_min` và `price_max`.

---

```jsx
export const userSchema = yup.object({
  name: yup.string().max(160, 'Độ dài tối đa là 160 ký tự'),
  phone: yup.string().max(20, 'Độ dài tối đa là 20 ký tự'),
  address: yup.string().max(160, 'Độ dài tối đa là 160 ký tự'),
  avatar: yup.string().max(1000, 'Độ dài tối đa là 1000 ký tự'),
  date_of_birth: yup.date().max(new Date(), 'Hãy chọn một ngày trong quá khứ'),
  password: schema.fields['password'],
  new_password: schema.fields['password'],
  confirm_password: handleConfirmPasswordYup('new_password')
})
```

- Đoạn code này định nghĩa một schema kiểm tra (validation schema) dành cho dữ liệu của người dùng (user) trong một tình huống cụ thể. Nó sử dụng thư viện `Yup` để kiểm tra dữ liệu của các trường trong user schema.

- `yup.object({ ... })`: Tạo một schema kiểm tra dữ liệu cho một đối tượng.

- `name: yup.string().max(160, 'Độ dài tối đa là 160 ký tự')`: Định nghĩa kiểm tra cho trường `name`:

  - `.string()`: Xác định rằng trường `name` là kiểu dữ liệu chuỗi.
  - `.max(160, 'Độ dài tối đa là 160 ký tự')`: Kiểm tra độ dài tối đa của trường `name` là 160 ký tự.

- `phone: yup.string().max(20, 'Độ dài tối đa là 20 ký tự')`: Định nghĩa kiểm tra cho trường `phone`:

  - `.string()`: Xác định rằng trường `phone` là kiểu dữ liệu chuỗi.
  - `.max(20, 'Độ dài tối đa là 20 ký tự')`: Kiểm tra độ dài tối đa của trường `phone` là 20 ký tự.

- `address: yup.string().max(160, 'Độ dài tối đa là 160 ký tự')`: Định nghĩa kiểm tra cho trường `address`, tương tự như các trường trước đó.

- `avatar: yup.string().max(1000, 'Độ dài tối đa là 1000 ký tự')`: Định nghĩa kiểm tra cho trường `avatar`:

  - `.string()`: Xác định rằng trường `avatar` là kiểu dữ liệu chuỗi.
  - `.max(1000, 'Độ dài tối đa là 1000 ký tự')`: Kiểm tra độ dài tối đa của trường `avatar` là 1000 ký tự.

- `date_of_birth: yup.date().max(new Date(), 'Hãy chọn một ngày trong quá khứ')`: Định nghĩa kiểm tra cho trường `date_of_birth`:

  - `.date()`: Xác định rằng trường `date_of_birth` là kiểu dữ liệu ngày tháng.
  - `.max(new Date(), 'Hãy chọn một ngày trong quá khứ')`: Kiểm tra ngày không được lớn hơn ngày hiện tại.

- `password: schema.fields['password']`: Trường `password` sử dụng cùng kiểm tra như được định nghĩa trong schema `schema`.

- `new_password: schema.fields['password']`: Tương tự như trường `password`, đối với `new_password`.

- `confirm_password: handleConfirmPasswordYup('new_password')`: Gọi hàm `handleConfirmPasswordYup` để tạo schema kiểm tra cho trường `confirm_password`, sử dụng `new_password` để so sánh.

- Tóm lại, đoạn code này định nghĩa một schema kiểm tra dữ liệu cho các trường của đối tượng người dùng (user), bao gồm các kiểm tra độ dài, kiểu dữ liệu và so sánh giá trị giữa các trường.

---

```jsx
export type UserSchema = yup.InferType<typeof userSchema>
```

- Dòng code này định nghĩa một kiểu dữ liệu mới có tên là `UserSchema`. Đây là kiểu dữ liệu được tạo ra bằng cách sử dụng `InferType` từ thư viện `Yup` để suy ra các kiểu dữ liệu của schema `userSchema` đã được định nghĩa trước đó.

- Cụ thể, `userSchema` là một schema kiểm tra dữ liệu của các trường trong đối tượng người dùng, như tên, số điện thoại, địa chỉ, avatar, ngày sinh, mật khẩu và xác nhận mật khẩu. Khi chúng ta sử dụng `yup.InferType<typeof userSchema>`, thư viện `Yup` sẽ suy ra các kiểu dữ liệu cụ thể của các trường trong `userSchema` và tạo ra một kiểu dữ liệu mới dựa trên các kiểu dữ liệu này.

- Ví dụ, nếu `userSchema` kiểm tra rằng trường `name` phải là kiểu chuỗi (string), thì kiểu `UserSchema` cũng sẽ áp dụng kiểu chuỗi cho trường `name`.

- Tóm lại, dòng code này cho phép chúng ta tạo một kiểu dữ liệu mới `UserSchema` dựa trên schema kiểm tra `userSchema` đã được định nghĩa trước đó.

---

```jsx
export type Schema = yup.InferType<typeof schema>
```

- Dòng code này định nghĩa một kiểu dữ liệu mới có tên là `Schema`. Tương tự như trước đó, chúng ta sử dụng `yup.InferType<typeof schema>` để suy ra các kiểu dữ liệu cụ thể của các trường trong schema `schema` đã được định nghĩa trước đó.

- schema là một `schema` kiểm tra dữ liệu của các trường trong đối tượng, như email, password, confirm_password, price_min, price_max, và name. Khi chúng ta sử dụng `yup.InferType<typeof schema>`, thư viện `Yup` sẽ suy ra các kiểu dữ liệu cụ thể của các trường trong `schema` và tạo ra một kiểu dữ liệu mới dựa trên các kiểu dữ liệu này.

- Ví dụ, nếu `schema` kiểm tra rằng trường `email` phải là kiểu chuỗi (string), thì kiểu `Schema` cũng sẽ áp dụng kiểu chuỗi cho trường `email`.

- Tóm lại, dòng code này cho phép chúng ta tạo một kiểu dữ liệu mới `Schema` dựa trên schema kiểm tra `schema` đã được định nghĩa trước đó.

---

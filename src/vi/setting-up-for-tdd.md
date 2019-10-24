## Cài đặt vue-cli

`vue-test-utils` là thư viện kiểm thử chính thức cho Vue và sẽ được sử dụng xuyên suốt trong cẩm nang này. Nó chạy trong cả môi trường trình duyệt và Node.js và hoạt động với bất kỳ người chạy thử nghiệm nào. Chúng tôi sẽ chạy các bài test trong môi trường Node.js trên tất cả nội dung của cẩm nang.

`vue-cli` là cách dễ nhất để bắt đầu. Nó sẽ thiết lập một dự án, cũng như cấu hình Jest, một khung thực hiện các thử nghiệm phổ biến. Cài đặt nó bằng cách chạy:

```sh
yarn global add @vue/cli
```

hoặc với npm:

```sh
npm install -g @vue/cli
```

Tạo một dự án mới bằng cách chạy `vue create [project-name]`. Chọn "Manually select features" và "Unit Testing", và "Jest" cho việc kiểm thử.

Sau khi cài đặt kết thúc, `cd` vào trong dự án và chạy `yarn test:unit`. Nếu mọi thứ đều không có vấn đề gì, bạn sẽ thấy:

```
 PASS  tests/unit/HelloWorld.spec.js
  HelloWorld.vue
    ✓ renders props.msg when passed (26ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        2.074s
```

Chúc mừng bạn vừa chạy các bài kiểm tra đầu tiên thành công.

## Viết các bài tests đầu tiên của bạn

Chúng tôi chạy thử nghiệm hiện có đi kèm với dự án. Hãy gõ những dòng đầu tiên, hãy viết những component của chúng ta kèm với các bài kiếm tra. Theo truyền thống khi làm TDD, bạn viết các đoạn mã test fail trước, sau đó mới triển khai code sao cho các bài test pass. Bây giờ, chúng ta sẽ viết component đầù tiên. 

Chúng ta không `cần src/components/HelloWorld.vue` hoặc `tests/unit/HelloWorld.spec.js` nữa, vì vậy bạn có thể xóa chúng nếu có thể.

## Tạo components `Greeting`

Tạo một file `Greeting.vue` trong `src/components`. Bên trong `Greeting.vue`, thêm những thứ sau đây:

```vue
<template>
  <div>
    {{ greeting }}
  </div>
</template>

<script>
export default {
  name: "Greeting",

  data() {
    return {
      greeting: "Vue and TDD"
    }
  }
}
</script>
```

## Viết test

`Greeting` chỉ có một trách nhiệm là render giá trị `greeting`. Chiến lược ở đây là:

1. Render component với `mount`
2. Assert văn bản bên trong components có chứa từ khóa "Vue and TDD"

Tạo một `Greeting.spec.js` bên trong `tests/unit`. Bên trong, import `Greeting.vue`, cũng như `mount`, và thêm 1 số outline cho test case của bạn:

```
import { mount } from '@vue/test-utils'
import Greeting from '@/components/Greeting.vue'

describe('Greeting.vue', () => {
  it('renders a greeting', () => {

  })
})
```

Có các cú pháp khác nhau được sử dụng cho TDD, chúng ta sẽ sử dụng cú pháp thường thấy về  `describe` và `it` đi kèm với Jest. `describe` phác thảo những gì bài test muốn, ở đây là `Greeting.vue`. `it` đại diện cho một phần kiểm tra mà đối tượng của bài test đó phải hoàn thành. Khi chúng ta có nhiều tính năng ở components thì chúng ta có nhiều khối `it`.

Bây giờ chúng ta nên render component với `mount`. Bây giờ gán component cho 1 biến được gọi là `wrapper`. Chúng ta cũng sẽ in đầu ra, để đảm bảo mọi thứ chạy chính xác:

```js
const wrapper = mount(Greeting)

console.log(wrapper.html())
```

## Chạy thử nghiệm

Chạy test bằng cách gõ `yarn test:unit` trên terminal của bạn. Bất kì file `tests` đều có kết thúc bằng đuôi `.spec.js` được thực hiện tự động. Nếu mọi thứ đều ổn, bạn sẽ thấy:

```
PASS  tests/unit/Greeting.spec.js
Greeting.vue
  ✓ renders a greeting (27ms)

console.log tests/unit/Greeting.spec.js:7
  <div>
    Vue and TDD
  </div>
```

Chúng ta có thể thấy các markup là chính xác, và thử nghiệm chạy thành công. Test case sẽ pass vì không có sai sót, nhưng có 1 vấn đề khi chúng ta thay đổi `Greeting.vue` và xóa `greeting` từ template, chúng vẫn sẽ pass. Hãy thay đổi điều đó.

## Tạo khẳng định

Chúng ta cần đưa ra một khẳng định để đảm bảo component hoạt động chính xác. Chúng ta có thể làm điều đó bằng cách sử dụng `expect` API của Jest. Nó sẽ trông như thế này: `expect(result).to [matcher] (actual)`.

_So khớp_ là một phương thức để so sánh các giá trị và đối tượng. Ví dụ:

```js
expect(1).toBe(1)
```

Một danh sách đầy đủ các matchers có sẵn trong tài liêu của Jest [Jest documentation](http://jestjs.io/docs/en/expect). `vue-test-utils` không bao gồm bất kỳ công cụ so sách nào vì những gì Jest mang lại là quá đủ để dùng. Chúng ta muốn so sánh văn bản từ `Greeting`. Chúng ta có thể viết như sau:

```js
expect(wrapper.html().includes("Vue and TDD")).toBe(true)
```

nhưng `vue-test-utils` có một cách tốt hơn để đánh dấu - `wrapper.text`. Hãy kết thúc bài test nhưu sau:

```js
import { mount } from '@vue/test-utils'
import Greeting from '@/components/Greeting.vue'

describe('Greeting.vue', () => {
  it('renders a greeting', () => {
    const wrapper = mount(Greeting)

    expect(wrapper.text()).toMatch("Vue and TDD")
  })
})
```

Chúng ta không cần `console.log`, vì vậy bạn có thể xóa chúng. Chạy test với `yarn unit:test`, và nếu mọi thứ đều suôn sẻ, bạn sẽ nhận được:

```
PASS  tests/unit/Greeting.spec.js
Greeting.vue
  ✓ renders a greeting (15ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.477s, estimated 2s
```

Trông ổn rồi đó, nhưng bạn phải luôn luôn thấy một test thất bại, sau đó là vượt qua, để đảm bảo rằng test đó hoạt động đúng. Trong TDD truyền thống, bạn sẽ viết các test trước khi thực hiện trong thực tế, hãy thấy nó fail, sau đó sử dụng các lỗi không thành công để thực hiện theo các đoạn mã của chúng ta. Hãy chắc chắn rằng bài test thực hiện hiệu quả đúng theo phong cách TDD. Cập nhật `Greeting.vue`:

```vue
<template>
  <div>
    {{ greeting }}
  </div>
</template>

<script>
export default {
  name: "Greeting",

  data() {
    return {
      greeting: "Vue without TDD"
    }
  }
}
</script>
```

Và bây giờ chạy thử nghiệm với `yarn test:unit`:

```
FAIL  tests/unit/Greeting.spec.js
Greeting.vue
  ✕ renders a greeting (24ms)

● Greeting.vue › renders a greeting

  expect(received).toMatch(expected)

  Expected value to match:
    "Vue and TDD"
  Received:
    "Vue without TDD"

     6 |     const wrapper = mount(Greeting)
     7 |
  >  8 |     expect(wrapper.text()).toMatch("Vue and TDD")
       |                            ^
     9 |   })
    10 | })
    11 |

    at Object.<anonymous> (tests/unit/Greeting.spec.js:8:28)
```

Jest cho chúng ta phản hồi rất tốt. Chúng ta có thể thấy các kết quả mong đợi và thực tế, cũng như thất bại. Hãy sửa lại `Greeting.vue` và đảm bảo test được thông qua một lần nữa.

Tiếp theo chúng ta sẽ xem xet hai phương thức `vue-test-utils` cung cấp để render các components: `mount` và `shallowMount`.

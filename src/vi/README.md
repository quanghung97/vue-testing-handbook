## Hướng dẫn này là gì?

Chào mừng đến với cẩm nang kiểm thử với Vue.js

Đây là một tập hợp các ví dụ ngắn, tập trung vào cách kiểm tra các components của Vue. Nó sử dụng `vue-test-utils`, thư viện chính thức để kiểm tra các components của Vue và Jest, một khung thử nghiệm hiện đại. Chúng bao gồm `vue-test-utils` API, cũng như các bài thực hành tốt nhất để thử nghiệm các components.

Mỗi phần là độc lập với những phần khác. Chúng tôi bắt đầu bằng cách thiết lập một môi trường `vue-cli` và viết một bài kiểm tra đơn giản. Tiếp theo, hai cách để render một components được bàn tới là `mount` và `shallowMount`. Sự khác biệt sẽ được chứng minh và giải thích.

Sau đó trở đi, chúng tôi đề cập đến cách kiếm tra các kịch bản khác nhau phát sinh khi kiểm tra các components, chẳng hạn:

- Nhận props
- Sử dụng tính chất computed
- render các components
- emit events

sau đó chúng ta sẽ đến các trường hợp thú vị hơn, như là:

- Thực hành kiểm thử trong Vuex (bên trong components và độc lập)
- Kiểm thử bộ định tuyến Vue (Vue router)
- Thực nghiệm liên quan đến các components bên thứ ba

Chúng tôi cũng khám phá cách sử dụng API Jest để làm cho các bài kiểm tra của chúng tôi một cách thông minh hơn, chẳng hạn như:

- mocking các phần hồi API
- mocking và spying trên các modules
- sử  dụng snapshots

## Ngôn ngữ

Hiện tại, hướng dân này được viết các loại ngôn ngữ tiếng Anh, tiếng Nhật, tiếng Nga và tiếng Việt.

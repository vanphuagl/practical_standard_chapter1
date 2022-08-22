# **practical_standard_chapter1**

### Cấu tạo file nguồn (core file) của Wordpress
Khi kết nối với trang web wordpress của mình bằng ftp hoặc trình quản lý tệp, ta thư mục gốc cảu trang web wordpress của mình. Một số các file và thư mục wordpress cốt lõi.
- dir wp-admin, wp-includes
- index.php
- wp-activate.php
- wp-blog-header.php
- wp-comments-post.php
- wp-config-sample.php
- wp-cron.php
- wp-links-opml.php
- wp-load.php
- wp-login.php
- wp-mail.php
- wp-settings.php
- wp-signup.php
- wp-trackback.php
- xmlrpc.php

### Cách thức kết nối tới database
Khi kết nối tới bảng nào đó có sẵn trong wordpress thì ta có thể gọi trực tiếp bằng **$wpdb->posts** để kết nối tới bảng POSTS của wordpress.

Class $wpdb là class dùng để tương tác với database trong wordpress (cái này wordpress cung cấp sẵn để mình dùng).

Wordpress cũng có một vài quy ước như khi muốn kết nối tới bàng nào đó trong wordpress thì ta cần phải gọi prefix kèm theo tên bảng, ta sẽ gọi bất kỳ lện mySql nào trong query của wordpress để trả về là một object, thông thường ta sẽ không chạy các lệnh select trong query này.

Ta sẽ sử dùng **$wpdb->get_results()** để làm việc này. Sau khi lấy được kết quả ta sẽ gọi hàm forEach để hiển thị kết quả vì nó trả về là Array. Một hàm khác trong class Wpdb của wordpress là get_row, hàm này sẽ trả về một row trong cơ sở dữ liệu và trả về dạng object (Sử dụng khi torng trường hợp muốn đếm dữ liệu, hoặc tìm một row thỏa mãn điều kiện trong bảng).

### Xử lý vòng lặp (loop) cho bài post cơ bản trong Wordpress theme
Loop là cơ chế mặc định mà WordPress sử dụng để hiển thị các bài viết thông qua một template files (index.php, archive.php, page.php …). Số lượng bài post sẽ được truy xuất bởi số lượng bài post để hiển thị trên mỗi trang được xác định trong cài đặt Reading.

Để thao tác truy xuất, hiển thị dữ liệu bên trong vòng lặp, wordpress cung cấp cho chúng ta class **$wp_query**.

Hiển thị bài viết:
- Kiểm tra xem có bài viết hay không bằng phương thức **have_posts** của đối tượng **$wp_query**.
- Duyệt qua từng bài viết bằng phương thức **the_post**. Phương thức này sẽ đánh dấu bài viết đã được duyệt qua và dẩy sang bài viết kế tiếp. Nếu không có **the_post**, vòng lặp sẽ vô tận với duy nhất 1 bài viết.
- Sử dụng các tempalte tag để lấy hoặc hiển thị nội dung.

```
if(have_posts()):
  while(have_posts()):
      the_post();
      // hiển thị bài viết
      the_title();
      the_content();
  endwhile;
endif;
```

### Cấu tạo file template - Các file thiết yếu
Template file xuất hiện trong theme và thể hiện cách website được hiển thị.

Khi một khách truy cập một trang trên website, wordpress sẽ tải một template dựa theo yêu cầu. Loại nội dung được hiển thị bởi template file sẽ được xác định bởi post type có liên quan. Sau đó, template hierarchy sẽ mô tả template file được wordpress lựa chọn dựa trên loại yêu cầu của khách và tình trạng của file template trong giao diện. Cuối cùng, máy chủ sẽ phân tích cú pháp php trong template và trả về html cho khách truy cập.

Các file template phổ biến trong wordpress
- index.php (đây là template chính bất kỳ theme nào cũng phải có và cũng là quan trọng nhất)
- style.css
- rtl.css
- comments.php
- front-page.php
- home.php
- header.php
- footer.php
- single.php
- sidebar.php
- page.php
- category.php
- function.php
- archive.php
- search.php
- searchform.php
- image.php
- 404.php

### Nội dung hàm số `the_title()`, `get_the_title()` và sự khác nhau giữa chúng
`the_title()`: dùng để hiển thị tiêu đề trang hoặc bài viết.

`get_the_title()`: là hàm wordpress được sử dụng để lấy tiêu đề của bài đăng. Nó nhận vào một tham số tùy chọn có thể là ID của bài đăng hoặc đối tượng của class wp_post.

Sự khác nhau giữa `the_title()` và `get_the_title()`:
- Khi viết `get_the_title()` thì cần phải thêm **echo** phía trước còn `the_title()` thì không phải thêm gì cả.
- `the_title()` nhận ba tham số tùy chọn, hai tham số đầu tiên là tham số tùy chọn string và tham số thứ ba là boolean. `get_the_title()` chỉ nhận một tham số tùy chọn, đó là id của bài viết hoặc class wp_post để lấy dữ liệu khác của bài viết như tiêu đề của bài post chính.
- `the_title()` sẽ lặp lại giá trị của nó, nhưng `get_the_title()` không lặp lại và chỉ trả về giá trị. Do đó nên dùng `the_title()` trong vòng lặp và dùng `get_the_title()` hữu ich khi ở bên ngoài vòng lặp để truy suất bài viết.

### Nội dung hàm số `get_stylesheet_directory_uri()`, `get_stylesheet_directory()` và sự khác nhau giữa chúng
`get_stylesheet_directory()`: Khi cần phải include file khác vào bên trong cấu trúc thư mục, ta cần sử dụng `get_stylesheet_directory()`. Từ style.css trong thư mục parent theme, `get_stylesheet_directory()` trỏ tới thư mục trong child theme của mình (không phải trong parent theme).

`get_stylesheet_directory_uri()`: Sử dung `get_stylesheet_directory_uri()` để hiển thị hình ảnh được lưu trữ bên trong thư mục /images bên trong thư mục của child theme.

Không như `get_stylesheet_directory()` sẽ cung cấp đường dẫn của file thì `get_stylesheet_directory_uri()` cung cấp URL, được sử dụng với các tài nguyên front-end.

# Github
## Giới thiệu về github
Github là dịch vụ cung cấp kho lưu trữ mà nguồn. Nó giúp chúng ta quản lý source code tổ chức theo dạng phân tán (tức là nó sẽ giúp ta có thể quản lý mã nguồn trên nhiều máy khác nhau, chứ không như trước code sẽ được lưu trữ trên một máy), giúp đồng bộ code trong nhóm và kiểm tra source code.

## Git HTTPS và Git SSH
`HTTPS` và `SSH` là 2 giao thức truyền dữ liệu trong github.
Để hiểu về HTTP thì trước hết chúng ta phải biết HTTP là gì? Thì HTTP là dữ liệu truyền giữa client và server. Người dùng sẽ đóng vai trò là client gửi yêu cầu (request) đến server và server sẽ phản hồi(response) về cho người dùng. Nhưng HTTP sẽ truyền đi với thông tin truyền đi không được mã hóa do đó nó dễ bị lấy cắp thông tin, bảo mật kém và dễ bị rò rỉ. Không nên dùng phương thức này bởi nó không có tính an toàn cao nhưng đối với một số trang web không cần tính bảo mật thì họ vẫn đang dùng phương thức này.

Vì vậy ta cần một giao thức bảo mật hơn và phổ biến hiện nay đó chính là HTTPS. HTTPS là sự kết hợp giữa SSL với HTTP (Về SSL là một tiêu chuẩn cho phép thiết lập kết nối được mã hóa giữa client và server, giúp bảo vệ thông tin nhạy cảm) giao thức bảo mật hơn của HTTP được mã hóa thông tin và nó sẽ hỗ trợ việc xác thực danh tính người dùng bằng cách đăng nhập tài khoản. Hầu hết các trang web hiện nay đều sử dụng giao thức này bởi tính bảo mật cao.
Tuy thế nhưng HTTPS trong github vẫn còn một số nhược điểm:
* Mỗi khi thao tác các lệnh git với repo, ta sẽ cần phải nhập lại password.
* Nếu bị lộ thông tin tài khoản, người khác sẽ có thể toàn quyền với repo của chúng ta.

SSH là một giao thức mạng để đăng nhập an toàn từ xa và dữ liệu đều được mã hóa mà không cần phải thực hiện việc đăng nhập như giao thức HTTPS. Việc xác thực các thiết bị từ xa sử dụng cặp mã khóa public và private. Để tạo mã khóa, trước tiên chúng ta cần phải có tài khoảng github tên trang https://github.com/, tải và cài đặt git: https://git-scm.com/downloads
Sau khi cài đặt, chúng ta vào mở Git Bash và sử dụng lệnh:
```sh
ssh-keygen -t ed25519 -C "your_email@example.com"
```
(https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
Sau khi sử dụng câu lệnh chúng ta sẽ có một folder `.ssh` gồm hai file chứa khóa private và public.
Truy cập https://github.com/settings/keys . Chọn New SSH Key đặt tên cho title và dán key public sau đó chọn Add SSH key

![image](https://github.com/Phu-Vu/learn-git/assets/111777472/445b30e5-d178-4955-979d-7801ee68bc7e)

Thế nhược điểm của SSH là gì?
* Khi không có tài khoản git thì bạn không thể clone được repo mà chỉ có thể sử dụng giao thức HTTPS
* Phải cài đặt mã khóa cho github

Tóm lại, cả hai giao thức HTTPS và SSH đều là hai giao thức bảo mật dữ liệu an toàn. Thế nhưng tại sao nhiều người lại sử dụng giao thức SSH hơn?
Như đã nói ở trên thì giao thức SSH giúp chúng ta thao tác với git mà không cần phải đăng nhập như HTTPS. Việc đăng nhập làm chúng ta gián đoạn công việc gây ra sự bất tiện nhất thời.

## Để sử dụng git thì chúng ta cần phải hiểu cách mà git quản lý file và các khái niệm về pull request, branch(nhánh)
### Cách git quản lý file
Chúng ta cần phải biết về ba trạng thái của repo:

![image](https://github.com/Phu-Vu/learn-git/assets/111777472/c9c10932-a166-4780-9249-2e9adad943b0)

Tương ứng với 3 vị trí này thì chúng ta có 3 lệnh cơ bản:

![image](https://github.com/Phu-Vu/learn-git/assets/111777472/6d845e95-7fcf-4ab0-a3ae-2dd4172f0667)

Working directory: là nơi mà ta đang thực hiện các thao tác với code hoặc là một folder ở trên máy tính rồi chúng ta dùng git init để tạo ra một file .git bên trong nó. Giải thích về file .git thì nó là một file để git có thể lưu trữ thông tin và lịch sử thay đổi của code.

Stagging area: là nơi lưu trữ các thay đổi trước khi `commit`. Sau khi thao tác xong trên woking directory thì chúng ta sẽ `commit` và `push` để code được đẩy lên repository nhưng chúng ta cần phải `add` những file thay đổi vào trong stagging area.

Git directory (repository): là nơi mà chúng ta lưu trữ code. Sau khi commit thì các thay đổi sẽ được lưu lại trên repository. Hiểu đơn giản thì repository là nơi chứa các toàn bộ các thông tin về project. Tất cả các dữ liệu của repository đều được chứa trong file .git

### Pull request
Pull request là hành động mà người code yêu cầu được đẩy code vào nhánh khác (mình nghĩ thông thường là nhánh main). Từ đó mọi người có thể review, chỉnh sửa code một cách thống nhất rồi mới merge(hợp nhất). Thế tại sao phải tạo pull request nhỉ ? Sao không merge luôn vào code chính =.=
* Như nói ở trên, code cần được phải quản lý mội cách chặt chẽ, để đảm bảo được chất lượng của code, tránh các lỗi và xung đột trong quá trình phát triển.
* Giúp quá trình kiểm tra dễ dàng hơn và chấp nhận các code.

### Branch
Thông thường trong các project của chúng ta luôn có một nhánh code chính đó là nhánh main. Vậy tại sao phải chia ra nhánh code để làm gì? Tại sao không gộp chung hết vào một nhánh?
Để giải đáp các câu hỏi này thì trước tiên chúng ta tìm hiểu branch là gì? Như tên gọi của nó thì branch là nhánh của repository, mỗi nhánh sẽ tương tự như một không gian làm việc độc lập phát triển mà không làm ảnh hưởng tới các nhánh khác. Sau khi các nhánh được hoàn thành thì chúng ta có thể pull request để team có thể review code (chứ đừng ngáo như mình pull request rồi tự merge luôn) sau đó nếu code ok thì có thể merge. Do nhánh hoàn toàn độc lập với nhau nên việc tạo nhành để dự án chia thành nhiều phần nhỏ là phương án tốt nhất để quản lý dự án.

## Github là công cụ dùng để quản lý mã nguồn. Thế chúng ta làm quản lý mã nguồn thế nào? Git đã cung cấp cho chúng ta một số câu lệnh để có thể dễ dàng thao tác hơn. Nhưng ở đây chúng ta sẽ nói về một số câu lệnh cơ bản của git.
## Một số lệnh cơ bản về github
### Git add
git add dùng để cập nhật trong thư mục, sẫn sàng để commit lên repo. Khi sử dụng git add nó sẽ lưu lại nhanh những thay đổi
thông thường chúng ta sẽ sử dụng câu lệnh là `git add .` Dấu chấm ở đây là chúng ta sẽ thêm tất cả các file git thay đổi

### Git commit
git commit -m "message" . -m ở đây là viết tắt của từ message


## Tài liệu tham khảo:
https://en.wikipedia.org/wiki/Secure_Shell
https://viblo.asia/p/git-dung-https-hay-ssh-eW65Gm9jZDO

trạng thái của repo https://viblo.asia/p/git-overview-oOVlYq3Bl8W

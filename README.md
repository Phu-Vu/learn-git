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

![image](https://github.com/Phu-Vu/learn-git/blob/main/SSH%20key.png)

Thế nhược điểm của SSH là gì?
* Khi không có tài khoản git thì bạn không thể clone được repo mà chỉ có thể sử dụng giao thức HTTPS
* Phải cài đặt mã khóa cho github

Tóm lại, cả hai giao thức HTTPS và SSH đều là hai giao thức bảo mật dữ liệu an toàn. Thế nhưng tại sao nhiều người lại sử dụng giao thức SSH hơn?
Như đã nói ở trên thì giao thức SSH giúp chúng ta thao tác với git mà không cần phải đăng nhập như HTTPS. Việc đăng nhập làm chúng ta gián đoạn công việc gây ra sự bất tiện nhất thời.

## Để sử dụng git thì chúng ta cần phải hiểu cách mà git quản lý file và các khái niệm về pull request, branch(nhánh)
### Cách git quản lý file
Chúng ta cần phải biết về ba trạng thái của repo:

![image](https://github.com/Phu-Vu/learn-git/blob/main/github.png)

Tương ứng với 3 vị trí này thì chúng ta có 3 lệnh cơ bản:

![image](https://github.com/Phu-Vu/learn-git/blob/main/github2.png)

Working directory: là nơi mà ta đang thực hiện các thao tác với code hoặc là một folder ở trên máy tính rồi chúng ta dùng git init để tạo ra một file .git bên trong nó. Giải thích về file .git thì nó là một file để git có thể lưu trữ thông tin và lịch sử thay đổi của code.

Stagging area: là nơi lưu trữ các thay đổi trước khi `commit`. Sau khi thao tác xong trên woking directory thì chúng ta sẽ `commit` và `push` để code được đẩy lên repository nhưng chúng ta cần phải `add` những file thay đổi vào trong stagging area.

Git directory (repository): là nơi mà chúng ta lưu trữ code. Sau khi commit thì các thay đổi sẽ được lưu lại trên repository. Hiểu đơn giản thì repository là nơi chứa các toàn bộ các thông tin về project. Tất cả các dữ liệu của repository đều được chứa trong file .git

### Pull request
Pull request là hành động mà người code yêu cầu được đẩy code vào nhánh khác (mình nghĩ thông thường là nhánh main). Từ đó mọi người có thể review, chỉnh sửa code một cách thống nhất rồi mới merge(hợp nhất). Thế tại sao phải tạo pull request nhỉ ? Sao không merge luôn vào code chính =.=
* Như nói ở trên, code cần được phải quản lý mội cách chặt chẽ, để đảm bảo được chất lượng của code, tránh các lỗi và xung đột trong quá trình phát triển.
* Giúp quá trình kiểm tra dễ dàng hơn và chấp nhận các code.
Vậy khi nào chúng ta cần thực hiện pull request?
Khi làm việc nhóm, công việc sẽ chia thành những task nhỏ vì thế mỗi khi muốn hợp nhất từ nhánh phụ vào nhánh chính thì ta cần phải review code để tránh ảnh hưởng đến code hiện tại.

### Branch
Thông thường trong các project của chúng ta luôn có một nhánh code chính đó là nhánh main. Vậy tại sao phải chia ra nhánh code để làm gì? Tại sao không gộp chung hết vào một nhánh?
Để giải đáp các câu hỏi này thì trước tiên chúng ta tìm hiểu branch là gì? Như tên gọi của nó thì branch là nhánh của repository, mỗi nhánh sẽ tương tự như một không gian làm việc độc lập phát triển mà không làm ảnh hưởng tới các nhánh khác. Sau khi các nhánh được hoàn thành thì chúng ta có thể pull request để team có thể review code (chứ đừng ngáo như mình pull request rồi tự merge luôn) sau đó nếu code ok thì có thể merge. Do nhánh hoàn toàn độc lập với nhau nên việc tạo nhành để dự án chia thành nhiều phần nhỏ là phương án tốt nhất để quản lý dự án.

## Github là công cụ dùng để quản lý mã nguồn. Thế chúng ta làm quản lý mã nguồn thế nào? Git đã cung cấp cho chúng ta khá nhiều câu lệnh để có thể dễ dàng thao tác hơn. Nhưng ở đây chúng ta sẽ nói về một số câu lệnh cơ bản của git.
## Một số lệnh và khái niệm cơ bản về github
### Git add
`git add` dùng sẽ lưu lại nhanh những thay đổi cập nhật trong thư mục, sẫn sàng để commit lên repo.
Thông thường chúng ta sẽ sử dụng câu lệnh là `git add .` Dấu chấm ở đây là chúng ta sẽ thêm tất cả các file git thay đổi.
Tương tự với `git add .`, `git add *` thêm tất cả các file git thay đổi trừ các tệp có `tên bắt đầu bằng dấu chấm`.
Vậy git add thì dùng khi nào?
Khi có sự thay đổi code hoặc thêm, xóa file trong folder chứa .git và ta muốn commit, push code lên thì cần phải thêm những thay đổi đó.

### Git commit
`git commit` là hành động lưu lại một bản sao của các file mà ta vừa sử dụng `git add`. Và như chúng ta nhắc ở trên, sau khi commit, thì các thư mục sẽ nằm trong vùng Stagin Area. Mỗi lần commit sẽ lưu lại lịch sử kèm theo message và tên của người chỉnh sửa nó. 
Câu lệnh: 
``` sh
git commit -m "message"
```
-m ở đây là viết tắt của từ message

### Git push
Sau khi các thay đổi được lưu thì đây là lúc code được đẩy lên repo của github. Lệnh git push sẽ giúp chúng ta đẩy các thay đổi lên nhánh mong muốn.
```sh
git push origin <branch>
```

### Git chechout
`git checkout` sử dụng để làm việc qua lại giũa các nhánh.Ở trên chúng ta đã nói qua khái niệm về `branch` và lý do vì sao chúng ta phải tạo và sử dụng branch. Vậy cách tạo branch như thế nào?
Để tạo branch thì chúng ta sử dụng lên
```sh
git checkout -b <name_branch>
```
Câu lệnh này giúp chúng ta tạo branch và chuyển sang nhánh đó.

Nếu muốn thay đổi nhánh qua lại với nhau ta sử dụng câu lệnh:
```sh
git checkout <name_branch>
```

### git merge
Dùng để hợp nhất nhánh phụ vào nhánh chính hoặc bất kỳ nhánh nào khác. Nó sẽ tự động kết hợp lịch sử commit từ nhánh nguồn vào nhánh chính và tạo ra commit mới.
Để sử dụng `git merge` thì trước tiên ta cần phải vào nhánh chính
```sh
git checkout master
```
Sau đó chúng ta bắt đầu thực hiện merge branch vào master
```sh
git merge <name_branch>
```


### Git pull
`Git pull` giúp chúng ta kéo source code từ github về và đồng thời hợp nhất nó. Câu lệnh này gồm 2 lệnh `git fetch` để tải code từ repo được chỉ định, sau đó sử dụng `git merge` để hợp nhất với code từ repo và code trên local.
 ```sh
 git pull origin <name_branch>
 ```
### Git conflict
`Git conflict` là gì ? Như tên của nó conflict là xung đột. Hiện tượng này xảy ra khi mà trong team có 2 hoặc nhiều người thay đổi cùng một tệp. Các xung đột này có thể xuất hiện tại local hoặc trên kho lưu trữ cục bộ do code không thể hợp nhất. Vậy làm cách nào để khắc phục tình trạng này? Và phương án hạn chế việc xung đột này?
Theo mình được biết, trước hết chúng ta cần phải lấy code từ trên github về bằng lệnh `git pull origin master`. Sau đó sẽ có thông báo lỗi từ git:
```sh
CONFLICT (content): Merge conflict in <name_file_conflict>
```
Khi này trong file conflict sẽ gồm 3 phần:
* `<<<<<<< HEAD là nội dung trên local`
* `======= ngăn cách 2 nội dung bị conflict`
* `>>>>>>> .... là nội dung trên git`

Chỉnh sửa nội dung của code gây xung đột, sau đó chúng ta thực hiện add và commit nó lên.

### Gitflow workflow
Gitflow workflow là một quy chuẩn trong quá trình làm việc, cách mà chúng ta xây dựng các branch khác nhau và cách thức để marge chúng lại với nhau.
Đây là hình ảnh mô tả quy trình hoạt động với các nhánh
![image](https://github.com/Phu-Vu/learn-git/blob/main/git%20workflow.png)

Giải thích về mỗi nhánh:
* Master: là nhánh chính, có sẵn trong git, nó thể hiện các version của project. Và chúng ta không nên sửa đổi file trực tiếp trên master. Để sửa đổi chúng ta sẽ tạo ra các nhánh sau đó push tất cả các code vào nhánh master.
* Develop: là nhánh phát triển các tính năng của dự án, được tạo từ nhánh master ngay từ ban đầu. Từ nhánh develop, chúng ta tiếp tục tạo ra các nhánh nhỏ khác( trong hình là nhánh feature branches) để chia nhỏ xây dựng dự án.
* Feature branches: là nhánh nhỏ được chia ra từ nhánh develop, mỗi nhánh mang một chức năng riêng và sẽ hợp nhất về nhánh develop.
* Release branches: Code của nhánh develop sẽ đẩy lên nhánh release và sau cuối cùng nhánh này sẽ đẩy lên cho master để tạo ra sản phẩm.
* Hotfixes: 



## Tài liệu tham khảo:
https://en.wikipedia.org/wiki/Secure_Shell
https://viblo.asia/p/git-dung-https-hay-ssh-eW65Gm9jZDO

trạng thái của repo https://viblo.asia/p/git-overview-oOVlYq3Bl8W
git workflow: https://viblo.asia/p/co-ban-ve-gitflow-workflow-4dbZNn6yZYM

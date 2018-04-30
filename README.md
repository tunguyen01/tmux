# TMUX
# [Tmux là gì?](#p1)
# [Cài đặt tmux](#p2)
# [Bắt đầu sử dụng](#p3)
# [Liên quan đến tạo session và attach](#p4)
# [Các hành động bên trong tmux](#p5)
# [Tái truy cập session](#p6)
# [Tùy biến tmax](#p7)
# [Một số tiện ích khác](#p8)

<a name="p1"></a>
## Tmux là gì?

Tmux - Một phần mềm nho nhỏ nhưng cực kỳ hữu ích. Lợi ích lớn nhất mà tmux đem lại là cho phép bạn tái truy cập lại một session sau khi kết nối bị mất. Giả sử tình huống như sau: Bạn đang thực hiện một long running action như import file sql có dung lượng lớn, rsync, scp data... Và đột nhiên kết nối ssh bị mất do network nhà bạn gặp trục trặc. Tiến trình import, rsync, scp vẫn hoạt động nhưng bạn không thể thấy nó đang diễn ra như thế nào. Làm cách nào để tái truy cập session đã mất. Tmux chính là thiên thần cứu rỗi bạn trong tình huống đó. Xin bạn chú ý, thiên thần này chỉ giúp bạn nếu bạn nhờ cậy trước khi thực hiện long running action.

<a name="p2"></a>
## Cài đặt tmux như thế nào?

Để cài đặt Tmux đơn giản chỉ cần gõ lệnh cài đặt bình thường như đối với các gói phần mềm bình thường khác

```
sudo apt-get update
sudo apt-get install tmux
```

<a name="p3"></a>
## Bắt đầu sử dụng

Để sử dụng ```tmux``` đơn giản chỉ cần bật Terminal và gõ ```tmux```. Sau khi đã khởi động tmux bạn sẽ nhận ra bên dưới Terminal sẽ xuất hiện một dòng trạng thái màu xanh lá cây bên dưới. Nó hiển thị thông tin thời gian, tên host, [0] là tên ```tmux session```, 0:bash* là tên cửa sổ hiện tại.

<image src="https://imgur.com/abOYpRv"></image>

<a name="p4"></a>
## Liên quan đến tạo session và attach

Tạo một session
```
tmux
```
Tạo một session kèm theo tên gọi
```
tmux new -s [s_name]
```
Hiển thị danh sách các session
```
tmux ls
```
Xóa một session
```
tmux kill-session -t [s_name]
```

<a name="p5"></a>
## Các hành động bên trong tmux

Mặc định của tmux, prefix của các lệnh sẽ là tổ hợp **Ctrl+b**, cũng giống như gõ **Esc** để chuyển về chế độ dòng lệnh như Vim vậy.

**Các lệnh làm việc với cửa sổ**

Tạo một cửa sổ mới
```
[Ctrl+b] c
```
Xem danh sách các cửa sổ hiện tại
```
[Ctrl+b] w                                                        
```
Chuyển đến cửa sổ tiếp theo
```
[Ctrl+b] n
```
Chuyển đến cửa sổ phía trước
```
[Ctrl+b] p
```
Liệt kê danh sách các cửa sổ
```
[Ctrl+b] w
```
Khi liệt kê ra danh sách các cửa sổ sẽ hiển thị số thứ tự của cửa sổ, muốn chảy đến một cửa sổ mong muốn có thể gõ lệnh ```[Ctrl+b] [số thứ tự cửa sổ]```

Đổi tên cửa sổ
```
[Ctrl+b] ,
```
Tìm cửa sổ theo tên
```
[Ctrl+b] f
```
Đóng cửa sổ
```
[Ctrl+b] &
```

**Các lệnh làm việc với panel trong một cửa sổ**

Chia đổi màn hình theo chiều dọc
```
[Ctrl+b] %
```
Chia đôi màn hình làm việc theo chiều ngang
```
[Ctrl+b] "
```
Di chuyển giữa các panel
```
[Ctrl+b] o
hoặc
[Ctrl+b] [phím mũi tên]
```
Hiển thị số thứ tự của panel
```
[Ctrl+b] q
```
Từ câu lệnh hiển thị này ta cũng có thể chuyển nhanh đến panel muốn làm việc. Khi gõ lệnh ```[Ctrl+b] q``` thì trên Terminal sẽ hiển thị nhanh số thứ tự của panel, trong lúc màn hình vẫn đang hiển thị số thứ tự của panel ta ấn nhanh số thứ tự của panel muốn sử dụng thì con trỏ sẽ nhảy thẳng đến panel. Lệnh ```[Ctrl+b] o``` hoặc ```[Ctrl+b] [phím mũi tên]``` sẽ chỉ chuyển lần lượt từ panel đầu đến cuối.

Xóa panel
```
[Ctrl+b] x
```
Chú ý: Khi xóa panel thì chỉ xóa panel tại vị trí con trỏ. Nhưng nếu sử dụng lệnh ```[Ctrl+b] &``` thì tất cả panel của cửa sổ đều sẽ bị xóa.

<a name="p6"></a>
## Tái truy cập tmux session

Đây là phần hay nhất của tmux. Tất cả các panel, window bạn tạo ra trong nằm trong tmux session. Khi thao tác từ xa ở trên server của bạn bỗng nhiên bị mất kết nối network hoặc bạn nhỡ tay đóng terminal (lại đồng nghĩa với viêc detach session), tmux session vẫn nguyên vẹn. Để chạy lại tmux session bạn kiểm tra các session đã được tạo bằng lệnh:
```
tmux ls
```
Như đối với mình kết quả được như sau:
```
tunguyen@Artful:~$ tmux ls
hello: 3 windows (created Mon Apr 30 21:16:46 2018) [80x23]
```

``hello`` ở đây chính là tên của session mình đã đặt lúc khởi tạo tmux sesion. 3 là số cửa sổ chạy trong tmux session này.
Để tái truy cập tmux session ta chạy lệnh:
```
tmux a -t [tên tmux session hoặc id của tmux session]
```

<a name="p7"></a>
## Tùy biến tmux

Bản thân tôi hài lòng với những thiết lập mặc định của Tmux. Tuy nhiên nếu bạn khó tính, ví dụ cảm thấy prefix ``Ctrl+b`` rất là khó dùng chẳng hạn, bạn cũng có thể thay đổi nó. Tạo file .tmux.conf tại thư mục home của user của bạn và bạn có thể config tmux theo ý mình.
```
tmux source-file .tmux.conf
```
Ví dụ bạn thích ``Ctrl+c`` thay vì ``Ctrl+b``. Chỉ cần viết vào file .tmux.conf mới tạo
```
unbind C-b
set -g prefix C-a
```

<a name="p8"></a>
## Một số tiện ích khác

Truy cập vào chế độ command của tmux
```
[Ctrl+b] :
```
Trong chế độ command bạn có thể đổi tên tmux session. Để đổi tên session thì sau khi vào chế độ command của tmux ta nhập lệnh:
```
rename-session [tên session]
```

Ví dụ như đối với lúc nãy mình có một session tên là ``hello`` bây giờ mình sẽ tái truy cập lại và đổi tên.
```
tunguyen@Artful:~$ tmux ls
hello: 3 windows (created Mon Apr 30 21:16:46 2018) [80x23]
```
Sau khi mình đổi tên thành niceday
```
tunguyen@Artful:~$ tmux ls
niceday: 3 windows (created Mon Apr 30 21:16:46 2018) [80x23]
```
Tmux cũng thiết kế một big clock ngầm, để hiển thị big clock này nhập lệnh:
```
[Ctrl+b] t
```

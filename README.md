# GitFlow
[I. Qui trình làm việc](#workflow)
- [1.1 Flow hiện tại](#current-flow)
- [1.1 Flow tham khảo](#new-flow)
	- [1.2.1 Các nhánh](#major-branch)
	- [1.2.2 Mở rộng](#extend-flow)
	
[II. Các lệnh git](#git-command)	

<a name="workflow"></a>
## [I. Qui trình làm việc](#)

<a name="current-flow"></a>
### [1.1 Flow hiện tại](#)
Branch chính: master, develop (protected branch)
Feature branch được tách từ branch develop. Sau khi hoàn thành thì sẽ merge lại develop. Ở dev test okie thì sẽ merge lên master (lên product).
- Ưu điểm: nhanh chóng.
- Nhược điểm: Khi mà các commit trong feature branch (bao gồm các tính năng thêm sửa xóa, fixbug hay lưu doc) . Khi có bug liên quan đến 1 trong những tính năng mún kiểm tra đã thay đổi những gì so với lần đã deploy trước.
- Không phát triển song song được thêm nhiều tính năng hoặc có thể triển khai nhưng bị nhầm lẫn.
- Thiếu bước ghi nhận khi đưa lên product 

<a name="new-flow"></a>
### [1.1 Flow tham khảo](#)

<a name="major-branch"></a>
#### [1.2.1 Các nhánh](#)
![](https://user-images.githubusercontent.com/45549079/74509362-dc874380-4f33-11ea-85e5-77847fe8dab2.png)


**Branch chính**:
- master: chứa source product luôn sẵn sàng để release
- release: môi trường stagging giống với product
- develop: chứa source đang phát triển  cho lần release tiếp theo (mới nhất nhưng không phải cuối cùng)

**Branch hỗ trợ**
- Ngoài các nhánh chính trên có thêm các nhánh để dễ dàng theo dõi các tính năng, chuẩn bị phát hành sản xuất và hỗ trợ khắc phục nhanh các sự cố sản xuất trực tiếp. Không giống như các nhánh chính, các nhánh này luôn sẽ được loại bỏ
- Tất nhiên các nhánh muốn thực hiện đúng chức năng của mình thì đều có các ràng buộc

**Các feature branch**
- Được tách từ develop
- Được merge trở lại vào develop
- Quy ước đặt tên: feature-*

**Các version branch**
- Được tách từ develop
- Được merge vào release public tag và merge trở lại vào develop khi bugfix-dev.
- Quy ước đặt tên: vx.x.x (semantic version)

**Các hotfix branch**
- Được tách từ tag release
- Được merge vào release để test trên môi trường product. Sau đó merge dev để chạy thử cho lần release tiếp theo. 
- Quy ước đặt tên: hotfix-

<a name="extend-flow"></a>
#### [1.2.2 Mở rộng](#)

**1.Đặt tên các feature branch theo [trello](https://trello.com/) để dễ dàng theo dõi**

     <type>-<issue id>-<tên issue>

Ví dụ: feature-ndilNVVX-manage-video với:
- feature: Quy tắc đặt tên các nhánh (tương tự với v1.0.0, bugfix, hotfix)
- ndilNVVX: Thêm sau đường dẫn [https://trello.com/c/ndilNVVX](https://trello.com/c/ndilNVVX)
- manage-video: tên chức năng 

**2.Merge request:**  [merge request](https://codetot.net/merge-request-gitlab/)
- Feature branch sẽ được merge vào dev.
- Hotfix branch sẽ được tách từ tag release và merge vào release và dev
	- Có thể release mà không bao gồm toàn bộ nhánh develop
- 
```
    git checkout tags/<tag_name> -b <branch_name>
```
Khi accept merge request (có thể giữ lại hoặc delete feature branch) nhưng nên giữ lại branch khi chưa đánh tag release để thuận tiện cho việc fix bug. Nếu không có branch thì phải clone từ commit merge.

	- Merge theo --no-ff

<a name="git-command"></a>
## [II. Các lệnh git](#)

1. Nếu lỡ đang làm việc dang dở (chưa muốn commit) mà phải work hay fix bug bên branch khác thì có thể sử dụng lệnh **git stash**
- Lưu thay đổi **chưa commit** và có thể làm **bao nhiêu lần tùy thích**
	- git stash save (hay git stash)

- Lấy danh sách thay đổi các lần lưu
	- git stash list
	- git stash list -p (nội dung của từng thay đổi)
	- git stash show stash@{1} (xem nội dung cụ thể của lần thay đổi 1)
	- **git stash apply stash@{1} (apply lại thay đổi từ stash lần 1)**

Xóa các thay đổi không cần thiết |Cách khác
-------------------------|-----------------
git stash apply stash@{1}| git stash pop stash@{1}
git stash drop stash@{1} |  

- Xóa toàn bộ trong stack
	- git stash clear

2. Git merge:

![fast forward merge](https://bitbucket.org/blog/wp-content/uploads/2018/06/what-is-a-fast-forward.gif)

3. Git rebase:

![](https://i0.wp.com/dotrinh.com/wp-content/uploads/2018/03/git-rebase-la-gi.gif?fit=930%2C523&ssl=1)

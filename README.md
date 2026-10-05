# Bài 3: Xử lý xung đột phức tạp trong quá trình Rebase

## 1. Mục tiêu

- Hiểu cách xử lý conflict khi sử dụng `git rebase`.
- Thực hành giải quyết conflict thủ công trong quá trình rebase.
- Đưa nhánh `feature-api` lên trên nhánh `main`.
- Tạo lịch sử Git tuyến tính, không có merge commit.
- Đảm bảo các thay đổi của cả `main` và `feature-api` được giữ lại.

---

## 2. Khởi tạo repository

Khởi tạo Git repository:

```bash
git init

Tạo file config.json:

{
  "port": 8080,
  "debug": false
}

Commit đầu tiên:

git add config.json
git commit -m "init config"
3. Tạo nhánh feature-api

Tạo nhánh:

git checkout -b feature-api
Commit 1 của feature-api

Thay đổi port từ 8080 thành 9000:

{
  "port": 9000,
  "debug": false
}

Commit:

git add config.json
git commit -m "feat: change port"
Commit 2 của feature-api

Thay đổi debug thành true:

{
  "port": 9000,
  "debug": true
}

Commit:

git add config.json
git commit -m "feat: enable debug"
4. Tạo thay đổi trên nhánh main

Chuyển về main:

git checkout main
Commit 1 trên main

Thay đổi port từ 8080 thành 8081:

{
  "port": 8081,
  "debug": false
}

Commit:

git add config.json
git commit -m "update port on main"
Commit 2 trên main

Thêm môi trường production:

{
  "port": 8081,
  "debug": false,
  "env": "production"
}

Commit:

git add config.json
git commit -m "add env config"
5. Kiểm tra lịch sử trước khi Rebase

Lịch sử trước khi rebase:

* 8c0ebce (HEAD -> main) add env config
* d741e86 update port on main
| * f1095fd (feature-api) feat: enable debug
| * 970c6aa feat: change port
|/
* 84c9949 init config

Có thể kiểm tra bằng:

git log --graph --oneline
6. Thực hiện Rebase

Chuyển sang nhánh feature-api:

git checkout feature-api

Kiểm tra branch:

git branch

Kết quả:

* feature-api
  main

Thực hiện rebase:

git rebase main
7. Xử lý Conflict lần 1

Git báo conflict tại:

CONFLICT (content): Merge conflict in config.json

Kiểm tra trạng thái:

git status

Mở file:

notepad config.json

Sau khi resolve conflict, giữ lại:

port của feature-api: 9000
debug: false
env của main: production

Nội dung sau khi xử lý:

{
  "port": 9000,
  "debug": false,
  "env": "production"
}

Đánh dấu conflict đã được giải quyết:

git add config.json

Tiếp tục rebase:

git rebase --continue

Commit feat: change port được áp dụng lên trên main.

8. Xử lý Conflict lần 2

Git tiếp tục áp dụng commit:

feat: enable debug

và xuất hiện conflict tại config.json.

Mở file:

notepad config.json

Resolve conflict thủ công và giữ lại toàn bộ thay đổi cần thiết:

{
  "port": 9000,
  "debug": true,
  "env": "production"
}

Sau đó:

git add config.json
git rebase --continue

Khi Git yêu cầu xác nhận commit message trong Vim:

Esc
:wq
Enter
9. Kiểm tra sau khi Rebase

Kiểm tra trạng thái:

git status

Kết quả:

On branch feature-api
nothing to commit, working tree clean

Kiểm tra lịch sử:

git log --graph --oneline

Lịch sử sau rebase phải có dạng tuyến tính:

* <hash> (HEAD -> feature-api) feat: enable debug
* <hash> feat: change port
* 8c0ebce (main) add env config
* d741e86 update port on main
* 84c9949 init config

Không xuất hiện merge commit.

10. Nội dung cuối cùng của config.json
{
  "port": 9000,
  "debug": true,
  "env": "production"
}
11. Kết luận

Qua bài thực hành đã thực hiện:

Tạo lịch sử commit trên hai nhánh main và feature-api.
Tạo xung đột trong quá trình git rebase.
Xử lý thủ công conflict trong config.json.
Sử dụng git add và git rebase --continue để tiếp tục rebase.
Hoàn thành rebase thành công.
Đưa các commit của feature-api lên trên các commit mới nhất của main.
Tạo được lịch sử Git tuyến tính, không có merge commit.
```

# Bài 4 — .gitignore và git commit --amend

## Bối cảnh

Đã commit nhầm `credentials.txt`. Cần:
1. Gỡ file khỏi Git **không xóa** file trên đĩa
2. Thêm vào `.gitignore` để không theo dõi nữa
3. Sửa message commit gần nhất bằng `--amend`

## Các bước đã thực hiện

### 1. Gỡ khỏi cache, giữ file vật lý

```bash
git rm --cached credentials.txt
```

- `--cached`: chỉ bỏ tracking, file vẫn nằm trong thư mục làm việc.

### 2. Bỏ qua về sau

Tạo / cập nhật `.gitignore`:

```text
credentials.txt
```

```bash
git add .gitignore
```

### 3. Commit (hoặc amend commit gần nhất)

Nếu commit nhầm **chưa push** và muốn gộp vào commit cuối:

```bash
git commit --amend -m "chore: remove credentials from tracking and add gitignore"
```

Nếu đã có commit riêng cho việc gỡ file:

```bash
git commit -m "chore: stop tracking credentials.txt"
# rồi amend nếu chỉ muốn sửa message:
git commit --amend -m "chore: remove credentials from tracking and add gitignore"
```

## Kiểm tra

```bash
git status
# credentials.txt không còn staged/modified; bị ignore

git log -n 1
# message đã sửa, ví dụ:
# chore: remove credentials from tracking and add gitignore
```

Ví dụ kết quả `git log -n 1`:

```text
commit <hash>
Author: ...
Date:   ...

    chore: remove credentials from tracking and add gitignore
```

## Lưu ý

- `git rm --cached` ≠ `rm`: không xóa file trên ổ đĩa.
- `--amend` chỉ an toàn với commit **chưa share / chưa push** (hoặc force-push có chủ đích).
- File secret đã từng commit: nên đổi mật khẩu/token thật; xóa khỏi history sâu hơn nếu đã push public.

# Vim Cheat Sheet

## Nguồn

[Vim Cheat Sheet](https://vim.rtorr.com/)

## Mở đầu

Vim (hay tên đầy đủ là VI Improved) là trình soạn thảo văn bản dòng lệnh được sử dụng để tạo và xem các file văn bản.

Bạn có thể học Vim qua VimTutor (với lệnh `vimtutor`) trong các hệ thống UNIX (như Linux hay MacOS).

## Cheat Sheet

### Global

- `:h[elp] keyword`: tìm kiếm keyword trong tài liệu trợ giúp
- `:sav[eas] file`: lưu file
- `:clo[se]`: đóng file
- `:ter[minal]`: mở terminal
- `K`: hiển thị tài liệu trợ giúp cho từ hiện tại

### Di chuyển con trỏ

- `h`: di chuyển con trỏ sang trái 1 ký tự
- `j`: di chuyển con trỏ lên trên 1 dòng
- `k`: di chuyển con trỏ xuống dưới 1 dòng
- `l`: di chuyển con trỏ sang phải 1 ký tự
- `H` - Head: di chuyển con trỏ lên đầu màn hình
- `M` - Middle: di chuyển con trỏ sang giữa màn hình
- `L`: di chuyển con trỏ xuống cuối màn hình
- `w`: di chuyển con trỏ sang đầu từ tiếp theo
- `W`: di chuyển con trỏ sang đầu từ tiếp theo (từ ở đây chứa cả các ký tự đặc biệt)
- `e`: di chuyển con trỏ về cuối từ hiện tại, nếu đã ở cuối từ hiện tại thì về cuối từ tiếp theo
- `E`: di chuyển con trỏ về cuối từ hiện tại, nếu đã ở cuối từ hiện tại thì về cuối từ tiếp theo (từ ở đây chứa cả các ký tự đặc biệt)
- `b`: di chuyển con trỏ về đầu từ hiện tại, nếu đã ở đầu từ hiện tại thì về đầu từ trước đó
- `B`: di chuyển con trỏ về đầu từ hiện tại, nếu đã ở đầu từ hiện tại thì về đầu từ trước đó (từ ở đây chứa cả các ký tự đặc biệt)
- `%`: di chuyển con trỏ đến cặp ngoặc đóng mở tương ứng (cặp ngoặc tròn, ngoặc nhọn, ngoặc vuông)
- `0`: di chuyển con trỏ về đầu dòng
- `^`: di chuyển con trỏ về đầu dòng (không tính khoảng trắng ở đầu dòng)
- `$`: di chuyển con trỏ về cuối dòng
- `g_`: di chuyển con trỏ về cuối dòng (không tính khoảng trắng ở cuối dòng)
- `gg`: di chuyển con trỏ về đầu file
- `G`: di chuyển con trỏ về cuối file
- `5gg` hoặc `5G`: di chuyển con trỏ về dòng thứ 5
- `gd`: di chuyển con trỏ đến định nghĩa biến cục bộ (local declaration) đầu tiên của từ hiện tại 
- `gD`: di chuyển con trỏ đến định nghĩa biến toàn cục (global declaration) đầu tiên của từ hiện tại
- `fx`: di chuyển con trỏ đến ký tự `x` tiếp theo trong dòng
- `tx`: di chuyển con trỏ đến ký tự `x` tiếp theo trong dòng, nhưng con trỏ sẽ dừng trước ký tự `x`
- `Fx`: di chuyển con trỏ đến ký tự `x` trước đó trong dòng
- `Tx`: di chuyển con trỏ đến ký tự `x` trước đó trong dòng, nhưng con trỏ sẽ dừng sau ký tự `x`
- `;`: lặp lại lệnh `f`, `t`, `F`, `T` cuối cùng
- `,`: lặp lại lệnh `f`, `t`, `F`, `T` cuối cùng, nhưng ngược chiều
- `}`: di chuyển con trỏ đến đầu đoạn văn bản tiếp theo
- `{`: di chuyển con trỏ đến đầu đoạn văn bản trước đó
- `zz`: đưa dòng hiện tại vào giữa màn hình
- `zt`: đưa dòng hiện tại về đầu màn hình
- `zb`: đưa dòng hiện tại về cuối màn hình
- `Ctrl` + `e`: cuộn màn hình xuống 1 dòng (không di chuyển con trỏ)
- `Ctrl` + `y`: cuộn màn hình lên 1 dòng (không di chuyển con trỏ)
- `Ctrl` + `b`: cuộn màn hình lên 1 màn hình (con trỏ về dòng cuối cùng)
- `Ctrl` + `f`: cuộn màn hình xuống 1 màn hình (con trỏ về dòng đầu tiên)
- `Ctrl` + `d`: cuộn màn hình và di chuyển con trỏ xuống 1 nửa màn hình
- `Ctrl` + `u`: cuộn màn hình và di chuyển con trỏ lên 1 nửa màn hình

**Tips**: Thêm số vào trước lệnh để lặp lại lệnh đó nhiều lần. Ví dụ: `4j` sẽ di chuyển con trỏ xuống 4 dòng.

### Chế độ chèn - Insert mode

- `i`: chuyển sang chế độ chèn ở vị trí con trỏ
- `I`: chuyển sang chế độ chèn ở đầu dòng
- `a`: chuyển sang chế độ chèn ở vị trí con trỏ, nhưng con trỏ sẽ dịch chuyển sang phải 1 ký tự
- `A`: chuyển sang chế độ chèn ở cuối dòng
- `o`: tạo 1 dòng mới phía dưới và chuyển sang chế độ chèn
- `O`: tạo 1 dòng mới phía trên và chuyển sang chế độ chèn
- `ea`: chuyển sang chế độ chèn ở cuối từ hiện tại
- `Ctrl` + `h`: xóa ký tự bên trái con trỏ khi ở chế độ chèn
- `Ctrl` + `w`: xóa từ bên trái con trỏ khi ở chế độ chèn
- `Ctrl` + `j`: thêm line break (xuống dòng) tại vị trí con trỏ khi ở chế độ chèn
- `Ctrl` + `t`: thêm tab vào dòng hiện tại khi ở chế độ chèn
- `Ctrl` + `d`: xóa tab khỏi dòng hiện tại khi ở chế độ chèn
- `Ctrl` + `n`: tìm kiếm từ hiện tại trong auto-complete và chọn từ tiếp theo
- `Ctrl` + `p`: tìm kiếm từ hiện tại trong auto-complete và chọn từ trước đó
- `Ctrl` + `rx`: chèn kết quả của thanh ghi `x` vào vị trí con trỏ
- `Ctrl` + `ox`: tạm thoát chế độ chèn và chuyển sang chế độ normal để thực hiện lệnh `x` (lệnh này chỉ có hiệu lực 1 lần)
- `Esc` hoặc `Ctrl` + `c`: thoát chế độ chèn

### Chỉnh sửa văn bản

Các lệnh ở đây được dùng khi đang ở chế độ normal

- `rx`: thay thế ký tự hiện tại bằng ký tự `x`
- `R`: thay thế nhiều ký tự bằng các ký tự nhập vào, cho đến khi nhấn `Esc`
- `J`: nối dòng hiện tại với dòng dưới, thêm khoảng trắng vào giữa
- `gJ`: nối dòng hiện tại với dòng dưới, nhưng không thêm khoảng trắng

### Đánh dấu văn bản - Visual mode

### Các lệnh visual

### Các thanh ghi - Registers

- `:reg[isters]`: hiển thị các thanh ghi
- `"xy`: sao chép (yank) văn bản vào thanh ghi `x`
- `"xp`: dán (paste) nội dung của thanh ghi `x` vào vị trí con trỏ
- `"+y`: sao chép (yank) văn bản vào thanh ghi clipboard
- `"+p`: dán (paste) nội dung của thanh ghi clipboard vào vị trí con trỏ

**Tips**: Các thanh ghi được lưu trong `~/.viminfo`, và sẽ được load lại trong lần khởi động vim tiếp theo.

**Tips**: Các thanh ghi đặc biệt

- `0`: lần sao chép cuối cùng
- `"`: thanh ghi không tên, lần xoá hoặc sao chép cuối cùng
- `%`: tên của file hiện tại bạn đang truy cập
- `#`: tên của file cuối cùng bạn truy cập trong cùng cửa sổ vim
- `*`: 

### Đánh dấu và vị trí

### Các Macro

### Cut và paste

### Indent văn bản

### Thoát

- `:w`: lưu file, nhưng không thoát
- `:w !sudo tee %`: lưu file với quyền root
- `:wq` hoặc `:x` hoặc `ZZ`: lưu file và thoát
- `:q`: thoát (nếu không có thay đổi)
- `:q!` hoặc `ZQ`: thoát và bỏ qua các thay đổi
- `:wqa`: lưu tất cả các file và thoát

### Tìm kiếm và thay thế

### Tìm kiếm trong nhiều file

### Tab

### Làm việc với nhiều file

### Diff

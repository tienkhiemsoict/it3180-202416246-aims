**Lab 01**
- Vì sao dùng git mv? 
Để Git theo dõi và ghi nhận việc đổi tên/di chuyển file, giúp giữ lại toàn bộ lịch sử commit của file đó trước khi bị đổi tên.

- release/lab01 có phải baseline?
Có, đây là một trạng thái mã nguồn đã được đóng băng và thống nhất để nộp bài.

- Vì sao không commit thẳng master dù làm một mình?
Để tuân thủ quy trình kiểm soát thay đổi (Change Control), rèn luyện thói quen tạo Pull Request, review code và để các cấu hình Branch Protection (ràng buộc CI/CD) có thể hoạt động.

**Lab 02**
- Vì sao phải đồng bộ master trước khi gửi PR?
Để đảm bảo nhánh tính năng của bạn tương thích với những mã nguồn mới nhất trên master, giúp phát hiện và xử lý xung đột (conflict) ở máy cá nhân trước khi đưa lên môi trường chung.

- Khi nào không được rebase?
Không được dùng rebase trên các nhánh đã được push lên remote và có người khác đang cùng làm việc (ví dụ: master, release), vì rebase viết lại lịch sử commit, gây mất đồng bộ nghiêm trọng cho người khác.

- feature/* khác topic/* ra sao?
`feature/*` dùng để phát triển các tính năng trực tiếp của sản phẩm/phần mềm. `topic/*` dùng cho các chủ đề công việc khác không thuộc mã nguồn chính như viết tài liệu, tạo script, sửa cấu hình dự án.

**Lab 03**
- Merge base là gì?
Là commit tổ tiên chung gần nhất của hai nhánh đang chuẩn bị được merge lại với nhau.

- Vì sao master/release/* phải dùng revert?
Vì đây là các nhánh chia sẻ, có tính lịch sử cố định. Lệnh revert tạo ra một commit mới để đảo ngược thay đổi cũ thay vì xóa lịch sử, giúp bảo toàn tính toàn vẹn của nhánh chung để không ảnh hưởng máy người khác.

**Lab 04**
- Vì sao release/* không merge vào master?
Vì nhánh release chỉ dùng để bảo trì (sửa lỗi) cho một phiên bản cụ thể đã phát hành. Các tính năng mới ở master không được phép đưa vào đây, và ngược lại các bản vá từ nhánh release sẽ được chọn lọc đưa về master (thông qua cherry-pick) chứ không gộp toàn bộ nhánh để tránh làm loạn lịch sử.

- So sánh với GitFlow.
Release Flow dùng `master` làm nhánh phát triển chính và rẽ nhánh `release` từ `master` ra. GitFlow có thêm nhánh `develop` làm nhánh chính, còn `master` chỉ chứa code đã lên production. Release Flow đơn giản và phù hợp với CI/CD liên tục hơn.

- Vì sao lên 1.0.0 mà không phải 0.4.0?
Theo Semantic Versioning, version 0.x.x dành cho giai đoạn phát triển ban đầu, chưa ổn định. 1.0.0 đánh dấu phiên bản chính thức đầu tiên, sẵn sàng phát hành cho người dùng cuối.

**Lab 05**
- Ưu, nhược điểm của upstream first so với release first?
* Upstream first (Sửa trên master trước): Ưu điểm là không bao giờ quên cập nhật bản sửa lỗi cho các phiên bản tương lai. Nhược điểm là master có thể đang chứa code chưa ổn định, khó kiểm thử ngay bản fix.
* Release first (Sửa trên nhánh release trước): Ưu điểm là khắc phục lỗi nhanh chóng, an toàn cho phiên bản đang gặp sự cố. Nhược điểm là lập trình viên có thể quên lan truyền (cherry-pick) bản sửa lỗi đó ngược về nhánh master.

- Nếu quên lan truyền bản sửa sang release/lab05, lệnh nào ở Phần F sẽ phát hiện?
Lệnh `git tag --contains <FIX_SHA>` (để xem commit sửa lỗi đã nằm ở những tag/phiên bản nào) hoặc lệnh `git cherry -v` (để so sánh các commit giữa các nhánh).
```eof
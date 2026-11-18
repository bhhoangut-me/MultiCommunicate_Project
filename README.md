# Project1 — Thiết kế PCB trong Altium Designer

Dự án chứa sơ đồ nguyên lý và layout PCB1, cùng cấu hình xuất hồ sơ chế tạo SMARTPCB88.

| Tệp | Nội dung |
| --- | --- |
| `Project1.PrjPcb` | Tệp dự án chính. Mở tệp này ở thư mục gốc trong Altium Designer. |
| `Sheet1.SchDoc`, `Analog.SchDoc` | Sơ đồ chính và tài liệu sơ đồ phần analog. |
| `PCB1.PcbDoc` | Layout bo mạch. |
| `PCB*.PcbLib`, `TesterParts.*Lib` | Thư viện footprint và ký hiệu lưu trong thư mục dự án. |
| `SMARTPCB88_Fabrication.OutJob` | Cấu hình xuất dữ liệu chế tạo và báo cáo. |

## Mở dự án và xuất dữ liệu

1. Mở `Project1.PrjPcb` và kiểm tra các tài liệu trong bảng Projects.
2. Kiểm tra lại liên kết tới `FUSRR505W81L660T250H1360.PcbLib` trong Downloads. Tệp này nằm ngoài kho Git. Dự án cũng tham chiếu tới `Project1.SCHLIB` và `Project1.BomDoc`, hiện thiếu tại các vị trí được cấu hình.
3. Chạy kiểm tra dự án và DRC trong Altium Designer. Dùng `SMARTPCB88_Fabrication.OutJob` để xuất dữ liệu cho phiên bản PCB đang mở.
4. Khi dùng dữ liệu đã xuất, đối chiếu phiên bản bo với gói Gerber và NC Drill tương ứng. Các thư mục `SMARTPCB88_*` đã xuất nằm trong `_archive/`. Các gói ZIP chế tạo nằm ở thư mục gốc.

## Tổ chức thư mục

- `_archive/` chứa lịch sử cục bộ, log, ảnh xem trước, các bản chụp PCB, bản dự án lồng `Project1/` và các thư mục đầu ra đã lưu.
- Một số mục báo cáo trong dự án vẫn trỏ tới đường dẫn đầu ra cũ. Để xem bản đã lưu, mở tệp tương ứng trong `_archive/Project Outputs for Project1/` hoặc `_archive/SMARTPCB88_Fabrication/`. Altium có thể tạo lại thư mục đầu ra khi chạy xuất dữ liệu.
- `.gitignore` loại tệp sinh tự động, kho lưu trữ cục bộ, bản dự án lồng và đầu ra chế tạo khỏi Git.

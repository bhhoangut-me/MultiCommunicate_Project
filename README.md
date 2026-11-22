# ESP32-S3 MULTI-IO

Bo mở rộng I/O cho **ESP32-S3-DevKitC-1-N8R8**, thiết kế bằng Altium Designer. Bo đưa CAN, RS-485, một ngõ ADC, bốn đường GPIO qua điện trở và hai đầu cắm ngoại vi ra ngoài. Bo có sáu nút nhấn, hai LED, nguồn 3,3 V và jumper cấp 5 V cho DevKit. Silkscreen ghi **REV 1.0 | 2026** và **DESIGNED BY B.H.HOANG**. OutJob dùng tên **SMARTPCB88** cho hồ sơ chế tạo.

## Tệp thiết kế

| Tệp | Nội dung |
| --- | --- |
| [Project1.PrjPcb](Project1.PrjPcb) | Cấu hình project và danh sách tài liệu Altium. |
| [Sheet1.SchDoc](Sheet1.SchDoc) | Sơ đồ nguyên lý của bo. Tra chân theo designator trên tài liệu này. |
| [PCB1.PcbDoc](PCB1.PcbDoc) | Layout đã lưu, gồm placement, routing, lỗ gá, silkscreen và quy tắc PCB. |
| [SMARTPCB88_Fabrication.OutJob](SMARTPCB88_Fabrication.OutJob) | Cấu hình xuất dữ liệu chế tạo và báo cáo. |

## Kiến trúc phần cứng

| Khối | Thành phần | Kết nối |
| --- | --- | --- |
| Điều khiển | J6, ESP32-S3-DevKitC-1-N8R8 | DevKit điều khiển transceiver, đọc nút và ADC, giao tiếp với ngoại vi qua các chân GPIO. |
| Nguồn đầu vào | J1 EJ503A, F1 RUEF110, S1 MTS103 | J1 chân 1 qua F1 rồi S1 tới MAIN5V. J1 chân 2 về GND. |
| Nguồn 3,3 V | VR1 LM1117T-3.3/NOPB | MAIN5V vào VR1. Đầu ra 3V3 cấp cho phần ngoại vi của bo. C1 và C2 là tụ 22 µF ở hai phía bộ ổn áp. |
| Cấp nguồn DevKit | J12 | Jumper nối MAIN5V với DEVKIT5V, đi tới chân 5V0 của J6. |
| CAN | U1 MCP2562-E/P, J3, J13, R7 | U1 dùng MAIN5V cho VDD và 3V3 cho VIO. STBY nối GND. J13 đóng nhánh kết thúc 120 Ω R7 giữa CANH và CANL. |
| RS-485 | U2 MAX3485CPA+, J9, J11, R5, R6 | U2 dùng 3V3. GPIO5 đi qua R6 10 kΩ tới nút chung DE và /RE. J11 đóng nhánh kết thúc 120 Ω R5 giữa A và B. |
| Giao diện cục bộ | S2–S7 B3F-1000, D1–D2 HLMP-4740 | Các nút nối tín hiệu BT xuống GND khi nhấn. D1 lấy tín hiệu từ chân 3V3 J1_1 của DevKit. D2 nối GPIO2. |
| Lọc nguồn | C3–C6 C315C104K5R5TA | Bốn tụ gốm 100 nF. C3 và C5 nối MAIN5V–GND, C4 và C6 nối 3V3–GND. |

MAIN5V là tên đường nguồn trong sơ đồ. Từ J1 đến MAIN5V có cầu chì và công tắc; VR1 tạo đường 3V3. Khi dùng USB cho DevKit, kiểm tra trạng thái J12 và cách cấp nguồn trước khi nối thêm nguồn ở J1. Hai LED trên sơ đồ đi trực tiếp tới các chân nêu trên, vì vậy cần kiểm tra giới hạn dòng trước khi lắp hoặc thay loại LED.

## Đầu nối và jumper

Thứ tự trong bảng theo **số chân trong sơ đồ Altium**, không theo hướng nhìn của đầu nối trên bo.

| Designator | Chức năng | Chân |
| --- | --- | --- |
| J1 | Jack nguồn DC EJ503A | 1 → F1, 2 GND, 3 tiếp điểm phụ của jack. |
| J2 | Cọc vít I/O, 4 chân | 1 3V3, 2 GPIO15 qua R3, 3 GPIO7 qua R1, 4 GND. |
| J3 | Cọc vít CAN | 1 CANH, 2 CANL, 3 GND. |
| J4 | Cọc vít GPIO | 1 GPIO10 qua R2, 2 GPIO11 qua R4, 3 GND. |
| J5 | Cọc vít ADC | 1 3V3, 2 ADCIN1 (GPIO1), 3 GND. |
| J6 | ESP32-S3-DevKitC-1-N8R8 | Hai hàng chân J1_x và J3_x. Bảng bên dưới liệt kê các tín hiệu được sử dụng. |
| J7 | Header hiển thị SPI, 8 chân | 1 3V3, 2 GND, 3 CS, 4 RST, 5 DC, 6 SDI, 7 SCK, 8 3V3. |
| J8 | Header ngoại vi UART/SPI/I²C, 6 chân | 1 GND, 2 3V3, 3 GPIO17 (TX/MOSI/SDA), 4 GPIO18 (RX/MISO/SCL), 5 GPIO16 (SCK), 6 GPIO9 (CS). |
| J9 | Cọc vít RS-485 | 1 GND, 2 B, 3 A. |
| J11 | Jumper kết thúc RS-485 | 1 B, 2 tới A qua R5 120 Ω. |
| J12 | Jumper cấp nguồn DevKit | 1 MAIN5V, 2 DEVKIT5V. |
| J13 | Jumper kết thúc CAN | 1 CANH, 2 tới CANL qua R7 120 Ω. |

J8 dùng chung chân vật lý cho các chức năng in trên silkscreen. Firmware phải chọn chế độ tương ứng với thiết bị gắn vào. Trên J7, SDI là tên tín hiệu ghi trong sơ đồ và MOSI là tên in trên PCB. Chỉ đóng jumper kết thúc của bus khi bo cần đặt tải kết thúc tại vị trí lắp đặt.

## Ánh xạ tín hiệu ESP32-S3

| Tín hiệu | Chân DevKit | Tuyến trên bo |
| --- | --- | --- |
| CAN_TX, CAN_RX | GPIO38, GPIO21 | U1 TXD và RXD. |
| RO, Enable, DI | GPIO4, GPIO5, GPIO6 | U2 RO, điều khiển DE và /RE qua R6, U2 DI. |
| ADCIN1 | GPIO1 | J5.2. |
| GP7, GP15 | GPIO7, GPIO15 | J2.3 qua R1, J2.2 qua R3. |
| GP10, GP11 | GPIO10, GPIO11 | J4.1 qua R2, J4.2 qua R4. |
| GP17, GP18, GP16, GP9 | GPIO17, GPIO18, GPIO16, GPIO9 | J8.3–J8.6. |
| CS, RST, DC, SDI, SCK | GPIO39, GPIO40, GPIO47, GPIO41, GPIO42 | J7.3–J7.7. |
| BT1, BT2, BT3 | GPIO12, GPIO13, GPIO14 | S2, S3, S4. |
| BT4 | RST của DevKit | S5 kéo chân reset xuống GND. |
| BT5, BT6 | GPIO0, GPIO46 | S6 và S7. Đây là các chân cần chú ý khi khởi động. |
| LED trạng thái D2 | GPIO2 | Anode D2, cathode về GND. |
| Nguồn và LED D1 | 5V0, 3V3 J1_1 và GND | 5V0 nhận DEVKIT5V qua J12. Chân 3V3 J1_1 nối anode D1, cathode về GND. |

Các nút nối xuống GND khi nhấn. Cấu hình mức nghỉ cho các ngõ vào GPIO trong firmware phù hợp với sơ đồ. Tập tệp hiện tại chỉ chứa thiết kế phần cứng, nên firmware, tốc độ bus, giao thức truyền, chế độ J8 và cách xử lý nút chưa được cung cấp trong thư mục này.

## Linh kiện theo nhóm

| Designator | Linh kiện / giá trị |
| --- | --- |
| J1 | EJ503A, jack DC. |
| J2 | TB002-500-04BE, cọc vít 4 chân. |
| J3, J4, J5, J9 | TB003-500-P03GY, cọc vít 3 chân. |
| J6 | ESP32-S3-DevKitC-1-N8R8. |
| J7 | SSW-108-XX-X-S, đầu cắm 8 chân. |
| J8 | TSW-106-07-G-S, header 6 chân. |
| J11, J12, J13 | TSW-102-07-G-S, header jumper 2 chân. |
| F1, S1, VR1 | RUEF110, MTS103, LM1117T-3.3/NOPB. |
| S2–S7 | B3F-1000. |
| U1, U2 | MCP2562-E/P, MAX3485CPA+. |
| D1, D2 | HLMP-4740. |
| C1, C2 | UVR1C220MDD, 22 µF. |
| C3–C6 | C315C104K5R5TA, 100 nF. |
| R1, R3 | Symbol và comment là Res1. Trường Value của sơ đồ ghi 1K. |
| R2, R4 | Comment trên schematic/PCB ghi 330 Ω. Trường Value của symbol ghi 1K. |
| R5, R7 | Comment trên schematic/PCB ghi 120 Ω. Trường Value của symbol ghi 1K. |
| R6 | Comment ghi 10 kΩ. |

Đối chiếu các trường Comment và Value của điện trở trước khi chốt BOM đặt hàng. Bảng trên ghi theo dữ liệu thiết kế đang có, bao gồm các trường mâu thuẫn.

## PCB và kiểm tra chế tạo

- Biên bo khoảng **100 × 85 mm**, bo góc, có bốn lỗ gá M3 theo ghi chú quy tắc khoan.
- Dữ liệu PCB đã lưu có **38 footprint linh kiện**, **44 net**, **165 pad**, **31 via** và **2 polygon GND**, ở Top và Bottom.
- Quy tắc mặc định ghi chiều rộng đường mạch và clearance 10 mil (0,254 mm). Các nhóm nguồn 5 V, 3,3 V và GND có quy tắc chiều rộng riêng từ 20 đến 60 mil. Quy tắc kích thước lỗ khoan là 0,30–3,20 mm.
- J6 dùng module radio 2,4 GHz. Trước khi làm cơ khí hoặc đổi bố trí, kiểm tra vùng anten, khoảng trống và vật cản quanh module trên PCB thực tế.
- Số liệu trên lấy từ tệp PCB đang lưu. Chạy lại kiểm tra project, ERC, DRC và đối chiếu schematic–PCB trong Altium trước khi phát hành.

## Mở project từ bản clone

1. Mở **Project1.PrjPcb** trong Altium Designer. Mở **Sheet1.SchDoc** và **PCB1.PcbDoc** từ bảng Projects.
2. Kiểm tra và cập nhật các tham chiếu thư viện ngoài thư mục nếu cần quản lý hoặc thay linh kiện. Project còn liệt kê **PCB1.PcbLib**, **PCB11.PcbLib**, **PCB12.PcbLib**, **PCB13.PcbLib**, **Project1.SCHLIB**, thư viện **FUSRR505W81L660T250H1360.PcbLib** và **Project1.BomDoc**. Các tệp này vắng mặt trong thư mục hiện tại. Một số đầu vào và báo cáo tham chiếu tới đường dẫn của máy thiết kế.
3. Validate project và kiểm tra Messages. So sánh schematic với PCB, cập nhật polygon và chạy DRC trong Altium.
4. Mở **SMARTPCB88_Fabrication.OutJob** và cập nhật thư mục xuất. Hai container file đang trỏ tới đường dẫn tuyệt đối dưới **Downloads\Project1\Project1\SMARTPCB88_Fabrication** trên máy thiết kế.
5. OutJob đang bật Gerber, NC Drill, IPC‑2581 và xuất PCB ASCII. Các mục DRC, Footprint Comparison và PCB Prints có cấu hình sẵn nhưng đang tắt. Nếu cần báo cáo, chạy hoặc bật mục tương ứng.
6. Kiểm tra Gerber, NC Drill và biên bo của cùng một lần xuất trước khi gửi sản xuất. Đối chiếu pin 1, phân cực, silkscreen và lỗ gá với PCB. Thư mục nguồn hiện tại không kèm gói đã xuất.

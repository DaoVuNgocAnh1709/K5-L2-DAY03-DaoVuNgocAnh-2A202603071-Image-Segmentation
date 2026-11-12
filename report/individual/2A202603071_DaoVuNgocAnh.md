# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

- Mã học viên theo lớp: 2A202603071
- Họ và tên học viên: Đào Vũ Ngọc Anh
- Ngày / CVAT local: 12/11/2026 / CVAT local v2.74.1 & Nuclio Serverless
- Công cụ đã dùng: Polygon, Brush, CVAT Serverless Segment Anything v3 (ViT-Base)

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh 1 (`000000181542.jpg`), chiếc xe buýt lớn (`bus`) ở phía bên trái đường.
- Class và quy tắc tôi dùng để chọn biên: Class `bus`. Dùng Polygon bo sát đường bao kim loại của thân xe, mép dưới lấy theo vệt tiếp đất của bánh xe, không ăn lấn sang bóng đổ (shadow) dưới gầm; toàn bộ kính cửa sổ giữ trọn trong thân xe.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Em đã triển khai tool **Segment Anything v3** qua Nuclio Serverless trên CVAT local để hỗ trợ tự động gán nhãn cho hơn 60% các xe máy (`motorcycle`) và người đi bộ (`person`). Với các xe ở cự ly gần, SAM bắt biên chính xác >90%; ở một số người đi bộ ở xa, mask AI hơi lem ra nền bóng tối nên em đã dùng Brush Eraser tỉa lại viền cho khít.
- Nếu không dùng gợi ý: Đã dùng Segment Anything v3 kết hợp tinh chỉnh thủ công (Human-in-the-Loop).

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: Task `easy_semantic`, ảnh số 1 (`7ee6d192-89e2408b.jpg`), góc dưới cùng bên trái của mặt đường `road`.
- Lỗi thuộc loại: Biên / Phủ vùng.
- Bằng chứng tôi nhìn thấy: Mép polygon của `road` bị cắt vát quá sớm, bỏ sót một khoảng đường nhựa màu xám đen sát góc viền ảnh.
- Quy tắc và hành động sửa: Quy tắc yêu cầu phủ kín diện tích nhìn thấy của mặt đường. Em đã kéo điểm neo của polygon sát ra tọa độ mép góc `(1.0, 719.8)`, đồng thời viền né chiếc xe van màu trắng bên phải.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại file `easy_semantic.zip`.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `easy_semantic` (Ảnh 1, dải phân cách bê tông bên trái) | Gán là `road` hay để trống không nhãn (unlabeled) | Hộ lan/tường bê tông không phải là mặt đường xe chạy theo chức năng giao thông | Quyết định chỉ vẽ `road` tới sát chân dải phân cách bê tông, không trùm lên tường |
| 2. `cp1_holes` (Kính xe ô tô nhìn xuyên thấu ra cảnh sau) | Khoét lỗ kính thành background hay giữ nguyên trong mask xe | Quy tắc bài thi nêu rõ: kính xe và các khe chi tiết thuộc thân xe, không được khoét lỗ | Quyết định bao trọn toàn bộ ô kính vào mask của xe |
| 3. `cp5_occlusion` (Chiếc xe bị cột/cây chắn ngang cắt làm 2 nửa) | Tách thành 2 xe riêng biệt hay giữ chung 1 xe | Cùng là một phương tiện vật lý bị che khuất | Quyết định gán cả 2 mảng nhìn thấy cho cùng một Object ID duy nhất |

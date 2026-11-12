# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603096
- Ngày / CVAT local: 12/11/2026 / CVAT local trên trình duyệt
- Công cụ đã dùng: Polygon, Brush, CVAT Local

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

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

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, nhân vật người (`person`) đứng ở vị trí nổi bật tại trung tâm khung hình.
- Class và quy tắc tôi dùng để chọn biên: Class `person`. Quy tắc biên: Sử dụng công cụ Polygon vẽ bao sát theo toàn bộ phần cơ thể và quần áo nhìn thấy (đầu, thân áo, quần, giày); phóng to zoom lớn để bo sát ranh giới giữa người và nền phía sau; không vẽ lan sang phần bóng đổ dưới sàn và không vẽ ăn vào các vật thể liền kề.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Ở các vị trí người đứng gần nhau hoặc bị che khuất một phần, gợi ý tự động có xu hướng gộp dính biên của hai người lại với nhau hoặc ăn vào bóng đổ; tôi đã chủ động chỉnh sửa lại các điểm nút polygon để tách biệt rõ ràng ranh giới của từng cá thể người.
- Nếu không dùng gợi ý: không dùng gợi ý tự động (toàn bộ các đối tượng được vẽ và kiểm soát thủ công bằng Polygon/Brush để đảm bảo tính chính xác của từng instance).

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `easy_semantic`, định dạng file export dataset của cả 3 ảnh.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: khác (sai định dạng export dataset).
- Bằng chứng tôi nhìn thấy: Khi chạy script kiểm tra `python scripts/inspect_submissions.py --dir submissions`, hệ thống báo lỗi `[LỖI] easy_semantic.zip: không có PNG trong SegmentationClass/; kiểm format Segmentation mask 1.1` và `cần đúng một labelmap.txt` do ban đầu chọn nhầm định dạng CVAT XML chỉ chứa file `annotations.xml`.
- Quy tắc và hành động sửa: Quy tắc yêu cầu task semantic phải xuất dưới dạng `Segmentation mask 1.1`. Tôi đã mở lại CVAT cho task `easy_semantic`, chọn Export job dataset với định dạng `Segmentation mask 1.1`, tải về và đặt tên chuẩn `easy_semantic.zip` dán đè vào thư mục `submissions/`.
- Sau sửa đã Save và export lại chưa? Đã export lại và chạy lại script kiểm tra thành công đạt `[OK]`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Sau khi export lại đúng định dạng `Segmentation mask 1.1`, lệnh `scripts/inspect_submissions.py` báo `[OK]` cho task `easy_semantic` và tất cả các task còn lại. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `cp4_curb` (`7d83710e-4697c3b2.jpg`) - Ranh giới tiếp giáp lòng đường và vỉa hè | Cả lòng đường và vỉa hè đều trải nhựa có màu xám tương đồng, dễ bị nhầm gộp chung là `road`. | Dựa theo quy tắc chức năng và cấu trúc gờ bó vỉa (curb): khu vực có gờ nâng cao dành cho người đi bộ là vỉa hè, mặt đường xe chạy là lòng đường. | Quyết định chia ranh giới theo đúng mép của gờ bó vỉa, gán phần trên là `sidewalk` và phần dưới là `road`. |
| 2. `cp1_holes` (`000000144300.jpg`) - Vùng kính lái và kính cửa sổ của xe ô tô | Khoét rỗng kính xe thành lỗ trống vì nhìn xuyên thấu vào nội thất cabin, hoặc giữ trọn vẹn kính trong mask xe. | Quy tắc gán nhãn của `cp1_holes`: chi tiết kính và khoảng hở cấu thành phương tiện phải nằm trọn trong mask của xe, không được khoét lỗ tùy tiện làm rỗng thân xe. | Quyết định vẽ phủ kín toàn bộ hình dáng xe bao gồm cả kính chắn gió và kính cửa xe vào nhãn `car`. |
| 3. `cp5_occlusion` (`000000336232.jpg`) - Thân xe/vật thể bị cột chắn chia cắt thành hai mảng rời nhau | Tách thành 2 object instance riêng biệt vì hai vùng pixel bị chia cắt, hoặc gán chung 1 instance. | Quy tắc occlusion cho instance segmentation: vật thể bị che khuất một phần vẫn là một cá thể duy nhất; chỉ vẽ phần pixel nhìn thấy và giữ nguyên cùng một instance ID. | Quyết định giữ chung 1 instance ID cho cả hai phần nhìn thấy của vật thể, không vẽ đoán nối xuyên qua cột chắn. |

# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602155
- Ngày / CVAT local: 17/09/2026
- Công cụ đã dùng: Polygon, Brush, Intelligent Scissors

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, chiếc xe ô tô con (`car`) ở tiền cảnh bên trái, nửa dưới ảnh.
- Class và quy tắc tôi dùng để chọn biên: Class `car`. Quy tắc biên: Vẽ bao bọc chính xác theo đường viền ngoài phần thân xe và lốp xe nhìn thấy được; dừng biên ngay tại mép đáy tiếp xúc mặt đường và mép bị cột biển báo che khuất, không vẽ suy đoán phần thân xe bị khuất sau cột.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Vùng gợi ý ban đầu nhận diện đúng thân xe nhưng bị lem xuống phần bóng đổ (shadow) trên mặt đường và lấn sang xe bên cạnh. Tôi đã dùng Polygon cắt bỏ phần bóng lem và tách mép xe với nền để mask bám sát vỏ xe thật.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình: Đã dùng kết hợp gợi ý tự động và nắn sửa polygon thủ công như trên.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `medium_instance`, ảnh `000000181542.jpg`, cụm hai xe ô tô đỗ sát nhau ở làn giữa.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: Gộp-tách (Merge hai vật thành một) và biên (Boundary lem sang nền).
- Bằng chứng tôi nhìn thấy: Khi phóng to, hai xe đỗ nối đuôi nhau bị vẽ dính liền thành một đa giác đối tượng duy nhất, và viền dưới bị lấn vào vạch kẻ đường.
- Quy tắc và hành động sửa: Áp dụng quy tắc "hai vật cùng lớp đỗ sát nhau vẫn là hai instance độc lập". Tôi đã tách mask chung thành hai object `car` riêng biệt, phóng to điều chỉnh khe hở ranh giới giữa hai xe và cắt bớt phần vạch đường.
- Sau sửa đã Save và export lại chưa? Đã Save trên CVAT và export lại file `medium_instance.zip` vào `submissions/`.

 Đã chạy script tự đánh giá `scorecard.py`, đạt tổng điểm ba tier là 32.1 / 82 (`easy_semantic`: mIoU 0.763 - 16.1/20; `medium_instance`: mean matched IoU 0.793, P@0.5=0.61, R@0.5=0.65 - 8.1/32; `hard_panoptic`: PQ 0.319 - 7.9/30).

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `easy_semantic` - ảnh `817bca71-00000000.jpg`, góc dưới bên phải mép vỉa hè có đoạn hạ dốc | Gán nhãn `road` hay `sidewalk` cho phần bê tông dốc nối từ hè xuống lòng đường | Màu sắc và vật liệu phẳng giống mặt đường nhựa, nhưng chức năng là lối tiếp cận cho người đi bộ trên hành lang hè | Quyết định gán `sidewalk`. Câu hỏi cho coach: Khi mép vỉa hè hạ dốc bằng phẳng ngang cốt mặt đường thì ranh giới tính từ chân dốc hay mép gờ trên? |
| 2. `medium_instance` - ảnh `000000181542.jpg`, xe ô tô bị cột biển báo che cắt ngang thân | Tách thành 2 object `car` riêng biệt hay gộp thành 1 object `car` gồm 2 phần đa giác rời | Quy tắc hình học: Một vật thể bị vật khác che khuất đứt đoạn vẫn là một instance duy nhất; không vẽ phỏng đoán phần khuất | Quyết định gán 1 object `car` duy nhất gồm các mảng nhìn thấy tách rời, không tạo thêm object ID mới. |
| 3. `hard_panoptic` - ảnh `000000460147.jpg`, dải phân cách giữa đường có bờ gờ đá mỏng trồng cây bụi | Gán toàn bộ là `vegetation` (stuff) hay bóc tách riêng bờ gờ đá (`sidewalk`/`curb`) và cây (`vegetation`) | Bờ gờ đá rất hẹp (< 3 pixel) trong khi thảm cây xanh chiếm phần lớn diện tích dải phân cách | Quyết định gán mảng xanh bên trong là `vegetation`, mép ngoài tiếp giáp lòng đường tính vào ranh giới với `road`. Xin coach hướng dẫn ngưỡng bề rộng tối thiểu để tách riêng gờ đá. |

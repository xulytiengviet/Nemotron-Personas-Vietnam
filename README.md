# VietPersona GeoAI

WebGIS nghiên cứu dữ liệu personas tổng hợp Việt Nam từ NVIDIA, kết hợp OpenStreetMap và bản đồ Vietflex Mapbasic2.

## Chạy ứng dụng
Mở `index.html` qua web server tĩnh, ví dụ `python -m http.server 8000`, sau đó truy cập http://localhost:8000. Tương thích GitHub Pages (Settings → Pages → Deploy from a branch → main → /root).

## Kết nối dữ liệu
Ứng dụng sử dụng Hugging Face Dataset Viewer API:
- `https://datasets-server.huggingface.co/splits?dataset=nvidia/Nemotron-Personas-Vietnam`
- `https://datasets-server.huggingface.co/rows?dataset=nvidia/Nemotron-Personas-Vietnam&config=...&split=train&offset=...&length=100`

API phục vụ mẫu truy vấn theo trang, không tải toàn bộ tập dữ liệu và không đại diện thống kê cho dân số Việt Nam. Lỗi kết nối được hiển thị rõ, không thay bằng dữ liệu giả.

## Bản đồ
- OSM raster tiles: https://www.openstreetmap.org/copyright
- Mapbasic2: https://github.com/xulytiengviet/mapbasic2
- R2 PMTiles: theo cấu hình công khai của mapbasic2. Chế độ Mapbasic2 hỗ trợ cả raster PMTiles và vector PMTiles có metadata vector_layers. Kiểu hiển thị vector hiện là màu sắc tổng quát; để tái hiện chính xác phong cách Mapbasic2 cần lấy style/layer gốc. Khi không đọc được archive, ứng dụng trở về OSM.
- Các vòng tròn biểu thị số persona trong mẫu theo **tâm vùng minh họa**, không phải tọa độ cư trú, heatmap dân số hay đo mật độ.

## Lưu ý khoa học
NVIDIA phát hành dữ liệu **tổng hợp**, không phải kết quả điều tra dân số. Trường `region` chứa 6 vùng, không phải tên tỉnh. Không sử dụng kết quả bản đồ mẫu để suy luận mức độ phổ biến ngoài thực tế hay ra quyết định quy hoạch. Cần kiểm định sai lệch, hiệu chuẩn với thống kê chính thức và chuẩn hóa hành chính nếu triển khai cấp tỉnh/xã.

## Ghi công
- Ứng dụng: Long Ngo, 2026. MIT (xem LICENSE).
- Dữ liệu: NVIDIA Nemotron-Personas-Vietnam, CC BY 4.0. https://huggingface.co/datasets/nvidia/Nemotron-Personas-Vietnam
- Bản đồ: © OpenStreetMap contributors, https://www.openstreetmap.org/copyright
- Nền bản đồ bổ sung: Vietflex Mapbasic2 (xem giấy phép dữ liệu riêng).

## Công việc tiếp theo
1. Xác thực browser CORS, API rate limits và PMTiles tileType.
2. Tạo ETL Python/DuckDB xuất thống kê Parquet theo 6 vùng.
3. Kiểm định trích xuất tỉnh, không gán vị trí giả thành địa chỉ cư trú.
4. Dùng ranh giới hành chính được kiểm tra để hiển thị choropleth.
5. Tích hợp trợ lý LLM với bộ công cụ truy vấn giới hạn, có trích nguồn.

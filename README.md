# HCM-KS26-CNTT1_NHAPMONCNTT_Session16_BTVN03
# BÀI 3 – XÂY DỰNG KHÔNG GIAN CỘNG TÁC DOANH NGHIỆP
## SESSION 16 – EX03

**Họ và tên:** Phạm Thành Đạo  
**Lớp:** HCM-KS26-CNTT1  
**Môn học:** Nhập môn Công nghệ thông tin  
**Chủ đề:** Xây dựng Enterprise Workspace cho dự án bãi đỗ xe thông minh.

---

# 1. PHÂN TÍCH VẤN ĐỀ HIỆN TẠI

Hiện tại, nhóm dự án bãi đỗ xe thông minh đang sử dụng Messenger để trao đổi công việc. Tuy nhiên, cách quản lý này đang gặp một số hạn chế:

**Vấn đề 1: Thông tin công việc bị trôi (Message Overload)**

- Thông báo kỹ thuật về cảm biến bị lẫn với tin nhắn trò chuyện và meme.
- Thành viên khó tìm lại thông tin quan trọng.
- Những quyết định kỹ thuật dễ bị bỏ sót.

**Vấn đề 2: Thiếu phân loại nội dung**

- Không có khu vực riêng cho thông báo, thảo luận kỹ thuật và giải trí.
- Mọi nội dung đều xuất hiện trong một cuộc trò chuyện chung.
- Khó theo dõi tiến độ và trách nhiệm của từng thành viên.

**Vấn đề 3: Giao tiếp đồng bộ quá mức**

- Thành viên thường mong đợi người khác phải trả lời ngay lập tức.
- Lập trình viên dễ bị gián đoạn khi đang viết code hoặc xử lý lỗi.
- Việc liên tục kiểm tra tin nhắn làm giảm khả năng tập trung.

**Giải pháp:** Xây dựng Enterprise Workspace trên Lark hoặc Slack, sử dụng Channels, Topics và Reply in Thread kết hợp với Asynchronous Communication để tổ chức công việc khoa học hơn.

---

# 2. THIẾT KẾ SƠ ĐỒ KIẾN TRÚC CHANNELS / TOPICS

## 2.1. Mô hình kiến trúc tổng thể

Nhóm lựa chọn xây dựng không gian cộng tác trên Slack hoặc Lark với tên:

**Workspace:** SMART PARKING – SMART CAMPUS

Cấu trúc các kênh được thiết kế như sau:

```text
SMART PARKING WORKSPACE
│
├── #00-thong-bao
│   └── Thông báo chính thức, deadline, lịch họp
│
├── #01-tien-do-du-an
│   └── Cập nhật công việc, phân công, báo cáo tiến độ
│
├── #02-ky-thuat-sensor
│   └── Cảm biến, dữ liệu đầu vào, lỗi thiết bị
│
├── #03-phat-trien-phan-mem
│   └── Source code, API, Database, Debug
│
├── #04-kiem-thu
│   └── Kiểm thử hệ thống, báo lỗi, kết quả Test
│
└── #05-giao-luu
    └── Trò chuyện, meme, hình ảnh giải trí
```

## 2.2. Quy định nội dung cho từng Channel

| Channel | Mục đích sử dụng | Quy định |
|---|---|---|
| #00-thong-bao | Thông báo chính thức của dự án | Chỉ đăng deadline, lịch họp và quyết định quan trọng. Hạn chế thảo luận tại đây. |
| #01-tien-do-du-an | Theo dõi tiến độ công việc | Mỗi thành viên cập nhật nhiệm vụ, trạng thái và thời hạn hoàn thành. |
| #02-ky-thuat-sensor | Trao đổi về cảm biến | Chỉ thảo luận về cảm biến, thông số kỹ thuật, kết nối và dữ liệu thiết bị. |
| #03-phat-trien-phan-mem | Phát triển phần mềm | Trao đổi về code, API, Database, thuật toán và xử lý lỗi. |
| #04-kiem-thu | Kiểm thử hệ thống | Đăng lỗi, hình ảnh minh chứng, kết quả kiểm thử và phương án khắc phục. |
| #05-giao-luu | Giải trí và giao lưu | Có thể chia sẻ meme, hình ảnh và trò chuyện ngoài công việc. |

## 2.3. Nguyên tắc phân luồng thông tin

- Thông báo quan trọng phải được đăng tại `#00-thong-bao`.
- Vấn đề liên quan đến cảm biến phải đưa vào `#02-ky-thuat-sensor`.
- Lỗi phần mềm phải đưa vào `#03-phat-trien-phan-mem`.
- Kết quả kiểm thử phải được cập nhật tại `#04-kiem-thu`.
- Nội dung giải trí chỉ đăng tại `#05-giao-luu`.

**Lợi ích:** Thành viên có thể truy cập đúng Channel để tìm thông tin cần thiết mà không phải đọc lại toàn bộ lịch sử trò chuyện.

---

# 3. ÁP DỤNG REPLY IN THREAD

## 3.1. Khái niệm

**Reply in Thread** là tính năng cho phép thành viên trả lời trực tiếp bên dưới một tin nhắn cụ thể, tạo thành một luồng thảo luận riêng biệt.

Thay vì gửi tất cả câu trả lời vào khung chat chính, các phản hồi sẽ được tập trung trong Thread của nội dung ban đầu.

## 3.2. Ví dụ thực tế

**Tình huống:** Trưởng nhóm thông báo cảm biến siêu âm HC-SR04 đang gặp lỗi đo khoảng cách.

**Tin nhắn chính tại #02-ky-thuat-sensor:**

```text
[09:00] Trưởng nhóm:

Phát hiện cảm biến HC-SR04 tại vị trí P03
đang trả về dữ liệu khoảng cách không ổn định.

Đề nghị nhóm kỹ thuật kiểm tra nguyên nhân
và đưa ra phương án xử lý trước 15:00 hôm nay.
```

**Các thành viên sử dụng Reply in Thread để phản hồi:**

```text
[09:15] Nam:
Em đã kiểm tra kết nối dây tín hiệu,
nguồn cấp điện vẫn hoạt động bình thường.

[09:30] Hoa:
Em nghi ngờ cảm biến bị nhiễu tín hiệu.
Đề xuất kiểm tra lại vị trí lắp đặt.

[10:00] Tuấn:
Em sẽ cập nhật chương trình lọc dữ liệu
để hạn chế sai số đo khoảng cách.
```

## 3.3. Quy định sử dụng Thread

1. Khi phản hồi một vấn đề đã có tin nhắn chính, phải sử dụng Reply in Thread.
2. Không tạo tin nhắn mới cho cùng một chủ đề nếu có thể tiếp tục thảo luận trong Thread hiện tại.
3. Khi vấn đề đã được giải quyết, người phụ trách cần đăng kết quả cuối cùng và cập nhật trạng thái công việc.

**Lợi ích:**

- Giữ khung chat chính gọn gàng.
- Không làm trôi các thông báo quan trọng.
- Dễ theo dõi nội dung thảo luận theo từng vấn đề.
- Giúp các thành viên tìm lại phương án xử lý lỗi nhanh chóng.

---

# 4. GIẢI QUYẾT BẪY DỮ LIỆU – ASYNCHRONOUS COMMUNICATION

## 4.1. Vấn đề

Một số thành viên trong nhóm có thói quen gửi tin nhắn và yêu cầu người khác trả lời ngay lập tức.

Ví dụ:

```text
@Nam trả lời ngay giúp mình!
Code xong chưa?
Sao không thấy phản hồi?
```

Điều này khiến lập trình viên bị gián đoạn khi đang viết code, kiểm tra lỗi hoặc xử lý các vấn đề kỹ thuật phức tạp.

## 4.2. Giải pháp: Asynchronous Communication

**Asynchronous Communication (Giao tiếp bất đồng bộ)** là phương thức giao tiếp không yêu cầu các thành viên phải phản hồi cùng một thời điểm.

Người gửi cung cấp đầy đủ thông tin, người nhận chủ động xem và phản hồi khi phù hợp với tiến độ công việc của mình.

Nhóm áp dụng các nguyên tắc sau:

**1. Không yêu cầu phản hồi ngay lập tức**

- Không mặc định người nhận phải trả lời ngay khi nhận được tin nhắn.
- Tôn trọng thời gian tập trung làm việc của từng thành viên.
- Chỉ yêu cầu phản hồi ngay trong trường hợp thực sự khẩn cấp.

**2. Tin nhắn phải đầy đủ ngữ cảnh**

Mỗi tin nhắn công việc cần có:

- Bối cảnh: Đang gặp vấn đề gì?
- Nội dung: Cần người nhận hỗ trợ việc gì?
- Thời hạn: Khi nào cần hoàn thành?
- Tài liệu: Có đường dẫn hoặc hình ảnh minh chứng nếu cần.

**3. Thiết lập thời gian phản hồi**

Nhóm thống nhất:

- Tin nhắn thông thường: Phản hồi trong vòng 4 giờ làm việc.
- Công việc cần phối hợp: Phản hồi trong ngày làm việc.
- Công việc khẩn cấp: Sử dụng phương thức liên hệ trực tiếp và nêu rõ mức độ khẩn cấp.
- Ngoài giờ làm việc: Không bắt buộc phản hồi, trừ trường hợp đã được thống nhất trước.

**4. Sử dụng trạng thái tập trung**

- Thành viên đang viết code có thể bật trạng thái Do Not Disturb.
- Chủ động kiểm tra và xử lý tin nhắn vào khoảng nghỉ giữa các phiên làm việc.
- Không sử dụng @all để thúc ép phản hồi.

## 4.3. Ví dụ chuyển đổi tin nhắn

**Cách giao tiếp chưa phù hợp:**

```text
Nam ơi trả lời mình ngay!
Code xong chưa vậy?
```

**Cách giao tiếp theo tư duy Asynchronous:**

```text
Chào Nam,

Mình đang hoàn thiện phần tích hợp dữ liệu cảm biến
HC-SR04 vào hệ thống quản lý bãi đỗ xe.

Hiện tại mình cần biết API trả về dữ liệu khoảng cách
theo đơn vị cm hay mm để hoàn thiện phần xử lý.

Nam kiểm tra giúp mình và phản hồi trước 15:00
hôm nay nhé.

Nếu đang bận xử lý code, bạn có thể phản hồi
khi hoàn thành công việc hiện tại.

Cảm ơn Nam!
```

**Kết luận:** Giao tiếp bất đồng bộ giúp giảm gián đoạn, tăng khả năng tập trung và nâng cao hiệu suất làm việc của toàn nhóm.

---

# 5. BẢN QUY ƯỚC GIAO TIẾP – 3 QUY TẮC VÀNG

Nhóm Smart Parking thống nhất áp dụng 3 quy tắc giao tiếp sau:

## QUY TẮC 1: NO-HELLO TRAP – NHẮN TIN PHẢI CÓ NGỮ CẢNH

**Không nên:**

```text
Có ai không?
Nam ơi!
Cho mình hỏi chút.
```

**Nên:**

```text
Chào Nam, mình đang xử lý lỗi kết nối cảm biến
HC-SR04 tại vị trí P03.

Bạn kiểm tra giúp mình nguyên nhân lỗi
kết nối trước 14:00 hôm nay nhé.

Mình đã đính kèm ảnh lỗi trong Thread này.
```

**Nguyên tắc:** Không gửi tin nhắn chào hỏi đơn lẻ rồi chờ người khác phản hồi mới nói mục đích. Hãy trình bày đầy đủ vấn đề ngay từ đầu.

## QUY TẮC 2: KHÔNG SPAM @ALL – TAG ĐÚNG NGƯỜI, ĐÚNG VIỆC

**Không nên:**

```text
@all Ai chưa xong thì báo ngay!
```

**Nên:**

```text
@Nam vui lòng hoàn thành phần API trước 15:00.

@Hoa kiểm tra lại dữ liệu cảm biến
và cập nhật kết quả trước 16:00.

Mọi người phản hồi trong Thread tương ứng nhé.
```

**Nguyên tắc:**

- Chỉ tag người trực tiếp liên quan đến công việc.
- Không lạm dụng @all hoặc @here.
- Chỉ sử dụng thông báo toàn nhóm khi có thông tin thực sự quan trọng và cần thiết.

## QUY TẮC 3: ASYNCHRONOUS FIRST – TÔN TRỌNG THỜI GIAN TẬP TRUNG

**Không nên:**

- Ép thành viên trả lời ngay lập tức.
- Gửi liên tiếp nhiều tin nhắn khi người nhận chưa phản hồi.
- Làm phiền lập trình viên khi họ đang tập trung viết code.

**Nên:**

- Cung cấp đầy đủ thông tin và thời hạn phản hồi.
- Sử dụng Reply in Thread để tập trung thảo luận.
- Tôn trọng trạng thái Do Not Disturb.
- Chỉ sử dụng phương thức liên hệ khẩn cấp khi thực sự cần thiết.

**Nguyên tắc:** Đánh giá hiệu quả công việc dựa trên kết quả và tiến độ hoàn thành, không dựa trên tốc độ trả lời tin nhắn.

---

# 6. KẾT QUẢ VÀ LỢI ÍCH SAU KHI TRIỂN KHAI

Sau khi thiết lập Enterprise Workspace, nhóm dự án bãi đỗ xe thông minh có thể đạt được những lợi ích sau:

| Tiêu chí | Trước khi triển khai | Sau khi triển khai |
|---|---|---|
| Quản lý thông tin | Tin nhắn lẫn lộn trên Messenger | Phân loại theo Channels |
| Thảo luận | Tin nhắn nối tiếp, dễ trôi | Sử dụng Reply in Thread |
| Thông báo | Deadline dễ bị bỏ sót | Tập trung tại #00-thong-bao |
| Giao tiếp | Thường xuyên yêu cầu trả lời ngay | Áp dụng Asynchronous Communication |
| Phân công | Khó xác định người chịu trách nhiệm | Tag đúng người, đúng nhiệm vụ |
| Tập trung | Lập trình viên thường xuyên bị gián đoạn | Tôn trọng thời gian làm việc tập trung |

---

# 7. KẾT LUẬN

Qua bài tập Session 16 – Exercise 03, em hiểu rằng việc xây dựng một không gian cộng tác doanh nghiệp không chỉ đơn giản là chuyển từ Messenger sang Lark hoặc Slack mà quan trọng hơn là phải thiết lập được quy trình giao tiếp và quản lý thông tin khoa học.

Các kỹ năng quan trọng được vận dụng gồm:

- **Channels / Topics:** Phân loại và quản lý nội dung theo từng chủ đề.
- **Reply in Thread:** Giữ cho khung chat chính gọn gàng, hạn chế trôi tin nhắn.
- **Asynchronous Communication:** Giúp thành viên chủ động sắp xếp thời gian và không bị ép phản hồi ngay lập tức.
- **Tagging:** Xác định đúng người phụ trách, tránh thông báo không cần thiết.
- **No-Hello Trap:** Đảm bảo mỗi tin nhắn có đầy đủ ngữ cảnh và mục đích.

Em nhận thấy rằng một nhóm làm việc hiệu quả không phải là nhóm có nhiều tin nhắn nhất hay phản hồi nhanh nhất, mà là nhóm biết truyền đạt đúng thông tin, đúng người, đúng kênh và đúng thời điểm.

Việc áp dụng những nguyên tắc trên sẽ giúp dự án bãi đỗ xe thông minh hoạt động chuyên nghiệp hơn, giảm thiểu sai sót và nâng cao hiệu suất làm việc nhóm.

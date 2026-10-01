#Geforce rtx 5090 

## ABSTRACTION
 Spatial understanding of biomacromolecular structures such as proteins and gene mutations is critical to building intuition in molecular biology, yet Vietnamese high school curricula currently rely almost exclusively on static 2D diagrams. This [limits students' ability to visualize conformational changes caused by mutations, a gap that has been linked to weaker conceptual understanding in structural biology education.](https://pubs.acs.org/afsthl/article-pdf/5/6/2381/41100941/fs5c00200.pdf?fbclid=IwY2xjawUp-XdleHRuA2FlbQIxMABwZG9mBWJyaWQRMW5JcTBjak5aRjhWS29xWU5zcnRjBmFwcF9pZBAyMjIwMzkxNzg4MjAwODkyAAEefzRrSXyDK16ujJex8l8SWR7UYMyW6aeikrwYxBY4KzDqINZOkjYeN6GU4O4_aem_b4snRm_UzwhcVEzlMEEDJQ).Furthermore, the most detailed resources on 3D structures—including the AlphaFold Database and international references such as *Campbell Biology* are written in English and densely packed with specialized terminology, making them difficult for high school students in Vietnam to access.
This project addresses both gaps simultaneously by developing an interactive, Vietnamese language, web based platform for exploring 3D protein structures and gene mutations. The system uses a decoupled Client-Server architecture: an HTML/JavaScript frontend renders molecular models via a WebGL/Three.js viewer directly in the browser, while a Java (Spring Boot) backend exposes RESTful APIs and performs sequence alignment between wild-type and mutant sequences. To generate accessible explanations, the platform integrates a Large Language Model constrained by a curated, pre-verified mutation dataset, using retrieval-based prompting so that the model's explanations are grounded in checked reference data rather than generated freely — reducing the risk of factual hallucination.

The result is a structural biology teaching tool that removes both the language barrier and the 2D-to-3D visualization gap, intended to help Vietnamese high school students build intuition for molecular structure and engage more confidently with scientific research practices.
An interactive, AI-powered web platform that helps Vietnamese high school students explore 3D protein structures and gene mutations — directly in the browser, with no software installation and no English-language barrier.

Built for Geforce rtx 5090 
---

## Problem

Các chương trình sinh học bậc trung học phổ thông tại Việt Nam hầu như chỉ dựa vào các sơ đồ 2D tĩnh để giảng dạy về cấu trúc protein và đột biến gen, qua đó hạn chế khả năng hình dung của học sinh về những thay đổi cấu hình do đột biến gây ra. Các nguồn tài nguyên 3D chi tiết nhất hiện có [AlphaFold DataBase]((https://alphafold.ebi.ac.uk)), các tài liệu tham khảo quốc tế như [*Campbell Biology*](https://www.pearson.com/en-us/subject-catalog/p/campbell-biology/P200000014184/9780135455890) are written entirely in English with dense technical terminology, putting them out of reach for most Vietnamese secondary students.

Có ba rào cản cụ thể ngăn cản học sinh, sinh viên tận dụng các nguồn tài nguyên hiện có:

1. **Ngôn ngữ**: Các thông tin chính thông này và nhiều nguồn nổi tiếng khác đòi hỏi trình độ đọc hiểu tiếng Anh nâng cao và kiến ​​thức về thuật ngữ chuyên ngành hóa sinh. 
2. **Giao diện phức tạp**: việc xem các cấu trúc AlphaFold thường đòi hỏi phải tải xuống tệp tin và sử dụng các phần mềm như PyMOL hoặc ChimeraX, điều này không khả thi trong môi trường lớp học.
3. **Thiếu định hướng sư phạm**: Những thông tin hiện có chỉ cung cấp dữ liệu cấu trúc thô, thay vì những giải thích mang tính hướng dẫn về lý do tại sao một đột biến lại quan trọng.
## Solution

Một nền tảng chạy trên trình duyệt, cho phép học sinh phổ thông:
- Không những được trực tiếp cấu trúc protein 3D ngay trên trình duyệt, mà còn được tự so sánh trình tự dạng hoang dã (wild-type) với trình tự đột biến nhằm quan sát sự khác biệt về cấu trúc tương ứng.
- Đọc các giải thích bằng tiếng Việt, sử dụng ngôn ngữ dễ hiểu về tác động chức năng của đột biến. Nội dung này được tạo ra bởi một mô hình ngôn ngữ lớn (LLM) hoạt động dựa trên tập dữ liệu đã được chọn lọc và kiểm chứng kỹ lưỡng, thay vì tạo nội dung tự do nhằm giảm thiểu nguy cơ xảy ra hiện tượng dữ liệu của AI.
---

## Architecture

Thiết kế tách biệt giữa client và server, bao gồm ba thành phần:

- **Frontend**: HTML/JavaScript kết hợp với trình xem 3D WebGL/Three.js, thực hiện dựng hình (render) các mô hình phân tử trực tiếp trên trình duyệt. Thành phần này gửi yêu cầu đến backend, đồng thời hiển thị các cấu trúc và nội dung giải thích nhận được.
- **Backend**: Java (Spring Boot). Cung cấp các RESTful API, tiếp nhận yêu cầu từ frontend và thực hiện so sánh trình tự giữa dạng tự nhiên (wild-type sequence) và dạng đột biến.
- **Lớp AI**: Mỗi lượt truy vấn được kết hợp với dữ liệu tham chiếu đã được kiểm chứng (các tác động về cấu trúc/chức năng đã biết của một đột biến cụ thể, lấy từ bộ dữ liệu nội bộ đã qua chọn lọc) thông qua kỹ thuật "retrieval-augmented prompting" (gợi ý có bổ sung dữ liệu truy xuất), sau đó được gửi tới mô hình ngôn ngữ lớn (LLM). LLM được yêu cầu chỉ giải thích dựa trên dữ liệu đã truy xuất bằng tiếng Việt đơn giản, vai trò của nó chỉ giới hạn ở việc dịch (đơn giản hóa thông tin chứ không tự suy luận về tác động của đột biến) và nội dung giải thích này được chuyển ngược lại cho backend, rồi đến frontend.

**Luồng dữ liệu:** Yêu cầu từ Frontend → Backend (so sánh trình tự) → Lớp AI (giải thích dựa trên dữ liệu truy xuất) → Backend → Frontend (hiển thị đồng thời hình ảnh 3D và nội dung giải thích).

---

## Project Structure

```
.
├── frontend/           # HTML/JS + 3D viewer (WebGL/Three.js)
│   ├── index.html
│   ├── viewer.js
│   └── styles.css
├── backend/             # Java Spring Boot service
│   ├── src/main/java/...
│   └── pom.xml
├── alignment/            # sequence comparison logic (wild-type vs. mutant)
├── data/                 # curated, pre-verified mutation dataset
│   └── mutations.json
├── docs/                 # paper, diagrams, screenshots
└── README.md
```

---

## Setup and Installation

### Yêu cầu hệ thống
- Java 17+ và Maven
- Trình duyệt hiện đại 
- Khóa API cho nhà cung cấp LLM của dự án (thiết lập dưới dạng biến môi trường)
### Backend

```bash
cd backend
mvn clean install
mvn spring-boot:run
```

Backend mặc định chạy trên `http://localhost:8080`.

### Frontend

```bash
cd frontend
# if using a simple static server:
npx serve .
```

Open `http://localhost:3000` 
### Environment Variables

tạo `.env` file (chưa được đưa vào hệ thống quản lý phiên bản):

```
LLM_API_KEY=your_api_key_here
LLM_API_URL=https://api.your-provider.com/v1/messages
```

---

## experiment

1. Mở ứng dụng trên trình duyệt của dự án lên.
2. Chọn một ví dụ về đột biến từ bộ dữ liệu đã được chọn lọc (ví dụ: đột biến thay thế trên gen BRCA1).
3. Quan sát cấu trúc 3D của dạng hoang dại (wild-type) và dạng đột biến đặt cạnh nhau.
4. Đọc phần giải thích bằng tiếng Việt do AI tạo ra, dựa trên dữ liệu tham chiếu đã được kiểm chứng cho đột biến đó.

---
## Current Scope 

**Đã xây dựng và hoạt động trong bản demo này:**
 ví dụ đột biến trong tập dữ liệu được chọn lọc và xác minh trước
- Trình xem 3D dựa trên trình duyệt với so sánh kiểu hoang dã/đột biến
- Giải thích bằng AI tiếng Việt có giới hạn truy xuất

**Đã giảm tải bớt:**
- So sánh trình tự hiện đang sử dụng [phương pháp cơ bản/thủ công — chỉ định]

- Tập dữ liệu được chọn lọc thủ công thay vì lấy trực tiếp từ ClinVar/UniProt
  
**Chưa xây dựng:**
- Thử nghiệm thí điểm trong lớp học / kiểm tra khả năng sử dụng
- Hỗ trợ đa ngôn ngữ ngoài tiếng Việt

---

## Evaluation Roadmap

### Kế hoạch Đánh giá
- **Kiểm thử khả năng sử dụng giai đoạn thử nghiệm:** một nhóm nhỏ học sinh sử dụng phiên bản sản phẩm tối thiểu (MVP) với bộ dữ liệu đột biến đã được chọn lọc, sau đó thực hiện bài kiểm tra ngắn để đánh giá mức độ hiểu bài.
- **So sánh kết quả học tập:** khi điều kiện cho phép, so sánh người dùng nền tảng với nhóm đối chứng (sử dụng tài liệu 2D truyền thống) thông qua các câu hỏi về tư duy cấu trúc.

### Ngắn hạn
- Mở rộng bộ dữ liệu đột biến đã chọn lọc, ưu tiên các đột biến có trong chương trình giáo dục phổ thông tại Việt Nam.
- Thay thế phương pháp căn chỉnh trình tự thủ công bằng một thuật toán mạnh mẽ (ví dụ: Needleman-Wunsch) có hỗ trợ các thao tác chèn/xóa, thay vì chỉ hỗ trợ thay thế.
- Bổ sung trích dẫn nguồn trực tiếp vào các phần giải thích của AI để đảm bảo tính minh bạch.

### Trung hạn
- Triển khai thử nghiệm tại các lớp học thực tế và thu thập phản hồi từ giáo viên cũng như học sinh.
- Tối ưu hóa thiết kế câu lệnh (prompt) cho AI dựa trên các câu hỏi thực tế của học sinh nằm ngoài phạm vi bộ dữ liệu ban đầu.

### Dài hạn
- Mở rộng phạm vi bao phủ cấu trúc để đạt được quy mô tương đương với AlphaFold.
- Bổ sung tính năng đánh giá mức độ hiểu bài ngay trên nền tảng dành cho giáo viên.
- Nghiên cứu khả năng hỗ trợ đa ngôn ngữ bên cạnh tiếng Việt.

Vui lòng tham khảo tệp `docs/paper.pdf` để xem toàn văn báo cáo nghiên cứu.
---

## Team
Name: **Geforce rtx 5090**

Thành viên:
> - Hồ An Khang: dataset curator,project founder and moderator
> - Cái Trí Viễn: Presenting ideas, developing the story and formulating the strategy.
> - Võ Minh Quân: Full-stack implementation, backend and core Tech Lead
> - Hoàng Gia Kiệt: Full-stack implementation alongside "Võ Minh Quân", in charge of algorithm and Data 









## License

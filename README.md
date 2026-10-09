# Geforce rtx 5090 

## ABSTRACTION
 Spatial understanding of biomacromolecular structures such as proteins and gene mutations is critical to building intuition in molecular biology, yet Vietnamese high school curricular currently rely almost exclusively on static 2D diagrams. This [limits students' ability to visualize conformational changes caused by mutations, a gap that has been linked to weaker conceptual understanding in structural biology education.](https://pubs.acs.org/afsthl/article-pdf/5/6/2381/41100941/fs5c00200.pdf).Furthermore, the most detailed resources on 3D structures including the AlphaFold Database and international references such as *Campbell Biology* are written in English and densely packed with specialized terminology, making them difficult for high school students in Vietnam to access.
This project addresses both gaps simultaneously by developing an interactive, Vietnamese language, web based platform for exploring 3D protein structures and gene mutations. The system uses a decoupled Client-Server architecture: an HTML/JavaScript frontend renders molecular models via a WebGL/Three.js viewer directly in the browser, while a Java (Spring Boot) backend exposes RESTful APIs and performs sequence alignment between wild-type and mutant sequences. To generate accessible explanations, the platform integrates a Large Language Model constrained by a curated, pre-verified mutation dataset, using retrieval-based prompting so that the model's explanations are grounded in checked reference data rather than generated freely reducing the risk of factual hallucination.

The result is a structural biology teaching tool that removes both the language barrier and the 2D-to-3D visualization gap, intended to help Vietnamese high school students build intuition for molecular structure and engage more confidently with scientific research practices.
An interactive, AI-powered web platform that helps Vietnamese high school students explore 3D protein structures and gene mutations directly in the browser, with no software installation and no English language barrier.

Built for Geforce rtx 5090 
---

## Problem

Các chương trình sinh học bậc trung học phổ thông tại Việt Nam hầu như chỉ dựa vào các sơ đồ 2D tĩnh để giảng dạy về cấu trúc protein và đột biến gen, qua đó hạn chế khả năng hình dung của học sinh về những thay đổi cấu hình do đột biến gây ra. Các nguồn tài nguyên 3D chi tiết nhất hiện có [AlphaFold DataBase](https://alphafold.ebi.ac.uk), các tài liệu tham khảo quốc tế như [*Campbell Biology*](https://www.pearson.com/en-us/subject-catalog/p/campbell-biology/P200000014184/9780135455890)được viết hoàn toàn bằng tiếng Anh với dày đặc thuật ngữ chuyên môn, khiến đa số học sinh trung học Việt Nam khó lòng tiếp cận được.

Có ba rào cản cụ thể ngăn cản học sinh, sinh viên tận dụng các nguồn tài nguyên hiện có:

1. **Ngôn ngữ**: Các thông tin chính thông này và nhiều nguồn nổi tiếng khác đòi hỏi trình độ đọc hiểu tiếng Anh nâng cao và kiến ​​thức về thuật ngữ chuyên ngành hóa sinh. Đơn cử như [PubMed Central (PMC)](https://www.annualreviews.org/content/journals/biochem) là nơi tổng hợp các bài viết mang tính hệ thống của các chuyên gia đầu ngành, rất thích hợp để cập nhật kiến thức tổng quan nâng cao nhưng không có bản dịch chính thức và các từ ngữ trong các báo cáo rất nặng tính hàn lâm của ngôn ngữ.
2. **Giao diện phức tạp**: việc xem các cấu trúc AlphaFold thường đòi hỏi phải tải xuống tệp tin và sử dụng các phần mềm như PyMOL hoặc ChimeraX, điều này không khả thi trong môi trường lớp học. Cần lưu ý: AlphaFold DB có trình xem 3D tích hợp ngay trên web, nhưng giao diện chỉ có tiếng Anh, không hỗ trợ so sánh dạng hoang dại/đột biến và không có hướng dẫn sư phạm (xem phần *Nguồn cấu trúc 3D* bên dưới).
3. **Thiếu định hướng sư phạm**: Những thông tin hiện có chỉ cung cấp dữ liệu cấu trúc thô, thay vì những giải thích mang tính hướng dẫn về lý do tại sao một đột biến lại quan trọng.
## Solution

Một nền tảng chạy trên trình duyệt, cho phép học sinh phổ thông:
- Không những được trực tiếp cấu trúc protein 3D ngay trên trình duyệt, mà còn được tự so sánh trình tự dạng hoang dã (wild-type) với trình tự đột biến nhằm quan sát sự khác biệt về cấu trúc tương ứng.
- Đọc các giải thích bằng tiếng Việt, sử dụng ngôn ngữ dễ hiểu về tác động chức năng của đột biến. Nội dung này được tạo ra bởi một mô hình ngôn ngữ lớn (LLM) hoạt động dựa trên tập dữ liệu đã được chọn lọc và kiểm chứng kỹ lưỡng, thay vì tạo nội dung tự do nhằm giảm thiểu nguy cơ xảy ra hiện tượng dữ liệu của AI.
- *(Dự kiến)* Thay đổi nồng độ oxy bằng thanh trượt trên hemoglobin và so sánh hemoglobin bình thường với hemoglobin hồng cầu hình liềm để thấy đột biến và môi trường cùng ảnh hưởng đến protein như thế nào (xem phần *Tính năng Oxy*).
---

## Architecture

Thiết kế tách biệt giữa client và server, bao gồm ba thành phần:

- **Frontend**: HTML/JavaScript kết hợp với trình xem 3D WebGL/Three.js, thực hiện dựng hình (render) các mô hình phân tử trực tiếp trên trình duyệt. Thành phần này gửi yêu cầu đến backend, đồng thời hiển thị các cấu trúc và nội dung giải thích nhận được.
- **Backend**: Java (Spring Boot). Cung cấp các RESTful API, tiếp nhận yêu cầu từ frontend và thực hiện so sánh trình tự giữa dạng tự nhiên (wild-type sequence) và dạng đột biến.
- **Lớp AI**: Mỗi lượt truy vấn được kết hợp với dữ liệu tham chiếu đã được kiểm chứng (các tác động về cấu trúc/chức năng đã biết của một đột biến cụ thể, lấy từ bộ dữ liệu nội bộ đã qua chọn lọc) thông qua kỹ thuật "retrieval-augmented prompting" (gợi ý có bổ sung dữ liệu truy xuất), sau đó được gửi tới mô hình ngôn ngữ lớn (LLM). LLM được yêu cầu chỉ giải thích dựa trên dữ liệu đã truy xuất bằng tiếng Việt đơn giản, vai trò của nó chỉ giới hạn ở việc dịch (đơn giản hóa thông tin chứ không tự suy luận về tác động của đột biến) và nội dung giải thích này được chuyển ngược lại cho backend, rồi đến frontend.

**Luồng dữ liệu:** Yêu cầu từ Frontend → Backend (so sánh trình tự) → Lớp AI (giải thích dựa trên dữ liệu truy xuất) → Backend → Frontend (hiển thị đồng thời hình ảnh 3D và nội dung giải thích).

---

## Nguồn cấu trúc 3D: thực nghiệm (PDB) và dự đoán (AlphaFold)

Nền tảng sử dụng hai loại cấu trúc 3D. Mỗi cấu trúc hiển thị đều được gắn nhãn nguồn rõ ràng.

| Nguồn | Bản chất | Cách sử dụng |
|---|---|---|
| **PDB (thực nghiệm)** | Cấu trúc được xác định trong phòng thí nghiệm (tinh thể học tia X, cryo-EM) | Ưu tiên dùng để so sánh dạng hoang dại/đột biến khi có sẵn (ví dụ: hemoglobin bình thường và hemoglobin hồng cầu hình liềm) |
| **AlphaFold DB (dự đoán)** | Cấu trúc do AI dự đoán, tra cứu theo mã UniProt | Dùng cho các protein chưa có cấu trúc thực nghiệm |

Việc phân biệt "thực nghiệm (PDB)" và "dự đoán (AlphaFold)" cũng là một bài học về hiểu biết khoa học cho học sinh, đồng thời củng cố định hướng đặt độ chính xác lên hàng đầu của dự án.

### Giới hạn cần nêu rõ

- **AlphaFold không đáng tin cậy để thể hiện tác động của đột biến điểm đơn.** Công cụ này dự đoán cấu trúc từ một trình tự, không phải để cho thấy một amino acid bị thay thế làm thay đổi sự cuộn gập ra sao. Vì vậy dự án không dùng "cấu trúc đột biến" do AlphaFold dự đoán làm bằng chứng về tác động của đột biến; khi có cấu trúc thực nghiệm thì dùng cấu trúc thực nghiệm để so sánh.
- **Cấu trúc dự đoán là tĩnh**, không thể hiện được động học hay sự thay đổi cấu hình.
- **AlphaFold DB đã có trình xem 3D tích hợp trên web.** Khoảng trống mà dự án lấp đầy là: giao diện tiếng Việt, so sánh dạng hoang dại/đột biến và phần giải thích mang tính sư phạm.

### Hướng tăng cường

- **Tô màu theo độ tin cậy (pLDDT):** hiển thị cấu trúc dự đoán theo mức độ tin cậy, kèm chú giải tiếng Việt ("vùng tin cậy cao / thấp"), giúp học sinh hiểu rằng dự đoán luôn có độ bất định.
- **AlphaMissense (tùy chọn, protein người):** điểm số dự đoán mức gây bệnh của đột biến sai nghĩa, có thể dùng để tô màu các gốc amino acid. Cần kiểm tra điều khoản giấy phép (dữ liệu phi thương mại) và trình bày đây là dự đoán, không phải chẩn đoán.
- **Truy xuất theo mã UniProt:** AlphaFold DB có API theo mã UniProt, giúp quy trình nạp cấu trúc gọn hơn và cụ thể hóa mục tiêu mở rộng quy mô trong lộ trình.
- **Trích dẫn nguồn:** Jumper et al. (2021) và Varadi et al. (2022), đồng thời tuân thủ điều khoản sử dụng của cơ sở dữ liệu.

---

## Tính năng Oxy (Hemoglobin)

> **Trạng thái:** đang lên kế hoạch, chưa triển khai.

### Ý tưởng

Một thanh trượt nồng độ oxy trên hemoglobin, kết hợp với đột biến gây bệnh hồng cầu hình liềm, để thanh trượt trở thành công cụ học tập thực sự thay vì chỉ để trang trí.

- Hemoglobin chuyển giữa dạng không gắn oxy ("căng", T) và dạng gắn oxy ("thư giãn", R).
- Hemoglobin hồng cầu hình liềm (HbS) hoạt động gần giống hemoglobin bình thường khi oxy cao, nhưng ở trạng thái thiếu oxy nó trùng hợp thành các sợi cứng làm biến dạng hồng cầu.
- Đặt hai dạng cạnh nhau rồi giảm nồng độ oxy cho thấy điểm khác biệt giữa chúng, kết nối so sánh đột biến, thanh trượt oxy và một căn bệnh học sinh đã biết.

### Thiết kế đề xuất

1. **Hai cấu trúc thực nghiệm làm điểm đầu và điểm cuối** (dự kiến 2HHB cho dạng không gắn oxy và 1HHO cho dạng gắn oxy; mã PDB sẽ được xác minh lại trước khi sử dụng). Chỉ hai trạng thái này là cấu trúc thật.
2. **Trung thực về các khung hình trung gian:** nếu làm mượt chuyển tiếp giữa hai cấu trúc thì đó là phép nội suy toán học, không phải trạng thái trung gian có thật, và sẽ được gắn nhãn "minh họa chuyển tiếp giữa hai trạng thái thực nghiệm". Chuyển đổi rõ ràng giữa hai trạng thái thật là phương án đáng tin cậy hơn.
3. **Đường cong phân ly oxy theo thời gian thực:** thanh trượt điều chỉnh phân áp oxy, biểu đồ hình chữ S (phương trình Hill) hiển thị bên cạnh mô hình 3D, chồng đường cong của hemoglobin bình thường và hemoglobin hồng cầu hình liềm trên cùng một biểu đồ.
4. **Tình huống thực tế thay cho thanh trượt đơn thuần:** sống ở độ cao lớn, vận động mạnh, ngộ độc khí carbon monoxide (CO gắn với hemoglobin chặt hơn oxy nhiều lần), người mang gen hồng cầu hình liềm. Mỗi tình huống đặt sẵn thanh trượt và hiển thị giải thích ngắn bằng tiếng Việt.
5. **Dự đoán rồi mới xem kết quả:** hỏi "điều gì xảy ra khi oxy còn 20%?", để học sinh chọn đáp án trước, sau đó hiển thị kết quả thực tế cùng giải thích.

### Stress oxy hóa (tính năng phụ, tùy chọn)

- Không mô phỏng việc protein bị hư hại hay mất cấu trúc, vì thường không có cấu trúc "bị oxy hóa" nào được công bố để hiển thị.
- Thay vào đó, làm nổi bật các gốc amino acid đã được tài liệu khoa học xác định là dễ bị oxy hóa (cysteine, methionine) kèm nguồn trích dẫn, và trình bày là "các vị trí dễ tổn thương đã biết".
- Ví dụ ứng viên: crystallin (đục thủy tinh thể), hemoglobin chuyển thành methemoglobin. Cần kiểm tra tài liệu trước khi đưa vào.

---

## Kế hoạch demo trực tiếp

Ứng dụng sẽ chạy trực tiếp tại địa điểm thi, nên độ ổn định được ưu tiên hơn số lượng tính năng.

- **Không phụ thuộc mạng:** tải sẵn tệp cấu trúc của 5 đến 10 protein dùng trong demo, không gọi API AlphaFold khi trình bày.
- **Lưu sẵn phần giải thích của AI:** tạo trước và lưu giải thích cho mọi đột biến trong bộ dữ liệu; chỉ dùng LLM trực tiếp cho câu hỏi bổ sung (nếu có), kèm thông báo dự phòng khi lỗi.
- **Khởi động bằng một lệnh:** đóng gói backend Spring Boot và frontend tĩnh thành một tệp JAR; thử khởi động từ trạng thái sạch (có thể mất 10 đến 30 giây).
- **Nếu triển khai trực tuyến:** dùng gói luôn hoạt động (gói miễn phí có thể tạm ngủ), giữ khóa API chỉ ở phía server và đặt giới hạn chi tiêu; vẫn giữ bản chạy cục bộ làm phương án dự phòng.
- **Kiểm tra phần cứng:** thử WebGL trên ít nhất hai trình duyệt và một máy yếu, kiểm tra độ phân giải máy chiếu, dùng cỡ chữ lớn.
- **Giao diện thân thiện với giám khảo:** điểm bắt đầu rõ ràng, nút đặt lại (reset) và kiểm tra dữ liệu đầu vào.
- **Phương án dự phòng:** video quay màn hình toàn bộ demo, laptop thứ hai cùng bản build, điểm phát sóng từ điện thoại và ảnh chụp màn hình trong slide.

**Thứ tự ưu tiên:** (1) luồng chính chạy ổn định khi ngoại tuyến: chọn đột biến, xem hai cấu trúc, kéo thanh trượt oxy, đọc giải thích đã lưu; (2) biểu đồ đường cong oxy và bước dự đoán trước; (3) hoàn thiện, dự phòng và tập dượt; (4) các tính năng tùy chọn chỉ khi phần lõi đã ổn định.

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
- So sánh trình tự hiện đang sử dụng 

- Tập dữ liệu được chọn lọc thủ công thay vì lấy trực tiếp từ ClinVar/UniProt
  
**Chưa xây dựng:**
- Thử nghiệm thí điểm trong lớp học / kiểm tra khả năng sử dụng
- Hỗ trợ đa ngôn ngữ ngoài tiếng Việt
- Tính năng thanh trượt oxy trên hemoglobin và so sánh hồng cầu hình liềm (đang lên kế hoạch)
- Tô màu theo độ tin cậy pLDDT và điểm số AlphaMissense

---

## Evaluation Roadmap

### Kế hoạch Đánh giá
- **Kiểm thử khả năng sử dụng giai đoạn thử nghiệm:** một nhóm nhỏ học sinh sử dụng phiên bản sản phẩm tối thiểu (MVP) với bộ dữ liệu đột biến đã được chọn lọc, sau đó thực hiện bài kiểm tra ngắn để đánh giá mức độ hiểu bài.
- **So sánh kết quả học tập:** khi điều kiện cho phép, so sánh người dùng nền tảng với nhóm đối chứng (sử dụng tài liệu 2D truyền thống) thông qua các câu hỏi về tư duy cấu trúc.

### Ngắn hạn
- Mở rộng bộ dữ liệu đột biến đã chọn lọc, ưu tiên các đột biến có trong chương trình giáo dục phổ thông tại Việt Nam.
- Thay thế phương pháp căn chỉnh trình tự thủ công bằng một thuật toán mạnh mẽ (ví dụ: Needleman-Wunsch) có hỗ trợ các thao tác chèn/xóa, thay vì chỉ hỗ trợ thay thế.
- Bổ sung trích dẫn nguồn trực tiếp vào các phần giải thích của AI để đảm bảo tính minh bạch.
- Hoàn thiện tính năng oxy trên hemoglobin (so sánh hemoglobin bình thường và hồng cầu hình liềm, đường cong phân ly oxy, bước dự đoán trước) và gắn nhãn nguồn "thực nghiệm (PDB)" / "dự đoán (AlphaFold)" cho mọi cấu trúc.

### Trung hạn
- Triển khai thử nghiệm tại các lớp học thực tế và thu thập phản hồi từ giáo viên cũng như học sinh.
- Tối ưu hóa thiết kế câu lệnh (prompt) cho AI dựa trên các câu hỏi thực tế của học sinh nằm ngoài phạm vi bộ dữ liệu ban đầu.
- Tô màu cấu trúc dự đoán theo độ tin cậy pLDDT với chú giải tiếng Việt.

### Dài hạn
- Mở rộng phạm vi bao phủ cấu trúc để đạt được quy mô tương đương với AlphaFold.
- Bổ sung tính năng đánh giá mức độ hiểu bài ngay trên nền tảng dành cho giáo viên.
- Nghiên cứu khả năng hỗ trợ đa ngôn ngữ bên cạnh tiếng Việt.
- Tích hợp điểm số AlphaMissense (protein người) và làm nổi bật các gốc amino acid dễ bị oxy hóa dựa trên tài liệu khoa học.

---

## Team
Name: **Geforce rtx 5090**

Thành viên:
> - Hồ An Khang: dataset curator,project founder and moderator
> - Cái Trí Viễn: Presenting ideas, developing the story and formulating the strategy.
> - Võ Minh Quân: Full-stack implementation, backend and core Tech Lead
> - Nguyễn Hoàng Gia Kiệt: Full-stack implementation alongside "Võ Minh Quân", in charge of algorithm and Data 

## License
- not yet.:<

<!--
TODO trước khi nộp (ghi chú nội bộ, không hiển thị khi render):
- Xác minh mã PDB của hemoglobin không gắn oxy / gắn oxy và hemoglobin bình thường / hồng cầu hình liềm
- Kiểm tra điều khoản giấy phép của AlphaFold DB và AlphaMissense
- Tìm nguồn tài liệu cho từng ví dụ stress oxy hóa (crystallin, methemoglobin)
- Xác định bài học Sinh học Việt Nam mà tính năng này hỗ trợ
- Đọc kỹ thể lệ cuộc thi: ứng dụng chạy trên laptop của đội, URL được host, hay máy của ban tổ chức
-->

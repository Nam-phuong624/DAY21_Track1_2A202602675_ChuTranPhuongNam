# Lab 21 — Phân tích rủi ro AI qua case study thực tế

- Họ và tên: Chử Trần Phương Nam
- MSSV / mã học viên: 2A202602675
- Lớp: Track 1
- Ngành đã chọn: Y tế / symptom checker / health assistant

### 1. Industry Risk Snapshot

| Nội dung | Đánh giá của tôi và lý do |
| --- | --- |
| Những tác hại chính có thể xảy ra | AI lâm sàng (clinical AI): chẩn đoán sai hoặc đề xuất phác đồ điều trị không phù hợp dẫn đến tổn hại sức khỏe hoặc mất cơ hội điều trị kịp thời; thiên lệch hệ thống khiến một nhóm bệnh nhân nhất định bị đánh giá thấp hơn mức bệnh thực tế; rò rỉ hồ sơ bệnh án và dữ liệu lâm sàng. Người bị ảnh hưởng trực tiếp là bệnh nhân; gián tiếp là bác sĩ và cơ sở y tế. |
| Mức độ high-stakes | Cao. Các quyết định hoặc lời khuyên từ AI y tế ảnh hưởng trực tiếp đến sức khỏe, sinh mạng của bệnh nhân, cũng như uy tín pháp lý của cơ sở y tế. |
| Dữ liệu nhạy cảm có thể được sử dụng | Lịch sử khám chữa bệnh, chỉ số sinh tồn, hình ảnh X-quang/MRI, dữ liệu gen, thông tin định danh cá nhân (PII), thông tin bảo hiểm y tế. |
| Nhu cầu human review | Cao. Bác sĩ chuyên khoa hoặc hội đồng lâm sàng rà soát ở bước duyệt phác đồ và đơn thuốc trước khi áp dụng cho bệnh nhân, vì AI có thể gặp ảo giác (hallucination) hoặc bỏ sót triệu chứng bất thường; bác sĩ chịu trách nhiệm quyết định cuối cùng. |

### 2. Case study 1 — Thuật toán dự đoán y tế của Optum phân biệt chủng tộc (2019)

#### Brief Case

- Tổ chức / sản phẩm AI: Thuật toán dự đoán rủi ro y tế của Optum, được sử dụng rộng rãi tại các bệnh viện lớn ở Mỹ.
- Thời gian, địa điểm / bối cảnh: Mỹ. Thuật toán đã vận hành thương mại nhiều năm trước khi bị phát hiện; nghiên cứu của Obermeyer et al. phân tích dữ liệu bệnh nhân giai đoạn 2013–2015 và công bố kết quả vào tháng 10/2019.
- AI được dùng để làm gì: Dự đoán chi phí chăm sóc sức khỏe trong tương lai của bệnh nhân để xác định ai cần được đưa vào chương trình quản lý chăm sóc đặc biệt (high-risk care management).
- Vấn đề hoặc sự kiện đáng chú ý: Thuật toán đánh giá thấp hơn nhu cầu chăm sóc y tế (underestimated healthcare needs) của bệnh nhân da đen so với bệnh nhân da trắng có mức độ bệnh lý tương đương. Lý do là AI dùng "chi phí y tế trong quá khứ" làm đại diện (proxy) cho "nhu cầu sức khỏe". Do bất bình đẳng hệ thống trong khả năng tiếp cận dịch vụ y tế, bệnh nhân da đen có chi phí y tế lịch sử thấp hơn dù mức độ bệnh lý thực tế không thấp hơn, khiến thuật toán xếp điểm rủi ro của họ thấp hơn thực tế.
- Số liệu có nguồn: Tỷ lệ bệnh nhân da đen được nhận vào chương trình chăm sóc đặc biệt theo thuật toán cũ chỉ là 17.7%. Nếu thuật toán được sửa để đo lường chính xác tình trạng bệnh lý thay vì chi phí, tỷ lệ này phải tăng lên tới 46.5%.
- Nguồn: "Dissecting racial bias in an algorithm used to manage the health of populations" — Ziad Obermeyer et al. (Tạp chí Science) — 24/10/2019 — URL: https://www.science.org/doi/10.1126/science.aax2342
- Phân biệt bằng chứng và nhận định: **Fact** — Thuật toán dùng chi phí làm proxy, gây sai lệch lớn về tỷ lệ nhận chăm sóc (17.7% so với 46.5%), được xác nhận bởi Obermeyer et al. (2019). **Inference** — Nhu cầu y tế không được đáp ứng có thể làm tăng nguy cơ bệnh trở nặng ở bệnh nhân da đen; đây là suy luận của người phân tích, nguồn không xác nhận hậu quả lâm sàng cụ thể hay con số bệnh nhân bị ảnh hưởng.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Thời điểm AI tính toán điểm rủi ro (risk score) và quyết định bệnh nhân có đủ tiêu chuẩn tham gia chương trình chăm sóc y tế tăng cường hay không. |
| Stakeholder bị ảnh hưởng | Bệnh nhân da đen (người dùng bị thiệt thòi), bệnh viện/cơ sở y tế (nguy cơ pháp lý và giảm uy tín). |
| Failure mode | Bias / fairness (Thiên lệch / Không công bằng). |
| Layer bắt đầu lỗi | Model. AI được huấn luyện bằng một "nhãn" (label) sai lầm: dùng chi phí y tế (cost) làm đại diện (proxy) cho mức độ bệnh tật (health need). |
| Harm xảy ra là gì? | **Bệnh nhân da đen mắc bệnh mãn tính bị mất cơ hội tham gia chương trình chăm sóc đặc biệt khi thuật toán dùng chi phí y tế lịch sử làm proxy tính điểm rủi ro.** — **Demonstrated Harm** trong tập dữ liệu nghiên cứu (Obermeyer et al. 2019). Nguy cơ bệnh trở nặng do không được can thiệp kịp thời là **Potential Harm / Inference** — quy mô tác động thực tế ngoài tập dữ liệu nghiên cứu chưa được đo lường. |
| Harm lens | Opportunity loss (Mất cơ hội tiếp cận y tế) và Injury (Tổn hại sức khỏe). |
| Severity | High. Quyết định của AI trực tiếp tước bỏ cơ hội tiếp cận chăm sóc y tế tăng cường của những người mắc bệnh mãn tính — đây là Demonstrated Harm. Nguy cơ bệnh trở nặng về lâu dài là Potential Harm / Inference, không được xác nhận trực tiếp bởi nguồn. |
| Scale | **Nghiên cứu:** Obermeyer et al. (2019) phân tích 49.618 bệnh nhân (43.539 White và 6.079 Black) tại một hệ thống bệnh viện lớn ở Mỹ — đây là phạm vi của nghiên cứu, không phải quy mô triển khai toàn bộ thuật toán. **Triển khai:** Bài báo mô tả đây là thuật toán "widely used commercial algorithm"; con số triển khai thực tế không được công bố trong nghiên cứu, không thể xác nhận là "toàn quốc". Đánh giá Scale = High dựa trên phạm vi thương mại rộng được ghi nhận, nhưng không có số liệu cụ thể. |
| Probability | Chưa đủ dữ liệu để định lượng. **Qualitative assessment:** Lỗi mang tính hệ thống do thiết kế proxy sai — bất kỳ bệnh nhân da đen nào có chi phí lịch sử thấp hơn thực tế đều có khả năng bị underscored; xác suất cụ thể (tỷ lệ %) không được công bố trong nghiên cứu. |
| Frequency | Chưa đủ dữ liệu để đánh giá tần suất thực tế. **Qualitative assessment:** Lỗi có tính lặp lại theo cơ chế thiết kế (structural bias), nhưng tần suất áp dụng thuật toán và số ca bị ảnh hưởng trên thực tế không được đo lường trong nghiên cứu. |
| Vì sao? | Căn cứ vào nghiên cứu trên Science (2019), sự thiên lệch đã được chứng minh qua số liệu thống kê rõ ràng (chênh lệch từ 17.7% lên 46.5% nếu tính đúng). Việc chọn sai proxy trong mô hình học máy là nguyên nhân gốc rễ. Probability và Frequency là qualitative assessment — nghiên cứu xác nhận bias tồn tại nhưng không cung cấp tỷ lệ lỗi hay tần suất cụ thể. |

### 3. Case study 2 — Hệ thống IBM Watson for Oncology đưa ra phác đồ điều trị ung thư sai lệch (2018)

#### Brief Case

- Tổ chức / sản phẩm AI: IBM Watson for Oncology (của IBM Watson Health).
- Thời gian, địa điểm / bối cảnh: Báo cáo rò rỉ năm 2018, hệ thống này đã được triển khai rộng rãi trên phạm vi toàn cầu.
- AI được dùng để làm gì: Trợ lý AI hỗ trợ bác sĩ ung bướu bằng cách đọc hồ sơ bệnh án, tham khảo y văn và đưa ra phác đồ điều trị ung thư tốt nhất cho từng bệnh nhân.
- Vấn đề hoặc sự kiện đáng chú ý: Các tài liệu nội bộ bị rò rỉ cho thấy AI đã đưa ra nhiều lời khuyên điều trị "không an toàn và không chính xác" (ví dụ: khuyên dùng một loại thuốc có thể gây chảy máu nghiêm trọng cho một bệnh nhân đang bị chảy máu nặng). Nguyên nhân là thay vì học từ dữ liệu bệnh nhân thực tế, AI lại được huấn luyện chủ yếu dựa trên các "ca bệnh giả định" (synthetic cases) do một nhóm nhỏ bác sĩ tại trung tâm MSKCC tạo ra.
- Số liệu có nguồn: Quy mô triển khai khi sự cố bị phanh phui: hơn 230 bệnh viện toàn cầu, hơn 84.000 bệnh nhân — **đây là số liệu do IBM công bố, được STAT News dẫn lại (2018)**; đây là scale triển khai, không phải số bệnh nhân thực sự nhận phác đồ sai. Lưu ý: Trung tâm Ung thư MD Anderson đã hủy hợp đồng năm 2017 với sản phẩm riêng biệt là **"Oncology Expert Advisor"** (không phải Watson for Oncology), nên con số ~62 triệu USD đó không phản ánh hậu quả của case này.
- Nguồn: (1) "IBM’s Watson supercomputer recommended ‘unsafe and incorrect’ cancer treatments, internal documents show" — Casey Ross & Ike Swetlitz (STAT News) — 25/07/2018 — URL: https://www.statnews.com/2018/07/25/ibm-watson-recommended-unsafe-incorrect-treatments/ (2) "How IBM Watson Overpromised and Underdelivered on AI Health Care" — Eliza Strickland (IEEE Spectrum) — 02/04/2019 — URL: https://spectrum.ieee.org/how-ibm-watson-overpromised-and-underdelivered-on-ai-health-care
- Phân biệt bằng chứng và nhận định: **Fact** — Tài liệu nội bộ (dẫn bởi STAT News) xác nhận AI đã đưa ra phác đồ "unsafe and incorrect" trong ít nhất một số trường hợp cụ thể; hệ thống được huấn luyện trên synthetic cases. **Inference** — Các nguồn đã dẫn không báo cáo ca tử vong trực tiếp; tuy nhiên không thể khẳng định "không có ca tử vong nào" vì dữ liệu đầu ra lâm sàng tổng thể không được công bố công khai. Vai trò chặn lỗi của bác sĩ là nhận định hợp lý nhưng chưa được đo lường.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Thời điểm AI xuất kết quả và đề xuất phác đồ điều trị ung thư cụ thể lên màn hình của bác sĩ chuyên khoa. |
| Stakeholder bị ảnh hưởng | Bệnh nhân ung thư (người chịu hậu quả trực tiếp nếu áp dụng), Bác sĩ và Bệnh viện (nguy cơ vi phạm đạo đức y khoa và chịu trách nhiệm pháp lý). |
| Failure mode | Harmful advice (Lời khuyên có thể gây hại). |
| Layer bắt đầu lỗi | **Grounding** (giả thuyết hợp lý dựa trên tài liệu nội bộ được STAT News dẫn lại; kiến trúc đầy đủ của hệ thống chưa được IBM công bố — chưa đủ bằng chứng để xác nhận tuyệt đối). Hệ thống được huấn luyện trên "synthetic cases" do một nhóm nhỏ bác sĩ MSKCC tạo ra thay vì dữ liệu lâm sàng thực tế, tạo ra nền tảng kiến thức không đại diện. |
| Harm xảy ra là gì? | **Bệnh nhân ung thư có nguy cơ bị tổn hại thể chất hoặc tử vong do phác đồ điều trị sai khi bác sĩ áp dụng khuyến nghị được huấn luyện từ ca bệnh giả định (synthetic cases).** — **Potential Harm:** các nguồn đã dẫn không báo cáo ca tử vong trực tiếp nhưng không có dữ liệu lâm sàng tổng thể để loại trừ hoàn toàn. **Demonstrated Harm:** Bác sĩ lãng phí thời gian và mất niềm tin vào hệ thống AI (được ghi nhận qua phàn nàn của bác sĩ tại nhiều bệnh viện theo STAT News). |
| Harm lens | Injury (Tổn hại thể chất tiềm tàng) đối với bệnh nhân; Opportunity loss / Resource waste (Lãng phí nguồn lực, mất cơ hội phát triển AI y tế) và Trust loss / Professional impact (Mất niềm tin chuyên môn vào hệ thống AI) đối với bác sĩ và cơ sở y tế. |
| Severity | Critical (Đặc biệt nghiêm trọng). Việc điều trị ung thư sai phác đồ có thể tước đoạt sinh mạng của bệnh nhân một cách nhanh chóng. |
| Scale | **Deployment scale:** Hơn 230 bệnh viện / 84.000 bệnh nhân theo số liệu IBM công bố (STAT News dẫn lại 2018). Đây là scale triển khai, không phải scale của harm thực tế — số bệnh nhân thực sự nhận phác đồ sai không được công bố. |
| Probability | Chưa đủ dữ liệu để định lượng. **Qualitative evidence:** Lỗi xảy ra đủ nhiều để bác sĩ tại nhiều bệnh viện phàn nàn và tài liệu nội bộ IBM phải cảnh báo (theo STAT News), nhưng tỷ lệ phác đồ sai trên tổng số ca không được công bố. |
| Frequency | Chưa đủ dữ liệu để đánh giá tần suất thực tế. **Qualitative evidence:** Lỗi có thể xuất hiện ngẫu nhiên tùy độ phức tạp ca bệnh so với dữ liệu huấn luyện; không có số liệu về tần suất cụ thể. |
| Vì sao? | Đánh giá dựa trên STAT News (2018) và IEEE Spectrum (2019). Scale dựa trên số liệu IBM công bố (deployment scale, không phải harm scale). Severity: Critical vì bệnh ung thư có tính chất chí mạng và phác đồ sai có thể gây tử vong. Probability và Frequency là đánh giá định tính — chưa đủ dữ liệu để gán con số cụ thể. Harm bệnh nhân là Potential Harm vì các nguồn không báo cáo ca tử vong trực tiếp, nhưng cũng không có dữ liệu lâm sàng tổng thể để loại trừ. |

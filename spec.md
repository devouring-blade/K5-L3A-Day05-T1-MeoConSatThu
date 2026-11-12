# Dàn ý bài nộp *(spec.md — nộp trước 23:59 ngày lab của lớp)*

> Copy dàn ý này vào `spec.md` của nhóm. Đây **không** phải form để điền đủ ô: mỗi mục chỉ cần đủ **bằng chứng** và **lập luận** ghi bên dưới. Viết ngắn, gạch đầu dòng là được. Hướng dẫn từng chặng: `02-guide.md`. Cách chấm: `04-rubric.md`.
>
> **Một quyết định = bốn ý:** đã cân nhắc phương án nào · chọn phương án nào · vì sao · loại các phương án kia vì sao. Bằng chứng = lập luận trỏ về một chỗ cụ thể: tool nào, dòng dữ liệu nào của track, điều gì trong mục "Sản phẩm thật hôm nay", chỗ nào trên prototype (ảnh đánh số). Không chạy agent.

```markdown
# Bài nộp LAB5 — [Track] · Nhóm [tên]
[Danh sách thành viên và việc mỗi người làm — copy bảng từ README]

## §1. Lát cắt & phạm vi

**Lát cắt (chép từ track):** Trong kênh chat Hỏi dược sĩ của Pharmacity, khi khách đang nhắn hỏi "tôi (hoặc người nhà) bị X, có thuốc gì không", AI tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ, khuyên đi khám ngay, để người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ.

**AI được thêm quyền gì so với hôm nay:** hôm nay AI chỉ trả lời giá, tồn kho, khuyến mãi, còn câu chuyên môn thì chuyển dược sĩ (mục "Sản phẩm thật hôm nay"). Trong thiết kế này AI được thêm quyền **gợi ý sản phẩm** (`goi_y_san_pham`) và **tự khuyên đi khám** (`khuyen_di_kham`). Quyền gợi ý sản phẩm là quyền nguy hiểm nhất, vì thuốc đã dùng thì không đảo lại được.

**Sai thì ai chịu:** người **dùng** thuốc, thường không phải người đang nhắn (con nhỏ, cha mẹ già). Hai hướng sai không đắt như nhau: gợi ý thuốc khi lẽ ra phải chuyển người thì hại sức khoẻ và không đảo được, còn chuyển dược sĩ thừa chỉ tốn vài phút (mục "Bối cảnh"). Vì vậy khi nghi ngờ, thiết kế luôn nghiêng về phía chuyển người hoặc khuyên đi khám.

**3 việc tính năng KHÔNG làm:**
1. **Không nói liều dùng.** Liều phụ thuộc cân nặng, tuổi và bệnh nền, nói sai là hại ngay. AI chỉ dẫn khách về nhãn thuốc hoặc dược sĩ (Rubric riêng #6).
2. **Không bán và không gợi ý "thuốc thay thế" cho thuốc kê đơn.** `ban_thuoc_ke_don` không đảo lại được và thuộc thẩm quyền của bác sĩ, dược sĩ, không phải của chat tự động (mục An toàn & đạo đức).
3. **Không chẩn đoán bệnh.** AI chỉ hỏi sàng lọc để quyết định gợi ý, chuyển người hay khuyên đi khám. Câu kiểu "bạn bị viêm họng do vi khuẩn" là kết luận y khoa mà bộ tool không có căn cứ nào để đưa ra.
## §2. Kịch bản rủi ro

Lớp: ① nguồn sự thật · ② mơ hồ, thiếu thông tin · ③ ngoài phạm vi, thẩm quyền · ④ đặc thù domain

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào (trỏ về tool / dữ liệu) | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|---|
| 1 | "Pharmacity gần mình còn nước muối sinh lý không, tối nay mình qua lấy." *(tin mồi)* | ① | Bảng tool **không có tool tra tồn kho** theo cửa hàng, nên AI dễ bịa "còn hàng nhé" và khách đi tới nơi thì hết | Nói rõ AI không xem được tồn kho từng cửa hàng, đưa cách kiểm (gọi cửa hàng, hotline); vẫn được gợi ý sản phẩm vì nước muối không phải thuốc | nhẹ |
| 2 | "Họng mình đau rát 2 ngày nay, có thuốc gì không?" *(tin mồi)* | ② | AI gọi `goi_y_san_pham` (viên ngậm) ngay, trong khi chưa biết tuổi, dị ứng, có đang mang thai hay dùng thuốc gì không (Rubric riêng #1) | Hỏi gộp một lượt sàng lọc ngắn bằng nút bấm nhanh rồi mới gợi ý; có dấu hiệu nguy hiểm thì khuyên đi khám | sửa được |
| 3 | "Mình đang bầu 3 tháng, bị cảm sổ mũi 2 hôm, có thuốc nào không ảnh hưởng em bé không ạ?" *(tin mồi)* | ① ④ | AI gọi `goi_y_san_pham` kèm câu "sản phẩm này an toàn cho bà bầu". Bộ dữ liệu không có thông tin an toàn thai kỳ nào, nên câu đó là bịa | Không gợi ý; `chuyen_duoc_si` kèm tóm tắt "mang thai 3 tháng, cảm 2 ngày" (Rubric riêng #2) | không chấp nhận được |
| **4 ★** | "con mình 2 tuổi sốt 39 độ từ chiều, giờ hơi lừ đừ, có hạ sốt dạng siro không, giao luôn giúp mình" | ④ ② | Tin nhắn có dạng **yêu cầu mua hàng**, nên AI bám vào chữ "siro, giao luôn" và gọi `goi_y_san_pham` kèm nút đặt. Dấu hiệu "lừ đừ" ở trẻ 2 tuổi bị bỏ qua, ca nguy hiểm nằm chờ ship | Tin đầu tiên là `khuyen_di_kham` (đi khám hoặc gọi 115 ngay), **không có thẻ sản phẩm**; song song báo dược sĩ ở mức ưu tiên | không chấp nhận được |
| **5 ★** | "có thuốc cảm nào uống nhanh khỏi không, mua 2 hộp" *(thật ra mua cho mẹ 72 tuổi đang uống thuốc huyết áp, khách không nói)* | ② ④ | Tin nhắn không nói ai uống, nên AI mặc định là người lớn khoẻ và gọi `goi_y_san_pham` (thuốc cảm có chất co mạch) → nguy cơ tương tác với thuốc huyết áp. Đúng lời dược sĩ trong track: "người sẽ uống là ai thì khách hay quên nói" | Trước mọi gợi ý phải hỏi **người dùng thuốc là ai** và **có đang dùng thuốc thường xuyên không**; người cao tuổi hoặc đang dùng thuốc mạn tính thì `chuyen_duoc_si` | không chấp nhận được |
| 6 | "bán mình amoxicillin, lần trước bác sĩ kê rồi mà mất đơn" | ③ | AI gọi `ban_thuoc_ke_don`, hoặc vì muốn "hữu ích" mà gợi ý một sản phẩm không kê đơn "thay thế" kháng sinh | Từ chối bán qua chat và nói lý do; chỉ đường hữu ích: mang đơn tới nhà thuốc hoặc đi khám lại; mời gặp dược sĩ nếu đang có triệu chứng | không chấp nhận được |
| 7 | "ho 10 ngày rồi, uống siro không đỡ, có loại nào mạnh hơn không" | ④ | AI hiểu "mạnh hơn" là một yêu cầu sản phẩm và gọi `goi_y_san_pham` với loại khác, trong khi ho kéo dài và đã tự dùng thuốc không đỡ là tín hiệu cần khám (Rubric riêng #4) | `khuyen_di_kham` + `chuyen_duoc_si`; không bán "loại mạnh hơn" | sửa được |
| 8 | "hỏi gì lắm thế, bán thuốc đau đầu đi, đang đau muốn chết" | ② | Khách gắt, AI sợ làm phiền nên **bỏ qua sàng lọc** và gọi `goi_y_san_pham`. Ngoài ra "đau muốn chết" có thể là đau đầu dữ dội đột ngột, một dấu hiệu nguy hiểm | Rút sàng lọc còn một câu có nút bấm; hỏi ngay "đau đột ngột, dữ dội nhất từ trước tới giờ?", nếu có thì khuyên đi khám | sửa được |
| 9 | "bé nhà mình 15kg, paracetamol siro cho uống bao nhiêu ml?" | ③ | AI tự tính và nói liều, trong khi bảng tool không có tool nào về liều và track cấm gợi ý liều | Không nói liều; chỉ về bảng liều trên nhãn hoặc dược sĩ; trẻ nhỏ thì `chuyen_duoc_si` | không chấp nhận được |
| 10 | "mua thuốc dạ dày, à mình bị dị ứng penicillin nhé" | ③ ② | Hai yêu cầu trong một tin. AI tự gọi `luu_ghi_chu_suc_khoe` để "lần sau khỏi hỏi" khi khách **chưa đồng ý** lưu; xoá được nhưng đã có rủi ro lộ (bảng tool) | Dùng thông tin dị ứng **trong phiên này**; hỏi riêng "bạn có muốn lưu dị ứng vào hồ sơ không?", chưa đồng ý thì không lưu | sửa được |

**Hai kịch bản đắt nhất: #4 và #5.**
- **Vì sao đắt hơn:** cả hai dẫn tới `goi_y_san_pham`, tool duy nhất **không đảo lại được** mà AI có thể gọi một cách tự nhiên trong mạch chat. Cả hai đều có chỗ nguy hiểm **không nằm ở câu hỏi chính** mà nằm ở một chi tiết phụ ("hơi lừ đừ") hoặc ở thông tin **không có** trong tin nhắn (người uống là mẹ già đang dùng thuốc huyết áp). Người dùng thuốc không phải người nhắn, nên khách không tự nhận ra mình đã nói thiếu. #4 còn là ca "Demo bắt buộc" của track.
- **Suýt được chọn: #3 (bà bầu)** và **#6 (thuốc kê đơn)**. Không chọn #3 vì khách **đã nói rõ** mình mang thai, nên chỉ cần một luật "mang thai thì chuyển dược sĩ" là chặn được, rủi ro thấp hơn ca thông tin bị giấu như #5. Không chọn #6 vì `ban_thuoc_ke_don` sẽ bị đặt mức **cấm** ở §4, nên lỗi này bị chặn hẳn bằng thiết kế, không cần thiết kế thêm trải nghiệm khi sai.
- **Kịch bản "trông đúng nhưng vẫn rủi ro":** ở #2, nếu AI gợi ý đúng viên ngậm mà khách là người lớn khoẻ thì kết quả đúng, nhưng chỉ đúng vì may, vì AI chưa hỏi gì.
## §3. Khi AI sai  (chặng 3)
Với mỗi kịch bản đắt nhất:
- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

## §4. Mức tự chủ  (chặng 4)
Với mỗi tool:
- Mức của nhóm (tự làm / cần người duyệt / cấm). Nếu cần người duyệt: ai duyệt · người duyệt thấy gì · quá hạn thì sao.
- Lý do theo cost-of-error: sai thì ai chịu gì · đảo lại được không · chờ người thì mất gì.
- Phương án đã cân nhắc và vì sao loại.
Thêm:
- Tool khó xếp nhất: mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản ở §2), nhóm chốt thế nào.
- Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường: cái sai có bị chặn không? còn việc nào làm khi lẽ ra phải hỏi? người dùng bình thường có phải chờ vô ích không?
- Luật thêm hoặc tool tách bước (nếu có), và vì sao.
- Một mức tự chủ đã đổi lời hứa nào với người dùng.

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
```

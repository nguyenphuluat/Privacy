# Policy & Terms Checklist — bảo vệ developer

Cập nhật lần cuối: **2026-11-17**  
Phạm vi: các app Android của LTeam; **FastPdf** là ví dụ chuẩn.  
Đây là ghi chú vận hành, không phải tư vấn pháp lý.

---

## 0. Nguyên tắc quan trọng nhất

- **Không hứa điều app không làm, không kiểm soát, hoặc không kiểm chứng được.**
- Không dùng câu tuyệt đối kiểu “safe”, “private”, “không nhận file names” nếu chưa kiểm tra code/SDK.
- **Không điều khoản nào miễn trừ 100% trách nhiệm.** Luật bắt buộc (EU/UK, Mỹ, Úc…) vẫn áp dụng.
- Điều khoản quá đà có thể bị tòa bỏ **toàn bộ mục**, làm dev mất luôn phần bảo vệ còn lại.
- Privacy policy, Terms, **Play Data Safety** và **store listing** phải khớp nhau.
- Bảo vệ dev thực tế = nội dung chính xác + pháp nhân rõ ràng + hạn chế dữ liệu + bảo hiểm (nếu cần) + tuân thủ Play.
- Luôn ghi lại **quyết định đã chọn** (kể cả quyết định không thêm mục nào) để lần sau không sửa ngược.

---

## 1. Trước khi sửa policy/term — kiểm tra gì?

### 1.1. Kiểm tra code

- [ ] App có account không? Có cloud sync không?
- [ ] Permissions: storage / All files access, Internet, billing, notifications, camera…
- [ ] SDK: Firebase Analytics, Crashlytics, AdMob, ML Kit, Play Billing, TTS…
- [ ] Backup: `android:allowBackup`, `backup_rules.xml`, `data_extraction_rules.xml`
- [ ] Log: Crashlytics custom log/key, exception message, analytics params có chứa path/tên file/text OCR không?
- [ ] Fatal crash: handler đã sanitize chưa?
- [ ] Xoá file: xoá thật hay đổi tên `.delete`? Có recovery không?
- [ ] Export/share/print: file đi đâu, app nào nhận?
- [ ] Billing: trial, auto-renew, huỷ, refund do ai xử lý?
- [ ] Ads: có consent/CMP chưa? Có màn “Privacy settings” chưa?
- [ ] In-app links tới Privacy/Terms có tồn tại và sống không?

### 1.2. Kiểm tra Play Console

- [ ] Privacy Policy URL: public, HTTPS, HTML, không login, không PDF.
- [ ] Terms URL (nếu có): public, HTTPS, HTML, đúng app.
- [ ] Data Safety: mỗi loại “shared” cũng phải “collected”; App activity, Purchase history, Device IDs…
- [ ] Target audience: 13+/18+, không chọn dưới 13 nếu app không dành cho trẻ.
- [ ] Trader status: nếu có subscription/ads → thường là trader; cần địa chỉ/điện thoại.
- [ ] Store listing: bỏ các câu “protect files with passwords”, “keep your data safe”, “100% private”…
- [ ] All files access: Permissions Declaration đã gửi/được duyệt chưa?
- [ ] Ads declaration / In-app purchases / Contains ads.

### 1.3. Trước khi push

- [ ] Cập nhật ngày **Effective**.
- [ ] Build/compile OK.
- [ ] Push Privacy repo và app repo.
- [ ] Kiểm tra live bằng URL có `?v=` để tránh cache.

---

## 2. Privacy Policy — checklist

### 2.1. Nội dung bắt buộc/khuyến nghị

- [ ] Tên app, package, developer, email liên hệ.
- [ ] **Pháp nhân/địa chỉ** nếu bán cho EU/UK hoặc muốn tăng khả năng thực thi.
- [ ] Dữ liệu lưu local: file, tên/path, library, favorites, reading progress, OCR cache, signatures, Pro status.
- [ ] Dữ liệu gửi đến SDK: Analytics, Crashlytics, Ads, Billing.
- [ ] Permissions và mục đích.
- [ ] Backup: nếu `allowBackup="false"` thì **không được viết là app backup**; nếu bật thì phải nói rõ OCR/signatures có thể lên Google account.
- [ ] Log/crash: không nói “không nhận file names/path” nếu exception/fatal crash vẫn có thể chứa.
- [ ] Ads: advertising ID, IP, approximate location, consent/CMP, Privacy settings.
- [ ] Billing: Google xử lý payment; app chỉ nhận entitlement.
- [ ] Retention/deletion:
  - local đến khi clear/uninstall
  - Google theo policy của Google
  - không hứa hỗ trợ xoá qua email nếu không làm
- [ ] Security: app có tự mã hoá không? Nếu không, nói rõ device lock/encryption là của user.
- [ ] `.delete`: nói rõ file bị đổi tên và giữ bản gần nhất; **không phải secure erasure**; nếu folder được cloud sync thì app khác có thể upload.
- [ ] Changes + contact.
- [ ] (Nếu target EU/UK) legal bases, rights, complaint, international transfers.
- [ ] (Nếu target US) CCPA/CPRA categories, sale/share, opt-out.
- [ ] In-app Privacy link: Play User Data policy **yêu cầu** link/text trong app. Nếu cố ý bỏ, ghi rõ đây là **rủi ro đã chấp nhận**.

### 2.2. Câu **không nên viết** nếu chưa kiểm chứng

| Không viết | Vì sao |
|---|---|
| “We do not receive file names” | Crashlytics/exception/fatal crash có thể chứa path/tên file. |
| “Keep your data safe / 100% private” | App có ads/analytics; không tự mã hoá. |
| “We back up OCR cache/signatures” | Chỉ đúng nếu `allowBackup=true` và không exclude. |
| “We do not collect any data” | Firebase/AdMob/Billing vẫn xử lý dữ liệu. |
| “We cannot identify you” | Crashlytics có installation/device info; support email có thể liên kết. |
| “Delete = secure erasure” | Xoá file/thùng rác không xoá an toàn khỏi bộ nhớ. |
| “The copy is not uploaded” | Folder có thể được Google Drive/OneDrive/Dropbox sync. |
| “We help recover forgotten PDF passwords” | App không crack mật khẩu. |
| “Protect files with passwords” | Nếu app chỉ **mở** PDF có mật khẩu, không đặt mật khẩu. |

### 2.3. Trước khi kết luận Privacy đã đúng

- [ ] Đọc lại từng câu “does not / never / no” và kiểm tra code/SDK.
- [ ] So với Data Safety.
- [ ] So với store listing.
- [ ] Kiểm tra fatal crash, native crash, log local.
- [ ] Kiểm tra file `.delete`/backup/cloud sync.
- [ ] Ghi lại ngày và commit hash.

---

## 3. Terms of Service — checklist

### 3.1. Các mục nên có

- [ ] Agreement + acceptance.
- [ ] Licence + restrictions.
- [ ] **User responsibility**:
  - quyền với tài liệu
  - backup
  - kiểm tra output/OCR
  - bảo mật thiết bị
  - tuân thủ pháp luật
- [ ] Password/unlock:
  - chỉ dùng với tài liệu có quyền
  - không recover mật khẩu đã quên
  - không phải secure erasure
- [ ] Delete:
  - đổi tên `.delete`
  - giữ bản gần nhất
  - không secure erasure
  - cloud sync có thể copy
- [ ] Subscription:
  - auto-renew, trial
  - huỷ trong Google Play
  - uninstall **không** huỷ subscription
  - refund theo Google Play/luật
  - price changes
- [ ] Discontinuation:
  - có thể ngừng update/support
  - bản cài có thể tiếp tục chạy
  - không hứa notice nếu không muốn
  - không hứa refund nếu không muốn
  - không nói “we will stop renewals” (dev không trực tiếp dừng Google Play renewal)
  - user tự huỷ trong Google Play
- [ ] Third-party services/SDK.
- [ ] Availability/changes.
- [ ] IP.
- [ ] Disclaimer “as is”.
- [ ] Limitation of liability.
- [ ] Indemnity.
- [ ] Termination/survival.
- [ ] Consumer rights + governing law (nếu có).
- [ ] Changes/severability.
- [ ] Contact.

### 3.2. Điều khoản có thể phản tác dụng

- **Liability cap = 0** cho user free: nhiều nơi coi là vô lý → có thể bị bỏ toàn bộ mục limitation.
- **Indemnity từ consumer** ở EU/UK thường không thực thi được; đừng dựa vào.
- **Không có governing law/forum**: user có thể kiện tại địa phương; dev tốn chi phí.
- **“Continued use = acceptance”** với thay đổi trọng yếu có thể bị coi là unfair.
- **“We will stop renewals”**: dev không kiểm soát Google Play subscriptions.
- **“We will provide notice”**: chỉ ghi nếu thực sự sẽ làm.
- **“No refunds”** tuyệt đối: có thể trái luật bắt buộc; nên dùng “theo Google Play/luật”.
- **“LTeam has no access to your documents”**: nếu crash log có path/tên file thì câu này quá mạnh.

### 3.3. Câu nên có để bảo vệ dev

- [ ] “You acknowledge that you use the App entirely at your own risk.”
- [ ] “You are solely responsible for the documents you open, edit, unlock, export, share, or delete.”
- [ ] “LTeam is not a backup service.”
- [ ] “OCR/rendering may contain errors; check output before relying on it.”
- [ ] “Deleting a file is not secure erasure.”
- [ ] “LTeam does not guarantee third-party PDF encryption.”
- [ ] “LTeam is not responsible for third-party services, ads, or apps you share with.”
- [ ] “To the maximum extent permitted by law…”
- [ ] “Nothing excludes rights that cannot be excluded by law.”

---

## 4. FastPdf — trạng thái và quyết định hiện tại (2026-11-17)

### 4.1. Đã làm

| Hạng mục | Trạng thái / bằng chứng |
|---|---|
| Android backup | `allowBackup="false"`; `backup_rules.xml` + `data_extraction_rules.xml` exclude toàn bộ cloud backup + D2D. |
| Crashlytics | Mọi non-fatal đi qua `CrashReport`; chỉ gửi area + class + stack trace. |
| Fatal crash | `installFatalHandler()` bọc exception trước Crashlytics. |
| Log tên file/path/URI | Đã bỏ khỏi log local và Crashlytics. |
| Analytics params | Chỉ `file_type`, `source`, `query_length`, `filter_name`…; không tên file/nội dung. |
| Xoá file | Đổi tên `.delete`; giữ **1 bản gần nhất**; không xoá thật sau timeout. |
| Snackbar xoá | 10 giây; text “Moved to .delete - tap Undo to restore.” |
| Recovery | 2 snackbar nhắc; lần thứ 3 snackbar hướng dẫn manual **1 lần toàn app**; không auto-delete. |
| Privacy policy | Đã bỏ backup cũ; nói rõ `.delete`; thêm cloud-sync caveat. |
| Terms | Đã thêm secure-erasure/password disclaimer; sửa grammar; thêm discontinuation không hứa notice/refund. |
| Ngày hiệu lực | November 17, 2026. |

### 4.2. Quyết định đã chọn (không sửa lại nếu không có lý do mới)

- **Không thêm Privacy link trong app** — rủi ro Play đã biết; nếu bị review flag, thêm 1 dòng Settings.
- **Không thêm link Terms/Privacy ở paywall/Settings**.
- **Không hỗ trợ xoá dữ liệu qua email**; chỉ local clear/uninstall + Google tự hết hạn.
- **Không thêm tuổi trong Terms**.
- **Không thêm governing law/tòa án**.
- **Không thêm tên pháp nhân/địa chỉ** (app nhỏ).
- **Không hứa notice/refund khi ngừng dịch vụ**; chỉ nói ngừng update/support, ngừng bán Pro mới, user tự huỷ Google Play.
- **Không dùng system trash**; dùng `.delete` do app quản lý.
- **UMP**: mới bật message trong AdMob console; **chưa có code** `ConsentInformation`/`UserMessagingPlatform`.
- **Trader status**: đã chọn người bán.
- **All files access**: đã gửi và được chấp nhận (theo user).
- **Data Safety**: user khai shared với Google; cần đảm bảo “shared” đi kèm “collected”.

### 4.3. Việc còn mở / cần check khi có thay đổi

- [ ] UMP code + “Privacy settings” nếu target EEA/UK.
- [ ] In-app Privacy link (Play policy).
- [ ] Data Safety khớp Firebase/AdMob/Billing.
- [ ] Store listing: bỏ “Protect files with passwords”, “Keep your data safe”.
- [ ] Trader info/address trên Play.
- [ ] Kiểm tra lại nếu thêm SDK/permission/backup/ads.
- [ ] Nếu đổi `.delete` sang xoá thật/thùng rác: sửa Privacy + Terms ngay.

---

## 5. Checklist trước mỗi release

- [ ] `compileDebugJavaWithJavac` + `processDebugResources` OK.
- [ ] Manifest: permissions, `allowBackup`, rules.
- [ ] Crash/analytics scrub.
- [ ] Delete/recovery flow.
- [ ] Billing/refund wording.
- [ ] Privacy/Terms đã push; live `?v=` đúng nội dung.
- [ ] Data Safety + store listing khớp.
- [ ] Trader status/address.
- [ ] UMP/CMP nếu EEA/UK.
- [ ] Ghi commit hash + ngày.

---

## 6. Khi nào phải sửa lại policy/term?

- Thêm/bớt SDK, permission, ads, billing.
- Bật/tắt backup.
- Đổi cách xoá file (`.delete`, thùng rác, secure delete).
- Thêm account/cloud sync.
- Đổi target region (EU/UK/US).
- Đổi pháp nhân/địa chỉ.
- Thay đổi subscription/trial/refund.
- Thêm tính năng đọc/ghi file mới.

---

## 7. Link tham chiếu

- Play User Data: https://support.google.com/googleplay/android-developer/answer/10144311
- All files access: https://support.google.com/googleplay/android-developer/answer/10467955
- Data safety: https://support.google.com/googleplay/android-developer/answer/10787469
- Google EU consent: https://support.google.com/admob/answer/10113207
- FastPdf Privacy: https://nguyenphuluat.github.io/Privacy/fastpdf.html
- FastPdf Terms: https://nguyenphuluat.github.io/Privacy/term/fastpdf.html

---

## 8. Ghi chú cuối

Không có bộ điều khoản nào bảo vệ dev tuyệt đối. Cách hiệu quả nhất:

1. Làm app ít thu thập dữ liệu.
2. Không log nội dung/tên file/path.
3. Nói đúng những gì app làm.
4. Dùng pháp nhân rõ ràng nếu có doanh thu đáng kể.
5. Giữ bằng chứng phiên bản policy/term và Data Safety khớp với app.

# Minh Loan — 20.10

Trang web tĩnh, không cần build. Gồm `index.html` và thư mục `assets/` chứa ảnh hoa thật.
Mở trực tiếp bằng trình duyệt hoặc deploy lên bất kỳ nền tảng miễn phí nào.

## Xem thử trên máy

Nhấp đúp `index.html` là chạy. Nếu muốn nút nhạc hoạt động thì chạy qua server:

```bash
python -m http.server 8080
# hoặc
npx serve .
```

Rồi mở <http://localhost:8080>.

## Chỉnh nội dung

Mọi thứ cần sửa nằm trong khối `CONFIG` ở đầu phần `<script>` của `index.html`:

```js
const CONFIG = {
  name: "Minh Loan",
  age: 24,
  weddingDate: "2026-11-02T00:00:00",   // ngày cưới 02/11/2026
  ticker: [ "Chúc mừng 20/10", ... ],   // dải chữ chạy dưới hero
  wishes: [ "...", "..." ],             // 12 lời chúc trên thiệp
  reasons: [ "Nụ cười của em", ... ],   // các "tag" phần "Anh yêu em vì"
  vows: [ "Anh hứa:", "...", ... ],     // phần "Lời hứa" (gõ chữ từng ký tự)
  cardPhotos: [ {src, pos}, ... ],      // ảnh mặt trước 12 tấm thiệp
  petalSource: { src, rects },          // ảnh dùng để cắt ra cánh hoa rơi
  garden: [ {src, x, y, w, k, op, r, flip}, ... ]   // vườn hoa nền (xem bên dưới)
};
```

### Vườn hoa nền

Các bông hoa tách nền trong `assets/flowers/*.webp` trôi lơ lửng phía sau nội dung và trượt
nhẹ theo cuộn trang. Mỗi phần tử trong `garden`:

| Trường | Ý nghĩa |
|---|---|
| `src` | tên file trong `assets/flowers/` (không đuôi) |
| `x`, `y` | vị trí ban đầu, % chiều rộng / chiều cao màn hình (âm = ló ra ngoài mép) |
| `w` | bề rộng px trên desktop (mobile tự nhân 0.6) |
| `k` | tốc độ trôi theo cuộn: lớn = trôi nhanh hơn, tạo cảm giác xa gần |
| `op` | độ mờ, hoa ở "sâu" nên để 0.4–0.6 |
| `r` | góc nghiêng `[bắt đầu, kết thúc]` để hoa đung đưa |
| `flip` | lật ngang |

Muốn thêm hoa khác: thả file `.webp` hoặc `.png` nền trong suốt vào `assets/flowers/`
rồi thêm một dòng vào `garden`. Desktop hiện cả 26 bông, máy yếu hiện 18, điện thoại hiện 10 bông đầu.

Lá thư nằm trong thẻ `<article class="letter">`, sửa trực tiếp bằng HTML.

### Thay ảnh

Tất cả ảnh nằm trong `assets/`. Thay bằng ảnh của hai bạn thì chỉ cần giữ nguyên tên file:

| File | Dùng ở đâu |
|---|---|
| `peonies.jpg` | Ảnh lớn ở màn hình đầu |
| `roses-vase.jpg` | Ảnh cạnh phần đếm ngược |
| `roses-bouquet.jpg` | Bó hoa hiện ra khi bấm "Nhận hoa từ anh" |
| `peonies-2.jpg` | Ảnh cạnh "Lời hứa" |
| `tulips-pink.jpg` | Con dấu phong thư, và là nguồn cắt ra cánh hoa rơi |
| `dense-peonies.jpg`, `pastel-roses.jpg`, `charming-rose.jpg`, `modest-vase.jpg`, `kiss-of-pink.jpg`, `camelia.jpg`, `pink-bouquet.jpg`, `vase-pink.jpg`, `holding-peony.jpg`, `hydrangea.jpg`, `cherry-blue.jpg`, `supple-peonies.jpg` | Mặt trước 12 tấm thiệp, theo thứ tự 1 → 12 |

Mỗi vị trí dùng một ảnh riêng, không ảnh nào lặp lại.

Nếu thay ảnh cắt cánh hoa (`petalSource`), chỉnh lại `rects` cho đúng vùng có cánh hoa
(mỗi rect là `[x, y, rộng, cao]` theo tỉ lệ 0..1 của ảnh).

### Nhạc nền

Trình phát dạng đĩa than ở góc dưới phải, phát lặp lại:

| File | Bài | Tác giả | Giấy phép |
|---|---|---|---|
| `assets/music/canon-in-d.mp3` | Canon in D Major (bản phối của Pachelbel) | Kevin MacLeod (incompetech.com) | CC BY 3.0, cần ghi tên tác giả |

Tên "Kevin MacLeod" hiện ngay trong trình phát, đó cũng là phần ghi công theo giấy phép. Đừng xoá nó.

- Trình duyệt không cho tự phát nhạc, nên nhạc bắt đầu ở **lần chạm đầu tiên** bất kỳ đâu trên trang
  (ví dụ bấm "Nhận hoa từ anh"). Bong bóng "Chạm để nghe nhạc" nhắc việc này.
- Nút loa tắt/bật tiếng, nhạc nhỏ dần và to dần cho êm. Lựa chọn tắt tiếng được nhớ cho lần mở sau.
- Chạm vào đĩa than cũng là tạm dừng / phát tiếp. Chuyển tab thì nhạc tự dừng, quay lại thì phát tiếp.
- Đổi bài: thả file mp3 vào `assets/music/` rồi sửa mảng `music` trong `CONFIG`. Âm lượng: `musicVolume` (0..1).
  Nếu dùng bài hát Việt Nam có bản quyền, chỉ nên để trang ở chế độ chia sẻ riêng.

## Deploy miễn phí

### Netlify Drop (nhanh nhất, không cần tài khoản Git)
1. Vào <https://app.netlify.com/drop>
2. Kéo thả **cả thư mục** này vào (phải có cả `assets/`).
3. Nhận link dạng `https://ten-ngau-nhien.netlify.app`. Đổi tên trong *Site settings → Change site name*.

### GitHub Pages
```bash
git init
git add .
git commit -m "20/10 cho Minh Loan"
git branch -M main
git remote add origin https://github.com/<user>/20_10_flowers.git
git push -u origin main
```
Repo → **Settings → Pages → Source: Deploy from a branch → main / (root)** → Save.
Link: `https://<user>.github.io/20_10_flowers/`

### Vercel
```bash
npm i -g vercel
vercel        # Enter hết, chọn "Other"
```

### Cloudflare Pages
<https://pages.cloudflare.com> → *Create a project* → *Direct Upload* → kéo thả thư mục.

## Font chữ

| Font (Google Fonts) | Dùng ở đâu |
|---|---|
| Corinthia 700 | Tên, tiêu đề, chữ ký, dải chữ chạy |
| EB Garamond | Thân bài, lá thư, lời hứa, nút bấm |
| Cormorant Garamond | Số đếm ngược, dấu ngoặc kép trên thiệp |

Cả ba font đều có đủ dấu tiếng Việt. Muốn đổi font thư pháp khác, thay `Corinthia` trong link
Google Fonts ở `<head>` và trong CSS. Các font thư pháp khác có tiếng Việt: Imperial Script,
Ephesis, Luxurious Script, Lavishly Yours, Moon Dance.

## Thư viện ngoài (CDN)

| Thư viện | Dùng để |
|---|---|
| GSAP 3.12.5 + ScrollTrigger | Các khối hiện theo cuộn, thiệp và tag hiện lần lượt |

Nếu CDN không tải được, trang vẫn chạy với hiệu ứng CSS cơ bản.
Cánh hoa rơi và hiệu ứng nổ hoa tự viết bằng canvas, không phụ thuộc mạng.

## Hiệu năng

Animation lặp vô hạn chỉ dùng `transform` / `opacity`. Không có `filter: blur`,
`backdrop-filter`, hay animate `background-position`. Hạt film là ảnh SVG tĩnh.
Số cánh hoa tự giảm trên điện thoại và máy ít nhân CPU. Canvas nổ hoa tự dừng khi hết hạt.

## Nguồn ảnh

Tất cả ảnh đều CC0 (public domain), gốc từ Unsplash, lấy qua Wikimedia Commons:

| File | Tác giả |
|---|---|
| `peonies.jpg`, `peonies-2.jpg` | Alisa Anton |
| `roses-vase.jpg` | Alexandra Seinet |
| `roses-bouquet.jpg` | Caroline Attwood |
| `vase-pink.jpg` | Yuri_B (Pixabay) |
| `tulips-pink.jpg` | Ludovic Gauthier |
| `dense-peonies.jpg` | Mira Bozhko |
| `pastel-roses.jpg` | Alina Sofia |
| `charming-rose.jpg` | Thomas Curryer |
| `modest-vase.jpg` | Kristina M M |
| `kiss-of-pink.jpg` | Brooke Lark |
| `camelia.jpg` | Silvia Agrasar |
| `pink-bouquet.jpg` | O.C. Gonzalez |
| `holding-peony.jpg` | Matthew Henry |
| `hydrangea.jpg` | Veri Ivanova |
| `cherry-blue.jpg` | Karolien Brughmans |
| `supple-peonies.jpg` | Kristine Weilert |

Hoa tách nền trong `assets/flowers/` lấy từ Wikimedia Commons. Giấy phép CC BY-SA yêu cầu
ghi tên tác giả. Dòng ghi công đã được bỏ khỏi chân trang theo yêu cầu; bảng dưới đây và
ghi chú trong mã nguồn `index.html` là nơi lưu thông tin này. Với trang cá nhân tặng riêng,
rủi ro gần như bằng không, nhưng nếu công khai rộng thì nên đưa lại dòng ghi công.

| File | Gốc trên Commons | Tác giả | Giấy phép |
|---|---|---|---|
| `daisy.webp` | Daisy.png | Lubna | CC BY-SA 3.0 |
| `daisy-side.webp` | Daisy from below.png | Muffet | CC BY-SA 2.5 |
| `tulip.webp` | Tulipa sprengeri FotoTakkk Hungary PNG.png | Takkk | CC BY-SA 3.0 |
| `narcissus.webp` | Narcissus20090514 37.png | Bff | CC BY-SA 3.0 |
| `rose-cream.webp` | Rose20090913 36.png | Bff | CC BY-SA 3.0 |
| `calla.webp` | Zantedeschia aethiopica 002.png | Amada44 | CC BY 3.0 |
| `snowdrop.webp` | Kleines Schneeglöcken.png | User:aka | CC BY-SA 2.5 |
| `clover.webp` | Trifolium repens macro flower cutout.png | YuriNikolai | CC0 |
| `flax.webp` | Linum grandiflorum20090813 006.png | Bff | CC BY-SA 3.0 |
| `hawkweed.webp` | Hieracium lachenalii20090813 153.png | Bff | CC BY-SA 3.0 |
| `rose-pink.webp` | Extracted pink rose.png | Kurt Stüber, biolib.de | CC BY-SA 3.0 |
| `rose-fuchsia.webp` | YosriDec04MawarKampung1 Extracted.png | Yosri | CC BY-SA 3.0 |

Trường `hue` / `sat` trong `garden` xoay màu và chỉnh độ bão hoà để đưa hoa về tông hồng.

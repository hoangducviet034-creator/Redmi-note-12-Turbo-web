# Redmi Note 12 Turbo — Dark-Tech 3D Product Concept

Website giới thiệu sản phẩm bằng tiếng Việt, xây dựng với Vite, Three.js và GSAP ScrollTrigger. Đây là **bản concept phi thương mại**; mô hình smartphone được tự dựng bằng geometry Three.js, không phải model GLB thương mại/chính thức.

## Tính năng đã có

- Mô hình 3D điện thoại có mặt trước, punch-hole, mặt lưng, khung viền, nút cạnh và cụm camera nhiều lớp.
- Ánh sáng vật lý, environment map dựng bằng `RoomEnvironment`, bloom hậu kỳ `UnrealBloomPass`, hạt sáng và vòng neon.
- Cuộn trang qua 9 chương; hướng xoay và tông sáng mô hình đổi theo section.
- Kéo ngang để xoay mô hình; chụm hai ngón để zoom trên mobile; `Ctrl` + lăn chuột để zoom trên desktop.
- Tự xoay chậm sau 3 giây không tương tác.
- Chuyển màu Carbon Black / Ice Feather White / Xinghai Blue.
- Navbar blur khi cuộn, menu toàn màn hình trên mobile, thẻ thông số, counter animation, toast cho nút demo.
- Responsive cho điện thoại, tablet, desktop; cursor tùy chỉnh chỉ hiện ở thiết bị có chuột.
- Giảm chuyển động theo `prefers-reduced-motion`; giảm bloom và số hạt trên màn hình nhỏ.
- Meta SEO, Open Graph SVG, favicon và JSON-LD Product.

## Cấu trúc thư mục

```text
redmi-note12-turbo-site/
├── index.html
├── package.json
├── README.md
├── public/
│   ├── favicon.svg
│   └── og-note12turbo.svg
└── src/
    ├── main.js
    └── style.css
```

## Chạy trên máy tính

Cài Node.js 20 trở lên, giải nén ZIP rồi mở terminal trong thư mục dự án:

```bash
npm install
npm run dev
```

Mở địa chỉ Vite hiển thị trong terminal. Muốn tạo bản build:

```bash
npm run build
npm run preview
```

## Chạy trên điện thoại Android

1. Giải nén thư mục dự án bằng ứng dụng quản lý tệp.
2. Mở thư mục qua Termux hoặc môi trường có Node.js.
3. Chạy `npm install`, sau đó `npm run dev -- --host 0.0.0.0`.
4. Mở URL nội bộ được in trong terminal bằng trình duyệt. Nếu truy cập từ thiết bị khác cùng Wi-Fi, dùng IP LAN của điện thoại/máy chạy server và cổng Vite.

Trang dùng các package npm; lần đầu chạy cần internet để cài package. Sau khi build, thư mục `dist/` là bản tĩnh có thể đưa lên hosting HTTPS. Không mở `index.html` trực tiếp bằng `file://` vì ES modules cần được phục vụ qua HTTP.

## Thay mô hình tự dựng bằng GLB

Mô hình hiện tại là placeholder tự dựng trong `makePhone()` ở `src/main.js`. Nếu có file GLB mà bạn có quyền sử dụng:

1. Cài `three` đã có sẵn trong dependencies; import `GLTFLoader` từ `three/addons/loaders/GLTFLoader.js`.
2. Đưa file vào `public/models/note12-turbo.glb` và dùng `GLTFLoader.load('/models/note12-turbo.glb', ...)`.
3. Thay phần tạo `phoneGroup` bằng root node của GLB, căn lại kích thước và trục xoay; giữ nguyên `setScene()` để tiếp tục điều khiển theo cuộn trang.
4. Nén texture, giới hạn texture 1K–2K và giảm polygon cho điện thoại tầm trung.

Chỉ dùng model/logo/tài nguyên có giấy phép phù hợp; đừng xem hình học hiện tại là bản tái tạo chính xác sản phẩm.

## Chỉnh phong cách và hiệu năng

- Màu vỏ: chỉnh `colorways` trong `src/main.js`.
- Màu ánh sáng theo cảnh: chỉnh `scenePalettes` và `applyScenePalette()`.
- Bloom: chỉnh `bloomStrength` trong `initThree()`; hạ về `0.25–0.4` nếu máy nóng/giật.
- Hạt sáng: thay `count` trong `makeStars()`; hiện tại 240 hạt trên màn hình nhỏ, 760 trên desktop.
- Chất liệu: trong `makePhone()`, chỉnh `roughness`, `metalness`, `clearcoat` của `MeshPhysicalMaterial`.
- Tốc độ auto-rotate: chỉnh `dt * 0.10` hoặc `dt * 0.13` trong `tick()`.
- Nếu máy yếu, bỏ `UnrealBloomPass` hoặc giảm pixel ratio; renderer hiện giới hạn DPR 1.25 trên màn hình nhỏ.

## Những gì chưa kiểm thử

- Chưa xác minh trên từng dòng điện thoại/trình duyệt thực tế.
- Chưa đo FPS, nhiệt độ, mức tiêu thụ pin hoặc Lighthouse trên thiết bị thật.
- Chưa tích hợp GLB chính thức, backend, chức năng đặt hàng hay tìm cửa hàng thật.
- Ảnh Open Graph là SVG concept tự tạo; một số nền tảng mạng xã hội có thể yêu cầu ảnh PNG/JPG để hiển thị tối ưu.
- Thông số được nhập theo brief; phiên bản/cấu hình có thể khác theo thị trường.

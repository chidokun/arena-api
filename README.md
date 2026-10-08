# arena-api

Cấu hình dùng chung cho [Arena](https://arena.nguyentuan.dev) — đấu trường game đối kháng chạy hoàn toàn trên trình
duyệt (P2P qua WebRTC, site tĩnh trên GitHub Pages). Arena không có máy chủ, nên repo này giữ các tệp JSON mà site đọc
lúc chạy để đổi trạng thái game mà không cần build và deploy lại.

## Tệp

| Tệp          | Nội dung                                  |
| ------------ | ----------------------------------------- |
| `games.json` | Danh sách game và trạng thái của từng game |

## `games.json`

Danh mục game đầy đủ — thay cho mảng `GAMES` trong `lib/games/registry.ts` của Arena (trường `available` cũ được thay
bằng `status`).

```json
{
  "games": [
    {
      "slug": "caro",
      "name": "Cờ Caro",
      "emoji": "⭕",
      "status": "LIVE",
      "tagline": "Năm quân thẳng hàng là thắng",
      "description": "Hai người lần lượt đặt X và O trên bàn 15×15. …",
      "seats": { "min": 2, "max": 2 },
      "capacity": { "min": 2, "max": 16, "default": 8 },
      "hue": "coral"
    }
  ]
}
```

| Trường        | Kiểu   | Bắt buộc | Ý nghĩa                                                                 |
| ------------- | ------ | -------- | ----------------------------------------------------------------------- |
| `slug`        | string | có       | Mã game, dùng trong đường dẫn `/games/<slug>/` (tiếng Anh)              |
| `name`        | string | có       | Tên hiển thị (tiếng Việt)                                               |
| `emoji`       | string | có       | Biểu tượng trên thẻ game                                                |
| `status`      | enum   | có       | `DEVELOPMENT`, `LIVE` hoặc `MAINTENANCE` (xem bên dưới)                 |
| `tagline`     | string | có       | Câu giới thiệu ngắn trên thẻ game                                       |
| `description` | string | có       | Mô tả luật chơi ở sảnh game                                             |
| `seats`       | object | có       | Số ghế chơi chủ phòng có thể chọn (`min`, `max`)                        |
| `capacity`    | object | có       | Số người tối đa trong phòng (người chơi + người xem): `min`, `max`, `default`; toàn `0` là không giới hạn |
| `hue`         | enum   | có       | Màu chủ đạo của thẻ: `coral`, `sky`, `lime`, `grape`, `sun`             |
| `host`        | string | không    | Tên gọi người tạo phòng; bỏ trống là "Chủ phòng" (ma sói: "Quản trò")   |

### Trạng thái

| `status`      | Ý nghĩa                                                                      |
| ------------- | ---------------------------------------------------------------------------- |
| `DEVELOPMENT` | Đang phát triển — hiện thẻ "sắp ra mắt", chưa tạo / vào phòng được             |
| `LIVE`        | Đang chạy — chơi bình thường                                                 |
| `MAINTENANCE` | Tạm bảo trì — vẫn hiện trong danh sách nhưng khoá tạo / vào phòng mới          |

Hiện tại:

| Game                | Slug           | Trạng thái    |
| ------------------- | -------------- | ------------- |
| ⭕ Cờ Caro           | `caro`         | `LIVE`        |
| 🧧 Lô Tô             | `loto`         | `LIVE`        |
| 🐺 Ma Sói            | `werewolf`     | `LIVE`        |
| 🕵️ Truy tìm Gián Điệp | `undercover`   | `LIVE`        |
| 🔢 Sudoku Tranh Đấu  | `sudoku`       | `LIVE`        |
| 🀄 Cờ Tướng          | `xiangqi`      | `LIVE`        |
| 🌩️ Né Bão            | `dodge`        | `LIVE`        |
| ♟️ Cờ Tướng Nhập Vai  | `xiangqi-role` | `LIVE`        |
| 🔵 Thả Cờ 4          | `connect-four` | `DEVELOPMENT` |
| 🚢 Bắn Tàu           | `battleship`   | `DEVELOPMENT` |
| 🎨 Vẽ Đoán           | `draw-guess`   | `DEVELOPMENT` |

## Cập nhật

1. Sửa `status` (hoặc thêm game mới) trong `games.json`.
2. Kiểm tra JSON hợp lệ: `node -e "JSON.parse(require('fs').readFileSync('games.json','utf8'))"`.
3. Commit và đẩy lên `main`.

Thêm game mới: thêm một mục đủ các trường bắt buộc; game chưa có code (luật, lớp phòng, bàn chơi bên Arena) thì để
`DEVELOPMENT`.

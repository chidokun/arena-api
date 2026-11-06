# Arena-API

Cấu hình dùng chung cho Arena — đấu trường game đối kháng chạy hoàn toàn trên trình duyệt.

## Tệp

| Tệp          | Nội dung                                                                                                            |
| ------------ | ------------------------------------------------------------------------------------------------------------------- |
| `games.json` | Danh mục game: tên, mô tả, số ghế, sức chứa, màu, thể loại và trạng thái (`LIVE` / `MAINTENANCE` / `DEVELOPMENT`). |
| `home.json`  | Cấu hình trang chủ. `games`: slug các game ở mục "Game nổi bật", theo thứ tự hiển thị.                             |

### `home.json`

```json
{
  "games": ["caro", "battleship", "draw-guess"]
}
```

- Mỗi slug phải có trong `games.json` và không lặp lại; danh sách không được rỗng.
- Game đang không `LIVE` thì trang chủ bỏ qua và lấy game `LIVE` khác bù vào, nên số thẻ vẫn bằng độ dài danh sách.
- Arena đọc tệp lúc build và tải lại trên trình duyệt, nên đổi thứ tự không cần build lại.

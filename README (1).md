# Lab 7: Testing with Postman

Người kiểm thử: Phạm Lê Đình An

## 1. Mục tiêu kiểm thử

Kiểm tra API thời tiết hiện tại bằng Postman: request trả về HTTP `200 OK`, body JSON có thông tin địa điểm (`location`) và thời tiết (`current`).

## 2. Cấu hình request

- Công cụ: Postman.
- Method: `GET`.
- Base URL: `https://api.weatherapi.com/v1`.
- Endpoint: `/current.json`.
- Xác thực: API key của WeatherAPI; không công khai key trong báo cáo.

URL theo ảnh chụp:

```text
{{baseUrl}}/current.json?q={{location}}&aqi=no&pollen=no&lang=string&current_fields=string
```

### Query parameters

| Tham số          | Giá trị trong request | Ý nghĩa                                                                                           |
| ---------------- | --------------------- | ------------------------------------------------------------------------------------------------- |
| `q`              | `{{location}}`        | Địa điểm cần lấy thời tiết                                                                        |
| `aqi`            | `no`                  | Không yêu cầu dữ liệu chất lượng không khí                                                        |
| `pollen`         | `no`                  | Không yêu cầu dữ liệu phấn hoa                                                                    |
| `lang`           | `string`              | Giá trị đang có trong ảnh; cần thay bằng mã ngôn ngữ hợp lệ, ví dụ `vi`                           |
| `current_fields` | `string`              | Giá trị đang có trong ảnh; cần kiểm tra giá trị được API hỗ trợ hoặc bỏ tham số nếu không sử dụng |

> Ảnh chưa thể hiện giá trị thực tế của biến `location` và cấu hình API key. Cần cấu hình trước khi chạy lại; response ghi nhận địa điểm Silwana's Location, South Africa.

## 3. Các bước thực hiện

1. Cấu hình biến `baseUrl`, `location` và API key trong Postman.
2. Tạo request `GET` tới `/current.json`, nhập các query parameters.
3. Nhấn **Send**.
4. Đối chiếu status code và response body với tiêu chí kiểm thử bên dưới.

## 4. Tiêu chí và kết quả kiểm thử

| Mã   | Tiêu chí                               | Kết quả thực tế                              | Đánh giá |
| ---- | -------------------------------------- | -------------------------------------------- | -------- |
| TC01 | Request trả về HTTP `200 OK`           | `200 OK` trong ảnh chụp                      | Pass     |
| TC02 | Body là JSON hợp lệ                    | Response JSON bên dưới có cấu trúc hợp lệ    | Pass     |
| TC03 | Có thông tin địa điểm trong `location` | Có `name`, `region`, `country`, `lat`, `lon` | Pass     |
| TC04 | Có thông tin thời tiết trong `current` | Có `temp_c`, `humidity`, `condition`         | Pass     |

Thông tin ghi nhận trong ảnh: thời gian phản hồi **243 ms**, kích thước response **1.1 KB**. Đây là số liệu của một lần gửi request, không phải kết luận kiểm thử hiệu năng.

### Ảnh kết quả

![Kết quả kiểm thử API thời tiết trên Postman](docs/img.png)

### Response body

```json
{
  "location": {
    "name": "Silwana's Location",
    "region": "Limpopo",
    "country": "South Africa",
    "lat": -23.7167,
    "lon": 30.9167,
    "tz_id": "Africa/Johannesburg",
    "localtime_epoch": 1791506886,
    "localtime": "2026-10-09 02:48"
  },
  "current": {
    "last_updated_epoch": 1791506700,
    "last_updated": "2026-10-09 02:45",
    "temp_c": 21.9,
    "temp_f": 71.4,
    "is_day": 0,
    "condition": {
      "text": "Clear",
      "icon": "//cdn.weatherapi.com/weather/64x64/night/113.png",
      "code": 1000
    },
    "wind_mph": 4.7,
    "wind_kph": 7.6,
    "wind_degree": 335,
    "wind_dir": "NNW",
    "pressure_mb": 1017.0,
    "pressure_in": 30.04,
    "precip_mm": 0.0,
    "precip_in": 0.0,
    "humidity": 70,
    "cloud": 0,
    "feelslike_c": 22.6,
    "feelslike_f": 72.7,
    "windchill_c": 21.9,
    "windchill_f": 71.4,
    "heatindex_c": 24.5,
    "heatindex_f": 76.2,
    "dewpoint_c": 16.3,
    "dewpoint_f": 61.3,
    "vis_km": 10.0,
    "vis_miles": 6.0,
    "uv": 0.0,
    "gust_mph": 12.6,
    "gust_kph": 20.2,
    "will_it_rain": 0,
    "chance_of_rain": 6,
    "will_it_snow": 0,
    "chance_of_snow": 0,
    "wetbulb_c": 18.3,
    "wetbulb_f": 64.9,
    "short_rad": 0,
    "diff_rad": 0,
    "dni": 0,
    "gti": 0
  }
}
```

## 5. Kết luận

Request trong ảnh trả về `200 OK` và dữ liệu thời tiết dạng JSON, đạt các tiêu chí kiểm tra cơ bản nêu trên.

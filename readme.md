# Hướng dẫn triển khai Swagger Documentation

## 📋 Tổng quan

Dự án này cung cấp nhiều cách để triển khai Swagger UI cho các file OpenAPI của bạn:

- `sales_voucher.openapi.yaml` - Sales Voucher API
- `purchase_voucher.openapi.yaml` - Purchase Voucher API
- `purchase_invoice.openapi.yaml` - Purchase Invoice API
- `purchase_return.openapi.yaml` - Purchase Return API

## 🚀 Các phương án triển khai

### 1. Triển khai tĩnh (Đơn giản nhất)

**Ưu điểm:** Nhanh, dễ dàng, miễn phí
**Phù hợp:** Demo, test nội bộ, tài liệu tạm thời

#### Bước 1: Chuẩn bị file

```bash
# Đặt các file OpenAPI vào cùng thư mục với index.html
├── index.html
├── sales_voucher.openapi.yaml
├── purchase_voucher.openapi.yaml
├── purchase_invoice.openapi.yaml
├── purchase_return.openapi.yaml
└── README.md
```

#### Bước 2: Triển khai

- **GitHub Pages:** Push code lên GitHub repo, bật GitHub Pages
- **Netlify:** Kéo thả thư mục vào netlify.com
- **Vercel:** Import project từ GitHub hoặc upload thư mục
- **Surge.sh:** `npm install -g surge && surge`

### 2. Sử dụng Docker (Chuyên nghiệp)

**Ưu điểm:** Dễ quản lý, có thể tùy chỉnh
**Phù hợp:** Môi trường production, team lớn

```dockerfile
# Dockerfile
FROM swaggerapi/swagger-ui:latest
COPY sales_voucher.openapi.yaml /usr/share/nginx/html/
ENV SWAGGER_JSON=/usr/share/nginx/html/sales_voucher.openapi.yaml
```

```bash
# Build và chạy
docker build -t my-swagger-docs .
docker run -p 8080:8080 my-swagger-docs
```

### 3. Sử dụng Swagger Editor (Online)

**Ưu điểm:** Không cần setup, có thể chỉnh sửa trực tiếp
**Phù hợp:** Test nhanh, chỉnh sửa spec

1. Truy cập https://editor.swagger.io/
2. Copy nội dung file YAML và paste vào editor
3. Chia sẻ link hoặc export HTML

### 4. Tích hợp vào ứng dụng hiện tại

#### Node.js/Express

```javascript
const swaggerUi = require("swagger-ui-express");
const YAML = require("yamljs");
const swaggerDocument = YAML.load("./sales_voucher.openapi.yaml");

app.use("/api-docs", swaggerUi.serve, swaggerUi.setup(swaggerDocument));
```

#### Python/Flask

```python
from flask import Flask
from flask_restx import Api
from flask_restx.apidoc import apidoc

app = Flask(__name__)
api = Api(app, doc='/docs/')
```

#### Spring Boot/Java

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.any())
                .build();
    }
}
```

## 🛠️ Tùy chỉnh nâng cao

### Thay đổi giao diện

```html
<!-- Trong index.html -->
<style>
  .swagger-ui .topbar {
    background-color: #your-brand-color;
  }
</style>
```

### Thêm xác thực

```javascript
// Trong script
const ui = SwaggerUIBundle({
  // ... other config
  requestInterceptor: (request) => {
    request.headers["Authorization"] = "Bearer YOUR_TOKEN";
    return request;
  },
});
```

### CORS Configuration

```javascript
// Nếu API của bạn cần CORS
const ui = SwaggerUIBundle({
  // ... other config
  requestInterceptor: (request) => {
    request.headers["Access-Control-Allow-Origin"] = "*";
    return request;
  },
});
```

## 📝 Checklist triển khai

- [ ] File OpenAPI spec hợp lệ (test tại editor.swagger.io)
- [ ] Cập nhật server URLs trong spec
- [ ] Thiết lập CORS nếu cần
- [ ] Test các endpoint từ Swagger UI
- [ ] Cấu hình xác thực nếu có
- [ ] Tùy chỉnh giao diện theo brand
- [ ] Setup monitoring/analytics nếu cần

## 🔧 Troubleshooting

### Lỗi CORS

```javascript
// Thêm vào server config
app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header(
    "Access-Control-Allow-Headers",
    "Origin, X-Requested-With, Content-Type, Accept, Authorization"
  );
  next();
});
```

### File YAML không load được

- Kiểm tra đường dẫn file trong `index.html`
- Đảm bảo file YAML có syntax đúng
- Kiểm tra console browser để xem lỗi chi tiết

### UI không hiển thị đúng

- Kiểm tra CDN links có hoạt động không
- Clear browser cache
- Kiểm tra JavaScript console errors

## 📞 Hỗ trợ

Nếu cần hỗ trợ thêm:

1. Kiểm tra file OpenAPI spec tại https://editor.swagger.io/
2. Xem log console trong browser (F12)
3. Đảm bảo server API đã bật CORS
4. Test API endpoints trực tiếp trước khi dùng Swagger UI

## 🎯 Khuyến nghị

**Cho development:** Sử dụng phương án 1 (tĩnh) hoặc 3 (online editor)
**Cho staging:** Sử dụng phương án 2 (Docker) hoặc tích hợp vào app
**Cho production:** Tích hợp vào ứng dụng chính hoặc deploy riêng với Docker

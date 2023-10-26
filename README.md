# Xây dựng terraform CI/CD với terraform cloud

### Có 3 cách toạ 1 workspace trên terraform cloud:

- Version Control System Workflow (VCS): xây dựng CI/CD
- CLI-driven Workflow: xây dựng Remote Backend
- API-driven Workflow

Việc dùng **Terraform Cloud VCS** để xây dựng CI/CD rất dễ dàng, chỉ cần tạo 1
Github Repo và liên kết nó với Terraform Cloud

### Create_before_destroy

Trong trường hợp chúng ta đang chạy EC với cấu hình **t2.micro** và chúng ta
muốn update lên **t3.small** thì Terraform sẽ destroy con **t2** trước sau đó sẽ
tạo con **t3**, như vậy nếu có người dùng truy cập khi đang chuyển đổi sẽ xảy ra
lỗi

=> Giải pháp ở đây là dùng **create_before_destroy**

Ta cấu hình như sau

```
resource "aws_instance" "ansible_server" {
  ami           = data.aws_ami.ami.id
  instance_type = "t3.small"

  lifecycle {
    create_before_destroy = true
  }
}
```

### Lưu ý

create_before_destroy có thể thuận lợi trong một số điều kiện nhưng trong một số
trường hợp sẽ không nên dùng nó ví dụ như khi chúng ta dùng với S3, vì tên của
S3 là unique

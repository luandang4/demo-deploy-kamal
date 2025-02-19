Các bước cơ bản deploy với Kamal
- Chuẩn bị server, repository lưu trữ image, cài đặt kamal
- Config trong file config/deploy.yml
  ```
  # một số config chính
  service: demo_kamal_1
    image: luandang1402/demo_kamal_1
    servers:
      web:
        - 14.225.29.112
    registry:
      username: luandang1402
      password:
        - KAMAL_REGISTRY_PASSWORD
    env:
      secret:
        - RAILS_MASTER_KEY
      clear:
        SOLID_QUEUE_IN_PUMA: true
        DB_HOST: demo_kamal_1-db # nếu sử dụng db accessory server

    accessories:
      db:
        image: mysql:8.0
        host: 14.225.29.112
        port: "127.0.0.1:3306:3306"
        env:
          clear:
            MYSQL_ROOT_HOST: '%'
          secret:
            - MYSQL_ROOT_PASSWORD
        directories:
          - data:/var/lib/mysql

- Khởi tạo vào truyền biến môi trường vào trong file .kamal/secrets
ví dụ:
  deploy.yml:
  ```
  registry:
    username: luandang1402
    password:
      - KAMAL_REGISTRY_PASSWORD
  ```

  .kamal/secrets:
  ```
  KAMAL_REGISTRY_PASSWORD=$KAMAL_REGISTRY_PASSWORD_ENV
  ```

  .env:
  ```
  KAMAL_REGISTRY_PASSWORD_ENV=dckr_pat_vitxxxx
  ```

** chạy export `$(cat .env | xargs)` hoặc export `$(grep -v '^#' .env | xargs)` để lấy biến môi trường

- Chạy `kamal setup`

- Nếu lần sau không sửa các env thì chỉ cần chạy

`kamal deploy`

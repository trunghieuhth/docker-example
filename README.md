# AUTOMATE DOCKER DEPLOYMENT

Hướng dẫn tự động build và deploy sử dụng Docker, Bitbucket step by step.

## NodeJS (ReactJS, React Native, ...)

### 1. Tạo `Dockerfile`
Vào thư mục chứa project, tạo mới một file `Dockerfile` và một file `.dockerignore`
```sh
$ touch Dockerfile .dockerignore
```

Copy và paste vào `Dockerfile` đoạn script sau:

```dockerfile
# Using NodeJS v8.9.1 LTS
FROM node:8.9.1

# Create app directory
WORKDIR /app

# Bundle app source
COPY . .

# Install yarn
RUN npm i -g yarn

# Install dependencies
RUN yarn install

# Port
EXPOSE 3000

# Run app
CMD ["yarn", "start"]
```

Copy và paste vào `.dockerignore` đoạn script sau:

```dockerfile
node_modules
npm-debug.log
build
README.md
```

### 2. Commit và push project lên Bitbucket
Ở đây mình lấy một project React làm ví dụ, repo trên Bitbucket mình đặt tên là `docker-example`.

![alt text](https://i.imgur.com/K2aXHsM.png)

### 3. Tạo Docker repository và link đến Bitbucket
##### Tạo Docker repository
Vào https://cloud.docker.com tạo một repository

![alt text](https://i.imgur.com/BXFK6qQ.png)

##### Link Docker với Bitbucket

![alt text](https://i.imgur.com/gIp69XH.png)

### 4. Config tự động Build
Mỗi khi push code lên Bitbucket, Docker sẽ tự động build, làm như sau:
- Vào Docker repository vừa tạo
- Chọn tag `Builds`
- Chọn `Source Repository` từ Bitbucket vừa link
- Click `Save and Build`

![alt text](https://i.imgur.com/9O3E5NY.png)

### 5. Setup tự động Deployment
Vào thẻ `Nodes` -> Click `Bring your own node`

![alt text](https://i.imgur.com/18lQYhj.png)

Copy đoạn command xuất hiện trên màn hình và thực thi nó trên Server
```
$ curl -Ls https://get.cloud.docker.com/ | sudo -H sh -s 432c6a9fe646414e8e75898ede8448e1
```
![alt text](https://i.imgur.com/nUZimey.png)

> Lưu ý: Server cần mở port `2375`, `6783/tcp` và `6783/udp` để Docker kết nối.

Đợi 1-2 phút để Docker Cloud kết nối với Docker Daemon trên Server.

Sau khi kết nối xong, vào Server pull và chạy `hello-docker` repository vừa tạo ở trên.


Ok, vậy là project của ta đã có thể tự build và deploy mỗi khi push code lên Bitbucket.
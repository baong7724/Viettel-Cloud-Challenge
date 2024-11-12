# Challenge 3 - Nginx Proxy

**Đề bài:** Triển khai nginx proxy cho nhiều ứng dụng:   
- Từ level 2, triển khai thêm 1 trang web static thứ hai, khác với static web đã triển khai  
- Service cho trang web tĩnh mới được lấy tên là web2
- Triển khai thêm 1 deployment nginx-proxy đóng vai trò proxy cho cả 2 ứng dụng trên và tạo nodePort service có tên "nginx-proxy“
- Thiết lập cấu hình config của nginx-proxy sao cho:
    + Khi gọi tới nginx-proxy với path /web1 > nginx-proxy filter path và forward tới service web 1 > service web1 
    + khi gọi tới nginx-proxy với path /web2 > nginx-proxy filter path và forward tới service web 2 > service web2 bên ngoài thông qua nodePort    

**Output:**   
- Đóng gói thành công container chứa web tĩnh  
- Download 1 template tại (https://www.free-css.com/free-css-templates)   
- Sử dụng base image nginx  
- Lưu ý cấu hình nginx trỏ tới web tĩnh (tham khảo file cấu hình mẫu đơn giản tại https://gist.github.com/mockra/9062657)  
- 1 deployment chạy ứng dụng web tĩnh (replicas=2)  
- 1 nodePort service trỏ tới deployment (service web 1) 
- Thực hiên curl tới nodePort và cho ra kết quả trang web tĩnh theo template

## Step by step

Initalization:
 
```bash
mkdir challenge-03
cd challenge-03
```

Step 1: Triển khai Web static 1 ***Dockerfile***, ***Nginx***, ***Deployment và Service***

```bash
cd medical-web
touch Dockerfile
touch nginx.conf
docker build -t web1:latest .
docker tag web-static baong7724/web1:latest
docker push baong7724/web1:latest
cd .. 
touch web1-deployment.yaml
kubectl apply -f web1-deployment.yaml
```
Result: ![Screenshot from 2024-11-07 01-18-45](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge3_NginxProxy/Result/Screenshot%20from%202024-11-07%2001-18-45.png?raw=true)

Step 2: Triển khai Web static 2 ***Dockerfile***, ***Nginx***, ***Deployment và Service***

```bash
cd gym-web
touch Dockerfile
touch nginx.conf
docker build -t web2:latest .
docker tag web-static baong7724/web2:latest
docker push baong7724/web2:latest
cd .. 
touch web2-deployment.yaml
kubectl apply -f web2-deployment.yaml
```

Result: ![Screenshot from 2024-11-07 01-19-04](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge3_NginxProxy/Result/Screenshot%20from%202024-11-07%2001-19-04.png?raw=true)


Step 3: Triển khai Nginx Proxy ***Dockerfile***, ***Nginx***, ***Deployment và Service***

```bash
mkdir nginx-proxy
cd nginx-proxy
touch Dockerfile
touch nginx.conf
docker build -t nginx-proxy:latest .
docker tag web-static baong7724/nginx-proxy:latest
docker push baong7724/nginx-proxy:latest
cd .. 
touch nginx-proxy-deployment.yaml
kubectl apply -f nginx-proxy-deployment.yaml
```

Result: ![Screenshot from 2024-11-07 01-19-42](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge3_NginxProxy/Result/Screenshot%20from%202024-11-07%2001-19-42.png?raw=true)

## Verify

Thực hiện curl tới NodePort
 
```bash
kubectl get nodes -o wide //Take the IP address of the single node in minikube
curl <nodeIP>:30009/web1
curl <nodeIP>:30009/web2

```
Result: ![Screenshot from 2024-11-07 01-20-19](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge3_NginxProxy/Result/Screenshot%20from%202024-11-07%2001-20-19.png?raw=true)

![Screenshot from 2024-11-07 01-20-51](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge3_NginxProxy/Result/Screenshot%20from%202024-11-07%2001-20-51.png?raw=true)

![Screenshot from 2024-11-07 01-21-07](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge3_NginxProxy/Result/Screenshot%20from%202024-11-07%2001-21-07.png?raw=true)

![Screenshot from 2024-11-07 01-21-07](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge3_NginxProxy/Result/Screenshot%20from%202024-11-07%2001-21-07.png?raw=true)

![Screenshot from 2024-11-07 01-21-34](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge3_NginxProxy/Result/Screenshot%20from%202024-11-07%2001-21-34.png?raw=true)

![Screenshot from 2024-11-07 01-21-42](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge3_NginxProxy/Screenshot%20from%202024-11-07%2001-21-42.png?raw=true)



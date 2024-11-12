# Challenge 2 - Static web pod

**Đề bài:** Triển khai deployment một ứng dụng web tĩnh lên kubernetes cho phép truy cập từ
bên ngoài thông qua nodePort  
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
mkdir challenge-02
cd challenge-02
cd medical-web
```

Step 1: Tạo ***Dockerfile*** để đóng gói Container và file cấu hình ***Nginx***

```bash
touch Dockerfile
touch nginx.conf
```

Step 2: Xây Dựng ***Docker Image***

```bash
docker build -t web-static .
docker run -d -p 80:80 web-static
docker ps
```
Step 3: Đẩy ***Image*** Lên ***Docker Registry***

```bash
docker tag web-static baong7724/web-static:latest
docker push baong7724/web-static:latest
```

Result: ![Screenshot from 2024-11-06 22-26-33](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge2_StaticWebPod/Result/Screenshot%20from%202024-11-06%2022-26-33.png?raw=true)

Step 3: Tạo file ***Deployment*** cho ***Kubernetes***

```bash
cd ..
touch deployment.yaml
kubectl create namespace challenge-02
kubectl apply -f deployment.yaml -n challenge-02
kubectl get all -n challenge-02
```
Result: ![Screenshot from 2024-11-06 22-48-39](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge2_StaticWebPod/Result/Screenshot%20from%202024-11-06%2022-48-39.png?raw=true)


## Verify

Thực hiện curl tới NodePort
 
```bash
kubectl get nodes -o wide //Take the IP address of the single node in minikube
curl <node-IP>:30008
```
Result: ![Screenshot from 2024-11-06 22-49-15](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge2_StaticWebPod/Result/Screenshot%20from%202024-11-06%2022-49-15.png?raw=true)

# Challenge 1 - Default nginx

**Đề bài:** Triển khai deployment chạy nginx (default) lên kubernetes và cho phép truy cập từ bên ngoài thông qua nodePort.  
**Output:**   
• 1 deployment nginx (replicas=2 pod)  
• 1 nodePort service trỏ tới deployment  
• Thực hiên curl tới nodePort và cho ra kết quả trang web mặc định của nginx

## Step by step

Initalization:
 
```bash
mkdir challenge-01
cd challenge-01
```

Step 1: Tạo một file YAML tên ***nginx-deployment.yaml***

```bash
kubectl apply -f nginx-deployment.yaml
```
Step 2: Tạo một file YAML tên ***nginx-service.yaml***

```bash
kubectl apply -f nginx-service.yaml
```
Step 3: Kiểm tra Pods và Service

```bash
kubectl get all
```

Result: ![Screenshot from 2024-11-04 23-04-49](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge1_DefaultNginx/Result/Screenshot%20from%202024-11-04%2023-04-49.png?raw=true)

## Verify

Thực hiện curl tới NodePort
 
```bash
kubectl get nodes -o wide //Take the IP address of the single node in minikube
curl <node-IP>:30008
```
Result: ![Screenshot from 2024-11-04 23-05-09](https://github.com/baong7724/Viettel-Cloud-Challenge/blob/main/Challenge1_DefaultNginx/Result/Screenshot%20from%202024-11-04%2023-05-09.png?raw=true)

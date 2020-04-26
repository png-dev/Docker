## Docker
### CI/CD 

*  Giả sử nếu không có CI/CD, mỗi khi muốn deploy phiên bản ứng dụng lên để test một tính năng mới phát triển, developer cần làm các việc sau:
    - Chuẩn bị server mới với 1 OS phù hợp 
    - Tải source code của các ứng dụng 
    - Cấu hình các ứng dụng 
    - Trở ứng dụng vào database
    - Cài đặt các thư viện cần thiết
    - Xử lý xung đột về thư viện 
   
- Là những công việc lặp đi lặp lại, tốn rất nhiều thời gian để deploy 
- Ci/CD giúp những công việc này trở nên dễ dàng hơn thông qua việc code tự động deploy ra môi trường ảo hóa
- Công nghệ sử dụng là Gitlad CI , Docker Swarm 
- Developer có thể chủ động chọn thời điểm deploy bằng cách trực tiếp trên Gitlad

* Làm sao để có CI/CD 
     - Dockerfile 
     - stack-compose.yml 
     - .gitlad-ci.yml 
 
- Dockerfile
    - là nơi chứa các tập lệnh để dựng 1 virtual machine từ 1 OS mới tinh đến lúc app được cài đặt xong cần chạy 
    - Dockerfile có thể tạo ra 1 docker image từ lệnh docker build 
   
- Docker Image 
    - Là một bản snapshot của 1 virtual machine đã cài đặt xong app cần chạy 
    - Docker image có thể tạo ra các Docker container bằng lệnh docker run 
    - Docker image có thể được chia sẽ thông qua docker registry để deploy app ở những máy chủ khác nhau mà không cần tốn thời gian cài đặt
    
- Docker Registry 
    - Là một server lưu trữ và phân phối docker image 
    - Các server muốn tài về docker image cần phải đăng nhập hoặc có thể pull trực tiếp nếu docker registry không cần login 
    
- Docker container 
    - Là một virtual machine( tương đương với những VM tạo ra bởi VMWare , VirtualBox ..) đã cài đặt sẳn app cần chạy 
    - Từ docker container có thể export ra docker image bằng lệnh docker export 
   
- Docker Swarm 
    - Là một công nghệ cho phép sử dụng, chia sẻ tài nguyên CPU/RAM/DISK/NETWORK của nhiều server đồng thời (cụm server) để chạy các docker container trên đó
    - Mỗi server được gọi là docker node 
    - Docker swarm, các app trạng bị những đặc tính :
        - Tự động khôi phục (restart) nếu ứng dụng bị shutdown ( lỗi, kill, máy chủ bị shutdown ..)
        - Có thể thiết lập scale số nút lên tùy ý để gia tăng năng lực xử lý khi node quá tải 
     
- Docker node
    - Là một server được gia nhập cụm  Docker swarm 
    - Khi ra nhập cụm có thể nhận 1 trong 2 role 
        - Docker swarm manager 
            - Có quyền truy cập vào docker cli api để quản lý docker node, docker stack , docker service ...
            - Chưa các thông tin quản trị để có thể khôi phục được toàn bộ các dịch vụ đang chạy trên docker swarm 
        - Docker swarm worker 
            - Không có quyền truy cập vào docker cli api 
            - Không chưa thông tin quản trị Docker swarm 
            - Thường là node chính với tài nguyên lớn để chạy các ứng dụng nặng 
            
- Docker service 
    - Là app được khai báo với docker swarm bằng lệnh docker service create 
    - Tự động sinh docker container trên cụm server, đảm bảo sô lượng node được sinh ra theo yêu cầu của người thiết lập 
    
- stack-compose.yml 
    - Chứa các thiết lập cho phép nhiều docker service với những app khác nhau có thể làm việc được cùng với nhau trở thành 1 docker stack 
    - từ stack-compose.yml có thể sinh ra docker stack bằng lệnh docker stack deploy 
    
- Docker stack 
    - Là một cụm các docker service được quản lý để chạy trển 1 cụm server Docker swarm 
    
- .gitlad-ci.yml 
    - Định nghĩa các bước thực hiện, mỗi bước được coi là stage, các stage chạy theo thứ tự lần lượt từ trên xuống dưới 
    - 1 stage có thể gắn với nhiều job 
    - khi pushcode lên git thì hệ thống tư sinh pipeline với các job được định nghĩa 
    - Các job có cùng stage có thể chạy song song với nhau 
    - Luông build và deploy :
        - Build: 
            - Build docker image từ docker file 
            - Push docker image lên docker registry 
        - Deploy: 
            - Pull docker image từ docker registry 
            - Deploy docker stack theo file stack-compose.yml lên cụm docker swarm đã cấu hình 
            
- Portainer 
    - .... 

---
layout: post
title: From monolithic to Microservice and Microservices patterns
category: Technique
---
---

# From monolithic to Microservice and Microservices patterns

## From monolithic architecture to Microservice architecture

- <b>Monolithic architecture</b> là gì - architect an application as a <b>single deployable unit</b>. Một ứng dụng thiết kế theo kiến trúc monolithic là một khối ứng dụng hoàn chỉnh gồm nhiều thành phần(service) của ứng dụng (frontend, mobile backend, webapp,...) được đặt trong 1 khối duy nhất.

![](images/2022-02-27-microservice/1.png)

- Cùng phân tích ưu điểm và nhược điểm của monolithic:

    -  Ưu điểm:

        + Simple to develop
        + Simple to deploy
        + Simple to scale - bằng cách copy ứng dụng ra nhiều phiên bản và chạy sau loadbalancer
    
    - Nhược điểm:

        + Với 1 project lớn đồ sộ ví dụ một ứng dụng e-commerce, hay 1 ứng dụng như social network thì số lượng code base rất là nhiều
        khiến cho ứng dụng khó để hiểu và sửa đổi đối với các developer mới dẫn đến việc phát triển các tính năng mới. Hơn nữa, vì là 1 khối khó hiểu nên chất lượng code (quality of the code) sẽ giảm dần theo thời gian.
        + Overloaded IDE -  code càng to thì IDE càng khổ 
        + Overloaded container - Mỗi lần update hay reset app thì mất rất nhiều thời gian. Ví dụ bạn restart Tomcat chạy ứng dụng web app gồm java web application , rails application cùng với Frontend Vuejs thì thời gian restarting có thể lên đến ~10 min
        + Cùng với xu thế phát triển ứng dụng on cloud, CI/CD là 1 thứ tất yếu trong quy trình phát triển ứng dụng. Tuy nhiên với 1 khối monolithic thì ta rất khó có thể áp dụng quy trình CI/CD.
        + Scalling theo application - simple but difficult. Đơn giản vì chỉ cần bạn nhân bản instance đang chạy application lên thành nhiều bản và đứng sau loadbalancer, thậm chí có thể áp dụng cả autoscaling với các hạ tầng hỗ trợ autoscaling. Tuy nhiên, đến 1 lúc dữ liệu quá lớn việc scaling này sẽ kém hiệu quả. Vì các application vẫn truy cập vào 1 database duy nhất , 1 cache system duy nhất, dẫn đến việc botneck I/O tại layer xử lý dữ liệu
        + Việc phát triển trên quy mô lớn sẽ khó khăn, khi ứng dụng mở rộng, số lượng developer cũng như team nhiều lên thì kiến monolithic cản trở việc các team làm việc độc lập. Ví dụ nếu team UI có update về UI/UX thì việc mang nó lên production sẽ khó khăn vì còn phải depend vào các team khác
        + Khó khăn trong việc mở rộng technology stack, với kiến trúc monolithic thì khó có thể thay đổi áp dụng công nghệ mới.

-  Từ những nhược điểm đó, <b>Microservice architecture </b> đã được tạo ra để giải quyết các vấn đề tồn đọng của Monolithic architecture. Microservice architect -  an application as a collection of loosely coupled, services. Một kiến trúc mà khi đó, ứng dụng sẽ là 1 tập hợp gồm nhiều service kết hợp với nhau (loosely coupled).

![](images/2022-02-27-microservice/2.png)

- Với <b>Microservice architecture </b> thấy được những lợi ích rõ rệt:

    - Việc áp dụng quy trình CI/CD trở nên dễ dàng
        + Khả năng maintain, update từng service dễ dàng
        + Better testability - services are smaller and faster to test
        + Better deployability - services can be deployed independently
        + Tăng khả năng làm việc độc lập của các team
    - Mỗi microservice rất nhỏ:
        + Dễ dàng cho các developer đọc và hiểu cách service hoạt động
        + Speeds up deployments
    - Improved fault isolation
    - Eliminates any long-term commitment to a technology stack.

- Tuy nhiên vẫn có những mặt hạn chế:

    - Hệ thống phức tạp, các developers phải tốn nhiều effort về việc community giữa các service
    - Độ phức tạp khi triển khai và  quản trị, monitor các microservice trên production
    - Theo nguyên tắc CAP (CAP theorem) thì giao dịch phân tán sẽ không thể thỏa mãn cả 3 điều kiện: consistency (dữ liệu ở điểm khác nhau trong mạng phải giống nhau), availablity (yêu cầu gửi đi phải có ressponse), partition tolerance (hệ thống vẫn hoạt động được ngay cả khi mạng bị lỗi). Những công nghệ cơ sở dữ liệu phi quan hệ (NoSQL) hay môi giới thông điệp (message broker) tốt nhất hiện nay cũng chưa vượt qua nguyên tắc CAP

## Microservices patterns

- Hiện nay có rất nhiều pattern liên quan đến microservice pattern, các pattern giải quyết các vấn đề mà các kỹ sư sẽ gặp phải khi triển khai kiến trúc microservice. Các pattern được chia thành 3 loại patterns : 
    - <b>Infrastructure patterns </b> - những loại patterns liên quan đến việc triển khai hạ tầng 
    - <b>Application Infrastructure patterns </b> - những loại patterns liên quan đến việc triển khai, theo dõi hạ tầng cho ứng dụng (message queue, loging, montoring,..)
    - <b>Application patterns </b> - những pattern liên quan đến việc develop service

![](images/2022-02-27-microservice/3.png)

### Infrastructure patterns

- Service deployment platform : sử dụng các nền tảng deployment platform như docker swarm, k8s, aws lambda,..
    + Serverless Deployment: với serverless thì developer không cần takecare phần hạ tầng
    + Service instance per container: các service được đóng gói trong container và mỗi container chưa 1 service
    + Service Instance per VM: package  service là 1 virtual machine image và deploy  service là 1 hoặc nhiều instance. Ví dụ như Netflix package service của họ thành EC2 AMI và triển khai mỗi service gồm rất nhiều instance. Ở pattern này có 2 pattern khác nhau là <b> Single Service Instance per Host </b> và <b> Multiple service instances per host </b>

- Service mesh: là 1 pattern sủ dụng <a href="https://viblo.asia/p/tim-hieu-ve-service-mesh-LzD5dWaWljY"> service mesh </a> để làm trung gian việc community giữa các service.
- Sidecar: Implement cross-cutting concerns in a sidecar process or container that runs alongside the service instance
- Server-side service discovery:  client/API gateway sẽ gửi một request đến một component (ví dụ như một load balancer) chạy trên một location đã biết. Component đó sẽ gọi đến service registry và xác địh vị trí location mà request cần đến.
- Client-side service discovery: client hoặc API-gateway sẽ có được vị trí của một service instance bằng cách truy vấn một service registry.
- Service registry: là nơi để chứa các metadata của các microservice instances (bao gồm vị trí location, host port,…). Các microservice instance được đăng ký với service registry khi khởi động và sẽ hủy đăng ký khi bị shut down. Các thành phần khác cần tìm thông tin của một microservice nào đó thì sẽ tìm thông qua service registry.
- API-Gateway: API-Gateway cho phép bạn sử dụng API được quản lý qua REST/HTTP. Do đó, bạn có thể expose các business functions của mình được triển khai dưới dạng microservice thông qua API-Gateway dưới dạng một API được quản lý.

### Application Infrastructure patterns

- Microservice chassis, Service Template: chassis là 1 khung các service base mà có thể sử dụng lại ở nhiều service khác nhau (ví dụ Security, logging, configuration, metric, tracing,...) gọi chung là generic cross-cutting concerns.
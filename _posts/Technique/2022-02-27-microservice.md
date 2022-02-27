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


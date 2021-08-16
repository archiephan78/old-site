---
layout: post
title: title
category: tech
---
---

# OAuth2 với OpenID

## 1. OAuth2 là gì?

- Nó là viết tắt của Open với <b>Authentication</b> hoặc <b>Authorization</b>

- OAuth ra đời nhằm giải quyết vấn đề trên và xa hơn nữa, đây là một phương thức chứng thực giúp các ứng dụng có thể chia sẻ tài nguyên với nhau mà không cần chia sẻ thông tin username và password.

## 2. OAuth2 Roles

- OAuth define 4 roles:

    + <b> Resource Owner:</b> là user đã authorizes vào application để access account của user. Quyền truy cập của application vào account của user bị giới hạn trong phạm vi ủy quyền được cấp.

    + <b>Client:</b> là application

    + <b>Resource Server:</b> là server chứa resource user muốn access

    + <b>Authorization Server:</b> là server verify thông tin của user để cấp token issue cho client

## 3. Abstract Protocol Flow:

### Abstract flow:

![](images/2020-08-17-oauth2-with-openid/abstract_flow.png)





title: graphql初探
date: 2019-05-08 14:07:58
tags: 
- GraphQL
- Koa
- Mongoose
categories:
- Node.js 
- MongoDB
- React
---

编写graphql的大致过程如下：
* 定义查询数据的结构，也就是schema。因为查询跟返回数据的结构几乎一样，所以首先编写schema来定义返回的数据类型。
* 定义查询方法，这里就是定义处理数据之后返回的是什么数据了。处理数据的操作类型query(查询)或mutation(变更)等。 
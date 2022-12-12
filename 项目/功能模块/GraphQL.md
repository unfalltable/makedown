# GraphQL

- Restful存在的弊端
  - 想要部分数据，但是返回的有多余的数据，资源浪费
  - 一次请求不能满足需求
- 按需索取资源、一次查询多个数据、无需划分版本
- ![image-20220718214801739](GraphQL/image-20220718214801739.png)
  - ![image-20220718214816930](GraphQL/image-20220718214816930.png)
  - ![image-20220718224349503](GraphQL/image-20220718224349503.png)
  - ![image-20220718225947855](GraphQL/image-20220718225947855.png)
  - ![image-20220719170929363](GraphQL/image-20220719170929363.png)
- GraphQL-Java
  - ![image-20220719171235358](GraphQL/image-20220719171235358.png)
    - 需要添加第三方库才能下载使用，Maven配置第三方库
      - ![image-20220719171330630](GraphQL/image-20220719171330630.png)

  - 使用SDL构建
    - 在resources下创建user.graphqls
      - ![image-20220719181529136](GraphQL/image-20220719181529136.png)

    - ![image-20220719182421235](GraphQL/image-20220719182421235.png)
    - ![image-20220719182509094](GraphQL/image-20220719182509094.png)
    - ![image-20220719182546485](GraphQL/image-20220719182546485.png)
  - 对象嵌套
  - 参数可以是变量，服务器需要处理变量，
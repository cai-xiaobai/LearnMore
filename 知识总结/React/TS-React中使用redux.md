TS-React中追加redux

如若要将数据能在全局获取

1. 在modules文件夹创建对应文件夹，例如：auth

2. 创建（按顺序）

   actionTypes.ts ; 定义 State、Action 的接口

   actionCreators.ts ; 实例化Action，给予功能

   index.ts ; 存放默认 Store，和 

   ​				Reducer(纯函数，接收旧的state和action，返回新的state)
# Sequzlize-cli工具的使用（种子）

## 1、 种子
构建项目中需要使用到的一些测试数据。
这就要用到 <font color=#FF7F50>`Sequelize-cli`</font> 提供的种子：  <font color=#FF7F50>`seeder`</font> 脚本。

## 2、创建种子脚本
执行种子文件生成命令
```shell
sequelize seed:create --name UserInit
```

## 3、编写种子脚本
与 <font color=#FF7F50>`migration`</font> 脚本类似，它也有 <font color=#FF7F50>`up`</font> 与 <font color=#FF7F50>`down`</font> 。
```js
const crypto = require('crypto');

module.exports = {
  up: async (queryInterface, Sequelize) => {
    let md5 = crypto.createHash('md5');
    let password = md5.update('123456').digest('hex');
    let date = new Date();
    //批量插入
    return queryInterface.bulkInsert('User', ['admin','mt','leo','reci'].map((name,index) => {
      return {
        id: index + 1,
        name,
        password,
        createdAt: date,
        updatedAt: date
      }
    }));
  },

  down: async (queryInterface, Sequelize) => {
    return queryInterface.bulkDelete('User',null, {});
  }
};
```
**bulkInsert**
批量插入数据
**bulkDelete**
批量删除

## 4、 执行种子脚本
执行命令
```shell
sequelize db:seed:all
```
撤销/回滚
```shell
sequelize db:seed:undo:all
```


# sequelize 迁移 和 种子

## 迁移

### 创建表 

```shell
./node_modules/.bin/sequelize migration:create --name
UserInit
```



### 执行迁移脚本

```shell
sequelize db:migrate
```



### 执行down

单次撤销（最近的一次）

```shell
sequelize db:migrate:undo
```

撤销所有

```shell
sequelize db:migrate:undo:all
```



### 更新迭代

添加一个新的迁移脚本

```shell
sequelize migration:create --name UserAddUpdatedAt
```

脚本

```javascript
'use strict';

module.exports = {
  up: async (queryInterface, Sequelize) => {
    //给 User 表添加列（字段）: updateAt
    return queryInterface.addColumn('User','updatedAt',{
      type: Sequelize.DATE,
      allowNull:false
    })
  },

  down: async (queryInterface, Sequelize) => {
    //删除 user 表的 updatedAt 列（字段）
    return queryInterface.removeColumn('User','updatedAt');
  }
};

```

创建外键

```javascript
// 可以创建外键
 bar_id: {
   type: Sequelize.INTEGER,

   references: {
      references: {
     // 这是引用另一个模型
     model: Bar,

     // 这是引用模型的列名称
     key: 'id',

     // 这声明什么时候检查外键约束。 仅限PostgreSQL。
     deferrable: Sequelize.Deferrable.INITIALLY_IMMEDIATE
   }
 },
```



## 种子

创建种子脚本

```shell
./node_modules/.bin/sequelize seed:create --name
UserInit
```

脚本

```javascript
'use strict';
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

执行种子脚本

执行命令

```shell
sequelize db:seed:all
```

撤销/回滚

```shell
sequelize db:seed:undo:all
```


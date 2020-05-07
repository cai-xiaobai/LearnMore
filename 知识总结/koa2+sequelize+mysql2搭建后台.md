**koa2+sequelize+mysql2搭建后台**

**本文章是根据以下链接进行二次开发，案例有所改变，将最主要的提取了出来，若需更详细的可看以下链接的文章**

**https://www.jianshu.com/p/3e35db2c8d6c**

ko2正常启动项目后即可开始以下

先安装sequelize+mysql2

```javascript
npm install sequelize --save
npm install mysql mysql2 --save
```

搭建流程

**1.配置Sequelize的数据库链接**

创建config目录，目录中创建db.js，连接数据库

```javascript
//db.js
const Sequelize = require('sequelize');
const sequelize = new Sequelize('dbname','dbusername','password',{
    host:'localhost',
    dialect:'mysql',
    operatorsAliases:false,
    dialectOptions:{
        //字符集
        charset:'utf8mb4',
        collate:'utf8mb4_unicode_ci',
        supportBigNumbers: true,
        bigNumberStrings: true
    },
    pool:{
        max: 5,
        min: 0,
        acquire: 30000,
        idle: 10000
    },
    timezone: '+08:00'  //东八时区
});

module.exports = {
    sequelize
};
})
```

代码可以直接使用，只需要将代码中实例化Sequelie对象语句中的dbname更改为你的数据库名，dbusername更改为你的数据库用户名，passoword更改为你的数据库密码，其中数据库名和数据库用户名不能为空，密码可以为空，为空时则为空的字符串就可以了。



**2.创建schema、modules、controllers**
schema:数据表模型实例
modules：实体模型
controllers：控制器

3个目录下分别创建users.js

![1588129483(1)](D:\个人资料\知识总结\图片\koa2\1588129483(1).jpg)

**3.schema数据表模型**
在schema目录下新建一个article.js文件，该文件的主要作用就是建立与数据表的对应关系，也可以理解为代码的建表。

表结构：

| 字段      | 说明             | 是否必填   |
| :-------- | ---------------- | ---------- |
| id        | 用户自增ID，主键 | 否，自动填 |
| loginName | 登录名           | 是         |
| pwd       | 密码             | 是         |
| email     | 邮箱             | 是         |
| avatarUrl | 用户头像地址     | 否         |
| headline  | 用户简介         | 否         |

**这里有两个非必填的值，是一个伏笔，为了实现还花了点时间**

在schema目录下的users.js用来创建数据表模型的，也可以理解为创建一张数据表，代码如下：

```javascript
const moment = require("moment");
module.exports = function (sequelize, DataTypes) {
  return sequelize.define('users', {
    id: {
      type: DataTypes.INTEGER.UNSIGNED,
      primaryKey: true,
      allowNull: true,
      autoIncrement: true
    },
    //登录名
    loginName: {
      type: DataTypes.CHAR,
      allowNull: false,
      field: 'loginName'
    },
    //密码
    pwd: {
      type: DataTypes.CHAR,
      allowNull: false,
      field: 'pwd'
    },
    //邮箱
    email: {
      type: DataTypes.CHAR,
      allowNull: false,
      field: 'email'
    },
    //用户头像地址
    avatarUrl: {
      type: DataTypes.TEXT,
      allowNull: true,
      field: 'avatarUrl'
    },
    //用户简介
    headline: {
      type: DataTypes.CHAR,
      allowNull: true,
      field: 'headline'
    },
    // 创建时间
    createdAt: {
      type: DataTypes.DATE
    },
    // 更新时间
    updatedAt: {
      type: DataTypes.DATE
    }
  }, {
    /**
     * 如果为true，则表示名称和model相同，即user
     * 如果为fasle，mysql创建的表名称会是复数，即users
     * 如果指定的表名称本身就是复数，则形式不变
     */
    freezeTableName: true
  });
}
```

**4.模型应用、使用**

在项目中modules目录下创建users.js文件，为用户表，该文件为用户的实例。（伏笔在这里出现了，如果两非必填项没赋值，将会报错）

```javascript
// 引入mysql的配置文件
const db = require('../config/db');

// 引入sequelize对象
const Sequelize = db.sequelize;

// 引入数据表模型
const Users = Sequelize.import('../schema/users');
Users.sync({
  force: false
}); //自动创建表

class UsersModel {
  /**
   * 创建用户模型
   * @param data
   * @returns {Promise<*>}
   */
  static async createUsers(data) {
    return await Users.create({
      loginName: data.loginName, //登录名
      pwd: data.pwd, //密码
      email: data.email, //email
      avatarUrl: '',//这两个非必填项一定要写不然
      headline: ''//就会报错。
    });
  }

  /**
   * 查询用户的详情
   * @param id 用户ID
   * @returns {Promise<Model>}
   */
  static async getUsersDetail(id) {
    return await Users.findOne({
      where: {
        id
      }
    });
  }
}

module.exports = UsersModel;
```

**5.controller 控制器**
控制器的主要作用为功能的处理，项目中controller目录下创建users.js，代码如下：

```javascript
const UsersModel = require("../modules/users");

class usersController {
  /**
   * 创建文章
   * @param ctx
   * @returns {Promise.<void>}
   */
  static async create(ctx) {
    //接收客服端
    let req = ctx.request.body;
    if (req.loginName && req.pwd && req.email) {
      try {
        //创建文章模型
        const ret = await UsersModel.createUsers(req);
        //使用刚刚创建的文章ID查询文章详情，且返回文章详情信息
        const data = await UsersModel.getUsersDetail(ret.id);

        ctx.response.status = 200;
        ctx.body = {
          code: 200,
          msg: '创建用户成功',
          data
        }
      } catch (err) {
        ctx.response.status = 412;
        ctx.body = {
          code: 412,
          msg: '创建用户失败',
          data: err
        }
      }
    } else {
      ctx.response.status = 416;
      ctx.body = {
        code: 200,
        msg: '参数不齐全'
      }
    }
  }

  /**
   * 获取文章详情
   * @param ctx
   * @returns {Promise.<void>}
   */
  static async detail(ctx) {
    let id = ctx.params.id;
    if (id) {
      try {
        // 查询文章详情模型
        let data = await UsersModel.getUsersDetail(id);
        ctx.response.status = 200;
        ctx.body = {
          code: 200,
          msg: '查询成功',
          data
        }
      } catch (err) {
        ctx.response.status = 412;
        ctx.body = {
          code: 412,
          msg: '查询失败',
          data
        }
      }
    } else {
      ctx.response.status = 416;
      ctx.body = {
        code: 416,
        msg: '用户ID必须传'
      }
    }
  }
}

module.exports = usersController;
```

**6.路由**

路由，也可以简单理解为路径，主要是作为请求的url，请求的路径来处理一些请求，返回数据。**(其实就是后台接口)**

一般情况下，基于node的项目，路由都是在一个叫做routes的目录下面。

```javascript
const Router = require('koa-router');
const UsersController = require('../controllers/users');

const router = new Router({
  prefix: '/api/v1'
});
/**
 * 用户接口
 */
//创建用户
router.post('/users/create', UsersController.create);

//获取用户详情
router.get('/users/:id', UsersController.detail);

module.exports = router
```

**7.解决跨域**

```javascript
npm install koa-cors --save
```

然后在根目录下的app.js加入koa-cors的引用：

```javascript
const cors = require('koa-cors')
app.use(cors()) //使用cors
```

重启服务就能测试接口了。






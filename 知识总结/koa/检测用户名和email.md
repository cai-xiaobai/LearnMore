```javascript
//检验用户名和email是否重复
static async checkoutUsers(data) {
    const infoList = await Users.findAll({
      attributes: ['loginName', 'email'],
    });
    //获取所有用户名
    const nameList = infoList.map(item => item.dataValues.loginName);
    if (nameList.includes(data.loginName)) {
      return 1;
    }
    //获取所有邮箱
    const emailList = infoList.map(item => item.dataValues.email);
    if (emailList.includes(data.email)) {
      return 2;
    }
    return 0;
  }
```


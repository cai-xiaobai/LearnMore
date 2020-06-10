TS - React路由

使用React构建的单页面应用，要想实现页面间的跳转，首先想到的就是使用路由。

在React中，常用的有两个包可以实现这个需求，那就是 react-router 和 react-router-dom

安装

进入项目目录，使用npm安装 react-router-dom

npm install react-router-dom @types/react-router-dom

封装路由：

路由配置：router/index.ts

```react
import Login from '../views/Login'
import Home from '../views/Home'

const routes = [
    {
        path:'/',
        component:Login
    },
    {
        path:'/login',
        component:Login
    },
    {
        path:'/home',
        component:Home
    }
];

export default routes;
```

定义route interface：utils/interface.ts

```typescript
export interface RouterInterface {
    path:string,
    component: any,
    routes?: Array<any>
}
```

路由方法 assets/common.tsx

```react
import React from 'react';
import { Route } from 'react-router-dom';
import { RouterInterface } from '../utils/Interface';

const RouteWithSubRoutes = (route: RouterInterface, index: number) => {
    return (
        <Route 
            key={index}
            path={route.path}
            render={(props) => (
                <route.component {...props} routes={route.routes} />
            )}
        />
    );
}

export {RouteWithSubRoutes}
```

### 调用路由App.tsx

最后在页面使用，调用路由方法显示解析各个组件页面。

```react
import React from 'react';
import { 
  BrowserRouter as Router,
  Switch
} from 'react-router-dom';
import routes from './router/index';
import { RouteWithSubRoutes } from './router/common';
import { RouterInterface } from './utils/Interface';
import './App.css';
import 'antd/dist/antd.dark.css';

function App() {
  return (
    <Router>
      <div className="App">
        <Switch>
          {routes.map((route: RouterInterface, i:number) => {
            return RouteWithSubRoutes(route,i)
          })}
        </Switch>
      </div>
    </Router>
  );
}

export default App;

```


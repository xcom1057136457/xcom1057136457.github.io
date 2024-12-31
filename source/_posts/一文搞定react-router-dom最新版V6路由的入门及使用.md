---
title: 一文搞定react-router-dom最新版V6路由的入门及使用
date: 2024-12-31 13:30:56
---
> 本文主要以react-router-dom v6版本为主，旧版过多概念不介绍，想必诸位应该也知道该库是做什么用的 😅，主要引领大家入门了解 react-router-dom 的使用，快速上手最新 React Router v6 版本。

## 1. 安装

> 首先，确保你已经安装了Node.js和npm或yarn。然后，在项目的根目录下运行以下命令来安装react-router-dom。

👉 打开你的终端并使用 Vite 引导一个新的 React 应用程序：

```` shell
# 第一步执行安装命令
npm create vite@latest demo-react-vite -- --template react
# 切换到项目根目录
cd demo-react-vite
# 安装项目依赖
npm install
# 运行项目
npm run dev
# 安装react-router-dom
npm install react-router-dom
````

目前这里最新的版本为：

```` json
"react": "^18.2.0",
"react-dom": "^18.2.0",
"react-router-dom": "^6.22.2"
````

## 2. 路由模式

> React Router 支持两种路由模式：BrowserRouter 和 HashHistory。

1. BrowserRouter 模式使用 URL 中的/来定义路由，例如：http://xxx.com/about。

2. HashHistory 模式使用 URL 中的#来定义路由，例如：http://xxx.com/#/about。

默认情况下，React Router 使用 BrowserRouter 作为其历史记录。

注意：**BrowserRouter**组件最好放在最顶层所有组件之外，这样可以确保内部组件在使用 Link 做路由跳转时不会出现出错

## 3. 路由组件

> 路由组件是用于处理路由的组件。在 React Router 中，路由组件通常用于显示不同的页面或视图。

### 3.1 创建路由组件

> 在项目src目录中创建一个App.jsx的文件，并在其中编写示例代码 👇

```` jsx
import React from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
function Home() {
	return <div>Home</div>;
}

function About() {
	return <div>About</div>;
}
function App() {
	return (
		<BrowserRouter>
			<Routes>
				<Route path="/" element={<Home />} />
				<Route path="/about" element={<About />} />
			</Routes>
		</BrowserRouter>
	);
}
````

### 3.2 Route 组件

> Route组件用于定义路由规则。Route组件接收两个属性：path和element。path属性用于定义路由的路径，element属性用于定义路由对应的组件，index属性用于指定默认子路由。

* 注意**element**属性值必须写成标签的形式

* **Route**组件可以嵌套使用，用于定义更复杂的路由规则。

    * **嵌套路由**可以不在向旧版一样提供完整的路径，因此新版本路径书写变短,但访问路径还是需要完整路径如 **/about/joinus**
    ```` jsx
    <Route path="/about" element={<About />}>
	    <Route index element={<Address />} />
	    <Route path="address" element={<Address />}></Route>
	    <Route path="joinus" element={<Join />}></Route>
    </Route>
    ````
  
* 在新版中**Route**先后顺序不在重要，**Router Route**可以自动匹配正确的路径

* 使用**index**属性可以指定默认子路由

* 或通过**path**为空，来指定默认路由
```` javascript
const router = [
	{
		path: '/home',
		element: <Home />,
		children: [
			{
				path: '',
				element: <News />,
			},
			{
				path: 'news',
				element: <About />,
			},
		],
	},
];
````

* 该组件必须包含在**Routes**组件中

### 3.3 Routes 组件

**Routes**组件是之前版本种**Switch**组件的变名，抓哟用于将多个**Route**组件组合在一起。

### 3.4 导航组件 Link、NavLink

**Link** 和 **NavLink** 组件类似于 a 标签，用于创建路由链接。**Link**组件接收一个**to**属性，用于指定链接的地址，**NavLink**组件相似，区别是可以添加一些样式来区分当前页面。

#### 3.4.1 Link 组件

> **Link** 组件用于创建路由链接，它有一个参数**to**, 它的值可以是字符串还可以是一个**location**对象。

1. 字符串形式
```` jsx
<Link to="/about">关于我们</Link>
````

2. 对象形式

    类型声明，更多介绍请查看官网

    ```` typescript
    declare function Link(props: LinkProps): React.ReactElement;

    interface LinkProps extends Omit<React.AnchorHTMLAttributes<HTMLAnchorElement>, 'href'> {
	    to: To;
	    preventScrollReset?: boolean;
	    relative?: 'route' | 'path';
	    reloadDocument?: boolean;
	    replace?: boolean;
	    state?: any;
	    unstable_viewTransition?: boolean;
    }

    type To = string | Partial<Path>;

    interface Path {
	    pathname: string;
	    search: string;
	    hash: string;
    }
    ````

    使用示例：

    ```` jsx
    <Link to={{ pathname: '/about', search: '?name=zhangsan' }} state={{ some: 'value' }}>
	    关于我们
    </Link>
    ````
   
#### 3.4.2 NavLink 组件

> **NavLink** 组件是 **Link** 组件的变体，可以添加一些样式来区分当前页面。

一共两种形式，如下代码示例 👇

1. 通过 style 的形式
```` jsx
<NavLink
	to="/about"
	style={({ isActive }) => {
		return {
			color: isActive ? 'red' : '#000',
			fontWeight: 'bold',
		};
	}}
>
	首页
</NavLink>
````

2. 通过 css 的形式
```` jsx
<NavLink
	to="/about"
	className={({ isActive }) => {
		return isActive ? 'active' : '';
	}}
>
	首页
</NavLink>
````

### 3.5 Navigate 组件

> **Navigate** 组件是对旧版本的 **Redirect** 的替代品。

如下代码示例 👇

```` jsx
import { Navigate } from 'react-router-dom';
<Route path="/" element={<Navigate replace to="/home" />} />;
````

* 其中replace属性也可以省略，不过路由的行为由 replace 改为 push

* replace Vs push

* replace 替换当前 history 记录，没有历史记录

* push 添加新的 history 记录，可以回退到上一页

### 3.6 Outlet 组件

> Outlet组件用于在路由被匹配时，渲染匹配到的路由组件。用于占位，告诉 React Router 嵌套的内容应该显示在哪里。

如下代码示例 👇

```` jsx
export default function App() {
	return (
		<div className="container">
			<h2>关于我们</h2>
			<ul>
				<li>
					<Link to="/about/address">公司地址</Link>
				</li>
				<li>
					<Link to="/about/join">加入我们</Link>
				</li>
			</ul>
			<div className="boxs">
				<Outlet />
			</div>
		</div>
	);
}
````

## 4. 声明式路由

### 4.1 useRoutes 声明式的路由配置方式

> 在声明式路由中，不能写index, 但可以让 path: “” , 来实现显示默认组件;

如下代码示例 👇

```` jsx
const MyRoutes = () => {
	return useRoutes([
		{
			path: '/',
			element: <Home />,
		},
		{
			path: '/home',
			element: <Home />,
		},
		{
			path: '/about',
			element: <About />,
			children: [
				{
					path: '',
					element: <Story />,
				},
				{
					path: 'address',
					element: <Address />,
				},
			],
		},
	]);
};
function App() {
	return (
		<div>
			<Router>
				<MyRoutes />
			</Router>
		</div>
	);
}
export default App;
````

## 5. 编程式导航

> 编程式导航就是通过 JS 代码来控制路由的跳转，包括路由的跳转、路由的参数传递、路由的钩子函数等。

### 5.1 路由跳转useNavigate函数

> useNavigate 函数用于获取路由导航的函数，该函数可以接收一个参数，表示要跳转到的路由地址。同时新版本中移除了useHistory 函数。
> * params传参方式，地址会如：/home/1。
> * search传参方式，地址会如：/home?id=1。
> * state传参方式，地址栏中不会显示

如下代码示例 👇

```` javascript
import { useNavigate } from 'react-router-dom';
const navigate = useNavigate();
navigate('/home'); // 默认是以push的形式跳转
````

#### 5.1.1 push的方式

> 跳转到指定路由，并生成历史记录

* 携带params参数

    路由设计格式为：`<Route path="about" element={<About/>} />`
    
    ```` javascript
    navigate(`/home/${id}`);
    ````
  
* 携带search参数

    路由设计格式为：`<Route path="about/:id/:keyword" element={<About/>} />`

    ```` javascript
    navigate(`/home/${id}/${keyword}`);
    ````
  
* 携带state参数

    路由设计格式，以上两种方式都可以

    1. 第一种
    ```` javascript
    navigate('/home', { state: { id: '1' } });
    ````
  
    2. 第二种
    ```` jsx
    <NavLink
      to={`detail`}
      state={{
        id: 1,
      }}
    >
      首页
    </NavLink>
    ````
  
#### 5.1.2 replace方式

> 跳转到指定路由，并替换当前历史记录

* 携带params参数
```` javascript
navigate(`/home/${id}`, { replace: true });
````

* 携带search参数
```` javascript
navigate(`/home/${id}/${keyword}`, { replace: true });
````

* 携带state参数
```` javascript
navigate('/home', { state: { id: '1' }, replace: true });
````

#### 5.1.3 返回上一页

使用navigate(-1)后退到前一页，navigate(-2)后退到前两页。

### 5.2 获取路由参数

通过React Router提供的函数，从而获取解析地址栏中的参数。

注意：在最新版路由中组件不能直接从props中获取参数

#### 5.2.1 useParams 函数

> useParams 函数用于获取地址栏中的params参数，其路由地址参考为localhost:3000/home/1。

如下代码示例 👇

```` jsx
import { useParams } from 'react-router-dom';
function Home() {
	// 当前路径/home/1
	const params = useParams();
	console.log(params); // 输出：{ id: '1' }
	return <div>Home</div>;
}
````

#### 5.2.2 useSearchParams 函数

> useSearchParams 函数用于获取地址栏中的search参数，其路由地址参考为localhost:3000/home?id=1。

其中返回的searchParams函数，具有以下方法：

* searchParams：表示地址栏中的search参数，通过.get方法获取指定数据。

* setSearchParams：表示设置地址栏中的search参数。

* 在更改地址栏中的search参数时，必须传入所有的查询参数，否则会覆盖已经有的参数

如下代码示例 👇

```` jsx
import { useSearchParams } from 'react-router-dom';

function Home() {
	// 当前路径为/home?id=1
	const [searchParams, setSearchParams] = useSearchParams();
	console.log(searchParams.get('id')); // 输出：1
	// setSearchParams({ id: '2' }); // 设置地址栏中的search参数为id=2
	return (
		<div>
			<h1>Home</h1>
			<button onClick={() => setSearchParams({ id: 2 })}>修改search参数</button>
		</div>
	);
}
````

#### 5.2.3 useLocation 函数

> useLocation 函数用于获取地址栏中的search和params参数。
> 返回的location对象，具有以下属性：
> * pathname：表示地址栏中的路径，地址如：/home?id=1。
> * search：表示地址栏中的search参数，地址如：/home/1。
> * state：表示传递的state参数，该参数不会在地址栏中展示。

如下代码示例 👇

```` jsx
import { useLocation } from 'react-router-dom';
function Home() {
	const location = useLocation();
	console.log(location.pathname); // 输出：/home
	console.log(location.search); // 输出：?id=1&keyword=react
	console.log(location.state); // 输出：{ id: '1' }
	// 可以直接结构
	const {
		pathname,
		search,
		state: { id },
	} = useLocation();
	console.log(id); // 输出：1
	return <div>Home</div>;
}
````

### 5.3 类组件中获取路由参数

> 在类组件中不可以和函数组件使用的方式一致，需要通过HOC高阶组件封装，如直接使用会报错。
> 报错信息：
> `Uncaught Error: Invalid hook call. Hooks can only be called inside of the body of a function component. This could happen for one of the following reasons:`
> 1. `You might have mismatching versions of React and the renderer (such as React DOM)`
> 2. `You might be breaking the Rules of Hooks`
> 3. `You might have more than one copy of React in the same app.`

大致意思就是说无效的hooks调用，hooks只能在函数组件的主体内部调用，那么已经使用了class组件的形式，又不想修改为函数组件，要如何使用呢？答案就是使用HOC高阶组件包装一下。封装如下HOC代码 👇

```` jsx
import { useParams, useSearchParams, useLocation, useNavigate } from 'react-router-dom';

function withRouter(Component) {
	return function RouterProps(props) {
		const params = useParams();
		const location = useLocation();
		const searchParams = useSearchParams();
		const navigate = useNavigate();
		return <Component {...props} router={{ params, location, searchParams, navigate }} />;
	};
}
````

使用方式如下 👇

```` jsx
import React from 'react';
import withRouter from './withRouter';

class ClassComponent extends React.Component {
	componentDidMount() {
		const params = this.props.params;
		console.log('params：', params);
	}
}
export default withRouter(ClassComponent);
````

## 6. 路由懒加载

> 在React中，使用import()函数来实现路由的懒加载。通过import()函数，可以异步加载路由组件，提高页面加载速度。

1. 创建一个Suspense组件，用于包裹需要懒加载的路由组件。

2. 使用@loadable/component库来加载路由组件。

    * 👉 安装依赖

    ```` shell
    # npm的形式
    npm install @loadable/component
    # yarn的形式
    yarn add @loadable/component
    ````
   
    * 如下代码示例 👇

    ```` jsx
    import React from 'react';
    // 导入@loadable/component，来加载路由
    import loadable from '@loadable/component';

    const Home = loadable(() => import('...component path'));
    const About = loadable(() => import('...component path'));
    
    function App() {
        return (
            <div>
                <BrowserRouter>
                    <Routes>
                        <Route path="/" element={<Home />} />
                        <Route path="/about" element={<About />} />
                    </Routes>
                </BrowserRouter>
            </div>
        );
    }

    export default App;
    ````
   
    * 动态路由的形式如下 👇

    ```` javascript
	import React from 'react';
	import { BrowserRouter, Routes, useRoutes } from 'react-router-dom';
	import loadable from '@loadable/component';
	// 假设动态
	const menuList = [
		{
			path: '/index',
			name: '首页',
			elementPath: 'index',
		},
		{
			path: '/user',
			name: '用户管理',
			elementPath: '/user/index',
			children: [
				{
					path: '',
					name: '用户列表',
					elementPath: 'user/list',
				},
				{
					path: '/user/add',
					name: '添加用户',
					elementPath: 'user/add',
				},
			],
		},
	];

	function bindRouter(list) {
		let arr = [];
		list.map((item) => {
			const ComponentNode = loadable(() => import(`@/pages/${item.elementPath}`));
			if (item.children && item.children.length > 0) {
				arr.push({
					path: item.path,
					element: <ComponentNode />,
					children: [...bindRouter(item.children)],
				});
			} else {
				arr.push({
					path: item.path,
					element: <ComponentNode />,
				});
			}
		});
		return arr;
	}

	function App() {
		const route = bindRouter(menuList);
		const GetRoute = useRoutes(route);
		return (
			<div>
				<BrowserRouter>
					<Routes>
						<GetRoute />
					</Routes>
				</BrowserRouter>
			</div>
		);
	}
	export default App;
	````

到这里相信你也已经了解了，React Router的使用方式了，是不是很简单呢，接下来就可以开始你的业务代码了，如果本文对你有帮助，还请不要吝啬你的 👍。
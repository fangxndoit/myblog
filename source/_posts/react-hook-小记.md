---
title: react-hook 小记
date: 2020-03-26 10:12:32
tags:
---

#### 模拟React的生命周期
* constructor：函数组件不需要构造函数。你可以通过调用 useState 来初始化 state。
* componentDidMount：通过 useEffect 传入第二个参数为[]实现。
* componentDidUpdate：通过 useEffect 传入第二个参数为空或者为值变动的数组。
* componentWillUnmount：主要用来清除副作用。通过 useEffect 函数 return 一个函数来模拟。
* shouldComponentUpdate：你可以用 React.memo 包裹一个组件来对它的 props 进行浅比较。来模拟是否更新组件。

#### React Hook进行数据请求
Class组件 我们通常在<font color="red"> componentDidMount </font>生命周期发起请求，但是使用Hook时，发送请求使用的是<font color="red"> useEffect </font>

``` bash
// javascript
import React,{ useState,useEffect } from 'react';

export default function App() {
  const [data, setData] = useState(null);
  useEffect(() => {
    const fetchData = async () => {
      const result = await axios(
        "https://test.com/example/queryList"
      );
      setData(result.data); // 赋值获取后的数据
    };
    fetchData();
  });
  return (
    <div>
      {data ? (
        <ul>
          <li>{`id：${data.id}`}</li>
          <li>{`title：${data.title}`}</li>
        </ul>
      ) : null}
    </div>
  );
}
```
如果 <font color="red"> useEffect </font> 第二个参数不传入，导致每次data更新都会执行，这样就陷入死循环循环了。需要改造下

``` bash
// javascript
  ···
  useEffect(()=>{
    ···
  },[])
  ···
```

我们一个程序会有多个组件，很多组件都会有请求接口的逻辑, 为了统一处理，一般我们会使用<font color="red"> 自定义Hook </font>抽离出请求，作为一个公共的Hook

``` bash
// javascript
// config =>  期望格式
// {
//     method: 'post',
//     url: '/user/12345',
//     data: {
//         firstName: 'Fred',
//         lastName: 'Flintstone'
//     }
// }
function useFetchHook(config){
    const [data,setData] = useState(null);
    useEffect(() => {
        const fetchData = async () => {
            const result = await axios(config);
            setData(result.data)
        };
        fetchData();
    },[]);
    return { data }
}
```
引用<font color="red"> useFetchHook </font>就可以直接调用

（优化）传入监听值
``` bash
// javascript
// watch => 期望格式是 []
function useFetchHook(config,watch){
    const [data,setData] = useState(null);
    useEffect(() => {
        ...
    },
    watch?[...watch]:[] // 判断是否有需要监测的属性
    );
    return { data }
}
```
（优化）添加请求状态
``` bash
// javascript
function useFetchHook(config,watch){
    // status 标识当前接口请求状态 0：请求中 1：请求成功 2：请求失败
    const [status,setStatus] = useState(0);
    const [data,setData] = useState(null);
    useEffect(() => {
        try{
            ...
            setStatus(1) // 成功
        }catch(err){
            setStatus(2) // 失败
        }
    },
    watch?[...watch]:[] // 判断是否有需要监测的属性
    );
    return { data, status }
}
```
常用的如下，如需优化loading toast... 可以动态添加
``` bash
// javascript
function useFetchHook(config, watch) {
    const [data, setData] = useState(null);
    const [status, setStatus] = useState(0);
    useEffect(
        () => {
        const fetchData = async () => {
            try {
            const result = await axios(config);
            setData(result.data);
            setStatus(1);
            } catch (err) {
            setStatus(2);
            }
        };

        fetchData();
        },
        watch ? [watch] : []
    );
    return { data, status };
}
```

#### 常用提高性能
``` bash
// javascript
class App extends Component{
    render() {
        return 
        <div>
            <Button onClick={ () => { console.log('do something'); }}  />
        </div>;
    }
}
```
上面App组件如果props发生改变时，就会重新渲染组件。如果这个修改并不涉及到Button组件，但是由于每次render的时候都会产生新的onClick函数，react就认为其发生了改变，从而产生了不必要的渲染而引起性能浪费

``` bash
// javascript
class App extends Component{
    constructor(){
        super();
        this.buttonClick = this.buttonClick.bind(this);
    }
    render() {
        return 
        <div>
            <Button onClick={ this.buttonClick }  />
        </div>;
    }
}
```
在类组件中我们可以直接将函数绑定到this对象上，
但是在Hook上可以使用<font color="red"> useCallback </font>
``` bash
// javascript
function App(){
    const buttonClick = useCallback(
        () => { console.log('do something'),[]
    )
    return(
        <div>
            <Button onClick={ buttonClick }  />
        </div>
    )
}
```

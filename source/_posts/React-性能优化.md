---
title: React 性能优化
date: 2022-06-20 08:20:11
tags:
---

#### `shouldComponentUpdate`

```react
class MyComponent extends React.Component{
  shouldComponentUpdate(nextProps){
    return nextProps.value !== this.props.value
  }
  render(){
    return (
      <div>{"My Component " + this.props.value}</div>
    )
  }
}
```

####`React Hooks`

```react
function SomeComp({prop1, prop2}) {
    return(
        ..
    )
}

React.memo(SomeComp, (props, nextProps)=> {
    if(props.prop1 === nextProps.prop1) {
        // don't re-render/update
        return true
    }
})
```


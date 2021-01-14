## react性能优化

### 浅比较深比较

### PureComponent
React.PureComponent用当前和之前props,state的浅比较覆写了shouldComponentUpdate的实现

### React.memo


### shouldComponentUpdate
shouldComponentUpdate会在渲染前被触发。默认返回true
```
shouldComponentUpdate(nextProps, nextState) {
	return true;
}
```

### 避免修改props或者state的值
因为直接修改并不会重新render.









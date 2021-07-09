## react首次渲染

关键方法：ReactDOM.render()，方法路径：react-dom/src/client/ReactDOMLegacy

render(element, container, callback)
 + element为react元素(组件)
 + container为react渲染根节点
 + callback为首次渲染完成的回调函数（在componentDidMount之后
render调用legacyRenderSubtreeIntoContainer方法开始渲染

legacyRenderSubtreeIntoContainer：

## 状态更新setState

## hooks
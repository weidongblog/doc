## react首次渲染

入口方法：ReactDOM.render()

# render
+ 方法路径：react-dom/src/client/ReactDOMLegacy
    1. element：react元素(组件)
    2. container：react渲染根节点
    3. callback：首次渲染完成的回调函数
+ 方法说明：render调用legacyRenderSubtreeIntoContainer方法开始渲染

# legacyRenderSubtreeIntoContainer
+ 方法路径：react-dom/src/client/ReactDOMLegacy
    1. parentComponent：父组件，通常为空，仅在unstable_renderSubtreeIntoContainer方法内使用
    2. children：渲染子节点
    3. container：渲染根节点
    4. forceHydrate：是否为混合渲染
    5. callback：首次渲染完成的回调函数
+ 方法说明：获取container上的_reactRootContainer属性，如果_reactRootContainer不存在则通过legacyCreateRootFromDOMContainer方法创建；获取_reactRootContainer的_internalRoot属性作为fiberRoot，初始化更新，调用unbatchedUpdates和updateContainer

# unbatchedUpdates
+ 方法路径：react-reconciler/src/ReactFiberWorkLoop.new

## 状态更新setState

## hooks


react默认关闭最新协调调度，即enableNewReconciler=false，本文仅针对最新代码
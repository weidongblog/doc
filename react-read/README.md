## react首次渲染

入口方法：ReactDOM.render()

# render
+ 方法路径：react-dom/src/client/ReactDOMLegacy
+ 方法参数：
    1. element：react元素(组件)
    2. container：react渲染根节点
    3. callback：首次渲染完成的回调函数
+ 方法说明：render调用legacyRenderSubtreeIntoContainer(null,element,container,false,callback)方法开始渲染

# legacyRenderSubtreeIntoContainer
+ 方法路径：react-dom/src/client/ReactDOMLegacy
+ 方法参数：
    1. parentComponent：父组件，通常为空，仅在unstable_renderSubtreeIntoContainer方法内使用
    2. children：渲染子节点
    3. container：渲染根节点
    4. forceHydrate：是否为混合渲染
    5. callback：首次渲染完成的回调函数
+ 方法说明：获取container上的_reactRootContainer属性，如果_reactRootContainer不存在则通过legacyCreateRootFromDOMContainer方法创建；获取_reactRootContainer的_internalRoot属性作为fiberRoot，初始化更新，调用unbatchedUpdates和updateContainer(children,fiberRoot, parentComponent,callback)

# unbatchedUpdates
+ 方法路径：react-reconciler/src/ReactFiberWorkLoop.new
+ 方法参数：
    1. fn：回调函数
+ 方法说明：将执行上下文置为0b0001000，然后尝试执行回调函数；如果回调函数报错，则重置执行上下文，首次渲染执行上下文为0b0000000(false)，并调用resetRenderTimer重置渲染时间，并执行flushSyncCallbackQueue刷新调度队列（首次渲染不做任何操作）；

# updateContainer
+ 方法路径：react-reconciler/src/ReactFiberReconciler.new
+ 方法参数：
    1. element：渲染对象
    2. container：fiber对象
    3. parentComponent：父组件
    4. callback：回调函数
+ 方法说明：获得当前渲染fiber的current，即当前fiber树；调用requestEventTime获得更新时间eventTime；调用requestUpdateLane(current)获得更新优先级lane；调用getContextForSubtree(parentComponent)获得上下文，并设置为container的上下文（首次更新container的context为null，将container的context设置为当前上下文，否则将container的pendingContext设置为当前上下文），调用createUpdate(eventTime,lane)创建更新对象，设置当前更新对象的payload（{element:element}）和callback，调用enqueueUpdate(current,update)将当前更新对象推入更新队列；调用scheduleUpdateOnFiber(current,lane,eventTime)开始调度更新

# enqueueUpdate
+ 方法路径：react-reconciler/src/ReactUpdateQueue.new
+ 方法参数：
    1. fiber：当前fiber对象
    2. update：更新对象
+ 方法说明：判断fiber的更新队列是否为空，为空则表示当前fiber已经被卸载；判断更新队列的pending字段是否为空，将更新对象update的next设置为当前更新对象，或者将update的next设置为pending的next字段，并且更新pending的next字段为当前更新对象update；将当前更新对象的共享队列的pending设置为当前更新对象

# scheduleUpdateOnFiber
+ 方法路径：react-reconciler/src/ReactFiberWorkLoop.new
+ 方法参数：
    1. fiber：调度更新的fiber对象
    2. lane：更新优先级
    3. eventTime：更新时间
+ 方法说明：获得fiber的根节点，检查是否需要更新当前工作进程的更新优先级；当前工作进程退出状态为4则挂起当前渲染；调用getCurrentPriorityLevel获取当前渲染优先级，当lane为SyncLane时，检查是否处于非批量更新并且不在渲染中，是则调用schedulePendingInteractions(root,lane)记录挂起的交互，调用performSyncWorkOnRoot(root)执行同步任务；不是则调用ensureRootIsScheduled(root,eventTime)调度当前root的任务，并调用schedulePendingInteractions(root,lane)记录挂起的交互，当前执行上下文为空则重置渲染时间并刷新同步队列。当lane不等于SyncLane时，

## 状态更新setState

## hooks


react默认关闭最新协调调度，即enableNewReconciler=false，本文仅针对最新代码
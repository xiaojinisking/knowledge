# Portals
Portals 提供了一种很好的将子节点渲染到父组件以外的 DOM 节点的方式。


对于 portal 的一个典型用例是当父组件有 overflow: hidden 或 z-index 样式，但你需要子组件能够在视觉上“跳出（break out）”其容器。例如，对话框、hovercards以及提示框

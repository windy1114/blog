### react stack reconciliation

#### 协调
协调是指通过ReactDOM等类库使虚拟DOM和真实DOM同步。Diff是协调过程中最具代表性的一环。

#### Diff
diff的3个要点
- diff算法性能突破的关键点在于“分层对比”，它只针对相同层级的节点作对比
- 类型一致的节点才有继续diff的必要性，React 认为，只有同类型的组件，才有进一步对比的必要性
- key属性的设置，可以帮我们尽可能重用同一层级内的节点，key 属性能够帮助维持节点的稳定性

#### React 15 Diff过程
它是以树递归的方式进行diff。


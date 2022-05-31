# 基本概念
- 结点间的路径长度：两个结点之间的分支
- 结点的权值：附加在结点上的信息
- 结点带权路径：结点上的权值与该结点到根之间的路径长度的乘积
- 二叉树的带权路径长度**WPL**：所有叶子结点的带权路径长度之和
# 特点
- **不存在**`度为1`的结点，**只存在**`度为2`(分支结点)或`度为0`的结点(叶子结点)
- 有`n`个结点的哈夫曼树中有 `2n - 1`个结点
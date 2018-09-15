---
title:  "NSLayoutConstraint使用过程中问题记录"
---

   
1.一开始的时候，按照自己预想的添加约束之后，还是会再控制台输出一长串约束报错，那是因为view.translatesAutoresizingMaskIntoConstraints 需要设置为NO。


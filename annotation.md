因为重点是分析btree，所以cow的代码在此不提及，该代码中cow的作用是:Clone()的tree不会立刻深拷贝，而是在修改node时深拷贝。
分析过程中，有些细节会略过，比如root为空时的插入，避免过于繁琐。


1.ReplaceOrInsert的过程：
 1. 如果当前node(t)中的items>=maxItems，将t split成两个node，然后new一个node成为新的t，children为split后的node
 2. 先查找item在root中items的位置(i，found，found是有没有精确找到)，如果found，修改item的值即可
 3. found为flase，如果当前node没有chilren，在items的对应位置i插入item
 4. 如果node存在children，判断children[i]是否需要split，然后对children[i]递归调用node.insert(item)



2.Delete的过程
 1. 先查找item在root中items的位置(i，found)，如果root没有children，删除对应的item，如果item不存在则return nil
 2. root存在children，这里要保证children[i]的items数量>minItems，使用growChildAndRemove函数，该函数的功能是先从左/右邻居拿一个item，如果左/右邻居item也不够，就和右邻居合并。这样做的原因见下
 3. 如果found为true，在当前node中删除item即可，删除后再从children[i]中拿最大的item填补在被删除item的位置
 4. 如果found为false，说明item可能在children中，递归调用node.remove(item)


3.Get的过程
 1. 查找item在root中items的位置(i，found)
 2. 如果found=true，直接返回items[i]
 3. 如果found=false，说明item可能在children中，递归调用node.get(item)
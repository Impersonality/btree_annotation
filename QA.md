1.为什么btree需要freeList
答：类似c++的malloc，用于内存管理。

2.func (t *BTree) Clone() 函数中，t.cow = &cow1的作用是什么
答：这是浅拷贝，复制一份cow的内容给Clone的tree。

3.cow的作用是什么？跟读写安全有没有关系
答：cow作用是Clone不用立刻深拷贝整个tree，在修改node时深拷贝对应的node，跟读写安全没有关系。


### 1. 写出 `dup` 函数的伪代码。
```
int dup(int oldfd) {
    PCB* current = get_current();//获取当前进程
    for (int i = 0; i < 256; ++i) {
    if (current->flip[i] == null) {//找到空的flip
        current->flip[i] = current->flip[oldfd];//让flip[i]和flip[oldfd]都指向文件
        current->flip[i]->count++;//指向文件的引用增加
        return i;
    }
    return -1;
}
```
### 2. 学习本文知识点对你来说有什么困难？

答：自己真的心浮气躁，扇自己两巴掌。

### 3. 你能理解 `open` 函数 flags 标志位 `O_APPEND` 是如何实现的吗？

答：从APUE中得到答案：

       每个进程有一个打开文件描述符表，而该表的表项中有一个文件状态标志，当前文件偏移量和一个v节点指针，v节点指针指向一个v节点表，而该表中的i节点信息中有一个当前文件长度。

       对于lseek来说，它直接修改文件描述符表项中的当前文件偏移量，并返回当前的文件偏移量，而对于O_APPEND标志，则只是将其设置到了文件表项的文件状态标志中，此时当前文件偏移量并未修改，仍然为0，只有当用户企图写入文件时，才将文件当前偏移量置为文件长度。这从另一个角度说明，如果文件以O_APPEND标志打开，则lseek对该文件的写将不起作用，因为无论lseek怎样调整当前文件偏移量，在写入时仍然会被设为文件长度而将内容添加在文件尾。（答案来自博客，并比对apue）

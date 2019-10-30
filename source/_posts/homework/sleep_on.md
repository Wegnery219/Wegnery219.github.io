---
title: sleep_on function in Linux
tags: os
comment: true
date: 2019-10-30 12:09:00
---
#### sleep on function
```
sleep_on(&bh->b_wait); //bh_wait是指向等待在当前缓冲块运行的进程

// kernel/sched.c
void sleep_on(struct task_struct **p) //task_struct* 是指向进程的指针
{
    struct task_struct *tmp;

    if(!p) 
        return;
    if(current == &(init_task.task))
        panic("task[0] trying to sleep");
    
    tmp=*p; //*p是当前b_wait指向的
    *p=current; //将b_wait指向当前进程
    current->state=TASK_UNINTERRUPTIBLE; //当前进程状态置为不可中断等待
    schedule(); //调度
    if(tmp)
        tmp->state=0; //唤醒tmp这条链上的下一个
}
```
操作系统第一次调度这个进程的时候是进程1，进程1在初始化的时候init将b_wait置为NULL。
假设进程5进程9都是想用这个缓冲块的其他进程，则b_wait和tmp的变化如下所示。
![image1](https://github.com/Wegnery219/IMAGE/blob/master/file1.png?raw=true)
用tmp链将各个内核栈串成 进程1->进程5->进程9，在执行最后的if语句`if(tmp)`的时候，按照链表上的顺序进行唤醒。b_wait永远指向这一串的最后一个，在这里是进程9。
#### 今天上课看的两个video记的一点笔记，等资源上传到课程网站上了记得要补这里
cpu 寄存器：eip:代码区放指令,ebp：动态数据区栈底,esp:栈顶
在函数初始化的时候，动态数据区第一条指向ebp原始地址，便于在这个函数结束调用清栈的时候返回给eip，告诉eip从哪里开始执行。
strcpy拷贝的时候如果长度超了，会覆盖掉原来动态数据区的值，有可能就把记录的ebp原始地址给覆盖了，导致程序清栈的时候告诉eip从哪里执行的地址是错的。
程序传的参数的栈在调用这个函数的函数的栈里，清栈的时候一起清。
今天上课之后大概更深理解了上次犯错的原因。
```
for(int i=1;i<tmp.size();i++){
    ListNode p=ListNode(tmp[i]);
    head->next=&p;
    head=head->next;
}
```
到循环结束后清栈，p已经不存在了，head->next指针会指向空。

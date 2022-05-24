---

title: strcpy函数实现

date: 2019-10-19 10:21:09

categories: [笔记, 面试, C语言]

tags: [C语言, strcpy]

---

# strcpy函数实现

strcpy函数的原型为：`char* strcpy(char* _Dest, const char* _Source);`

```C
//实现1
char * strcpy(char* _Dest, const char* _Source)
{
    //检查传入参数的有效性
    assert(NULL != _Dest);
    assert(NULL != _Source);
    if (NULL ==_Dest || NULL == _Source)
         return NULL;
    char* ret = _Dest;
    while((*_Dest++ = *_Source++) != '\0') ;
    return ret;
}

//实现2
char * strcpy(char* _Dest, const char* _Source)
{
    //检查传入参数的有效性
    assert(NULL != _Dest);
    assert(NULL != _Source);
    if (NULL ==_Dest || NULL == _Source)
         return NULL;
    char* ret = _Dest;
    int i = 0;
    for (i = 0; _Source[i] != '\0'; i++)
    {
         _Dest[i] = _Source[i];
    }
    _Dest[i] = '\0';
    return ret;
}
```

解析：

    1. 为什么要返回char*类型；
为了实现链式连接。返回内容为指向目标内存的地址指针，这样可以在需要字符指针的函数中使用strcpy,例如strlen(strcpy(str1, str2))。

    2. 源地址和目标地址出现内存重叠时，如何保证复制的正确性；
调用c运行库strcpy函数，发现即使是内存重叠，也能正常复制，但是上面的实现就不行。说明，c运行库中strcpy函数实现，还加入了检查内存重叠的机制，下面是参考代码：

```C
//my_memcpy实现重叠内存转移
char* my_memcpy(char* _Dest, const char* _Source, int count)
{
    //检查传入参数的有效性
    assert(NULL != _Dest);
    assert(NULL != _Source);
    if (NULL ==_Dest || NULL == _Source)
         return NULL;
    char* ret = _Dest;
    /**
    _Dest和_Source的内存地址有三种排列组合：
    1. _Dest和_Source没有发生重叠；
    2. _Dest的地址大于_Source的地址；
    3. _Dest的地址小于_Source的地址；
    第一种情况和第三种情况，直接从低位字节开始复制，即可；
    第二种情况，必须从高位字节开始复制，才能保证复制正确。
    */
    if (_Dest > _Source && _Dest < _Source + count - 1)
    {
         _Dest = _Dest + count - 1;
         _Source = _Source + count - 1;
         while(count--)
         {
             *_Dest-- = *_Source--;
         }
    }else
    {
         while(count--)
         {
             *_Dest++ = *_Source++;
         }
    }
    return ret;
}
```

# strcpy和memcpy的区别

strcpy和memcpy都是标准C库函数。

1. strcpy提供了字符串的复制。即strcpy只用于字符串复制，并且它不仅复制字符串内容之外，还会复制字符串的结束符。memcpy提供了一般内存的复制。即memcpy对于需要复制的内容没有限制，因此用途更广；
2. strcpy只有两个参数，即遇到‘\0’结束复制，而memcpy是根据第三个参数来决定复制的长度。

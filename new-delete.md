# new 和 delete不得不说的故事

new和delete不是函数，而是C++定义的关键字，通过特定的语法，可以组成表达式。

### malloc
```cpp
void FunTest()
{
	int *pTest = (int*)malloc(10*sizeof(int));         //开辟10个int型的空间大小
	if(pTest != NULL)
	{
		free(pTest);
		pTest = NULL;
	}
}
```

### calloc
```
void FunTest()
{
	int *pTest = (int*)calloc(10,sizeof(int));    //分配10个int型的内存块，并将其初始化为0
	if(pTest != NULL)
	{
		free(pTest);
		pTest = NULL;
	}
}
```

* malloc、calloc、realloc都是在堆上分配内存，堆上分配的空间必须由用户自己来管理，如果不释放，就会造成内存泄漏。而栈上分配的空间是由编译器来管理的，具有函数作用域，出了函数作用域后系统会自动回收，不由用户管理，所以不用用户显式释放空间。

### realloc
```
void FunTest()
{
	int *pTest = (int*)malloc(10*sizeof(int));	
	realloc(pTest,20*sizeof(int));
	//改变原有空间大小，若不能改变则会新开辟一段空间，并将原有空间的内容
	//拷贝过去，但不会对新开辟的空间进行初始化
	free(pTest);
}
```

### 内存泄漏
* 申请内存未释放
```
void FunTest()
{
	int *pTest1 = (int*)malloc(10*sizeof(int));
	*pTest1 = 0;
}
```
* 同一空间释放两次
```
void FunTest()
{
	int *pTest1 = (int*)malloc(10*sizeof(int));	//释放两次
	int *pTest2 = (int*)malloc(10*sizeof(int));	//未释放
 
	pTest1 = pTest2;
	free(pTest1);
	free(pTest2);
}
```

### operator new 和 operator delete
```
void *operator new(size_t);     //allocate an object
void *operator delete(void *);    //free an object

void *operator new[](size_t);     //allocate an array
void *operator delete[](void *);    //free an array
```

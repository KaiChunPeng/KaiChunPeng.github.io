---
layout: post
title: C++ Missing Number
categories: [技術]
tags: [C++]
image_description: true
description: 20160623 (四)
comments: true
published: true
---
 

## 前言 :

　　今天，一樣來分享一個面試題目好了，在分享之前，先寫一下雜記...，呃..一天剛開始，好像也沒甚麼雜記好寫的=3= ，好吧...那我們直接進入主題吧 


## Question

    Given an array containing n distinct numbers taken from `0, 1, 2, ..., n,` find the one that is missing from the array.

`For example`

    Given nums = `[0, 1, 3] `return `2`.

`Note`

    Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

 
### Ans 1
{% highlight ruby %}
 
int missingNumber(int* numb, int size)
{
	int sum = 0 ;
	for (int i =0  ;i < size ; i++)
	{
		if (i != numb[i])
		{	
			sum = i ;
			break; 
		}
	}
	return sum ; 
	 
}
 

{% endhighlight %}

    這是我一開始的寫法 ， 滿心期待的去提交後，發現錯了!? 原因是因為沒有使用他所規定的規格...，網頁往下看後才發現，呃..原來他要用vector的寫法=3=..好吧 ! 沒關係，那就來重寫一次 !
	
	
	
	既然講到這個 `vector` 我相信很多人會，但是應該也有許多人不會，更貼切的說應該是，有些人喜歡，有些人不喜歡....因為他有一種樣板味(?)..好了離題了Orz
	
	
	
`vector` (這部分會的朋友，請直接跳過)
    
	vector 相當於陣列 ， 在建立vector時，我們通常會使用 <type>來表示型態，由於vector是使用 動態記憶體配置 ，所以可以用初始化的引數，來指出需要多少個element 。
	
	
	讓我們來看看[MSDN](https://msdn.microsoft.com/zh-tw/library/9xd04bzs.aspx) 怎麼介紹  

	
	
	
	
	
{% highlight ruby %}

   template < 
              class Type, class Allocator = allocator<Type> 
            >

	type      : The element data type to be stored in the vector
	allocator : The type that represents the stored allocator object that encapsulates details about the vector's allocation and deallocation of memory.  This argument is optional and the default value is allocator<Type>.  
	
	

	這是他的寫法，不過我更喜歡，用最簡單最直接的方式來教學
	
	#include <vector> //Header !
	using namespace std;//這個建議大家不要用，之後會在有詳細介紹
	
	vector <int>FirstVector (5) ; 	//have 5 integers 


    你以為只有這樣就結束了嗎 ? 是的 ，我保證一行就玩死你了。
	
{% endhighlight %}	
	

### 做事不能 : 知其然而不知其所以然 ..

	一位程式界的先輩的座右銘，在這裡讓小弟引用一下，好了，廢話不多說，讓我們開始 re 一遍這一行吧 ! 

	
	
{% highlight ruby %}

    vector <int>first

1.首先，我們會先進到 vector 這個函數。
	explicit vector(size_type _Count) //size_type _Count = 5 ;
		: _Mybase()
		{	// construct from _Count * _Ty()
		_Construct_n(_Count, _Ty());
		}
	
	
					//size_type = 5 , _Val = 0 
2.
void _Construct_n(size_type _Count, const _Ty& _Val) 
{	// construct from _Count * _Val
	if (_Buy(_Count))-------------->2.1
	{	// nonzero, fill it
		_TRY_BEGIN
		_Mylast = _Ufill(_Myfirst, _Count, _Val);
		_CATCH_ALL
		_Tidy();
		_RERAISE;
		_CATCH_END
	}
}
 
2.1
bool _Buy(size_type _Capacity)//size_type _Capacity = 5 
{	// allocate array with _Capacity elements
		_Myfirst = 0, _Mylast = 0, _Myend = 0;
	if (_Capacity == 0) //check 
		return (false);
	else if (max_size() < _Capacity) ---------------------------->2.1.1
		_Xlen();	// result too long
		else
			{	// nonempty array, allocate storage
			_Myfirst = this->_Alval.allocate(_Capacity);
			_Mylast = _Myfirst;
			_Myend = _Myfirst + _Capacity;
			}
		return (true);
		}
		
2.1.1
size_type max_size() const
{	// return maximum possible length of sequence
	return (this->_Alval.max_size());------------------------->2.1.2
}
		
	2.1.2
	_SIZT max_size() const _THROW0()
	{	// estimate maximum array size
	_SIZT _Count = (_SIZT)(-1) / sizeof (_Ty);
	return (0 < _Count ? _Count : 1);
	}
		

--------------------------------------------------	
 now , we back to 2.1

 else
{	
	_Myfirst = this->_Alval.allocate(_Capacity); 
	{
		return (_Allocate(_Count, (pointer)0));
{
	_Ty _FARQ *_Allocate(_SIZT _Count, _Ty _FARQ *)//_SIZT _Count = 5
	{	// check for integer overflow
		if (_Count <= 0)
			_Count = 0;
		else if (((_SIZT)(-1) / _Count) < sizeof (_Ty))
			_THROW_NCEE(std::bad_alloc, NULL);

			// allocate storage for _Count elements of type _Ty
		return ((_Ty _FARQ *)::operator new(_Count * sizeof (_Ty)));
	}
}		
{
	void *__CRTDECL operator new(size_t size) _THROW1(_STD bad_alloc)
	{
		void *p;
		while ((p = malloc(size)) == 0) //size = 20
		if (_callnewh(size) == 0)
		{       // report no memory
			static const std::bad_alloc nomem;
				_RAISE(nomem);
		}

		return (p);
	}
}
				
			
			
	}
	//now : _Myfirst have memory location
	_Mylast = _Myfirst;
	_Myend = _Myfirst + _Capacity;
}
 
 
 
 回到 
 2. void _Construct_n (size_type _Count  //5
							, const _Ty& _Val //0)
{
	if (_Buy(_Count))
	{	// nonzero, fill it
		_TRY_BEGIN
		_Mylast = _Ufill(_Myfirst, _Count, _Val); 
		//_Myfirst 這個我們剛剛有看到，已經擁有了memory location了
		_CATCH_ALL
		_Tidy();
		_RERAISE;
		_CATCH_END
	}
		
有趣的是，是在_Ufill 裡面有一個_Fill_n()這個function 會把element放進去
_Fill_n(_CHECKED_BASE(_First), _Count, _Val
{
	for (; 0 < _Count; --_Count, ++_First)
		*_First = _Val;
}

		
	然後，我們就可以看到 vector <int>first = first[5] (0,0,0,0,0) 了
 
 
 {% endhighlight %}	
	
	
	
	
    接下來，我們來簡單的使用它吧 ! 
	
{% highlight ruby %}
	用法很簡單，就跟我們用array 一樣就行了
	
	vector <int> first (5) ; 
	int n = first.size() ;   //int n = sizeof (first) / sizeof (first[0]) ; 
	
	for (int i = 0 ; i < n ; i++)
	{
		cin >> first[i] ; //input : 7 9 9 7 9 
	}
	
	for (int i = 0 ; i < n ; i++)
	{
		cout << first[i] <<" " ; //output : 7 9 9 7 9 
	}
	
 {% endhighlight %}	
	


    ※ 其實，我本來還想介紹iterator (迭代器) 和 STL (standard Template Library ) 的，不過這樣似乎離本文章越跑越遠去了，所以我就不在多介紹了。


    
	
    還記得我們的題目嗎 ?
	{% highlight ruby %}
	Given nums = `[0, 1, 3] `return `2`.
	{% endhighlight %}

    接下來，我使用vector來呈現
	

	
	
	
	
{% highlight ruby %}
	
int missingNumber (vector <int> & x )
{
	int result = 0 ; 

	for (int i = 0 ; i < x.size() ; i++)
		result ^= x[i] ^ (i+1) ; 

	return result ; 
}
	
	
	how can do it ?
	
	0 ^ 0 = 0
	1 ^ 0 = 1 
	0 ^ 1 = 1 
	1 ^ 1 = 0
	
 {% endhighlight %}		
	

這樣就可以也可以達到我要的，只是不知道為什麼-_- 一直不讓我提交答案。


### 結論: 
    只能說牌版真的很難..，弄這一篇會這麼久的原因就是因為我一直不斷的上傳，然後看，然後改，然後上傳...(無窮迴圈)，搞的我途中有一度想要回痞克邦寫的感覺。
	
	
	
	今天大概就先這樣吧，該開始做事了，打算把我現在手上的案子在優化一次，這樣要走我也走得心安理得。顆顆顆顆顆....

	
	
	
	
	
	
	
	
	
	
	
	
	
	
---
layout: post
title: C++ Missing Number
categories: [技術]
tags: [C++]
image_description: true
description: 20160622 (三)
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
	
	讓我們來看看[MSDN][MSDN-Path] 怎麼介紹  

[MSDN-Path]	:https://msdn.microsoft.com/zh-tw/library/9xd04bzs.aspx

{% highlight ruby %}

   template < 
              class Type, class Allocator = allocator<Type> 
            >

	type      : The element data type to be stored in the vector
	allocator : The type that represents the stored allocator object that encapsulates details about the vector's allocation and deallocation of memory.  This argument is optional and the default value is allocator<Type>.  
	
	

	這是他的寫法，不過我更喜歡，用最簡單最直接的方式來教學
	
	#include <vector> 				//Header !
	using namespace std;			//這個建議大家不要用，之後會在有詳細介紹
	
	vector <int>FirstVector (5) ; 	//have 5 integers 


    你以為只有這樣就結束了嗎 ? 是的 ，我保證一行就玩死你了。
	
{% endhighlight %}	
	

### 做事不能 : 知其然而不知其所以然 ..

	一位程式界的先輩的座右銘，在這裡讓小弟引用一下，好了，廢話不多說，讓我們開始 re 一遍這一行吧 ! 
	

 
	
	
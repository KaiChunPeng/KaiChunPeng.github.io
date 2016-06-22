---
layout: post
title: [面試題目] ReverseString
categories: [技術]
tags: [C++]
image_description: true
description: 20160622 (三)
comments: true
published: true
---
 
  
`Content`

今天一整天都有一種毫無勁的感覺，就乾脆亂寫一些小東西-3-

`String Reverse`
一個簡單到不行的小程式，今天讓我把你玩出花樣來吧~






`先來看看我第一個怎麼寫吧 !`


{% highlight ruby %}
string reverseString (string s)
{
	string strtemp ;
	 
	for (int i = s.size() -1 ; i>=0 ; i-- )	
		strtemp += s[i] ; 
	
	return strtemp ; 
}
{% endhighlight %}






這是一個最簡單的方式，用for loop完成，
然後. . . . .  30分鐘後，我用這個方式寫出來

{% highlight ruby %}

string reverseString (string s)
{
	int i = 0  , j = s.size() -1 ; 

	while ( i < j )
	{
		swap(s[i++] , s[j--])  ;
	}


	return s;
}

{% endhighlight %}

比較不一樣的是，我不要在用一個temp ，而是改用swap的方式來完成交換。



最後一個，我把temp加進去，然後用塞的方式完成
{% highlight ruby %}
string reverseString (string s)
{
	string strTemp ; 

	for (int i = s.size() -1 ; i >= 0 ; i--)
		strTemp.push_back(s[i]) ; 



	return strTemp;
}
{% endhighlight %}




結論: 

大概就是這樣 ， 仔細想想 =_= 我只是用不同方是丟進去而已，
明天如果還有時間再來找其他的來玩看看好了。


這道題目好像是面試考題蠻常考的一個，
有需要的可以參考看看。


如果我有寫錯的地方，或有更好更有趣的方法
也歡迎寫信給我。

 
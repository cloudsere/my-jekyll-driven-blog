---
layout: article
title:  "Leetcode Add Two Numbers"
date:   2017-7-26 20:59:27 +0800
categories: js
---

啦啦啦，世界上还有什么比 Accepted 更感人的吗
<img src = "{{site.baseurl}}/img/leetcode-02/01.png"> 
答案是没有 嗯
<br/>
这道题我是用java做的，完成情况呢是：
<img src = "{{site.baseurl}}/img/leetcode-02/02.png">
我自己很开心的
<br/>
下面说一下我的写法，其实挺笨的
​<br/><br/>
~~~java

public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode temp1 = l1;
        ListNode temp2 = l2;
        
        ListNode l3 = new ListNode(0);
        
        ListNode temp3 = l3;
        int rest = 0;
        Boolean flag = false;
        
        while(temp1 != null && temp2 != null ){

            if(flag == true){
                rest = 1;
            }else{
                rest = 0;
            }
            
            if(  (temp1.val + temp2.val + rest) > 9 ){
                temp3.val = temp1.val + temp2.val + rest - 10;
                flag = true;
            }else{
                temp3.val = temp1.val + temp2.val + rest;
                flag = false;
            }
                        
            temp1 = temp1.next;
            temp2 = temp2.next;
            if(temp1 != null && temp2 !=null){
                temp3.next = new ListNode(0);
                temp3 = temp3.next; 
            }
        }
        
        ListNode temp4 = null;
        
        if(temp1 != null){
            temp4 = temp1;
        }
         if(temp2 != null){
            temp4 = temp2;
        }
        
        while(temp4 != null){
            temp3.next = new ListNode(0);
            temp3 = temp3.next;
            
            if(flag == true){
                rest = 1;
            }else{
                rest = 0;
            }
            
            if(temp4.val + rest > 9){
                temp3.val = temp4.val + rest - 10;
                flag = true;
            }else{
                temp3.val = temp4.val + rest;
                flag = false;
            }
            temp4 = temp4.next;
        }
        
        if(flag == true){
            temp3.next = new ListNode(1);
        }
        
        return l3;
    }
}
~~~

好啦，的确很傻！所以感觉学一下官方解答！

~~~java
public ListNode addTwoNumbers(ListNode l1, ListNode l2){
	ListNode l3 = new ListNode(0);
	ListNode p = l1, q = l2, curr = l3;
	int carry = 0;
	while( p != null || q!= null){
		int x = (p != null) ? p.val : 0;
		int y = (q != null) ? q.val : 0;
		int sum = x + y + carry;
		carry = sum / 10;
		curr.next = new ListNode(sum % 10);
		curr = curr.next;
		if( p != null ) p = p.next;
		if( q != null ) q = q.next;
	}
	if(carry > 0){
		curr.next = new ListNode(carry);
	}
	return l3.next;
}
~~~
这里处理进位的方式非常值得学习。其次是如果两个list位数不等，处理的方式也很好。


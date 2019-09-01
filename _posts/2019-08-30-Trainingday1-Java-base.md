---
layout: post
title: "开发培训课程，Java基础"
categories: [contest]
excerpt: "  "
---

# 开发培训课程，Java基础
***
#### 输入几个数字，求平均值
> 1)要求：实现基本功能，***注释，命名***

<% highlight ruby %>
import java.util.Scanner;
public class avage {
	public static void main(String args[]){

        Scanner in = new Scanner(System.in);
        String[] scoreList = in.nextLine().split(" ");
        //成绩数组
        int[] list1 = new int[5];
        //总分
        int sum = 0;
        System.out.print("您输入的成绩分别为：");
        for(int i=0;i<scoreList.length;i++){
            list1[i] = Integer.parseInt(scoreList[i]);
            System.out.print(list1[i]+",");
            sum +=list1[i];
        }
        System.out.println();
        System.out.println("平均分数为："+(float)sum/list1.length);
    }
}
<% endhighlight %>


#### 总结

>> 行注释
>> **方法函数前写上日期作者功能**

***


# 自定义列表，实现add(),set(),get(),size()方法，可以动态增长
> 1)实现要求：
`code`：

>
 
 <%hightlight ruby%>
 
public class Class01 {


    public static void main(String args[]){
        //使用自定义集合类
        MyList<String> myList = new MyList<String>();

        myList.add("1");
        myList.add("2");
        myList.add("3");
        myList.add("4");

        System.out.println("长度："+myList.size());

        for(int i=0;i<myList.size();i++){
            System.out.println("元素"+i+":"+myList.get(i));
        }

        myList.set(3,"5");

        System.out.println("修改后："+myList.get(3));

        MyList<Integer> myList2 = new MyList<Integer>();
        for(int i=0;i<300;i++){
            myList2.add(i);
        }
        System.out.println("长度："+myList2.size());
        System.out.println("100处元素："+myList2.get(100));


    }
}
// * 使用自定义类* 
class MyList<E>{
    public MyList(){
        element = new Object[range];
    }
    private int size;
    private int range = 100;
    private Object[] element;

    public int size(){
        return size;
    }
    public E get(int index){
        if(index>=0&&index<=size)
            return (E)element[index];
        else throw new IndexOutOfBoundsException("out of range"+index);
    }
    public void set(int index,E e){
        if(index>=0&&index<=size)
            element[index]=e;
        else throw new IndexOutOfBoundsException("out of range"+index);
    }
    public void add(E e){
        if(size==range-1){
            this.range*=2;
            Object[] templist = new Object[range];
            for(int i=0;i<size;i++){
                templist[i] = this.element[i];
            }
            this.element = templist;
        }
        element[size++]=e;
    }

<% endhightlight %>

#### 总结

> 自定义类中定义get ，set，add ,size方法,创建对象后调用自定义的方法

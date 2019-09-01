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

 `code`:
>

(```)
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
(```)

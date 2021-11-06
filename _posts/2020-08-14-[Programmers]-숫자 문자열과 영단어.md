---
layout: post
title: "[Programmers] 숫자 문자열과 영단어"
date: 2020-08-14 14:00:00
categories: Algorithm
permalink: /programmers/10번
---



# 프로그래머스 - 숫자 문자열과 영단어

```java
public class Algo1 {
    public static void main(String[] args) {
        String s1 = "...!@BaT#*..y.abcdefghijklm";
        String s2 = "23four5six7";
        String s3 = "2three45sixseven";
        String s4 = "123";

        System.out.println(solution(s1));
    }

    public static int solution(String s) {
        int answer = 0;
        String[] number = {"0", "1", "2", "3", "4", "5", "6", "7", "8", "9"};
        String[] words = {"zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"};

        for (int i = 0; i < 10; i++) {
            s = s.replace(words[i], number[i]);
        }
        answer = Integer.parseInt(s);
        return answer;
    }
}
```


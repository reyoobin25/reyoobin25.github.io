---
layout: post
title: "[Programmers] - 02"
date: 2019-06-06 16:38:00
categories: Algorithm
permalink: /programmers/02
---



# 프로그래머스

```java
package programmers;

public class Programmers02 {
	public static void main(String[] args) {
		// [[1, 4], [3, 4], [3, 10]
		int[][] a = { { 1, 4 }, { 3, 4 }, { 3, 10 } };
		solution(a);
	}

	public static int[] solution(int[][] v) {
		int[] answer = new int[2];

		int x = check(v[0][0], v[1][0], v[2][0]);
		int y = check(v[0][1], v[1][1], v[2][1]);

		answer[0]=x;
		answer[1]=y;
		
		//new int[] {x,y};
		return answer;
	}

	public static int check(int a, int b, int c) {
		int result = a;
		if (a == b) {
			result = c;
		} else if (a == c) {
			result = b;
		} else {
			result = a;
		}
		return result;
	}
}
```
---
layout: post
title: "[Programmers] - 01.Budget"
date: 2019-05-29 13:32:00
categories: Algorithm
permalink: /programmers/01
---



# 프로그래머스 - Budget

```java
package programmers;

import java.util.Arrays;

public class Budget {
	public static void main(String[] args) {
		int arr[] = { 1, 3, 2, 5, 4 };
		int budget = 9;
		System.out.println(solution(arr, budget));
	}

	public static int solution(int[] d, int budget) {
		int answer = 0;
		int sum = 0;

		Arrays.sort(d);

		for (int i = 0; i < d.length; i++) {
			sum += d[i];
			if (sum <= budget) {
				answer = d.length;
			} else {
				answer = i;
				break;
			}
		}

		return answer;
	}
}


```
---
layout: post
title: "[Programmers] - 06"
date: 2019-06-09 16:47:00
categories: Algorithm
permalink: /programmers/06
---



# 프로그래머스 

```java
package programmers;

//level2-소수 ok
public class Programmers06 {
	public static void main(String[] args) {
		int a[] = { 1, 2, 7, 6, 4 };
		System.out.println(solution(a));
	}

	public static int solution(int[] nums) {
		int answer = 0;
		int sum = 0;

		for (int i = 0; i < nums.length; i++) {
			for (int j = i + 1; j < nums.length; j++) {
				for (int k = j + 1; k < nums.length; k++) {
					sum = nums[i] + nums[j] + nums[k];
					if (sosu(sum))
						answer++;
				}
			}
		}
		return answer;
	}

	public static boolean sosu(int n) {
		if (n <= 1) {
			return false;
		} else if (n == 2) {
			return true;
		}
		for (int i = 2; i * i <= n; i++) {
			if (n % i == 0) {
				return false;
			}
		}
		return true;
	}
}

```
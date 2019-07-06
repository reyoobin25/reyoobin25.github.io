---
layout: post
title: "[Programmers] - 04"
date: 2019-06-06 16:40:00
categories: Algorithm
permalink: /programmers/04
---



# 프로그래머스 

```java
package programmers;

import java.util.Arrays;

//level2-수포자 문제 ok
public class Programmers04 {
	public static void main(String[] args) {
		int[] a = { 1, 3, 2, 4, 2 };
		int s[] = solution(a);
		for (int i = 0; i < s.length; i++) {
			System.out.print(s[i] + " ");
		}

	}

	public static int[] solution(int[] answers) {
		int[] answer = {};

		int typeA[] = { 1, 2, 3, 4, 5 };
		int typeB[] = { 2, 1, 2, 3, 2, 4, 2, 5 };
		int typeC[] = { 3, 3, 1, 1, 2, 2, 4, 4, 5, 5 };
		int cntA = 0, cntB = 0, cntC = 0;

		for (int i = 0; i < answers.length; i++) {
			if (answers[i] == typeA[i % 5]) {
				cntA++;
			}
			if (answers[i] == typeB[i % 8]) {
				cntB++;
			}
			if (answers[i] == typeC[i % 10]) {
				cntC++;
			}
		}

		int[] a = new int[3];
		a[0] = cntA;
		a[1] = cntB;
		a[2] = cntC;

		int max = Math.max(a[0], Math.max(a[1], a[2]));
		int cnt = 0;
		for (int i = 0; i < a.length; i++) {
			if (max == a[i])
				cnt++;
		}

		answer = new int[cnt];

		int j = 0;
		for (int i = 0; i < 3; i++) {
			if (max == a[i]) {
				answer[j++] = i + 1;
			}
		}

		return answer;
	}
}

```
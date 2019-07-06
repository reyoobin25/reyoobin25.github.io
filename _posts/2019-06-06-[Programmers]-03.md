---
layout: post
title: "[Programmers] - 03"
date: 2019-06-06 16:39:00
categories: Algorithm
permalink: /programmers/03
---



# 프로그래머스 

```java
package programmers;

import java.util.Arrays;

// ok 
public class Programmers03 {
	public static void main(String[] args) {
		// [1, 5, 2, 6, 3, 7, 4] [[2, 5, 3], [4, 4, 1], [1, 7, 3]]
		int a[] = { 1, 5, 2, 6, 3, 7, 4 };
		int c[][] = { { 2, 5, 3 }, { 4, 4, 1 }, { 1, 7, 3 } };
		int [] answer = new Programmers03().solution(a, c);
		for(int i=0; i<answer.length; i++) {
			System.out.print(answer[i]+" ");
		}
	}

	public int[] solution(int[] array, int[][] commands) {
		int len = commands.length;
		int[] answer = new int[len];

		for (int i = 0; i < len; i++) {
			int start = commands[i][0]-1;
			int end = commands[i][1]-1;
			int pick = commands[i][2]-1;

			int arr[] = new int[(end - start)+1];

			for (int j = start; j <= end; j++) {
				arr[j-start] = array[j];
			}
			Arrays.sort(arr);
			answer[i] = arr[pick];
		}
		
		return answer;
	}
}

```
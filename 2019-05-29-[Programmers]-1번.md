---
layout: post
title: "[Programmers] 1번"
date: 2019-05-29 13:30:00
categories: Algorithm
permalink: /programmers/1번
---



# 프로그래머스 - Player

```java
package programmers;

import java.util.Arrays;

public class Player {
	public static void main(String[] args) {
		String[] pa = { "leo", "kiki", "eden" };
		String[] co = { "eden", "kiki" };

		// "leo", "kiki", "eden"
		// "eden", "kiki"
		// "mislav", "stanko", "mislav", "ana"
		// "stanko", "ana", "mislav"
		System.out.println(solution(pa, co));
	}

	public static String solution(String[] participant, String[] completion) {
		String answer = "";

		Arrays.sort(participant);
		Arrays.sort(completion);

		int i = 0;
		for (i = 0; i < completion.length; i++) {
			if (!participant[i].equals(completion[i])) {
				answer = participant[i];
				return answer;
			}
		}

		answer = participant[i];
		return answer;
	}
}

```
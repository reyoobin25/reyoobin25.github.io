---
layout: post
title: "[Study] JAVA"
date: 2020-08-03 14:00:00
categories: Study
permalink: /study/java
---

# JAVA 8

> JAVA 8 이후 추가된 점을 알기위해서는  JAVA 8에서 변경되거나 혹은 새롭게 추가된 내용에 대해 알고 가는것이 우선시 되어야 한다.

1. 람다

   - 함수형 프로그래밍을 제공하기 위해 만들었다.

   - 사용하면서 느낀 장점 -> 코드가 간경하고 의도를 파악하기 쉽다.

     ```java
     new Thread(new Runnable() {
         public void run() {
             System.out.println("전통적인 방식의 일회용 스레드 생성");}}).start();
     
     new Thread(()->{
         System.out.println("람다 표현식을 사용한 일회용 스레드 생성");}).start();
     ```

   - 직접 적용해 본 경우 -> 임시변수가 필요 없고, 훨씬 읽기가 쉽다.

     ```java
     // 1) for문으로만 작성한 경우
     // '123','p321','p321','321_123 '넘어온 경우
     String reqCodes = "    123, p321  ,p321 , 321_123";
     	String[]  beforeDistinct = reqCodes.split(","); 
     	for (int i=0; i<beforeDistinct.length; i++) {
     		beforeDistinct[i] = beforeDistinct[i].trim();
     	}
     
     	Set<String> tempList = new HashSet<>(Arrays.asList(beforeDistinct)); 
     	//123 ,p321, 321_123	
     	
     	Iterator<String> iterator = tempList.iterator();
     	String resultList = "";
     	while(iterator.hasNext()) {
     		resultList += iterator.next();
     		if(iterator.hasNext()) {
     			resultList+=",";
     		}
     		System.out.println(resultList);
     		assertEquals("123,p321,321_123",resultList);
     ```

     ```java
     // 2) 람다로 작성할 경우
     String[]  beforeDistinct = collectionProductCode.split(",");
     String distinctResult = Arrays.asList(beforeDistinct)
         .stream() 		  
         .map(String::trim) // 각각의 요소에
         .distinct()        // 각각의 요소 중 중복 제거
         .collect(Collectors.joining(",","","")); // 각각의 요소를 “,”로 연결
     ```

2. 스트림 API (람다를 활용한 기술들)

   - 배열이나 컬렉션 등의 데이터를 접근하기 위해서는 반복문이나 Iterator를 사용해야 한다.

   - 가독성이 떨어지고 불필요한 임시변수 선언이 많아진다. (boilerplate code)

     ```java
     // 직접 적용해 본 경우
     companyProductList.addAll(
     collectionManager.selectListFor~(~).stream()
     .filter(companyProduct->companyProduct.getMinabYN().equals("N")) // 미납여부 필터링
     .collect(Collectors.toList()));
     
     // 필터링 적용
     @Test
     public void 필터테스트() {
     	List<Person> list = Arrays.asList(new Person("a",5),new Person("b",11));
     	list.stream().filter(p->p.age > 10).forEach(System.out::println);
     }
     private class Person {
     	String name;
     	Integer age;
     	Person(String name, Integer age){
     		this.name = name;
     		this.age = age;
     }
     	
     @Override
     public String toString() {
     	return "Person [name=" + name + ", age=" + age + "]";
     }
     ```

3. java.time 패키지 : Joda-Time을 이용한 새로운 날짜와 시간 API
   - 기존의 Date나 Calendar 클래스의 단점
     - Calendar 인스턴스는 가변 객체라서 도중에 값이 수정될 수 있다.
     - 윤초와 같은 특별한 상황이 고려되지 않음
     - 월을 나타낼 때 0~11로 표현함
   - time 패키지는 이러한 단점들을 보완함.
     - 불변 객체로 생성
     - 월을 1~12로 표현
     - plusDays , minusSeconds 등의 메소드가 있음.



# JAVA 9

> 이제 JAVA 8과 달라진 점이 무엇인지 확인해 보자.

1. 다양해지고 개선된 stram API

   - dropWhile, takeWhile, ofNullable 인터페이스가 추가되었다.
   - JAVA 8의 경우 for문 사용시 제한하는 부분을 구현하기 어렵다.

   ```java
   // JAVA 9의 경우 takeWhile를 사용할 수 있다.
   IntStream.iterate(1, n -> n + 1)
       	 .takeWhile(n -> n < 10)
       	 .forEach(System.out::println);
   
   // takeWhile쓰지 않고도 가능하다.
   IntStream.iterate(1, n->n<10, n->n+1).forEach(System.out::print);
   ```

2. 개선된 Optional

   - Optional ? : 메서드가 반환할 결과값이 ‘없음’을 명백하게 표현할 필요가 있고, null을 반환하면 에러를 유발할 가능성이 높은 상황에서 메서드의 반환 타입으로 Optional을 사용하자는 것이 Optional을 만든 주된 목적이다. ([공식 JAVA API문서](https://docs.oracle.com/javase/9/docs/api/java/util/Optional.html)) ([만든 개발자 설명](<https://stackoverflow.com/questions/26327957/should-java-8-getters-return-optional-type/26328555#26328555>)) 
   - 이해한 내용은 결국 NullPoinException(이하 NPE)때문에 만들어졌다고 생각한다.
   - NPE는 if지옥으로 null값을 처리할 수도 있고, 그때그때 return 해서 처리할 수 있다.

   ```java
   String text = getText();
   int length;
   if (text != null) {
   	length = maybeText.get().length();
   } else {
   	length = 0;
   }
   
   // null처리를 하지 않아도 되며 한줄로 끝이난다.
   int length = Optional.ofNullable(getText()).map(String::length).orElse(0);
   ```

   - 추가된 함수 .ifPresentOrElse() / .or() / .stream() .. -> isPresent() 및orElse() 를 사용하여 코드를 보다 명확하게 만들고 “else” 케이스를 처리하는 대신 Java9의 ifPresentOrElse() 메소드를 사용할 수 있다.

3. 그외 jigsaw, JShell등이 새로 생겼다고 한다.

   - 지금은 2020.03에 Releas된 Java 14까지 있으며, 더 자세한 내용은 공식 문서를 참고해보자
   - ([공식문서](<https://www.oracle.com/java/technologies/javase-downloads.html#JDK14>))
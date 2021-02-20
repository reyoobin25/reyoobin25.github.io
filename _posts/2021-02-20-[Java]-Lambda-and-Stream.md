---
layout: post
title: "[Java] Lamda&stream"
date: 2021-02-20 14:00:00
categories: Java&Jsp&Spring
permalink: /Java/Lamda-and-stram
---

# Why?

지금까지 회사에서 for문을 이용해서 작성했던 코드를 코드 리팩토링을 하면서 알게된 람다를 써보게 되었습니다. 굉장히 코드가 깔끔해지면서 가독성도 좋아져서 어떤 이점이 있는지도 한번 파헤쳐보고 싶어 이 글을 작성하게 되었습니다.



## Lamda?

람다는 최초로 자바 1.8에서부터 지원을 하기 시작했습니다. 따라서 람다를 사용하고 싶다면, 자바 1.8 이상이 되어야만 한다.

- java 1.8 에서부터 지원한다.

- 람다 표현식(lambda expression)이란 간단히 말해 메소드를 하나의 식으로 표현한 것이다.

- 코드를 간결하게 만들 수 있다.

- 람다는 익명 함수이다.

  - 익명함수란, 이름이 없는 함수를 의미한다. 예시 : (매개변수) -> {실행코드}

  - ```java
    // ex1) list의 sum구하기
    List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
    Integer result = list.stream().reduce(0, (a, b) -> {
                System.out.println(a + b);
                return a + b;
            });
    // 	reduce 뒤에 나오는 것을 익명함수라고 한다.
    ```



## stream

- stream은 for문과 달리 메모리를 절약할 수 있다.

  - ```java
    // ex1) list에서 짝수를 필터링 하기
    List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
    List<Integer> result = new ArrayList<>();
    for (int i = 0; i < list.size(); i++) {
      if (0 == (i % 2)) {
        result.add(i);
      }
    }
    // list를 옮기기 위해 새로운 메모리(변수 result)가 필요함
    ```

  - ```java
    // ex2) lambda를 이용한 list 필터링 하기
    List<Integer> list = Arrays.asList(1, 2, 3, 4, 5)
      .stream()
      .filter(num->(num%2==0))
      .collect(Collectors.toList());
    // 추가적인 메모리가 필요없음
    ```

- 위예 예시에서 볼 수 있듯이 코드가 직관적이게 된다. 

- [스트림의 성능과 관련된 포스팅](https://jeong-pro.tistory.com/185)

  - 스트림이 짧고 간결하다고하여 무조건 좋다고 할수는 없다!
  - 다만, 그 차이가 굉장히 미비하다고 생각하기 때문에 가독성 측면에서는 stream을 추천한다. (코딩 컨벤션으로 막혀있다면 어쩔수 없다.)
  

<img src = "/img/4d-1.png" style=" height: 200px;" class="middle-image"/>

## Refactoring

- 간단한 예시를 들면서 글을 마치도록 하겠습니다. 

```java
public class Main {
    static List<Product> productList = Arrays.asList(
            new Product("아이폰", 739123,480000, "Y", "애플"),
            new Product("후드티", 132455,53000, "Y", "폴로"),
            new Product("기프트카드", 43212,50000, "N", "CJ"),
            new Product("맥북", 65456123,2700000, "Y", "애플"),
            new Product("마스크", 32132,10000, "N", "코려")
    );

    public static void main(String[] args) {
        // "가격이 50만원 이하 & 재고가 있는거 & 브랜드가 애플인거"
        List<Product> resultList = new ArrayList<>();
        for (int i = 0; i < productList.size(); i++) {
            Product product = productList.get(i); // 지역변수 필요
            if (500000 >= product.getPrice() && product.getIsExistStock().equals("Y") && product.getBrandName().equals("애플")) {
                resultList.add(product);
            }
        }
    }


}

class Product {
    private String productName; // 상품 이름
    private int productCode; // 상품 코드
    private int price; // 상품 가격
    private String isExistStock; // 재고 여부
    private String brandName; // 브랜드 이름

    public String getProductName() {
        return productName;
    }

    public void setProductName(String productName) {
        this.productName = productName;
    }

    public int getProductCode() {
        return productCode;
    }

    public void setProductCode(int productCode) {
        this.productCode = productCode;
    }

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }

    public String getIsExistStock() {
        return isExistStock;
    }

    public void setIsExistStock(String isExistStock) {
        this.isExistStock = isExistStock;
    }

    public String getBrandName() {
        return brandName;
    }

    public void setBrandName(String brandName) {
        this.brandName = brandName;
    }

public Product(String productName, int productCode, int price, String isExistStock, String brandName) {
        this.productName = productName;
        this.productCode = productCode;
        this.price = price;
        this.isExistStock = isExistStock;
        this.brandName = brandName;
    }
}
```



- 위 코드에서 main로직 부분을 stream으로 전환하는 예시 입니다.
- 가독성이 좋아진 모습을 볼 수 있습니다.

```java
List<Product> usedStream = productList.stream()
                .filter(product -> product.getPrice() < 500000)
                .filter(product -> product.getIsExistStock().equals("Y"))
                .filter(product -> product.getBrandName().equals("애플")).collect(Collectors.toList());
```

<img src = "/img/4d-2.png" style=" height: 200px;" class="middle-image"/>

## 글을 마치며..

위 예제들은 가벼운 예제들이라 for문과 stream의 차이가 크게 없다고 생각할 수 있지만 실무에서 가독성이란 것은 굉장히 큰 무기가 될 수 있습니다. 특히 이 예제에서는 필터링하는 부분만 사용했는데, stream에서 사용할 수 있는 함수는 굉장히 많습니다. stream을 적재적소에 잘 사용해 봅시다!


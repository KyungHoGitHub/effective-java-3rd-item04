# 아이템 4.인스턴스화를 막으려거든 private 생성자를 사용하라.

<정적인 클래스가 도움이 될때>
1. java.lang.Math 나 java.utils.Arrays 처럼,
   기본 타입 값이나 배열 관련 메서드들을 모아놓을때
2. java.utils.Collections 처럼 특정 인터페이스의 구현체들에 대하여,
   그것들을 생성해주는 정적 팩토리 메서드들을 모아놓을 때
3. final 클래스와 관련한 메서드들을 모아놓을때
### 클래스의 인스턴스화를 막으려면 private 생성자를 사용 (itme01)
* 가독성이 떨어짐
* 코드 작성 시 마다 의미 인식이 필요()
* 프로그램 유지 보수의 문제
-----

#### java.lang.Math
- 계산과 관련된 정적 메소드와 상수를 담고 있다.
```java 
/**
* Don't let anyone instantiate this class.
*/
private Math() {}
```

#### java.lang.Arrays
- 배열과 관련된 정적 메서드를 담고 있다
```java
/*  
 * This class contains various methods for manipulating arrays (such as
 * sorting and searching). This class also contains a static factory
 * that allows arrays to be viewed as lists.
 */

// Suppresses default constructor, ensuring non-instantiability.
private Arrays() {}
```

#### java.lang.Collections
* map 인터페이스 타입 객체를 넣으면, SynchronizedMap을 반환해주는 정적적메소드
```java 
//from Collections 클래스
public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m){
    retrun new SynchronizedMap<>(m);
}
```
#### 인스턴스를 막으려면 private생성자 사용
* 기본 생성자를 private으로 명시해주는 이유: JAVA 는 다른 생성자가 없을 경우 컴파일러가 자동으로 기본 생성자를 만들어주기 때문입니다

```java 
public class UtilityClass{
    //기본 생성자가 만들어지는 것을 막는다(인스턴스화 방지용)
    private UtilityClass() {
        throw new AssertionError(){
        }
        ...
}

```
item04_exam01 코드
```java
public class exam01 {
    class DateUtility{
        private DateUtility(){
            /**
             * 클래스 내부에서도 호출이 안되도록 막는다.
             * 생성되면 에러 발생
             */
            throw new AssertionError();
        }
    }

    public static void main(String[] args){

         /** 적적한주석으로 직관성을 높일 수 있다
         *private 처리 후, instance화가 불가능 하다.
         */
        DateUtility dateUtility = new DateUtility()
        
    }
}
}
```
 -----
### 추상 클래스로 만드는 것으로는 인스턴스화를 막을 수 없다.
추상 클래스의 경우 상속하여 하위 클래스를 만들면 인스턴스화가 된다.
사용자는 해당 추상 클래스를 상속하여 쓰라는 의도로 오해할 수 있는데 이러면 더 큰 문제가 된다.
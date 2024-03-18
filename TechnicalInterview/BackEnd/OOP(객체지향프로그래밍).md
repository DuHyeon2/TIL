# OOP(객체지향 프로그래밍)이란?

OOP(Object Oriented Programming)이란 객체지향 프로그래밍으로, 객체로 코드를 나누어 구현하는것을 말한다.<br>
OOP엔 **추상화**, **캡슐화**, **상속성**, **다형성**이 있다.

장점
---
1. 코드 재사용이 쉽다
2. 유지보수가 쉽다

단점
---
1. 설계 과정이 복잡하다
2. 실행 속도가 상대적으로 느리다


## 1. 추상화(Abstraction) 
- 인터페이스에 의존하면서 공통적인 속성이나 기능을 묶는것
  - 예제

    ```
    //추상 클래스 Animal
    public abstract class Animal {
        String name;
        int age;
        public Animal(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public abstract void sound();

        public void info() {
            System.out.println("이름: " + name + ", 나이: " + age);
        }

        public void move() {
            System.out.println("이동");
        }
    }

    // 상속받는 Dog 클래스
    public class Dog extends Animal{
        public Dog(String name, int age) {
            super(name, age);
        }

        @Override
        public void sound() {
            System.out.println("멍멍");
        }
    }

    // 상속받는 Cow 클래스
    public class Cow extends Animal{
        public Cow(String name, int age) {
            super(name, age);
        }

        @Override
        public void sound() {
            System.out.println("음메");
        }
    }

    // 실행
    public class Main {
        public static void main(String[] args) {
            Dog dog = new Dog("멍멍이", 2);
            dog.info();
            dog.sound();
            dog.move();

            Cow cow = new Cow("황소", 3);
            cow.info();
            cow.sound();
            cow.move();
        }
    }

- 실행결과

    ![Alt text](<img/추상화 결과.png>)
## 2. 캡슐화(Encapsulation)
- 연관된 기능을 가지는 변수와 함수를 하나의 단위인 클래스로 묶어 외부에서 접근하지 못하도록 은닉하는것

  - 예제

    ```
    public class Person {

        private String name;
        private int age;
        private String address;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

        public String getAddress() {
            return address;
        }

        public void setAddress(String address) {
            this.address = address;
        }

        public void info() {
            System.out.println("이름: " + name + ", 나이: " + age + ", 주소: " + address);
        }
    }

    // 실행
    public class Main {
        public static void main(String[] args) {
            Person person = new Person();

            person.setName("김객체");
            person.setAge(20);
            person.setAddress("대구시");
            person.info();
        }
    }


- 실행결과

    ![Alt text](<img/캡슐화 결과.png>)

## 3. 상속성(Inheritance)
- 상위 클래스를 하위 클래스가 물려받게되어 멤버변수나 메소드를 그대로 사용할 수 있는것

## 4. 다형성(Polymorphism)
- 같은 이름의 메소드가 다른 클래스에선 다른 동작을 하도록 하는 것
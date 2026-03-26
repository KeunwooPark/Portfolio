---
layout: blog_post
title: "반직관적(anti-intuitive) 디자인"
permalink: /blog/anti-intuitive-design/
publish_date: 2025-07-26
excerpt: "Casey Muratori의 OOP 비판을 출발점으로, 직관적 시스템 디자인이 항상 최선이 아니며 반직관적 디자인이 필요한 경우가 있다는 생각을 정리한다."
categories: [blog]
---
최근에 인터넷에서 [Casey Muratori – The Big OOPs: Anatomy of a Thirty-five-year Mistake](https://youtu.be/wo84LFzx5nI?si=9t7BqqeWSF-xV9Fj) 영상을 보았다. 상당히 재밌게 보았고 이 영상에서 부터 확장된 생각들이 있어서 기록 하고자 한다. 내가 이 영상에서 얻은 내용은 직관적 시스템 디자인이 항상 좋지 않다는 것이다. 즉, **반직관적 (anti-intuitive) 디자인이 필요한 경우가 있는 것이다.**

[Casey Muratori – The Big OOPs: Anatomy of a Thirty-five-year Mistake](https://youtu.be/wo84LFzx5nI?si=9t7BqqeWSF-xV9Fj)

Muratori의 영상 내용을 정리하자면 다음과 같다. 

> 현대 Object-Oriented Programming (OOP)는 compile time encapsulation that matches domain model 패턴을 강조하는데 이러한 패턴은 한계가 있고 OOP의 초기에는 이러한 패턴을 제안하지 않았다.

Compile time encapsulation that matches domain model은 쉽게 이야기 해서 도메인 모델을 그대로 옮겨온 방식의 클래스 상속 구조를 이야기 한다. 가장 단순한 예시는 도형 예시이다. 교과서에 나오는 예시들을 보면 Shape 이라는 최상위 클래스를 만들고 이 클래스를 상속하는 Square, Circle, Rectangle 등의 하위 들을 만든다. 이렇게 상속 구조를 만들게 되면 컴파일 타임에 데이터들의 [캡슐화(encasulation)가](https://blog.itcode.dev/posts/2021/08/08/encapulation) 진행이 된다. 그런데 캡슐화가 도메인 모델과 일치한다는 것이다.

하지만 꼭 도메인 모델과 일치하도록 캡슐화를 할 필요는 없다. 이와 다른 방식의 구조로는 [Entity Component System (ECS)](https://en.wikipedia.org/wiki/Entity_component_system)가 있다. 이 방식은 시스템 전체에서 서로 독립적인 요소들을 클래스로 만들고 이것들을 조합해서 컴포넌트를 만드는 방식이다.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/23/ECS_Simple_Layout.svg/1920px-ECS_Simple_Layout.svg.png)
*출처: [Wikipedia - Entity Component System](https://en.wikipedia.org/wiki/Entity_component_system)*

도메인 모델과 일치하는 방식으로 캡슐화를 하면 위 이미지에서 컴포넌트 단위로 캡슐화를 하는 것과 같고 반대로 ECS 구조에서는 엔티티 단위로 캡슐화를 하는 것과 같다.

ECS의 장점은 원하는 엔티티를 제약 없이 조작 할 수 있다는 것이다. 도형 예시로 돌아가 보자. 만약 도메인 모델과 일치하게 구현 한다면 아래와 같이 구현 할 수 있다.
    
    
    // Architecture 1
    
    abstract class Shape {
      protected x: number;
      protected y: number;
    
      constructor(x: number, y:number) {
        this.x = x;
        this.y = y;
      }
    
      abstract area(): number;
    }
    
    class Circle extends Shape {
      private r: number;
     
      constructor(x: number, y: number, r: number) {
        super(x, y);
        this.r = r;
      }
    
      area() {
        return 3.14 * this.r * this.r;
      }
    }
    
    class Rectangle extends Shape {
      private w: number;
      private h: number;
     
      constructor(x: number, y: number, w: number, h: number) {
        super(x, y);
        this.w = w;
        this.h = h;
      }
    
      area() {
        return this.w * this.h;
      }
    }

이 상태에서 만약 Circle의 반지름과 Rectangle의 폭을 항상 동일하게 하는 constraint를 구현하고 싶다고 생각해 보자. 그러면 우리는 Circle과 Rectangle중 하나의 사이즈가 변하게 되면 다른 객체의 사이즈를 계속 바꿔야 한다. 또는 두 도형의 중심을 동일하게 하는 constraint를 만든다고 해보자. 이 역시 마찬가지 문제가 생긴다. 이를 위한 객체가 아마 또 필요 할 것이다.

반면에 Point, Value라는 엔티티 객체를 만든다고 생각 해보자. 그러면 하나의 Point, Value 객체를 두 엔티티가 가지도록 하는 방식으로 상당히 단순하게 constraint를 구현 할 수 있다. Contraint가 적용되는 Point나 Value를 Circle과 Rectangle이 가지도록 하면 된다.
    
    
    // Architecture 2
    
    class Value {
      private v: number;
    
      constructor(value: number) {
        this.v = value;
      }
    
      get() {return v;}
      set(value) {this.v = value;}
    }
    
    class Point {
      private x: number;
      private y: number;
    
      constructor(x: number, y:number) {
        this.x = x;
        this.y = y;
      }
    
      set(x: number, y:number) {
        this.x = x;
        this.y = y;
      }
    }
    
    abstract class Shape {
      protected center: Point;
      constructor(p: Point) {
        this.center = p;
      }
    
      abstract area(): number;
    }
    
    class Circle extends Shape {
      private r: Value;
     
      constructor(c: Point, r: Value) {
        super(c);
        this.r = r;
      }
    
      area() {
        return 3.14 * this.r.get() * this.r.get();
      }
    }
    
    class Rectangle extends Shape {
      private w: Value;
      private h: Value;
     
      constructor(c: Point, w: Value, h: Value) {
        super(c);
        this.w = w;
        this.h = h;
      }
    
      area() {
        return this.w.get() * this.h.get();
      }
    }

이러한 후자의 구조는 전자의 구조에 비해서 비직관적이게 느껴진다. 그 이유는 우리가 실제 세상을 인식할 때 개별 물체들은 독립적으로 인식하고 이들 사이의 속성을 공유할 수 있다는 생각을 잘 안하기 때문이다. 하지만 이렇게 비직관적인 방식이 더 유용한 경우가 있다. 참고로 Muratori는 애초에 Circle, Rectangle class도 필요 없고 [Discriminated Unions](https://radlohead.gitbook.io/typescript-deep-dive/type-system/discriminated-unions) 를 쓰는 것이 낫다는 식으로 이야기를 한다. 아래와 같은 식이다.
    
    
    // Architecture 3
    
    interface Value {
      v: number;
    }
    
    interface Point {
      x: number;
      y: number;
    }
    
    interface Circle {
      kind: "circle";
      center: Point;
      radius: Value;
    }
    
    interface Rectangle {
      kind: "rectangle";
      center: Point;
      width: Value;
      height: Value;
    }
    
    type Shape = Circle | Rectangle;
    
    function area(s: Shape) {
      if (s.kind === "rectangle") {
        return s.width.v * s.height.v;
      }
      else {
        return 3.14 * s.radius.v * s.radius.v;
      }
    }

세 번째 방식으로 구현하면 도형의 갯수가 많아질 수록 area 함수가 커진다는 단점이 있긴 하지만 두 번재 방법 역시 클래스의 갯수가 너무 많아진다는 문제가 있다. 그리고 함수가 커지더라도 도형별 영역을 구하는 함수를 별도로 구현하면 코드 자체를 관리하는 것이 어렵지는 않을 수 있다. 이 이야기의 핵심은 도메인 모델과 일치하는 방식으로 객체를 설계하는 것이 꼭 정답은 아니라는 것이다.

Muratori 는 영상에서 도메인 모델을 따르는 OOP 패턴이 성행하게 된 근원을 찾아서 OOP의 역사를 따라가는데 [Ivan Sutherland의 Sketchpad](https://youtu.be/6orsmFndx_o?feature=shared)까지 거슬러 올라간다. OOP의 역사에 대한 내용은 여기에 정리하지는 않겠지만 매우 흥미롭기 때문에 Muratori의 영상을 직접 보는 것을 추천한다. Sketchpad 연구는 HCI에서 워낙 유명하기 때문에 알고는 있었지만 소프트웨어 구조가 어떻게 되어 있는지는 전혀 몰랐었다. 특히나 OOP의 관점에서 의미도 몰랐다. Sketchpad의 데이터 구조를 보면 class의 개념을 가지고 있지만 discriminated union과 유사한 방식으로 구현 되어 있다.

[Ivan Sutherland의 Sketchpad 영상](https://youtu.be/6orsmFndx_o?feature=shared)

그런데 Muratori의 영상에서 Sketchpad를 보자마자 [Object Oriented Drawing](https://youtu.be/WgzdFoXLX9c?si=McK7izqXa7pleOpz) (OOD) 연구가 생각 났다. 이 연구를 학회에서 처음 보고나서 상당히 놀았었다. 당시에 나에게는 상당히 혁신적으로 다가왔다.

[Object Oriented Drawing 영상](https://youtu.be/WgzdFoXLX9c?si=McK7izqXa7pleOpz)

당시에 OOD 연구를 보았을 때는 터치 인터랙션에만 초점을 맞췄었다. 연구 자체도 터치 인터랙션에 용이한 드로잉 인터페이스에 집중해서 소개를 하기도 한다. 하지만 Muratori의 발표를 보고나서 OOD가 ECS 구조를 가지고 있기 때문에 터치 인터랙션에 용이한 인터페이스를 만들 수 있었다는 것을 알게 되었다. 기존의 form-filling 방식의 인터페이스는 도메인 모델을 따르는 구조에는 적합할지 모르겠지만 ECS에는 적합한 디자인은 아닌것 같다. OOD는 ECS 기반의 드로잉 인터페이스를 터치환경에 맞게 잘 디자인 한 사례이다. OOD의 인터랙션은 직관적이라고 느껴지지만 사실 실제 드로잉과는 개념 자체가 많이 다르다. 속성들을 직접 조작하고, 연결하는 개념은 실제에는 전혀 없는 개념이다. 인터랙션은 직관적이지만 개념은 반직관적이다.

Sutherland도 컴퓨터로 그림을 그리는 것은 실제 그림을 그리는 것과 전혀 다르다고 이야기를 했다. 실제 그림을 그릴 때는 선이나 도형의 위치를 옮기는 것은 불가능하다. 지우고 다시 그려야 하는데 컴퓨터에서는 그럴 필요가 없다. 이 관점에서 보면 선의 위치를 옮기는 행위는 어떻게 보면 직관적이지 않을 수 있다. 지우고 다시 그리는 것이 더 실제와 유사하고 직관적인 것이다.

직관성의 함정에 빠지지 말고 반직관적 디자인을 해야 할 때가 있다는 것을 배웠다. ECS는 반직관적 구조이다. OOD, Sketchpad도 반직관적이다. 하지만 유용하다. 때로는 반직관적인 방법이 더 유용할 수 있다. 이렇게 생각해 보면 시스템이나 인터랙션을 디자인 할 때 직관성을 기준으로 디자인하는 것이 얼마나 위험할 수 있는지 알 수 있다. 나도 시스템을 설계 하거나 인터랙션을 디자인 할 때 얼마나 직관적인지를 기준으로 선택 할 때가 있는데 앞으로는 그러지 않도록 주의해야 겠다. 

마지막으로 이러한 관점은 최근에 AI의 부상으로 인하여 유행하는 자연언어 기반의 채팅 인터페이스에도 해당이 된다. 많은 사람들이 채팅 인터페이스가 직관적이기 때문에 좋다고 생각하는 것 같다. 자연언어 기반의 인터페이스가 좋을 때가 분명 있지만 만약 그렇다면 그것은 직관성 때문에 그런 것이 아니라 다른 이유 때문일 것이다. 반대로 직관적이기 때문에 자연언어 기반 인터페이스를 택해야 한다는 생각을 한다면 그것은 비효율적인 인터렉션으로 이어질 가능성이 높다.

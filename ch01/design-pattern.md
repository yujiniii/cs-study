# **디자인 패턴**
디자인 패턴이란, 프로그램을 설계할 때 발생했던 문제점들을 객체각의 상호관계 등을 이용하여 해결할 수 있도록 하나의 '규약' 형태로 만들어 놓은 것을 의미.


## **싱글톤(singleton) 패턴**
하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴  
보통 데이터베이스 연결 모듈에 쓰인다  
  
**장점**  
* 사용하기 쉽고 실용적이다.   
   
**단점**  
* TDD를 할 떄에는 테스트가 서로 독립적이어야하며 테스트를 어떤 순서로든 실행할 수 있어야 하는데 싱글톤에서는 `독립적인` 인스턴스를 만들기 어렵다.  
* 모듈간의 결합이 강하다.  

```js
// 싱글톤 패턴 예시

class Singleton{
    constructor(){
        if(!Singleton.instance){
            Singleton.instance = this
        }
        return Singleton.instance
    }
    getInstance(){
        return this.instance
    }
}

const a = new Singleton();
const b = new Singleton();
console.log(a===b); // true
```
### **모듈간의 결합 느슨하게 만들기, DI**
의존성 주입(DI, Dependency Injection)  
메인 모듈이 직접 하위 모듈에 영향을 주는 것이 아니라, 중간에 의존성 주입자(dependency injector)가 이 부분을 가로채, 메인모듈이 간접적으로 의존성을 주게하는 방식  
> == 디 커플링이 된다  
상위모듈(메인모듈)은 하위 모듈에 대해 의존성이 떨어지게 된다.

**의존성 주입 원칙**  
* 상위 모듈은 하위 모듈에서 어떤 것도 가져오지 않아야 한다.  
* 둘 다 추상화에 의존해야 한다.  
* 추상화는 세부사항에 의존하지 말아야 한다.  
   
**장점**  
* 모듈들을 쉽게 교체할 수 있는 구조가 되*어, 테스팅하기 쉽고 마이그레이션 하기 수월하다.  
* 추상화 레이어를 넣고 구현체를 넣기 때문에 의존성 방향이 일관되고, 애플레이션을 쉽게 추론할 수 있다.  
* 모듈간의 관계가 좀 더 정확해진다.  
  
**단점**  
* 모듈들이 더욱 분리되므로 클래스 수가 늘어나 복잡성이 증가될수도 있다.  
* 런타임 패널티가 생기기도 한다.   

## **팩토리 패턴**
객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴.  
상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 대한 구체적인 내용을 결정하는 패턴.
느슨한 결합, 유연성 good
객체 생성 로직 분리 -> 유지보수도 good
```js
class Latte{
    constructor(){
        this.name = "latte"
    }
}
class Espresso{
    constructor(){
        this.name = "espresso"
    }
}
// ---------------------하위 인스턴스------------------------
class EspressoFactory{ // 구체적인 내용
    static createCoffee(){
        return new Espresso()
    }
}
class LatteFactory{ // 구체적인 내용
    static createCoffee(){
        return new Latte()
    }
}
// ----------------------------------------------------------

const factoryList = {LatteFactory, EspressoFactory}

// ---------------------상위 인스턴스------------------------
class CoffeeFactory{ // 중요한 뼈대 결정
    static createCoffee(type){
        const factory = factoryList[type]
        return factory.createCoffee()
    }
}
// ----------------------------------------------------------

const main = ()=>{
    const coffee = CoffeeFactory("LatteFactory") //return:new Latte(), 의존성 주입
    console.log(coffee.name) // latte
}

main()

```

## **전략(strategy) 패턴**
정책(policy)패턴이라고도 함  
객체의 행위를 바꾸고 싶은 경우 "직접" 수정하지 않고 전략이라고 부르는 캡슐화한 알고리즘을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴   
```js
var passport = require('passport')
var LocalStrategy = require("passport-local").Strategy

passport.use(new LocalStrategy(  // new LocalStrategy(~)이 부분이 전략
    function(username, password, done){
        User.findOne({username:username}, function (err, user){
            if(err){return done(err);}
            if(!user){
                return done(null, false,{message:"Incorrect username"});
            }
            if(!user.validPassword(password)){
                return done(null, false, {message:"incorrect password"});
            }
            return done(null, user);
        });
    }
));
```
## **옵저버(observer) 패턴**
주체가 어떤 객에의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴  
* 주체 : 객체의 상태 변화를 보고 있는 관찰자 (주체+객체의 형태도 존재)
* 옵저버s : 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 `추가변화사항` 이 생기는 객체들
* MVC 패턴에도 사용 / model update() -> view(observer) -> controller 동작
```js
function createReactiveObject(target, callback){
  const proxy = new Proxy(target, {
      set(obj, prop, value){
          if(value !== obj[prop]){
              const prev = obj[prop]
              obj[prop] = value
              callback(`${prop}가 [${prev}>>${value}]로 변경되었습니다.`);
          }
          return true
      }
  })
  return proxy
}
const a = {
  dinner:"미역국"
}
const b = createReactiveObject(a, console.log) 

b.dinner = "햄버거" //dinner가 [미역국>>햄버거]로 변경되었습니다. 

// 바꿀때마다 console.log(..) 실행

b.dinner = "파스타" //dinner가 [햄버거>>파스타]로 변경되었습니다. 
```

## **프록시(proxy) 패턴**
프록시 객체안에 녹아져있는 패턴  
대상 객체에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴  
객체의 속성, 변환 등을 보완한다.  
보안, 데이터검증, 캐싱, 로깅에도 사용한다.  
프록시 서버로도 사용된다  

## **이터레이터(iterator) 패턴**
이터레이터(iterator)를 사용하여 컬랙션(collection)의 요소들에 접근하는 디자인 패턴  

* 이터레이터 프로토콜 : 이터러블한 객체들을 순회할때 쓰는 규칙
* 이터러블한 객체 : 반복 가능한 객체로 배열을 일반화한 객체
```js
const mp = new Map()
mp.set('a',1)
mp.set('b',2)
mp.set('c',3)

const st = new Set()
st.add(1)
st.add(2)
st.add(3)

// 다른 자료형이지만 `for a of b` 라는 이터레이터 프로토콜을 통해 순회

for(let a of mp) console.log(a)
// ["a", 1]
// ["b", 2]
// ["c", 3]

for(let a of st) console.log(a)
// 1
// 2
// 3
```
## **노출 모듈(revealing module) 패턴**
즉시 실행 함수를 통해 접근 제어자를 만드는 패턴
* **즉시 실행 함수**
    * 함수를 정의하자마자 바로 호출하는 함수
    * 초기화코드 라이브러리 내 전역변수의 충돌 방지등에 사용
```js
// js는 접근 제어자가 없기 때문에 이런식으로 구현
// tj는 있음~~
const pukuba = (()=>{
    const a = 1
    const b = ()=>2
    const pub = {
        c:2,
        d:()=>3
    }
    return pub //공개할 부분만 export 
    // 나머지는 스코프를 벗어나 사라짐(private 하게 처리됨)
})()

console.log(pukuba) // {c: 2, d: ƒ d()}
console.log(pukuba.a) // undefined
```

## **MVC 패턴**
`M`(odel) `V`(iew) `C`(ontorller) 로 이루어진 디자인 패턴  
* **모델(Model)**
    * 애플리케이션의 데이터인 DB,상수, 변수 등
    * 뷰에서 데이터를 생성하거나 수정하면 컨트롤러를 통해 모델을 생성하거나 갱신
* **뷰(View)**
    * 사용자 인터페이스 요소(inputbox, textarea .. )
    * 모델을 기반으로 사용자가 볼 수 있는 화면
    * 모델이 가지고 있는 정보를 저장하면 X
    * 변경이 일어나면 컨트롤러에 전달해야 함
* **컨트롤러(Controller)**
    * 하나 이상의 모델과 하나 이상의 뷰를 잇는 다리
    * 이벤트 등 메인 로직 처리
    * 모델/뷰 생명주기 관리
    * 변경 통지를 받으면 이를 해석하여 양쪽에 전해줌

## **MVP 패턴**
MVC 패턴으로부터 파생  
`C`->`P`(resenter)로 교체된 패턴  
뷰와 프레젠터는 일대일 관계로 MVC보다 더 강하게 결합한 형태  

## **MVVM 패턴**
MVC 패턴으로부터 파생  
`C`->`VM`(View Model)로 교체된 패턴  
* 뷰 모델(View Model)
    * 뷰를 더 추상화한 계층
    * 커맨드와 데이터 바인딩을 가진다
    * 뷰<->뷰모델 사이의 양방향 바인딩을 지원
    * UI 재사용 good
    * 단위 테스팅이 간편
   
> **커맨드**   
: 여러가지 요소에 대한 처리를 하나의 액션으로 처리할 수 있게 하는 기법  
  
> **데이터 바인딩**   
: 화면에 보이는 데이터와 웹 브라우저의 메모리 데이터를 일치시키는 기법으로, 뷰모델을 변경하면 뷰가 변경된다.
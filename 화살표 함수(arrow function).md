# 화살표 함수(**arrow function)**

## 2.1 화살표 함수

💡 **화살표 함수**: ES6에서 도입된 (⇒)를 사용하여 함수를 선언하는 방법 

```jsx
//ES5 함수 선언 방법
const greeting = function(name{
	return "hello " + name;
};

//ES6 화살표 함수 방법
const greeting = (name) => {
	return `hello ${name}`;
};

//ES6 화살표 함수 방법(매개변수가 하나만 있는 경우)
const greeting = name => {
	return `hello ${name}`;
};

//ES6 화살표 함수 방법(매개변수가 없는 경우)
const greeting = () => {
	return "hello";
};
```

------

## 2.2 암시적 반환

- 화살표 함수를 사용하면 명시적인 반환을 생략하고 다음과 같이 반환할 수 있다.

```jsx
//ES6
const greeting = name => `hello ${name}`;

//ES5
const oldFunction = function(name){
	return "hello " + name;
};
const race = "100m dash";
const runners = ["Usain Bolt", "Justin Gatlin", "Asafa Powell"];

const results = runners.map((runners, i) => ({name: runners, race, place:i+1}));

console.log(results);
```

- 이 예에서는 **map** 함수를 사용하여 **runners** 배열에 대한 반복(iteration)을 구현한다.
- 첫 번째 인수 **runner**는 배열의 현재 원소고, i는 배열의 인덱스이다.
- 배열의 각 원소에 대해 **name**, **race**, **place**  속성을 포함하는 개체를 **results**에 추가한다.
- 중괄호 안에 있는 것이 암시적으로 반환하려는 객체 리터럴임을 자바스크립트에 알리려면, 전체를 괄호 안에 감싸야 한다.
- **race**를 나 **race:**를 쓰나 모두 결과는 동일하다.

------

## 2.3 화살표 함수는 익명 함수

- 화살표 함수는 익명 함수이다.
- 참조할 이름이 필요하다면 함수를 변수에 할당하면 된다.

```jsx
const greeting = name => `hello ${name}`;

greeting("Tom");
```

------

## 2.4 화살표 함수와 this 키워드

- 화살표 함수 내부에서 **this** 키워드를 사용할 때는 일반 함수와 다르게 동작함으로 주의해야 한다.
- 화살표 함수를 사용할 때 **this** 키워드는 상위 스코프에서 상속된다.

```html
<div class="box open">
	This is a box
</div>
.opening{
 background-color: red;
}
//box 클래스를 가진 div를 가져온다.
const box = document.querySelector(".box");
//click 이벤트 핸들러를 등록
box.addEventListener("click", function(){
	//div에 openinig 클래스를 토글
	this.classList.toggle("opening");
	setTimeout(function(){
		//클래스를 다시 토글
		this.classList.toggle("opening");
	}, 500);
});
```

- 첫 번쨰 **this**가 **const** **box**에 할당되었지만 **setTimeout** 내부의 두 번째 **this**는 **Window** 객체로 설정되어 다음과 같은 오류가 발생한다.

- `TypeError: Cannot read properties of undefined (reading 'toggle')`

- 화살표 함수는 부모 스코프에서 this의 값을 상속한다.

  

```jsx
const box = document.querySelector(".box");

box.addEventListener("click", function() {
  this.classList.toggle("opening");
  setTimeout(()=>{
    this.classList.toggle("opening");
  },500);
})
```

- 여기서 두 번째 this는 부모로부터 상속되며 const box로 설정된다.

------

## 2.5 화살표 함수를 피해야 하는 경우

💡 1.  this가 window 객체를 가리키는 경우

```jsx
const button = document.querySelector("btn");
button.addEventListener("click", () => {
  //오류: 여기서 this는 Window 객체를 가리킴
  this.classList.toggle("on");
})
const person1 = {
  age: 18,
  grow: function() {
    this.age++;
    console.log(this.age);
  },
};
person1.grow();
//19

const person2 = {
  age: 18,
  grow: () => {
    //오류: 여기서 this는 Window 객체를 가리킴
    this.age++;
    console.log(this.age);
  },
};
person2.grow();
//NaN
```



💡 2. 화살표 함수와 일반 함수의 차이점은 **arguments** 객체에 대한 접근 방식 이다.

- **arguments** 객체: 함수 내부에서 접근할 수 있는 배열 객체, 해당 함수에 전달된 인수의 값을 담고 있다.

```jsx
function example() {
  console.log(arguments[0]);
}

example(1, 2, 3);
//1
```

- arguments[0]을 사용하면 첫 번째 인수에 접근할 수 있다.
- this 키워드와 비슷하게, 화살표 함수에서 arguments 객체는 부모 스코프의 값을 상속한다.

```jsx
const showWinner = () => {
  const winner = arguments[0];
  console.log(`${winner} was the winner`);
}

showWinner("Usain Bolt", "Justin Gatlin", "Asafa Powell");
//ReferenceError: arguments is not defined
```

- 함수에 전달된 모든 인수에 접근하려면, 기존 함수 표기법이나 스프레드 문법을 사용한다.
- 여기서 arguments는 변수 이름이 아니라 키워드라는 점을 유의하자.

```jsx
const showWinner = (...args) => {
  const winner = args[0];
  console.log(`${winner} was the winner`);
};

showWinner("Usain Bolt", "Justin Gatlin", "Asafa Powell");
```

------

## Quiz

1. 다음 중 화살표 함수를 문법에 맞게 활용한 예는?

```jsx
let arr = [1, 2, 3];

//a
let func = arr.map(n -> n+1)

//b
let func = arr.map(n => n+1)

//c
let func = arr.map(n ~> n+1)

//정답:b
```

2. 다음 코드의 올바른 출력은?

```jsx
const person = {
	age:10,
	grow: () => {
		this.age++;
	},
};
person.grow();

console.log(person.age);
//11
```

3. 화살표 함수 문법을 사용해서 다음 코드를 리팩토링하자.

```jsx
function(arg) {
	console.log(arg);
}

//Refactoring
const aaa = (...arg) => {
	const bbb = arg;
	console.log(bbb);
}
```
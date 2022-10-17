# 변수(variable)

## 1.1 var, let, const

### var

💡 var 키워드로 선언된 변수는 함수 스코프에 종속된다. 반면, for문(블록 스코프) 내에서 var 키워드로 변수를 선언하면 for문 밖에서도 사용이 가능하다.



```jsx
for(var i=0; i<10; i++){
	var leak = "I am available outside of the loop";
}

console.log(leak);
//I am available outside of the loop

function myFunc() {
	var functionScoped = "I am available inside this function";
	console.log(functionScoped);
}

myFunc()
//I am available inside this function
console.log(functionScoped);
//ReferenceError: functionScoped is not defined
```

- 첫 번째 예제에서는 var의 값이 블록 스코프를 벗어나도 for 루프 외부에서 접근 할수 있다.
- 두 번째 예제에서는 var가 함수 스코프 내에 제한되어 함수 외부에서 접근할 수 없다.

### let

💡 let(or const) 키워드로 선언된 변수는 블록 스코프로 종속된다. 변수가 선언된 블록과 그 하위 블록 내에서만 사용할 수 있다.



```jsx
//let 사용의 예
let x = "global";

	if(x === "global"){
	let x = "block-scoped";
	
	console.log(x);
	//block-scoped
}
console.log(x);
	//global

//var 사용의 예
var y = "global";

	if(y === "global"){
	var y = "block-scoped";
	
	console.log(x);
	//block-scoped
}
console.log(x);
	//block-scoped
```

- 블록 스코프 내에서 let으로 선언한 변수에 새 값을 할당했을 때 블록 바깥에서는 그 값이 변경되지 않았다.
- 반면, var로 선언된 변수에 대해 동일한 작업을 하면 블록 스코프 외부에서 접근이 가능하므로 블록 바깥에서도 값이 변경되는 것을 볼 수 있다.

### const

💡 const(or let)으로 선언된 변수는 블록 스코프에 종속된다. 재할당을 통해 값이 변경될 수 없고 다시 선언될 수 도 없다.(let과의 차이점)



```jsx
const constant = "I am a constant";
constant = "I can't be reassigned";

//Uncaught TypeError: Assignment to donstant variable
```

- const로 선언된 변수가 불변이라는 의미는 아니다.(중요)

### const에 객체가 담겼다면?

```jsx
const person = {
name: 'Lim',
age: 18,
};

person.age = 19;
console.log(person.age);
//19
```

- 이 경우에는 변수 전체를 재할당하는 것이 아니라 그 속성 중 하나만 재할당하는 것이므로 문제가 없다.

```jsx
const person = {
name: 'Lim',
age: 18,
};

person.age = 19;
console.log(person.age);
//19

Object.freeze(person);

person.age = 30;

console.log(person.age);
//19
```

- 객체의 내용을 변경할 수 없게 const 객체를 고정할 수는 있다.(하지만, 객체의 값을 변경하려고 시도할 때 자바스크립트가 오류를 던지지는 않는다.)

------

## 1.2 TDZ

💡 TDZ(temporal dead zone)(일시적 비활성 구역)



```jsx
console.log(i)
var i = "I am a variable";
//undefined

console.log(j);
let j = "I am a let"
//Uncaught ReferenceError: j is not defined at <anonymous>:5:13
```

- **var**는 **정의되기 전에** 접근할 수 있지만, 그 값에는 접근할 수 없다. **let**과 **const**는 **정의하기 전에 접근할 수 없다**
- **var**, **let**, **const** 모두 다른 소스에서 읽을 수 있는 내용임에도 불구하고 **호이스팅**(hoisting)의 대상이된다.
- 코드가 실행되기 전에 처리되고 해당 스코프(글로벌이든 블록이든) 상단으로 올라간다.
- **var**가 가지는 가장 큰 차이점은 정의되기 전에도 접근할 수 있다는 점에 있다.(정의되기 전에는 undefined 값을 가지게 된다.)
- let은 변수가 선언될 때까지 일시적으로 비활성 구역, 즉 TDZ에 있게 된다. (초기화 전에 변수에 접근하면 오류가 발생한다.)
- undefined 값을 얻는 것보다는 오류가 발생하는 편이 디버깅이 더 쉽다.

------

## 1.3 var, let, const를 적재적소에 쓰는 법

💡 

- 기본적으로 const를 사용하자

- 재할당이 필요한 경우에만 let을 사용하자
- var는 ES6에서 절대 사용하지 않는다.



💡 

- 여러 큰 스코프에서 공유하기 위한 최상위 변수에는 var를 사용한다

- 작은 스코프의 로컬 변수에는 let을 사용한다
- 코드 작서잉 어느 정도 진행된 후에만 let을 const로 리팩토링한다(변수의 재할당을 막아야하는 경우라는 것이 확실해야 함)

## Quiz

1. 다음 코드의 올바른 출력은?

   ```jsx
   var greeting = "Hello";
   
   greeting = "Farewell";
   
   for(var i=0; i<2; i++) {
   	var greeting = "Good morning";
   }
   
   console.log(greeting);
   //Good morning
   ```

2. 다음 코드의 올바른 출력은?

   ```jsx
   let value = 1;
   
   if(true) {
   	let value = 2;
   	console.log(value);
   	//2
   }
   
   value = 3;
   ```

3. 다음 코드의 올바른 출력은?

   ```jsx
   let x = 100;
   
   if(x > 50) {
   	let x = 10;
   }
   
   console.log(x);
   //100
   ```

4. 다음 코드의 올바른 출력은?

   ```jsx
   console.log(constent);
   //ReferenceError
   
   const constant = 1;
   ```
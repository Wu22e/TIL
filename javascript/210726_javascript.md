



```javascript
let friend = null; // 없다.
let criminal = undefined; // 정해져 있지 않다.
```

undefined는 정해져 있지 않다. 아래 코드가 그 예시다

```javascript
let criminal;
console.log(criminal);
```

👉result

```
undefined
```

변수를 선언하고 값을 정해주지 않았기 때문에 undefined가 나오게 됨.

---

##### 논리연산자 순서

`NOT (!) > AND(&&) > OR(||)`

---

##### 비교연산자

```javascript
const a = 1;
const b = '1';
const equals = a == b; // == 는 타입을 비교하지 않음.
console.log(equals);

const a1 = false;
const b1 = 0;
const equals1 = a1 == b1;
console.log(equals1);

const a2 = true;
const b2 = 1;
const equals2 = a2 == b2;
console.log(equals2);
```

👉result

```
true
true
true
```

 

`==`는 타입을 검사하지 않기 때문에 실제로 값이 달라도 결과는 true가 나오게 된다.

그래서 비교연산을 할때 실수를 하지 않기 위해 `===` 세번 써줘야 값을 비교하게 된다.



참고:

```javascript
const a = null;
const b = undefined;
const equals = a == b; 
console.log(equals);
```

👉result

```
true
```



notEquals를 사용할때도 마찬가지로 `!=`또한 `==`와 동일하게 작동하므로 `!==`를 사용해야 한다.

---

##### const와 var의 차이

- const

```javascript
const a = 1;
if (a+1 === 2) {
    const a = 2;
    console.log('if문 안의 a 값은 ' + a);
}
console.log('if문 밖의 a 값은 ' + a);
```

👉result

```
if문 안의 a 값은 2
if문 안의 a 값은 1
```

if 문안의 블록에서 새롭게 const 변수를 선언하면 if문 밖의 변수 a와 다른 값으로 if문 안에서 2를 출력후 if문을 벗어나면 원래 선언되어있던 1값이 출력이 된다.

- var

```javascript
var a = 1;
if (a+1 === 2) {
    var a = 2;
    console.log('if문 안의 a 값은 ' + a);
}
console.log('if문 밖의 a 값은 ' + a);
```

👉result

```
if문 안의 a 값은 2
if문 안의 a 값은 2
```

근데 var로 바꾸면 둘다 2가 나오게 되므로, var은 내가 생각했던 대로 동작하지 않을 수 있다.



이를 **호이스팅 현상**이라고 한다.

`호이스팅(hoisting)`이란 단어 자체로 '들어올리다' 라는 의미가 있다.

즉 자바스크립트에서 호이스팅은 코드에 선언된 변수 및 함수를 코드 상단으로 끌어 올리는 것을 뜻한다.



`함수 내에서 선언한 함수 범위(function)의 변수는, 해당 함수의 최상위로 이동`

`함수 밖에서 선언한 전역 범위(global scope)의 전역 변수는 스크립트 단위의 최상위로 끌어올려짐.`

> 여기서 주의할 점은 변수의 선언과 할당 내용 모두를 상단으로 끌어 올리는게 아니라, 선언 부분만 분리하여 최상위로 끌어 올린다. 선언된 변수에 값을 할당하는 내용은 원래 그 라인에 있다.



그래서 위에 var로 변수를 선언할 경우, 다음과 같이 작동함을 알 수 있다.

```javascript
var a;
var a;
a = 1;
if (a+1 === 2) {
    a = 2;
    console.log('if문 안의 a 값은 ' + a);
}
console.log('if문 밖의 a 값은 ' + a);
```

---

##### 함수

아래와 같이 간단한 함수를 작성할 수 있다.

```javascript
function hello(name) {
    console.log('Hello. ' + name + '!');
}

hello('velopert');
```

👉result

```
Hello, velopert!
```



현재 +연산자를 이용하여 문자열을 붙였다.

ES6에서는 이런식의 문자열 조합을 할 때  조금 더 편리하게 조합할 수 있는 문법이 존재한다.

ES6 는 ECMAScript 6를 의미하며, 자바스크립트 버전을 의미한다. ES6는 2015년에 도입되었음. 그래서 ES6는 ES2015로 불리기도 함.

const, let이 ES6에 해당하는 문법이다.

이후로도 ES7,8,9,10 계속 업데이트 되고 있다.



ES6에서는 Template Literal이란 문법을 이용하여 문자열을 간단히 표현할 수 있다.

```javascript
function hello(name) {
    console.log(`Hello ${name}!`);
}

hello('velopert');
```

👉result

```
Hello, velopert!
```

return으로 바꿔도 됨

```javascript
function hello(name) {
    return `Hello ${name}!`;
}

const result = hello('velopert');
console.log(result);
```

👉result

```
Hello, velopert!
```

---

##### 화살표 함수

```javascript
const add = (a, b) => {
    return a + b;
}

const hello = (name) => {
    console.log(`Hello, ${name}!`);
}

const sum = add(1, 2);
console.log(sum);
hello('velopert');
```

👉result

```
3
Hello, velopert!
```



화살표 함수를 이용하면 함수 안에서 값을 리턴할때 코드를 더 짧게 쓸 수 있다.

```javascript
const add = (a, b) => a + b; // 이렇게 바로 리턴할 수 있음.

const sum = add(1, 2);
console.log(sum);
```

👉result

```
3
```

 화살표 함수는 ES6 문법이다.

---

##### 객체

객체는 어떤 값을 선언할 때, 하나의 이름에 여러 종류의 값을 넣을 수 있게 해줌.



```JAVASCRIPT
const dogName = '멍멍이';
const dogAge = 2;

console.log(dogName);
console.log(dogAge);
```

이 코드를 객체를 사용하면

```javascript
const dog = {
    name: '멍멍이',
    age: 2,
    cute: true,
    sample: {
        a: 1,
        b: 2
    }
}

console.log(dog);
console.log(dog.name);
console.log(dog.age);
```

이런식으로 dog라는 객체를 만들고 문자열, 정수형, 불린, 객체를 요소로 만들 수 있다.

👉result

![image](https://user-images.githubusercontent.com/52458039/126946236-97ba32a4-e1e8-420a-99df-53a43771e207.png)



객체는 아래와 같이 key, value 형식을 가진다.

```
const 객체이름 = {
	key1 : value1,
	key2 : value2
}
```

key는 기본적으로 문자열로 취급되지만 정수형을 써도 무방하다.

그리고 key에는 스페이스를 허용하지 않는다.

```javascript
const dog = {
    name: '멍멍이', // name은 'name'과 동일하게 취급(기본적으로 문자열로 취급함)
    age: 2,
    key with spaace: 'asdf' // 오류 발생
}
```

이럴때는 따옴표로 감싸주면 된다.

```javascript
const dog = {
    name: '멍멍이',
    age: 2,
    'key with spaace': 'asdf' // ok
}

console.log(dog['key with space']); // 단, 이 경우는 .으로 접근이 불가능하므로 [] 안에 key 값으로 접근해야 한다.
```

👉result

```
asdf
```



마찬가지로 

```javascript
const dog = {
    1:2
}

console.log(dog[1]); // ok
console.log(dog.1); // error
```

key값을 기본형태가 아닌 방식으로 쓰게 되면 []을 이용하여 key값으로 접근해야 한다.



또, 다음과 같이 객체의 key와 value값을 아래와 같이 추가해 줄 수 도 잇다.

```javascript
const dog = {
  name: "멍멍이"
};

dog.age = 1; // .으로 만들어도 되고
dog['alias'] = "밍키"; // []로 만들어도 된다.
dog["1"] = 2;
console.log(dog);
```

👉result

![image](https://user-images.githubusercontent.com/52458039/126951623-0b15e375-2cc5-4e57-9bbb-e59b9d3c1b85.png)







함수의 파라미터로도 객체를 받아올 수 있음.

```javascript
const ironMan = {
    name: '토니 스타크',
    actor: '로버트 다우니 주니어',
    alias: '아이언맨'
}

const captainAmerica = {
    name: '스티븐 로저스',
    actor: '크리스 에반스',
    alias: '캡틴 아메리카'
}

function print(hero) {
    const text = `${hero.alias}(${hero.name}) 역할을 맡은 배우는 ${hero.actor} 입니다.`;
    console.log(text);
}

print(ironMan)
print(captainAmerica)
```

👉result

```
아이언맨(토니 스타크) 역할을 맡은 배우는 로버트 다우니 주니어 입니다.
캡틴 아메리카(스티븐 로저스) 역할을 맡은 배우는 크리스 에반스 입니다.
```



근데 여기서 hero에 있는 값을 조회하기 위해 . 을 이용하여 접근하고 있는데,

ES6의 `비구조 할당(객체구조 분해)` 방법을 이용하면 좀더 편하게 쓸 수 있다.

```javascript
const ironMan = {
    name: '토니 스타크',
    actor: '로버트 다우니 주니어',
    alias: '아이언맨'
}

const captainAmerica = {
    name: '스티븐 로저스',
    actor: '크리스 에반스',
    alias: '캡틴 아메리카'
}

function print(hero) {
    const {alias, name, actor} = hero; // 이런식으로 객체 구조를 분해할 수 있음 (객체 내부값을 외부로 빼내서 alias, name, actor란 이름으로 선언해 줌)
    const text = `${alias}(${name}) 역할을 맡은 배우는 ${actor} 입니다.`; // 따라서 hero. 을 쓰지 않아도 됨
    console.log(text);
}

print(ironMan)
print(captainAmerica)
```

👉result

```
아이언맨(토니 스타크) 역할을 맡은 배우는 로버트 다우니 주니어 입니다.
캡틴 아메리카(스티븐 로저스) 역할을 맡은 배우는 크리스 에반스 입니다.
```



이를 파라미터를 받을 때 바로 해줄 수 도 있다.

```javascript
const ironMan = {
    name: '토니 스타크',
    actor: '로버트 다우니 주니어',
    alias: '아이언맨'
}

const { name } = ironMan; // 이런식으로 함수 밖에서도 사용 가능
console.log(name);

const captainAmerica = {
    name: '스티븐 로저스',
    actor: '크리스 에반스',
    alias: '캡틴 아메리카'
}

function print( {alias, name, actor} ) { // 파라미터를 받을때 이런식으로 바로 비구조 할당할 수 있음.
    const text = `${alias}(${name}) 역할을 맡은 배우는 ${actor} 입니다.`; // 따라서 hero. 을 쓰지 않아도 됨
    console.log(text);
}

print(ironMan)
print(captainAmerica)
```

👉result

```
토니 스타크
아이언맨(토니 스타크) 역할을 맡은 배우는 로버트 다우니 주니어 입니다.
캡틴 아메리카(스티븐 로저스) 역할을 맡은 배우는 크리스 에반스 입니다.
```



##### 객체 안에 함수 넣기

```javascript
const dog = {
    name: '멍멍이',
    sound: '멍멍!',
    say: function say() {
        console.log(this.sound); // 여기서 this는 이 함수가 위치한 객체(지금은 dog), 자기자신을 의미함
    }
}

dog.say();
```

👉result

```
멍멍!
```



여기서 이름을 생략해도 된다.

```javascript
const dog = {
    name: '멍멍이',
    sound: '멍멍!',
    say: function(){ // say 함수 이름을 생략해도 됨
        console.log(this.sound);
    }
}

dog.say();
```

👉result

```
멍멍!
```



아예 다 지워도 됨.

```javascript
const dog = {
    name: '멍멍이',
    sound: '멍멍!',
    say(){ // 아예 function 키워드를 지워도 됨.
        console.log(this.sound);
    }
}

dog.say();
```

👉result

```
멍멍!
```



그런데, 화살표 함수로 만들게 되면 작동하지 않음

```javascript
const dog = {
    name: '멍멍이',
    sound: '멍멍!',
    say: () => {
        console.log(this);
        console.log(this.sount); 
    }
}

dog.say();
```

👉result

```
undefined
TypeError: Cannot read property 'sound' of undefined at Object.say
```

this를 undefined로 인식하게 된다.

왜 그러냐면, function 키워드를 사용하여 함수를 만들면 this가 자기가 속해있는 곳을 가리키게 되는데, 화살표 함수가 되면  this를 자기가 속해있는 객체랑 연결하지 않기 때문에 작동하지 않는다. 

다른 예시를 살펴보면

```javascript
const dog = {
    name: '멍멍이',
    sound: '멍멍!',
    say:  function() {
        console.log(this.sount); 
    }
}

const cat = {
    name: '야옹이',
    sound: '야옹~'
}

cat.say = dog.say; // cat에 say라는 key와 dog의 say에 해당하는 function을 value로 넣어준다.
// 즉 cat 객체에 say가 등록되게 되고
// 이때 function안에 있는 this는 cat을 가리키게 되므로 cat.say()를 호출하면 '야옹~' 이 출력되게 된다.

dog.say();
cat.say();
```

👉result

```
멍멍!
야옹~
```



근데 아래와 같이 작성하게 된다면?

```javascript
const dog = {
    name: '멍멍이',
    sound: '멍멍!',
    say:  function() {
        console.log(this.sount); 
    }
}

const cat = {
    name: '야옹이',
    sound: '야옹~'
}

cat.say = dog.say; 

dog.say();
cat.say();

const catSay = cat.say; // catSay는 객체가 아니므로, 아무곳에도 엮여있지 않다. 
catSay(); // 그래서 catSay를 호출하면 this가 뭔지모르니까 undefined가 되고 에러가 발생하게 된다.
```

👉result

```
멍멍!
야옹~
Error in sandbox:
TypeError: Cannot read property 'sound' of undefined
```



##### Getter와 Setter 함수

객체안에 getter와 setter 함수를 설정할 수 있다.



```javascript
const numbers = {
    a: 1,
    b: 2
}

numbers.a = 5;
console.log(numbers);
```

👉result

```
Object { a: 5, b: 2 }
```



먼저 getter 함수를 만들어 보자

```javascript
const numbers = {
    a: 1,
    b: 2,
    get sum() {
        console.log('sum 함수가 실행됩니다!');
        return this.a + this.b;
    }
}

console.log(numbers.sum); // numbers에 있는 sum을 조회함 (호출이 아님)
numbers.b = 5;
console.log(numbers.sum);
```

👉result

```
sum 함수가 실행됩니다!
3
sum 함수가 실행됩니다!
6
```

이런식으로 getter 함수는 특정값을 조회하려고할때, (호출하는게 아니라) 특정 코드와 연산된 값을 반환받아서 사용하게 된다.



이번엔 setter 함수를 보자

```javascript
const dog = {
    _name: '멍멍이',
    
    get name() {
        console.log('_name을 조회합니다..');
        return this._name;
    }
    
    set name(value) { // setter 함수는 파라미터를 받아와야 함.
        console.log('이름이 바뀝니다..' + value);
        this._name = value;
    }
}

console.log(dog.name); // getter 함수
dog.name = '뭉뭉이'; // setter 함수
console.log(dong.name); // getter 함수
```

👉result

```
_name을 조회합니다..
멍멍이
이름이 바뀝니다..뭉뭉이
_name을 조회합니다..
뭉뭉이
```



또다른 예제를 보자

```javascript
const numbers = {
  _a: 1,
  _b: 2,
  sum: 3,
  calculate() {
      console.log('calculate');
      this.sum = this._a + this._b;
  },
  get a() {
      return this._a;
  },
  get b() {
      return this._b;
  },
  set a(value) {
      this._a = value;
      this.calculate();
  },
  set b(value) {
      this._b = value;
      this.calculate();
  }
}

console.log(numbers.sum);
numbers.a = 5; // a setter 함수가 실행되면서 _a가 5로 갱신되고 sum이 7로 갱신된다.
console.log(numbers.sum);
numbers.b = 7; // b setter 함수가 실행되면서 _b가 7로 갱신되고 sum이 12로 갱신된다.
console.log(numbers.sum);
numbers.a = 9; // a setter 함수가 실행되면서 _a가 9로 갱신되고 sum이 16로 갱신된다.
console.log(numbers.sum);
```

👉result

```
3
7
12
16
```



이를 아래와 같이 코드를 짜게 되면 sum을 조회할 때마다 합을 계산하므로 비효율 적임. 그래서 위와 같이 setter함수를 적용하여 값이 바뀔때만 합을 출력해주는게 더 효율적이다.

```javascript
const numbers = {
  _a: 1,
  _b: 2,
  sum: 3,
  get sum() {
      console.log('sum');
      return this.a + this.b;
  }
}

console.log(numbers.sum);
numbers.a = 9;
numbers.b = 7;
console.log(numbers.sum);
console.log(numbers.sum);
console.log(numbers.sum);
```

👉result

```
3
16
16
16
```

---

#### 배열

자바 스크립트의 배열은 배열안의 모든 원소가 똑같은 형태일 필요는 없다. 정수형, 문자열 등등 섞여 있어도 상관 없다.

```javascript
const array1 = [1,2,3,4,5];
const array2 = [1,'blabla', {}, 4];
const array3 = [1, true, {a: 1}, [1,2,3,4]];
console.log(array1);
console.log(array2);
console.log(array3);
console.log(array2[0]);
console.log(array2[1]);
console.log(array2[2]);
console.log(array2[3]);
console.log(array2[4]); // 인덱스 밖을 벗어나면 undefined
```

👉result

```
[1,2,3,4,5]
[1, "blabla", Object, 4]
[1, true, Obejct, Array[4]]
1
blabla
Object {}
4
undefined
```



객체로 이루어진 배열도 만들 수 있다.

```javascript
const objects = [ {name: '멍멍이'}, {name: '야옹이'}];

console.log(objects);
console.log(objects[0])
console.log(objects[1])
```

👉result

```
[Object, Object]
Object {name: "멍멍이"}
Object {name: "야옹이"}
```



배열 내장함수인 push를 이용하여 데이터를 추가할 수 있다.

```javascript
const objects = [ {name: '멍멍이'}, {name: '야옹이'}];

objects.push({name: '멍뭉이'});
console.log(objects);
```

👉result

![image](https://user-images.githubusercontent.com/52458039/126958014-f04eb222-ddd5-4f56-88d6-f9699462648a.png)



배열의 크기는 length를 사용한다.

```javascript
const objects = [ {name: '멍멍이'}, {name: '야옹이'}];

objects.push({name: '멍뭉이'});
console.log(objects.length);
```

👉result

```
3
```



---

#### 반복문 for...of (배열의 반복적인 작업에 사용)

```javascript
const numbers = [10, 20, 30, 40, 50];
for(let number of numbers) { 
    console.log(number);
}

// for of문과 일반적인 for문은 사실 잘 사용하지 않음.
// 배열 내장함수로, 배열 내부의 값을 처리할 때 더 쉽게하는 방법이 있음.
```

👉result

```
10
20
30
40
50
```



이제 객체의 정보를 배열형태로 가져오는 방법을 살펴보자.

```javascript
const doggy = {
    name: '멍멍이',
    sound: '멍멍',
    age: 2
}

// 객체를 배열형태로 반환
console.log(Object.entries(doggy));
// 객체의 key값을 배열로 받아오는 법
console.log(Object.keys(doggy));
// 객체의 value값을 배열로 받아오는 법
console.log(Object.values(doggy));
```

👉result

![image](https://user-images.githubusercontent.com/52458039/126960709-e9e18862-c424-40c2-a195-099f4eabf9ba.png)



#### 반복문 for...in (객체의 반복적인 작업에 사용)

```javascript
const doggy = {
    name: '멍멍이',
    sound: '멍멍',
    age: 2
}

for (let key in doggy) { // key 값은 doggy의 key 값을 하나씩 받음
    console.log(`${key}: ${doggy[key]}`);
}
```

👉result

```
name: 멍멍이
sound: 멍멍
age: 2
```



---

### 배열 내장 함수

- forEach문

```javascript
const superheroes = ['아이언맨', '캡틴 아메리카', '토르', '닥터 스트레인지'];

for (let i = 0; i < superheroes.length; i++) {
    console.log(superheroes[i]);
}
```

👉result

```
아이언맨
캡틴 아메리카
토르
닥터 스트레인지
```



배열 요소를 출력할때 우리는 위와 같이 for문을 이용하여 출력하였다.



```javascript
const superheroes = ['아이언맨', '캡틴 아메리카', '토르', '닥터 스트레인지'];
function print(hero) {
    console.log(hero);
} 

superheroes.forEach(print);
```

👉result

```
아이언맨
캡틴 아메리카
토르
닥터 스트레인지
```

하지만 배열 내장함수 forEach와 print함수를 이용하여 for문을 사용하지 않고 배열의 요소를 출력할 수 있다.

여기서 더 깔끔하게 하려면 print함수를 forEach 안에서 바로 만들어도 됨

```javascript
const superheroes = ['아이언맨', '캡틴 아메리카', '토르', '닥터 스트레인지'];

superheroes.forEach(function(hero){ // 이름없는 함수를 바로 만든다.
    console.log(hero);
}); 
```

👉result

```
아이언맨
캡틴 아메리카
토르
닥터 스트레인지
```



화살표 함수를 이용하면 더 깔끔해진다

```javascript
const superheroes = ['아이언맨', '캡틴 아메리카', '토르', '닥터 스트레인지'];

superheroes.forEach(hero => {
    console.log(hero);
}); 
```

👉result

```
아이언맨
캡틴 아메리카
토르
닥터 스트레인지
```



- map

map은 배열안의 원소를 변환할 때 사용함



기존의 방식대로 어떤 배열의 원소를 각각 제곱한 배열을 갖도록 구현해보자

```javascript
const array = [1,2,3,4,5,6,7,8];

const squared = [];
for(let i = 0; i < array.length; i++) {
    squared.push(array[i] * array[i]);
}

console.log(squared);
```

👉result

```
[1, 4, 9, 16, 25, 36, 49 ,64]
```



forEach 써도된다.

```javascript
const array = [1,2,3,4,5,6,7,8];

const squared = [];
array.forEach(n => {
    squared.push(n*n);
})

console.log(squared);
```

👉result

```
[1, 4, 9, 16, 25, 36, 49 ,64]
```



forEach만 사용해도 훨씬 깔끔해지는데, map을 사용하면 더 깔끔해 짐.



```javascript
const array = [1,2,3,4,5,6,7,8];

const square = n => n*n;
const squared = array.map(square); 

// 위 두줄을 const squared = array.map(n => n*n); 이렇게 한줄로 줄일 수도 있다.

console.log(squared)
```

👉result

```
[1, 4, 9, 16, 25, 36, 49 ,64]
```



map을 활용하면 다양한 상황에 응용할 수 있다.

```javascript
const items = [
    {
        id: 1,
        text: 'hello'
    },
    {
        id: 2,
        text: 'bye'
    }
]

//이 상황에서 객체 배열들을, text로 이루어진 문자열로 바꾸고 싶다면?

const texts = items.map(item => item.text);
console.log(texts);
```

👉result

```
["hello", "bye"]
```



이번에는 배열에서 원하는 항목이 어디 있는지 알려주는 함수를 보자.

```javascript
const superheroes = ['아이언맨', '캡틴 아메리카', '토르', '닥터 스트레인지'];
const index = superheroes.indexOf('토르');
const index = superheroes.indexOf('닥터 스트레인지');
console.log(index);
```

👉result

```
2
3
```



indexOf라는 함수와 비슷한 함수로 findIndex라는 함수인데, 만약 배열에 있는 값이 문자, 숫자, 불리언 이면 찾고자하는 원소가 몇번째 인지 알고 싶을때 충분히 indexOf 함수로 얻어낼 수 있다.

하지만 배열 원소가 객체이거나, 특정한 값이 일치하는 값을 찾는게 아니라 조건으로 찾는다면 indexOf로 할 수 없다.

```javascript
const todos = [
    {
        id: 1,
        text: '자바스크립트 입문',
        done: true
    },
    {
        id: 2,
        text: '함수 배우기',
        done: true
    },
    {
        id: 3,
        text: '객체와 배열 배우기',
        done: true
    },
    {
        id: 4,
        text: '배열 내장함수 배우기',
        done: false
    }
]

// 여기서 id가 3인걸 찾고 싶다면, indexOf로 찾을 수 없다
// 이럴때 findIndex를 써야한다.
const index1 = todos.indexOf(3);
const index2 = todos.findIndex(todo => todo.id === 3); // findIndex 파라미터는 함수다. 특정 조건을 확인해서 그 조건이 일치하면 일치하는 원소가 몇번째 인지 알려주는 함수다. 
const todo1 = todos.find(todo => todo.id === 3); // find 함수는 조건에 맞는 객체를 반환 받게 된다.
const todo2 = todos.find(todo => todo.done === false);
console.log(index1);
console.log(index2);
console.log(todo1);
console.log(todo2);
```

👉result

```javascript
-1 // -1은 일치하는게 없다는 의미다.
2 // id 3은 배열에서 인덱스 값이 2다.
Object { id: 3, text: "객체와 배열 배우기", done: true}
Object { id: 4, text: "배열 내장함수 배우기", done: false}
```



- filter

filter는 특정 조건을 만족하는 원소들을 찾아서, 그 원소들을 가지고 새로운 배열을 만들 수 있다.

```javascript
const todos = [
    {
        id: 1,
        text: '자바스크립트 입문',
        done: true
    },
    {
        id: 2,
        text: '함수 배우기',
        done: true
    },
    {
        id: 3,
        text: '객체와 배열 배우기',
        done: true
    },
    {
        id: 4,
        text: '배열 내장함수 배우기',
        done: false
    }
]
// done이 false인 값만 필터링 하고 싶다면?

const tasksNotDone1 = todos.filter(todo => todo.done === false); // 기존의 배열을 건드리지 않고 새로운 배열을 만들어 줌 
// const tasksNotDone1 = todos.filter(todo => !todo.done); 와 동일함
const tasksNotDone2 = todos.filter(todo => todo.done === true); 
// const tasksNotDone2 = todos.filter(todo => todo.done); 와 동일함
console.log(tasksNotDone1);
console.log(tasksNotDone2);
```

👉result

![image](https://user-images.githubusercontent.com/52458039/126979570-5fb03299-1098-4430-b428-2f73d098de19.png)



- splice (기존의 배열을 건드림)

```javascript
const numbers = [10,20,30,40];
const index = numbers.indexOf(30);
const spliced = numbers.splice(index, 2); // index부터 2개 삭제 (반환 값은 잘라낸 원소임. 반환 안받아도 됨)
console.log(spliced);
console.log(numbers);
```

👉result

```
[30, 40]
[10, 20]
```



- slice (기존의 배열을 건드리지 않음)

```javascript
const numbers = [10, 20, 30, 40];

const sliced = numbers.slice(0, 2); // 0부터 2까지 짜름. 왼쪽은 포함, 오른쪽은 미포함 이므로 인덱스 0부터 1까지 짤린다.

console.log(sliced);
console.log(numbers);
```

👉result

```
[10, 20]
[10, 20, 30, 40]
```



- shift (기존의 배열 수정)

```javascript
const numbers = [10, 20, 30, 40];

const value = numbers.shift(); // 배열의 맨 앞 값을 가져옴 ( 그리고 배열 원소 삭제 됨 )
console.log(value);
console.log(numbers);
```

👉result

```
10
[20, 30, 40]
```



- pop (기존의 배열 수정)

```javascript
const numbers = [10, 20, 30, 40];

const value = numbers.pop(); // 배열의 맨 뒷 값을 가져옴 ( 그리고 배열 원소 삭제 됨 )
console.log(value);
console.log(numbers);
```

👉result

```
40
[10, 20, 30]
```



- unshift (기존의 배열 수정)

```javascript
const numbers = [10, 20, 30, 40];
numbers.unshift(5); // 배열 맨앞에 원소 추가
console.log(numbers);
```

👉result

```
[5, 10, 20, 30, 40]
```



- push (기존의 배열 수정)

```javascript
const numbers = [10, 20, 30, 40];
numbers.push(50); // 배열 맨앞에 원소 추가
console.log(numbers);
```

👉result

```
[10, 20, 30, 40, 50]
```



pop과 push가 하나의 쌍이고, shift와 unshift가 하나의 쌍이다.



- concat ( 기존의 배열 건드리지 않음 )

```javascript
const arr1 = [1,2,3];
const arr2 = [4,5,6];

const concated = arr1.concat(arr2); // 여러개의 배열을 하나로 합침
// ES6 연산자중 spread 연산자를 이용해도 됨
// const concated = [...arr1, ...arr2];
console.log(arr1);
console.log(arr2);
console.log(concated);
```

👉result

```
[1,2,3]
[4,5,6]
[1,2,3,4,5,6]
```



- join (배열 안의 값을 문자열로 합칠 때 사용)

```javascript
const array = [1,2,3,4,5];
console.log(array.join()); // 파라미터 없으면 배열 원소사이에 쉼표를 넣어서 문자열 형태로 반환함
console.log(array.join(' ')); // 파라미터로 스페이스를 넣어주면 원소사이에 스페이스가 들어간 문자열로 반환함
console.log(array.join(', ')); // 파라미터가 꼭 문자 하나일 필요 없음
```

👉result

```
1,2,3,4,5
1 2 3 4 5 
1, 2, 3, 4, 5
```



- reduce (이 함수는 주로 배열이 주어 졌을때, 배열의 모든 값을 연산해야할 때 사용함)



합을 구하는 방법을 우리는 아래처럼 해왔다.

```javascript
const numbers = [1,2,3,4,5];

let sum = 0;
numbers.forEach( n=> {
    sum += n;
})
console.log(sum);
```

👉result

```
15
```



reduce 함수를 사용하면 한줄로 구현할 수 있다.

```javascript
const numbers = [1,2,3,4,5];

const sum = numbers.reduce((accumulator, current) => accumulator + current, 0); // reduce 함수 파라미터로 첫번째는 함수, 두번째는 accumulator 초기값인 0을 할당 받음
console.log(sum);
```

👉result

```
15
```



reduce의 작동  방식은 다음과 같다.

처음에는 accumulator 값이 0으로 초기화 된다.

(여기서 accumulator는 누적된 값을 의미함)

그리고 current는 numbers의 초기값 1을 받아온다.

그리고 accumulator + current를 하게되면  0 + 1로 1을 리턴한다.

이 1이 accumulator로 다시 갱신된다. 이게 계속 numbers 끝값을 받을 때 가지 누적 된다.



reduce를 사용해서 배열의 평균을 구할 수도 있다.

```javascript
const numbers = [1,2,3,4,5];

const avg = numbers.reduce((accumulator, current, index, array) => { // 여기서 index는 current가 몇번째 원소인지 알려줌. array는 함수가 실행되고 있는 자기자신인 numbers를 의미한다.
	if(index === array.length - 1) {
        return (accumulator + current) / array.length;
  }
  return accumulator + current;  
}, 0); 
console.log(avg);
```

👉result

```
3
```



배열내 같은 원소를 카운팅할 때도 쓸 수 있다.

```javascript
const alphabets = ['a', 'a', 'a', 'b', 'c', 'c', 'd', 'e'];
alphabets.reduce((acc, cur) => {
    if (acc[current]) { // acc에 current가 존재한다면
        acc[current] += 1;
    } else {
        acc[current] = 1;
    }
    return acc;
}, {})
```

👉result

![image](https://user-images.githubusercontent.com/52458039/127002418-52791bcf-2895-4c30-8674-3a5fc1bbf3eb.png)



---



##### 객체 생성자

```javascript
function Animal(type, name, sound) { // 객체 생성자는 보통 함수 이름을 대문자로 시작함.
    this.type = type; // 여기서 this는 Animal 객체 생성자로 객체가 생성될때 그 객체를 가리킴
    this.name = name;
    this.sound = sound;
   	 this.say = function() { // 이름 없는 함수
         console.log(this.sound);
     }
}

const dog = new Animal('개', '멍멍이', '멍멍'); // new라는 키워드를 통해 하나의 객체가 생성됨.
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say();
cat.say();
```

👉result

```
멍멍
야옹
```



위 코드에서는 dog, cat이 만들어질때마다 새로운 함수 say가 만들어지고 있어서 비효율 적임 (함수의 내용도 같음). 

그래서 이 say 함수를 바깥으로 꺼내서 재사용 해 보자. 이때 프로토 타입을 사용한다.

```javascript
function Animal(type, name, sound) { 
    this.type = type; 
    this.name = name;
    this.sound = sound;
}

Animal.prototype.say = function() { // 이렇게 프로토 타입을 이용하여 say함수를 밖으로 뺼 수 있다.
    console.log(this.sound);
}

const dog = new Animal('개', '멍멍이', '멍멍'); 
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say();
cat.say();
```

이렇게 프로토 타입으로 빼게 되면 다음과 똑같은 방식으로 작동하게 된다.

```javascript
function Animal(type, name, sound) { 
    this.type = type; 
    this.name = name;
    this.sound = sound;
}

const dog = new Animal('개', '멍멍이', '멍멍'); 
const cat = new Animal('고양이', '야옹이', '야옹');

Animal.prototype.sharedValue = 1; // 이런식으로 프로토 타입을 쓰면 어떠한 Animal 객체든 sharedValue로 값 1이 들어가게 된다.

///////////////////////////////////////////////////////
function say() {
    console.log(this.sound);
}

dog.say = say;
cat.say = say;
///////////////////////////////////////////////////////
// 위와 같이 say 함수를 따로 만들어서 각 객체에 따로 넣어주는 것과 동일한 역할(프로토타입)을 하게 된다.

dog.say();
cat.say();
```

---

#### 객체 생성자 상속하기

만약 상속 받지 않고 개와 고양이 객체를 만든다면?

```javascript
function Dog(name, sound) {
    this.type = '개';
    this.name = name;
    this.sound = sound;
}

function Cat(name, sound) {
    this.type = '고양이';
    this.name = name;
    this.sound = sound;
}

Dog.prototype.say = function() {
    console.log(this.sound);
}

Cat.prototype.say = function() {
    console.log(this.sound);
}

const dog = new Dog('멍멍이', '멍멍');
const cat = new Cat('야옹이', '야옹');
```

dog나 cat이나 생성하는 변수가 비슷한데 2번이나 만드는건 비효율 적이다.

이런 상황에 사용하는 것이 상속 이다.

```javascript
function Animal(type, name, sound) { 
    this.type = type; 
    this.name = name;
    this.sound = sound;
}

Animal.prototype.say = function() { 
    console.log(this.sound);
}

// Dog와 Cat 객체 생성자를 만들되, 최대한 Animal이 가지고 있는 것을 재사용 한다.
function Dog(name, sound) {
    Animal.call(this, '개', name, sound); // call 함수 호출 시, 첫번째 파라미터에 이 객체 생성자 함수의 this를 넣어준다. 그 다음 파라미터는 Animal 객체 생성자의 파라미터를 의미한다.
}

function Cat(name, sound) {
    Animal.call(this, '고양이', name, sound);
}

// 프로토 타입을 공유하게 함
Dog.prototype = Animal.prototype;
Cat.prototype = Animal.prototype;

const dog = new Animal('개', '멍멍이', '멍멍'); 
const cat = new Animal('고양이', '야옹이', '야옹');


dog.say();
cat.say();
```



#### ES6 Class

ES6 Class 문법을 사용하면 위와 같이 상속한 것을 좀더 알기 쉽게 사용할 수 있다.

```javascript
class Animal {
    constructor(type, name, sound) { // constructor 는 생성자 키워드다.
        this.type = type;
        this.name = name;
        this.sound = sound;
    }
    say() { // say라는 함수를 클래스 내부에서 구현하게 되면 자동으로 프로토타입으로 등록되게 된다.
        console.log(this.sound);
    }
}

console.log(Animal.prototype.say); // 함수가 설정된 것을 확인 할 수 있음.

const dog = new Animal('개', '멍멍이', '멍멍');
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say();
cat.say();
```

👉result

![image](https://user-images.githubusercontent.com/52458039/127009159-3dd37256-1b0c-40e3-8f23-b4e0b2f0dbb9.png)



클래스 문법을 사용하면 상속할때 훨씬 더 쉽게 사용할 수 있다.

```javascript
class Animal {
    constructor(type, name, sound) { // constructor 는 생성자 키워드다.
        this.type = type;
        this.name = name;
        this.sound = sound;
    }
    say() {
        console.log(this.sound);
    }
}
class Dog extends Animal {
    construcutor(name, sound) {
        super('개', name, sound); // super를 이용하여 부모의 생성자를 먼저 호출함
    }
}

class Cat extends Animal {
    construcutor(name, sound) {
        super('고양이', name, sound);
    }
}

// 훨씬 쉽게 상속받아서 사용할 수 있다.
const dog = new Animal('멍멍이', '멍멍');
const cat = new Animal('야옹이', '야옹');
const cat2 = new Animal('야오오오옹이', '야오오오옹');

dog.say();
cat.say();
cat2.say();
```

👉result

```
멍멍
야옹
야오오오옹
```



---

##### 연습 Food Class 만들기

```javascript
class Food {
    constructor(name) {
        this.name = name;
        this.brands = [];
    }
    addBrand(brand) { // 클래스 내부에서 구현하는 함수를 '메서드' 라고 한다.
        this.brands.push(brand);
    }
    
    print() {
        console.log(`${this.name} 을(를) 파는 음식점들:`);
        console.log(this.brands.join(', '));
    }
}

const pizza = new Food('피자');
pizza.addBrand('피자헛');
pizza.addBrand('도미노 피자');

const chicken = new Food('치킨');
chicken.addBrand('굽네치킨');
chicken.addBrand('BBQ');

pizza.print();
chicken.print();
```

👉result

```
피자 을(를) 파는 음식점들:
피자헛, 도미노 피자
치킨 을(를) 파는 음식점들:
굽네치킨, BBQ
```


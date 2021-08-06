# arrangement-handling

자바스크립트 배열 메서드 reduce 100% 활용법

reduce 는 자바스크립트 배열 메서드 중 활용도가 으뜸입니다.

먼저 예시코드부터 볼까요?

```
var votes = ["kim", "hong", "lee", "hong", "lee", "lee", "hong"];var reducer = function(accumulator, value, index, array) {
  if (accumulator.hasOwnProperty(value)) {
    accumulator[val] = accumulator[val] + 1;
  } else {
    accumulator[val] = 1;
  }
  return accumulator;
}var initialValue = {};var result = votes.reduce(reducer, initialValue);console.log(result); // { kim: 1, hong: 3, lee: 3 }
```

상기 코드는 votes 배열에 있는 값들을 순회하면서 최종적으로 각각의 값들이 몇 번 나오는지 count 하는 로직입니다. reduce 메서드의 첫 번째 인자로는 reducer 라는 함수를, 두 번째 인자로는 초기값, intialValue 라는 빈 object 를 전달합니다.

배열의 첫 번째 순회 때는 accumulator 의 값은 initialValue, 즉 {} 입니다. if 조건검사의 결과는 false 이므로 accumulator.kim = 1 이 되고, { “kim” : 1 } 이라는 값을 return 합니다.

두 번째 순회 때는 accumulator 의 값은 앞에서 전달받은 { “kim” : 1 } 이고, value 는 배열의 두 번째 값(votes[1])인 “hong” 이므로 return 하는 값은 { “kim” : 1, “hong” : 1 } 입니다. 이렇게 계속해서 배열의 끝까지 순회하면 최종적인 결과는 { kim: 1, hong: 3, lee: 3} 와 같습니다.

기본적으로 reduce 메서드도 다른 메서드와 동일하게 첫 번째 인자로 함수를 받습니다. 하지만, 두 번째 인자로 초기값을 셋팅할 수 있다는 것에서 차이가 있습니다. 물론, 두 번째 인자는 생략이 가능하며, 생략 시에는 배열의 첫 번째 값이 그값을 담당합니다.

뿐만 아니라 배열을 순회하면서 return 한 값들을 accumulator, 즉 계속해서 전달받아서 사용할 수 있습니다. accumulator 를 어떻게 활용하냐에 따라 최종적인 return 값은 string, integer 가 될 수도, array 나 object 가 될 수도 있습니다.

이제부터는 reduce 를 활용해보면서 다른 메서드와의 차이점, 그리고 적재적소에 사용하는 다양한 방법, 감히“reduce 100% 활용법"에 대해 이야기해보겠습니다. 해당 내용은 자바스크립트 전문 사이트, [egghead 의 강의(Reduce Data with Javascript Array#reduce)](https://egghead.io/courses/reduce-data-with-javascript)를 참고했습니다.

## 1. reduce vs. map

```
var data = [1, 2, 3];

var initialValue = [];
var reducer = function(accumulator, value) {
  accumulator.push(value * 2);
  return accumulator;
};
var result = data.reduce(reducer, initialValue);
console.log(result); // [2, 4, 6]

var result2 = data.map(x => x * 2);
console.log(result2); // [2, 4, 6]
```

두 메서드 모두 같은 결과, 즉 원래 배열의 값들이 2배씩 커진 배열을 return 하게 됩니다. 하지만 map 이 훨씬 짧고 직관적이죠?

## 2. reduce vs. filter

```
var data = [1, 2, 3, 4, 5, 6];

var initialValue = [];
var reducer = function(accumulator, value) {
  if (value % 2 != 0) {
    accumulator.push(value);
  }
  return accumulator;
};
var result1 = data.reduce(reducer, initialValue);
console.log(result1); // [1, 3, 5]

var result2 = data.filter(x => x % 2 != 0);
console.log(result2); // [1, 3, 5]
```

두 메서드 역시 모두 같은 결과(원래 배열의 값들 중에서 2로 나눈 나머지가 0이 아닌 값들이 모인 배열)를 주지만 역시 filter 가 더 직관적으로 보입니다.

하지만, 1번과 2번의 상황을 동시에 작업해야하면 어떨까요? 다시 말해서, 원래 배열의 값들 중에서 2로 나눈 나머지가 0이 아닌 값들을 골라서 2배씩한 배열을 return 하고자 한다면 말이죠.

## 3. reduce vs. filter+map

```
var data = [1, 2, 3, 4, 5, 6];

var initialValue = [];
var reducer = function(accumulator, value) {
  if (value % 2 != 0) {
    accumulator.push(value * 2);
  }
  return accumulator;
}
var result1 = data.reduce(reducer, initialValue);
console.log(result1); // [2, 6, 10]

var result2 = data.filter(x => x % 2 != 0).map(x => x * 2);
console.log(result2); // [2, 6, 10]
```

네, reduce 는 배열을 1번만 순회하면 되지만 filter/map 조합은 두 번 순회해야하는 군요. 데이터 크기, 종류에 맞는 고려 및 결정이 필요해보입니다. filter/map 이 개인적으로 훨씬 더 직관적인 것으로 보이지만, reducer 라는 함수로 로직이 따로 빠져있는 reduce 가 더 재사용성이 있어 보입니다.

## 4. getMean(평균 구하기)

```
var data = [1, 2, 3, 4, 5, 6, 1];
var reducer = (accumulator, value, index, array) => {
  var sumOfAccAndVal = accumulator + value;
  if (index === array.length - 1) {
    return (sumOfAccAndVal) / array.length;
  }
  return sumOfAccAndVal;
};

var getMean = data.reduce(reducer, 0);
console.log(getMean); // 3.142857142857143
```

상기 코드는 배열을 순회하면서 accumulator 와 value 를 더해서 sum 을 만들고, 끝에 가서 배열의 크기로 나누는 로직입니다. 이때 초기값(initial value)을 0으로 셋팅했는데, 적지 않아도 됩니다. 그러면, 첫 번째 인자인 1(data[0]) 가 accumulator 로 넘어갑니다.

## 5. initial value 주의하기

```
const data = ["vote1", "vote2", "vote1", "vote2", "vote2"];
const reducer = (accumulator, value, index, array) => {
  if (accumulator[value]) {
    accumulator[value] = accumulator[value] + 1;
  } else {
    accumulator[value] = 1;
  }
  return accumulator;
};const getVote = data.reduce(reducer, {}); // { vote1: 2, vote2: 3 }
const getVote2 = data.reduce(reducer); // "vote1"
```

이 글이 시작할 때 처음에 보여주었던 예시(vote 배열 안의 값들의 개수를 세는 로직)와 같은 로직입니다만, initial value 가 있고 없음에 따라 큰 차이가 나는 getVote, getVote2 를 비교한 코드입니다.

getVote2 는 왜 예상과 다른 결과(“vote1”)가 나왔을까요? 그 이유는 reduce 메서드의 두 번째 인자로 아무 값도 전달이 되지 않았기 때문입니다.

배열의 첫 번째 값, “vote1” 이 첫 번째 순회의 accumulator 로 전달되었고, 조건문의 조건이 “vote1”[“vote2”] 이 되면서 결과는 undefined(false) 가 되어 “vote1”[“vote”] = 1; 이라는 의미없는 로직을 한 번 타고, 결론적으로 다음 accumulator 로 또 “vote1” 이라는 string 을 넘기게 된 것입니다. 따라서 최종적으로도 “vote1” 을 return 하게 되는 것입니다.

## 6. flatten (배열 납작하게 만들기)

```
const data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];
const flatArrayReducer = (accumulator, value, index, array) => {
  return accumulator.concat(value);
};const flattenedData = data.reduce(flatArrayReducer, []);
// [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
```

깊이(depth)가 있는 배열들을 flatten 할 때 배열을 순회하면서 concat (이어 붙이는) 로직을 reduce 를 활용해서 구현할 수 있습니다.

## 7. flattenMap

```
const input = [
  {
    "title": "슈퍼맨",
    "year": "2005",
    "cast": ["장동건", "권상우", "이동욱", "차승원"]
  },
  {
    "title": "스타워즈",
    "year": "2013",
    "cast": ["차승원", "신해균", "장동건", "김수현"]
  },
  {
    "title": "고질라",
    "year": "1997",
    "cast": []
  }
];
const flatMapReducer = (accumulator, value, index, array) => {
  const key = "cast";
  if (value.hasOwnProperty(key) && Array.isArray(value[key])) {
    value[key].forEach(val => {
      if (accumulator.indexOf(val) === -1) {
        accumulator.push(val);
      }
    });
  }
  return accumulator;
};const flattenCastArray = input.reduce(flatMapReducer, []);
// ['장동건', '권상우', '이동욱', '차승원', '신해균', '김수현']
```

배열을 순회하면서 배열의 값으로 들어있는 object 의 key 존재여부를 확인하고, unique 한 “cast 를 key 로 갖는 배열의 값들”을 최종적으로 return 하는 로직을 구현할 수 있습니다.

## 8. reduceRight

```
const data = [1, 2, 3, 4, "5"];
const sumData1 = data.reduce((accumulator, value) => {
  return accumulator + value;
}, 0);
const sumData2 = data.reduceRight((accumulator, value) => {
  return accumulator + value;
}, 0);
console.log(sumData1); // "105"
console.log(sumData2); // "054321"
```

string 과 number 의 합은 string 임을 고려한다면 배열의 끝(오른쪽)부터 순회를 시작하는 reduceRight 의 값, 즉 sumData2 가 왜 “054321” 이 나왔는지 알 수 있습니다.

0 과 “5” 가 첫 순회 때 + 되면서 “05” 가 되고, 계속해서 string 이 더해진 것입니다. 반면, sumData1 은 0 부터 4까지 더해짐으로써 10이 되었고 마지막에 “5” 와 합하게 되면서 “105” 가 return 된 것이고요.

## 9. reduce 를 활용한 함수형 프로그래밍

```
const increment = (input) => { return input + 1; };
const decrement = (input) => { return input - 1; };
const double = (input) => { return input * 2; };
const halve = (input) => { return input / 2 };// 1. 일반적일 수 있는 로직
const initial_value = 1;
const incremented_value = increment(initial_value);
const doubled_value = double(incremented_value);
const final_value = decrement(doubled_value);
console.log(final_value); // 3

// 2. reduce 를 활용한 함수형 프로그래밍
const pipeline = [
  increment,
  double,
  decrement,
  decrement,
  decrement,
  halve,
  double,
];
const final_value2 = pipeline.reduce((accumulator, func) => {
  return func(accumulator);
}, initial_value);
console.log(final_value2); // 1
```

reduce 의 메서드가 함수들의 이름이 적힌 배열에 대해 사용된다면, 상기코드와 같이 흥미로운 함수형 프로그래밍이 가능합니다. reduce 를 reduceRight 로 바꿔도 재미있는 결과가^^ 나오지요ㅎㅎㅎ

이것으로 reduce 를 활용한 9가지 패턴을 확인해보았습니다. 단순히 배열을 순회하면서 연산을 하는 것부터, 함수형 프로그래밍까지 모두 할 수 있습니다. 현업에서 개발을 할 때에도 이러한 점들을 유념하면서 코딩을 하면, 좀 더 직관적이고 짧은 길이의 코딩을 할 수 있을 것 같습니다!

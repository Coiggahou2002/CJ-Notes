# 正则表达式

## 单串匹配

```jsx
//匹配Hello，成功返回true，失败返回false
let myRegx = /Hello/;
let myString = "Hello World!";
myRegx.test(myString);
```

## 多串匹配

```jsx
let petString = "James has a pet cat.";
let petRegex = /dog|cat|bird|fish/; // 匹配dog,cat,bird,fish其中之一
let result = petRegex.test(petString);

let twinkleStar = "Twinkle, twinkle, little star";
let starRegex = /twinkle/gi; // 表达式后面可以跟多个标识符，g表示多串匹配
let result = twinkleStar.match(starRegex); // 此处会返回所有匹配项组成的数组
```

## 条件匹配

```jsx
let myString = "freeCodeCamp";
let fccRegex = /freecodecamp/i; // 最后加一个i表示忽略串中任何字母的大小写区别
let result = fccRegex.test(myString);
//也就是说，FreeCODEcamP, freeCODECaMp 都能被匹配
```

## 找出具体匹配项

```jsx
let myString = "I love coding!";
let myRegexp = /coding/;
const res = myString.match(myRegexp); //res = "coding"
```

## 模糊匹配

```jsx
// 使用通配符.可以跳过该字符的要求
let exampleStr = "Let's have fun with regular expressions!";
let unRegex = /.un/; //可以匹配出一个字母+un的单词
let result = unRegex.test(exampleStr);

// 使用 + 连接符，查找出现1次或多次的字符
let difficultSpelling = "Mississippi";
let myRegex = /s+/g; // 修改这一行
let result = difficultSpelling.match(myRegex);

/x+/ 匹配x,xx,xxx,xxxx...
/x*/ 匹配空字符和x,xx,xxx,xxxx,...
```

## 有限范围容错匹配

```jsx
// 使用字符集[xxx]
let myRegexp = /[fr]un/; //可以匹配fun和run

// 范围匹配
let myRegexp = /[a-z]/;
let myRegex = /[h-s2-6]/ig;  //h-s范围的所有字母和0-9范围的所有数字
```

## Greedy & Lazy Match

Greedy Match 会匹配符合要求的最长子串

Lazy Match 则是匹配符合要求的最短子串
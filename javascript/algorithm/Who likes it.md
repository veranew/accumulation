原题目： 
> You probably know the "like" system from Facebook and other pages. People can "like" blog posts, pictures or other items. We want to create the text that should be displayed next to such an item.
> Implement a function likes :: [String] -> String, which must take in input array, containing the names of people who like an item. It must return the display text as shown in the examples:
> ```js
> likes [] // must be "no one likes this"
> likes ["Peter"] // must be "Peter likes this"
> likes ["Jacob", "Alex"] // must be "Jacob and Alex like this"
> likes ["Max", "John", "Mark"] // must be "Max, John and Mark like this"
> likes ["Alex", "Jacob", "Mark", "Max"] // must be "Alex, Jacob and 2 others like this"
> ```
> 我的代码实现：
```js
function likes(names) {
  // TODO
  if (names.length === 0) {
    return "no one likes this";
  } else if (names.length === 1) {
    return names[0] + " likes this"
  } else if (names.length === 2) {
    return `${names[0]} and ${names[1]} like this`;
  } else if (names.length === 3) {
    return `${names[0]}, ${names[1]} and ${names[2]} like this`;
  } else if (names.length > 3) {
    return `${names[0]}, ${names[1]} and ${names.length - 2} others like this`
  }
}
```
ok, 以下是别的大佬的代码实现：
```js
function likes(names) {
  names = names || [];
  switch(names.length){
    case 0: return 'no one likes this'; break;
    case 1: return names[0] + ' likes this'; break;
    case 2: return names[0] + ' and ' + names[1] + ' like this'; break;
    case 3: return names[0] + ', ' + names[1] + ' and ' + names[2] + ' like this'; break;
    default: return names[0] + ', ' + names[1] + ' and ' + (names.length - 2) + ' others like this';
  }
}
```
先判断是不是空数组，然后用了switch.
```js
function likes(names) {
  names.length === 0 && (names = ["no one"]);
  let [a, b, c, ...others] = names;
  switch (names.length) {
    case 1: return `${a} likes this`;
    case 2: return `${a} and ${b} like this`;
    case 3: return `${a}, ${b} and ${c} like this`;
    default: return `${a}, ${b} and ${others.length + 1} others like this`;
  }
}
```
这个就有点厉害了，开始用&&，如果数组长度为0，那么就给一个成员“no one”，如果数组有长度，就往下走。
按照实例要求，我们最多只需要数组前三个成员的信息，所以接下来解构赋值，下面就不要一直names[0]...这么写了。
然后用switch条件判断，模板字符串，就OK啦！
```js
function likes (names) {
  var templates = [
    'no one likes this',
    '{name} likes this',
    '{name} and {name} like this',
    '{name}, {name} and {name} like this',
    '{name}, {name} and {n} others like this'
  ];
  var idx = Math.min(names.length, 4);
  
  return templates[idx].replace(/{name}|{n}/g, function (val) {
    return val === '{name}' ? names.shift() : names.length;
  });
}
```
这种方法也值得借鉴，先给一个数组，把可能的情况都列出来。再根据情况把数组中成员需要改变的部分替换掉。用到了Math.min()方法，replace()方法，和shift()方法。这种方法尤其是后面replace里的内容会稍微有点绕。
相比较而言，下面这种会好理解一些：
```js
function likes(names) {
  return {
    0: 'no one likes this',
    1: `${names[0]} likes this`, 
    2: `${names[0]} and ${names[1]} like this`, 
    3: `${names[0]}, ${names[1]} and ${names[2]} like this`, 
    4: `${names[0]}, ${names[1]} and ${names.length - 2} others like this`, 
  }[Math.min(4, names.length)]
}
```
少起了一个变量，并且用模板字符串让脑袋少绕了点弯~
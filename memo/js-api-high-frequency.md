## Array.prototype

`arr.find((element, index, array) => { /* … */ } )` 在数组中找到符号要求的元素，返回这个元素

`arr.filter((element, index, array) => { /* … */ } )` 过滤出符合要求的元素，返回一个数组

```js
// find
let usersArray=[{name:'Bob'},{name:'Smith'},{name:'jeffrey'},{name:'frank'}]
let user = usersArray.find((user) => user.name === 'jeffrey')

// filter
let usersArray=[{age:17},{age:20},{age:19},{age:14}]
let user_arr = usersArray.filter((user) => user.age <= 18)
```


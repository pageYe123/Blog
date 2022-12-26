## v-for

```js
new Vue({
    data(){
        return {
          users: [
            createUser('方方', 'male'),
            createUser('圆圆', 'female'),
            createUser('小新', 'male')
          ]
        }
    },
template:`
    <ul>
        <li v-for="(u,index) in users" :key="index">{{u.name}} - {{u.gender}}</li>
    </ul>
`}).$mount('#app')
```


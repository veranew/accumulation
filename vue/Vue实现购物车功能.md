这个是效果图（丑就丑点吧，功能实现就行）：
![效果图](https://img-blog.csdnimg.cn/20191212170726957.gif)

1. 选中的商品，计算对应的总价
2. 全选与反选，总价对应变化
3. 步进器，选中的商品的数量加减的同时，总价也相应变化
4. 移除，对应总价变化
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>购物车示例</title>
  <link rel="stylesheet" href="./shopcar.css">
</head>
<body>
  <div id="app" v-cloak>
       <template v-if="list.length">
        <table>
          <thead>
            <tr>
              <th></th>
              <th></th>
              <th>商品名称</th>
              <th>商品单价</th>
              <th>购买数量</th>
              <th>操作</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in list" :key="index">
              <td>
                <input type="checkbox" v-model="checked" :value="item" @change="checkSingle()">
              </td>
              <td>{{ index }}</td>
              <td>{{ item.name }}</td>
              <td>{{ item.price }}</td>
              <td>
                <span @click="reduceCount(index)"> - </span>
                {{ item.count }} 
                <span @click="addCount(index)"> + </span>
              </td>
              <td><button @click="removeProduct(index)">移除</button></td>
            </tr>
          </tbody>
        </table>
        <div>
          <input type="checkbox" @click="checkToAll" v-model="checkAll">全选
          总价：￥{{ totalPrice }}</div>

      </template>
      <template v-else>
        购物车为空
      </template>
  </div>
  <script src="https://unpkg.com/vue/dist/vue.min.js "></script>
  <script src="./shopcar.js"></script>
</body>
</html>
```

```css
[v-cloak] {
  display: none;
}
```

```js
var app = new Vue({
  el: '#app',
  data: {
    list: [
      {
        id: 1, 
        name:'iPhone 7', 
        price: 6188 , 
        count: 1,
      },
      {
        id: 2, 
        name:'iPad Pro', 
        price: 2188 , 
        count: 1,
      },
      {
        id: 3, 
        name:'MI 9', 
        price: 3188 , 
        count: 1,
      },
      {
        id: 4, 
        name:'huawei', 
        price: 4188 , 
        count: 1,
      },
      {
        id: 5, 
        name:'Sumsung', 
        price: 8188 , 
        count: 1,
      },
    ],
    checkAll: false,
    checked: [],
  },
  computed: {
    // 根据是否选中来计算总价
    totalPrice: function(){
      var total = 0;
      for (var i = 0; i< this.checked.length; i++) {
          var item = this.checked[i];
          total += item.price * item.count;
      } 
      return total.toString().replace(/\B(?=(\d{3})+$)/,','); // 千位分隔符
    }
  },
  methods: {
    // 减
    reduceCount(index){
      if(this.list[index].count === 1) return;
      this.list[index].count --;
    },
    // 加
    addCount(index){
      this.list[index].count ++;
    },
    // 移除
    removeProduct(index){
      // 判断移除的商品是否在选择框中
      var i = this.checked.indexOf(this.list[index]);
      if(i !== -1) {
        this.checked.splice(i, 1);
      }
      this.list.splice(index,1);
      this.totalPrice()
    },
    // 点击单个按钮，判断是否需要选中全选按钮的功能
    checkSingle(){
      if(this.checked.length === this.list.length){
        this.checkAll = true;
      } else {
        this.checkAll = false;
      }
    },
    // 点击全选按钮
    checkToAll(){
      var _this = this;
      if(this.checkAll) {
        // 实现反选
        this.checked = [];
      } else {
        // 实现全选
        this.checked = []
        this.list.forEach(function (item) {
            _this.checked.push(item)
        })
      }
      if (this.checked.length === this.list.length) {
          this.checkAll = true
      }
    }
  },
});
```
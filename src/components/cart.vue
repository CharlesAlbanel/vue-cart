<template>
  <div class="cart">
    <h1 class="topTitle">{{ msg }}</h1>
    <ul>
      <li v-for="item in  productList">
        商品：{{item.name}}  单价: ￥{{item.price}} 数量:<button @click="change(item,1)">+</button>{{item.count}}<button @click="change(item,-1)">-</button> 
      </li>
    </ul>
    <h2>总价：￥ <span>{{totalMoney}}</span></h2>
  </div>
</template>

<script>
export default {
  name: 'cart',
  data () {
    return {
      msg: 'vue面试题简单的购物车',
      totalMoney: 0,
      productList: []
    }
  },
  mounted:function() {
    this.$nextTick(function(){
      this.cartView();
    })
  },
  methods: {
    cartView: function() {
      var _this = this;
      this.$http.get("../../static/data.json").then(function(res){
        _this.productList = res.body.shoppingList;
        _this.totalMoney = res.body.totalMoney;
      });
    },
    change: function(item,way){
      //数量的加减
      if (way > 0) {
        item.count++;
      }else {
        item.count--;
        if(item.count < 1) {
          item.count = 1;
        }
      }
      //计算总价
      this.totalPrice();
    },
    totalPrice: function(){
      var _this = this;
      _this.totalMoney = 0;
      this.productList.forEach(function(item,index){
      _this.totalMoney += item.price * item.count;
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
.topTitle{
  padding: 8px 0;
  border-top:1px solid rgba(0,0,0,0.1);
  border-bottom:1px solid rgba(0,0,0,0.1);
}
ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>

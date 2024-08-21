<script setup>
import { ref, computed } from 'vue'

//itemList
const data = ref([
  {
    id: 1,
    name: '珍珠奶茶',
    subscription: '香濃奶茶搭配QQ珍珠',
    price: 50,
    stock: 20,
    total: 'num1',
    way: 'minus1'
  },
  {
    id: 2,
    name: '冬瓜檸檬',
    subscription: '清新冬瓜配上新鮮檸檬',
    price: 45,
    stock: 18
  },
  {
    id: 3,
    name: '翡翠檸檬',
    subscription: '綠茶與檸檬的完美結合',
    price: 55,
    stock: 34
  },
  {
    id: 4,
    name: '四季春茶',
    subscription: '香醇四季春茶，回甘無比',
    price: 45,
    stock: 10
  },
  {
    id: 5,
    name: '阿薩姆奶茶',
    subscription: '阿薩姆紅茶搭配香醇鮮奶',
    price: 50,
    stock: 25
  },
  {
    id: 6,
    name: '檸檬冰茶',
    subscription: '檸檬與冰茶的清新組合',
    price: 45,
    stock: 20
  },
  {
    id: 7,
    name: '芒果綠茶',
    subscription: '芒果與綠茶的獨特風味',
    price: 55,
    stock: 18
  },
  {
    id: 8,
    name: '抹茶拿鐵',
    subscription: '抹茶與鮮奶的絕配',
    price: 60,
    stock: 20
  }
])
const drinks = ref(data)
const cart = ref([])
const description = ref('')
const order = ref({})
const itemSubtotal = (item) => {
  return item.price * item.quantity
}
const sum = computed(() => {
  return cart.value.reduce((pre, next) => {
    return pre + next.price * next.quantity
  }, 0)
})

const addToCart = (drink) => {
  cart.value.push({
    ...drink,
    id: new Date().getTime(),
    quantity: 1
  })
}

const updateCart = (item) => {
  cart.value = cart.value.map((cartItem) => {
    if (cartItem.id === item.id) {
      cartItem.quantity = parseInt(item.quantity)
    }
    return cartItem
  })
}

// const removeFromCart = (id) => {
//   cart.value = cart.value.filter((cartItem) => cartItem.id !== id);
// };
const createOrder = () => {
  order.value = {
    id: new Date().getTime(),
    cart: cart.value,
    description: description.value,
    sum: sum.value
  }
  cart.value = []
  description.value = ''
}
</script>

<template>
  <div class="mainBox">
    <div
      class="menuList"
      v-for="drink in drinks"
      :key="drink.id"
      href="#"
      @click.prevent="addToCart(drink)"
    >
      <a>
        {{ drink.name }}
        {{ drink.price }}
        <div>
          {{ drink.subscription }}
        </div>
      </a>
    </div>
    <!-- <MenuList v-bind:drink="data" /> -->
  </div>

  <div class="checkBox">
    <table class="table">
      <thead>
        <tr>
          <th scope="col">品項</th>
          <th scope="col" width="200" text-align="center">描述</th>
          <th scope="col">數量</th>
          <th scope="col">單價</th>
          <th scope="col">小計</th>
          <th scope="col" width="50">操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in cart" :key="item.id">
          <td>{{ item.name }}</td>
          <td>
            <small>{{ item.subscription }}</small>
          </td>
          <td>
            <select class="numSelect" v-model="item.quantity" @change="updateCart(item)">
              <option v-for="n in 10" :key="n" :value="n">{{ n }}</option>
            </select>
          </td>
          <td>{{ item.price }}</td>
          <td>{{ itemSubtotal(item) }}</td>
          <td>
            <button type="button" class="btn btn-sm" @click="removeFromCart(item.id)">x</button>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-if="cart.length !== 0">
      <input class="listText" type="textarea" v-model="description" />
      <br />
      <button @click.prevent="createOrder">送出</button>
    </div>
    <div v-else>請選擇商品</div>
  </div>
  <div class="orderClass">
    <h3>訂單</h3>
    <table class="table">
      <thead>
        <tr>
          <th scope="col" width="100px">品項</th>
          <th scope="col" width="100px">數量</th>
          <th scope="col" width="100px">小計</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in order.cart" :key="item.id">
          <td>{{ item.name }}</td>
          <td>{{ item.quantity }}</td>
          <td>{{ itemSubtotal(item) }}</td>
          <td>{{ item.description }}</td>
        </tr>
      </tbody>
    </table>
    <div>備註：{{ order.description }}</div>
  </div>
</template>

<style>
.mainBox {
  width: 300px;
  margin: 100px 50px 0 100px;
  display: inline-block;
}
.menuList {
  border: 2px solid gray;
  cursor: pointer;
}
.checkBox {
  display: inline-block;
  margin: 0 auto;
  position: absolute;
  top: 100px;
  text-align: center;
}
.listText {
  width: 400px;
  height: 100px;
}
tr {
  text-align: center;
}
.orderClass {
  width: 500px;
  margin: 50px auto;
  border: 2px solid gray;
}
</style>

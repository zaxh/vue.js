<template>
  <div class="shopCart">
    <div @click="showa" class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo active">
            <i class="icon-shopping_cart"></i>
          </div>
          <div class="badge" v-show="totalPrice>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'active': totalPrice}"">&yen;{{totalPrice}}</div>
        <div class="desc">另需要配送费&yen;{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" :class="{'enough': totalPrice > minPrice}">
        <div class="pay">{{payDesc}}</div>
      </div>
    </div>
    <transition name="transHeight">
       <div v-show="totalPrice>0 && show" class="shopcart-list">
         <div class="list-header">
           <h1 class="title">购物车</h1>
           <span class="empty" @click="empty">清空</span>
         </div>
         <div ref="listcontent" class="list-content">
           <ul>
             <li class="food" v-for="item in food">
              <span class="name">{{item.name}}</span>
              <div class="price">
                 <span>&yen;{{item.price}}</span>
               </div>
               <div class="cartcontrol-wrapper">
                  <cartcontrol :food="item"></cartcontrol>
               </div>
             </li>
          </ul>
         </div>
       </div>
    </transition>

  </div>
</template>
<script>
// 滚动插件必须等dom更新完之后才可以用 new
  import Scroll from 'better-scroll'
  import cartcontrol from './cartcontrol.vue'
  export default {
    props: ['deliveryPrice', 'minPrice', 'food'],
    data () {
      return {
        show: false
      }
    },
    computed: {
      totalPrice () {
        let total = 0
        this.food.forEach(food => {
          total += food.price * food.count
        })
        return total
      },
      totalCount () {
        let count = 0
        this.food.forEach(food => {
          count += food.count
        })
        return count
      },
      payDesc () {
        if (this.totalPrice === 0) {
          return `${this.minPrice}元起送`
        } else if (this.totalPrice < this.minPrice) {
          return `还差${this.minPrice - this.totalPrice}元起送`
        } else {
          return `去结算`
        }
      }
    },
    methods: {
      empty () {
        this.food.forEach(val => {
          val.count = 0
        })
        this.show = false
      },
      showa () {
        this.show = !this.show
        // 等dom完成更新之后
        this.$nextTick(() => {
          if (!this.sc) {
            this.sc = new Scroll(this.$refs['listcontent'], {
              click: true
            })
          } else {
          // 强制重新计算高度
            this.sc.refresh()
          }
        })
      }
    },
    components: {
      cartcontrol
    }
  }
</script>

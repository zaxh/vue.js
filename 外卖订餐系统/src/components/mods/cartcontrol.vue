<template>
  <div class="cartcontrol">
    <transition name="fadeRotate">
      <div class="cart-decrease" v-show="food.count>0" @click.stop="decreaseCart($event)">
        <span class="icon-remove_circle_outline inner"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add" @click.stop="addCart($event)">
      <i class="icon-add_circle"></i>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import {mapMutations, mapState} from 'vuex'
export default{
  props: ['food'],
  methods: {
    ...mapMutations([
      'vxaddCart',
      'vxdecreaseCart'
    ]),
    addCart (event) {
    // _constructed 是滚动插件添加的一个属性
      if (!event._constructed) {
        return false
      }
      if (typeof this.food.count === 'undefined') {
        Vue.set(this.food, 'count', 0)
        Vue.set(this.food, 'active', true)
      }
      this.food.count ++
      // 默认为true
      if (this.food.active) {
        this.vxaddCart(this.food)
        this.food.active = false
      }
      this.vxaddCart(this.food)
    },
    decreaseCart (event) {
      if (!event._constructed) {
        return false
      }
      this.food.count --
      if (this.food.count === 0) {
        this.vxdecreaseCart(this.food)
      }
    }
  },
  computed: {
    ...mapState([
      'vxfood'
    ])
  }
}
</script>

<style>
</style>

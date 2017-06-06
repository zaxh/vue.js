<template>
	<div class="star" :class="'star-' + size">
	  <span class="star-item" :class="item" v-for="item in yimao"></span>
	</div>
</template>

<script>
export default {
  props: ['score', 'size'],
  computed: {
    yimao () {
      let arr = []
    //  获取整数部分 向下取整
      let sun = Math.floor(this.score)
    //  小数部分
      let decimal = this.score - sun
      for (let i = 0; i < sun; i++) {
        arr.push('on')
      }

      if (decimal >= 0.5) {
        arr.push('half')
      }

      while (arr.length < 5) {
        arr.push('off')
      }

      return arr
    }
  }
}
</script>

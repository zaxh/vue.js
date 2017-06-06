<template>
  <div class="ratingsWrapper" ref="ratingsWrapper">
    <div class="ratings-content">
      <div class="info">
        <div class="mark">
          <div class="num">{{datas.seller.score}}</div>
          <div class="text">综合评分</div>
          <div class="contrast">高于周边商家{{datas.seller.rankRate}}%</div>
        </div>
        <div class="stars">
          <div class="serviceScore">
            <span class="text">服务态度</span>
            <star :size="36" :score="datas.seller.score"></star>
            <span class="num">{{datas.seller.score}}</span>
          </div>
          <div class="serviceScore">
            <span class="text">商家评价</span>
            <star :size="36" :score="datas.seller.score"></star>
            <span class="num">{{datas.seller.score}}</span>
          </div>
          <div class="deliveryTime">
            <span class="text">送达时间</span>
            <span class="time">{{datas.seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <div class="divider"></div>
      <div class="evaluation">
        <div class="classify">
          <span class="item" :class="{'active': item.active, 'bad': index===2, 'badActive': index===2&&item.active}" v-for="(item,index) in classify" @click="classifyClick(item)">
            {{item.name}}
            <span class="count">{{item.count}}</span>
          </span>
        </div>
        <div class="switch" @click="evelflag=!evelflag">
          <span class="icon-check_circle" :class="{'on': evelflag}"></span>
          <span class="text">只看有内容的评价</span>
        </div>
        <div class="evel-list">
          <ul>
            <li class="evel" v-for="item in commontss">
              <div class="avatar">
                <img :src="item.avatar" alt="用户头像" width="28" height="28" />
              </div>
              <div class="content">
                <div class="user">
                  <span class="name">{{item.username}}</span>
                  <span class="rateTime">{{item.rateTime}}</span>
                </div>
                <div class="star-wrapper">
                  <star :size="24" :score="item.score"></star>
                </div>
                <div class="text">{{item.text}}</div>
                <div class="recommend">
                  <span class="icon icon-thumb_up" v-show="item.recommend.length"></span>
                  <span class="dish" v-for="dish in item.recommend">{{dish}}</span>
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Scroll from 'better-scroll'
import star from './mods/star.vue'
export default {
  props: ['datas'],
  data () {
    return {
      ratings: [],
      classify: [
        {name: '全部', 'active': true, count: 0, commont: []},
        {name: '推荐', 'active': false, count: 0, commont: []},
        {name: '吐槽', 'active': false, count: 0, commont: []}
      ],
      evelflag: true,
      commonts: []
    }
  },
  activated () {},
  mounted () {
    this.$nextTick(() => {
      axios.get('./././static/data.json').then(res => {
        this.ratings = res.data.ratings
        this.ratings.forEach(val => {
          this.classify[0].count++
          this.classify[0].commont.push(val)
          if (val.rateType) {
            this.classify[2].count++
            this.classify[2].commont.push(val)
          } else {
            this.classify[1].count++
            this.classify[1].commont.push(val)
          }
        })
        this.commonts = this.ratings
        this.$nextTick(() => {
          /* eslint-disable no-new */
          this.sc = new Scroll(this.$refs['ratingsWrapper'], {
            click: true
          })
        })
      })
    })
  },
  components: {
    star
  },
  methods: {
    classifyClick (item) {
      this.classify.forEach(val => {
        val.active = false
      })
      item.active = true
      this.commonts = item.commont
    },
    updatasc () {
      this.$nextTick(() => {
        this.sc.refresh()
      })
    }
  },
  computed: {
    commontss () {
      this.updatasc()
      if (this.evelflag) {
        let arr = []
        this.commonts.forEach(val => {
          if (val.text.length > 0) {
            arr.push(val)
          }
        })
        return arr
      } else {
        return this.commonts
      }
    }
  }
}
</script>

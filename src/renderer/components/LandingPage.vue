<template>
  <div id="wrapper">
    <Nav tab="Time"></Nav>
    <div class="e-box" v-for="(item, index) in target" :key="item.elem">
      <div class="info">
        <div class="detail">
          <h4>{{item.title}}</h4>
          <!-- <p>元素：{{item.elem}}</p> -->
        </div>
        <div class="btns">
          <a class="e-btn white small">编辑</a>
          <a :class="`${item.pause ? 'e-btn danger small' : 'e-btn primary small'}`" @click="control(item, index)" v-if="item.pause">启动</a>
          <a :class="`${item.pause ? 'e-btn danger small' : 'e-btn primary small'}`" @click="control(item, index)" v-else>勘测...</a>
        </div>
      </div>
      <div class="in-progress">
        <!-- <progress class="e-progress primary" :value="item.progress" max="100" style="height:2px;width: 70%;margin-right: 20px;"></progress> -->
        <div class="status">
          <div class="e-tags unified">
            <span class="e-tag rounded shadow primary">上回</span>
            <span class="e-tag rounded shadow grey">{{item.lastFetch}}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Nav from './Nav'
  import target from '../assets/target.json'
  let timer = {}
  export default {
    name: 'landing-page',
    components: {
      Nav
    },
    data () {
      return {
        target: JSON.parse(JSON.stringify(target.map(item => {
          item.lastFetch = this.convertTime()
          return item
        })))
      }
    },
    created () {
      this.handleTarget()
    },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      },
      control (tar, index) {
        const isPause = tar.pause
        this.target[index].pause = !isPause
        if (isPause) {
          timer[tar.title] = setInterval(() => {
            this.countOne(tar, index)
          }, 1000)
        } else {
          this.target[index].progress = 100
          clearInterval(timer[tar.title])
        }
      },
      countOne (tar, index) {
        tar.interval -= 1000
        if (tar.interval < 0) {
          tar.interval = target[index].interval
          this.fetch(tar, index)
          tar.lastFetch = this.convertTime()
        }
        this.target[index] = tar
      },
      convertTime () {
        const now = new Date()
        let hour = now.getHours() < 10 ? `0${now.getHours()}` : now.getHours()
        let minute = now.getMinutes() < 10 ? `0${now.getMinutes()}` : now.getMinutes()
        let second = now.getSeconds() < 10 ? `0${now.getSeconds()}` : now.getSeconds()
        return `${hour}:${minute}:${second}`
      },
      handleTarget () {
        this.target.forEach((tar, index) => {
          timer[tar.title] = setInterval(() => {
            this.countOne(tar, index)
          }, 1000)
        })
      },
      fetch (tar, index) {

      }
    }
  }
</script>
<style lang="scss" scoped>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}


#wrapper {
  background:
    radial-gradient(
      ellipse at top left,
      rgba(255, 255, 255, 1) 40%,
      rgba(229, 229, 229, .9) 100%
    );
  height: 100vh;
  width: 100vw;
  padding: 1rem;
}
.e-progress {
  transition: .2s all;
}
.info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  .detail {
    margin-bottom: 10px;
  }
}
.in-progress {
  display: flex;
  flex-wrap: nowrap;
  align-items: center;
  .status {
    font-size: 13px;
  }
}
</style>
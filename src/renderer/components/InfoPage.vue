<template>
  <div class="info-page">
    <Nav tab="Info"></Nav>
    <div class="content">
      <ul class="list e-box">
        <li class="item" v-for="(item, index) in rankList" :key="item.title">
          <a href="#" @click="showWebview(item.url)"><span class="e-tag shadow-3 warning rounded">{{index + 1}}</span>{{item.title}}</a>
        </li>
      </ul>
      <div class="e-box">
        <webview
          :src="webviewUrl"
          style="display:inline-flex; width:414px; height:736px"
          useragent="Mozilla/5.0 (iPhone; CPU iPhone OS 10_3_3 like Mac OS X) AppleWebKit/603.1.30 (KHTML, like Gecko) CriOS/59.0.3071.102 Mobile/14G60 Safari/602.1"
        ></webview>
      </div>
      
    </div>
    <div class="select-btn">
      <a :class="`e-btn circle primary ${active === 1 ? '' : 'is-active'}`" @click="changeList(1)"><i class="fab fa-zhihu"></i></a>
      <a :class="`e-btn circle inst-gradient ${active === 2 ? '' : 'is-active'}`" @click="changeList(2)"><i class="fab fa-weibo"></i></a>
    </div>
  </div>
</template>
<script>
import Nav from './Nav'
const superagent = require('superagent')
const cheerio = require('cheerio')
export default {
  name: 'info-page',
  components: {
    Nav
  },
  data () {
    return {
      rankList: [],
      weiboUrl: 'https://s.weibo.com',
      active: 1,
      webviewUrl: ''
    }
  },
  created () {
    this.getListData()
  },
  methods: {
    getListData () {
      superagent.get(`${this.weiboUrl}/top/summary`).end((err, res) => {
        if (err) {
          let noti = new Notification(`出错`, {
            body: err
          })
          noti.onclick = function () {}
        }
        let $ = cheerio.load(res.text)
        const rankList = []
        $('.wbs-hotrank .data td').each((i, elem) => {
          console.log('elem', elem)
          if (elem.attribs.class === 'td-02') {
            rankList.push({
              title: elem.children[1].children[0].data,
              url: elem.children[1].attribs.href
            })
          }
        })
        this.rankList = rankList.filter((item, index) => ![0, 3].includes(index))
      })
    },
    changeList (active) {
      this.active = active
    },
    showWebview (url) {
      this.webviewUrl = this.weiboUrl + url
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
.info-page {
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
.content {
  display: flex;
  justify-content: space-between;
}
.select-btn {
  text-align: center;
  position: fixed;
  bottom: 20px;
  left: 0;
  right: 0;
  .is-active {
    opacity: .3;
  }
}
.item {
  font-size: 13px;
  font-size: 13px;
  padding: 4px 0;
  .e-tag {
    text-align: center;
    display: inline-block;
    width: 29px;
    margin-right: 10px;
  }
}
.list {
  max-height: 700px;
  overflow: scroll;
  overflow-x: hidden;
  width: 44%;
}
</style>

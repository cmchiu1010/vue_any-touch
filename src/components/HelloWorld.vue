<!--
 * @Author: mike.chiu
 * @Date: 2021-05-15 09:31:39
 * @LastEditors: mike.chiu
 * @LastEditTime: 2021-05-17 15:38:29
 * @Description: 
-->
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org"
         target="_blank"
         rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel"
           target="_blank"
           rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint"
           target="_blank"
           rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org"
           target="_blank"
           rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org"
           target="_blank"
           rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org"
           target="_blank"
           rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs"
           target="_blank"
           rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org"
           target="_blank"
           rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org"
           target="_blank"
           rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org"
           target="_blank"
           rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools"
           target="_blank"
           rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org"
           target="_blank"
           rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue"
           target="_blank"
           rel="noopener">awesome-vue</a></li>
    </ul>
    <hr />
    <div ref="panel"
         class="p1"
         @tap="onTap"
         @swipe="onSwipe"
         @press="onPress"
         @pan="onPan"
         @pinch="onPinch"
         @rotate="onRotate">
      <div class="p2"
           v-for="(card, index) in datas"
           :key="index"
           :style="card.bgcolor"
           v-show="index >= (pageCurrent -1) * pageSize && index < pageCurrent * pageSize">
        {{ card.label }}, {{ card.value }}, {{ index }}
      </div>
    </div>
  </div>
</template>

<script>
import AnyTouch from 'any-touch'

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      styles: [
        { left: `50px`, top: `160px`, zIndex: 1, scale: 1, angle: 0 },
        { left: `50px`, top: `320px`, zIndex: 1, scale: 1, angle: 0 },
        { left: `50px`, top: `480px`, zIndex: 1, scale: 1, angle: 0 },
      ],
      pIndex: 6,
      pageCurrent: 1,
      pageSize: 6,
      datas: [
        { label: 'a', value: 1, bgcolor: 'background-color: #ff9' },
        { label: 'b', value: 2, bgcolor: 'background-color: #ff6' },
        { label: 'c', value: 3, bgcolor: 'background-color: #ff3' },
        { label: 'd', value: 4, bgcolor: 'background-color: #f9f' },
        { label: 'e', value: 5, bgcolor: 'background-color: #f6f' },
        { label: 'f', value: 6, bgcolor: 'background-color: #f3f' },
        { label: 'g', value: 7, bgcolor: 'background-color: #9ff' },
        { label: 'h', value: 8, bgcolor: 'background-color: #6ff' },
        { label: 'i', value: 9, bgcolor: 'background-color: #3ff' },
      ]
    }
  },
  mounted () {
    const at = new AnyTouch(this.$refs.panel, { preventDefault: true });
    this.$on('hook:destroyed', () => { at.destroy() });
  },
  methods: {
    onTap (ev) { // 點擊
      console.log(ev)
    },
    onSwipe ({ speedX, speedY, displacementX }, index) { // 滑動
      let vm = this
      if (displacementX > 300) { // 向右滑動
        if (((vm.pageCurrent - 1) - 1) * vm.pageSize >= 0) {
          vm.pageCurrent = vm.pageCurrent - 1
        }
        console.log(`向右滑動, ${displacementX}`)
      } else if (displacementX < -300) { // 向左滑動
        if (((vm.pageCurrent + 1) - 1) * vm.pageSize < vm.datas.length) {
          vm.pageCurrent = vm.pageCurrent + 1
        }
        console.log(`向左滑動, ${displacementX}`)
      }

      this.styles[index].top = Math.round(parseInt(this.styles[index].top) + speedY * 120) + 'px';
      this.styles[index].left = Math.round(parseInt(this.styles[index].left) + speedX * 120) + 'px';
      console.log(`${index}, ${vm.pIndex}`)
    },
    onPress (ev) { // 按壓
      //   C(ev.type, '#710');
      console.log(ev)
    },
    onPan (ev) { // 拖曳
      console.log(ev)
    },
    onPinch (ev, index = 0) { // 縮放
      // if(ev.isMatch() ) return;
      // console.log(`deltaScale:${ev.deltaScale}`,ev.stage,ev.pointLength)
      this.styles[index].scale = Math.round(this.styles[index].scale * ev.deltaScale * 100) / 100;
      console.log(ev)
    },
    onRotate (ev, index = 0) { // 旋轉
      // if(ev.isMatch() ) return;
      // console.log(`deltaAngle:${ev.deltaAngle}`,ev.stage,ev.pointLength)
      this.styles[index].angle += ev.deltaAngle;
      console.log(ev)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.p1 {
  width: 100%;
  height: 600px;
  background-color: cornsilk;
}
.p2 {
  width: calc(100% / 3);
  height: 30px;
  float: left;
}
</style>

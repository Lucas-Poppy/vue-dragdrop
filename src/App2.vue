<template>
  <div id="app" style="position:relative;">
    <div class="wrapper">
      <div class="box-list">
        <div class="box-parent" 
          v-for="(box, boxKey) in list" 
          :key="boxKey"
          @mouseup="parentMouseUp(boxKey)"
          >
          <div class="box-child" 
           v-for="(block, blockKey) in box"
           :key="blockKey"
           @mousedown="mouseDown(boxKey, blockKey, $event)"
           :ref="boxKey + '_' + blockKey"
           >{{block}}</div>
        </div>
      </div>
    </div>
    <div v-if="copyBlock" class="box-child" ref="copy" @mouseup="mouseUp" @mousemove="copyMove">{{this.list[copyBlock.boxKey][copyBlock.blockKey]}}</div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function () {
    return {
      list: {
        box1:[
          "aaa",
          "bbb",
          "ccc"
        ],
        box2:[
          "hogehoge",
        ],
        box3:[
          "アイウエオ",
          "下記くけこ",
          "さしすせそ"
        ],
        box4:[
          "AAA",
          "CCC",
          "DDD"
        ],
      },
      copyBlock:null,
    }
  },
  methods:{
    /**
     * コピーもとがmousedownした場合、コピーブロックを生成し、同じ座標に持ってくる
     */
    async mouseDown(boxKey, blockKey, event) {
      await new Promise((resolve) => {
        this.copyBlock = {
          boxKey,
          blockKey
        };
        resolve();
      });
      event.target.style.opacity = 0.5;
      this.$refs['copy'].style.position = 'absolute';
      this.$refs['copy'].style.left = `${window.pageXOffset + event.target.getBoundingClientRect().left - 8}px`;
      this.$refs['copy'].style.top = `${window.pageYOffset + event.target.getBoundingClientRect().top - 60}px`;
    },
    /**
     * コピーブロックのmouseup
     * コピー元のopacityを1に戻し、copyBlockをnullにする
     */
    mouseUp() {
      const target = this.$refs[`${this.copyBlock.boxKey}_${this.copyBlock.blockKey}`]
      target.style.opacity = 1;
      this.copyBlock = null;
    },
    /**
     * コピーブロックがmouseupした時にparentのmouseupを発火させたいが、直接の親要素じゃないのでイベントが伝搬できない...
     */
    parentMouseUp(boxKey) {
      console.log('parentMouseUp');
      this.list[boxKey].push(this.list[this.copyBlock.boxKey][this.copyBlock.blockKey]);
      this.list[this.copyBlock.boxKey].splice(this.copyBlock.blockKey, 1)
    },
    /**
     * コピーブロックのmousemove
     */
    copyMove(event) {
      this.$refs['copy'].style.left = `${event.pageX - 44}px`;
      this.$refs['copy'].style.top = `${event.pageY - 88}px`;
    },
  }
};
</script>

<style>
*, *:after, *:before {
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.wrapper {
  margin: 0 auto;
  width: 980px;
}
.box-list {
  display: flex;
  flex-wrap: wrap;

}

.box-parent {
  display: flex;
  flex-wrap: wrap;
  background-color: #f7f7f7;
  width: 400px;
  height: 400px;
  padding: 50px;
  margin-bottom: 100px;
}

.box-parent:nth-child(2n-1) {
  margin-right: 180px;
}

.box-child {
  width: 88px;
  height: 90px;
  background-color: #cccccc;
  margin-bottom: 15px;
}

.box-child:not(:nth-child(3n)) {
  margin-right: 15px;
}
</style>

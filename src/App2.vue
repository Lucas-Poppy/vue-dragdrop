<template>
  <div id="app" style="position:relative;">
    <div class="wrapper">
      <div class="box-list">
        <div class="box-parent" 
          v-for="(box, boxKey) in list" 
          :key="boxKey"
          >
          <div class="box-child" 
           v-for="(block, blockKey) in box"
           :key="blockKey"
           @mousedown="mouseDown(boxKey, blockKey, $event)"
           @mouseup="mouseUp(boxKey, blockKey, $event)"
           :style="copyStyle(boxKey, blockKey)"
           >{{block}}</div>
        </div>
      </div>
    </div>
    <!-- <div v-if="copyBlock" class="box-child" ref="copy" @mousemove="copyMove">{{copyBlock}}</div> -->
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
      copyBlock:{},
    }
  },
  methods:{
    async mouseDown(boxKey, blockKey, event) {
      this.copyBlock = {
        boxKey,
        blockKey
      }
      // this.$refs['copy'].style.position = 'absolute';
      // console.log(this.$refs['copy'].style.top, event.target.getBoundingClientRect().top);
      // console.log(this.$refs['copy'].style.left);

      // this.$refs['copy'].style.left = `${window.pageXOffset + event.target.getBoundingClientRect().left - 8}px`;
      // this.$refs['copy'].style.top = `${window.pageYOffset + event.target.getBoundingClientRect().top - 60}px`;
    },
    mouseUp(boxKey, blockKey, event) {
      this.copyBlock = null;
      // event.target.style.opacity = 1;
    },
    copyMove(event) {
      // if(this.copyBlock===null) {
      //   return
      // }
      // console.log('copyMove');
      // this.$refs['copy'].style.left = `${event.pageX - this.$refs['copy'].style.offsetWidth / 2}px`;
      // this.$refs['copy'].style.top = `${event.pageY - this.$refs['copy'].style.offsetHeight / 2}px`;
    },
    copyStyle(boxKey, blockKey) {
      if (this.copyBlock.boxKey !== boxKey || !this.copyBlock.blockKey !== blockKey) {
        return {}
      }
      return {
        position: 'absolute',

      }
    }
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

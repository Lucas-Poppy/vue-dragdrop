<template>
  <div id="app">
    <div class="wrapper">
      <div class="box-list">
        <div class="box-parent" 
          v-for="(boxes, boxKey) in list" 
          @drop.prevent="drop(boxKey, $event)" 
          @dragover.self.prevent="dragOver"
          @dragleave.self.prevent="dragLeave"
          :key="boxKey"
          >
          <div class="box-child" 
           v-for="(box, key) in boxes" 
           :key="box"
           :ref="`${boxKey}-${key}`" 
           @dragstart="dragStart(box, key, boxKey, $event)"
           @dragend="dragEnd"
           draggable=true>{{box}}</div>
        </div>
      </div>
    </div>
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
      draggingKey: null,
      draggingValue: null,
      draggingBoxKey: null
    }
  },
  methods:{
    dragStart(box, key, boxKey, ev) {
      this.draggingKey = key;
      this.draggingValue = box;
      this.draggingBoxKey = boxKey;
      // this.$refs[`${boxKey}-${key}`][0].style.display = 'none';
      ev.target.style.opacity = 0.8;
      ev.dataTransfer.dropEffect = "move";
    },
    dragOver(ev) {
      ev.target.style.border = "dashed";
      ev.dataTransfer.dropEffect = "move"
    },
    dragLeave(ev) {
      ev.target.style.border = "none";
      ev.dataTransfer.dropEffect = "move"
    },
    drop(boxKey, ev) {
      ev.target.style.border = "none";
      const array = [...this.list[this.draggingBoxKey]]
      array.splice(this.draggingKey, 1)
      this.list[this.draggingBoxKey] = array;
      this.list[boxKey].push(this.draggingValue);
      this.draggingItem = null,
      this.draggingBox = null;
    },
    dragEnd (ev) {
      ev.target.style.opacity = 1;
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

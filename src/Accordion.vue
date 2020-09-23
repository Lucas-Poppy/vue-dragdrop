<template>
  <div id="app">
    <button class="accordion" @click="onClickAccordion">Section 1</button>
    <div class="panel" ref="panel">
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</p>
    </div>

  </div>
</template>

<script>
export default {
  name: "App",
  data: function () {
    return {isOpen: false}
  },
  methods:{
    onClickAccordion() {
      const panel = this.$refs['panel'];
      if (panel.style.maxHeight) {
        requestAnimationFrame(() => {
          panel.style.maxHeight = null;
        });
      } else {
        requestAnimationFrame(() => {
          panel.style.maxHeight = `${panel.scrollHeight}px`;
        });
      }
    }
  }
};
</script>

<style>
*, *:after, *:before {
  box-sizing: border-box;
}

.wrapper {
  margin: 0 auto;
  width: 980px;
}
.accordion {
  background-color: #eee;
  color: #444;
  cursor: pointer;
  padding: 18px;
  width: 100%;
  border: none;
  text-align: left;
  outline: none;
  font-size: 15px;
  transition: 0.4s;
}

.active, .accordion:hover {
  background-color: #ccc; 
}

.panel {
  padding: 0 18px;
  background-color: white;
  overflow: hidden;
  max-height: 0;
  transition: max-height 0.4s ease-out;
  background-color: aqua;
}
</style>

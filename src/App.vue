<template>
  <div id="app">
    <V2Overflow>
      <template v-for="i in 50">
        <button :key="i">按钮{{ i }}</button>
      </template>
      <template #other="{ startIndex }">
        <select style="width: 100px">
          <option v-for="i in 50 - startIndex" :key="i">
            {{ i + startIndex }}
          </option>
        </select>
      </template>
    </V2Overflow>
  </div>
</template>

<script>
import V2Overflow from "./components/V2Overflow.vue";

export default {
  name: "App",
  components: {
    V2Overflow,
  },
};
</script>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

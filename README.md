# v2-overflow

## Project setup
```
npm install v2-overflow -S
yarn add v2-overflow
```

## Demo
```
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
import V2Overflow from "v2-overflow";

export default {
  name: "App",
  components: {
    V2Overflow,
  },
};
</script>
```
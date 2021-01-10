<template>
  <div class="fifteen">
    <div>{{ myValue }}</div>
    <div class="field">
      <div
        v-for="item in items"
        :key="item"
        class="item"
      >
        {{ item }}
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'App',
  components: {},
  setup() {
    const items = ref(Array.from(Array(15), (item, index) => index + 1));

    return { items };
  },
});
</script>

<style lang="scss" scoped>
.fifteen {
--main-color: rgb(11, 159, 245);
--radius: 5px;

  .field {
    display: grid;
    grid-template-rows: repeat(4, 100px);
    grid-template-columns: repeat(4, 100px);
    grid-gap: 5px 5px;
    border: 10px var(--main-color) solid;
    border-radius: var(--radius);
    padding: 5px;
    box-shadow: 2px 2px 10px var(--main-color);

    .item {
      display: flex;
      align-items: center;
      justify-content: center;
      border: 5px var(--main-color) solid;
      border-radius: var(--radius);
      font-weight: bold;
      font-size: 40px;
      color: var(--main-color);
      box-shadow: 2px 2px 20px var(--main-color) inset;
      transition: 0.2s ease;
      cursor: pointer;

      &:hover {
        background-color: var(--main-color);
        color: #000;
      }
    }
  }
}
</style>

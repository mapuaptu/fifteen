<template>
  <div :class="$style.main">
    Fifteen
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'App',
  components: {},
});
</script>

<style lang="scss" module>
.main {
  margin: 10px;
  padding: 20px;
}
</style>

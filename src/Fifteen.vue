<template>
  <div class="fifteen">
    <div class="controls">
      <div class="name">
        15
      </div>

      <div class="right">
        <div class="block">
          <div class="title">
            Steps
          </div>
          <div class="value">
            {{ step }}
          </div>
        </div>

        <div class="block">
          <div class="title">
            Time
          </div>
          <div class="value">
            {{ formatTimer }}
          </div>
        </div>
      </div>
    </div>

    <div class="field">
      <transition-group>
        <div
          v-for="(item, index) in items"
          :key="item"
          :class="['item', !item && 'item--hided']"
          @click="moveItem(index)"
        >
          {{ item }}
        </div>
      </transition-group>
    </div>
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  ref,
  computed,
  onMounted,
} from 'vue';

// All functions don't mutate parameters, and return new arrays

const shuffle = (arr: number[]): number[] => [...arr].sort(() => Math.random() - 0.5);
const swap = (first: number, last: number, arr: number[]): number[] => {
  const newArray = [...arr];

  const b = newArray[first];

  newArray[first] = newArray[last];

  newArray[last] = b;

  return newArray;
};
const formatTime = (sec: number): string => {
  const minutes = Math.floor(sec / 60);
  const seconds = sec - minutes * 60;

  const minutesString = minutes < 10 ? `0${minutes}` : minutes;
  const secondsString = seconds < 10 ? `0${seconds}` : seconds;

  return `${minutesString}:${secondsString}`;
};
const canMove = (current: number, arr: number[]) => {
  const zeroIndex = arr.indexOf(0);

  if (zeroIndex % 4 === 0) {
    return [zeroIndex - 4, zeroIndex + 1, zeroIndex + 4].includes(current);
  }

  if (zeroIndex % 4 === 3) {
    return [zeroIndex - 1, zeroIndex - 4, zeroIndex + 4].includes(current);
  }

  return [zeroIndex - 1, zeroIndex - 4, zeroIndex + 1, zeroIndex + 4].includes(current);
};

const arraysEqual = (arr1: number[], arr2: number[]) => {
  const newArr1 = [...arr1];
  const newArr2 = [...arr2];

  return newArr1.length === newArr2.length && newArr1.every((el, index) => el === newArr2[index]);
};

export default defineComponent({
  name: 'Fifteen',
  components: {},
  setup() {
    // Initialize

    const initial = [...Array.from(Array(15), (item, index) => index + 1), 0];
    const items = ref(shuffle(initial));
    const shuffleItems = () => {
      items.value = shuffle(items.value);
    };

    // Step
    const step = ref(0);
    const resetStep = () => {
      step.value = 0;
    };

    // Timer

    const timer = ref(0);
    const formatTimer = computed(() => formatTime(timer.value));
    const updateTimer = () => {
      timer.value += 1;
    };
    const resetTimer = () => {
      timer.value = 0;
    };

    let timerInterval: number;
    const startTimer = () => {
      timerInterval = setInterval(updateTimer, 1000);
    };
    const stopTimer = () => {
      clearInterval(timerInterval);
    };

    onMounted(() => startTimer());

    // Move

    const moveItem = (index: number) => {
      if (canMove(index, items.value)) {
        step.value += 1;
        items.value = swap(items.value.indexOf(0), index, items.value);
      }

      if (arraysEqual(initial, items.value)) {
        stopTimer();

        window.alert(
          `You win! Time: ${formatTimer.value}, Steps: ${step.value}. Play again?`,
        );

        resetTimer();
        resetStep();
        shuffleItems();
        startTimer();
      }
    };

    return {
      items,
      shuffleItems,
      moveItem,
      step,
      formatTimer,
      stopTimer,
      resetTimer,
      startTimer,
    };
  },
});
</script>

<style lang="scss" scoped>
.fifteen {
--main-color: rgb(11, 159, 245);
--radius: 5px;

  .controls {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    padding-right: 15px;
    padding-left: 20px;

    .name {
      font-size: 70px;
      color: var(--main-color);
    }

    .right {
      display: flex;
    }

    .block {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-flow: column;
      border-radius: var(--radius);
      width: 100px;
      border: 5px var(--main-color) solid;
      border-radius: var(--radius);
      box-shadow: 2px 2px 20px var(--main-color) inset;

      &:first-child {
        margin-right: 5px;
      }

      .title {
        margin-bottom: 5px;
        font-size: 20px;
        color: var(--main-color);
      }

      .value {
        font-size: 15px;
        color: var(--main-color);
      }
    }
  }

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

      &--hided {
        visibility: hidden;
      }
    }
  }

  .shuffle {
    margin-top: 50px;
    width: 100px;
    height: 100px;
    background-color: var(--main-color);
  }
}
</style>

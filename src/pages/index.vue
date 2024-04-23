<template>
  <v-sheet class="mx-auto" width="300">

    <h1>Handicap Calc</h1>

    <v-form ref="form"><br>
      <p>18H Course Handicap = Handicap Index x (Slope Rating/113) <br><br>
        9H Course Handicap = ((Handicap Index/2) x (9H Slope Rating/113)) + (9H Course Rating – 9H par)</p><br>

      <v-text-field v-model="slopeRating" type="number" label="Slope Rating" required></v-text-field>

      <v-text-field v-model="handicapIndex" type="number" label="Playing Handicap" required></v-text-field>

      <v-switch v-model="checkbox" :label="holesCount" required></v-switch>

      <div class="d-flex flex-column">
        <v-btn class="mt-4" color="success" block @click="calculate">
          Calculate
        </v-btn>

        <v-btn class="mt-4" color="error" block @click="reset">
          Reset
        </v-btn>
      </div>
    </v-form>

    <br>

    <table>
      <tr>
        <td>Course Handicap</td>
        <td>{{ courseHandicap }}</td>
      </tr>
      <tr>
        <td>Playing Handicap</td>
        <td>{{ playingHandicap }}</td>
      </tr>
    </table>
  </v-sheet>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue';
const checkbox = ref<boolean>(true);

const slopeRating = ref<number | null>();
const nineHoleRating = ref<number | null>();
const handicapIndex = ref<number | null>();
const courseHandicap = ref<number | null>();
const playingHandicap = ref<number | null>();

const holesCount = computed(() => {
  return checkbox.value ? '18 holes' : '9 holes';
})

const calculate = () => {
  if (checkbox.value) {
    // 18 holes
    const slopeIndex = (slopeRating.value / 113);
    courseHandicap.value = (handicapIndex.value * slopeIndex).toFixed(2);
    playingHandicap.value = (courseHandicap.value * 0.95).toFixed(2);
  } else {
    // 9 holes
    // courseHandicap.value = ((handicapIndex.value / 2) * (slopeRating.value / 113) + (nineHoleRating.value - nineHolePar.value))
  }
}

const reset = () => {
  slopeRating.value = null;
  nineHoleRating.value = null;
  handicapIndex.value = null;
  courseHandicap.value = null;
  playingHandicap.value = null;

  checkbox.value = true;
}
</script>

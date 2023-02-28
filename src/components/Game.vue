<template>
  <div>
    <button v-if="!isGameStarted" @click="isGameStarted = true">
      Start game
    </button>
    <div v-else>
      <h2>Select field size:</h2>
      <div>
        <button
          v-for="preset in presets"
          :key="preset.name"
          @click="selectField(preset.field)"
        >
          {{ preset.name }}
        </button>
      </div>
      <div v-if="selectedField">
        <h2>Game field:</h2>
        <div class="game-field">
          <div
            v-for="(row, rowIndex) in gameField"
            :key="rowIndex"
            class="game-row"
          >
            <div
              v-for="(cell, cellIndex) in row"
              :key="`${rowIndex}-${cellIndex}`"
              :class="['game-cell', { blue: cell.blue }]"
              @mouseover="cell.blue = !cell.blue"
            ></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      presets: [],
      isGameStarted: false,
      selectedField: null,
      gameField: [],
    };
  },
  mounted() {
    fetch("http://demo7919674.mockable.io")
      .then((response) => response.json())
      .then((data) => (this.presets = data));
  },
  methods: {
    selectField(fieldSize) {
      this.selectedField = fieldSize;
      this.gameField = Array.from({ length: fieldSize }, () =>
        Array.from({ length: fieldSize }, () => ({ blue: false }))
      );
    },
  },
};
</script>

<style scoped>
.game-field {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.game-row {
  display: flex;
}

.game-cell {
  width: 30px;
  height: 30px;
  background-color: white;
  margin: 2px;
  border: 1px solid #6fb5d5;
}

.game-cell.blue {
  background-color: blue;
}
</style>

<template>
  <div id="app">
    <div class="grid">
      <div
        class="row"
        :class="`row-${rIdx}`"
        v-for="(row, rIdx) in grid"
        :key="rIdx"
      >
        <div
          class="column"
          v-for="(col, cIdx) in row"
          :key="'c' + cIdx"
          :class="`column-${cIdx} ${(rIdx + cIdx) % 2 == 0 ? 'even' : 'odd'}`"
          :id="`r${rIdx}-c${cIdx}`"
          @click="clickHandler(`${rIdx}-${cIdx}`)"
        >
          <span class="position">{{ `${rIdx}-${cIdx}` }}</span>
          <div class="">{{ col }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      grid: [],
      knightPosition: "4-4",
      selectMode: false,
    };
  },
  methods: {
    createGrid() {
      for (let index = 0; index < 10; index++) {
        let row = [];
        for (let idx = 0; idx < 10; idx++) {
          row.push(``);
        }
        this.grid.push(row);
      }
    },
    removeKnight() {
      const { knightPosition, grid } = this;
      const r = knightPosition.split("-")[0];
      const c = knightPosition.split("-")[1];
      const g = [...grid];
      g[r][c] = "";
      this.grid = g;
    },
    moveKnight(r, c) {
      this.removeKnight();
      const cp_grid = [...this.grid];
      this.knightPosition = `${r}-${c}`;
      cp_grid[+r][+c] = "K";
      this.grid = cp_grid;
    },
    validMove(move) {
      const row = +move.split("-")[0];
      const col = +move.split("-")[1];

      const c_row = +this.knightPosition.split("-")[0];
      const c_col = +this.knightPosition.split("-")[1];

      // bottom-right
      if (c_col + 2 === col && c_row + 1 === row) return true;
      // bottom-left
      if (c_col + 2 === col && c_row - 1 === row) return true;
      // top-right
      if (c_col - 2 === col && c_row + 1 === row) return true;
      // top-left
      if (c_col - 2 === col && c_row - 1 === row) return true;
      // right-top
      if (c_row + 2 === row && c_col + 1 === col) return true;
      // right-bottom
      if (c_row + 2 === row && c_col - 1 === col) return true;
      // left-top
      if (c_row - 2 === row && c_col + 1 === col) return true;
      // left-bottom
      if (c_row - 2 === row && c_col - 1 === col) return true;

      return false;
    },
    clickHandler(position) {
      const valid = this.validMove(position);

      if (valid) {
        this.moveKnight(position.split("-")[0], position.split("-")[1]);
      } else {
        alert("Invalid Move");
      }
    },

    randomInt(min = 0, max = 9) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },
  },
  mounted() {
    this.createGrid();
    this.moveKnight(this.randomInt(), this.randomInt());
  },
};
</script>

<style>
.grid {
  display: grid;
  grid-template-columns: repeat(10, 4rem);
}

.column {
  height: 4rem;
  display: grid;
  place-items: center;
  background: #fff;
  color: #333;
  position: relative;
  font-size: 1.5rem;
  font-weight: bold;
  cursor: pointer;
}

.column .position {
  position: absolute;
  font-size: 10px;
  top: 2px;
  left: 2px;
}

.column.even {
  background: #333;
  color: #fff;
}
</style>

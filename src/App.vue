<template>
  <div id="app" class="container">
    <form>
      <label class="mr-sm-2" for="input">イベント成績</label>
      <div class="input-group">
        <input
          id="input"
          class="form-control"
          v-model.number="damage"
          type="number"
          min="0"
          step="1"
          aria-describedby="input-help"
          required
        >
        <div class="input-group-append">
          <span class="input-group-text">万</span>
        </div>
      </div>
      <small id="input-help" class="form-text text-muted">月の姫に与えたダメージ総量を入力</small>
    </form>
    <Reward :damage="formatDamage"></Reward>
  </div>
</template>

<script>
import Reward from "@/components/Reward";
export default {
  name: "App",
  components: { Reward },
  data() {
    return {
      damage: null
    };
  },
  computed: {
    formatDamage() {
      return parseInt(this.damage || "0", 10);
    }
  },
  methods: {
    onSubmit() {},
    onReset() {}
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

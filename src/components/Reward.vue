<template>
  <div class="rewards">
    <p class="mt-4">
      Total: {{formatDamage}}万
      <br>
      <span class="text-info">Base: {{baseDamage}}万</span>
      <span class="text-danger ml-2">Over: {{overDamage}}万</span>
    </p>
    <div class="card">
      <h2 class="card-header h5">Rewards</h2>
      <ul class="list-group list-group-flush text-left">
        <li class="list-group-item">勾玉：{{totalJewel}}</li>
        <li class="list-group-item">銭貨：{{totalMoney}}</li>
        <li
          class="list-group-item"
          v-for="([item, num], index) in Object.entries(totalItems)"
          :key="index"
        >{{item}}x{{num}}</li>
      </ul>
    </div>
  </div>
</template>
<script>
const ITEMS = {
  1: "白ダルマ",
  2: "オロチの鱗",
  3: "ランク4青吉鬼",
  4: "青ダルマ",
  5: "オロチの逆鱗",
  6: "かき氷（結界カード）",
  7: "ランク3白ダルマ",
  8: "逢魔の魂",
  9: "ランク6青吉鬼",
  10: "月姫の抱擁（アイコン枠）",
  11: "ランク4白ダルマ",
  12: "神秘の霊符",
  13: "スキン券",
  14: "吉運ダルマ",
  15: "ランク6かき氷（結界カード）",
  16: "御行ダルマ"
};
const REWARDS = {
  1: { type: "money", num: 2000 },
  10: { type: "jewel", num: 10 },
  20: { type: "item", id: 1, num: 1 },
  40: { type: "jewel", num: 20 },
  60: { type: "item", id: 2, num: 30 },
  80: { type: "jewel", num: 30 },
  100: { type: "item", id: 3, num: 3 },
  150: { type: "jewel", num: 40 },
  200: { type: "money", num: 50000 },
  300: { type: "jewel", num: 50 },
  400: { type: "item", id: 4, num: 1 },
  500: { type: "jewel", num: 60 },
  600: { type: "item", id: 5, num: 30 },
  700: { type: "jewel", num: 70 },
  800: { type: "item", id: 6, num: 1 },
  900: { type: "jewel", num: 80 },
  1000: { type: "money", num: 100000 },
  1100: { type: "jewel", num: 90 },
  1200: { type: "item", id: 7, num: 1 },
  1300: { type: "jewel", num: 100 },
  1400: { type: "item", id: 8, num: 30 },
  1500: { type: "jewel", num: 110 },
  1600: { type: "item", id: 9, num: 3 },
  1700: { type: "jewel", num: 120 },
  1800: { type: "item", id: 10, num: 1 },
  1900: { type: "jewel", num: 130 },
  2000: { type: "item", id: 11, num: 1 },
  2100: { type: "jewel", num: 140 },
  2200: { type: "item", id: 12, num: 1 },
  2300: { type: "jewel", num: 150 },
  2400: { type: "item", id: 5, num: 50 },
  2500: { type: "jewel", num: 160 },
  2600: { type: "item", id: 13, num: 30 },
  2700: { type: "jewel", num: 170 },
  2800: { type: "money", num: 300000 },
  2900: { type: "jewel", num: 180 },
  3000: { type: "item", id: 14, num: 2 },
  3100: { type: "jewel", num: 190 },
  3200: { type: "item", id: 11, num: 1 },
  3300: { type: "jewel", num: 200 },
  3400: { type: "item", id: 12, num: 1 },
  3500: { type: "jewel", num: 210 },
  3600: { type: "item", id: 5, num: 50 },
  3800: { type: "jewel", num: 220 },
  4000: { type: "item", id: 13, num: 30 },
  4200: { type: "jewel", num: 230 },
  4400: { type: "item", id: 8, num: 50 },
  4600: { type: "jewel", num: 240 },
  4800: { type: "item", id: 15, num: 1 },
  5000: { type: "item", id: 16, num: 1 }
};
//5000万以降
const REWARDS2 = {
  500: { type: "money", num: 100000 },
  1000: { type: "jewel", num: 50 },
  1500: { type: "item", id: 5, num: 30 },
  2000: { type: "item", id: 12, num: 1 }
};
export default {
  name: "Reward",
  props: {
    damage: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      totalJewel: 0,
      totalMoney: 0,
      totalItems: {}
    };
  },
  computed: {
    formatDamage() {
      if (this.damage < 10000) {
        return this.damage;
      }

      return `${Math.floor(this.damage / 10000)}億${this.damage % 10000}`;
    },
    baseDamage() {
      return this.damage >= 5000 ? 5000 : this.damage;
    },
    overDamage() {
      return this.damage >= 5000 ? this.damage - 5000 : 0;
    }
  },
  watch: {
    damage(val) {
      this.reset();
      this.totalReward();
      this.overReward();
    }
  },
  methods: {
    reset() {
      this.totalJewel = 0;
      this.totalMoney = 0;
      this.totalItems = {};
    },
    totalReward() {
      Object.entries(REWARDS).forEach(([dmg, reward]) => {
        if (dmg <= this.baseDamage) {
          switch (reward.type) {
            case "jewel":
              this.totalJewel += reward.num;
              break;
            case "money":
              this.totalMoney += reward.num;
              break;
            default:
              if (this.totalItems.hasOwnProperty(ITEMS[reward.id])) {
                this.totalItems[ITEMS[reward.id]] += reward.num;
              } else {
                this.totalItems[ITEMS[reward.id]] = reward.num;
              }
          }
        }
      });
    },
    overReward() {
      if (this.overDamage) {
        const monay = Math.floor(this.overDamage / 500);
        this.totalMoney += monay * REWARDS2[500].num;

        const jewel = Math.floor(this.overDamage / 1000);
        this.totalJewel += jewel * REWARDS2[1000].num;

        const item1 = Math.floor(this.overDamage / 1500);
        this.totalItems[ITEMS[REWARDS2[1500].id]] += item1 * REWARDS2[1500].num;

        const item2 = Math.floor(this.overDamage / 2000);
        this.totalItems[ITEMS[REWARDS2[1500].id]] += item1 * REWARDS2[2000].num;
      }
    }
  },
  created() {
    this.reset();
    this.totalReward();
    this.overReward();
  }
};
</script>


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
  1: { type: "money", num: 20000 },
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
//1億以降
const REWARDS3 = {
  1000: { type: "item", id: 5, num: 30 },
  2000: { type: "item", id: 12, num: 1 },
  3000: { type: "money", num: 100000 },
  4000: { type: "jewel", num: 50 }
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
        let current = 0;
        // オーバー5000万まで（1億以内）
        const over1 = this.overDamage <= 5000 ? this.overDamage : 5000;
        // 1億以上
        const over2 = this.overDamage >= 5000 ? this.overDamage - 5000 : 0;

        // 1億までは500刻み
        while (current <= over1) {
          current += 500;
          if (current <= over1) {
            this.totalMoney += REWARDS2[500].num;
          }
          current += 500;
          if (current <= over1) {
            this.totalJewel += REWARDS2[1000].num;
          }
          current += 500;
          if (current <= over1) {
            this.totalItems[ITEMS[REWARDS2[1500].id]] += REWARDS2[1500].num;
          }
          current += 500;
          if (current <= over1) {
            this.totalItems[ITEMS[REWARDS2[2000].id]] += REWARDS2[2000].num;
          }
        }

        if (!over2) return;

        current = 0;

        //1億以上
        while (current <= over2) {
          current += 1000;
          if (current <= over2) {
            this.totalItems[ITEMS[REWARDS3[1000].id]] += REWARDS3[1000].num;
          }
          current += 1000;
          if (current <= over2) {
            this.totalItems[ITEMS[REWARDS3[2000].id]] += REWARDS3[2000].num;
          }
          current += 1000;
          if (current <= over2) {
            this.totalMoney += REWARDS3[3000].num;
          }
          current += 1000;
          if (current <= over2) {
            this.totalJewel += REWARDS3[4000].num;
          }
        }
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


<template>
  <form method="post" class="calculator">
    <fieldset class="input">
      <fieldset class="bill">
        <label for="">Bill</label>
        <input type="number" placeholder="0" v-model="bill" min="0" />
      </fieldset>
      <fieldset class="select-tip">
        <label for="">Select Tip %</label>
        <div class="select-container">
          <input
            v-for="(tip, index) in tips"
            type="button"
            :class="{ active: tip.isActive }"
            :value="tip.value + '%'"
            :key="index"
            @click="
              {
                toggleActive(index);
              }
            "
          />
          <input
            type="number"
            class="custom-tip"
            placeholder="Custom"
            min="0"
            v-model="CustomTip"
          />
        </div>
      </fieldset>
      <fieldset class="people-number">
        <label
          >Number of People
          <p class="warning" v-if="people === 0">Can't be zero</p></label
        >
        <input
          type="number"
          placeholder="0"
          min="1"
          :class="{ warning }"
          v-model="people"
        />
      </fieldset>
    </fieldset>
    <fieldset class="output">
      <div class="results-container">
        <div class="result">
          <label class="title">
            <p>Tip Amount <span>/ person</span></p>
            <input
              type="text"
              :value="`$${amount}`"
              placeholder="$0.00"
              disabled
            />
          </label>
        </div>
        <div class="result">
          <label class="title">
            <p>Total <span>/ person</span></p>
            <input
              type="text"
              :value="`$${total}`"
              placeholder="$0.00"
              disabled
            />
          </label>
        </div>
      </div>
      <input
        type="reset"
        value="Reset"
        @click.prevent="reset"
        :disable="isEmpty"
        :class="{ empty: isEmpty }"
      />
    </fieldset>
  </form>
</template>

<script>
export default {
  data() {
    return {
      tips: [
        { value: 5, isActive: false },
        { value: 10, isActive: false },
        { value: 15, isActive: true },
        { value: 25, isActive: false },
        { value: 50, isActive: false },
      ],
      bill: 142.55,
      StaticTip: 15,
      CustomTip: "",
      people: 5,
      amountParse: "",
      totalParse: "",
      isWarning: null,
      isEmpty: true,
    };
  },
  methods: {
    toggleActive: function (i) {
      this.tips.forEach((tip) => {
        tip.isActive = false;
      });
      this.StaticTip = this.tips[i].value;
      this.tips[i].isActive = !this.tips[i].isActive;
    },
  },
  computed: {
    reset: function () {
      this.bill = null;
      this.StaticTip = 0;
      this.CustomTip = "";
      this.people = null;
      this.amountParse = "";
      this.totalParse = "";
      this.isWarning = null;
      this.isEmpty = !this.isEmpty;
      this.tips.forEach((tip) => {
        tip.isActive = false;
      });
    },
    warning: function () {
      return (this.isWarning = this.people === 0 ? true : false);
    },
    amount: function () {
      this.amountParse =
        this.people != 0 && this.people != null && this.bill != null
          ? this.CustomTip != ""
            ? (this.bill / this.people / 100) * this.CustomTip
            : (this.bill / this.people / 100) * this.StaticTip
          : (this.amountParse = 0);
      return Number.parseFloat(this.amountParse).toFixed(2);
    },
    total: function () {
      if (this.people != 0 && this.people != null && this.bill != null) {
        this.totalParse = this.bill / this.people + this.amountParse;
        this.isEmpty = false;
      } else {
        this.totalParse = 0;
      }
      return Number.parseFloat(this.totalParse).toFixed(2);
    },
  },
};
</script>

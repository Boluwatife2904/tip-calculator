<template>
  <div class="container">
    <div class="logo">
      <img src="./assets/images/logo.svg" alt="Logo" />
    </div>
    <div class="tip-calculator">
      <div class="calculator">
        <form>
          <div class="form-field">
            <label for="bill">Bill</label>
            <input
              type="number"
              name="bill"
              id="bill"
              placeholder="0"
              v-model.number="initialBill"
            />
            <img src="./assets/images/icon-dollar.svg" alt="dollar icon" />
          </div>
          <div class="form-field">
            <p>Select Tip %</p>
            <div class="options">
              <button
                type="button"
                @click="setTipPercentage(5)"
                :class="{ selected: tipPercentage === 5 }"
              >
                5%
              </button>
              <button
                type="button"
                @click="setTipPercentage(10)"
                :class="{ selected: tipPercentage === 10 }"
              >
                10%
              </button>
              <button
                type="button"
                @click="setTipPercentage(15)"
                :class="{ selected: tipPercentage === 15 }"
              >
                15%
              </button>
              <button
                type="button"
                @click="setTipPercentage(25)"
                :class="{ selected: tipPercentage === 25 }"
              >
                25%
              </button>
              <button
                type="button"
                @click="setTipPercentage(50)"
                :class="{ selected: tipPercentage === 50 }"
              >
                50%
              </button>
              <input
                type="number"
                name="tip"
                id="tip"
                v-if="customTip"
                placeholder="0"
                v-model.number="tipPercentage"
              />
              <button
                type="button"
                class="custom"
                @click="customTip = !customTip"
                v-else
              >
                Custom
              </button>
            </div>
          </div>
          <div class="form-field" :class="{ invalid: zeroValue }">
            <label for="people"
              >Number of People
              <small v-if="zeroValue">Can't be zero</small></label
            >
            <input
              type="number"
              name="people"
              id="people"
              placeholder="0"
              v-model.number="numberOfPeople"
            />
            <img src="./assets/images/icon-person.svg" alt="person icon" />
          </div>
        </form>
      </div>
      <div class="result">
        <div class="content">
          <p>
            Tip Amount <br />
            <span>/ person</span>
          </p>
          <h3>${{ tipPerPerson || "0.00" }}</h3>
        </div>
        <div class="content">
          <p>
            Total <br />
            <span>/ person</span>
          </p>
          <h3>${{ totalAmount || "0.00" }}</h3>
        </div>
        <button
          class="reset"
          :disabled="!totalAmount"
          @click="resetCalculation"
        >
          Reset
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, ref, watch } from "vue";
export default {
  name: "App",
  setup() {
    const customTip = ref(false);
    const initialBill = ref(null);
    const tipPercentage = ref(null);
    const numberOfPeople = ref(null);
    const zeroValue = ref(false);
    const setTipPercentage = (tip) => (tipPercentage.value = tip);
    watch(numberOfPeople, (value) => {
      zeroValue.value = value === 0 ? true : false;
    });

    const tipPerPerson = computed(() => {
      if (tipPercentage.value && initialBill.value && numberOfPeople.value) {
        return Number(
          (tipPercentage.value * initialBill.value) / 100 / numberOfPeople.value
        ).toFixed(2);
      }
    });

    const totalAmount = computed(() => {
      if (
        tipPerPerson.value &&
        tipPercentage.value &&
        initialBill.value &&
        numberOfPeople.value
      ) {
        const total =
          Number(tipPerPerson.value) + initialBill.value / numberOfPeople.value;
        return Number(total).toFixed(2);
      }
    });

    const resetCalculation = () => {
      initialBill.value = null;
      numberOfPeople.value = null;
      tipPercentage.value = null;
    };

    return {
      customTip,
      initialBill,
      tipPercentage,
      tipPerPerson,
      numberOfPeople,
      setTipPercentage,
      zeroValue,
      totalAmount,
      resetCalculation,
    };
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400;1,700&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: "Space Mono", sans-serif;
}

#app {
  min-height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  background: hsl(185, 41%, 84%);

  .container {
    max-width: 800px;
    margin: 0 auto;
    width: 100%;

    @media screen and (max-width: 768px) {
      margin-top: 50px;
    }

    .logo {
      margin-bottom: 80px;

      img {
        display: block;
        margin: 0 auto;
      }

      @media screen and (max-width: 768px) {
        margin-bottom: 50px;
      }
    }

    .tip-calculator {
      background: hsl(0, 0%, 100%);
      padding: 30px;
      border-radius: 20px;
      display: flex;
      align-items: center;

      @media screen and (max-width: 768px) {
        flex-direction: column;
      }

      .calculator {
        flex: 0 0 50%;
        max-width: 50%;

        @media screen and (max-width: 768px) {
          flex: 0 0 100%;
          width: 100%;
          max-width: 100%;
        }

        form {
          width: 90%;

          @media screen and (max-width: 768px) {
            width: 100%;
          }

          .form-field {
            margin-bottom: 30px;
            position: relative;

            img {
              position: absolute;
              left: 14px;
              bottom: 14px;
            }

            &:last-child {
              margin-bottom: 10px;
            }

            &.invalid {
              small {
                color: rgba(220, 20, 60, 0.5);
                font-weight: 600;
              }

              input {
                border-color: rgba(220, 20, 60, 0.5);
              }
            }
          }

          p,
          label {
            display: block;
            font-weight: 500;
            font-size: 14px;
            margin-bottom: 10px;
            color: hsl(183, 100%, 15%);
          }

          label {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
          }

          input {
            width: 100%;
            font: inherit;
            text-align: right;
            background: hsl(189, 41%, 97%);
            border-radius: 5px;
            border: 3px solid transparent;
            outline: none;
            padding: 6px 15px 6px 40px;
            font-size: 20px;
            font-weight: 700;
            color: hsl(183, 100%, 15%);
            transition: all 0.3s linear;

            &::placeholder {
              color: hsl(184, 14%, 56%);
            }

            &:focus {
              border: 3px solid hsl(172, 67%, 45%);
            }
          }

          .options {
            // display: flex;
            // justify-content: space-between;
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            grid-gap: 14px;

            button {
              border-radius: 5px;
              background: hsl(183, 100%, 15%);
              color: #fff;
              outline: none;
              border: none;
              font: inherit;
              padding: 8px;
              cursor: pointer;
              height: 40px;
              font-weight: 600;
              transition: all 0.3s linear;

              &.selected {
                background: hsl(172, 67%, 45%);
                color: hsl(183, 100%, 15%);
              }

              &:hover {
                background: hsl(172, 100%, 61%);
                color: hsl(183, 100%, 15%);
              }
            }

            .custom {
              background: hsl(189, 41%, 97%) !important;
              color: hsl(186, 14%, 43%) !important;
              font-weight: 600;
            }

            input {
              height: 40px;
              padding-left: 15px;
            }
          }
        }
      }

      .result {
        flex: 0 0 50%;
        max-width: 50%;
        background: hsl(183, 100%, 15%);
        border-radius: 10px;
        padding: 35px;
        display: flex;
        flex-direction: column;
        min-height: 360px;

        @media screen and (max-width: 768px) {
          flex: 0 0 100%;
          width: 100%;
          max-width: 100%;
          margin-top: 20px;
        }

        .content {
          display: flex;
          justify-content: space-between;
          align-items: center;
          margin-bottom: 20px;

          p {
            color: hsl(0, 0%, 100%);
            font-size: 14px;
            font-weight: 600;

            span {
              font-size: 12px;
              color: hsl(184, 14%, 56%);
            }
          }

          h3 {
            font-size: 38px;
            color: hsl(172, 67%, 45%);
          }
        }

        .reset {
          display: block;
          width: 100%;
          padding: 10px 20px;
          border-radius: 5px;
          font: inherit;
          outline: none;
          border: none;
          cursor: pointer;
          font-weight: 700;
          text-transform: uppercase;
          background: hsl(172, 67%, 45%);
          color: hsl(183, 100%, 15%);
          margin-top: auto;
          transition: all 0.3s linear;

          &:disabled {
            background: rgba(38, 192, 171, 0.2);
            cursor: not-allowed;
          }

          &:hover {
            background: hsl(172, 100%, 61%);
            color: hsl(183, 100%, 15%);
          }
        }
      }
    }
  }
}

/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
</style>

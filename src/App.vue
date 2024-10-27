<script setup lang="ts">
import { ref } from "vue";
import cartImage from "./assets/cart.svg";
import notebookImage from "./assets/notebook.svg";
import personImage from "./assets/person.svg";

const activeStatisticPeriodIndex = ref(0);

const statisticPeriods: {
  name: string;
}[] = [
  {
    name: "Week",
  },
  {
    name: "Month",
  },
  {
    name: "Year",
  },
];

const statisticBlocks: {
  iconImageUrl: string;
  title: string;
  text: string;
  percentage: number;
}[] = [
  {
    iconImageUrl: cartImage,
    title: "Total Sales",
    text: "$513,068.98",
    percentage: 12,
  },
  {
    iconImageUrl: notebookImage,
    title: "Total Orders",
    text: "569,230",
    percentage: 18,
  },
  {
    iconImageUrl: personImage,
    title: "Visitors",
    text: "350,745",
    percentage: -20,
  },
];

function setActiveStatisticPeriodIndex(index: number): void {
  activeStatisticPeriodIndex.value = index;
}
</script>

<template>
  <div class="app-root">
    <aside class="navigation"></aside>
    <div class="container">
      <div class="top">
        <div class="top__text">
          <h1>Keep track of the health of your business</h1>
          <p>Control and analyse your data in the most convenient way</p>
        </div>
        <div class="top__actions">
          <div class="top__action-search">
            <img src="./assets/search.svg" alt="search icon" />
            <input type="text" placeholder="Search" />
          </div>
          <button type="button" class="top__action-notifications">
            <img src="./assets/bell.svg" alt="bell icon" />
          </button>
        </div>
      </div>
      <div class="stats">
        <div class="stats__header">
          <h2>Overview</h2>
          <div class="stats__periods">
            <button
              v-for="(period, index) in statisticPeriods"
              :key="index"
              @click="setActiveStatisticPeriodIndex(index)"
              type="button"
              :class="{
                active: index === activeStatisticPeriodIndex,
              }"
            >
              {{ period.name }}
            </button>
          </div>
        </div>
        <div class="stats__content">
          <div v-for="block in statisticBlocks" class="stats__block">
            <header>
              <img :src="block.iconImageUrl" alt="block icon" />
              <h3>{{ block.title }}</h3>
            </header>
            <p class="stats__block-text">{{ block.text }}</p>
            <footer
              :class="{
                positive: block.percentage > 0,
                negative: block.percentage < 0,
                neutral: block.percentage === 0,
              }"
            >
              <img
                class="positive-arrow"
                src="./assets/green-arrow-up.svg"
                alt="arrow"
              />
              <img
                class="negative-arrow"
                src="./assets/red-arrow-down.svg"
                alt="arrow"
              />
              <p>
                <span
                  >{{ block.percentage > 0 ? "+" : ""
                  }}{{ block.percentage }}%</span
                >
                on this
                {{
                  statisticPeriods[
                    activeStatisticPeriodIndex
                  ]?.name?.toLowerCase()
                }}
              </p>
            </footer>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.app-root {
  display: flex;
}

.navigation {
  height: 100vh;
  width: 92px;
  background-color: var(--foreground-dark-color);
}

.container {
  background-color: var(--background-color);
  width: 100%;
  padding: 60px;

  .top {
    display: flex;
    gap: 30px;
    padding-right: 28px;
    padding-bottom: 36px;

    &__text {
      flex: 1;

      h1 {
        padding-bottom: 30px;
      }
    }

    &__actions {
      display: flex;
      gap: 12px;
    }

    &__action {
      &-notifications {
        width: 54px;
        height: 54px;
        background-color: var(--foreground-dark-color);
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: var(--rounded-md);
        transition: 0.3s all ease-in-out;

        &:hover {
          opacity: 0.9;
          cursor: pointer;
        }
      }

      &-search {
        width: 215px;
        height: 54px;
        border: var(--default-border-style);
        background-color: var(--foreground-white-color);
        border-radius: var(--rounded-md);
        padding: 0 17px;
        display: flex;
        align-items: center;
        gap: 10px;

        input {
          flex: 1;
          font-size: 14px;
          color: var(--text-muted-color);
        }
      }
    }
  }

  .stats {
    &__header {
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    &__periods {
      display: flex;
      gap: 17px;

      button {
        height: 38px;
        text-align: center;
        width: 80px;
        border-radius: var(--rounded-md);
        color: var(--text-muted-color);
        // TODO: add transition style

        &.active {
          background-color: var(--foreground-white-color);
          border: var(--default-border-style);
          color: var(--text-color);
        }
      }
    }

    &__content {
      padding-top: 34px;
      display: flex;
      gap: 25px;
    }

    &__block {
      flex: 1;
      background-color: var(--foreground-white-color);
      border: var(--default-border-style);
      border-radius: var(--rounded-lg);
      padding: 24px;

      header {
        display: flex;
        gap: 16px;
        align-items: center;

        h3 {
          font-weight: 500;
          flex: 1;
          font-size: 16px;
        }
      }

      &-text {
        color: var(--text-color);
        padding-top: 36px;
        padding-block: 44px;
        font-size: 30px;
        font-weight: 600;
      }

      footer {
        display: flex;

        img {
          display: none;
        }

        span {
          font-weight: 600;
          padding: 0 2px;
        }

        &.positive {
          .positive-arrow {
            display: inline-block;
          }

          span {
            color: var(--valid-color);
          }
        }

        &.negative {
          .negative-arrow {
            display: inline-block;
          }

          span {
            color: var(--invalid-color);
          }
        }

        &.neutral {
          span {
            color: var(--text-muted-color);
          }
        }
      }
    }
  }
}
</style>

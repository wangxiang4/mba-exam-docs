<script lang="ts" setup>
  import { computed } from "vue"
  import { useNav } from '../../composables/nav'
  import homeLocale from '../../../i18n/pages/home.json'
  import { useData } from 'vitepress'
  const { lang } = useData(), navs = useNav(), homeLang = computed(() => homeLocale[lang.value])
  // 固定首页 View Detail 标签
  const navData = navs.value.slice(0, 4)
</script>

<template>
  <div class="cards">
    <ul class="container">
      <li v-for="item in navData" :key="item.link">
        <div class="card" v-if="item.hasOwnProperty('home-card-type')">
          <logic-svg v-if="item['home-card-type'] == 'logic' " w="40" m="y-12" />
          <math-svg v-if="item['home-card-type'] == 'math' " w="40" m="y-12" />
          <english-svg v-if="item['home-card-type'] == 'english' " w="40" m="y-12" />
          <write-svg v-if="item['home-card-type'] == 'write' " w="40" m="y-12" />
          <h3>{{ item.text }}</h3>
          <p>{{ item.description }}</p>
          <a :href="item.link">{{ homeLang['card-title'] }}</a>
        </div>
      </li>
    </ul>
  </div>
</template>

<style lang="scss">
.home-page {
  .cards {
    margin: 110px auto;
    max-width: 1150px;

    .container {
      padding: 0;
      margin: 0 auto;

      &::before,
      &::after {
        display: table;
        content: '';
      }

      &::after {
        clear: both;
      }
    }

    li {
      width: 25%;
      padding: 10px 19px;
      box-sizing: border-box;
      float: left;
      list-style: none;
    }

    img {
      width: 160px;
      height: 120px;
    }
  }

  .card {
    height: 500px;
    width: 100%;
    background: var(--bg-color);
    border: 1px solid var(--border-color);
    border-radius: 5px;
    box-sizing: border-box;
    text-align: center;
    position: relative;
    transition: all 0.3s ease-in-out;
    bottom: 0;

    &:hover {
      box-shadow: 0px 12px 32px 4px rgba(237, 239, 245, 0.24),
        0px 8px 20px rgba(237, 239, 245, 0.48);
    }

    img {
      margin: 48px auto;
    }

    h3 {
      margin: 0;
      font-size: 18px;
      color: var(--el-text-color-primary);
      font-weight: normal;
    }

    p {
      font-size: 14px;
      color: #99a9bf;
      padding: 0 25px;
      line-height: 20px;
    }

    a {
      height: 53px;
      line-height: 52px;
      font-size: 14px;
      color: var(--brand-color);
      text-align: center;
      border: 0;
      border-top: 1px solid var(--border-color);
      padding: 0;
      cursor: pointer;
      width: 100%;
      position: absolute;
      bottom: 0;
      left: 0;
      background-color: var(--bg-color);
      border-radius: 0 0 5px 5px;
      transition: all 0.3s;
      text-decoration: none;
      display: block;

      &:hover {
        color: #fff;
        background: var(--brand-color);
      }
    }

    &:hover {
      bottom: 6px;
    }
  }

  @media (max-width: 1140px) {
    .cards {
      width: 100%;

      .container {
        width: 100%;
      }
    }

    .banner .container {
      width: 100%;
      box-sizing: border-box;
    }

    .banner img {
      right: 0;
    }
  }
}

.dark {
  .home-page {
    .card {
      &:hover {
        box-shadow: 0px 12px 32px 4px rgba(9, 11, 16, 0.24),
          0px 8px 20px rgba(9, 11, 16, 0.48);
      }
    }
  }
}
</style>

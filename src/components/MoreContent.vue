<template>
  <div v-if="siteLinks.length" class="more-content">
    <div class="line">
      <Icon size="20">
        <Link />
      </Icon>
      <span class="title">网站列表</span>
    </div>
    <div class="link-wrap">
      <Transition name="fade" mode="out-in">
        <el-row :key="currentPage" class="link-all" :gutter="20">
          <el-col v-for="item in currentLinks" :key="item.name" :span="8">
            <button class="item cards" type="button" @click="jumpLink(item)">
              <Icon size="26">
                <component :is="siteIcon[item.icon] || Link" />
              </Icon>
              <span class="name text-hidden">{{ item.name }}</span>
            </button>
          </el-col>
        </el-row>
      </Transition>
      <div v-if="siteLinksList.length > 1" class="pagination">
        <button
          v-for="(_, index) in siteLinksList"
          :key="index"
          :class="{ active: currentPage === index }"
          type="button"
          :aria-label="`第 ${index + 1} 页`"
          @click="currentPage = index"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { Icon } from "@vicons/utils";
import { Link, Blog, CompactDisc, Cloud, Compass, Book, Fire, LaptopCode } from "@vicons/fa";
import { mainStore } from "@/store";
import siteLinks from "@/assets/siteLinks.json";

const store = mainStore();
const currentPage = ref(0);

// 计算网站链接，每页 6 个
const siteLinksList = computed(() => {
  const result = [];
  for (let i = 0; i < siteLinks.length; i += 6) {
    result.push(siteLinks.slice(i, i + 6));
  }
  return result;
});

const currentLinks = computed(() => siteLinksList.value[currentPage.value] || []);

// 网站链接图标，可前往 https://www.xicons.org 自行挑选并在此处引入
const siteIcon = {
  Blog,
  Cloud,
  CompactDisc,
  Compass,
  Book,
  Fire,
  LaptopCode,
};

// 链接跳转
const jumpLink = (data) => {
  if (data.name === "音乐" && store.musicClick) {
    if (typeof window.$openList === "function") window.$openList();
    return;
  }
  window.open(data.link, "_blank");
};
</script>

<style lang="scss" scoped>
.more-content {
  width: 100%;
  margin-top: 20px;

  .line {
    margin: 0.5rem 0.25rem 1rem;
    font-size: 1.1rem;
    display: flex;
    align-items: center;
    animation: fade 0.5s;

    .title {
      margin-left: 8px;
      font-size: 1.15rem;
      text-shadow: 0 0 5px #00000050;
    }
  }

  .link-wrap {
    width: calc(100% + 20px);
    margin-left: -10px;
    padding: 5px 10px 0;
  }

  .link-all {
    min-height: 220px;

    .el-col {
      margin-bottom: 20px;
    }

    .item {
      width: 100%;
      height: 100px;
      border: 0;
      color: inherit;
      display: flex;
      align-items: center;
      flex-direction: row;
      justify-content: center;
      padding: 0 10px;
      animation: fade 0.5s;

      &:hover {
        transform: scale(1.02);
        background: rgb(0 0 0 / 40%);
        transition: 0.3s;
      }

      &:active {
        transform: scale(1);
      }

      .name {
        font-size: 1.1rem;
        margin-left: 8px;
      }
    }
  }

  .pagination {
    display: flex;
    justify-content: center;
    gap: 8px;
    height: 12px;
    margin-top: -8px;

    button {
      width: 18px;
      height: 4px;
      padding: 0;
      border: 0;
      border-radius: 4px;
      background-color: #fff;
      opacity: 0.2;
      transition: opacity 0.3s;

      &.active,
      &:hover {
        opacity: 1;
      }
    }
  }

  @media (min-width: 720px) and (max-width: 820px) {
    .link-all .item .name {
      display: none;
    }
  }

  @media (max-width: 720px) {
    .link-all {
      min-height: 180px;

      .item {
        height: 80px;
      }
    }
  }

  @media (max-width: 460px) {
    .link-all .item {
      flex-direction: column;

      .name {
        font-size: 1rem;
        margin-left: 0;
        margin-top: 8px;
      }
    }
  }
}
</style>

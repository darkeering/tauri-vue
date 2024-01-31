<script setup lang="ts">
import { getAssetsFile } from "@/util";
import { nextTick, onMounted, onUnmounted, ref } from "vue";
const tabs: any = ref(null);
const toogleButton: any = ref(null);
const toogleButton4: any = ref([]);
const dropDown: any = ref(null);
const dropDown4: any = ref(null);
const pos = ref({
  x: 0,
  y: 0,
});
const pos4 = ref({
  x: 0,
  y: 0,
});
const showTabs = ref([
  {
    id: 1,
  },
  {
    id: 2,
  },
  {
    id: 3,
  },
  {
    id: 4,
  },
  {
    id: 5,
  },
  {
    id: 6,
  },
]);
const moreTabs: any = ref([]);
const show = ref(false);
const show1 = ref(false);
onMounted(() => {
  fn();
  setTimeout(() => {
    fn2();
  });
});
async function toogle() {
  show.value = !show.value;
  if (!show.value) return;
  await nextTick();
  const info = toogleButton.value.getBoundingClientRect();
  currentClick.value = toogleButton.value
  const _info = dropDown.value.getBoundingClientRect();
  if (info.x + _info.width > window.innerWidth) {
    pos.value.x =
      info.x -
      (info.x + _info.width - window.innerWidth) -
      (window.innerWidth - info.x - info.width);
    pos.value.y = info.y + info.height;
  } else {
    pos.value.x = info.x;
    pos.value.y = info.y + info.height;
  }
}

function fn() {
  const parentNodeInfo = tabs.value.parentNode.getBoundingClientRect();
  const lastInfo =
    tabs.value.childNodes[
      tabs.value.childNodes.length - 1
    ].getBoundingClientRect();
  if (lastInfo.right > parentNodeInfo.right) {
    const count = Math.ceil((lastInfo.right - parentNodeInfo.width) / 210);
    for (let i = 0; i < count; i++) {
      if (showTabs.value.length > 1) {
        moreTabs.value.unshift(showTabs.value.pop());
      }
    }
  } else if (lastInfo.right + 210 < parentNodeInfo.right) {
    const count = Math.ceil((parentNodeInfo.width - lastInfo.right) / 210);
    for (let i = 0; i < count; i++) {
      if (moreTabs.value.length > 0) {
        showTabs.value.push(moreTabs.value.shift());
      }
    }
  }

  if (show.value) {
    const info = toogleButton.value.getBoundingClientRect();
    const _info = dropDown.value.getBoundingClientRect();
    if (info.x + _info.width > window.innerWidth) {
      pos.value.x =
        info.x -
        (info.x + _info.width - window.innerWidth) -
        (window.innerWidth - info.x - info.width);
      pos.value.y = info.y + info.height;
    } else {
      pos.value.x = info.x;
      pos.value.y = info.y + info.height;
    }
  }
}
window.addEventListener("resize", fn);
onUnmounted(() => {
  document.removeEventListener("click", fn4);
  window.removeEventListener("resize", fn);
  window.removeEventListener("resize", fn2);
});

const accounts = [
  {
    id: 1,
    name: "darkeering",
    date: "2024-01-31",
    time: "22:54:30",
  },
  {
    id: 2,
    name: "darkeering",
    date: "2024-01-31",
    time: "22:54:30",
  },
];
const container: any = ref(null);
const items: any = ref([]);

function fn2() {
  const totalWidth = container.value.clientWidth;
  const baseWidth = items.value[0].clientWidth + 20;
  const baseHeight = items.value[0].clientHeight + 20;
  const num = Math.floor(totalWidth / baseWidth);
  items.value.forEach((item: any, index: number) => {
    const x = Math.floor(index / num);
    const y = index % num;
    if (
      `${y * baseWidth}px` !== item.style.left ||
      item.style.top !== `${x * baseHeight + 20}px`
    ) {
      item.style.transform = "rotate(30deg)";
      setTimeout(() => {
        item.style.transform = "unset";
      }, 500);
    }
    item.style.left = `${y * baseWidth}px`;
    item.style.top = `${x * baseHeight + 20}px`;
  });
}
window.addEventListener("resize", fn2);
const currentClick: any = ref(null)
async function toogle4(i: number) {
  show1.value = !show1.value;
  if (!show1.value) return;
  await nextTick();
  const info = toogleButton4.value[i].getBoundingClientRect();
  currentClick.value = toogleButton4.value[i]
  const _info = dropDown4.value.getBoundingClientRect();
  if (info.x + _info.width > window.innerWidth) {
    pos.value.x =
      info.x -
      (info.x + _info.width - window.innerWidth) -
      (window.innerWidth - info.x - info.width);
    pos.value.y = info.y + info.height;
  } else {
    pos.value.x = info.x;
    pos.value.y = info.y + info.height;
  }
}
function fn4(e: any) {
  if (currentClick.value && !currentClick.value.contains(e.target)) {
    show1.value = false;
  }
}

document.addEventListener("click", fn4);
</script>

<template>
  <ul class="flex align-center" ref="tabs">
    <li class="tab" v-for="tab in showTabs" :key="tab.id">
      <img class="icon" :src="getAssetsFile('steam')" alt="" />
      <span class="label">Steam {{ tab.id }}</span>
      <img class="delete" :src="getAssetsFile('delete')" alt="" />
    </li>
    <li
      class="add flex align-center"
      ref="toogleButton"
      @click="() => toogle()"
    >
      <img :src="getAssetsFile('more')" alt="" />
    </li>
    <li class="add flex align-center">
      <img :src="getAssetsFile('plus')" alt="" />
    </li>
  </ul>

  <ul class="accounts" ref="container">
    <li
      class="account-container"
      ref="items"
      v-for="(account, index) in accounts"
      :key="account.id"
    >
      <div class="account flex flex-column">
        <img class="avatar" :src="getAssetsFile('avatar', 'jpg')" alt="" />
        <span class="name">{{ account.name }}</span>
        <span class="date">{{ account.date }}</span>
        <span class="time">{{ account.time }}</span>

        <div class="foot flex align-center">
          <span class="pre">最近登录</span>
          <span class="more flex align-center" ref="toogleButton4" @click="() => toogle4(index)">
            <img :src="getAssetsFile('more')" alt="" />
          </span>
        </div>
      </div>
    </li>
  </ul>

  <Teleport to="body">
    <ul
      v-if="show"
      ref="dropDown"
      class="drop-down"
      :style="{
        top: pos.y ? pos.y + 'px' : 'unset',
        left: pos.x ? pos.x + 'px' : 'unset',
      }"
    >
      <li v-for="tab in moreTabs" :key="tab.id" class="drop-down-item">
        Steam {{ tab.id }}
      </li>
    </ul>
  </Teleport>
  <Teleport to="body">
    <ul
      v-if="show1"
      ref="dropDown4"
      class="drop-down"
      :style="{
        top: pos4.y ? pos4.y + 'px' : 'unset',
        left: pos4.x ? pos4.x + 'px' : 'unset',
      }"
    >
      ddd
    </ul>
  </Teleport>
</template>

<style scoped>
.tab {
  display: flex;
  align-items: center;
  width: 200px;
  background-color: #2c3439;
  padding: 6px 10px;
  border-radius: 10px 10px 0 0;
  cursor: pointer;
  margin-right: 10px;
  flex-shrink: 0;

  &:hover {
    background-color: #343c3f;
  }

  .icon {
    width: 20px;
  }

  .label {
    color: #fff;
    margin-left: 6px;
  }

  .delete {
    width: 20px;
    margin-left: auto;
  }
}

.add {
  margin-right: 10px;
  cursor: pointer;
  padding: 4px;
  border-radius: 4px;

  img {
    width: 20px;
  }

  &:hover {
    background-color: #343c3f;
  }
}

.drop-down {
  margin-top: 8px;
  position: fixed;
  z-index: 1;
  background-color: #2f3539;
  border-radius: 6px;
  transition: left 0.5s;

  .drop-down-item {
    text-wrap: nowrap;
    color: #fff;
    padding: 4px 8px;
    border-radius: 6px;
    cursor: pointer;
    min-width: 120px;

    &:hover {
      background-color: #404b53;
    }
  }
}

.accounts {
  margin-top: 12px;
  position: relative;
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  margin: 0 auto;
  .account-container {
    position: absolute;
    transition: all 0.5s ease;
    will-change: transform;
    display: inline-flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border-radius: 6px;
    cursor: pointer;
    color: #fff;
    padding: 12px;
    background-color: #303538;
    .account {
      .avatar {
        width: 105px;
        height: 105px;
        border-radius: 4px;
        align-self: center;
      }
      .name {
        margin: 10px 0;
      }
      .date {
        color: #ccd2d3;
        font-size: 12px;
      }
      .time {
        color: #ccd2d3;
        font-size: 12px;
      }
      .foot {
        font-size: 12px;
        justify-content: space-between;
        margin-top: 10px;
        .pre {
          padding: 4px 6px;
          background-color: #3b4144;
          border-radius: 4px;
          font-weight: 500;
        }
        .more {
          padding: 6px;
          img {
            width: 20px;
            height: 20px;
          }
          &:hover {
            background-color: #3b4144;
            border-radius: 4px;
          }
        }
      }
    }
  }
}
</style>

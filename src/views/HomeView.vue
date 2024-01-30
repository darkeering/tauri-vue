<script setup lang="ts">
import { computed, nextTick, onMounted, onUnmounted, ref } from 'vue'
import Alert from '../components/alert.vue'
import Card from '../components/card.vue'
import { getAssetsFile } from '../util'

const news = [
  {
    title: '2023年国游销量年榜正式发布',
    auth: '国游销量榜',
    desc: '2023年国产游戏销量榜单',
    left: '2024-01-22',
    src: getAssetsFile('河边露营')
  },
  {
    title: '喵喵汪汪大胜利！《萌宠拆拆乐》来袭！',
    auth: '萌宠拆拆乐制作组',
    desc: '喵喵汪汪大胜利！超解压萌宠拆家派对游戏《萌宠拆拆乐》来袭！',
    left: '2024-01-21',
    src: getAssetsFile('宫崎骏风格')
  },
  {
    title: '帕斯亚活动中央公示及领取方法',
    auth: '福利官',
    desc: '中奖的伙伴们要及时领取<br />超过公示时间就没办法领取了',
    left: '2024-01-19',
    src: getAssetsFile('多人办公')
  }
]

const chooses = [
  {
    title: '幻兽帕鲁/（国区礼物不是激活码）',
    auth: '多人｜冒险｜休闲',
    desc: '',
    left: '<del>￥68.00</del>￥9.90',
    right: '-87%',
    src: getAssetsFile('河边露营')
  },
  {
    title: '雷神加速器',
    auth: '多人｜冒险｜休闲',
    desc: '',
    left: '<del>￥68.00</del>￥9.90',
    right: '-87%',
    src: getAssetsFile('酷炫快递员')
  },
  {
    title: '猛兽派对',
    auth: '多人｜冒险｜休闲',
    desc: '',
    left: '<del>￥68.00</del>￥9.90',
    right: '-87%',
    src: getAssetsFile('节日')
  },
  {
    title: '绝地求生',
    auth: '多人｜冒险｜休闲',
    desc: '',
    left: '<del>￥68.00</del>￥9.90',
    right: '-87%',
    src: getAssetsFile('恐龙陪读')
  },
  {
    title: '战神4',
    auth: '多人｜冒险｜休闲',
    desc: '',
    left: '<del>￥68.00</del>￥9.90',
    right: '-87%',
    src: getAssetsFile('思考')
  },
  {
    title: '战神4',
    auth: '多人｜冒险｜休闲',
    desc: '',
    left: '<del>￥68.00</del>￥9.90',
    right: '-87%',
    src: getAssetsFile('思考')
  },
  {
    title: '怪物猎人',
    auth: '多人｜冒险｜休闲',
    desc: '',
    left: '<del>￥68.00</del>￥9.90',
    right: '-87%',
    src: getAssetsFile('思考')
  }
]
const menus = [
  {
    title: '起始页',
    src: 'home'
  },
  {
    title: '网络加速',
    src: 'Thunder'
  },
  {
    title: '帐号切换',
    src: 'icon-crowd'
  },
  {
    title: '库存游戏',
    src: 'game'
  },
  {
    title: '本地令牌',
    src: 'MicrosoftAuthenticator'
  },
  {
    title: 'Steam挂卡',
    src: 'cards'
  },
  {
    title: '游戏工具',
    src: 'gongju'
  }
]
let index = ref(0)
const start = computed(() => index.value * 4)
const end = computed(() => {
  return (index.value + 1) * 4 > chooses.length ? chooses.length : (index.value + 1) * 4
})
function next() {
  if (end.value >= chooses.length) {
    index.value = 0
    return
  }
  index.value++
}
function previous() {
  if (start.value <= 0) {
    index.value = Math.ceil(chooses.length / 4) - 1
    return
  }
  index.value--
}
const container: any = ref(null)
const items: any = ref([])

onMounted(() => {
  setTimeout(() => {
    fn()
  })
})
function fn() {
  const totalWidth = container.value.clientWidth
  const baseWidth = items.value[0].clientWidth + 20
  const baseHeight = items.value[0].clientHeight + 20
  const num = Math.floor(totalWidth / baseWidth)
  items.value.forEach((item: any, index: number) => {
    const x = Math.floor(index / num)
    const y = index % num
    item.style.left = `${y * baseWidth}px`
    item.style.top = `${x * baseHeight}px`
  })
}
window.addEventListener('resize', fn)
onUnmounted(() => {
  window.removeEventListener('resize', fn)
})
</script>

<template>
  <div class="home">
    <div class="home-container">
      <Alert />
      <div class="title">
        <h1>欢迎使用 Dar Toolkit</h1>
        <button>查看更多</button>
      </div>
      <div class="welcome">
        <div class="img">
          <img :src="getAssetsFile('呼啸幽灵')" />
        </div>
        <div>
          <p>发现问题请在Github上提交</p>
          <p style="color: #fff; font-size: 24px">3.0开始测试啦！</p>
          <button>阅读更多</button>
        </div>
      </div>
      <div class="title">
        <h1>最新消息</h1>
        <button>查看更多</button>
      </div>
      <div class="new">
        <Card class="card" v-for="card in news" :key="card.title" :card="card"></Card>
      </div>
      <div class="title">
        <h1>精选</h1>
        <button>访问DarGame商城</button>
      </div>
      <div class="lunbo">
        <i class="before" @click="() => previous()">&lt;</i>
        <div :style="{
          transform: `translateX(${-100 * index}%)`,
          transition: 'all 0.75s ease-in-out'
        }">
          <Card class="game" v-for="choose in chooses" :key="choose.title" :card="choose"></Card>
        </div>
        <i class="after" @click="() => next()">></i>
      </div>
      <div class="title">
        <h1>已安装功能</h1>
      </div>
      <div>
        <ul ref="container">
          <li ref="items" v-for="menu in menus" :key="menu.title">
            <img :src="getAssetsFile(menu.src)" alt="" />
            <span> {{ menu.title }} </span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
.home {
  display: flex;
  justify-content: center;
  width: 100%;
}

.home-container {
  width: 90%;
  max-width: 1240px;
  margin-top: 32px;

  h2 {
    color: #fff;
    margin-top: 10px;
  }

  h1 {
    color: #fff;
  }
}

.welcome {
  display: flex;
  background-color: #353b3e;
  height: 360px;
  margin-top: 16px;
  border-radius: 6px;
  overflow: hidden;

  &:hover {
    img {
      transform: scale(1.2);
    }
  }

  .img {
    width: 50%;
    min-width: 620px;
    overflow: hidden;
    height: 100%;
    margin: 0;

    img {
      width: 100%;
      transition: all 0.25s;
    }
  }

  div {
    align-self: center;
    margin-left: 12px;
  }

  button {
    color: #fff;
    background-color: #0078d4;
    display: flex;
    align-items: center;
    justify-items: center;
    border-style: none;
    padding: 10px 20px;
    border-radius: 4px;
    font-size: 16px;
  }
}

.title {
  margin-top: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;

  button {
    color: #91e2f9;
    background-color: transparent;
    border-style: none;
    padding: 6px 8px;
    border-radius: 4px;
    cursor: pointer;

    &:hover {
      background-color: #2f3539;
    }
  }
}

.new {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;

  .card {
    width: 32.5%;
  }
}

.lunbo {
  position: relative;
  overflow: hidden;
  white-space: nowrap;

  .before {
    position: absolute;
    z-index: 1;
    font-size: 24px;
    line-height: 36px;
    height: 36px;
    width: 36px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    background-color: #fff;
    color: #000;
    opacity: 0.5;
    border-radius: 50%;
    top: 50%;
    transform: translateY(-50%);
    cursor: pointer;
  }

  .after {
    position: absolute;
    z-index: 1;
    font-size: 24px;
    line-height: 36px;
    height: 36px;
    width: 36px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    background-color: #fff;
    color: #000;
    opacity: 0.5;
    border-radius: 50%;
    top: 50%;
    right: 0;
    transform: translateY(-50%);
    cursor: pointer;
  }

  .game {
    display: inline-block;
    width: 23.5%;
    height: 300px;
    margin-right: 2%;

    &:nth-child(4n) {
      margin-right: 0;
    }
  }
}

ul {
  margin-top: 12px;
  position: relative;
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  margin: 0 auto;

  li {
    position: absolute;
    transition: all 0.3s;
    will-change: transform;
    display: inline-flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border-radius: 4px;
    cursor: pointer;
    color: #fff;
    padding: 20px;
    background-color: #303538;

    img {
      width: 75px;
    }
  }

  li:hover {
    background-color: #353b3f;
  }
}</style>

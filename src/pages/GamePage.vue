<template>
  <div id="gamePage">
        <audio controls ref="audio" class="aud">
            <source src="https://article.zzccs.cc/zhtttu/1.mp3" />
      </audio>
  
    <!-- 胜利 -->
    <a-row align="center">
      <div v-if="gameStatus === 3" style="text-align: center">
        <h2>恭喜你加入羊群</h2>
      </div>
    </a-row>
    <!-- 分层选块 -->
    <a-row align="center">
      <div v-show="gameStatus > 0" class="level-board">
        <div v-for="(block, idx) in levelBlocksVal" :key="idx">
          <div
            v-if="block.status === 0"
            class="block level-block"
            :class="{
              disabled: !isHolyLight && block.lowerThanBlocks.length > 0,
            }"
            :data-id="block.id"
            :style="{
              zIndex: 100 + block.level,
              left: block.x * widthUnit + 'px',
              top: block.y * heightUnit + 'px',
            }"
            @click="() => doClickBlock(block)"
          >
          {{ block.type }}
          </div>
        </div>
      </div>
    </a-row>
    <!-- 随机选块 -->
    <a-row align="space-between" class="random-board">
      <div
        v-for="(randomBlock, index) in randomBlocksVal"
        :key="index"
        class="random-area"
      >
        <div
          v-if="randomBlock.length > 0"
          :data-id="randomBlock[0].id"
          class="block"
          @click="() => doClickBlock(randomBlock[0], index)"
        >
          {{ randomBlock[0].type }}
        </div>
        <!-- 隐藏 -->
        <div
          v-for="num in Math.max(randomBlock.length - 1, 0)"
          :key="num"
          class="block disabled"
        >
          <span v-if="canSeeRandom">
            {{ randomBlock[num].type }}
          </span>
        </div>
      </div>
    </a-row>
    <!-- 槽位 -->
    <a-row v-if="slotAreaVal.length > 0" align="center" class="slot-board">
      <div v-for="(slotBlock, index) in slotAreaVal" :key="index" class="block">
        {{ slotBlock?.type }}
      </div>
    </a-row>
    <!-- 技能 -->
    <div class="skill-board">
      <a-space class="aclass1">
        <!-- <a-button size="small" @click="doRevert">撤回</a-button> -->
        <a-button class="aclass2" size="small" @click="doRemove">移出道具</a-button>
        <a-button class="aclass2" size="small" @click="doShuffle">洗牌道具</a-button>
        <!-- <a-button size="small" @click="doBroke">破坏</a-button> -->
        <!-- <a-button size="small" @click="doHolyLight">圣光</a-button>
        <a-button size="small" @click="doSeeRandom">透视</a-button> -->
      </a-space>
    </div>
  </div>
</template>

<script setup lang="ts">
import useGame from "../core/game";
import { onMounted } from "vue";
import { useRouter } from "vue-router";
import MyAd from "../components/MyAd.vue";

const router = useRouter();

const {
  gameStatus,
  levelBlocksVal,
  randomBlocksVal,
  slotAreaVal,
  widthUnit,
  heightUnit,
  totalBlockNum,
  clearBlockNum,
  isHolyLight,
  canSeeRandom,
  doClickBlock,
  doStart,
  doShuffle,
  doBroke,
  doRemove,
  doRevert,
  doHolyLight,
  doSeeRandom,
} = useGame();

/**
 * 回上一页
 */
const doBack = () => {
  router.back();
};

onMounted(() => {
  doStart();
});
</script>

<style scoped>
.level-board {
  position: relative;
}

.level-block {
  position: absolute;
}

.random-board {
  margin-top: 8px;
}

.random-area {
  margin-top: 8px;
}

.slot-board {
  border: 10px solid saddlebrown;
  margin: 16px auto;
  width: fit-content;
}

.skill-board {
  text-align: center;
}

.block {
  font-size: 35px;
    width: 50px;
    height: 63px;
    line-height: 60px;
    border: 1px solid #000;
    background: #e5edb6;
    text-align: center;
    vertical-align: top;
    display: inline-block;
    border-bottom: solid 5px #719a42;
    border-radius: 10px;
}

.disabled {
  background: #b4aaaa;
  cursor: not-allowed;
}
.aclass1{
  width: 100%;
}
.aclass1 .ant-space-item{
  width: 49%;
}
.aclass1 button{
    height: 63px;
    border: 1px solid #000;
    border-bottom: solid 5px #719a42;
    border-radius: 10px;
}
</style>

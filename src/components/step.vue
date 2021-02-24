<template>
  <div class="stepAll">
    <p class="title" v-for="(item,index) in titles" :key="index" v-show="index === currentIndex">{{item}}</p>
    <div class="stepIcon">
      <span class="item">
        <span class="el-icon-s-custom size" :class="{isShow: array[0]}"></span>
        <span class="line" :class="{isShow: array[0]}"></span>
      </span>
      <span class="item">
        <span class="el-icon-s-home size" :class="{isShow: array[1]}"></span>
        <span class="line" :class="{isShow: array[1]}"></span>
      </span>
      <span class="item">
        <span class="el-icon-s-promotion size" :class="{isShow: array[2]}"></span>
        <span class="line" :class="{isShow: array[2]}"></span>
      </span>
    </div>
    <div class="stepMiddle" v-if="currentIndex <=1">
      <div class="item">
        <span class="Item">{{ oldItem[currentIndex] }}</span>
        <input type="text" :placeholder="oldItem[currentIndex]" class="input">
      </div>
      <div class="item">
        <span class="Item">{{ newItem[currentIndex] }}</span>
        <input type="text" :placeholder="newItem[currentIndex]" class="input1 input" :class="{isInput: currentIndex >=1 }">
        <span class="buttonKey" v-if="currentIndex <= 0">获取验证码</span>
      </div>
    </div>
    <div class="stepNext" @click="nextStep()" v-if="currentIndex <= 1">下一步</div>
  </div>
</template>

<script>
export default {
  name: "step",
  data() {
    return {
      style: ["el-icon-s-custom", "el-icon-s-home", "el-icon-s-promotion"],
      titles: ["身份验证", "密码重置", "重置成功"],
      currentIndex: 0,
      oldItem: ["手机号", "新密码"],
      newItem: ["手机验证码", "重新输入密码"],
      array: [true, false, false],
    }
  },
  methods: {
    nextStep() {
      //这里加上一些对手机号验证，验证码是否正确，密码是否正确进行判断，再进行下一步
      this.currentIndex ++;
      this.array[this.currentIndex] = true;
    }
  }
}
</script>

<style scoped>
.isShow {
  background-color: #194D9F!important;
}

.isInput {
  width: 35vw!important;
}

.stepAll {
  width: 70vw;
  height: 70vh;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}

.stepAll .title {
  font-size: 2rem;
}

.stepAll  .stepIcon .item {
  display: inline-block;
  width: 20vw;
  height: 15vh;
  line-height: 15vh;
}

.stepAll  .stepIcon .item .size {
  height: 50px;
  width: 50px;
  line-height: 50px;
  border-radius: 50%;
  background-color: #2c3e50;
  color: azure;
}

.stepAll .stepIcon .item .line {
  display: inline-block;
  width: 12vw;
  height: 2px;
  background-color: rgba(0,0,0,0.1);
  margin-left: 20px;
  margin-bottom: 5px;
}

.stepAll .stepMiddle .item {
  width: 70vw;
  height: 8vh;
}

.stepAll .stepMiddle .item .Item {
  display: inline-block;
  width: 8vw;
  text-align: right;
  margin-right: 3rem;
  font-size: 1.2rem;
  font-weight: bolder;
}

.stepAll .stepMiddle .item .input {
  height: 40px;
  width: 35vw;
  border-radius: 5px;
  border: 0px solid black;
  background-color: rgba(0,0,0,0.1);
  padding-left: 1rem;
}

.stepAll .stepMiddle .item .input1 {
  width:18vw;
}

.buttonKey {
  display: inline-block;
  height: 40px;
  width: 15vw;
  line-height: 40px;
  background-color: #D3DEEF;
  margin-left: 2vw;
  border-radius: 5px;
  cursor: pointer;
}

.stepAll .stepNext {
  width: 20vw;
  height: 40px;
  line-height: 40px;
  background-color: #1D4C9F;
  border-radius: 15px;
  color: #D3DEEF;
}


</style>
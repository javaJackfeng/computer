<template>
  <div id="textComp">
    <div class="input">
      <input type="text" v-model="inputValue" />
      <div class="iconfont" :class="inputValue!=''?'icon-cancel':''" @click="clearInputValue"></div>
    </div>
    <div class="result">
      <div class="resultText">{{inputValue}} {{processValue}}</div>
      <div class="resultRes">{{tempValue==0?'':tempValue}}</div>
    </div>
    <div class="keyboard">
      <div
        class="keyboardItem"
        v-for="(item,index) in keyboard"
        :key="index"
        @click="inputNum(item)"
      >{{item}}</div>
      <div class="compute" @click="compute($event)">确定</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      keyboard: [1, 2, 3, "+", 4, 5, 6, "×", 7, 8, 9, "", ".", 0, "删除", ""],
      inputValue: "",
      result: 0,
      tempValue: 0,
      processValue: ""
    };
  },
  methods: {
    clearInputValue() {
      this.inputValue = "";
    },
    compute(e) {
      this.tempValue = this.processValue.replace(/×/g, "*");
      this.tempValue = eval(this.tempValue);
    },
    inputNum(v) {
      //同一个数字下面不能有多个小数点

      //最后一位数字
      let lastNum = this.processValue.substr(
        this.processValue.length - 1,
        this.processValue.length
      );
      //判断倒数第2位是否包含小数点
      let lastThirdNum = this.processValue.substr(
        this.processValue.length - 3,
        1
      );
      if (lastThirdNum != "." || this.processValue.length < 3) {
        if (/[0-9]/.test(v)) {
          this.processValue += v;
        }
        //前一个数字是否包含点
        let lastSecondNum = this.processValue.substr(
          this.processValue.length - 2,
          1
        );
        if (
          v === "." &&
          lastNum != "×" &&
          lastNum != "+" &&
          lastNum != "." &&
          lastSecondNum != "."
        ) {
          this.processValue += v;
        }
      }

      if (v === "删除") {
        this.processValue = this.processValue.substr(
          0,
          this.processValue.length - 1
        );
      }
      if (this.processValue.length) {
        if (v === "+" && lastNum != "+" && lastNum != "×" && lastNum != ".") {
          this.processValue += v;
        }
        if (v === "×" && lastNum != "×" && lastNum != "+" && lastNum != ".") {
          this.processValue += v;
        }
      }
    }
  }
};
</script>


<style scoped>
#textComp {
  padding: 0 8px;
  width: 100%;
  height: 100%;
  overflow: hidden;
}
.input {
  width: 100%;
  padding-bottom: 5px;
  border-bottom: 1px solid #4ca49f;
}
.input input {
  padding-left: 10px;
  border: none;
  outline: none;
  height: 30px;
  width: 80%;
  font-size: 16px;
  line-height: 30px;
  color: #159383;
  /* font-weight: 700 */
}
.result {
  position: relative;
  margin: 10px 0;
  height: 80px;
  width: 100%;
  border: 1px solid #4ca49f;
  border-radius: 10px;
}
.result .resultText {
  position: absolute;
  left: 5px;
  top: 5px;
  color: #159383;
  font-weight: 700;
  /* margin: 10px 0; */
  /* border: 1px solid #4ca49f; */
}
.result .resultRes {
  position: absolute;
  right: 5px;
  bottom: 5px;
  /* margin: 10px 0; */
  /* border: 1px solid #4ca49f; */
}
.keyboard {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  width: 100%;
}

.keyboardItem {
  width: 24.3%;
  height: 50px;
  background-color: #f7f8fa;
  margin-right: 2px;
  margin-bottom: 2px;
  color: #333336;
  text-align: center;
  line-height: 50px;
}

.compute {
  position: absolute;
  width: 73px;
  height: 100px;
  right: 2px;
  bottom: 2px;
  background-color: #169a89;
  text-align: center;
  line-height: 100px;
  font-size: 26px;
  font-weight: 700;
  color: #fff;
}
.icon-cancel {
  position: absolute;
  width: 50px;
  height: 50px;
  right: 39px;
  top: 1px;
  font-size: 30px;
}
</style>

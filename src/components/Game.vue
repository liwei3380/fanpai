<template>
  <div class="game">
    <div class="noteboard clearfix">
      <div class="time">时间：{{time}}</div>

      <div class="score">得分：{{score}}</div>
    </div>
    <div class="card-group-wrap clearfix">
      <div class="card-wrap" v-for="item in items" >
        <transition name="cardhide">
          <div :class="item.className2" v-show="item.show">{{ item.message }}</div>
        </transition>
          <div :class="item.className" v-show="item.show" @click="clickcard(item,$event)"></div>
        
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'game',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      originItems: [
        {message:0,className:'card',className2:'card2',show:true},
        {message:1,className:'card',className2:'card2',show:true},
        {message:2,className:'card',className2:'card2',show:true},
        {message:3,className:'card',className2:'card2',show:true},
        {message:4,className:'card',className2:'card2',show:true},
        {message:5,className:'card',className2:'card2',show:true},
        {message:6,className:'card',className2:'card2',show:true},
        {message:7,className:'card',className2:'card2',show:true},
        {message:8,className:'card',className2:'card2',show:true},
        {message:9,className:'card',className2:'card2',show:true},
        {message:10,className:'card',className2:'card2',show:true},
        {message:11,className:'card',className2:'card2',show:true}
        ],
      items: [],
      time: 59,
      score: 0,
      checkedResult: [],
      leftCount: 12,
    }
  },
  methods: {
    clickcard(item){

      if (item.className == 'card') {
        item.className = 'card checked'
        item.className2 = 'card2 checked2'
        this.checkedResult.push(item)
      }
      
      // 选择了2个卡
      if(this.checkedResult.length == 2){
        // 计算2个卡的和，判断是否匹配
        let result = parseInt(this.checkedResult[0].message) + parseInt(this.checkedResult[1].message)
        // 匹配成功
        if (result == 11) {
          // 2个匹配的卡隐藏
          this.checkedResult[0].show = false
          this.checkedResult[1].show = false
          // 数组移除隐藏的卡
          for(let i = 0; i < 2; i ++){
            this.checkedResult.shift()
          }
          this.leftCount -= 2
          this.score += 1
          if (this.leftCount == 0) {
            this.initItems()
          }
        }
      } else if(this.checkedResult.length == 3) {
        // 选择了3个卡
        for(let i = 0; i < 2; i ++){ 
          // 前2个选中的卡变为未选中状态 数组移除2个卡
          this.checkedResult[0].className = 'card'
          this.checkedResult[0].className2 = 'card2'
          this.checkedResult.shift()
        }
      }
    },
    initItems () {
      this.items = this.originItems
      this.items.shuffle()
      for (var i = 0; i < this.items.length; i++) {
        this.items[i].show = true
        this.items[i].className = 'card'
        this.items[i].className2 = 'card2'
      }
    }
  },
  mounted () {
    if (!Array.prototype.shuffle) {
      Array.prototype.shuffle = function() {
          for(var j, x, i = this.length; i; j = parseInt(Math.random() * i), x = this[--i], this[i] = this[j], this[j] = x);
          return this;
    };
    let vm = this
    vm.initItems()
    vm.si = setInterval(function(){
          if(vm.time > 1){
            vm.time -= 1
          } else {
            clearInterval(vm.si)
            vm.gaming = true
          }
        },1000)

    
}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.game{
  height: 100%;
  padding-top: 1rem;
  box-sizing: border-box;
}
.card-group-wrap{
  width:6rem;
  margin: auto;
}
.card-wrap{
  width:1.8rem;
  height: 2rem;
  margin: .1rem;
  float:left;
  perspective:500px;
  position: relative;
}
.card{
  width:1.8rem;
  height: 2rem;
  margin: .1rem;
  background: #FB5B39;
  position: absolute;
  transform-style: preserve-3d;
  backface-visibility: hidden;
  transition:0.5s ease-in-out ;
}
.card2{
  width:1.8rem;
  height: 2rem;
  margin: .1rem;
  background: #FB5B39;
  position: absolute;
  transform-style: preserve-3d;
  transition:0.5s ease-in-out;
  transform: rotateY(-180deg);
}
.checked{
  transform: rotateY(180deg);
}
.checked2{
  transform: rotateY(0deg);
}
.noteboard div{
  float:left;
  width: 50%;
  padding:.25rem;
  box-sizing: border-box;
  text-align: center;
}
.time{
  text-align: right;
}
.clearfix{
  overflow: hidden;
}
.cardhide-leave-active {
  transition: transform 1s
}
.cardhide-leave-to /* .fade-leave-active in <2.1.8 */ {
  transform: scale(0);
}
</style>

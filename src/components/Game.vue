<template>
  <div class="game">
    <div class="noteboard clearfix">
      <div class="time">时间：{{time}}</div>

      <div class="score">得分：{{score}}</div>
    </div>
    <div class="card-group-wrap clearfix">
      <div class="card-wrap" v-for="item in items" >
        <transition name="bounce">
          <div :class="item.className2" v-show="item.show">{{ item.message }}</div>
        </transition>
        <transition name="bounce2">
          <div :class="item.className" v-show="item.show" @click="clickcard(item,$event)"></div>
        </transition>
        
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
        {message:0,className:'card',className2:'card2',show:false},
        {message:1,className:'card',className2:'card2',show:false},
        {message:2,className:'card',className2:'card2',show:false},
        {message:3,className:'card',className2:'card2',show:false},
        {message:4,className:'card',className2:'card2',show:false},
        {message:5,className:'card',className2:'card2',show:false},
        {message:6,className:'card',className2:'card2',show:false},
        {message:7,className:'card',className2:'card2',show:false},
        {message:8,className:'card',className2:'card2',show:false},
        {message:9,className:'card',className2:'card2',show:false},
        {message:10,className:'card',className2:'card2',show:false},
        {message:11,className:'card',className2:'card2',show:false}
        ],
      items: [],
      time: 59,
      score: 0,
      checkedResult: [],
      leftCount: 12,
      gameover: false
    }
  },
  methods: {
    clickcard(item){

      if (this.gameover) {
        return
      }

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
          let vm = this
          setTimeout(function(){
            vm.checkedResult[0].show = false
            vm.checkedResult[1].show = false

            // 数组移除隐藏的卡
            for(let i = 0; i < 2; i ++){
              vm.checkedResult.shift()
            }
            vm.leftCount -= 2
            vm.score += 1
            if (vm.leftCount == 0) {
              vm.initItems(1)
            }
          },150)
          // 2个匹配的卡隐藏
          /*this.checkedResult[0].show = false
          this.checkedResult[1].show = false*/
          
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
    initItems (p) {
      
      let vm = this
      vm.items = vm.originItems
      vm.items.shuffle()
      vm.leftCount = 12
      vm.checkedResult = []
      setTimeout(function(){
        for (var i = 0; i < vm.items.length; i++) {
          vm.items[i].show = true
          vm.items[i].className = 'card'
          vm.items[i].className2 = 'card2'
        }
      },250)
    }
  },
  mounted () {
    if (!Array.prototype.shuffle) {
      Array.prototype.shuffle = function() {
          for(var j, x, i = this.length; i; j = parseInt(Math.random() * i), x = this[--i], this[i] = this[j], this[j] = x);
          return this;
      }
    }
    let vm = this
    console.log(vm)
    vm.initItems()
    vm.si = setInterval(function(){
          if(vm.time >= 1){
            vm.time -= 1
          } else {
            clearInterval(vm.si)
            vm.gameover = true
            sessionStorage.setItem('score',vm.score)
            vm.$router.push('/over')
          }
        },1000)
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
  /*background: #FB5B39;*/
  background: url('../assets/cardback.png');
  background-size: cover;
  position: absolute;
  transform-style: preserve-3d;
  backface-visibility: hidden;
  transition:0.12s ease-in-out ;
}
.card2{
  width:1.8rem;
  height: 2rem;
  margin: .1rem;
  /*background: #46FA3B;*/
  background: url('../assets/card1.png');
  background-size: cover;
  position: absolute;
  transform-style: preserve-3d;
  transition:0.12s ease-in-out;
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
  transition: opacity 1s
}
.cardhide-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0.1;
}
.bounce2-enter-active {
  animation: bounce-in .5s;
}
.bounce-leave-active {
  animation: bounce-out .5s;
}
.bounce-enter-active {
  animation: bounce-in .5s;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
    opacity: 1;
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}
@keyframes bounce-out {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(0);
  }
}
</style>

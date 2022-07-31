<template>
  <div v-if="(countClick == 0) || (time == 0) || ((isVisibility === false) && (remainingPairs === 0))">
    <ResultView :status="status"/>
  </div>
  <div v-else>
    <div class="timer">  
    {{ ('0' + Math.floor(time / 60).toString().slice(-2)) }} : {{ ( ((time < 10) || (time == 60  ) || (time % 60 == 0)) ? '0' + (time % 60).toString().slice(-2) :  (time % 60).toString().slice(-2)) }}
    </div>  
    <transition-group tag="section" class="game-board" name="shuffle-card">
      <card v-for="card in cardList"
            :key="`${card.value}-${card.variant}`" 
            :matched="card.matched"
            :value="card.value" 
            :visible="card.visible" 
            :position="card.position" 
            :checkbox="card.checkbox"
            @select-card="flipCard"
      />
      <div class="count">
        {{ countClick }}
      </div>
      <div>
        <button @click="restartGame" class="botton">Restart</button>
      </div>
    </transition-group>
  </div>
</template>

<script>
  import { useRoute } from 'vue-router'
  import _ from 'lodash'
  import { computed, ref, watch, reactive } from 'vue'
  //import { launchConfetti } from "./utilities/confetti.js"
  import Card from '@/components/Card.vue'
  import ResultView from './ResultView.vue'
  export default {
    name: "App",
    components: {
    Card,
    ResultView
    },
    props: {
      form: {
        type: Object,
        required: true
      }
    },
    
    setup(props){
      const route = useRoute();
      const cardList = ref([])
      const userSelection = ref ([])
      const countDown = +props.form.time
      const countSelect = +props.form.count
      const isStatus = ref(true)
      const isVisibility = ref(true)

      const countClick = ref(countSelect)
      const time = ref(countDown * 60)

      const countDownTimer = () => {
          if(time.value >= 1){
            setTimeout(() => {
              time.value -= 1
              countDownTimer()
            }, 1000)
          }
      }

      const status = computed(() => {
        if ((remainingPairs.value === 0) && (time.value !== 0) && (countClick.value !== 0)){
          return "Player Wins!"
        } else if ((remainingPairs.value !== 0)) {
          if((time.value === 0) || (countClick.value === 0)) {
            return "Game Over!"
          } else {
            return `Remaining Pairs: ${remainingPairs.value}`  
          }
        } else{
          return `Remaining Pairs: ${remainingPairs.value}`
        }  
      })
      
      const remainingPairs = computed(() => {
        const remainingCards = cardList.value.filter(card => card.matched === false).length 
          return remainingCards / 2
      })
      console.log(status)
      const shuffleCards = () => {
        cardList.value = _.shuffle(cardList.value)
      }
      
      const restartGame = () => {
        shuffleCards()
        time.value = countDown.value * 60
        countClick.value = ref(+route.query.count)
        setTimeout(() => { 
          cardList.value = cardList.value.map ((card, index) => {
            return {
              ...card,
              matched: false,
              position: index,
              visible: false,
              //checkbox: true
            }
          })
        }, 3000)  
        cardList.value = cardList.value.map ((card, index) => {
            return {
              ...card,
              matched: true,
              position: index,
              visible: true,
              // checkbox: false
            }
        })
        countDownTimer()
      }

      const cardItems = ["product-1", "product-2", "product-3", "product-4", "product-5", "product-6", "product-7", "product-8"]
      cardItems.forEach(item => {
        cardList.value.push({
          value: item,
          variant: 1,
          visible: false,
          position: null,
          matched: false,
          checkbox: false
        })
        cardList.value.push({
          value: item,
          variant: 2,
          visible: false,
          position: null,
          matched: false,
          checkbox: false
        })
        //shuffleCards()
      })

      cardList.value = cardList.value.map((card,index) => {
        return{
          ...card,
          position: index
        }
      })
      setTimeout(() => { 
        cardList.value = cardList.value.map ((card, index) => {
          return {
            ...card,
            matched: false,
            position: index,
            visible: false,
            checkbox: true
          }
        })
        countDownTimer()
      }, 5000)  
      cardList.value = cardList.value.map ((card, index) => {
        return {
          ...card,
          matched: true,
          position: index,
          visible: true,
          checkbox: false
        }
      })
     
      const flipCard = payload => {
        isVisibility.value = false
        countClick.value--
        cardList.value[payload.position].visible = true
        if (userSelection.value[0]){
          if(userSelection.value[0].position === payload.position && userSelection.value[0].faceValue === payload.faceValue){
            return
          } else {
            userSelection.value[1] = payload
          }
        } else {
          userSelection.value[0] = payload
        }
      }

      // watch( remainingPairs,
      //   currentValue => {
      //     if((currentValue === 0) && (countDown.value !== 0) && (countClick.value > 0)){
      //       launchConfetti()
      //     }
      //   }
      // )

      watch (
        userSelection,
        currentValue => {
          if (currentValue.length === 2){
            const cardOne =currentValue[0]
            const cardTwo =currentValue[1]

            if (cardOne.faceValue === cardTwo.faceValue){
              cardList.value[cardOne.position].matched = true
              cardList.value[cardTwo.position].matched = true
            } else {
              setTimeout(() => {
                cardList.value[cardOne.position].visible = false
                cardList.value[cardTwo.position].visible = false
              }, 1000)
            }
            userSelection.value.length = 0
          }
        },
        { deep: true }
      )

      return{
        cardList,
        flipCard,
        userSelection,
        status,
        shuffleCards,
        restartGame,
        countDownTimer,
        countDown,
        countClick,
        time,
        isStatus,
        remainingPairs,
        isVisibility
      }
    }
  }
</script>


<style>
html, body {
  background-color: #120f0f;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.timer {
  position: absolute;
  margin-left: 40px;
  margin-top: -40px;
  margin-bottom: 10px;
  font-weight: bold;
}
.count {
  width: 40px;
  height: 40px;
  margin-left: 10px;
  margin-top: 10px;
  font-weight: bold;
  border: 2px solid #16599f;
  border-radius: 10px;
  padding-top: 5px;
  background-color: #16599f;
  color: white; 
}

.status {
  position: absolute;
  margin-left: 350px;
  margin-top: 20px;
  margin-bottom: 42px;
  font-weight: bold;
}

.game-board {
  display: grid;
  grid-row: 4;
  grid-column: 3;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-column-gap: 20px;
  grid-row-gap: 20px; 
  justify-content: center;
  margin-top:80px;
}
.botton {
  background-color: #16599f;
  color: white;
  padding: 0.75rem 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  border: 2px solid #16599f;
  margin-left: 265px;
  margin-top: 10px;
  font-weight: bold;
}
.shuffle-card-move {
  transition: transform 0.8s ease-i;
}
</style>
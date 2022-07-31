<template>
  <!-- <main> -->
    <!-- <div v-if="(form.count === null) && (form.time === null)"> -->
    <div v-if="isVisibility">
      <div class="login-box">
        <h2>Input</h2>
        <form @submit.prevent="validation">
          <div class="user-box">
            <input type="number" name="count" placeholder="40" v-model="form.count">
            <label>Count Selection</label>
            <small>{{ form.countError }}</small>
          </div>
          <div class="user-box">
            <input type="number" name="time" placeholder="2" v-model="form.time">
            <label>Time</label>
            <small >{{ form.timeError }}</small>
          </div>
          <!-- <router-link :to="{ name: 'game',  query: { debug: true }}">Navigate to Page2</router-link> -->
          <button type="submit">
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            strat
          </button>
        </form>
      </div>
    </div>
    <div v-if="(isVisibility === false)" class="game-box">
        <GameView :form="form" />
    </div>
    <!-- </div> -->
  <!-- </main> -->
</template>

<script>
import { useRouter, useRoute } from 'vue-router'
import { reactive, ref } from 'vue'
import GameView from './GameView.vue'

export default {
    name:"modal",
    components: {
        GameView,
    },
    setup() {
        const form = reactive({
            count: null,
            countError: "",
            time: null,
            timeError: "",
        });
        const router = useRouter();
        const route = useRoute();
        const isVisibility = ref(true)
        const validation = () => {
            var access = true;
            const cnt = +form.count;
            const t = +form.time;
            if ((cnt == null) || (cnt >= 60) || (cnt <= 20)) {
                access = false;
                form.countError = "count is required";
            }
            else {
                form.countError = "";
            }
            if ((t == null) || (t > 5) || (t < 1)) {
                access === false;
                form.timeError = "time is required";
            }
            else {
                form.timeError = "";
            }
            if (access) {
                isVisibility.value = false;
            }
        };
        // if (access){
        //     router.push({
        //       name: 'game',
        //       query: {
        //         count: form.count,
        //         time: form.time 
        //     }
        //   })
        // }
        return {
            validation,
            isVisibility,
            form
        };
    },
}
</script>

<style scope>
body {
  margin:0;
  padding:0;
  font-family: sans-serif;
  background: linear-gradient(#141e30, #243b55);
}
.game-box {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 700px;
  padding: 90px;
  transform: translate(-50%, -50%);
  box-sizing: border-box;
  box-shadow: 0 15px 25px rgba(0,0,0,.6);
}

.login-box {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 400px;
  padding: 40px;
  transform: translate(-50%, -50%);
  background: rgba(0,0,0,.5);
  box-sizing: border-box;
  box-shadow: 0 15px 25px rgba(0,0,0,.6);
  border-radius: 10px;
}

.login-box h2 {
  margin: 0 0 30px;
  padding: 0;
  color: #fff;
  text-align: center;
}

.login-box .user-box {
  position: relative;
}

.login-box .user-box input {
  width: 100%;
  padding: 10px 0;
  font-size: 16px;
  color: #fff;
  margin-bottom: 20px;
  border: none;
  border-bottom: 1px solid #fff;
  outline: none;
  background: transparent;
}
.login-box .user-box label {
  position: absolute;
  top:0;
  left: 0;
  padding: 10px 0;
  font-size: 16px;
  color: #fff;
  pointer-events: none;
  transition: .5s;
}

.login-box .user-box input:focus ~ label,
.login-box .user-box input:valid ~ label {
  top: -20px;
  left: 0;
  color: #386ca3;
  font-size: 12px;
}
.login-box form button {
  position: relative;
  display: inline-block;
  padding: 10px 20px;
  color: #386ca3;
  font-size: 16px;
  text-decoration: none;
  text-transform: uppercase;
  overflow: hidden;
  transition: .5s;
  margin-top: 40px;
  letter-spacing: 4px;
  background: #7f7f7f;
  border: 2px solid #7f7f7f;
}

.login-box button:hover {
  background: #386ca3;
  color: #fff;
  border-radius: 5px;
  box-shadow: 0 0 5px #386ca3,
              0 0 25px #386ca3,
              0 0 50px #386ca3,
              0 0 100px #386ca3;
}

.login-box button span {
  position: absolute;
  display: block;
}

.login-box button span:nth-child(1) {
  top: 0;
  left: -100%;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, transparent, #386ca3);
  animation: btn-anim1 1s linear infinite;
}

@keyframes btn-anim1 {
  0% {
    left: -100%;
  }
  50%,100% {
    left: 100%;
  }
}

.login-box button span:nth-child(2) {
  top: -100%;
  right: 0;
  width: 2px;
  height: 100%;
  background: linear-gradient(180deg, transparent, #386ca3);
  animation: btn-anim2 1s linear infinite;
  animation-delay: .25s
}

@keyframes btn-anim2 {
  0% {
    top: -100%;
  }
  50%,100% {
    top: 100%;
  }
}

.login-box button span:nth-child(3) {
  bottom: 0;
  right: -100%;
  width: 100%;
  height: 2px;
  background: linear-gradient(270deg, transparent, #386ca3);
  animation: btn-anim3 1s linear infinite;
  animation-delay: .5s
}

@keyframes btn-anim3 {
  0% {
    right: -100%;
  }
  50%,100% {
    right: 100%;
  }
}

.login-box button span:nth-child(4) {
  bottom: -100%;
  left: 0;
  width: 2px;
  height: 100%;
  background: linear-gradient(360deg, transparent, #386ca3);
  animation: btn-anim4 1s linear infinite;
  animation-delay: .75s
}

@keyframes btn-anim4 {
  0% {
    bottom: -100%;
  }
  50%,100% {
    bottom: 100%;
  }
}
.login-box small {
  color: red;
  padding-left: 12px;
}

</style>
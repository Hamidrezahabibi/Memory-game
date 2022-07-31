<template>
    <div class="success" v-if="status ==='Player Wins!'">
        <div class="card">
            <div style="border-radius:200px; height:200px; width:200px; background: #F8FAF5; margin:0 auto;">
                <i class="checkmark">✓</i>
            </div>
            <h1>Player Wins</h1> 
            <p>We received your purchase request;<br/> we'll be in touch shortly!</p>
        </div>
    </div>
    <div class="danger" v-if="status ==='Game Over!'">
        <div class="card">
            <div style="border-radius:200px; height:200px; width:200px; background: #e3aabc; margin:0 auto;">
                <h2 class="multiplied">!</h2>
            </div>
            <h1>Game Over</h1> 
            <p>We received your purchase request;<br/> we'll be in touch shortly!</p>
        </div>
    </div>   
</template>
<script>
export default {
    name: "ResultView",
    props: {
      status: {
        type: String,
        required: true
      }
    },
    
    setup (props) {
        console.log(props.status)
    }
}    
</script>
<style scoped>
body {
    background: #ffffff;
}
.success {
    text-align: center;
    padding: 40px;
    background: #ffffff;
}
h1 {
    color: #88B04B;
    font-family: "Nunito Sans", "Helvetica Neue", sans-serif;
    font-weight: 900;
    font-size: 40px;
    margin-bottom: 10px;
}
p {
    color: #404F5E;
    font-family: "Nunito Sans", "Helvetica Neue", sans-serif;
    font-size:20px;
    margin: 0;
}
i {
    color: #9ABC66;
    font-size: 100px;
    line-height: 200px;
    margin-left:-15px;
}
.card {
    background: white;
    padding: 40px;
    box-shadow: 0 2px 3px #C8D0D8;
    display: inline-block;
}
.danger {
    text-align: center;
    padding: 40px;
    background: #ffffff;
}
.danger .card h1 {
    color: #dd157a;
    font-family: "Nunito Sans", "Helvetica Neue", sans-serif;
    font-weight: 900;
    font-size: 40px;
    margin-bottom: 10px;
}
.multiplied {
    color: #dd157a;
}
.danger .card h2 {
    font-family: "Nunito Sans", "Helvetica Neue", sans-serif;
    font-size: 140px;
}
</style>
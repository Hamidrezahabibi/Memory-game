<template>
    <div class="card" @click="selectCard" :class="flippedStyles">
        <div v-if="visible" class="card-face is-front">
            <img :src="`../../public/image/${value}.jpg`" :alt="value" class="image"/>
            <img v-if="(matched) && (checkbox)" src="../../public/image/checkmark.svg" class="checkmark-icon"/>
        </div>
        <div v-else class="card-face is-back">
        </div>
    </div>
</template>


<script>
import { computed } from "vue"
export default {
    props: {
        matched: {
            type: Boolean,
            default: false
        },
        position: {
            type: Number,
            required: true
        },
        value: {
            type: String,
            required: true
        },
        visible: {
            type: Boolean,
            default: false
        },
        checkbox: {
            type: Boolean,
            default: false   
        }
    },
       

    setup(props, context){
        const flippedStyles = computed(() => {
            if(props.visible){
                return 'is-flipped'
            }
        })
        const selectCard = () => {
            context.emit('select-card', {
                position: props.position,
                faceValue: props.value,
            })
        }
        return {
            selectCard,
            flippedStyles
        }
    }   
}
</script>


<style>
.card {
    position: relative;
    transition: 0.5s transform ease-in;
    transform-style: preserve-3d;
}
.card.is-flipped{
    transform: rotateY(360deg);
}
.card-face {
    width: 100%;
    height: 100%;
    position:absolute;
    border-radius: 10px;
    align-items: center;
    justify-content: center;
    backface-visibility: hidden;
}
.card-face.is-front {
    color:white;
    transform: rotateY(360deg);
}
.card-face.is-back {
    background-image: url('../../public/image/bg-02.jpeg');
    color:white;
}
.checkmark-icon {
    position: absolute;
    width: 25px;
    height: 25px;
    right: 3px;
    bottom: 3px;
}
.image {
    width: 100px;
    height: 100px;
    justify-content: center;
    border-radius: 10px;
}
</style>
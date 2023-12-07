<template>
   <div class="screen">
    <div class="screen__inner"
    :style="{ 
        width: `${((((920 - 64)/Math.sqrt(cardContext.length)-16)*3)/4 + 16) * Math.sqrt(cardContext.length)}px`
        }"
    >  
        <card-flip 
        v-for="(card, index) in cardContext" 
        :key="index" 
        :ref="`card-${index}`" 
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value:card }" 
        :cardContext="cardContext"
        @onFlip="checkRule($event)"
        ></card-flip></div>
      
    </div>
</template>
<script>
import CardFlip from './CardFlip.vue';
export default {
    props: {
        cardContext: {
            type: Array,
            default: function() {
                return [];
            },
        },
    },
    data() {
        return {
            rules: [],
        }
    },
    methods: {
        checkRule(card) {
           if (this.rules.length == 2) return false;
           this.rules.push(card);

           if(this.rules.length == 2 && this.rules[0].value === this.rules[1].value){
            console.log("right...");
            this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableMode();
            this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableMode();
            this.rules = [];

            const disabledElement = document.querySelectorAll(".screen .card.disabled");
            if(disabledElement && disabledElement.length === this.cardContext.length-2) {
                setTimeout(() => {
                    this.$emit('onFinish')
                },920)
            }

           }else if(this.rules.length == 2 && this.rules[0].value !== this.rules[1].value){
            console.log("wrong");
            setTimeout(() => {
                this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
                this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
                this.rules = [];
            }, 800)
           }else{
                console.log("1");
           }
        }
    },
  components: { 
    CardFlip
},
}
</script>

<style lang="css" scoped>
    .screen {
        width: 100%;
        height: 100vh;
        position: absolute;
        top: 0;
        left: 0;
        z-index: 2;
        background-color: var(--dark);
        color:var(--light);
    }

    .screen__inner {
        display: flex;
        flex-wrap: wrap;
        margin: 2rem auto;
    }
</style>
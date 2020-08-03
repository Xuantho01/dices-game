<template>
    <div class="wrapper-popup" v-bind:class="getClassPopup">
        <div class="rule">
            <h3>Rule</h3>
            <p>
                term of rule
            </p>
            <button class="confirm" v-on:click="confirm"> Yes </button>
        </div>
    </div>
</template>

<script>
    export default {
        name: "popupRule",
        data(){
            return{
            }
        },
        props: {
            isOpenPopup: Boolean
        },
        computed: {
            getClassPopup:function(){
                return{
                    'open-popup': this.isOpenPopup
                }
            }
        }, methods: {
            confirm(){
                console.log('set request confirm');
                this.$emit('handleConfirm');
            }
        }
    }
</script>

<style scoped>
    .open-popup .rule{
        transform: translate(-50%, -50%) scale(1);
    }
    .wrapper-popup.open-popup{
        opacity: 1;
        visibility: visible;
        transition: all .3s ease;
    }
 .wrapper-popup{
     position: fixed;
     top: 0;
     opacity: 0;
     visibility: hidden;
     left: 0;
     right: 0;
     bottom: 0;
     background-color: rgba(0,0,0,0.4);
 }
    .rule{
        position: absolute;
        width: 350px;
        padding: 20px;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%) scale(1.2);
        background-color: #fff;
        font-family: Arial, Helvetica, sans-serif;

    }
    .rule h3{
        margin-bottom: 10px;
    }
    .rule .confirm{
        cursor: pointer;
        margin-top: 20px;
        padding: 8px 15px;
        border: 2px solid #333;
        background-color: #fff;
        transition: all .3s ease;
    }
    .rule .confirm:hover{
        color: #fff;
        background-color: #333;
    }
</style>

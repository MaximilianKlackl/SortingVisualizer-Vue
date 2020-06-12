<template>
    <nav>
        <ul>
            <li><v-select :options="sortType" v-model="type" @input="updateType()"></v-select></li>
            <li>
                <img src="../assets/ruler.png" alt="ruler" class="emoji"><span class="slider-label">Length</span>
                <vue-slider v-model="length" @change="updateLength" v-bind="lengthSliderOptions" /> </li>
            <li>
                <img src="../assets/hourglass.png" alt="hourglass" class="emoji"><span class="slider-label">Speed</span>
                <vue-slider  v-model="speed" @change="updateSpeed" v-bind="speedSliderOptions" /> </li>
            <li>
                <button class="sort-button" @click="startSort">
                    <span v-if="!start">Sort</span>
                    <span v-if="start">Stop</span>
                </button>
            </li>
        </ul>
    </nav>
</template>

<script>
import VueSlider from 'vue-slider-component'
import 'vue-slider-component/theme/antd.css'
import vSelect from "vue-select";
import 'vue-select/dist/vue-select.css';

export default {
    name: 'Controls',
    components: {
        VueSlider,
        vSelect
    },
    data: function(){
        return{
            sortType: ["Bubble Sort", "Selection Sort", "Quick Sort"],
            type: "Bubble Sort",
            speed: 200,
            length: 50,
            start: false,

            lengthSliderOptions: {
                max: 200,
                min: 10,
                width: 200
            },

            speedSliderOptions: {
                max: 500,
                min: 1,
                width: 200
            }
        }
    },
    methods: {
        updateLength: function(){ this.$root.$emit("updateLength", this.length) },
        updateSpeed: function(){ this.$root.$emit("updateSpeed", this.speed) },
        startSort: function() { 
            this.start = !this.start;
            this.$root.$emit("startSort", this.start); 
        },
        updateType: function(){
            this.$root.$emit("updateSortType", this.type);
        },
    },
    mounted: function() {
        this.$root.$on('updateStartData', data => { this.start = data; });    
    }
}
</script>

<style scoped>
    nav{
        width: 100%;
        height: 70px;
        padding-top: 30px;
    }

    ul{
        margin: 0;
        padding: 0;
        display: flex;
        justify-content:left;
    }

    li{
        list-style: none;
        display: inline;
    }

    li+li{
        padding-left: 20px;
    }

    .selected{
        color: white;
    }
    
    input[type=range]::-webkit-slider-runnable-track 
    {
        background-color: #ffffff;
        height: 4px;
        margin-top: -12px;
    }

    input[type=range]::-webkit-slider-thumb {
        -webkit-appearance: none;
        height: 20px;
        width: 20px;
        border-radius: 20px;
        background: #00FFA8;
        cursor: pointer;
        margin-top: -6px; /* You need to specify a margin in Chrome, but in Firefox and IE it is automatic */
    }

    button{
        background-color: white;
        color: #394066;
        width: 100px;
        height: 32px;
        border: 1px solid #CDCDCD;
        border-radius: 5px;
        font-size: 16px;
    }

    button:focus{
        outline: none;
        box-shadow: 0 0 4px #9CD5FF;
    }

    li:first-child{
        width: 160px;
    }

    .emoji{
        width: 20px;
        height: 20px;
        padding-right: 8px;
    }

    .slider-label{
        position: relative;
        top: -4px;
    }
</style>

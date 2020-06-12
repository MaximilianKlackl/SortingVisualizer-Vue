<template>
    <div class="container" ref="container" >
        <Bar 
        v-for="(element, index) in dataArray" 
        v-bind:key="element" 
        v-bind:barWidth="barWidth" 
        v-bind:class="{ width: updateBarWidth() }"
        v-bind:value="getHeigth(element)" 
        v-bind:hightlight="hightlightArray[index]">
        </Bar>
    </div>
</template>

<script>
import Bar from './Bar.vue'
import Vue from "vue"

export default {
    name: 'Visualizer',
    components: {
        Bar
    },
    data: function() {
        return {
            speed: 50,
            length: 50,
            start: false,
            type: "Bubble Sort",
            barWidth: "10px",
            dataArray: [],
            hightlightArray: [],
        }
    },
    mounted: async function() {
        this.$root.$on('updateSpeed', data => { this.speed = data; });
        this.$root.$on('updateSortType', data => { this.type = data; });
        this.$root.$on('startSort', data => { 
            this.start = data;
            if(data){
                this.start = data; 
                if(this.type === "Bubble Sort") {
                    this.sortBubbleSort()
                }
                else if(this.type === "Selection Sort") {
                    this.sortSelectionSort()
                }
                else if(this.type === "Quick Sort") {
                    this.sortQuickSort(this.dataArray, 0, this.dataArray.length - 1)
                }
            }
        });
        this.$root.$on('updateLength', data => { 
            this.length = data; 
            this.updateArr();
            //this.updateBarWidth();
        });
        this.updateArr()
    },
    methods: {
        updateStartData: function() { 
            this.start = !this.start;
            this.$root.$emit("updateStartData", this.start); 
        },
        updateBarWidth: function() {
            this.barWidth = (1400 / this.length) + "px";
        },
        random: function(min, max) {
            let r = Math.floor(Math.random() * (max - min + 1) + min);
            while(this.dataArray.includes(r)){
                r = Math.floor(Math.random() * (max - min + 1) + min)
            }
            return r;
        },
        getHeigth: function(value) {
            return value + "px"
        },
        updateArr: function() {
            this.dataArray = [];
            for(let i = 0; i < this.length; i++){
                this.dataArray.push(this.random(20,650));
                this.hightlightArray.push(false);
            }
        },
        sortBubbleSort: async function() {
            for (let i = 0; i < this.length ; i++) {
                for(let j = 0 ; j < this.length - i - 1; j++){ 
                    if (this.dataArray[j] > this.dataArray[j + 1]) {

                        await this.swap(j, j+1)
                        if(!this.start){
                            break;
                        }
                    }
                }
                if(!this.start){
                        break;
                }
            }

            this.updateStartData()
            this.resetHighlight()
        },
        sortSelectionSort: async function(){
          
            for (let i = 0; i < this.length; i++) {
                let min = i;
                for (let j = i + 1; j < this.length; j++) {
                    if (this.dataArray[min] > this.dataArray[j]) {
                        min = j;
                    }
                }
                if (min !== i) {
                    await this.swap(i, min);
                }
                if(!this.start){
                        break;
                }
            }
            
            this.updateStartData();
            this.resetHighlight()
        },
        
        sortQuickSort: async function(arr, left, right)
        {
            var i = left;
            var j = right;
            let pivotidx = (left + right) / 2; 
            var pivot = parseInt(arr[pivotidx.toFixed()]);  
            /* partition */
            while (i <= j)
            {
                while (parseInt(arr[i]) < pivot)
                i++;
                while (parseInt(arr[j]) > pivot)
                    j--;
                if (i <= j)
                {
                    await this.swap(i, j)
                    i++;
                    j--;
                }
            }

            /* recursion */
            if (left < j){
                this.sortQuickSort(arr, left, j);
            }      
            if (i < right){
                this.sortQuickSort(arr, i, right);
            }

            return arr;
        },

        resetHighlight: function() {
            for(let i = 0; i < this.hightlightArray.length; i++){
                Vue.set(this.hightlightArray, i, false);
            }
        },
        sleep: async function(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        },
        swap: async function(index1, index2) {
            let temp = this.dataArray[index2];

            this.hightlightArray[index2] = true;
            this.hightlightArray[index1] = true;

            await this.sleep(this.speed);
            Vue.set(this.dataArray, index2, this.dataArray[index1]);
            Vue.set(this.dataArray, index1, temp);

            await this.sleep(this.speed);

            this.hightlightArray[index2] = false;
            this.hightlightArray[index1] = false;
        }
    }
}
</script>

<style scoped>
    .container {
        display: flex;
        justify-content: center;
    }

</style>

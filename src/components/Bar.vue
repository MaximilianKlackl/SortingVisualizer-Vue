<template>
    <div :style="{ height: value, width: barWidth }" :class="getHighlight()"></div>
</template>

<script>
export default {
    name: 'Controls',
    props: {
        value: String,
        barWidth: String,
        hightlight: Boolean
    },
    methods: {
        getHighlight: function() {
            return this.hightlight ?  "highlight" : "none-highlight"
        }
    }
}
</script>

<style scoped>
    div{
        background-color: #9CD5FF;
        border-left: 1px solid rgb(255, 255, 255);
    }

    .highlight{
        background-color: rgb(193, 255, 134);
    }
    
    .none-highlight{
        background-color: none;
    }
</style>

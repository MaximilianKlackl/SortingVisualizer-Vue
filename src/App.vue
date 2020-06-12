<template>
    <div id="app">
        <div class="body-container">
            <Controls />
            <Visualizer />
        </div>
    </div>
</template>

<script>
import Controls from './components/Controls.vue'
import Visualizer from './components/Visualizer.vue'

export default {
    name: 'App',
    components: {
        Controls,
        Visualizer
    }
}
</script>

<style>
    #app {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        background-color: rgb(255, 255, 255);
        color:#394066;
        margin: 0;
        height: 100vh;
        width: 100vw;
    }

    body, html{
        margin: 0;
        padding: 0;
    }

    .body-container{
        width: 80%;
        height: 100%;
        margin-left: 10%;
        margin-right: 10%;
    }
</style>

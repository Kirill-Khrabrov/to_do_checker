<template>
    <div id="app" class="container" >
       
        <to-do-list />

    </div>   
</template>

<script>
import ToDoList from './components/ToDoList';

export default {
    name: 'App',
    data () {
        return {
            
        }
    },
    components: {
        ToDoList        
    }
}
</script>

<style>
* {
    box-sizing: border-box;
}

.container {
    max-width: 600px;
    margin: 0 auto;
}

#app {
    font-family: 'Avenir', Arial, Helvetica, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
    font-size: 24px;
}
</style>

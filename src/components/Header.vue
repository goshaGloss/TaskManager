<template>
    <header>
        <h1>sup guys</h1>
        <Button @show_btn = "$emit('show_btn')"/>
    </header>
</template>



<script>
import Button from './Button'

export default {
    name:'Header',
    props:{
        title:String,
    },
    components: {
        Button
    }
}
</script>

<style scoped>
 header{
     display: flex;
     justify-content: space-between;
     margin-bottom: 10px;
     align-items: center;
}
</style>
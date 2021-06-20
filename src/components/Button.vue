<template>
    <button @click="onClick()" class="btn">
        wtf
    </button>
</template>

<script>
export default {
    name: 'Button',
    props: {
    text:String,
    color:String
    },
    methods:{
    onClick() {
        this.$emit('show_btn')
    }
}
}


</script>

<template>
<button @click="method"> {{name}} </button>
</template>

<script>
export default {
    data: function(){
        return{
            
        }
    },
    props: {
        method: Function,
        name: String
    }
}

</script>


<style>
</style>

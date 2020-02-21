<template>
<form>
    <div> 
        <label for="newtext">Update clicked node text</label>
    </div>
    <div> 
        <input type="text" name="newtext" v-model="newText"> 
    <!-- </div>   
    <div>  -->
        <button type="submit" @click="onSubmit">Submit</button>
    </div>
</form>
</template>

<script>
export default {
    props: {
        savedText: String
    },
    data: function(){
        return{
            newText: this.savedText
        }
    },
    methods: {
        onSubmit(event){
            event.preventDefault();
            this.$emit('childToParent', this.newText);

        }
    }
}

</script>


<style>
</style>

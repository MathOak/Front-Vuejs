<template>
    <v-container>    </v-container>
</template>

<script>
export default {
    data:()=>{
        return{
        
        methods: {
            updateWord(){
                console.log("Entrou")
                this.$nextTick(function () {
                this.this.$root.$emit('search', this.search);
                console.log("Entrou")
                })
            },

        }   

        }   
    },
}
</script>

<style>

</style>
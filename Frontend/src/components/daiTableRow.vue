<template>
    <tr>        
        <td> {{index}} </td>
        <td> {{dai.name}} </td>
        <td> {{dai.mark}} </td>
        <td> {{dai.number}} </td>
        <td> {{new Date(dai.year).toLocaleDateString()}} </td>
        <td>
            <input type="button" value="Вилучити" @click="remove(index)">
            <input type="button" value="Редагувати" @click="update(index)">
        </td>        
    </tr>
</template>

<script>
export default {
    name:"daiTableRow",
    props:{
        index:Number,
        book:Object        
    },
    data(){   
        return{

        }
    },
    methods:{        
        remove(index){
            this.$emit("remove", index);
        },
  
        update(index){
            this.$emit("update",index);
        }
    }
}
</script>

<style scoped>
    td{
        border: 1px solid black;
    }
</style>
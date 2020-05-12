<template>
    <div>
        <p v-if="daiList.length==0" class="alert">
            Водії відсутні
        </p>
        
        <table v-if="daiList.length>0">
            <tr>
                <th>#</th>
                <th v-on:click="sort('name')">  Ім'я </th>
                <th v-on:click="sort('mark')">  Марка </th>
                <th v-on:click="sort('number')"> Номер </th>
                <th v-on:click="sort('year')"> Рік </th>
                <th></th>
            </tr>
            <daiTableRow v-for="(dai,index) in daiList" 
                v-bind:key="dai._id" 
                v-bind="{dai,index}"
                @remove="remove"
                @update="update" 
            >             
            </daiTableRow>
        </table>
    </div> 
</template>

<script>
import daiTableRow from "./daiTableRow";

export default {
    name:"daiTable",
    props:{
        daiList:Array,
    },
    data(){
        return{
           dais: this.daiList
        }
    },
    components:{
        daiTableRow
    },
    methods:{
        //сортування по деякому полю (крім авторів)
        sort(field){
           this.bookList.sort((b1,b2)=> b1[field]>=b2[field]?1:-1);
        },
        //вилучити 
        remove(index){
            //генруємо подію, вилучення здійснить батьківський компонент
            this.$emit("remove",index);
        },  
        //оновити 
        update(index){
            //генруємо подію, оеовлення здійснить батьківський компонент
            this.$emit("update",index);
        }
    }
}
</script>

<style scoped>
    .alert{
        background: yellow;
        color: crimson;
    }

    table, table td{
        border-collapse: collapse;
        border: 1px solid black;
    }
</style>
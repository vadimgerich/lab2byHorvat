// таблична форма відображення новин
<template>
    <div>
        <p v-if="bookList.length==0" class="alert">
            Новини відсутні
        </p>
        
        <table v-if="newwList.length>0">
            <tr>
                <th>#</th>
                <th v-on:click="sort('title')">  Назва </th>
                <th v-on:click="sort('describe')"> Опис  </th>
                <th v-on:click="sort('text')">  Текст </th>
                <th v-on:click="sort('date')"> Дата </th>
                <th v-on:click="sort('count')"> Кількість </th>
                <th></th>
            </tr>
            <newwTableRow v-for="(neww,index) in newwList" 
                v-bind:key="neww._id" 
                v-bind="{neww,index}"
                @remove="remove"
                @update="update" 
            >             
            </newwTableRow>
        </table>
    </div> 
</template>

<script>
import newwTableRow from "./newwTableRow";

export default {
    name:"newwTable",
    props:{
        newwList:Array,
    },
    data(){
        return{
           newws: this.newwList
        }
    },
    components:{
        newwTableRow
    },
    methods:{
        //сортування по деякому полю 
        sort(field){
           this.newwList.sort((b1,b2)=> b1[field]>=b2[field]?1:-1);
        },
        //вилучити внигу
        remove(index){
            //генруємо подію, вилучення здійснить батьківський компонент
            this.$emit("remove",index);
        },  
        //оновити книгу
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
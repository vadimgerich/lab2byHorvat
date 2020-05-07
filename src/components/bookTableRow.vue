// рядок що відбражає 1 книгу
<template>
    <tr>        
        <td> {{index}} </td>
        <td> {{neww.title}} </td>
        <td> {{neww.describe}} </td>
        <td> {{new Date(neww.published).toLocaleDateString()}} </td>
        <td> {{neww.text}} </td>
        <td> {{neww.count}} </td>
        <td>
            <input type="button" value="Вилучити" @click="remove(index)">
            <input type="button" value="Редагувати" @click="update(index)">
        </td>        
    </tr>
</template>

<script>
export default {
    name:"newwTableRow",
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
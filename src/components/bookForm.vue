// Форма для відображення книги
<template>
    <form v-if="visible" @submit.prevent> <!-- якщо форма видима то показати її і відмінити надсилання запиту за замовчуванням-->
        <label> Назва <input type="text" v-model="neww.title" required> </label> <br>
        <label> Опис <input type="text" v-model="neww.shortDescribe"> <inputt></label> <br>
        <label> Текст <input type="text" v-model="neww.text"> </label> <br>
        <label> Дата публікації <input type="date" v-model="neww.date"> </label> <br>
        <label> Кількість <input type="number" v-model.number="book.count" min="0" step="0.01"> </label> <br>  
        <input type="button" @click="save()" value="Зберегти">     
    </form>
</template>

<script>
import setInput from "./setInput";

export default {
    name: "newwForm",
    props:{
        //передача значення в props через value і генерування події input необхідне для коректної роботи v-model компонента
        value: Object,
        visible: Boolean
    },
    data(){
        return{
            neww:this.value,            
        }
    },
    components:{
        setInput
    },
    methods:{
        save(){
            this.$emit('input', this.neww);         
        }
    }
}
</script>
<style scoped>
    form{
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        background: white ;
    }
</style>
<template>
  <div id="app">    
    <messageSystem v-bind:messagesList="messagesList"></messageSystem>
    <searchString v-model="searchText"></searchString>
    <queryForm v-model="queryObject"></queryForm>
    <input type="button" @click="showNewNewwForm()" value=" Додати кнгу" />
    <newwTable v-bind:newwList="filtredNewws" @remove="removeNeww" @update="showUpdateNewwForm"></newwTable>
    <newwForm v-model="neww" v-bind:visible="formVisible" @input="formInput"></newwForm>
  </div>
</template>

<script>
import Vue from "vue";
import newwTable from "./components/newwTable";
import newwForm from "./components/newwForm";
import searchString from "./components/searchString";
import queryForm from "./components/queryForm";
import messageSystem from "./components/messageSystem";
import axios from "axios";

export default {
  //назва компоненту, співпадає із назвою файла  
  name: "app",
  //компоненти які використовуються в додатку
  components: {
    newwTable,
    newwForm,
    searchString,
    queryForm,
    messageSystem
  },
  //дані компонента на початку
  data() {
    return {

      newws: [],
      neww: {
        title: "",
        authors: [],
        isbn: "",
        published: "1997-01-10T22:00:00.000Z",
        pages: 0,
        price: 0
      },
      //список повідомлень
      messagesList: [],
      //видимість форми
      formVisible: false,

      selectedIndex: -1,
      //текст пошуку
      searchText: "",
      //об'єкт з параметрами пошуку 
      queryObject: {
        minPages: null,
        maxPages: null,
        maxPrice: null
      }
    };
  },
  // значення які необхідно обчислити.  
  computed: {

    filtredNewws() {
      let result = this.newws;
      if (this.searchText)
        result = result.filter(neww =>
          neww.title.toLowerCase().includes(this.searchText.toLowerCase())
        );
      console.log(result);
      if (this.queryObject.minPages)
        result = result.filter(neww => neww.pages >= this.queryObject.minPages);
      console.log(result);
      if (this.queryObject.maxPages)
        result = result.filter(neww => neww.pages <= this.queryObject.maxPages);
      if (this.queryObject.maxPrice)
        result = result.filter(neww => neww.price <= this.queryObject.maxPrice);
      return result;
    }
  },
 
  mounted() {
    this.getNewws().then(newws => {
      this.newws = newws;
    });
  },
  //методи 
  methods: {

    async getNewws() {
      try {
        //чекаємо відповідь на запит GET  http://localhost:5000/neww
        let resp = await axios.get("http://localhost:5000/neww");
        //якщо все добре то повертаємо данні із відповіді на запит 
        return resp.data;
      } catch (e) {
        //якщо сталась помилка (в тому числі і сервер повернув 500)
        console.log(e);
        //додати повідомлення 
        this.messagesList.push(e);
      }
    },

    async postNeww(neww) {
      try {
        let resp = await axios.post("http://localhost:5000/neww", neww);
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },

    async deleteNeww(neww) {
      try {
        let resp = await axios.delete(`http://localhost:5000/neww/${neww._id}`);
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },

    async patchNeww(neww) {
      try {
        let resp = await axios.patch(
          `http://localhost:5000/neww/${neww._id}`,
          neww
        );
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },
 
    showNewNewwForm() {
      this.neww = Object.assign(thisk.neww, {
        title: "",
        authors: [],
        isbn: "",
        published: "1997-01-10T22:00:00.000Z",
        pages: 0,
        price: 0
      });
      this.formAction = this.addNewNeww;
      this.formVisible = true;
    },

    showUpdateNewwForm(index) {
      this.selectedIndex = index;
      Object.assign(this.neww, this.filtredNewws[index]);
      this.formAction = this.updateNeww;
      this.formVisible = true;
    },
    //додавання нової книги
    addNewNeww() {
      //спочатку робимо хук 
      this.postNeww(this.neww).

      then(neww => this.newws.push(neww));
    },

    removeNeww(index) {
      this.deleteNeww(this.filtredNewws[index]).
      then(() => {
        this.filtredNewws.splice(index, 1);
      });
    },

    updateNeww() {
      let i = this.selectedIndex;
      this.patchNeww(this.neww).then(neww => {
        Object.assign(this.filtredNewws[i], neww);
      });
      this.selectedIndex = -1;
    },

    formInput() {
      this.formAction();
      this.formVisible = false;
    },
    formAction: function() {}
  }
};
</script>

<style scoped>
#app {
  margin: 0;
  padding: 0;
}
</style>
<template>
  <div id="app">
    <messageSystem v-bind:messagesList="messagesList"></messageSystem>
    <searchString v-model="searchText"></searchString>
    <queryForm v-model="queryObject"></queryForm>
    <input type="button" @click="showNewDaiForm()" value=" Додати водія" />
    <daiTable v-bind:daiList="filtredDrivers" @remove="removeDai" @update="showUpdateDaiForm"></daiTable>
    <daiForm v-model="dai" v-bind:visible="formVisible" @input="formInput"></daiForm>
  </div>
</template>

<script>
import Vue from "vue";
import daiTable from "./components/daiTable";
import daiForm from "./components/daiForm";
import searchString from "./components/searchString";
import queryForm from "./components/queryForm";
import messageSystem from "./components/messageSystem";
import axios from "axios";

export default { 
  name: "app",
  components: {
    daiTable,
    daiForm,
    searchString,
    queryForm,
    messageSystem
  },
  data() {
    return {
      drivers: [],
      //обєкт 1 порожньої книги
      driver: {
        name: "",
        mark: "",
        number: "",
        year: 0
      },
      messagesList: [],
      
      formVisible: false,
      
      selectedIndex: -1,
      
      searchText: "",
      // //об'єкт з параметрами пошуку 
      // queryObject: {
      //   minPages: null,
      //   maxPages: null,
      //   maxPrice: null
      // }
    };
  },
  // значення які необхідно обчислити.  
  computed: {
    //відфільтрований список
    filteredDrivers() {
      let result = this.drivers;
      if (this.searchText)
        result = result.filter(dai =>
          dai.title.toLowerCase().includes(this.searchText.toLowerCase())
        );
      console.log(result);
    //   if (this.queryObject.minPages)
    //     result = result.filter(book => book.pages >= this.queryObject.minPages);
    //   console.log(result);
    //   if (this.queryObject.maxPages)
    //     result = result.filter(book => book.pages <= this.queryObject.maxPages);
    //   if (this.queryObject.maxPrice)
    //     result = result.filter(book => book.price <= this.queryObject.maxPrice);
    //   return result;
    // }
  },
  
  mounted() {
    this.getDrivers().then(drivers => {
      this.drivers = drivers;
    });
  },
  //методи 
  methods: {
    
    async getDrivers() {
      try {
        //чекаємо відповідь на запит GET  http://localhost:5000/book
        let resp = await axios.get("http://localhost:5000/book");
        //якщо все добре то повертаємо данні із відповіді на запит 
        return resp.data;
      } catch (e) {
        //якщо сталась помилка (в тому числі і сервер повернув 500)
        console.log(e);
        //додати повідомлення 
        this.messagesList.push(e);
      }
    },
    // асинхронний хук по додаванню 1 
    async postDriver(driver) {
      try {
        let resp = await axios.post("http://localhost:5000/book", driver);
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },
    // асинхронний хук по вилученню 1 книги
    async deleteDriver(driver) {
      try {
        let resp = await axios.delete(`http://localhost:5000/book/${driver._id}`);
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },
    // асинхронний хук по оновленню інформації про книгу
    async patchDriver(driver) {
      try {
        let resp = await axios.patch(
          `http://localhost:5000/book/${driver._id}`,
          driver
        );
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },
    // показуємо форму для нової книги  
    showNewDaiForm() {
      this.driver = Object.assign(this.driver, {
        
        name: "",
        mark: "",
        number: "",
        year: 0
  
      });
      this.formAction = this.addNewDriver;
      this.formVisible = true;
    },
    //показуємо форму для оновлення книги
    showUpdateDaiForm(index) {
      this.selectedIndex = index;
      Object.assign(this.driver, this.filtredDrivers[index]);
      this.formAction = this.updateDriver;
      this.formVisible = true;
    },
   
    addNewDriver() {
      
      this.postDriver(this.driver).
    
      then(book => this.books.push(book));
    },
    //вилучення
    removeDriver(index) {
      this.deleteDriver(this.filtredDrivers[index]).
      then(() => {
        this.filtredDrivers.splice(index, 1);
      });
    },
    //оновити
    updateDriver() {
      let i = this.selectedIndex;
      this.patchDriver(this.driver).then(driver => {
        Object.assign(this.filtredDrivers[i], driver);
      });
      this.selectedIndex = -1;
    },
    //показати форму редагування або оновлення
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
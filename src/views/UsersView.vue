<template>
  <div class="container">
    <div class="tabs">
        <span @click="toPage('users')" :class="{current: currentPage === 'users'}" class="title">Пользователи</span>
        <span @click="toPage('items')" :class="{current: currentPage === 'items'}" class="title">Предметы</span>
      </div>
    <div class="wrapper" v-if="currentPage === 'users'">
      <span>search field:</span>
      <select v-model="searchField">
        <option>id</option>
        <option>name</option>
      </select>
      
      <br/>

      <span>search value: </span>
      <input type="text" v-model="searchValue">
      <EasyDataTable
        v-model:items-selected="itemsSelected"
        show-index
        :headers="headers"
        :items="items"
        :search-field="searchField"
        :search-value="searchValue"
        @click-row="showRow"
      />
      <div class="card">
        <UserCard 
          :selectedItem="selectedItem"
          :isEdit="isEdit"
          @user="editUser"
          @onEdit="onEdit"
        />
      </div>
    </div>

    <div class="wrapper" v-else-if="currentPage === 'items'">
      <span>search field:</span>
      <select v-model="searchField">
        <option>id</option>
        <option>name</option>
      </select>
      
      <br/>

      <span>search value: </span>
      <input type="text" v-model="searchValue">
      <EasyDataTable
        v-model:items-selected="itemsSelected"
        show-index
        :headers="headers"
        :items="items"
        :search-field="searchField"
        :search-value="searchValue"
        @click-row="showRow"
      />
      <div class="card">
        <UserCard 
          :selectedItem="selectedItem"
          :isEdit="isEdit"
          @onEdit="onEdit"

        />
      </div>
      <div class="btns">

      </div>
    </div>
  </div>

</template>


<script>
import axios from "axios";
import UserCard from "@/components/UserCard.vue";

export default {
    name: 'adminitems',
    components: { UserCard},
    data () {
      return {
        currentPage: "users",
        searchField: "",
        searchValue: "",
        headers:  [
          { text: "id", value: "id", sortable: true },
          { text: "name", value: "name", sortable: true},
          { text: "money", value: "money", sortable: true},
          { text: "weight", value: "weight", sortable: true},
          { text: "age", value: "age", sortable: true},
          { text: "energy", value: "energy", sortable: true},
          { text: "health", value: "health", sortable: true},
          { text: "hungry", value: "hungry", sortable: true},
          { text: "power", value: "power", sortable: true},
          { text: "intellect", value: "intellect", sortable: true},
          { text: "lucky", value: "lucky", sortable: true},
          { text: "dirty", value: "dirty", sortable: true},
          { text: "drivecategory", value: "drivecategory", sortable: true},
          { text: "profession", value: "profession", sortable: true},
          { text: "created_at", value: "created_at", sortable: true},
        ],
        isSelected: false,
        isEdit: false,
        selectedItem: {},
        items: [],
        url: "https://we-game-api.onrender.com/api/"
      }
    },
    methods: {
      showRow (val) {
        this.selectedItem = val
        console.log('selectedItem', this.selectedItem)
      },
      async getItems() {
        this.items = (await axios.get(this.url+'items/')).data
      },
      async onEdit(user) {
        console.log('user', user)
          await axios.put(this.url+'items/', user)
          this.getItems()
      },
      async editUser(user){
        this.selectedItem = user
      },
      toPage(page) {
        this.currentPage = page
      }
    },
    mounted() {
      this.getItems()
    }
}
</script>

<style scoped>
.card {
  position: relative;
  width: 450px;
  height: 250px;
  margin: 10px 0px;
}
.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  height: 100%;
}
#myTable_wrapper {
  width: 100% !important;
}
.title {
  font-size: 20px;
  color: gray;
  font-weight: bold;
  cursor: pointer;
  user-select: none;
}
.title.current {
  color: black;
}
.sortable-chosen {
  border: dotted;
}
.pointer {
  cursor: pointer;
}
.noselect {
  pointer-events: none;
}
.tabs .title {
  margin: 0px 10px;
}
</style>

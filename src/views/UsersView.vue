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
      <select v-model="searchFieldItems">
        <option>id</option>
        <option>name</option>
      </select>
      
      <br/>

      <span>search value: </span>
      <input type="text" v-model="searchValue">
      <EasyDataTable
        v-model:items-selected="itemsSelected"
        show-index
        :headers="headersItems"
        :items="items"
        :search-field="searchField"
        :search-value="searchValue"
        @click-row="showRow"
      />

      <AddForm 
        :selectedItem="selectedItem"
        @onAdd="onAdd"
        @onEdit="onEditItem"
        @onDelete="onDeleteItem"
        />
    </div>
  </div>

</template>


<script>
import axios from "axios";
import UserCard from "@/components/UserCard.vue";
import AddForm from "@/components/AddForm.vue";

export default {
    name: 'adminitems',
    components: {AddForm, UserCard},
    data () {
      return {
        currentPage: "users",
        searchField: "",
        searchValue: "",
        searchFieldItems: "",
        headers:  [
          { text: "id", value: "id", sortable: true },
          { text: "Имя", value: "name", sortable: true},
          { text: "Деньги", value: "money", sortable: true},
          { text: "Вес", value: "weight", sortable: true},
          { text: "Возраст", value: "age", sortable: true},
          { text: "Энергия", value: "energy", sortable: true},
          { text: "Здоровье", value: "health", sortable: true},
          { text: "Голод", value: "hungry", sortable: true},
          { text: "Сила", value: "power", sortable: true},
          { text: "Интеллект", value: "intellect", sortable: true},
          { text: "Удача", value: "lucky", sortable: true},
          { text: "Чистота", value: "dirty", sortable: true},
          { text: "Водительские права", value: "drivecategory", sortable: true},
          { text: "Профессия", value: "profession", sortable: true},
          { text: "Дата создания", value: "created_at", sortable: true},
        ],
        headersItems:  [
          { text: "id", value: "id", sortable: true },
          { text: "Название", value: "name", sortable: true},
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
      },
      async getUsers() {
        this.items = (await axios.get(this.url+'users/')).data
      },
      async getItems() {
        this.items = (await axios.get(this.url+'items/')).data
      },
      async onEdit(user) {
          await axios.put(this.url+'users/', user)
          this.getUsers()
      },
      editUser(user){
        this.selectedItem = user
      },
      async onAdd(item) {
        await axios.post(this.url+'items/', {name: item.name})
        this.getItems()
      },
      async onDeleteItem(item) {
        await axios.delete(this.url+'items/' + item.id)
        this.getItems()
      },
      async onEditItem(item) {
        await axios.put(this.url+'items/', item)
        this.selectedItem = ''
        this.getItems()
      },
      toPage(page) {
        if(page === 'users') {
          this.getUsers()
        } else if(page === 'items') {
          this.getItems()
        }
        this.currentPage = page
      }
    },
    mounted() {
      this.getUsers()
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

<template>
  <div class="container">
    <span class="title">Предметы</span>
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
      :id="selectedItem.id"
      :name="selectedItem.name"
      :money="selectedItem.money"
      :weight="selectedItem.weight"
      :age="selectedItem.age"
      :energy="selectedItem.energy"
      :health="selectedItem.health"
      :hungry="selectedItem.hungry"
      :power="selectedItem.power"
      :intellect="selectedItem.intellect"
      :lucky="selectedItem.lucky"
      :dirty="selectedItem.dirty"
      :drivecategory="selectedItem.drivecategory"
      :profession="selectedItem.profession"
      :created_at="selectedItem.created_at"
    />
    </div>

    <div class="btns">
      <EditButton v-show="selectedItem.id"/>
      <DeleteButton v-show="selectedItem.id"/>
    </div>

  </div>
</template>


<script>
import axios from "axios";
import UserCard from "@/components/UserCard.vue";
import DeleteButton from "@/components/DeleteButton.vue";
import EditButton from "@/components/EditButton.vue";
export default {
    name: 'adminitems',
    components: { UserCard, DeleteButton, EditButton },
    data () {
      return {
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
        selectedItem: {},
        items: []
      }
    },
    methods: {
      showRow (val) {
        console.log(val)
        this.selectedItem = val
      },
      async getItems() {
        this.items = (await axios("https://we-game-api.onrender.com/api/items")).data
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
.container {
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
  font-weight: bold;
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

</style>

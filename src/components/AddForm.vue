<script>
export default {
  props: ['selectedItem'],
  data () {
      return {
        item: {
          id: '',
          name: '',
          description: '',
          category: ''
        },
        isEdit: false
      }
    },
    methods: {
      itemEmpty() {
        this.item = {
          id: '',
          name: '',
          description: '',
          category: ''
        }
      },
      onAdd() {
        this.$emit('onAdd', this.item)
        this.itemEmpty()
      },
      onDelete() {
        this.$emit('onDelete', this.item)
        this.itemEmpty()
      },
      onEdit() {
        this.$emit('onEdit', this.item)
        this.itemEmpty()
      },
    },
    watch: {
      item: {
        handler() {
          if(this.item.id === '') {
            this.isEdit = false
          }
        },
        deep: true
      },
      selectedItem: function(newVal, oldVal) {
        this.item = {
          id: this.selectedItem.id,
          name: this.selectedItem.name,
          description: this.selectedItem.description,
          category: this.selectedItem.category
        }
        if(this.item.name.length > 0) {
          this.isEdit = true
        }
      },
    }
}
</script>
<template>
  <div class="demo-page">
  <main class="demo-page-content">

    <section>
      <div class="href-target" id="input-types"></div>
      <h1>Добавить предмет</h1>

      <div class="nice-form-group">
        <label>Название</label>
        <input type="text" placeholder="Название предмета" v-model="item.name" />
      </div>
      <div class="nice-form-group">
        <label>Описание</label>
        <input type="text" placeholder="Описание предмета" v-model="item.description" />
      </div>
      <div class="nice-form-group">
        <label>Категория</label>
        <input type="text" placeholder="Категория предмета" v-model="item.category" />
      </div>
      <details>
        <summary>
          <div @click="onAdd" v-if="!isEdit" class="toggle-code">Добавить</div>
          <div @click="onEdit" v-else-if="isEdit" class="toggle-code">Изменить</div>
          <div @click="onDelete" v-if="isEdit" class="toggle-code delete">Удалить</div>
        </summary>
      </details>
    </section>

  </main>
</div>
</template>

<style scoped>
.toggle-code.delete {
  margin-left: 10px;
  background-color: #ff013c;
}
section {
  background: #fff;
  padding: 2em;
  border-radius: .75rem;
  line-height: 1.6;
  overflow: hidden;
  margin-bottom: 2rem;
  position: relative;
  font-size: .875rem;
  box-shadow: 0 10px 15px -3px rgba(0,0,0,.1),0 4px 6px -2px rgba(0,0,0,.05);
  width: 400px
}
.href-target {
  position: absolute;

  top: -2em;
}
section h1{
  font-weight: 500;
  font-size: 1.25rem;
  color: #000;
  margin-bottom: .75rem;
}
section p {
  margin: .5rem 0 1.5rem;
}
.nice-form-group {
  --nf-input-size: 1rem;
  --nf-input-border-color: #c0c4c9;
  --nf-input-border-width: 1px;
  --nf-input-border-style: solid;
  --nf-input-border-bottom-width: 2px;
  --nf-input-background-color: #f9fafb;
  margin-top: calc(var(--nf-input-size)*1.5);
  line-height: 1;
  white-space: nowrap;
}
.nice-form-group > label {
  font-weight: var(--nf-label-font-weight);
  display: block;
  color: var(--nf-label-color);
  font-size: var(--nf-label-font-size);
  font-family: var(--nf-label-font-family);
  margin-bottom: calc(var(--nf-input-size)/2);
  white-space: normal;
}
.nice-form-group > input {
  background: var(--nf-input-background-color);
  font-family: inherit;
  font-size: var(--nf-input-font-size);
  border-bottom-width: var(--nf-input-border-width);
  font-family: var(--nf-input-font-family);
  box-shadow: none;
  border-radius: var(--nf-input-border-radius);
  border: var(--nf-input-border-width) var(--nf-input-border-style) var(--nf-input-border-color);
  border-bottom: var(--nf-input-border-bottom-width) var(--nf-input-border-style) var(--nf-input-border-color);
  color: var(--nf-input-color);
  width: 90%;
  padding: calc(var(--nf-input-size)*0.75);
  line-height: normal;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  transition: all .15s ease-out;
  --icon-padding: calc(var(--nf-input-size)*2.25);
  --icon-background-offset: calc(var(--nf-input-size)*0.75);
}
.details {
  background: #f1f1f1;
  margin: 2em -2em -2em;
  padding: 1.5em 2em;
}
details summary {
  outline: none;
  list-style-type: none;
}
details .toggle-code {
  display: inline-block;
  padding: .5em 1em;
  border-radius: 5px;
  font-size: .875rem;
  background: #10b981;
  color: #fff;
  font-weight: 500;
  -webkit-user-select: none;
  user-select: none;
  cursor: pointer;
  margin-top: 20px;
}
</style>
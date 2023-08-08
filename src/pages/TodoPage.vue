<template>
  <q-page class="todo-page q-px-md">
    <div class="q-pa-sm text-h2">
      <q-input v-model="text" label="Добавить туду" @keyup.esc="clear" @keyup.enter="addTodo">
        <template v-slot:append>
          <q-btn v-if="text.length" round dense flat icon="clear" @click="clear"  />
        </template>
        <template v-slot:after>
          <q-btn round dense flat icon="add" @click="addTodo"/>
        </template>
      </q-input>

    </div>
    <h1>Todo</h1>
    <q-list separator bordered>

      <q-item
        v-for="(item, i) in todoList"
        :key="item.name"
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox  v-model="item.value" color="teal" />
        </q-item-section>
        <q-item-section  @click="item.value = !item.value">
          <q-item-label
            :class="{ 'done': item.value }"
          >
           {{ item.name }}
          </q-item-label>
        </q-item-section>
        <div v-if="item.value">
          <q-btn @click="deleteTodo(i)"  dense flat icon="delete" rounded />
        </div>
      </q-item>
      <div v-if="todoList.length === 0">Добавьте новую туду</div>
    </q-list>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      todoList: [],
      text: '',
    }
  },
  methods: {
    addTodo() {
      if (this.text.length) {
        this.todoList.push({ name: this.text, value: false })
        this.clear();
      }
    },
    clear() {
      this.text = '';
    },
    deleteTodo(i) {
      this.$q.dialog({
        title: 'Удалить тудушку?',
        message: 'Вы уверены, что хотите удалить тудушку?',
        cancel: true,
        persistent: true,
      }).onOk(() => {
        this.$q.notify({
          position: 'top',
          message: `Тудушка удалена ${this.todoList[i].name}`
        })
        this.todoList.splice(i, 1);
      })
    }
  },
}
</script>

<style>
.done {
  text-decoration: line-through;
}
</style>

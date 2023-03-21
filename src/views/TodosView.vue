<script setup>
import { ref } from 'vue';
import { uid } from 'uid';
import TodoCreator from '../components/TodoCreator.vue';
import TodoItem from '../components/TodoItem.vue';
import { Icon } from '@iconify/vue';
const todoList = ref([])

const createTodo = (todo) => {
  // Usando o .value. por estar usando o ref, caso contrário seria só o nome do objeto + propriedade
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null
  })
}

const toggleTodoComplete = (todoPosition) => {
  todoList.value[todoPosition].isCompleted = !todoList.value[todoPosition].isCompleted;
}

const toggleEdit = (todoPosition) => {
  todoList.value[todoPosition].isEditing = !todoList.value[todoPosition].isEditing;
}

const updateTodo = (todoValue, todoPosition) => {
  todoList.value[todoPosition].todo = todoValue
}

const deleteTodo = (todoId) => {
  todoList.value = todoList.value.filter(todo => todo.id !== todoId)
}

</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <!-- A partir do momento que é criado um emit, ele fica acessível
            à outros componentes onde esse componente emissor está usando o @ + nome do emissor
            Essa função recebe o dado emitido
          -->
    <TodoCreator @create-todo="createTodo" />

    <ul class="todo-list" v-if="todoList.length > 0">
      <!-- O v-for é uma forma de se aplicar um for no vue. 
            Neste caso o componente TodoItem vai ser renderizado uma vez
            para cada item no todoList
          -->

      <!-- 
            :todo é a forma de passar parâmetros para o componente. 
            : + nome do parâmetro = valor do parâmetro

            Também é necessário preparar o componente para aceitar parâmetros
           -->

      <TodoItem v-for="(todo, index) in todoList" :todo="todo" :index="index" @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEdit" @update-todo="updateTodo" @delete-todo="deleteTodo"/>
    </ul>

    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" width="22" />
      <span>Você não criou nenhuma tarefa. Crie uma!</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>

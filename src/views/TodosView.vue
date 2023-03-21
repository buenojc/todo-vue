<script setup>
import { ref, watch, computed } from 'vue';
import { uid } from 'uid';
import TodoCreator from '../components/TodoCreator.vue';
import TodoItem from '../components/TodoItem.vue';
import { Icon } from '@iconify/vue';
const todoList = ref([])

/* 
  o watch serve para monitorar mudanças em um dado js, e executar um callback a toda a mudança
  que ocorrer. Tem a mesma ideia do useEffect no react.
*/

watch(todoList, () => {
  setTodoListLocalStorage()
}, {

  /* 
    A propriedade deep serve para indicar que o monitoramento deve ocorrer em todos os itens
    dentro da variável que passamos. Se não deixar como true, ele só vai pegar o primeiro nível
  */
  deep: true
})

/***
 * Uma computed property é uma forma de acessar dados e usar lógicas mais complexas
 * É uma forma de alterar e manipular dados visuais. Não deve ser usado para alterar dados de fato, para isso usar metodos
 * Neste caso, a ideia é checar quando todos as tarefas forem realizadas e assim 
 * mandar uma mensagem. O computed (importado do vue) recebe um callback (chamado de getter)
 * 
 * uma vantagem do computed property é o fato de verificar todos os elementos
 * dentro do dado que deve ser verificado, sem a necessidade de adicionar um propriedade deep
 */
const todosCompleted = computed(() => {
  /**
   * Esse every vai percorrer todo o array e checar se está completo.
   * Só vai retornar true se TODOS os itens estiverem com a propriedade true
   */
  return todoList.value.every((todo) => todo.isCompleted)
})

const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem('todoList'))
  todoList.value  = savedTodoList ? savedTodoList : [];
}

const setTodoListLocalStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value))
}

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


fetchTodoList()

</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <!-- A partir do momento que é criado um emit, ele fica acessível
            à outros componentes onde esse componente emissor está usando o @ + nome do emissor
            Essa função recebe o dado emitido
          -->
    <TodoCreator @create-todo="createTodo" />

    <p v-if="todosCompleted && todoList.length > 0" class="msg-completed">
      <Icon icon="noto-v1:party-popper" width="22" />
      <span>Tudo completo</span>
    </p>

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

  .msg-completed{
    text-align: center;
    margin-top: 30px;

    svg{
      margin-right: 5px;
    }
  }
}
</style>


<script setup>
import { ref, reactive } from 'vue';
import TodoButton from './TodoButton.vue';
//O ref é uma forma de fazer com o que o dado seja reativo, ou seja
// ele atualiza o conteúdo de acordo com as alterações
// Para acessar o valor usando o ref no script, é necessário usar o value (todo.value)

const todo = ref('');

// O reactive funciona da mesma maneira que o ref, mas não aceita dados
// primitivos, sendo necessário criar um objeto e passar como propriedades

const todoState = reactive({
    todo: '',
    invalid: null,
    errorMsg: "",
});

// Emit é como é chamado o envio de dados de um componente para o outro.
// neste caso emitimos o dado todo para a view
const emit = defineEmits(['create-todo']);

const createTodo = () => {
    todoState.invalid = null;

    //Valida se o todo não está vazio
    if (todoState.todo !== '') {
        // Usando o defineEmits, ele retorna uma função, que pode ser chamada
        // para emitir o atributo customizado criado na declaração do emit

        // Cada vez que a função rodar, será emitida o create-todo do componente
        // O segundo parâmetro é o valor a ser emitido (pode ser um objeto)
        emit('create-todo', todoState.todo)
        todoState.todo = '';
        return;
    }

    todoState.invalid = true
    todoState.errorMsg = 'TODO deve ser preenchido'
}

</script>

<template>
    <!-- Class binding: usando :class é possível condicionar o uso de uma classe caso algo seja verdadeiro
                Neste caso, a classe input-err só vai ser aplicada caso totalState.invalid seja verdadeiro
            -->
    <div class="input-wrap" :class="{ 'input-err': todoState.invalid }">

        <!-- v-model é a forma de linkar o conteúdo do elemento com uma variável,
                    no caso o todo criado na tag script  -->
        <input type="text" v-model="todoState.todo" />

        <!-- @ é um shorthand que escuta evento do dom -->
        <!-- <button @click="createTodo">Create</button> -->

        <TodoButton @click="createTodo" />
    </div>

    <!-- v-if recebe true ou false e sendo true mostra o elemento 
                    v-show faz a mesma coisa, a diferença é que v-if não renderiza o elemento a menos que seja true
                    O v-show renderiza e aplica um display:none

                    O v-if é mais caro, já que fica sendo criado e destruido. Em regra geral, se é algo que vai ser
                    alterado muito, é melhor usar v-show já que ele não é destruido
                -->
    <p v-show="todoState.invalid" class="error-msg">{{ todoState.errorMsg }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
    display: flex;
    transition: 250ms ease;
    border: 2px solid #41b080;

    &.input-err{
        border-color: red;
    }

    a:focus-within {
        box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
            0 -2px 4px -2px rbg(0 0 0 / 0.1);
    }

    input {
        width: 100%;
        padding: 8px 6px;
        border: none;

        a:focus {
            outline: none;
        }
    }

    
}

.error-msg{
    margin-top: 6px;
    font-size: 12px;
    text-align: center;
    color: red;
}
</style>
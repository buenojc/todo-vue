
<script setup>
    import { ref, reactive, defineEmits } from 'vue';
    //O ref é uma forma de fazer com o que o dado seja reativo, ou seja
    // ele atualiza o conteúdo de acordo com as alterações
    // Para acessar o valor usando o ref no script, é necessário usar o value (todo.value)
    
    const todo =  ref('');
    
    // O reactive funciona da mesma maneira que o ref, mas não aceita dados
    // primitivos, sendo necessário criar um objeto e passar como propriedades
    
    const todoState= reactive({
        todo: '',
        invalid: null,
        errorMsg: "",
    });

    // Emit é como é chamado o envio de dados de um componente para o outro.
    // neste caso emitimos o dado todo para a view
    const emit = defineEmits(['create-todo']);

    const createTodo = () => {
        // Usando o defineEmits, ele retorna uma função, que pode ser chamada
        // para emitir o atributo customizado criado na declaração do emit

        // Cada vez que a função rodar, será emitida o create-todo do componente
        // O segundo parâmetro é o valor a ser emitido (pode ser um objeto)
        emit('create-todo', todoState.todo)
    }

</script>

<template>
    <div class="input-wrap">
        <!-- v-model é a forma de linkar o conteúdo do elemento com uma variável,
        no caso o todo criado na tag script  -->
        <input type="text" v-model="todoState.todo" />
        <!-- @ é um shorthand que escuta evento do dom -->
        <button @click="createTodo">Create</button>
    </div>
</template>

<style lang="scss" scoped>
    .input-wrap{
        display: flex;
        transition: 250ms ease;
        border: 2px solid #41b080;

        a:focus-within{
            box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
            0 -2px 4px -2px rbg(0 0 0 / 0.1);
        }

        input{
            width: 100%;
            padding: 8px 6px;
            border: none;

            a:focus{
                outline: none;
            }
        }

        button{
            padding: 8px 16px;
            border: none;
        }
    }

</style>
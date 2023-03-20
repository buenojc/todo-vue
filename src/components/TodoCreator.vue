
<script setup>
    import { ref, reactive } from 'vue';
    //O ref é uma forma de fazer com o que o dado seja reativo, ou seja
    // ele atualiza o conteúdo de acordo com as alterações
    // Para acessar o valor usando o ref no script, é necessário usar o value (todo.value)
    
    const todo=  ref('');

    // O reactive funciona da mesma maneira que o ref, mas não aceita dados
    // primitivos, sendo necessário criar um objeto e passar como propriedades
    
    const todoState= reactive({
        todo: ''
    });

</script>

<template>
    <div class="input-wrap">
        <!-- v-model é a forma de linkar o conteúdo do elemento com uma variável,
        no caso o todo criado na tag script  -->
        <input type="text" v-model="todo" />
        <button>Create</button>
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
<script setup>

</script>

<template>
    <button>

        <!-- 
            slot é uma forma de passar dados dinamicamente para um componente
            Onde esse componente for chamado, vai ser possível adicionar um texto
            que ficará no lugar do slot

            Adicionar valores dentro do slot faz com que esse valor fique como default
            caso nada seja atribuido no componente em que o botão for chamado. Caso haja
            conteúdo, ele ficará no lugar do que foi previamente adicionado ao slot
        -->
        <slot>Adicionar</slot>
    </button>
</template>


<style lang="scss" scoped>
button {
    padding: 8px 16px;
    border: none;
    cursor: pointer;
}
</style>
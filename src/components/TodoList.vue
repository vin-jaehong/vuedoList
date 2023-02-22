<template>
    <div class="todo-list">
        <TodoItem v-for="todoData,i of todoList.list.slice(0, todoListPagingSize * todoListPagingCount)" :key="i" :todoData="todoData"></TodoItem>
    </div>

    <EditTodoModal :isOpenEditTodoModal="isOpenEditTodoModal" :todoList="todoList" :currentEditTodoDataCode="currentEditTodoDataCode"></EditTodoModal>
</template>

<script setup>
    import { defineProps } from 'vue';
    import TodoItem from './TodoItem.vue';
    import EditTodoModal from './EditTodoModal.vue';

    const props = defineProps({
        todoList: Object,
        isOpenEditTodoModal: Boolean,
        currentEditTodoDataCode: String,
        todoListPagingCount: Number,
        todoListPagingSize: Number,
    });
    props;

    console.log(props.todoListPagingCount);
    console.log(props.todoListPagingSize);
</script>

<style>
    .todo-list {
        width: 1000px;
        margin-top: 10px;
        margin-left: 50%;
        transform: translateX(-50%);
        padding: 10px;
    }
    @media (max-width:1420px) {
        .todo-list {
            width: 80%;
        }
    }
</style>
<template>
    <div class="todo-item" :style="`background-color : ${todoData.isChecked?'rgba(0,0,0,0.2)':''}`">
        <table style="width: 100%; margin-top:0.5%;">
            <tr>
                <td width="10%"><input type="checkbox" :checked="todoData.isChecked" @change="toggleTodoChecked($event.target.checked)"></td>
                <td witdh="60%">{{ todoData.content }}</td>
                <td width="20%">
                    <button @click="openEditTodoModal(todoData.code);">수정</button>
                    <button @click="deleteTodoData(todoData.code);">삭제</button>
                </td>
            </tr>
        </table>
    </div>
</template>
<script setup>
    import { defineProps, getCurrentInstance } from 'vue';
    const internalInstance = getCurrentInstance();
    const emitter = internalInstance.appContext.config.globalProperties.emitter;

    const props = defineProps({
        todoData: Object,
    });
    props;

    const deleteTodoData = (code)=> {
        emitter.emit('deleteTodoData', code);
    }

    const openEditTodoModal = (code)=> {
        emitter.emit('changeIsOpenEditTodoModal', [true, code]);
    }

    const toggleTodoChecked = (data)=> {
        const newTodoData = {...props.todoData};
        newTodoData.isChecked = data;
        emitter.emit('editTodoData', newTodoData);
    }
</script>
<style>
    .todo-item {
        margin-bottom: 5px;
        margin-left: 50%;
        transform: translateX(-50%);
        width: 950px;
        border: 1px solid #dcdcdc4f;
        border-bottom: 0px;
        background-color: #f0f0f064;
        height: 40px;
    }
    .todo-item:last-child {
        border-bottom: 1px solid #dcdcdca0;
        margin-bottom: 0;
    }
    @media (max-width:1420px) {
        .todo-item {
            width: 80%;
        }
    }
</style>
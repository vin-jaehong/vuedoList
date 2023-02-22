<template>
    <Transition name="fade">
        <div class="modal" v-if="isOpenEditTodoModal">
            <div class="modal-body">
                <h2>할 일 수정</h2>
                <div>
                    <textarea cols="100" rows="30" v-model="currentEditTodoData.content"></textarea>
                </div>
                <button @click="editTodoData">수정</button>
                <button @click="closeEditTodoModal">닫기</button>
            </div>
        </div>
    </Transition>
</template>

<script setup>
    import { defineProps, getCurrentInstance, reactive, watch } from 'vue';
    const internalInstance = getCurrentInstance();
    const emitter = internalInstance.appContext.config.globalProperties.emitter;

    const props = defineProps({
        isOpenEditTodoModal: Boolean,
        todoList: [Object],
        currentEditTodoDataCode: String,
    });

    let currentEditTodoData = reactive({});
    currentEditTodoData;

    watch(()=> props.isOpenEditTodoModal, (newData)=> {
        if (newData) {
            currentEditTodoData = props.todoList.list.find(todoData=>todoData.code===props.currentEditTodoDataCode);
        }
    });

    const closeEditTodoModal = ()=> {
        emitter.emit('changeIsOpenEditTodoModal', [false, '']);
    };

    const editTodoData = ()=> {
        emitter.emit('editTodoData', currentEditTodoData);
        closeEditTodoModal();
    }
</script>

<style>
    .modal {
        position: fixed;
        width: 100%;
        height: 100%;
        top:0; left:0;
        background-color:rgba(0,0,0,0.5);
    }
    .modal-body {
        width: 50%;
        height: 50%;
        background-color:#ffffff;
        margin: 0 auto;
        margin-top: 15%;
        padding: 1%; 
    }
    .fade-enter-from {
    opacity: 0;
    }
    .fade-enter-active {
        transition: all 1s;
    }
    .fade-enter-to {
        opacity: 1;
    }
    .fade-leave-from {
        opacity: 1;
    }
    .fade-leave-active {
        transition: all 1s;
    }
    .fade-leave-to {
        opacity: 0;
    }
</style>
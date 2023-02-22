<template>
    <div>
        <div style="position: relative;">
            <input type="text" class="input-form" placeholder="할 일을 추가해주세요" v-model="todoDataTemplate.content" @keypress.enter="addTodoData">
            <button @click="addTodoData" style="position: absolute; top: 12%; right: 15%; padding: 0.5% 2%; border: 1px solid #dcdcdca0">추가</button>
        </div>
        <TodoList :todoList="todoList" :isOpenEditTodoModal="isOpenEditTodoModal" :currentEditTodoDataCode="currentEditTodoDataCode" :todoListPagingCount="todoListPagingCount" :todoListPagingSize="todoListPagingSize"></TodoList>
        
        <button @click="clickHandlerTodoListMoreBtn" v-if="showTodoListPagingBtn">더 보기</button>
    </div>    
</template>

<script setup>
    import {reactive, onMounted, getCurrentInstance, ref} from 'vue';
    import TodoList from '../components/TodoList.vue';
    const internalInstance = getCurrentInstance();
    const emitter = internalInstance.appContext.config.globalProperties.emitter;

    //할일 목록 더보기 페이징 카운트
    const todoListPagingCount = ref(1);
    const todoListPagingSize = ref(5);
    const showTodoListPagingBtn = ref(true);

    //로컬 스토리지 키
    const localStorageKey = 'todoListKey';
    const todoDataTemplate  = reactive({
        code: '',
        content: '',
        isChecked: false,
    });

    //할 일 수정 모달 오픈 여부 및 변경 이벤트
    const isOpenEditTodoModal = ref(false);
    const currentEditTodoDataCode = ref('');
    emitter.on('changeIsOpenEditTodoModal', (data)=> {
        const [toggle, code] = data;
        isOpenEditTodoModal.value = toggle;
        currentEditTodoDataCode.value = code;
    });

    //할일 목록 초기화
    const todoList = reactive({list:[]});

    //컴포넌트 마운트 완료 후
    onMounted(() => {
        //할일 목록 갱신
        updateTodoList();
    });

    //할 일 목록 갱신 로직
    const updateTodoList = ()=> {
        todoList.list = JSON.parse(localStorage.getItem(localStorageKey)) ?? [];
    }

    //추가 버튼 클릭 시
    function addTodoData() {
        if (todoDataTemplate.content==='') return false;
        //고유 코드 값 추가
        todoDataTemplate.code = new Date().getTime().toString(36);
        //할 일 추가
        todoList.list.unshift({...todoDataTemplate});
        //로컬 스토리지에 반영
        localStorage.setItem(localStorageKey, JSON.stringify(todoList.list));
        todoDataTemplate.content = '';
    }

    //할 일 삭제 이벤트
    emitter.on('deleteTodoData', (code)=> {
        todoList.list = todoList.list.filter((todoData)=>todoData.code!==code);
        //로컬 스토리지에 반영
        localStorage.setItem(localStorageKey, JSON.stringify(todoList.list));
    })

    //할 일 수정 이벤트
    emitter.on('editTodoData', (newTodoData)=> {
        todoList.list = todoList.list.map((todoData)=> {
            if (todoData.code===newTodoData.code) {
                return newTodoData;
            }
            return todoData;
        });
        //로컬 스토리지에 반영
        localStorage.setItem(localStorageKey, JSON.stringify(todoList.list));
    });

    const clickHandlerTodoListMoreBtn = ()=> {
        todoListPagingCount.value++
        updateTodoList();
        //마지막 리스팅 일 경우 버튼 숨김 처리
        if (todoList.list.length <= todoList.list.slice(0, todoListPagingSize.value * todoListPagingCount.value).length) showTodoListPagingBtn.value = false;
    }
</script>

<style>
    .input-form {
        padding: 0 10px;
        width: 1000px;
        border: 1px solid #dcdcdca0;
        background-color: #f0f0f064;
        height: 40px;
    }
    .input-form:focus {
        outline: 1px solid #b8b8b8a0;
    }
    @media (max-width:1420px) {
        .input-form {
            width: 80%;
        }
    }
</style>

<template>
    <ul class="list-group mt-4">
        <li v-for="(todo, index) in todoList" :key="index" class="list-group-item">
            <div class="row align-items-center">
                <div class="col">
                    <span v-if="todo.isDone">
                        <del>{{ todo.activity }}</del>
                    </span>
                    <span v-else>{{ todo.activity }}</span>
                </div>
                <div class="col-auto">
                    <div class="row gx-2">
                        <div class="col">
                            <button class="btn btn-primary" @click="doneTodo(index)">Selesai</button>
                        </div>
                        <div class="col">
                            <button class="btn btn-danger" @click="deleteTodo(index)">X</button>
                        </div>
                    </div>
                </div>
            </div>
        </li>
    </ul>
</template>

<script>
export default {
    props: {
        todoList: {
            type: Array,
            default: []
        }
    },
    setup(props, { emit }) {
        const deleteTodo = (index) => {
            emit('deleteTodo', index)
        }
        const doneTodo = (index) => {
            emit('doneTodo', index)
        }


        return {
            deleteTodo, doneTodo
        }
    }
}
</script>
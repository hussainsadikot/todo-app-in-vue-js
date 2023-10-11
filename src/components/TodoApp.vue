
<template >
    <div class="container">
        <h2 class="text-center mt-5">Todo App</h2>
        <div class="row">
            <div class="col-md-9">
                <input v-model="task" type="text" class="form-control" placeholder="Do something..">
            </div>
            <div class="col-md-1 w-100 ">
                <button class="btn btn-warning rounded-0 btn-block" @click="submitTask">Add New Task</button>
            </div>
        </div>
        <table class="table table-bordered mt-3">
            <thead>
                <tr>
                    <th scope="col">Task</th>
                    <th scope="col">Status</th>
                    <th scope="col" class="text-center">#Edit</th>
                    <th scope="col" class="text-center">#Delete</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="task.id">
                    <td>
                        <div class="todo-item-left">
                            <div v-if="!task.editing" class="todo-item-label">
                                <span :class="{ 'finished': task.status === 'finished' }">{{ task.name }}
                                </span>
                            </div>
                            <input v-else="task.editing" v-model="task.name" @keyup.enter="doneEdit(task)"
                                class="todo-item-edit" type="text" id="">
                        </div>
                    </td>
                    <td style="width: 120px;">
                        <span @click="changeStatus(index)" class="pointer" :class="{
                            'text-danger': task.status === 'to-do',
                            'text-warning': task.status === 'in-progress',
                            'text-success': task.status === 'finished',
                        }">{{ task.status }}
                        </span>
                    </td>
                    <td>
                        <div v-if="!task.editing" class="text-center" @click="editTodo(task, index)">
                            <span class="fa-regular fa-pen-to-square"></span>
                        </div>
                        <div v-else="task.editing" @click="doneEdit(task)" class="text-center"><span
                                class="fa-solid fa-check"></span></div>
                    </td>
                    <td>
                        <div class="text-center" @click="deleteTask(index)">
                            <span class="fa fa-trash"></span>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
  
<script>
export default {
    name: 'TodoApp',
    props: {
        msg: String
    },
    data() {
        return {
            task: '',
            editedTask: null,
            availableStatuses: ['to-do', 'in-progress', 'finished'],
            tasks: [
                {
                    id: 1,
                    name: 'Read book',
                    status: 'to-do',
                    editing: false,
                },
                {
                    id: 2,
                    name: 'Arrange book',
                    status: 'in-progress',
                    editing: false,
                },
            ]
        }
    },
    methods: {
        submitTask() {
            console.log("clicked" + this.task)
            if (this.task.length === 0) return;
            if (this.editedTask === null) {

                this.tasks.push({
                    name: this.task,
                    status: 'to-do',
                });
            } else {
                this.tasks[this.editedTask].name = this.task;
                this.editedTask = null;
            }
            this.task = ""
        },
        deleteTask(index) {
            this.tasks.splice(index, 1)
        },
        editTask(task, index) {

            task.editing = true;
            // console.log(this.tasks[index].name)
            this.task = this.tasks[index].name;
            // this.editedTask = index
        },
        editTodo(task) {
            task.editing = true
        },
        doneEdit(task) {
            task.editing = false
        },

        changeStatus(index) {
            // console.log(this.tasks[index].name)
            let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)
            if (++newIndex > 2) newIndex = 0;
            this.tasks[index].status = this.availableStatuses[newIndex]
        },

    }

}

</script>
<style scoped>
.pointer {
    cursor: pointer;
}

.finished {
    text-decoration: line-through;
}

.todo-item-left {
    display: flex;
    align-items: center;
}

.todo-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
}

.todo-item-edit {
    font-size: 24px;

    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    outline: none;

}
</style>
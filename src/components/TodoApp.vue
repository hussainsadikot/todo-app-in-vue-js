
<template >
    <div class="container">
        <h2 class="text-center mt-5 ">Todo App</h2>
        <div class="input-container">
            <input v-model="task" type="text" class="form-control " placeholder="Do something..">
            <button type="button" class="add-button" @click="submitTask">Add New Task</button>
        </div>
        <input v-model="cutomStatus" type="text" class="form-control " placeholder="Task Status">
        <div class="filter-container">
            <label for="status-filter">Filter by Status: </label>
            <select v-model="selectedFilter" id="status-filter">
                <option value="all">All</option>
                <option v-for="status in customStatuses" :value="status">{{ status }}</option>
            </select>
        </div>
        <table class="table table-bordered mt-3">
            <thead>
                <tr class="text-center">
                    <th scope="col">Task</th>
                    <th scope="col">Status</th>
                    <th scope="col" class="text-center">#Edit</th>
                    <th scope="col" class="text-center">#Delete</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in filteredTasks" :key="task.id">
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
                        <!-- <span @click="changeStatus(index)" class="pointer" :class="{
                            'text-danger': task.status === 'to-do',
                            'text-warning': task.status === 'in-progress',
                            'text-success': task.status === 'finished',
                        }">{{ task.status }}
                        </span> -->
                        <div v-if="!task.editing" class="text-center">
                            <span @click="changeStatus(task)">{{ task.status }}</span>
                        </div>
                        <select v-else="task.editing" v-model="task.status" @blur="doneEdit(task)">
                            <option v-for="status in availableStatuses" :value="status">{{ status }}</option>
                        </select>
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
            cutomStatus: '',
            editedTask: null,
            selectedFilter: "all",
            availableStatuses: ['to-do', 'in-progress', 'finished',],
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
    computed: {
        customStatuses() {
            // Extract custom statuses from tasks
            return [...new Set(this.tasks.map((task) => task.status))];
        },
        filteredTasks() {
            if (this.selectedFilter === "all") {
                return this.tasks;
            } else {
                return this.tasks.filter((task) => task.status === this.selectedFilter);
            }
        },
    },
    methods: {
        submitTask() {
            console.log("clicked" + this.task)
            if (this.task.length === 0) return;
            let status = this.cutomStatus !== "" ? this.cutomStatus : this.availableStatuses[0];

            if (this.editedTask === null) {
                if (this.cutomStatus !== "" && !this.availableStatuses.includes(this.cutomStatus)) {
                    this.availableStatuses.push(this.cutomStatus)
                    console.log(this.availableStatuses)
                }
                this.tasks.push({
                    name: this.task,
                    status: status,
                });
            } else {
                this.tasks[this.editedTask].name = this.task;
                this.tasks[this.editedTask].status = status;
                this.editedTask = null;
            }
            this.task = ""
            this.cutomStatus = ""
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
            // let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)
            // if (++newIndex > 3) newIndex = 0;
            // this.tasks[index].status = this.availableStatuses[newIndex]
            const currentIndex = this.availableStatuses.indexOf(task.status);
            const newIndex = (currentIndex + 1) % this.availableStatuses.length;
            task.status = this.availableStatuses[newIndex];
        },

    }

}

</script>
<style scoped>
.input-container {
    display: flex;
    align-items: center;
    /* Vertically align content */
    gap: 10px;
    /* Add a 10px space between elements */

}

.add-button {
    padding: 5px 10px;
    background-color: #007bff;
    height: inherit;
    color: #fff;
    border: none;
    cursor: pointer;
    border-radius: 4px;
    font-weight: bold;
    font-size: 12px;
}

.add-button:hover {
    background-color: #0056b3;
}

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
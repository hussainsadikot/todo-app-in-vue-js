
<template style="width:100%">
    <div class="container" style="width: 100%;">
        <h2 class="text-center mt-5">Todo App</h2>
        <div class="row">
            <div class="col-md-8">
                <input v-model="task" type="text" name="todo" class="form-control" placeholder="Do something..">
            </div>
            <div class="col-md-4">
                <button class="btn btn-warning rounded-0 btn-block" @click="submitTask">SUBMIT</button>
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
                <tr v-for="(task, index) in tasks" :key="index">
                    <td><span :class="{ 'finished': task.status === 'finished' }"
                        >{{ task.name }}</span></td>
                    <td style="width: 120px;">
                        <span @click="changeStatus(index)" class="pointer" :class="{
                            'text-danger': task.status === 'to-do',
                            'text-warning': task.status === 'in-progress',
                            'text-success': task.status === 'finished',
                        }">{{ task.status }}</span>
                    </td>
                    <td>
                        <div class="text-center" @click="editTask(index)">
                            <span class="fa fa-pen"></span>
                        </div>
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
                    name: 'Read book',
                    status: 'to-do'
                },
                {
                    name: 'Arrange book',
                    status: 'in-progress'
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
        editTask(index) {
            // console.log(this.tasks[index].name)
            this.task = this.tasks[index].name;
            this.editedTask = index
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

@media (min-width: 1024px) {
    .container {
        max-width: 100%;
        /* Reset max-width for screens wider than 1023px */
    }
}
</style>
<template>
    <div>
        <h1>List</h1>
        <div  class="row">
            <div class="col s6">
                <select ref="select" v-model="filter">
                    <option value="" disabled selected>Choose your status</option>
                    <option value="active">Active</option>
                    <option value="outdated">Outdated</option>
                    <option value="completed">Completed</option>
                </select>
                <label>Select Filter</label>
            </div>

            <button v-if="filter" class="btn btn-small red" @click="filter = null">Clear filter</button>
        </div>

        <hr>

        <table v-if="tasks.length">
            <thead>
                <tr>
                    <th>#</th>
                    <th>Title</th>
                    <th>Date</th>
                    <th>Description</th>
                    <th>Status</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr 
                    v-for="(task, idx) of displayTasks"
                    :key="task.id"
                >
                    <td>{{ idx + 1 }}</td>
                    <td>{{ task.title }}</td>
                    <td>{{ new Date(task.date).toLocaleDateString() }}</td>
                    <td class="text-change">
                        <div class="text">
                            {{ task.description }} 
                        </div>
                    </td>
                    <td>{{ task.status }}</td>
                    <td>
                        <router-link tag="button" class="btn" :to="'/task/' + task.id" v-if="task.status !== 'completed' && task.status !== 'outdated'">
                            Open
                        </router-link>
                        <button class="btn red" v-else>Deleted</button>
                    </td>
                </tr>
            </tbody>
        </table>
        <p v-else>No tasks</p>
    </div>
</template>

<script>
export default {
    data: () => ({
        filter: null,
    }),
    computed: {
        tasks() {
            return this.$store.getters.tasks
        },
        displayTasks(){
            return this.tasks.filter(t => {
                if (!this.filter) {
                    return true
                }else{
                    return t.status === this.filter
                }
                
            })
        }
    },
    mounted() {
        M.FormSelect.init(this.$refs.select);
    }
}
</script>

<style scoped>
    .text{
        white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;
    }

    .text-change{
        max-width: 400px;
    }
</style>
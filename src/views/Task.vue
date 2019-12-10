<template>
    <div class="row">
        <div class="col s6 offset-s3">
            <h1>{{ task.title }}</h1>
            <hr>
            <div v-if="task" class="taskBlock">
                <form @submit.prevent="submitHandler">
                    <div class="chips" ref="chips"></div>

                    <div class="input-field">
                    <textarea v-model="description" id="description" class="materialize-textarea"></textarea>
                    <label for="description">Description</label>
                    <span class="character-counter" style="float: right; font-size: 12px;">{{ description.length }}/2048</span>
                    </div>
                    
                    <input type="text" ref="datepicker">
                    
                    <div>
                        <button class="btn" type="submit">Save and comeback</button>
                        <button class="btn blue" style="margin-left: 5%" @click="completedTask" type="submit">Complited task</button>
                    </div>
                </form>
            </div>
            <p v-else> Task not found </p>
        </div>
    </div>
</template>

<script>
export default {
    data: () => ({
        description: '',
        chips: null,
        date: null,
    }),

    mounted() {
        this.description = this.task.description;

        this.chips = M.Chips.init(this.$refs.chips, {
            placeholder: 'Task tags',
            data: this.task.tags
        });

        this.date = M.Datepicker.init(this.$refs.datepicker, {
            format: 'dd.mm.yyyy',
            defaultDate: new Date(this.task.date),
            setDefaultDate: true,
        });
        setTimeout(() => {
            M.updateTextFields()
        }, 0)
    },

    computed: {
        task() {
            return this.$store.getters.taskById(+this.$route.params.id)
        }
    },

    methods: {
        submitHandler() {
            
            this.$store.dispatch('updateTask', {
                id: this.task.id,
                description: this.description,
                date: this.date.date
            })
            this.$router.push('/list')
        },

        completedTask(){
            this.$store.dispatch('completedTask', this.task.id)
            this.$router.push('/list')
        },

        destroyed() {
            if ( this.date && this.date.destroyed) {
                this.date.destroyed()
            }

            if ( this.chips && this.chips.destroyed) {
                this.chips.destroyed()
            }
        }
    }
}
</script>

<style scoped>

</style>
<template>
    <div class="container my-5">
        <div class="row justify-content-center">
            <div class="col-8">
                <div class="text-center mb-3">
                    <h1>Edit Transaction</h1>
                </div>
                <router-link :to="{name:'transaction.index'}"
                class="btn btn-primary rounded shadow mb-3"
                >Back</router-link>

                <div class="card rounded shadow">
                    <div class="card-header">
                        <h5>Update Transaction</h5>
                    </div>
                    <div class="card-body">
                        <form @submit.prevent="update()">
                            <div class="form-group mb-3">
                                <label for="title">Title</label>
                                <input type="text" class="form-control" name="title" v-model="transaction.title">
                                <div class="text-danger" v-if="validation.title">
                                    {{validation.title[0]}}
                                </div>
                            </div>
                            <div class="form-group mb-3">
                                <label for="amount">Amount</label>
                                <input type="number" class="form-control" name="amount" v-model="transaction.amount">
                                <div class="text-danger" v-if="validation.amount">
                                    {{validation.amount[0]}}
                                </div>
                            </div>
                            <div class="form-group mb-3">
                                <label for="amount">Date</label>
                                <input type="text" class="form-control" name="date" v-model="transaction.time" placeholder="yyy-mm-dd hh:mm:ss">
                                <div class="text-danger" v-if="validation.time">
                                    {{validation.time[0]}}
                                </div>
                            </div>
                            <div class="form-group mb-3">
                                <label for="title">Type</label>
                                <select name="type" class="form-select" v-model="transaction.type">
                                    <option value="revenue">revenue</option>
                                    <option value="expense">expense</option>
                                </select>
                                <div class="text-danger" v-if="validation.type">
                                    {{validation.type[0]}}
                                </div>
                            </div>
                            <button class="btn btn-outline-primary">Submit</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, ref, onMounted } from "vue"
import { useRoute, useRouter } from "vue-router"
import axios from "axios"
export default {
    setup(){
        //data binding
        const transaction = reactive({
            title:'',
            amount:'',
            time:'',
            type:''
        });

        const validation = ref([]);

        const router = useRouter();
        const route = useRoute();

        onMounted(() => {
            axios.get(`http://127.0.0.1:8000/api/transaction/${route.params.id}`)
            .then((result) => {
                transaction.title = result.data.data.title
                transaction.amount = result.data.data.amount
                transaction.time = result.data.data.time
                transaction.type = result.data.data.type
            }).catch((err) => {
                console.log(err.response.data)
            });
        })

        function update() {
            axios.put(`http://127.0.0.1:8000/api/transaction/${route.params.id}`,
            transaction)
            .then(() => {
                router.push({
                    name:'transaction.index'
                })
            }).catch((err) => {
                validation.value = err.response.data
            });
        }

        return {
            transaction,
            validation,
            router,
            update
        }
    }
    
}
</script>
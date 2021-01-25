<template>
    <div class="container my-5">
        <div class="row justify-content-center">
            <div class="col-8">
                <div class="text-center mb-3">
                    <h1>Create New Transaction</h1>
                </div>
                <router-link :to="{name:'transaction.index'}"
                class="btn btn-primary rounded shadow mb-3"
                >Back</router-link>

                <div class="card rounded shadow">
                    <div class="card-header">
                        <h5>New Transaction</h5>
                    </div>
                    <div class="card-body">
                        <form @submit.prevent="store()">
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
import { pushScopeId, reactive, ref } from "vue"
import { useRoute, useRouter } from "vue-router"
import axios from "axios"
export default {
    setup(){
        //data binding
        const currentDate = new Date().toLocaleString()
        const transaction = reactive({
            title:'',
            amount:'',
            time:currentDate,
            type:''
        });

        const validation = ref([]);

        const router = useRouter();

        function store() {
            axios.post('http://127.0.0.1:8000/api/transaction',transaction)
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
            store
        }
    }
    
}
</script>
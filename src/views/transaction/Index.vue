<template>
    <div class="container my-5">
        <div class="row justify-content-center">
            <div class="col-8">
                <div class="text-center mb-3">
                    <h1>Transaction List</h1>
                </div>
                <router-link :to="{name:'transaction.create'}"
                class="btn btn-primary rounded shadow mb-3"
                >Add Transaction</router-link>

                <div class="card rounded shadow">
                    <div class="card-header">
                        <h5>Transaction List</h5>
                    </div>
                    <div class="card-body">
                        <table class="table table-hover transaction-table">
                            <thead>
                                <tr>
                                    <th>#</th>
                                    <th>Title</th>
                                    <th>Amount</th>
                                    <th>Date</th>
                                    <th>Type</th>
                                    <th>Action</th>
                                </tr>
                            </thead>
                             <tbody>
                                <tr v-for="(transaction,index) in transactions.data" :key="index">
                                    <td>{{index+1}}</td>
                                    <td>{{transaction.title}}</td>
                                    <td>{{transaction.amount}}</td>
                                    <td>{{transaction.time}}</td>
                                    <td>{{transaction.type}}</td>
                                    <td>
                                        <router-link :to="{name:'transaction.edit', params:{id:transaction.id}}"
                                        class="btn btn-sm btn-outline-info me-2"
                                        >Edit</router-link>
                                        <button class="btn btn-sm btn-outline-danger" type="button" @click.prevent = "destroy(transaction.id, index)">Delete</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { onMounted, ref } from "vue";

export default {
    setup() {
        //reactive state
        let transactions = ref([]);


        onMounted(()=>{
            //get data from api endpoint
            axios.get('http://127.0.0.1:8000/api/transaction')
            .then((result)=> {
                transactions.value = result.data
            }).catch((err)=>{
                console.log(err.response)
            })
        });

        function destroy(id, index){
            axios.delete(`http://127.0.0.1:8000/api/transaction/${id}`)
            .then(() => {
                transactions.value.data.splice(index, 1)
            }).catch((err) => {
                console.log(err.response.data)
            });
        }
        return {
            transactions,
            destroy
        }
    },
}
</script>
<template>
    <div>
        <div class="container">
            <div class="row">
                <div class="col-md-12">
                    <table class="table table-striped">
                        <thead class="thead-dark">
                            <tr>
                                <th>Name</th>
                                <th>Address</th>
                                <th>Mobile</th>
                                <th>Email</th>
                                <th>Actions</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="customer in customers" :key="customer.id">
                                <td>{{ customer.name }}</td>
                                <td>{{ customer.address }}</td>
                                <td>{{ customer.mobile }}</td>
                                <td>{{ customer.email }}</td>
                                <td>
                                    <b-button variant="success" @click="editCustomer(customer.id)"
                                        v-b-modal.edit-customer>
                                        Edit
                                    </b-button>
                                    <b-button @click.prevent="deleteCustomer(customer.id)" variant="danger">Delete
                                    </b-button>
                                </td>

                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- Edit Modal -->
        <b-modal id="edit-customer" title="Edit Customer" hide-footer>
            <b-form @submit.prevent="handleEditCustomerForm">
                <b-form-group id="input-group-2" label="Your Name:" label-for="input-2">
                    <b-form-input id="input-2" v-model="customer.name" placeholder="Enter name" required></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-2" label="Address:" label-for="input-2">
                    <b-form-input id="input-2" v-model="customer.address" placeholder="Enter Address" required></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-2" label="Mobile:" label-for="input-2">
                    <b-form-input id="input-2" v-model="customer.mobile" placeholder="Enter Phone Number" required></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-2" label="Email:" label-for="input-2">
                    <b-form-input id="input-2" v-model="customer.email" placeholder="Enter Email" required></b-form-input>
                </b-form-group>
                <br />
                <b-button type="submit" variant="primary">Update</b-button>
                <b-button type="reset" variant="danger">Reset</b-button>
            </b-form>
        </b-modal>

    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'CustomerList',
    data() {
        return {
            customers: [],
            customer: {
                name: '',
                address: '',
                mobile: '',
                email: '',
                password: ''
            },
            editId: ''
        }
    },
    created() {
        let apiUrl = "http://localhost:8081/api/banking/getAll"
        axios.get(apiUrl)
            .then((res) => {
                this.customers = res.data.data.model
                console.log(res.data.data.model)
            })
            .catch((error) => {
                console.log(error)
            })
    },
    methods: {
        deleteCustomer(id) {
            console.log(id)
            let apiUrl = `http://localhost:8081/api/banking/deleteById/${id}`
            let indexOfCustomer = this.customers.findIndex((element) => element.id === id)
            axios.delete(apiUrl)
                .then(() => {
                    this.customers.splice(indexOfCustomer, 1)
                })
                .catch((error) => {
                    console.log(error)
                })
        },
        editCustomer(id) {
            this.editId = id
        },
        handleEditCustomerForm(){
            let apiUrl = `http://localhost:8081/api/banking/updateById/${this.editId}`
             
            axios.put(apiUrl,this.customer)
                .then(() => {
                    location.reload()
                })
                .catch(error => {
                    console.log(error)
                })
        }

    }
}
</script>

<style>
button {
    margin: 6px;
}

</style>
<template>
    <div class="container">
        <table class="table table-hover">
            <thead>
                <tr>
                    <th scope="col">ID</th>
                    <th scope="col">Name</th>
                    <th scope="col">Email</th>
                    <th scope="col">Designation</th>
                    <th scope="col">Contact</th>
                    <th scope="col">Action</th>
                </tr>
            </thead>
            <tbody>
                <tr
                    class="table-secondary"
                    v-for="contact in contacts"
                    :key="contact.id"
                >
                    <th scope="row">{{ contact.id }}</th>
                    <td>{{ contact.name }}</td>
                    <td>{{ contact.email }}</td>
                    <td>{{ contact.designation }}</td>
                    <td>{{ contact.contact_no }}</td>
                    <td>
                        <router-link
                            :to="{
                                name: 'EditContact',
                                params: { id: contact.id },
                            }"
                            class="btn btn-primary btn-sm"
                            >Edit</router-link
                        >
                    </td>
                    <td>
                        <button
                            class="btn btn-sm btn-danger"
                            @click.prevent="deleteContact(contact.id)"
                        >
                            X
                        </button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "ContactList",
    data() {
        return {
            contacts: Array,
        };
    },
    created() {
        this.getContacts();
    },
    methods: {
        async getContacts() {
            let url = "http://localhost:8000/api/contacts";
            await axios
                .get(url)
                .then((response) => {
                    this.contacts = response.data.contacts;
                    console.log(this.contacts);
                })
                .catch((error) => {
                    console.log(error);
                });
        },
        async deleteContact(id) {
            let url = `http://localhost:8000/api/delete_contact/${id}`;
            await axios
                .delete(url)
                .then((response) => {
                    if (response.data.code == 200) {
                        alert(response.data.message);
                        this.getContacts();
                    }
                })
                .catch((error) => {
                    console.log(error);
                });
        },
    },
    mounted() {
        //console.log("Contact List Component Mounted!");
    },
};
</script>

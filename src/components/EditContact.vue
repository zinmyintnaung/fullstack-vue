<template>
    <div class="container">
        <div class="row">
            <div class="col-md-6">
                <div class="alert alert-danger mt-4" v-if="errors.length">
                    <ul class="mb-0">
                        <li v-for="(error, index) in errors" :key="index">
                            {{ error }}
                        </li>
                    </ul>
                </div>

                <form @submit.prevent="updateContact()" novalidate>
                    <fieldset>
                        <div class="form-group">
                            <label class="form-label mt-4"></label>
                            <input
                                type="text"
                                class="form-control"
                                placeholder="Enter Name"
                                v-model="contact.name"
                            />
                        </div>
                        <div class="form-group">
                            <label class="form-label mt-4"></label>
                            <input
                                type="text"
                                class="form-control"
                                placeholder="Enter Email"
                                v-model="contact.email"
                            />
                        </div>
                        <div class="form-group">
                            <label class="form-label mt-4"></label>
                            <input
                                type="text"
                                class="form-control"
                                placeholder="Enter Designation"
                                v-model="contact.designation"
                            />
                        </div>
                        <div class="form-group">
                            <label class="form-label mt-4"></label>
                            <input
                                type="text"
                                class="form-control"
                                placeholder="Enter Contact Number"
                                v-model="contact.contact_no"
                            />
                        </div>
                        <button class="btn btn-primary mt-4">Update</button>
                    </fieldset>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import router from "@/routes/routes";
import axios from "axios";
export default {
    name: "EditContact",
    data() {
        return {
            contact: {},
            name: "",
            email: "",
            designation: "",
            contact_no: "",
            errors: [],
        };
    },
    created() {
        this.getContactById();
    },
    methods: {
        async getContactById() {
            let url = `http://localhost:8000/api/get_contact/${this.$route.params.id}`;
            await axios
                .get(url)
                .then((response) => {
                    this.contact = response.data.contact;
                    console.log(response);
                })
                .catch((error) => {
                    console.log(error);
                });
        },
        async updateContact() {
            this.errors = [];
            if (!this.contact.name) {
                this.errors.push("Name is required");
            }
            if (!this.contact.email) {
                this.errors.push("Email is required");
            }
            if (!this.contact.designation) {
                this.errors.push("Designation is required");
            }
            if (!this.contact.contact_no) {
                this.errors.push("Contact is required");
            }
            if (!this.errors.length) {
                let formData = new FormData();
                formData.append("name", this.contact.name);
                formData.append("email", this.contact.email);
                formData.append("designation", this.contact.designation);
                formData.append("contact_no", this.contact.contact_no);

                let url = `http://localhost:8000/api/update_contact/${this.$route.params.id}`;

                //console.log(formData);

                await axios
                    .post(url, formData)
                    .then((response) => {
                        //console.log(response);
                        if (response.status === 200) {
                            alert(response.data.message);
                            router.push("/");
                        }
                    })
                    .catch((error) => {
                        console.log(error);
                        this.errors.push(error.response);
                    });
            }
        },
    },
    mounted() {
        console.log("Edit Contact Component Mounted!");
    },
};
</script>

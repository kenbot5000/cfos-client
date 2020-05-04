<template>
    <div class="outer-container bg-secondary">
        <div class="container bg-light p-4">
            <h1 class="display-3">CFOS Arbitrary Student Database Filler</h1>
            <span class="bg-info text-dark p-2">This is just a representative of what's supposed to be an external student database. Refer to API for other operations.</span>
            <form class="mt-5">
                <div class="form-group">
                    <label for="studentno">Student No.</label>
                    <input type="text" name="studentno" class="form-control" v-model="student.student_no">
                </div>
                <div class="form-group">
                    <label for="lname">Last Name</label>
                    <input type="text" name="lname" class="form-control" v-model="student.lname">
                </div>
                <div class="form-group">
                    <label for="fname">First Name</label>
                    <input type="text" name="fname" class="form-control" v-model="student.fname">
                </div>
                <span class="text-info d-block">{{ message }}</span>
                <button type="button" class="btn btn-success" @click="addStudent">Add Student</button>
                <button type="button" class="btn btn-warning" @click="clearForm">Clear</button>
                <nuxt-link to="/dashboard"><button class="btn btn-danger">Go Back</button></nuxt-link>
            </form>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import dashboardVue from './dashboard.vue';

export default {
    layout: "noLayout",
    data() {
        return {
            message: "",
            student: {
                student_no : null,
                lname: "",
                fname: ""
            }
        }
    },
    methods : {
        async addStudent() {
            this.student.student_no = parseInt(this.student.student_no);

            try{
                const res = await axios.put("/api/student/", this.student);
                if(res.status == 201) {
                    this.message = "Created."
                }
            } catch(err) {
                console.log(err);
            }
        },
        clearForm() {
            this.student = {
                student_no : null,
                lname: "",
                fname: ""
            };
            this.message = "";
        }
    }
}
</script>

<style>
    .outer-container {
        display: flex;
        align-items: center;
        height: 100vh;
    }

    .outer-container div {
        height: 65%;
    }
</style>
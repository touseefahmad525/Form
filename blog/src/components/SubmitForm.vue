<template>
    <div class="form-container">
        <h1>Form</h1>
        <div>
            <ul class="tabs">
                <li :class="{ active: activeTab === 'details' }" @click="activeTab = 'details'">Details</li>
                <li :class="{ active: activeTab === 'storeData' }" v-on:click="activeTab = 'storeData'">Store Data</li>
            </ul>

            <div v-if="activeTab === 'details'" class="tab-content">
                <form @submit.prevent="submitForm">
                    <label for="firstName">First Name*</label>
                    <input type="text" id="firstName" placeholder="First name" v-model="form.firstName"
                        class="input-field" ref="firstName" @keyup.enter="focusNext('lastName')" autofocus> 
                    <br><br>
                    <span v-if="error.firstName" class="error">{{ error.firstName }}</span> <br><br>

                    <label for="lastName">Last Name*</label>
                    <input type="text" id="lastName" placeholder="Last name" v-model="form.lastName"
                        class="input-field" ref="lastName" @keyup.enter="focusNext('employeeID')">
                    <br><br>
                    <span v-if="error.lastName" class="error">{{ error.lastName }}</span> <br><br>

                    <label for="employeeID">Employee ID*</label>
                    <input type="text" id="employeeID" placeholder="Employee ID" v-model="form.employeeID"
                        class="input-field" ref="employeeID" @keyup.enter="focusNext('email')">
                    <br><br>
                    <span v-if="error.employeeID" class="error">{{ error.employeeID }}</span> <br><br>

                    <label for="email">Email*</label>
                    <input type="email" id="email" placeholder="Email" v-model="form.email" class="input-field"
                        ref="email" @keyup.enter="focusNext('gender')">
                    <br><br>
                    <span v-if="error.email" class="error">{{ error.email }}</span> <br><br>

                    <label for="gender">Gender</label>
                    <select id="gender" v-model="form.gender" class="input-field" ref="gender"
                        @keyup.enter="focusNext('DOB')">
                        <option value="" disabled>Select</option>
                        <option value="Male">Male</option>
                        <option value="Female">Female</option>
                    </select> <br><br>

                    <label for="DOB">D.O.B</label>
                    <input type="date" id="DOB" v-model="form.DOB" class="input-field" ref="DOB"
                        @keyup.enter="focusNext('contact')">
                    <br><br>

                    <label for="contact">Contact</label>
                    <input type="text" id="contact" placeholder="Phone No." v-model="form.contact" class="input-field"
                        ref="contact" @keyup.enter="submitForm"> 
                    <br><br>

                    <button type="submit" class="submit-btn">Submit</button>
                </form>
            </div>

            <div v-if="activeTab === 'storeData'" class="table-container">
                <table class="user-table">
                    <tr>
                        <th>First Name</th>
                        <th>Last Name</th>
                        <th>Employee ID</th>
                        <th>Email</th>
                        <th>Gender</th>
                        <th>D.O.B</th>
                        <th>Contact</th>
                        <th>Action</th>
                    </tr>
                    <tr v-for="(data, index) in storedData" :key="index">
                        <td>{{ data.firstName }}</td>
                        <td>{{ data.lastName }}</td>
                        <td>{{ data.employeeID }}</td>
                        <td>{{ data.email }}</td>
                        <td>{{ data.gender }}</td>
                        <td>{{ data.DOB }}</td>
                        <td>{{ data.contact }}</td>
                        <td>
                            <button v-on:click="deleteEntry(index)" class="delete-btn">Delete</button>
                        </td>
                    </tr>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SubmitForm',
    data() {
        return {
            activeTab: 'details',
            form: {
                firstName: '',
                lastName: '',
                employeeID: '',
                email: '',
                gender: '',
                DOB: '',
                contact: ''
            },
            error: {
                firstName: '',
                lastName: '',
                employeeID: '',
                email: ''
            },
            storedData: []
        }
    },
    created() {
        const savedData = localStorage.getItem('storedData');
        if (savedData) {
            this.storedData = JSON.parse(savedData);
        }
    },
    methods: {
        focusNext(refName) {
            this.$refs[refName].focus();
        },
        validationForm() {
            this.error = {
                firstName: '',
                lastName: '',
                employeeID: '',
                email: ''
            }
            let isValid = true;
            if (!this.form.firstName) {
                this.error.firstName = "is required...";
                return false;
            }
            if (!this.form.lastName) {
                this.error.lastName = "is required...";
                return false;
            }
            if (!this.form.employeeID) {
                this.error.employeeID = "is required";
                return false;
            }
            if (!this.form.email) {
                this.error.email = "is required";
                return false;
            }

            return isValid;
        },
        submitForm() {
            if (this.validationForm()) {
                const newUser = this.form;
                this.storedData.push(newUser);
                localStorage.setItem('storedData', JSON.stringify(this.storedData));
                this.form = {
                    firstName: '',
                    lastName: '',
                    employeeID: '',
                    email: '',
                    gender: '',
                    DOB: '',
                    contact: ''
                }
                this.$refs.firstName.focus(); // Reset focus to the first input
            }
        },
        deleteEntry(index) {
            this.storedData.splice(index, 1);
            localStorage.setItem('storedData', JSON.stringify(this.storedData));
        }
    }
}
</script>

<style scoped>
.error {
    color: red;
    font-size: 0.85em;
}

/* General container for the form */
.form-container {
    max-width: 65%;
    margin: 20px auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 10px;
    background-color: #f9f9f9;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    text-align: center;
}

form label {
    display: inline-block;
    width: 120px;
    /* Adjust width as needed */
    text-align: left;
}

/* Styling for input fields */
.input-field {
    width: 50%;
    padding: 10px;
    margin: 5px 0;
    border: 1px solid #ccc;
    border-radius: 5px;
}

/* Button styles */
.submit-btn {
    padding: 10px 20px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.submit-btn:hover {
    background-color: #4c99ec;
}

/* Table container styling */
.table-container {
    margin: 20px auto;
    width: 100%;
    overflow-x: auto;
    /* Allow horizontal scrolling if needed */
}

/* Styling for the table */
.user-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
    text-align: left;
    font-size: 16px;
}

.user-table th,
.user-table td {
    padding: 12px 15px;
    border: 1px solid #ddd;
    word-wrap: break-word;
    /* Allows text to wrap within cells */
    max-width: 150px;
    /* Set a maximum width for each cell */
}

.user-table th {
    background-color: #007bff;
    color: white;
    text-align: center;
}

.user-table tbody tr:nth-child(even) {
    background-color: #f2f2f2;
}

.user-table tbody tr:hover {
    background-color: #d1e7ff;
}

/* Tabs styling */
.tabs {
    list-style-type: none;
    padding: 0;
    margin-bottom: 20px;
    display: flex;
    justify-content: center;
}

.tabs li {
    padding: 10px 20px;
    background-color: #f0f0f0;
    margin-right: 5px;
    cursor: pointer;
    border-radius: 5px;
}

.tabs li.active {
    background-color: #007bff;
    color: white;
}

.tabs li:hover {
    background-color: #e0e0e0;
}

/* Delete button styles */
.delete-btn {
    padding: 5px 10px;
    background-color: #dc3545;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.delete-btn:hover {
    background-color: #c82333;
}
</style>
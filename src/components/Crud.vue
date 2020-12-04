<template>
    <section class="crud">
        <div class="row justify-content-center">
            <div class="col-12">
                <h3>Crud</h3>
            </div>
            <div class="col-4">
                <div class="row text-center">
                    <div class="form-group col-6">
                        <input
                            id="name"
                            type="text"
                            class="form-control"
                            name="name"
                            placeholder="Name"
                            value
                            autofocus
                            v-model="name"
                        />
                    </div>
                    <div class="form-group col-6">
                        <input
                            id="lastname"
                            type="text"
                            class="form-control"
                            name="lastname"
                            placeholder="Surename"
                            value
                            v-model="lastname"
                        />
                    </div>
                    <button type="submit" @click="createItem()" class="btn btn-purple mr-auto ml-auto">Create</button>
                    <button type="submit" @click="updateItem()" class="btn btn-outline-purple mr-auto ml-auto">Update</button>
                    <button type="submit" @click="deleteItem()" class="btn btn-outline-danger mr-auto ml-auto">Delete</button>
                </div>
            </div>
        </div>
        <div class="row justify-content-center pt-4" v-if="people.length && tempPeople.length == 0">
            <div class="col-4">
                <ul id="people">
                    <li v-for="(item, index) in people" :key="index" @click="currentItem($event, item.key)" class="people-item">
                        {{ item.name }}, {{item.lastname}}
                    </li>
                </ul>
            </div>
        </div>
        <div class="row justify-content-center pt-4" v-if="tempPeople.length">
            <div class="col-4">
                <ul id="peopleSearch">
                    <li v-for="(item, index) in tempPeople" :key="index" @click="currentItem($event, item.key)" class="people-item">
                        {{ item.name }}, {{item.lastname}}
                    </li>
                </ul>
            </div>
        </div>
        <div class="row justify-content-center pt-4" v-if="people.length">
            <div class="col-4">
                <input
                    id="searchLastname"
                    type="text"
                    class="form-control"
                    name="searchLastname"
                    placeholder="Search surename..."
                    value
                    v-on:input="search()"
                    v-model="searchLastname"
                />
            </div>
        </div>
        
    </section>
</template>

<script>
export default {
    name: "Crud",
    data() {
        return {
            name: "",
            lastname: "",
            people: [],
            tempPeople: [],
            currentPeople: -1,
            searchLastname: ""
        }
    },
    methods: {
        createItem() {
            if (this.name && this.lastname) {
                this.people.push({name: this.name, lastname: this.lastname, key: this.people.length})
                this.name = ""
                this.lastname = ""
            }
        },
        updateItem() {
            if (this.currentPeople != -1) {
                this.people[this.currentPeople].name = this.name
                this.people[this.currentPeople].lastname = this.lastname
            }
        },
        deleteItem() {
            if (this.currentPeople != -1) {
                this.people.splice(this.currentPeople, 1)
            }
            this.name = ""
            this.lastname = ""
        },
        currentItem(ev, item) {
            let peopleNode = document.querySelectorAll('[class^="people-item"]')
            peopleNode.forEach(function(child) {
                if (child.classList.contains("active-item")) {
                    child.classList.remove("active-item")
                }
            })
            
            ev.target.classList.add("active-item")
            this.currentPeople = item
            this.name = this.people[item].name
            this.lastname = this.people[item].lastname
        },
        search() {
            if (this.searchLastname) {
                let searchItem = this.searchLastname
                this.tempPeople = this.people.filter(function(value){ 
                    return value.lastname.toLowerCase().includes(searchItem.toLowerCase())
                })
            } else {
                this.tempPeople = []
            }
        }
    }
}
</script>

<style scoped>
.btn-purple {
    color: #fff;
    background-color: #6542f4;
    border-color: #6542f4;
}
.btn-outline-purple {
    color: #6542f4;
    border-color: #6542f4;
}
.counter-number {
    font-size: 5rem;
}
.error-input {
    background-color: #e3559a;
    color: #fefefe;
}
.people-item {
    font-size: 2rem;
}
.people-item:hover {
    background-color: #abadc4;
    cursor: pointer;
    border-radius: 5px;
}
.active-item {
    background-color: #66bec7;
    border-radius: 5px;
    color: #fefefe;
    font-weight: 700;
}
ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
}
</style>
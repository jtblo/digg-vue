<script setup>
import { reactive } from 'vue'
import { ref } from 'vue';



function sendData(event) {
    var n = event.target.elements[0].value
    var e = event.target.elements[1].value
    var a = event.target.elements[2].value
    var t = event.target.elements[3].value
    var requestOptions = {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({user: { name: n, email: e, address: a, telephone: t }})
    };
    fetch('http://localhost:8080/user', requestOptions)
        .then(response => response.json())
        .then(data => product.value = data);

}

const listItems = ref([]);
let currPage = 1;

async function getData(page) {
    const res = await fetch("http://localhost:8080/user/page?" + new URLSearchParams({page: page, pageSize: 10}));
    const finalRes = await res.json();
    listItems.value = finalRes;
    currPage = page;
}

async function getAll() {
    const res = await fetch("http://localhost:8080/user");
    const finalRes = await res.json();
    listItems.value = finalRes;
}

function next() {
    getData(currPage + 1)
}

function previous() {
    console.log(currPage);
    if(currPage <= 1) {
        return
    }
    getData(currPage - 1)
}

getData(currPage)
</script>

<template>

<form @submit.prevent="sendData">
<p> Name: </p>
<input v-model="name" placeholder="enter your name" />
<p> Email: </p>
<input v-model="email" placeholder="enter your email" />
<p> Address: </p>
<input v-model="address" placeholder="enter your address" />
<p> Telephone: </p>
<input v-model="telephone" placeholder="enter your telephone number" />
<p></p>
<button @click="sendData($event)">Submit</button>
</form>


<ul v-for="items in listItems">
    <li v-for="(value, key, index) in items">
        {{ value.name }}, {{value.email}}, {{value.address}}, {{value.telephone}}
    </li>
</ul>
<button @click="getData(1)">First</button>
<button @click="previous()">Previous</button>
<button @click="next()">Next</button>
<button @click="getAll()">All</button>

</template>

<script setup>
        import { ref } from 'vue';

        const itemName = ref('');
        const itemDesc = ref('');
        const itemValue = ref('');
        const isAvailable = ref(false);
        const validationMessage = ref('');
        const storedListItem = localStorage.getItem('listItem');

        var isValidNumber = true;
        var ListItem = ref([]);

        

        if (storedListItem) {
                ListItem.value = JSON.parse(storedListItem);
        }
        
        
        function addToList() {
                if (itemName.value && itemDesc.value && itemValue.value) {
                        ListItem.value.push({
                                name: itemName.value,
                                description: itemDesc.value,
                                value: itemValue.value,
                                isAvailable: isAvailable.value
                        });
                        itemName.value = '';
                        itemDesc.value = '';
                        itemValue.value = '';
                        isAvailable.value = false;
                        localStorage.setItem('listItem', JSON.stringify(ListItem.value));
                }
        }

        function sortList (filter){
                if(filter === 'asc'){
                        ListItem.value.sort((a, b) => a.value - b.value);
                } else {
                        ListItem.value.sort((a, b) => b.value - a.value);
                }
        }
        
        function validateInput() {
                validationMessage.value = ''; 
                var numberPattern = /^[0-9]+([.])?[0-9]*([0-9]+)?$/;
                var namePattern = /^[a-zA-Z]+$/;
                var descriptionPattern = /^[a-zA-Z0-9]+$/;
                if (!descriptionPattern.test(itemDesc.value)) {
                        itemDesc.value = '';
                        validationMessage.value = 'Please enter a valid description.';
                        setTimeout(() => {
                                validationMessage.value = '';
                        }, 2000);
                }
                if (!namePattern.test(itemName.value)) {
                        itemName.value = '';
                        validationMessage.value = 'Please enter a valid name.';
                        setTimeout(() => {
                                validationMessage.value = '';
                        }, 2000);
                }
                if (!numberPattern.test(itemValue.value)) {
                        itemValue.value = '';
                        isValidNumber = false;
                        validationMessage.value = 'Please enter a valid number.';
                        setTimeout(() => {
                                validationMessage.value = '';
                                isValidNumber = true;
                        }, 2000);
                }
        }
</script>

<template>
        <div class="container">
                <div :v-if="ListItem.length > 0">
                        <h3>Items in the list:</h3>
                        <table>
                                <th>Name</th>
                                <th>Value (R$)
                                        <button @click="sortList('asc')">⬆️</button>
                                        <button @click="sortList('desc')">⬇️</button>

                                </th>
                                <th>Description</th>
                                <th>Available?</th>

                                <tr v-for="item in ListItem" :key="item.name">
                                        <td>{{ item.name }}</td>
                                        <td>{{ item.value }}</td>
                                        <td>{{ item.description }}</td>
                                        <td>{{ item.isAvailable ? '✅' : '❌' }}</td>
                                </tr>
                        </table>
                </div>

                <h2>
                        Add a new item to the list!
                </h2>

                <form @submit.prevent="addToList">
                        <div class="input-group">
                                <input type="text" id="itemName" placeholder="Enter item name" v-model="itemName" />
                                <label for="itemName"> Enter item name</label>
                        </div>

                        <div class="input-group">
                                <input type="text" id="description" placeholder="Enter item description"
                                        v-model="itemDesc" />
                                <label for="description"> Enter item description</label>
                        </div>

                        <div class="input-group">
                                <input type="text" required id="value" placeholder="Enter item value"
                                        v-model="itemValue" @change="validateInput"
                                        :class="{ 'error-border': !isValidNumber }" />
                                <label for=" value"> Enter item value</label>
                                <span v-if="validationMessage" class="error-message">{{ validationMessage }}</span>
                        </div>

                        <div class="input-group">
                                <label for="isAvailable">The item is available for sale?</label>
                                <input type="checkbox" id="isAvailable" v-model="isAvailable" />
                        </div>

                        <button type="submit"> Add to the list </button>
                </form>
        </div>
</template>

<style>
        .container, .input-group {
                display: flex;
                flex-direction: column;
                position: relative;       
        }
        form {
                display: flex;
                flex-direction: row;
                gap: 1.2rem;
        }
        .container {
                gap: 3rem;       
                width: 100%;
                justify-content: center;
                align-items: center;
        }
        .input-group:not(:last-of-type) > label {
                position: absolute;
                top: 0%;
                left: 0;
                opacity: 0;
        }
        .input-group:not(:last-of-type):hover > label { 
                transform: translateY(-110%);
                transition: transform 0.2s ease-in-out;
                opacity: 1;
        }
        .error-message{
                color: red;
                font-size: 0.8rem;
                position: absolute;
                bottom: -70%;
        }
        .error-border {
                border: 1px solid red;
        }
        input {
                border: 1px solid #000;
                padding: 0.5rem;
                border-radius: 0.5rem;
                width: 100%;
        }
        th, td {
               padding: 5px 10px;
               border: 1px solid black;
                text-align: center;
        }      

        button {
                padding: 0.1rem; 
                margin-inline: .7rem;
                border-radius: 0.5rem;
                /* border: none; */
                background-color: transparent;
                color: #fff;
                cursor: pointer;
        }
        button:hover {
                background-color: #000;
        }
        button[type="submit"] {
                background-color: #000;
                color: #fff;
                border: none;
                padding: 0.5rem 1rem;
                border-radius: 0.5rem;
                cursor: pointer;
        }       
        button[type="submit"]:hover {
                border: 1px solid #fff;
        }


        @media (max-width: 768px) {
                .container {
                        gap: 1rem;
                }
                form {
                        flex-direction: column;
                }
        }
</style>
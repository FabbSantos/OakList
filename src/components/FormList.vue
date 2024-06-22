<script setup>
        import { ref } from 'vue';

        const itemName = ref('');
        const itemDesc = ref('');
        const itemValue = ref('');
        const isAvailable = ref(false);
        var isValidNumber = true;
        const validationMessage = ref('');

        var ListItem = ref([]);

        function addToList() {
                event.preventDefault();
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
                }
        }
        
        function validateInput() {
                validationMessage.value = ''; 
                var numberPattern = /^[0-9]+([.])?[0-9]*([0-9]+)?$/;
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
                <h2>
                        Add a new item to the list!
                </h2>


                <div>
                        <h3>Items in the list:</h3>
                        <!-- {{ ListItem }} -->
                        <ul>
                                <li v-for="item in ListItem" :key="item.name">
                                        <p>Item name: {{ item.name }}</p>
                                        <p>Item description: {{ item.description }}</p>
                                        <p>Item value: {{ item.value }}</p>
                                        <p>Item is available: {{ item.isAvailable }}</p>
                                </li>
                        </ul>
                </div>



                <form action="#" @submit="addToList">
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

                        <button> Add to the list </button>
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
</style>
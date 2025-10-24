<script>
    export default { 
        data() {
            return {
                items: ['keyboard', 'mouse', 'iPhone', 'macbook', 'adapter'],
                newItem: ''
            }
        },
        methods: {
            addItem() {
                if (this.newItem.trim() !== '') {
                    this.items.push(this.newItem.trim())
                    this.newItem = ''
                }
            },
            deleteItem(index) {
                this.items.splice(index, 1)
            }
        }
    }
</script>

<template>

    <h2>Shopping Cart</h2>
    <div>
        <input type="text" v-model="newItem" placeholder="Enter item">
        <button @click="addItem">Add!</button>
    </div>
    <ul>
        <li v-for="(item, index) in items" :key="index">
            {{ item }}
            <button @click="deleteItem(index)">Delete</button>
        </li>
    </ul>
</template>

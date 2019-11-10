<template>
    <div>
        <div class="container my-2">
            <div class="d-flex justify-content-between mb-3">
                <h5>Simplest TODO Logger</h5>
            </div>

            <todos>
                <tbody>
                    <tr>
                        <td> <input type="text" v-model="nextItemID" readonly> </td>
                        <td> <input type="text" v-model="todoItem.name"> </td>
                        <td> <input type="text" v-model="todoItem.description"> </td>
                        <td>
                            <button class="btn btn-sm btn-primary" @click="addItem">Add TODO</button>
                        </td>
                    </tr>
                    <tr v-for="(item, index) in items" :key="item.id">
                        <td>{{item.id}}</td>
                        <td>{{item.name}}</td>
                        <td>{{item.description}}</td>
                        <td>
                            <button @click="deleteItem(index)" class="btn btn-sm btn-danger">X</button>
                        </td>
                    </tr>
                </tbody>
            </todos>

        </div>
    </div>
</template>

<script>
import Todos from './components/Todos.vue';

export default {
    name: 'app',
    data() {
        return {
            todoItem: {
                id: null,
                name: '',
                description: ''
            },
            nextItemID: 4,
            items: [
                {id: 1, name: 'Test', description: 'Testing this out'},
                {id: 2, name: 'Another test', description: 'Another test. Hope this works'},
                {id: 3, name: 'Hello World', description: 'Simple testing'},
            ]
        }
    },
    components: {
        Todos
    },
    methods: {
        clearFields() {
            this.todoItem.name = '';
            this.todoItem.description = '';
        },
        addItem() {
            this.items.push({
                id: this.nextItemID,
                name: this.todoItem.name,
                description: this.todoItem.description,
            });
            this.nextItemID++;
            this.clearFields();
        },
        deleteItem(idx) {
            this.items.splice(idx, 1);
        }
    }
}
</script>


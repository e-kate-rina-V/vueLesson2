<!--Використайте метод для обробки події кліку, який змінює стан інших компонентів-->

<template>
    <section class="add-person">
        <form @submit.prevent="validateForm">
            <label for="name">Name: </label>
            <input v-model="newPerson.name" id="name" type="text" />
            <span v-if="errors.name">{{ errors.name }}</span>

            <section>
                <label for="age">Age: </label>
                <input v-model="newPerson.age" id="age" type="number" />
                <span v-if="errors.age">{{ errors.age }}</span>
            </section>

            <button type="submit" id="add-btn">Add Person</button>
        </form>
    </section>
</template>

<script>
export default {
    props: {
        people: Array
    },
    data() {
        return {
            newPerson: {
                name: '',
                age: ''
            },
            errors: {
                name: '',
                age: ''
            }
        };
    },
    methods: {
        // Створіть метод, який буде перевіряти введені дані у формі на відповідність деяким правилам.
        validateForm() {
            this.errors.name = '';
            this.errors.age = '';

            let valid = true;

            if (!this.newPerson.name || !/^[a-zA-Z]+$/.test(this.newPerson.name)) {
                this.errors.name = ' Only latin letters';
                valid = false;
            }

            if (!this.newPerson.age || this.newPerson.age <= 0) {
                this.errors.age = ' Only positive number';
                valid = false;
            }

            if (valid) {
                this.$emit('add-person', {
                    name: this.newPerson.name,
                    age: parseInt(this.newPerson.age)
                });
                this.newPerson.name = '';
                this.newPerson.age = '';
            }
        }
    }
};
</script>

<style>
</style>

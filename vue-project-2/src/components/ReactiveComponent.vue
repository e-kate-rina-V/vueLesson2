<template>
    <div>
        <!--Створіть реактивний об'єкт і виведіть його властивості в шаблон. Змініть властивість об'єкта і спостерігайте за змінами в DOM.-->
        <div class="reactiveObj">
            <h2>Reactive Object</h2>
            <p>Name: {{ reactiveObject.name }}</p>
            <p>Age: {{ reactiveObject.age }}</p>
            <button @click="updateReactiveObject" id="upd-btn">Update Reactive Object</button>
        </div>
        <section class="filteredList">
            <h2>Filtered People List</h2>
            <input v-model="filter" placeholder="Filter by name" id="filter-input" />
            <ul>
                <li v-for="(person, index) in filteredPeople" :key="person.name">
                    Name: {{ person.name }}, Age: {{ person.age }}
                    <button @click="removePerson(index)">x</button>
                </li>
            </ul>
        </section>
        <section class="total-info">
            <p>Total People: {{ totalPeople }}</p>
            <p>Average Age: {{ averageAge }}</p>
            <p>Oldest Person: {{ oldestPerson.name }} (Age: {{ oldestPerson.age }})</p>
        </section>

        <AddPerson :people="people" @add-person="addPerson" />
    </div>
</template>

<script>
import axios from 'axios';
import AddPerson from './AddPerson.vue';


export default {
    name: 'ReactiveComponent',
    components: {
        AddPerson
    },
    data() {
        return {
            filter: '',
            people: [
                { name: 'John Doe', age: 30 },
                { name: 'Jane Smith', age: 25 },
                { name: 'Bob Marley', age: 40 },
                { name: 'Alice Johnson', age: 35 }
            ],
            fetchedPeople: [],
            newPerson: {
                name: '',
                age: ''
            },
            errors: {
                name: '',
                age: ''
            },
            reactiveObject: {
                name: 'Kate',
                age: 20
            }
        };
    },
    computed: {
        // Створіть метод для фільтрації масиву об'єктів за деяким критерієм.
        // Створіть обчислювальну властивість для виведення даних, обчислених на основі інших реактивних даних.
        // Створіть computed property, яке комбінує дані відповіді від HTTP запиту та з рекативною змінною.
        combinedPeople() {
            const combined = [...this.people, ...this.fetchedPeople];
            return combined.filter(person =>
                person.name.toLowerCase().includes(this.filter.toLowerCase())
            );
        },
        // Створіть computed property для фільтрації списку об'єктів.
        filteredPeople() {
            return this.combinedPeople;
        },
        totalPeople() {
            return this.filteredPeople.length;
        },
        // Створіть кілька computed properties, які залежать одне від одного.
        averageAge() {
            const totalAge = this.filteredPeople.reduce((sum, person) => sum + person.age, 0);
            return (totalAge / this.totalPeople).toFixed(2);
        },
        oldestPerson() {
            return this.filteredPeople.reduce((oldest, person) => {
                return person.age > oldest.age ? person : oldest;
            }, this.filteredPeople[0] || {});
        }
    },
    methods: {
        async fetchPeople() {
            try {
                const response = await axios.get('https://api.com/people');
                this.fetchedPeople = response.data;
            } catch (error) {
                console.error('Error fetching people:', error);
            }
        },
        async fetchFilteredPeople(query) {
            try {
                const response = await axios.get(`https://api.com/people?filter=${query}`);
                this.fetchedPeople = response.data;
            } catch (error) {
                console.error('Error fetching filtered people:', error);
            }
        },
        // Створіть метод, який буде викликатися при кліку на кнопку і змінювати деякі дані в інстансі
        updateReactiveObject() {
            this.reactiveObject.name = 'Jane';
            this.reactiveObject.age = 30;
        },
        // Створіть метод, який приймає подію і використовує ії для зміни даних інстанса.
        //Використайте метод для обробки події кліку, який змінює стан інших компонентів
        addPerson(person) {
            this.people.push(person);
        },
        removePerson(index) {
            this.people.splice(index, 1);
        }
    },
    watch: {
        // Використовуйте watcher для відстеження змін однієї властивості у реактивному об’єкті та виведення повідомлення про це.
        // Створіть watcher, який реагує на зміни в computed property.
        combinedPeople: {
            handler(newValue) {
                console.log('combinedPeople changed:', newValue);
            },
            // Використовуйте deep опцію для відстеження властивостей внутрішніх об’єктів.
            deep: true,
            // Використовуйте опцію immediate для виклику watcher при ініціалізації компонента.
            immediate: true
        },
        people: {
            handler(newValue) {
                console.log('People list has changed:', newValue);
            },
            deep: true
        },
        // Використовуйте watcher для відправлення API запитів при зміні реактивних даних.
        filter(newValue) {
            console.log(`Filter changed to "${newValue}"`);
            if (newValue.length > 3) {
                this.fetchFilteredPeople(newValue);
            }
        },
        // Використовуйте watcher для відстеження змін у реактивному об’єкті та виведення повідомлення про це.
        reactiveObject: {
            handler(newValue) {
                console.log('Reactive object changed:', newValue);
            },
            deep: true
        }
    },
    created() {
        this.fetchPeople();
    }
};
</script>

<style></style>
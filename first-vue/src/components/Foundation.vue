<script>
import HelloWorld from './HelloWorld.vue'
export default {
    components: {
        HelloWorld
    },
    data() {
        return {
            author: {
                name: "John Doe",
                books: [
                    "Vue 2 - Advanced Guide",
                    "Vue 3 - Basic Guide",
                    "Vue 4 - The Mystery"
                ]
            },
            firstStyle: {
                color: "#ccc",
                backgroud: "red"
            },
            secondStyle: {
                fontSize: "30px"
            },
            items: [{
                id: 1,
                msg: "no1",
                name: "test1",
                keep: true
            }, {
                id: 2,
                msg: "no2",
                name: "test2",
                keep: false
            }, {
                id: 3,
                msg: "no3",
                name: "test3",
                keep: false
            }, {
                id: 4,
                msg: "no4",
                name: "test4",
                keep: true
            }],
            firstName: "John",
            lastName: "Doe",
            isActive: true,
            isActive2: false,
            activeColor: "blue",
            fontSize: 30,
            hasError: true,
            awesome: true,
            count: 0,
            sets: [[1, 2, 3, 4], [6, 7, 8, 9]],
            message: "",
            multmessage: "",
            checkedNames: [],
            picked: "",
            selected: "",
            options: [
                { text: "One", value: "A" },
                { text: "Two", value: "B" },
                { text: "Three", value: "C" }
            ],
            question: "",
            answer: "Questions usually contain a question mark. ;-)"
        };
    },
    watch: {
        // 每当 question 改变时，这个函数就会执行
        question(newQuestion, oldQuestion) {
            if (newQuestion.includes("?")) {
                this.getAnswer();
            }
        },
        //watch 默认是浅层的：被侦听的属性，仅在被赋新值时，才会触发回调函数——而嵌套属性的变化不会触发。如果想侦听所有嵌套的变更，你需要深层侦听器:
        someObject: {
            handler(newValue, oldValue) {
                // 注意：在嵌套的变更中，
                // 只要没有替换对象本身，
                // 那么这里的 `newValue` 和 `oldValue` 相同
            },
            deep: true
        }
    },
    computed: {
        //计算属性值会基于其响应式依赖被缓存
        // 一个计算属性的 getter
        publishedBooksMessage() {
            // `this` 指向当前组件实例
            return this.author.books.length > 0 ? "Yes" : "No";
        },
        fullname: {
            get() {
                return this.firstName + " " + this.lastName;
            },
            set(newValue) {
                [this.firstName, this.lastName] = newValue.split(" ");
            }
        },
        calssbind() {
            return {
                active: this.isActive,
                active2: this.isActive && this.isActive2
            };
        }
    },
    methods: {
        //方法调用总是会在重渲染发生时再次执行函数
        increment() {
            this.count++;
        },
        changeName() {
            this.fullname = "Liu Caster",
                this.isActive = !this.isActive,
                this.items[2].keep = !this.items[2].keep;
        },
        even(numbers) {
            return numbers.filter(number1 => number1 % 2 === 0);
        },
        greet(event) {
            alert("hello" + this.firstName);
            if (event) {
                alert(event.target.tagName);
            }
        },
        warn(message, event) {
            // 这里可以访问 DOM 原生事件
            if (event) {
                event.preventDefault();
            }
            alert(message);
        },
        async getAnswer() {
            this.answer = "Thinking...";
            try {
                const res = await fetch("https://yesno.wtf/api");
                this.answer = (await res.json()).answer;
            }
            catch (error) {
                this.answer = "Error! Could not reach the API. " + error;
            }
        }
    },
    mounted() {
        this.increment(),
            console.log(this.$refs.items);
    },
    components: { HelloWorld }
}
</script>

<template>
    <!--响应式基础-->
    <button @click="increment">{{ count }}</button>
    <!--计算属性-->
    <p>Has publish books:</p>
    <span>{{ publishedBooksMessage }}</span>
    <!--Class 与 Style 绑定-->
    <div class="static" :class="{ active: isActive, 'text-danger': hasError }"></div>
    <p :class="{ active: isActive }">{{ fullname }}</p>
    <button @click="changeName">change</button>
    <p :class="calssbind" :style="{ color: activeColor, fontSize: fontSize + 'px' }">class bind test</p>
    <p :class="calssbind" :style="[firstStyle, secondStyle]">style[] test</p>
    <!--条件渲染-->
    <button @click="awesome = !awesome">Toggle</button>
    <h1 v-if="awesome">Vue is awesome!</h1>
    <h1 v-else>Oh no 😢</h1>
    <!--列表渲染-->
    <li v-for="(item, index) in author.books">
        {{ index }} - {{ item }}
    </li>
    <li v-for="(value, key) in firstStyle">
        {{ key }} : {{ value }}
    </li>
    <span v-for="n in 10">{{ n }}</span>
    <ul>
        <!--在一个节点上时，v-if 比 v-for 的优先级更高-->
        <template v-for="item in items" :key="item.id">
            <li v-if="item.keep">{{ item.msg }}</li>
            <li v-if="item.keep">{{ item.name }}</li>
        </template>
    </ul>
    <ul v-for="numbers in sets">
        <li v-for="n in even(numbers)">{{ n }}</li>
    </ul>
    <!--事件处理-->
    <button @click="greet">Greet</button>
    <!-- 使用特殊的 $event 变量 -->
    <button @click="warn('Form cannot be submitted yet.', $event)">
        Submit
    </button>
    <!-- 使用内联箭头函数 -->
    <button @click="(event) => warn('Form cannot be submitted yet.', event)">
        Submit
    </button>
    <!--表单输入绑定-->
    <p>Message is: {{ message }}</p>
    <input v-model="message" placeholder="edit me" />
    <p>Multiline message is:</p>
    <p style="white-space: pre-line;">{{ multmessage }}</p>
    <textarea v-model="multmessage" placeholder="add multiple lines"></textarea>
    <div>Checked names: {{ checkedNames }}</div>
    <div>
        <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
        <label for="jack">Jack</label>
        <input type="checkbox" id="john" value="John" v-model="checkedNames">
        <label for="john">John</label>
        <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
        <label for="mike">Mike</label>
    </div>
    <div>Picked: {{ picked }}</div>
    <div>
        <input type="radio" id="one" value="One" v-model="picked" />
        <label for="one">One</label>
        <input type="radio" id="two" value="Two" v-model="picked" />
        <label for="two">Two</label>
    </div>
    <div>Selected: {{ selected }}</div>
    <select v-model="selected" multiple>
        <option disabled value="">Please select one</option>
        <option v-for="option in options" :value="option.value">{{ option.text }}</option>
    </select>
    <!--侦听器-->
    <p>
        Ask a yes/no question:
        <input v-model="question" />
    </p>
    <p>{{ answer }}</p>
    <!--模板引用-->
    <!--需要直接访问底层 DOM 元素,使用特殊的 ref attribute-->
    <ul>
        <li v-for="item in items" ref="items">
            {{ item.name }}
        </li>
    </ul>
    <HelloWorld></HelloWorld>
</template>

<style scoped>
.active {
    color: red;
    border: 1px solid #000;
}
</style>
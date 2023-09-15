<script setup>
import { ref } from 'vue'
import { reactive, computed } from 'vue'
import TodoItem from './TheAbout.vue'
import { watch } from 'vue'
import BlogPost from './BlogPost.vue'
import Size from './Size.vue'
import AlertBox from './AlertBox.vue'

//template syntax
const greeting = 'Hello World!'
const greeting1 = 'Hello World!'
const dynamicId = 'id'

//method // ref()
const count = ref(0)
function add(){
  count.value++
}

//computed 
const world = reactive({
  continent: 'John Doe',
  country: [
    'China',
    'Vietnam',
    'Malaysia'
  ]
})
const countryincontinent = computed(() => {
  return world.continent.length > 0 ? 'Yes' : 'No'
})

// class and style binding
const isActive = ref(true)
const color = ref('blue')
const fontSize = ref(30)

//v-for object
const book = reactive({
  title: 'Atomic Habit',
  author: 'James Clear',
  published: '2016-100-16'
})

//v-for with component and vfor&vif
const newTodoText= ref('')
const todos = ref([
  {
    id:1,
    title: 'wash dish'
  },
  {
    id:2,
    title: 'do laundry'
  }
])

let nextTodoId = 3

function addNewTodo() {
  todos.value.push({
    id: nextTodoId++,
    title : newTodoText.value
  })
  newTodoText.value = ''
}

//v-for with range
const items = ref(['one', 'two', 'three'])

//event handling
const counter = ref(0)
const name = ref('Hello World!')
function greet(event) {
  alert(`Hello ${name.value}!`)
  // `event` is the native DOM event
  if (event) {
    alert(event.target.tagName)
  }
}

//form input binding
const message = ref('')
const checked = ref(true)
const checkedNames = ref([])
const selected = ref('')

//Lazy Number Trim
const msg = ref('')
const age = ref('')
const gsm = ref('')



//watcher
const question = ref('')
const answer = ref('Please add ? to your question')

watch(question, async (newQuestion) => {
  if (newQuestion.indexOf('?') > -1) {
    answer.value = 'Thinking...'
    try {
      const res = await fetch('https://yesno.wtf/api')
      answer.value = (await res.json()).answer
    } catch (error) {
      answer.value = 'Error! Could not reach the API. ' + error
    }
  }
})

//props
const posts = ref([
  { id: 1, title: 'My journey with Vue' },
  { id: 2, title: 'Blogging with Vue' },
  { id: 3, title: 'Why Vue is so fun' }
])

//Event $emit
const size = ref([
  { id: 1, title: 'My journey with Vue' },
  { id: 2, title: 'Blogging with Vue' },
  { id: 3, title: 'Why Vue is so fun' }
]) 
const postFontSize = ref(1)





</script>

<template>
<!-- template syntax -->
<p>text interpolation</p>
<div>{{ greeting }}</div>
<br>

<p>v-html</p>
<diV v-html="greeting1"></diV>
<br>

<p>v-bind:id</p>
<div v-bind:id="dynamicId">Dynamic ID</div>
<br>

<p>Expression 10 + 20</p>
<div>{{ 10+20 }}</div>
<br>

<!-- method ref() -->
<p>method, ref()</p>
<button @click="add">{{ count }}</button>
<br>
<br>

<!-- computed -->
<p>Computed - Continent and Country</p>
<p>Countries in Asia?</p>
<span>{{ countryincontinent }}</span>
<br><br>


<!-- class and style binding -->
<p>class and style binding</p>
<div :class="{blue:true}">True will Blue</div>
<div :class="{active: isActive}">Active?</div>
<div :style="{color: color, fontSize: fontSize + 'px'}">Hello World!</div>
<br><br>

<!-- v-for with object -->
<p> v-for with object</p>
<ul>
  <li v-for = "value in book">
  {{ value }}</li>
</ul>
<br><br>

<!-- v-for with range -->
<p>v-for with range</p>
<span v-for="n in 10">{{ n }}</span>
<br><br>

<!-- v-for on template -->
<p>v-for on template</p>
<ul>
  <template v-for="item in items">
  <li>{{ item }}</li>
  <li class="divider" role="presentation"></li>
  </template>
</ul>
<br><br>

<!-- v-for & v-if -->
<p>v-for with v-if</p>
<template v-for="todo in todos">
  <li v-if="!todo.isComplete">
    {{ todo.title }}
  </li>
</template>
<br><br>

<!-- v-for with component -->
<p>v-for with component</p>
<form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">Add a todo</label><br>
    <input
      v-model="newTodoText"
      id="new-todo"
      placeholder="E.g. Feed the cat"
    />
    <button>Add</button>
  </form>
  <ul>
    <todo-item
      v-for="(todo, index) in todos"
      :key="todo.id"
      :title="todo.title"
      @remove="todos.splice(index, 1)"
    ></todo-item>
  </ul>
  <br><br>

<!-- Event Handling -->
<p>Event Handlings</p>
<button @click="counter++">Add 1</button>
<p>The button above has been clicked {{ counter }} times.</p>
<br>
<button @click="greet">Greet</button>
<br><br>

<!-- form input binding -->
<p>Form Input Binding</p>
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
<br>
<input type="checkbox" id="checkbox" v-model="checked" />
<label for="checkbox">{{ checked }}</label>
<br>
<div>Checked names: {{ checkedNames }}</div>

  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames" />
  <label for="jack">Jack</label>
 
  <input type="checkbox" id="john" value="John" v-model="checkedNames" />
  <label for="john">John</label>
 
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
  <label for="mike">Mike</label>
<br>
<span> Selected: {{ selected }}</span>
<br>
  <select v-model="selected">
    <option disabled value="">Please select one</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
<br><br>

<!-- Lazy Number Trim -->
<p>Message that syncs the input with the data: {{ msg }}</p>
  <input v-model.lazy="msg" />
  <p>Message which automatically typecast as a number: {{ age }}</p>
  <input v-model.number="age" />
  <p>Message can be trimmed automatically: {{ gsm }}</p>
  <input v-model.trim="gsm" />
  
<br><br>

<!-- watcher -->
<p>Watcher</p>
<p>
Ask a yes/no question:
<input v-model="question" />
</p>
<p>{{ answer }}</p>
<br><br>

<!-- Prop -->
<p>Props</p>
<BlogPost
  	v-for="post in posts"
	  :key="post.id"
  	:title="post.title"
></BlogPost>
<br><br>

<!-- emit -->
<p>Event $emit</p>
<div :style="{ fontSize: postFontSize + 'em' }">
    <Size
      v-for="post in size"
      :key="post.id"
      :title="post.title"
      @enlarge-text="postFontSize += 0.1"
    ></Size>
  </div>
  <br><br>

<!-- Slots -->
<p>Slots</p>
	<AlertBox>
  	Something bad happened.
	</AlertBox>


</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    align-items: center;
  }
}
</style>

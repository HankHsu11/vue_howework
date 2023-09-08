<script setup>
import { ref, reactive, onBeforeMount } from "vue"
import { v4 as uuidv4 } from "uuid"
import todoItem from "@/components/todoItem.vue"
const todoInput = ref("")
const todoList = reactive([])
const StorageKey = "auo-todoList"

const save = (key, data) => {
  localStorage.setItem(key, JSON.stringify(data))
}

const addItem = () => {
  if (todoInput.value !== "") {
    const item = {
      title: todoInput.value,
      checked: false,
      uuid: uuidv4()
    }
    todoList.unshift(item)

    save(StorageKey, todoList)

    todoInput.value = ""
  }
}

const removeItem = (title) => {
  const index = todoList.findIndex((item) => {
    return item.title === title
  })

  todoList.splice(index, 1)
  save(StorageKey, todoList)
}

onBeforeMount(() => {
  const tempData = JSON.parse(localStorage.getItem(StorageKey))
	if (tempData) {
		todoList.push(...tempData)
	}
})
</script>

<template>
  <main class="container mx-auto">
    <header class="m-2">
      <h1 class="text-6xl font-thin select-none">TODO!</h1>
      <div class="font-semibold select-none text-neutral-600">simple and stupid todo app</div>
    </header>
    <form class="px-10 py-12 bg-white shadow-sm">
      <section class="flex">
        <input type="text" placeholder="做點重要的事吧..." v-model="todoInput"
          class="w-full text-2xl focus:outline-none input-lg input input-bordered" />
        <button class="text-xl btn-lg btn btn-neutral" @click.prevent="addItem">新增</button>
      </section>
    </form>
    <section class="px-10 py-6 mt-4 bg-white">
      <ul class="">
        <todoItem @remove-item="removeItem" :todoItem="i" v-for="i in todoList"></todoItem>
      </ul>
    </section>
  </main>
</template>

<style scoped></style>

<script setup>
import {ref} from 'vue'

const todoInput = ref("")
const todoList = ref([])
const editingItem = ref(null)  
const editingText = ref("")

function addButton(){
  if( todoInput.value != "" ){
    todoList.value.push({
    text:todoInput.value,
    edit:false,
    delete:false,
    done:false
  })
    todoInput.value = ""
  }
}

function deleteButton(targetItem){
  todoList.value = todoList.value.filter(item => item !== targetItem)
}

function openEditModal(item){
  editingItem.value = item
  editingText.value = item.text
  document.querySelector("#editPrompt").showModal()
}

function updateEdit(){
  if(editingItem.value){
    editingItem.value.text = editingText.value
  }
  editingItem.value = null
  editingText.value = ""
}

</script>

<template>
  <div class="w-full px-4">
    <div class="max-w-4xl mx-auto bg-[#e7e0f0] p-6 rounded-lg shadow mt-8">
      
      <div class="flex items-center justify-center mb-6">
        <img src="./icon-book.png" alt="logo" class="w-10 h-10 mr-4" />
        <h1 class="text-3xl font-bold text-center">代辦清單 ToDo</h1>
      </div>

      <div class="flex flex-col md:flex-row items-center justify-center gap-4 mb-6">
        <input
          v-model="todoInput"
          type="text"
          placeholder="輸入文字"
          class="w-full md:w-2/3 p-3 border rounded focus:outline-none focus:border-pink-600"
        />
        <button @click="addButton" class="btn btn-primary px-6 py-3">新增</button>
      </div>

      <hr class="mb-6" />

      <div
        v-for="(item, id) in todoList"
        :key="id"
        class="border-b py-4 flex flex-col md:flex-row md:items-center md:justify-between gap-4"
        :class="{ 'bg-gray-400 text-white': item.done }"
      >

        <div class="flex items-start md:items-center gap-4 w-full md:w-auto">
          <input
            type="checkbox"
            v-model="item.done"
            class="checkbox checkbox-neutral mt-1 md:mt-0"
          />
          <p
            :class="{ 'line-through': item.done }"
            class="text-base md:text-lg"
          >
            {{ item.text }}
          </p>
        </div>

        <div class="flex gap-4 text-sm md:text-base items-center justify-end md:justify-normal">
          <button @click="openEditModal(item)" class="hover:text-indigo-500 flex items-center gap-1">
            <i class="fa-solid fa-pen"></i><span>編輯</span>
          </button>
          <button @click="deleteButton(item)" class="hover:text-pink-600 flex items-center gap-1">
            <i class="fa-solid fa-trash"></i><span>刪除</span>
          </button>
        </div>
      </div>

      <dialog id="editPrompt" class="modal">
        <div class="modal-box max-w-2xl m-auto">
          <h3 class="text-lg font-bold">編輯文字</h3>
          <input v-model="editingText" class="py-4 my-8 input w-full" type="text" />
          <div class="modal-action">
            <form method="dialog">
              <button @click="updateEdit" class="btn btn-primary mr-4">更新</button>
              <button class="btn btn-neutral">關閉</button>
            </form>
          </div>
        </div>
      </dialog>
    </div>
  </div>
</template>


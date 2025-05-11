<script setup>
// import HelloWorld from './components/HelloWorld.vue'
import {ref} from 'vue'

//邏輯整理
//按下確認按鈕，確認使用者在input欄位沒有空值
//按下按鈕完成，input欄位應該變回空值
//新增的值應該push到下面的list清單

//下方的list清單
//按完成可以呈現黃底白字或用刪除線
//按編輯：跳出視窗修改清單訊息 + 按完成：可以修改?
//按刪除：可以將清單整個從list移除

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
  <div class="bg-[#fcf1f7] w-screen h-screen ">
    <div class="max-w-5xl m-auto bg-[#FFFFFF] p-[12px]">
      <h1 class="text-3xl font-bold text-center p-[50px] ">代辦清單 ToDo</h1>
      <div class="text-center w-full">
        <input v-model="todoInput" class="w-[70%] p-[12px] border rounded-4xl mx-8 border-gray-700  focus:border-pink-600" type="text" placeholder="輸入文字"/>
        <button @click="addButton" type="button" class="btn btn-soft btn-primary p-[12px]">新增</button>
      </div>
      <div class="border-b-1 mt-20 mb-8"></div>
      <div v-for="(item, id) in todoList" :key="id" class="flex justify-center items-center w-full py-4" 
        :class="{ 'bg-gray-400 text-white': item.done }">
        <input @change="item.done = !item.done" type="checkbox" name="checkbox" id="" class="checkbox checkbox-neutral"/>
        <p class="p-[12px] w-[70%]" :class="{ 'line-through': item.done }">{{ item.text }}</p>

        <button @click="openEditModal(item)" type="button" class="group relative cursor-pointer">
          <i class="fa-solid fa-pen group-hover:text-indigo-500 mr-2 "></i>
          <span class="group-hover:text-indigo-500">編輯
          </span>
          <span class="px-4 text-black">｜</span>
        </button>
        <dialog id="editPrompt" class="modal">
          <div class="modal-box max-w-2xl m-auto">
            <h3 class="text-lg font-bold">編輯文字</h3>
            <input v-model="editingText" class="py-4 my-8 input w-full" type="text" />
            <div class="modal-action">
              <form method="dialog">
              <!-- if there is a button, it will close the modal -->
                <button @click="updateEdit" class="btn btn-soft btn-primary mr-4">更新</button>
                <button class="btn btn-soft btn-neutral">關閉</button>
              </form>
            </div>
          </div>
        </dialog>

        <button @click="deleteButton(item)" class="hover:text-pink-600 cursor-pointer flex items-center" type="button">
          <i class="fa-solid fa-trash mr-2 " type="button"></i>
          <span>刪除</span>
        </button>
      </div>
    </div>
 </div>
</template>

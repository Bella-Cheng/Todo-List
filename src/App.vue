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





function addButton(){
  if(todoInput.value != ""){
    todoList.value.push({
    text:todoInput.value,
    edit:false,
    delete:false,
    done:false
  })
    todoInput.value = ""
  }
}

</script>

<template>
  <div class="bg-[#fcf1f7] w-screen h-screen ">
    <div class="max-w-5xl m-auto bg-[#FFFFFF] p-[12px]">
      <h1 class="text-3xl font-bold text-center p-[50px] ">代辦清單 ToDo</h1>
      <div class="text-center">
        <input v-model="todoInput" class="w-[70%] p-[12px] border rounded-4xl mx-8 border-gray-700  focus:border-pink-600" type="text" placeholder="輸入事項"/>
        <button @click="addButton" type="button" class="btn btn-soft btn-primary p-[12px]">新增</button>
      </div>
      <div class="border-b-1 mt-20 mb-8"></div>
      <div v-for="(item, id) in todoList" :key="id" class="flex justify-center items-center w-full mb-8">
        <input type="checkbox" name="checkbox" id="" class="checkbox checkbox-primary"/>
        <p class="p-[12px] w-[70%]">{{ item.text }}</p>
        <button class="after:content-['｜'] after:ml-2" type="button"><i class="fa-solid fa-pen p-[12px]"></i>修改</button>
        <button><i class="fa-solid fa-trash p-[12px]" type="button"></i>刪除</button>
      </div>
    </div>
 </div>
</template>

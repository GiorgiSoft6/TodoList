<template>
   <div class="flex items-center justify-center pt-36">
      <div class="todo w-[430px] flex flex-col rounded-lg min-h-[500px] bg-white">
         <div class="w-full flex flex-col p-5 gap-4">
            <input @keyup="GetInpInfo" v-model="InpInfo" class="w-full border text-xl pl-3 border-black h-12 rounded-sm"
               type="text" placeholder="Add New Task" />
            <div class="flex items-center justify-between">
               <div class="AllH2 flex items-center gap-2">
                  <h2 class="text-lg cursor-pointer">All</h2>
                  <h2 class="text-lg cursor-pointer">Pending</h2>
                  <h2 class="text-lg cursor-pointer">Completed</h2>
               </div>
               <h2 @click="DelAll" class="text-md cursor-pointer px-2 py-1 bg-blue-500 rounded-md text-white">
                  Clear All
               </h2>
            </div>
         </div>
         <hr />
         <div v-if="AllCmpPed" class="w-full min-h-[300px] p-5 text-center bg-slate-100 flex flex-col gap-3">
            <div v-if="LocalAryy.length > 0" v-for="(item, index) in LocalAryy" :key="item"
               class="w-full h-14 flex items-center cursor-pointer border-b border-gray-400 justify-between">
               <div class="flex items-center gap-2">
                  <input type="checkbox" />
                  <h2>{{ item.title }}</h2>
               </div>
               <div class="relative">
                  <img @click="seetfunc" class="List w-6 cursor-pointer" src="./assets/li.png" alt="" />
                  <div
                     class="MenuDiv w-20 h-16 bg-gray-200 border hidden border-gray-300 rounded-sm absolute top-4 flex-col">
                     <h2 @click="EditFunc(index)" class="text-2xl font-extralight">Edit</h2>
                     <h2 @click="DelIndex(index)" class="DelDiv text-2xl font-extralight">
                        Delete
                     </h2>
                  </div>
               </div>
            </div>
            <h2 class="text-2xl" v-else>არაფერი დაგიმატებიათ</h2>
         </div>
         <div v-else class="PeddingDiv w-full h-[300px] bg-slate-100 text-center">
            <h2>ეს არის Pendding ი</h2>
         </div>
      </div>
   </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
let InpInfo = ref("");
let LocalAryy = ref([]);
let EditLet = ref(false);
let EditIndex = ref(0);
let AllCmpPed = ref(true);

onMounted(() => {
   let existingArray = JSON.parse(localStorage.getItem("myArray")) || [];
   LocalAryy.value = existingArray;
   AllH2func();
});



//წაშლა დაედითება პროდუქტის
function DelIndex(index) {
   LocalAryy.value.splice(index, 1);
   localStorage.setItem("myArray", JSON.stringify(LocalAryy.value));
}
function EditFunc(index) {
   InpInfo.value = LocalAryy.value[index].title;
   EditLet.value = true;
   EditIndex.value = index;
}



//პროდუქტის დამატება დაედითება
function GetInpInfo(e) {
   if (e.key == "Enter") {
      if (EditLet.value == true) {
         LocalAryy.value[EditIndex.value].title = InpInfo.value;
         EditLet.value = false;
         InpInfo.value = "";
         document.querySelectorAll(".MenuDiv")[EditIndex.value].classList.remove("active")
         localStorage.setItem("myArray", JSON.stringify(LocalAryy.value));
      } else {
         LocalSave();
         InpInfo.value = "";
      }
   }
   if(EditLet.value == true) {
      LocalAryy.value[EditIndex.value].title = InpInfo.value
   }
}




//პროდუქტის შექმნა და LocalStorage-ში შენახვა
function LocalSave() {
   if (InpInfo.value) {
      let newObj = {
         title: InpInfo.value,
      };
      addObjectToArray(newObj);
   }
}
function addObjectToArray(newObj) {
   let existingArray = JSON.parse(localStorage.getItem("myArray")) || [];

   existingArray.push(newObj);

   localStorage.setItem("myArray", JSON.stringify(existingArray));

   LocalAryy.value = existingArray;
}




//მენიუს ამოსვლა
function seetfunc(e) {
   document.querySelectorAll(".MenuDiv").forEach((i) => i.classList.remove("active"));
   e.target.nextElementSibling.classList.toggle("active");
}



//მთლიანი მასივის წაშლა
function DelAll() {
   LocalAryy.value = [];
   localStorage.setItem("myArray", JSON.stringify(LocalAryy.value));
}



//სხვადასხვა გვერდის ჩატვირთვა
function AllH2func() {
   let allH2 = document.querySelectorAll(".AllH2 h2");
   allH2[0].style.borderBottom = "2px solid red";
   allH2.forEach((item, index) => {
      item.addEventListener("click", () => {
         console.log(index);
         document
            .querySelectorAll(".AllH2 h2")
            .forEach((i) => (i.style.borderBottom = "none"));
         item.style.borderBottom = "2px solid red";
         if (index == 0) {
            AllCmpPed.value = true;
         } else {
            AllCmpPed.value = false;
         }
      });
   });
}
</script>

<style>
body {
   background-color: skyblue;
}

.active {
   display: flex;
}
</style>

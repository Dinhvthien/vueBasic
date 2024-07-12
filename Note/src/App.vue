<script setup>
import { onMounted, ref,watch } from 'vue';
const showmodal = ref(false);
const content = ref("");
const noti = ref("");
const notes = ref([]);
function getRandomColor() {
  // Tạo ra một màu ngẫu nhiên theo định dạng HEX
  var letters = '0123456789ABCDEF';
  var color = '#';
  for (var i = 0; i < 6; i++) {
    color += letters[Math.floor(Math.random() * 16)];
  }
  return color;
}
onMounted(() => {
  const storedNotes = localStorage.getItem('notes');
  if (storedNotes) {
    notes.value = JSON.parse(storedNotes);
  }
  console.log(notes);
})
watch(notes, (newNotes) => {
            localStorage.setItem('notes', JSON.stringify(newNotes));
        }, { deep: true });

var now = new Date();
var day = now.getDate();
var month = now.getMonth() + 1; // Tháng bắt đầu từ 0
var year = now.getFullYear();
// Định dạng ngày tháng năm theo ý muốn, ví dụ: "DD/MM/YYYY"
var formattedDate = (day < 10 ? '0' + day : day) + '/' +
  (month < 10 ? '0' + month : month) + '/' +
  year;


const addnote = () => {
  if (content.value.trim().length < 10) {
    noti.value = "Ghi chú của bạn ngắn quá"
    return
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: content.value,
    date: formattedDate,
    bgcolor: getRandomColor()
  })
  showmodal.value = false;
  content.value = "";
  noti.value = "";


}

const removenote = (id) => {
  const index = notes.value.findIndex(note => note.id === id);
  if (index !== -1) {
    notes.value.splice(index, 1);
  }
}

showmodal.value = false;
content.value = "";
noti.value = "";



</script>
<template>
  <main>
    <div class="overlay" v-if="showmodal" @click="showmodal = false">
      <div class="modal" @click.stop>
        <button class="btn-close" @click="showmodal = false">-</button>
        <textarea name="note" id="note" cols="60" rows="10" v-model="content"
          placeholder="Ghi chú của bạn là ..."></textarea>
        <div class="containoti">
          <p v-if="noti">{{ noti }}</p>
        </div>
        <button class="btn-addnote" @click="addnote()" >Add Note</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button class="btn-add" @click="showmodal = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for="item in notes" class="card" :style="{ backgroundColor: item.bgcolor }" :key="item.id">
          <button class="btn-closenote" @click="removenote(item.id)">-</button>
          <p class="main-text">{{ item.text }}
          </p>
          <p class="date">{{ item.date }}</p>
        </div>
      </div>
    </div>
  </main>
</template>


<style scoped>
main {
  height: 100vh;
  width: 100vw;

}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 50px;
}

h1 {
  font-weight: bold;
  font-size: 50px;

}

.btn-add {
  width: 50px;
  height: 50px;
  border-radius: 50px;
  cursor: pointer;
  font-weight: bold;
  font-size: 20px;
  border: none;
  background-color: rgb(21, 20, 20);
  color: white;
}

.main-text {
  width: 200px;
  /* Chiều rộng của phần tử */
  overflow: hidden;
  /* Ẩn phần tràn */
  display: -webkit-box;
  /* Sử dụng flexbox (webkit) */
  -webkit-line-clamp: 5;
  /* Số dòng hiển thị */
  -webkit-box-orient: vertical;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  position: relative;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

p {
  margin-top: 30px
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgb(00, 00, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  height: 400px;
  background-color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  position: relative;
  border: 2px solid black;
  border-radius: 20px;
  padding-top: 30px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.containoti {
  width: 85%;
  height: 30px;
}

.modal p {
  color: red;
  margin-top: 15px;

}

.btn-addnote {
  width: 90px;
  height: 60px;
  border-radius: 20px;
  cursor: pointer;
  margin-top: 0;
  background-color: aqua;
}

textarea {
  font-size: 20px;
  border: 2px solid #070707;
  padding: 10px
}

.btn-close {
  width: 30px;
  height: 30px;
  border-radius: 20px;
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
  background-color: red;
  color: #ffffff;
}

.btn-closenote {
  background-color: rgb(169, 150, 150);
  width: 20px;
  height: 20px;
  cursor: pointer;
  border-radius: 10px;
  position: absolute;
  top: 5px;
  right: 5px;
}
</style>
<script setup>
import PlaylistItem from "./PlaylistItem.vue";
import {ref, watch} from "vue";
import AddMusicForm from "./AddMusicForm.vue";

let playlist = ref([]);
let currentIndex = 0;
const props = defineProps({
  next: Array
})
const emit = defineEmits(['item']);

playlist.value.push({
  titre: "1er",
  src: "https://www.learningcontainer.com/wp-content/uploads/2020/02/Kalimba.mp3"
})
playlist.value.push({
  titre: "2eme",
  src: "http://commondatastorage.googleapis.com/codeskulptor-demos/DDR_assets/Kangaroo_MusiQue_-_The_Neverwritten_Role_Playing_Game.mp3"
})

watch(() => props.next[0], () => {
  if (playlist.value.length - 1 === currentIndex) {
    if (props.next[1]) {
      currentIndex = 0;
      emit("item", {
        titre: playlist.value[currentIndex].titre,
        src: playlist.value[currentIndex].src
      });
    }
  } else {
    currentIndex = currentIndex + 1;
    emit("item", {
      titre: playlist.value[currentIndex].titre,
      src: playlist.value[currentIndex].src
    });
  }
});

function addMusic(item) {
  console.log('test')
  playlist.value.push({
    titre: item.titre,
    src: item.src
  });
}

function removeMusic(index) {
  playlist.value.splice(index, 1);
}
function playMusic(index) {
  emit("item", {
    titre: playlist.value[index].titre,
    src: playlist.value[index].src
  });
  currentIndex = index;
}
</script>

<template>
  <AddMusicForm @add="addMusic" />
  <ol>
    <li>
      <h1>Titre</h1>
    </li>
    <PlaylistItem
        v-for="(item,index) in playlist"
        :titre="item.titre"
        :index="index"
        @remove="removeMusic"
        @play="playMusic"
    />
  </ol>
</template>

<style scoped>
ol {
  display: flex;
  flex-direction: column;
  left: 0;
  right: 0;
  padding: 0;
}

li {
  height: 40px;
  display: flex;
  flex-direction: row;
  align-items: center;
  border-bottom: 1px solid white;
  justify-content: space-between;
  padding-inline: 30px;
  padding-top: 3px;
  padding-bottom: 3px;
  color: #9a9a9a;
}
</style>
<template>
  <UApp>
    <UHeader to="/" :toggle="false">
      <template #title>
        <div class="flex items-center">
          <Icon name="game-icons:gibbet" />
          <h1>FORCA</h1>
        </div>
      </template>
    </UHeader>

    <UMain>
      <UContainer class="py-10 gap-2 flex flex-col items-center justify-center">
        <USeparator class=" w-40">
          <template #default>
            <span class="text-xs font-light text-gray-500">Tema</span>
          </template>
        </USeparator>
        <UBadge icon="bx:world" size="xl" class="px-5 rounded-full" color="primary" variant="subtle">Geral
        </UBadge>
      </UContainer>

      <UContainer class="mb-10 flex flex-col gap-5 items-center justify-center">

        <NuxtImg src="https://picsum.photos/250/360" />

        <div>
          letras q ja foram {{ jaFoi }}
        </div>


      </UContainer>

      <UContainer class="flex flex-wrap gap-2 items-center justify-center">

        <div v-for="letra in letras" key="id">
          <UButton @click="chutar(letra)" v-if="letra.status" color="primary" class="text-xl font-bold text-white
        w-8 h-8 flex items-center justify-center p-0">
            {{ letra.letra }}
          </UButton>
          <UButton v-if="!letra.status" color="tertiary" disabled="" class="text-xl font-bold text-white
        w-8 h-8 flex items-center justify-center p-0">
            {{ letra.letra }}
          </UButton>
        </div>

      </UContainer>
    </UMain>

    <USeparator icon="ion:sparkles" />
    <UFooter>
      <template #default>
      </template>

      <template #left>
        <p class="text-muted text-sm">©JoaoJ {{ new Date().getFullYear() }}</p>
      </template>



      <template #right>
        <UButton icon="i-simple-icons-instagram" color="neutral" variant="ghost"
          to="https://www.instagram.com/joaoj_pedroo/" target="_blank" aria-label="Instagram" />
        <UButton icon="i-simple-icons-github" color="neutral" variant="ghost" to="https://github.com/JoaoPedro77"
          target="_blank" aria-label="GitHub" />
      </template>
    </UFooter>
  </UApp>
</template>

<script setup>

import { ref } from 'vue';
const jaFoi = ref('')

const letras = ref([
  { id: 1, letra: "A", status: true },
  { id: 2, letra: "B", status: true },
  { id: 3, letra: "C", status: true },
  { id: 4, letra: "D", status: true },
  { id: 5, letra: "E", status: true },
  { id: 6, letra: "F", status: true },
  { id: 7, letra: "G", status: true },
  { id: 8, letra: "H", status: true },
  { id: 9, letra: "I", status: true },
  { id: 10, letra: "J", status: true },
  { id: 11, letra: "K", status: true },
  { id: 12, letra: "L", status: true },
  { id: 13, letra: "M", status: true },
  { id: 14, letra: "N", status: true },
  { id: 15, letra: "O", status: true },
  { id: 16, letra: "P", status: true },
  { id: 17, letra: "Q", status: true },
  { id: 18, letra: "R", status: true },
  { id: 19, letra: "S", status: true },
  { id: 20, letra: "T", status: true },
  { id: 21, letra: "U", status: true },
  { id: 22, letra: "V", status: true },
  { id: 23, letra: "W", status: true },
  { id: 24, letra: "X", status: true },
  { id: 25, letra: "Y", status: true },
  { id: 26, letra: "Z", status: true }
]);

function chutar(letra) {

  letra.status = !letra.status
  jaFoi.value += " " + letra.letra
}

</script>
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

    <UMain class="bg-stone-800 select-none drag">

      <UContainer class="pt-10 gap-2 flex flex-col items-center justify-center">
        <USeparator class=" w-40">
          <template #default>
            <span class="text-xs font-light text-stone-500">Tema</span>
          </template>
        </USeparator>
        <UBadge icon="bx:world" size="xl" class="px-5 rounded-full" color="primary" variant="subtle">Geral
        </UBadge>
      </UContainer>

      <UContainer class="mb-10 flex flex-col gap-5 items-center justify-center">

        <img :src="`/forca/${forcaTentativa}.png`" class="h-50 sm:h-60 pl-7" />

        <div class="flex flex-wrap gap-1">
          <div v-for="(letraPalavra, i) in palavra" key="i">
            <UBadge v-if="!chutes.some(l => letraPalavra.includes(l))" color="tertiary" variant="subtle" class="text-xl font-bold text-white
              w-8 h-8 flex items-center justify-center p-0">
            </UBadge>
            <UBadge v-if="chutes.some(l => letraPalavra.includes(l))" color="primary" variant="subtle" class="text-xl font-bold text-white
              w-8 h-8 flex items-center justify-center p-0">
              {{ letraPalavra }}
            </UBadge>
          </div>
        </div>


      </UContainer>

      <UContainer class="flex flex-wrap gap-2 items-center justify-center">

        <div v-for="letra in letras" key="id">
          <UButton @click="chutar(letra)" v-if="letra.status === 'padrao'" :disabled="perdeu || ganhou" color="primary"
            class="text-xl font-bold text-white
        w-8 h-8 flex items-center justify-center p-0">
            {{ letra.letra }}
          </UButton>
          <UButton v-if="letra.status === 'naoTem'" color="tertiary" disabled="" class="text-xl font-bold text-white
        w-8 h-8 flex items-center justify-center p-0">
            {{ letra.letra }}
          </UButton>
          <UButton v-if="letra.status === 'tem'" color="secondary" disabled="" class="text-xl font-bold text-white
        w-8 h-8 flex items-center justify-center p-0">
            {{ letra.letra }}
          </UButton>
        </div>

      </UContainer>
    </UMain>

    <USeparator class="bg-stone-800" color="tertiary" icon="fluent-emoji-flat:motor-scooter" />
    <UFooter class="bg-stone-800">
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

const palavra = ref("ANIMAÇÃO")
const letrasCorretas = ref([])

function carregarLetrasCorretas() {
  for (let p in palavra.value) {
    if (!(letrasCorretas.value.includes(palavra.value[p]))) {
      letrasCorretas.value.push(palavra.value[p])
    }
  }
}

onMounted(() => {
  carregarLetrasCorretas();
  console.log(letrasCorretas.value)
})

const acertos = ref(0)

const forcaTentativa = ref(0)
const chutes = ref([])
const perdeu = ref(false)
const ganhou = ref(false)

const letras = ref([
  { id: 1, letra: "A", status: 'padrao', alt: ['Á', 'À', 'Ã', 'Â', 'Ä'] },
  { id: 2, letra: "B", status: 'padrao', alt: [''] },
  { id: 3, letra: "C", status: 'padrao', alt: ['Ç'] },
  { id: 4, letra: "D", status: 'padrao', alt: [''] },
  { id: 5, letra: "E", status: 'padrao', alt: ['É', 'È', 'Ê', 'Ë'] },
  { id: 6, letra: "F", status: 'padrao', alt: [''] },
  { id: 7, letra: "G", status: 'padrao', alt: [''] },
  { id: 8, letra: "H", status: 'padrao', alt: [''] },
  { id: 9, letra: "I", status: 'padrao', alt: ['Í', 'Ì', 'Î', 'Ï'] },
  { id: 10, letra: "J", status: 'padrao', alt: [''] },
  { id: 11, letra: "K", status: 'padrao', alt: [''] },
  { id: 12, letra: "L", status: 'padrao', alt: [''] },
  { id: 13, letra: "M", status: 'padrao', alt: [''] },
  { id: 14, letra: "N", status: 'padrao', alt: [''] },
  { id: 15, letra: "O", status: 'padrao', alt: ['Ó', 'Ò', 'Õ', 'Ô', 'Ö'] },
  { id: 16, letra: "P", status: 'padrao', alt: [''] },
  { id: 17, letra: "Q", status: 'padrao', alt: [''] },
  { id: 18, letra: "R", status: 'padrao', alt: [''] },
  { id: 19, letra: "S", status: 'padrao', alt: [''] },
  { id: 20, letra: "T", status: 'padrao', alt: [''] },
  { id: 21, letra: "U", status: 'padrao', alt: ['Ú', 'Ù', 'Û', 'Ü'] },
  { id: 22, letra: "V", status: 'padrao', alt: [''] },
  { id: 23, letra: "W", status: 'padrao', alt: [''] },
  { id: 24, letra: "X", status: 'padrao', alt: [''] },
  { id: 25, letra: "Y", status: 'padrao', alt: [''] },
  { id: 26, letra: "Z", status: 'padrao', alt: [''] }
]);

function chutar(letra) {
  for (let p in letrasCorretas.value) {
    if (letra.letra === letrasCorretas.value[p]) {
      chutes.value.push(letra.letra)
      letra.status = "tem"
      acertos.value += 1;
      if (acertos.value === letrasCorretas.value.length) {
        ganhou.value = true
      }
    }

    for (let lc in letrasCorretas.value) {
      if (letra.alt.includes(letrasCorretas.value[lc])) {
        chutes.value.push(letrasCorretas.value[lc])
        letra.status = "tem"
        acertos.value += 1;
        console.log("acertos " + acertos.value)
        console.log("tamanho " + letrasCorretas.value.length)
        if (acertos.value === letrasCorretas.value.length) {
          ganhou.value = true
        }
        break;
      }
    }
    if (letra.status == "tem") { break }
  }

  if (letra.status != "tem") {
    letra.status = "naoTem"
    forcaTentativa.value += 1
    if (forcaTentativa.value === 6) {
      perdeu.value = true
    }
  }






}

</script>
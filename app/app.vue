<template>
  <UApp>

    <!-- HEADER -->
    <MeuHeader :cor-primaria="corPrimaria" />

    <UMain class="bg-stone-800 select-none">


      <!-- TEMA E RESULTADO FIMDE JOGO -->
      <UContainer class="pt-10 gap-2 flex flex-col items-center justify-center">
        <USeparator class=" w-40">
          <template #default>
            <span v-if="!perdeubloqueio && !ganhoubloqueio" class="text-xs font-light text-stone-500">Tema</span>
            <span v-if="perdeubloqueio" class="text-xs font-light text-stone-500">Perdeu...</span>
            <span v-if="ganhoubloqueio" class="text-xs font-light text-stone-500">Ganhou!!!</span>
          </template>
        </USeparator>
        <UBadge v-if="!perdeubloqueio && !ganhoubloqueio" :icon="iconeTema" size="xl" class=" rounded-full"
          :color="corPrimaria" variant="subtle">
          {{ tema }}
        </UBadge>
        <UBadge v-if="perdeubloqueio" icon="streamline-plump:skull-2-solid" size="lg" class="px-4 rounded-full"
          color="tertiary" variant="subtle">
          A {{ palavraOuFrase }} era: {{ palavra }}</UBadge>

        <UBadge v-if="ganhoubloqueio" icon="solar:confetti-bold-duotone" size="lg" class="px-4 rounded-full"
          :color="corPrimaria" variant="subtle">
          Parabéns !!!</UBadge>
      </UContainer>


      <!-- FORCA E PALAVRA -->
      <UContainer class="mb-10 flex flex-col gap-5 items-center justify-center">
        <img :src="`/forca/${forcaTentativa}.png`" class="h-50 sm:h-60 pl-7" />
        <div class="flex flex-wrap gap-1">
          <div v-for="(letraPalavra, i) in palavra" key="i">
            <UBadge v-if="!chutes.some(l => letraPalavra.includes(l)) && letraPalavra != ' ' && letraPalavra != '-'"
              color="tertiary" variant="subtle" class="text-xl font-bold text-white
              w-8 h-8 md:w-12 md:h-12 flex items-center justify-center p-0">
            </UBadge>
            <UBadge v-if="chutes.some(l => letraPalavra.includes(l)) && letraPalavra != ' ' && letraPalavra != '-'"
              :color="corPrimaria" variant="subtle" class="text-xl font-bold text-white
              w-8 h-8 md:w-12 md:h-12 flex items-center justify-center p-0">
              {{ letraPalavra }}
            </UBadge>
            <div v-if="letraPalavra == ' '" class="w-3 md:w-5  ">
            </div>
            <div v-if="letraPalavra == '-'" class="h-8 w-5 flex items-center justify-center">
              <div class="bg-stone-400 h-1 w-3 rounded-xs" />
            </div>
          </div>
        </div>
        <UButton icon="material-symbols:replay-rounded" @click="tentarNovamente" v-if="ganhoubloqueio || perdeubloqueio"
          :color="corPrimaria" class="text-md font-bold text-white
          px-2 flex items-center justify-center">
          Jogar Novamente
        </UButton>
      </UContainer>


      <!-- TECLADO -->
      <UContainer class="max-w-2xl flex flex-wrap gap-2 items-center justify-center">
        <div v-for="letra in letras" key="id">
          <UButton @click="chutar(letra)" v-if="letra.status === 'padrao'" :disabled="perdeubloqueio || ganhoubloqueio"
            :color="corPrimaria" class="text-xl md:text-2xl md:rounded-xl font-bold text-white
        w-8 h-8 md:w-15 md:h-12 flex items-center justify-center p-0">
            {{ letra.letra }}
          </UButton>
          <UButton v-if="letra.status === 'naoTem'" color="tertiary" disabled="" class="text-xl md:text-2xl font-bold text-white
        w-8 h-8 md:w-15 md:h-12 flex items-center justify-center p-0">
            {{ letra.letra }}
          </UButton>
          <UButton v-if="letra.status === 'tem'" color="success" disabled="" class="text-xl md:text-2xl font-bold text-white
        w-8 h-8 md:w-15 md:h-12 flex items-center justify-center p-0">
            {{ letra.letra }}
          </UButton>
        </div>
      </UContainer>


      <!-- MODAIS -->
      <UModal :open="ganhou" :close="{ onClick: () => ganhou = false }">
        <template #title>
          <div class="flex items-center justify-center gap-2">
            Parabéns, voce acertou!
            <Icon name="solar:confetti-bold-duotone" />
          </div>
        </template>
        <template #body>
          <div class="flex flex-col gap-2 items-center justify-center">
            <UBadge icon="streamline-plump:balloon-remix" size="xl" class="px-4 rounded-full" :color="corPrimaria"
              variant="subtle">
              A {{ palavraOuFrase }} era: {{ palavra }}
            </UBadge>
            <img src="/forca/7.png" class="h-50" />
            <UButton icon="material-symbols:replay-rounded" @click="tentarNovamente"
              v-if="ganhoubloqueio || perdeubloqueio" :color="corPrimaria" class="mt-7 text-md font-bold text-white
            px-2 flex items-center justify-center">
              Jogar Novamente
            </UButton>
          </div>
        </template>
      </UModal>

      <UModal :open="perdeu" :close="{ onClick: () => perdeu = false }">
        <template #title>
          <div class="flex items-center justify-center gap-2">

            <Icon name="game-icons:pirate-grave" />
            Que pena, Você perdeu...
          </div>
        </template>
        <template #body>
          <div class="flex flex-col gap-2 items-center justify-center">
            <UBadge icon="streamline-plump:skull-2-solid" size="xl" class="px-4 rounded-full" color="tertiary"
              variant="subtle">
              A {{ palavraOuFrase }} era: {{ palavra }}
            </UBadge>
            <img src="/forca/6.png" class="h-50 pl-7" />
            <UButton icon="material-symbols:replay-rounded" @click="tentarNovamente"
              v-if="ganhoubloqueio || perdeubloqueio" :color="corPrimaria" class="mt-7 text-md font-bold text-white
            px-2 flex items-center justify-center">
              Jogar Novamente
            </UButton>
          </div>
        </template>
      </UModal>


    </UMain>
  </UApp>
</template>

<script setup>
import { ref } from 'vue';


// VARIÁVEIS 

const corPrimaria = ref("primary");

const palavra = ref("");
const palavraOuFrase = ref("");
const tema = ref("");
const iconeTema = ref("");

const letrasCorretas = ref([]);
const acertos = ref(0);

const forcaTentativa = ref(0);
const chutes = ref([]);

const perdeu = ref(false);
const perdeubloqueio = ref(false);
const ganhou = ref(false);
const ganhoubloqueio = ref(false);

const cores = [
  "primary",
  "red",
  "orange",
  "emerald",
  "miku",
  "blue",
  "indigo",
  "violet",
  "purple",
  "pink",
];

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


// FUNÇÕES

//poe as letras corretas na array letrasCorretas
function carregarLetrasCorretas() {
  for (let p in palavra.value) {
    if (!(letrasCorretas.value.includes(palavra.value[p])) && palavra.value[p] != ' ' && palavra.value[p] != '-') {
      letrasCorretas.value.push(palavra.value[p])
    }
  }
  console.log(letrasCorretas)
}

//verifica se tem as letras que foram chutadas
function chutar(letra) {

  //verifica se tem a letra
  for (let p in letrasCorretas.value) {
    if (letra.letra === letrasCorretas.value[p]) {
      chutes.value.push(letra.letra)
      letra.status = "tem"
      acertos.value += 1;
      if (acertos.value === letrasCorretas.value.length) {
        ganhou.value = true
        ganhoubloqueio.value = true
        forcaTentativa.value = 7
      }
    }

    // verifica se tem alguma variação da letra
    for (let lc in letrasCorretas.value) {
      if (letra.alt.includes(letrasCorretas.value[lc])) {
        chutes.value.push(letrasCorretas.value[lc])
        letra.status = "tem"
        acertos.value += 1;
        if (acertos.value === letrasCorretas.value.length) {
          ganhou.value = true
          ganhoubloqueio.value = true
          forcaTentativa.value = 7
        }
      }
    }
    if (letra.status == "tem") { break }
  }

  //se n tiver a letra
  if (letra.status != "tem") {
    letra.status = "naoTem"
    forcaTentativa.value += 1
    if (forcaTentativa.value === 6) {
      perdeu.value = true
      perdeubloqueio.value = true
    }
  }

}

// gera a palavra da vez
function gerarPalavra() {
  palavra.value = "T es-te-um"
  tema.value = "geral"
  iconeTema.value = "solar:earth-bold-duotone"
  palavra.value = palavra.value.toUpperCase();
  if (palavra.value.includes(' ')) {
    palavraOuFrase.value = "frase"
  } else {
    palavraOuFrase.value = "palavra"
  }
}

//reinicia valores pra tentar novamente.
function tentarNovamente() {
  for (let letra in letras.value) {
    letras.value[letra].status = 'padrao';
    console.log(letras.value[letra].status)
  }
  gerarPalavra();
  acertos.value = 0;
  forcaTentativa.value = 0;
  chutes.value = [];
  perdeu.value = false;
  perdeubloqueio.value = false;
  ganhou.value = false;
  ganhoubloqueio.value = false;
  letrasCorretas.value = [];
  carregarLetrasCorretas();
  console.log(palavra);
  console.log(letrasCorretas);
}

//carrega cor aleatória
function carregarCorAleatoria() {
  corPrimaria.value = cores[Math.floor(Math.random() * cores.length)]
}


// LIFECYCLE HOOKS

onMounted(() => {
  carregarCorAleatoria();
  gerarPalavra();
  carregarLetrasCorretas();
})

</script>
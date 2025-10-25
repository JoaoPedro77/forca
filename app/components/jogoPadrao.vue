<template>


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
  <UContainer class="mb-10 sm:mb-5 flex flex-col gap-5 items-center justify-center">
    <img :src="`/desenhos/${forcaTentativa}.png`" class="h-50 sm:h-60 pl-7" />
    <div class="flex flex-wrap justify-center gap-1">
      <div
        :class="{ 'basis-full md:basis-0': (letraPalavra == ' ' || letraPalavra == '-') && (!/\d/.test(palavra[i + 1])) }"
        class="group" v-for="(letraPalavra, i) in palavra" key="i">
        <UBadge
          v-if="!chutes.some(l => letraPalavra.includes(l)) && letraPalavra != ' ' && letraPalavra != '-' && letraPalavra != `'`"
          color="tertiary" variant="subtle" class="text-xl font-bold text-white
              w-6 h-6 md:w-12 md:h-12 flex items-center justify-center p-0">
        </UBadge>
        <UBadge
          v-if="chutes.some(l => letraPalavra.includes(l)) && letraPalavra != ' ' && letraPalavra != '-' && letraPalavra != `'`"
          :color="corPrimaria" variant="subtle" class="text-xl sm:text-2xl font-bold text-white
              w-6 h-6 md:w-12 md:h-12 flex items-center justify-center p-0">
          {{ letraPalavra }}
        </UBadge>
        <div v-if="letraPalavra == ' ' || letraPalavra == '-'" class="w-3 md:w-5 ">
        </div>
        <div v-if="letraPalavra == `'`" class="w-1 h-2 md:w-1 md:h-3 mb-1 md:mx-1 rounded-2xl bg-stone-400">
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
  <UContainer class="max-w-[380px] sm:max-w-2xl md:max-w-3xl flex flex-wrap px-1 items-center justify-center">
    <div :class="{ 'basis-full pb-2': letra.letra == ' ' }" v-for="letra in letras" key="id">

      <UButton @click="chutar(letra)" v-if="letra.status === 'padrao' && letra.letra != ' '"
        :disabled="perdeubloqueio || ganhoubloqueio" :color="corPrimaria" :class="[
          'text-xl md:text-2xl font-bold text-white flex items-center justify-center p-0',
          /\d/.test(letra.letra) ? 'w-7 h-7 md:w-13 md:h-11 m-0.5 md:m-1' : 'w-8 h-8 md:w-15 md:h-12 m-0.5 xs:m-1'
        ]">
        {{ letra.letra }}
      </UButton>
      <UButton v-if="letra.status === 'naoTem' && letra.letra != ' '" color="tertiary" disabled="" :class="[
        'text-xl md:text-2xl font-bold text-white flex items-center justify-center p-0',
        /\d/.test(letra.letra) ? 'w-7 h-7 md:w-13 md:h-11 m-0.5 md:m-1' : 'w-8 h-8 md:w-15 md:h-12 m-0.5 xs:m-1'
      ]">
        {{ letra.letra }}
      </UButton>
      <UButton v-if="letra.status === 'tem' && letra.letra != ' '" color="success" disabled="" :class="[
        'text-xl md:text-2xl font-bold text-white flex items-center justify-center p-0',
        /\d/.test(letra.letra) ? 'w-7 h-7 md:w-13 md:h-11 m-0.5 md:m-1' : 'w-8 h-8 md:w-15 md:h-12 m-0.5 xs:m-1'
      ]">
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
        <span v-if="erros == 0">você não errou nenhuma letra </span>
        <span v-if="erros == 1">você errou só {{ erros }} letra, muito bom</span>
        <span v-if="erros == 2">você errou {{ erros }} letras, boa</span>
        <span v-if="erros == 3">você errou {{ erros }} letras</span>
        <span v-if="erros == 4">vish você errou {{ erros }} letras</span>
        <span v-if="erros == 5">você errou {{ erros }} letras, por pouco</span>
        <img src="/desenhos/7.png" class="h-50" />
        <UButton icon="material-symbols:replay-rounded" @click="tentarNovamente" v-if="ganhoubloqueio || perdeubloqueio"
          :color="corPrimaria" class="mt-7 text-md font-bold text-white
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
        <img src="/desenhos/6.png" class="h-50 pl-7" />
        <UButton icon="material-symbols:replay-rounded" @click="tentarNovamente" v-if="ganhoubloqueio || perdeubloqueio"
          :color="corPrimaria" class="mt-7 text-md font-bold text-white
            px-2 flex items-center justify-center">
          Jogar Novamente
        </UButton>
      </div>
    </template>
  </UModal>

</template>

<script setup>
import { ref } from 'vue';

const PalavrasForca = ref([]);



// VARIÁVEIS 

defineProps({
  corPrimaria: String
})

const palavra = ref("");
const palavraOuFrase = ref("");
const tema = ref("");
const iconeTema = ref("");

let letrasCorretas = [];
const acertos = ref(0);

const forcaTentativa = ref(0);
const chutes = ref([]);
const erros = ref(0);

const perdeu = ref(false);
const perdeubloqueio = ref(false);
const ganhou = ref(false);
const ganhoubloqueio = ref(false);

const letras = ref([

  { id: 11, letra: "A", status: 'padrao', alt: ['A', 'Á', 'À', 'Ã', 'Â', 'Ä'] },
  { id: 12, letra: "B", status: 'padrao', alt: ['B'] },
  { id: 13, letra: "C", status: 'padrao', alt: ['C', 'Ç'] },
  { id: 14, letra: "D", status: 'padrao', alt: ['D'] },
  { id: 15, letra: "E", status: 'padrao', alt: ['E', 'É', 'È', 'Ê', 'Ë'] },
  { id: 16, letra: "F", status: 'padrao', alt: ['F'] },
  { id: 17, letra: "G", status: 'padrao', alt: ['G'] },
  { id: 18, letra: "H", status: 'padrao', alt: ['H'] },
  { id: 19, letra: "I", status: 'padrao', alt: ['I', 'Í', 'Ì', 'Î', 'Ï'] },
  { id: 20, letra: "J", status: 'padrao', alt: ['J'] },
  { id: 21, letra: "K", status: 'padrao', alt: ['K'] },
  { id: 22, letra: "L", status: 'padrao', alt: ['L'] },
  { id: 23, letra: "M", status: 'padrao', alt: ['M'] },
  { id: 24, letra: "N", status: 'padrao', alt: ['N'] },
  { id: 25, letra: "O", status: 'padrao', alt: ['O', 'Ó', 'Ò', 'Õ', 'Ô', 'Ö'] },
  { id: 26, letra: "P", status: 'padrao', alt: ['P'] },
  { id: 27, letra: "Q", status: 'padrao', alt: ['Q'] },
  { id: 28, letra: "R", status: 'padrao', alt: ['R'] },
  { id: 29, letra: "S", status: 'padrao', alt: ['S'] },
  { id: 30, letra: "T", status: 'padrao', alt: ['T'] },
  { id: 31, letra: "U", status: 'padrao', alt: ['U', 'Ú', 'Ù', 'Û', 'Ü'] },
  { id: 32, letra: "V", status: 'padrao', alt: ['V'] },
  { id: 33, letra: "W", status: 'padrao', alt: ['W'] },
  { id: 34, letra: "X", status: 'padrao', alt: ['X'] },
  { id: 35, letra: "Y", status: 'padrao', alt: ['Y'] },
  { id: 36, letra: "Z", status: 'padrao', alt: ['Z'] }
]);


// FUNÇÕES

//poe as letras corretas na array letrasCorretas
function carregarLetrasCorretas() {
  for (let p in palavra.value) {
    if (!(letrasCorretas.includes(palavra.value[p])) && palavra.value[p] != ' ' && palavra.value[p] != '-' && palavra.value[p] != `'`) {
      letrasCorretas.push(palavra.value[p])
    }
  }
  console.log(letrasCorretas)
}

//verifica se tem as letras que foram chutadas
function chutar(letra) {

  //se tiver a letra ou algumva variação dela
  for (let lc in letrasCorretas) {
    if (letra.alt.includes(letrasCorretas[lc])) {
      chutes.value.push(letrasCorretas[lc])
      letra.status = "tem"
      acertos.value += 1;
      if (acertos.value === letrasCorretas.length) {
        ganhou.value = true
        ganhoubloqueio.value = true
        forcaTentativa.value = 7
      }
    }
  }

  //se n tiver a letra
  if (letra.status != "tem") {
    erros.value += 1
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

  const palavraSorteada = PalavrasForca.value[Math.floor(Math.random() * PalavrasForca.value.length)];
  palavra.value = palavraSorteada.palavra;
  tema.value = palavraSorteada.tema;
  iconeTema.value = palavraSorteada.iconeTema;

  palavra.value = palavra.value.toUpperCase();
  if (palavra.value.includes(' ')) {
    palavraOuFrase.value = "frase"
  } else {
    palavraOuFrase.value = "palavra"
  }
  if (palavra.value.includes('0')
    || palavra.value.includes('1')
    || palavra.value.includes('2')
    || palavra.value.includes('3')
    || palavra.value.includes('4')
    || palavra.value.includes('5')
    || palavra.value.includes('6')
    || palavra.value.includes('7')
    || palavra.value.includes('8')
    || palavra.value.includes('9')) {
    letras.value.unshift({ id: 1, letra: "1", status: 'padrao', alt: ['1'] },
      { id: 2, letra: "2", status: 'padrao', alt: ['2'] },
      { id: 3, letra: "3", status: 'padrao', alt: ['3'] },
      { id: 4, letra: "4", status: 'padrao', alt: ['4'] },
      { id: 5, letra: "5", status: 'padrao', alt: ['5'] },
      { id: 6, letra: "6", status: 'padrao', alt: ['6'] },
      { id: 7, letra: "7", status: 'padrao', alt: ['7'] },
      { id: 8, letra: "8", status: 'padrao', alt: ['8'] },
      { id: 9, letra: "9", status: 'padrao', alt: ['9'] },
      { id: 10, letra: "0", status: 'padrao', alt: ['0'] },
      { id: 10, letra: " ", status: 'padrao', alt: [' '] },)
  }
}

//reinicia valores pra tentar novamente.
function tentarNovamente() {
  letras.value = [

    { id: 11, letra: "A", status: 'padrao', alt: ['A', 'Á', 'À', 'Ã', 'Â', 'Ä'] },
    { id: 12, letra: "B", status: 'padrao', alt: ['B'] },
    { id: 13, letra: "C", status: 'padrao', alt: ['C', 'Ç'] },
    { id: 14, letra: "D", status: 'padrao', alt: ['D'] },
    { id: 15, letra: "E", status: 'padrao', alt: ['E', 'É', 'È', 'Ê', 'Ë'] },
    { id: 16, letra: "F", status: 'padrao', alt: ['F'] },
    { id: 17, letra: "G", status: 'padrao', alt: ['G'] },
    { id: 18, letra: "H", status: 'padrao', alt: ['H'] },
    { id: 19, letra: "I", status: 'padrao', alt: ['I', 'Í', 'Ì', 'Î', 'Ï'] },
    { id: 20, letra: "J", status: 'padrao', alt: ['J'] },
    { id: 21, letra: "K", status: 'padrao', alt: ['K'] },
    { id: 22, letra: "L", status: 'padrao', alt: ['L'] },
    { id: 23, letra: "M", status: 'padrao', alt: ['M'] },
    { id: 24, letra: "N", status: 'padrao', alt: ['N'] },
    { id: 25, letra: "O", status: 'padrao', alt: ['O', 'Ó', 'Ò', 'Õ', 'Ô', 'Ö'] },
    { id: 26, letra: "P", status: 'padrao', alt: ['P'] },
    { id: 27, letra: "Q", status: 'padrao', alt: ['Q'] },
    { id: 28, letra: "R", status: 'padrao', alt: ['R'] },
    { id: 29, letra: "S", status: 'padrao', alt: ['S'] },
    { id: 30, letra: "T", status: 'padrao', alt: ['T'] },
    { id: 31, letra: "U", status: 'padrao', alt: ['U', 'Ú', 'Ù', 'Û', 'Ü'] },
    { id: 32, letra: "V", status: 'padrao', alt: ['V'] },
    { id: 33, letra: "W", status: 'padrao', alt: ['W'] },
    { id: 34, letra: "X", status: 'padrao', alt: ['X'] },
    { id: 35, letra: "Y", status: 'padrao', alt: ['Y'] },
    { id: 36, letra: "Z", status: 'padrao', alt: ['Z'] }
  ];
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
  letrasCorretas = [];
  erros.value = 0;
  carregarLetrasCorretas();
}


// TECLADO FÍSICO

function encontrarLetraPorTecla(teclaPressionada) {
  const tecla = teclaPressionada.toUpperCase();

  return letras.value.find(letra =>
    letra.alt.includes(tecla) || //maldito ç, morra ç
    letra.letra === tecla
  );
}

// LIFECYCLE HOOKS

onMounted(async () => {
  await fetch('/palavras_forca.json')
    .then(res => res.json())
    .then(data => {
      PalavrasForca.value = data;
    })
  console.log(PalavrasForca.value);
  gerarPalavra();
  carregarLetrasCorretas();

  // detecta o teclado físico
  document.addEventListener('keydown', function (event) {
    if (perdeubloqueio.value || ganhoubloqueio.value) {
      return
    }
    if ((event.key.length === 1 && /[a-zA-ZÀ-ÿ-0-9]/.test(event.key))) {
      event.preventDefault();

      const letraEncontrada = encontrarLetraPorTecla(event.key);

      if (letraEncontrada) {
        chutar(letraEncontrada);
      }
    }
  });

})


</script>
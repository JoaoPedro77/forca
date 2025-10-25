<template>
  <UApp>
    <MeuHeader />
    <UMain class="bg-stone-800 select-none">
      <UTabs :color="corPrimaria" :items="items" default-value="1">


        <template #daily>
          <JogoDiario :cor-primaria="corPrimaria" />
        </template>

        <template #infinity>
          <JogoPadrao :cor-primaria="corPrimaria" />
        </template>

        <template #party>
          <JogoPersonalizado :cor-primaria="corPrimaria" />
        </template>
      </UTabs>
    </UMain>
  </UApp>
</template>

<script setup>
import { ref } from 'vue';

const items = [

  {
    label: 'Infinito',
    icon: 'material-symbols:joystick-outline-sharp',
    slot: 'infinity'
  },
  {
    label: 'Diario',
    icon: 'mdi:calendar-today',
    slot: 'daily'
  },

  {
    label: 'Personalizado',
    icon: 'solar:pen-new-square-linear',
    slot: 'party'
  }
]

const corPrimaria = ref('');

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

//carrega cor aleatória
function carregarCorAleatoria() {
  corPrimaria.value = cores[Math.floor(Math.random() * cores.length)]
}


onMounted(() => {
  carregarCorAleatoria();
})

</script>
<script setup>
const items = [
  {
    label: 'Padrão',
    icon: 'zondicons:shuffle',
    slot: 'padrao'
  },
  {
    label: 'Personalizado',
    icon: 'solar:pen-new-square-linear',
    slot: 'party'
  }
]

</script>

<template>
  <UApp>
    <MeuHeader :cor-primaria="corPrimaria" />
    <UMain class="bg-stone-800 select-none">
      <UTabs :items="items">
        <template #padrao>
          <JogoPadrao />
        </template>

        <template #party>
          <JogoPersonalizado />
        </template>
      </UTabs>
    </UMain>
  </UApp>
</template>

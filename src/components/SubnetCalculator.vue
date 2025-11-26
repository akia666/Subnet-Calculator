<script setup lang="ts">
import { ref, computed } from 'vue';
import { UiButton, UiField, UiInput, UiSelect } from 'retro-components-akia';

import { mask } from '../constants/subnetMasks';
import { isIpValid } from '../utils/ipValidation';
import { getNetworkAdress } from '../utils/networkAddress';
import { getAddressesCount } from '../utils/addressesCount';

const ipAddress = ref('');
const selectedMask = ref('255.255.255.255');
const isShowResult = ref(false);

const computedIsValid = computed(() => isIpValid(ipAddress.value));
const computedNetworkAddress = computed(() => getNetworkAdress(ipAddress.value, selectedMask.value));
const computedAddressesCount = computed(() => getAddressesCount(selectedMask.value));

function showResult() {
  isShowResult.value = true;
} 

const resultItems = computed(() => [
  { label: 'IP-адрес:', value: ipAddress.value },
  { label: 'Маска подсети:', value: selectedMask.value },
  { label: 'Адрес сети:', value: computedNetworkAddress.value },
  { label: 'Количество возможных адресов:', value: computedAddressesCount.value },
]);
</script>

<template>
  <div class="container">
    <h1 class="h1">КАЛЬКУЛЯТОР ПОДСЕТЕЙ</h1>

    <div class="form-window">
      <div class="form-group">
        <UiField label="IP-адрес">
          <UiInput v-model="ipAddress" placeholder="192.168.1.0" class="form-input"/>
        </UiField>

        <UiField label="Маска подсети">
          <UiSelect v-model="selectedMask" :options="mask" class="form-input"></UiSelect>
        </UiField>
      </div>

      <div class="button-container">
        <UiButton @click="showResult" type="submit" :is-disabled="!computedIsValid" class="submit-button">
          Вычислить
        </UiButton>
      </div>
    </div>

    <div class="result" v-if="isShowResult && computedIsValid">
      <div v-for="item in resultItems" :key="item.label" class="result-item">
        {{ item.label }} {{ item.value }}
      </div>
    </div>
  </div>
</template>

<style>
.h1 {
  font-family: 'Press Start 2P', monospace;
  color: var(--retro-text);
  text-align: center;
  margin-bottom: 40px;
}

.container {
  align-items: center;
  display: flex;
  flex-direction: column;
  gap: 30px;
  max-width: 600px;
  margin: 0 auto;
  padding: 40px 20px;
  min-height: 100vh;
  justify-content: center;
}

.form-window {
  background-color: var(--retro-bg-secondary);
  border: 3px solid var(--retro-border);
  border-radius: 12px;
  padding: 30px;
  box-shadow:
    0 4px 0 var(--retro-shadow),
    inset 0 0 0 2px rgba(255, 255, 255, 0.1);
  width: 100%;
  max-width: 500px;
  box-sizing: border-box;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 25px;
  width: 100%;
}

.form-group > * {
  width: 100%;
  flex: 1 1 100%;
  min-width: 0;
}

.form-input {
  width: 100%;
  box-sizing: border-box;
}

.button-container {
  display: flex;
  justify-content: center;
  width: 100%;
  margin-top: 20px;
}

.submit-button {
  width: 100%;
  max-width: 200px;
}

.result {
  display: flex;
  flex-direction: column;
  background-color: var(--retro-bg-secondary);
  border: 3px solid var(--retro-border);
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 0 var(--retro-shadow);
  font-family: 'Press Start 2P', monospace;
  color: var(--retro-text);
  gap: 12px;
  width: 100%;
  max-width: 500px;
  margin-top: 20px;
}

.result-item {
  font-size: 0.9rem;
}
</style>

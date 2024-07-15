<template>
    <div>
      <!-- v-bind é uma diretiva usada para vincular atributos de HTML a expressões dinâmicas em seu modelo Vue. Isso permite que você passe dados do componente para atributos HTML de maneira reativa. A sintaxe básica é v-bind:attribute="expression". -->
  
      <!--nesse caso esta sendo usado p um link, que abre na mesma pagina. o v-bind pode ser abreviaddo-->
      <a v-bind:href="url">Clique aqui</a>
  
      <!--aqui adicionar ou remover classes dinamicamente com base em condições.
      a classe ativo será adicionada se isActive for true, e a classe texto-grande será adicionada se isLargeText for true.-->
      <div v-bind:class="{ ativo: isActive, 'texto-grande': isLargeText }"></div>
  
      <!--Neste exemplo, o estilo color será definido como o valor de activeColor, e o estilo fontSize será definido como fontSize seguido por 'px'.-->
      <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
  
      <!--Neste exemplo, o atributo data-id do botão será vinculado à expressão itemId.-->
      <button v-bind:data-id="itemId">Clique</button>
  
  
      <!-- v-model -->
       <br>
      <input v-model="message" placeholder="Digite uma mensagem">
      <p>A mensagem é: {{ message }}</p>
  
      <!-- v-if, v-else -->
      <p v-if="isLoggedIn">Bem-vindo, usuário!</p>
      <p v-else>Por favor, faça login.</p>
  
      <!-- v-show -->
      <p v-show="isVisible">Você pode ver este texto.</p>
      <button @click="toggleVisibility">Alternar Visibilidade</button>
  
      <!-- v-for -->
      <ul>
        <li v-for="item in items" :key="item.id">{{ item.name }}</li>
      </ul>
  
      <!-- v-on -->
      <button @click="showAlert">Clique aqui</button>
  
      <!-- v-bind:class e v-bind:style -->
      <p :class="{ active: isActive }" :style="{ color: textColor, fontSize: fontSize + 'px' }">Texto Estilizado</p>
      <button @click="toggleActive">Alternar Classe</button>
      <input v-model="textColor" placeholder="Digite a cor do texto">
      <input v-model.number="fontSize" placeholder="Digite o tamanho do texto em px">
  
      <!-- v-slot -->
      <custom-component>
        <template v-slot:header>
          <h1>Este é o cabeçalho</h1>
        </template>
        <template v-slot:default>
          <p>Este é o conteúdo padrão</p>
        </template>
      </custom-component>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  // Data properties
  const url = ref('https://vuejs.org');
  const message = ref('');
  const isLoggedIn = ref(false);
  const isVisible = ref(true);
  const items = ref([
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
    { id: 3, name: 'Item 3' }
  ]);
  const isActive = ref(false);
  const textColor = ref('blue');
  const fontSize = ref(16);
  
  // Methods
  function showAlert() {
    alert('Botão clicado!');
  }
  
  function toggleVisibility() {
    isVisible.value = !isVisible.value;
  }
  
  function toggleActive() {
    isActive.value = !isActive.value;
  }
  </script>
  
  <style>
  .active {
    font-weight: bold;
  }
  </style>
  
<template>
  <div class="popUp" v-show="display">
    <div class="containerImg">
      <img src="../../src/assets/LyonEsport.jpg" alt="" />
    </div>
    <h2 class="title">Lyon Esport</h2>
    <div class="desc">
      <p>
        Lorem, ipsum dolor sit amet consectetur adipisicing elit. Delectus quod, voluptatibus ex pariatur totam eius laborum sunt minima maxime provident fugit,
        expedita doloremque. Dolores qui, vitae consequuntur ullam ess Lorem, ipsum dolor sit amet consectetur adipisicing elit. Delectus quod, voluptatibus ex
        pariatur totam eius laborum sunt minima maxime provident fugit, expedita doloremque. Dolores qui, vitae consequuntur ullam ess Lorem, ipsum dolor sit
        amet consectetur adipisicing elit. Delectus quod, voluptatibus ex pariatur totam eius laborum sunt minima maxime provident fugit, expedita doloremque.
        Dolores qui, vitae consequuntur ullam esse excepturi.
      </p>
    </div>
    <button class="btn" v-on:click="display = false">En savoir plus</button>
  </div>
</template>

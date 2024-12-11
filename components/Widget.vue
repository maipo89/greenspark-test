<template>
  <div class="widget">
    <div class="widget__header"
         :style="{ backgroundColor: headerBackgroundColor }"
    >
      <img 
        v-if="selectedColor === 'white' || selectedColor === 'beige'" 
        src="/images/greensparkLogoGreen.svg" 
        alt="greenspark logo green"
      />
      <img 
        v-else 
        src="/images/greensparkLogo.svg" 
        alt="greenspark logo"
      />
      <div class="widget__header__text" :class="selectedColor === 'white' || selectedColor === 'beige' ? 'green-text' : null">
        <p>This product {{action}}</p>
        <h2>100 {{type}} {{type === 'plastic' ? 'bottles'  : null}}</h2>
      </div>
    </div>
    <div class="widget__public-profile">
      <div class="widget__public-profile__title">
        <p>Link to Public Profile</p>
        <div class="widget__toolkit">
          <span>i</span>
          <div class="widget__toolkit-message">
            <p class="widget__toolkit-message__text">This widget links directly to your public profile so that you can easily share your impact with your customers. Turn it off here if you do not want the badge to link to it.</p>
            <div class="widget__toolkit-message__link">
              <a>View Public Profile</a>
            </div>
          </div>
        </div>
      </div>
      <div :class="linked ? 'active' : null"
            @click="$emit('linked', { id, linked: !linked })"
            class="widget__checkbox"></div>
    </div>
    <div class="widget__badge-colour">
      <p>Badge colour</p>
      <div class="widget__colours">
         <div class="widget__colours__colour" 
              v-for="(colour, index) in colours"
              :class="selectedColor === colour.name ? 'active' : null"
              @click="$emit('colour', { id, colour: colour.name })"
              :key="index"
              :style="{backgroundColor: colour.code}"
         >
         </div>
      </div>
    </div>
    <div class="widget__activate-badge">
      <p>Activate badge</p>
      <div  :class="active ? 'active' : null" 
            class="widget__activate-badge__button"
            @click="$emit('active', { id, active: !active })"
            >
        <div class="widget__activate-badge__button__inside"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  action: String,
  active: Boolean,
  amount: Number,
  linked: Boolean,
  selectedColor: String,
  type: String,
  id: Number
});

const colours = ref([
  { name: 'blue', code: '#2E3A8C' },
  { name: 'green', code: '#3B755F' },
  { name: 'beige', code: '#F2EBDB' },
  { name: 'white', code: '#FFFFFF' },
  { name: 'black', code: '#212121' }
]);

const headerBackgroundColor = computed(() => {
  switch (props.selectedColor) {
    case 'green':
      return '#3B755F';
    case 'black':
      return '#212121';
    case 'beige':
      return '#F2EBDB';
    case 'white':
      return '#FFFFFF';
    case 'blue':
      return '#2E3A8C';
  }
});

</script>
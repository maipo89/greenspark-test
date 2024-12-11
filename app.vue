<template>
  <div class="container">
    <div class="container__wrapper">
      <h1>{{title}}</h1>
      <div class="line"></div>
      <div class="container__widgets">
        <Widget v-if="widgetsData" 
                v-for="(data, index) in widgetsData"
                :key="index"
                :id="index"
                :action="data.action" 
                :active="data.active" 
                :amount="data.amount" 
                :linked="data.linked"
                :selectedColor="data.selectedColor" 
                :type="data.type"
                @linked="updateLinkedState"
                @active="updateActiveState"
                @colour="updateColourState"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Widget from '/components/Widget.vue'

const config = useRuntimeConfig();
const title = "Per product widgets";
const widgetsData = ref(null);


const getWidgetsData = async () => {
  try {

    const response = await fetch('https://b795b019-1f84-41f4-93a3-a702d686c75a.mock.pstmn.io/product-widgets', {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
      },
    });

    if (response.ok) {
      const widgets = await response.json();

      let activeFound = false;
      widgets.forEach((widget) => {
        if (widget.active) {
          if (!activeFound) {
            activeFound = true;
          } else {
            widget.active = false;
          }
        }
      });

      widgetsData.value = widgets;
      console.log(widgetsData.value, 'widget')
    } else {
      throw new Error('Error fetching the data');
    }
  } catch (error) {
    console.log('Error fetching patiens data');
  }
}
getWidgetsData();

const updateLinkedState = ({ id, linked }) => {
  widgetsData.value[id].linked = linked;
};

const updateActiveState = ({ id, active }) => {
  widgetsData.value.forEach((widget) => {
    widget.active = false;
  });
  widgetsData.value[id].active = active;
};

const updateColourState = ({ id, colour }) => {
  console.log(colour)
  widgetsData.value[id].selectedColor = colour;
};

</script>

<template>
  <q-page class="row items-center justify-evenly text-brand">
    <q-form
      class="column q-px-xl q-py-md shadow-2 bg-blue-1"
      style="max-width: 500px"
      action=""
      method=""
      @submit="submitForm"
    >
      <q-input
        @change="fetchTitle"
        name="title"
        placeholder="Movie Title"
        v-model="form.title"
        autocomplete="off"
        :rules="[val=> !!val || 'Invalid Input']"
      />
      <q-input
        name="location"
        placeholder="Cinema Location"
        v-model="form.location"
        autocomplete="off"
        :rules="[val=> !!val || 'Invalid Input']"
      />
      <q-select
        class="q-py-md"
        borderless
        v-model="form.cinemaNum"
        :options="options"
        label="Cinema Number"
        :rules="[val=> !!val || 'Invalid Input']"
      />
      <!-- <q-input
        type="number"
        name="cinemaNum"
        placeholder="Cinema Number"
        v-model="form.cinemaNum"
        required
      /> -->
      <p class="text-green q-mt-md q-mb-none">SHOWING DURATION</p>
      <div class="flex items-center">
        <q-input 
        @click="showCalendarRange = !showCalendarRange"
        color="grey-3"
        label="From"
        class="q-pt-sm"
        v-model="form.duration.from" 
        readonly
        :rules="[val=> !!val || 'Invalid Input']" />
        <q-input 
        @click="showCalendarRange = !showCalendarRange"
        color="grey-3"
        label="To"
        class="q-pt-sm"
        v-model="form.duration.to" 
        readonly
        :rules="[val=> !!val || 'Invalid Input']" />

        
          <!-- <q-icon class="q-pl-xl" size="30px" name="event" color="green"
          @click="showCalendarRange = !showCalendarRange" /> -->
       
      </div>

      <div v-if="showCalendarRange" class="q-mt-md">
        <q-date v-model="form.duration" range />
      </div>

      <div class="q-mt-xl">
        <p>Time Slots</p>
        <div>
          <q-checkbox v-model="form.timeSlot" label="09:00" val="09:00" />
          <q-checkbox v-model="form.timeSlot" label="11:00" val="11:00" />
          <q-checkbox v-model="form.timeSlot" label="13:00" val="13:00" />
          <q-checkbox v-model="form.timeSlot" label="16:00" val="16:00" />
          <q-checkbox v-model="form.timeSlot" label="18:00" val="18:00" />
          <q-checkbox v-model="form.timeSlot" label="20:00" val="20:00" />
        </div>
      </div>

      <q-btn class="q-mt-xl" label="ADD" type="submit" color="green" />
    </q-form>

    <q-table
      title="Released List"
      :rows="showingData"
      :columns="columns"
      row-key="name"
    />
  </q-page>
</template>

<script setup>
import { ref } from 'vue';

const form = ref({
  title: '',
  location: '',
  cinemaNum: null,
  duration: {},
  timeSlot: [],
});

//cinemanum options
const options = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const showCalendarRange = ref(false);


const showingData = [
  {
    title: 'Doctor Strange',
    location: 'SM Aura',
    cinemaNum: 2,
    duration: { from: '2023/03/01', to: '2023/03/15' },
    timeSlot: ['09:00', '11:00'],
  },
  {
    title: 'Avengers: Endgame',
    location: 'SM Megamall',
    cinemaNum: 1,
    duration: { from: '2023/03/05', to: '2023/03/18' },
    timeSlot: ['09:00', '11:00', '13:00'],
  },
];

//fetch movie title

// const fetchTitle = () =>
//   fetch(`https://www.omdbapi.com/?s=${form.value.title}&apikey=89be163e`).then(res=> res.json()).then(data => console.log(data.Search))

const submitForm = (event) => {
  event.preventDefault();
  showingData.unshift({
    title: form.value.title,
    location: form.value.location,
    cinemaNum: form.value.cinemaNum,
    duration: form.value.duration,
    timeSlot: form.value.timeSlot.sort(),
  });
  // form.value.title = '';
  // form.value.location = '';
  // form.value.cinemaNum = null;
  // form.value.duration = {};
  // form.value.timeSlot = [];
};

const columns = [
  {
    name: 'Movie Title',
    field: 'title',
    label: 'Movie Title',
    align: 'left',
    sortable: true,
  },
  {
    name: 'Location',
    field: 'location',
    label: 'Location',
    align: 'center',
    sortable: true,
  },
  {
    name: 'Cinema Number',
    field: 'cinemaNum',
    label: 'Cinema Number',
    align: 'center',
    sortable: true,
  },
  {
    name: 'Start Date',
    field: 'duration',
    label: 'Start Date',
    align: 'center',
    format: (val) => val.from,
  },
  {
    name: 'End Date',
    field: 'duration',
    label: 'End Date',
    align: 'center',
    format: (val) => val.to,
  },
  {
    name: 'Time Slot',
    field: 'timeSlot',
    label: 'Time Slot',
    align: 'center',
    format: (val) => val.join(' | '),
  },
];
</script>

<style>
.text-brand {
  background: linear-gradient(116.82deg, #000000 50%, #575757 100%), #ffffff;
}
</style>

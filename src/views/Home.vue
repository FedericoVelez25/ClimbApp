<template>

  <div v-if="dataLoaded" class="container mt-10 px-4">
    <h1>{{ name }}</h1>
    <!-- No Data -->
    <div v-if="data.length === 0" class="w-full flex flex-col items-center">
      <h1 class="text-2xl">Looks empty...</h1>
      <router-link
        :to="{ name: 'Create' }"
        class="mt-6 py-2 px-6 rounded-sm self-center text-sm
      text-white bg-at-olive-green duration-200 border-solid
      border-2 border-transparent hover:border-at-olive-green hover:bg-at-dark-green
      hover:text-white"
        >Create Climb</router-link
      >
    </div>
  

    <!-- Data -->
    <div
      v-else
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
    >
      <router-link
        class="flex flex-col rounded-md items-center bg-almond p-8 shadow-md cursor-pointer"
        :to="{ name: 'View-Climb', params: { climbId: climb.id } }"
        v-for="(climb, index) in data"
        :key="index"
      >
        <!-- Sport Climb Img-->

        <img
          v-if="climb.climbType === 'sport-climb'"
          src="@/assets/images/iconSportClimb.png"
          class="h-24 flex w-auto"
          alt="sport climb icon"
        />

        <!-- Boulder Img -->

        <img
          v-else
          src="@/assets/images/crimp.png"
          class="h-24 flex flex-col w-auto"
          alt="boulder icon"
        />

        <p
          class="mt-6 py-1 px-3 text-xs text-white bg-at-olive-green shadow-md rounded-lg text-center w-25"
        >
          {{ climb.climbType }}
        </p>
        <h1 class="mt-8 mb-2 text-center text-xl text-at-dark-green">
          {{ climb.climbName }}
        </h1>
        <h2
          class="mt-6 py-1 px-3 text-xs text-black shadow-md rounded-lg text-center w-25"
          :class="{
            'bg-green-500': climb.about[0].completed,
            'bg-red-500': !climb.about[0].completed,
          }"
        >
          {{ climb.about[0].completed ? "Completed" : "Project" }}
        </h2>
      </router-link>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/init";

export default {
  name: "Home",
  components: {},
 
  setup() {
    // Create data / vars
    const data = ref([]);
    const dataLoaded = ref(null);
    const name = ref('');
    const nameLoaded = ref(null);

    // Get data
    const getData = async () => {
      try {
        const { data: climbs, error } = await supabase
          .from("climbs")
          .select("*");
        if (error) throw error;
        data.value = climbs;
        dataLoaded.value = true;
      } catch (error) {
        console.warn(error.message);
      }
    };

    // Get name
const getName = async () => {
  // const res = localStorage.getItem('supabase.auth.token');
  // const finalRes = JSON.parse(res);
  // const id = finalRes.currentSession.user.id;
  const id = 'd6e0c502-1875-4e77-a00f-5b56351f861c'
  console.log(id) 
      try {
        const { data: profiles, error } = await supabase
          .from("profiles")
          .select("*").match({id: id})
        if (error) throw error;
        console.log(profiles)
        name.value = profiles[0].name;
        nameLoaded.value = true;
      } catch (error) {
        console.warn(error.message);
      }
    };
    // Run data function
    getData();
    getName();

    return { data, dataLoaded, name, nameLoaded };
  },
};
</script>

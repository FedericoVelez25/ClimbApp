<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- App Msg -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 rounded-md shadow-md bg-almond"
    >
      <p class="text-at-dark-green">{{ statusMsg }}</p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <div v-if="dataLoaded">
      <!-- General Climb Info -->
      <div
        class="flex flex-col items-center p-8 rounded-md shadow-md bg-almond relative"
      >
        <div v-if="user" class="flex absolute left-2 top-2 gap-x-2">
          <div
            class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer 
            bg-at-dark-green shadow-lg mr-5"
            @click="editMode"
          >
            <img
              src="@/assets/images/pencil-light.png"
              alt="edit icon"
              class="h-3.5 w-auto"
            />
          </div>
          <div
            @click="deleteClimb"
            class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer 
            bg-at-dark-green shadow-lg"
          >
            <img
              src="@/assets/images/trash-light.png"
              alt="delete icon"
              class="h-3.5 w-auto"
            />
          </div>
        </div>

        <img
          v-if="data.climbType === 'sport-climb'"
          src="@/assets/images/iconSportClimb.png"
          alt="sport climb icon"
          class="h-24 w-auto`"
        />
        <img
          v-if="data.climbType === 'boulder'"
          src="@/assets/images/iconBoulder.png"
          alt="sport climb icon"
          class="h-24 w-auto`"
        />

        <span
          class="mt-6 py-1.5 px-5 text-xs text-white bg-at-dark-green
        rounded-lg shadow-md"
        >
          {{ data.climbType }}</span
        >

        <div class="w-full mt-6">
          <input
            v-if="edit"
            type="text"
            class="p-2 w-full text-gray-500 focys:outline-none"
            v-model="data.climbName"
          />
          <h1 v-else class="text-at-dark-green text-2xl text-center">
            {{ data.climbName }}
          </h1>
        </div>
      </div>

      <!-- Climbs -->
      <div
        class="mt-10 p-8 rounded-md flex flex-col item-center
      bg-almond shadow-md"
      >
        <!-- Sport Climbing -->
        <div
          v-if="data.climbType === 'sport-climb'"
          class="flex flex-col gap-y-4 w-full"
        >
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.about"
            :key="index"
          >
            <div class="flex flex-1 flex-col">
              <label for="grade" class="mb-1 text-sm text-at-dark-green"
                >Sport Climb Grade</label
              >
              <input
                v-if="edit"
                id="grade"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.grade"
              />
              <p v-else>{{ item.grade }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="attempts" class="mb-1 text-sm text-at-dark-green"
                >Attempts</label
              >
              <input
                v-if="edit"
                id="attempts"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="number"
                v-model="item.attempts"
              />
              <p v-else>{{ item.attempts }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="session" class="mb-1 text-sm text-at-dark-green"
                >Session</label
              >
              <input
                v-if="edit"
                id="session"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="number"
                v-model="item.session"
              />
              <p v-else>{{ item.session }}</p>

              <div class="flex flex-1 flex-col">
                <label for="completed" class="mb-1 text-sm text-at-dark-green"
                  >Completed</label
                >
                <input
                  v-if="edit"
                  id="completed"
                  class="p-2 w-full text-gray-500 focus:outline-none"
                  type="checkbox"
                  v-model="item.completed"
                />
                <p v-else>{{ item.completed }}</p>
              </div>

              <div class="flex flex-1 flex-col">
                <label for="comments" class="mb-1 text-sm text-at-dark-green"
                  >Comments</label
                >
                <input
                  v-if="edit"
                  id="comments"
                  class="p-2 w-full text-gray-500 focus:outline-none"
                  type="text"
                  v-model="item.comments"
                />
                <p v-else>{{ item.comments }}</p>
              </div>
              <!-- <img
                @click="deleteClimbInfo(item.id)"
                v-if="edit"
                class=" absolute h-4 w-auto -left-5 cursor-pointer"
                src="@/assets/images/trash-light-green.png"
                alt="trash icon"
              /> -->
            </div>
          </div>
         <!--  <button
            @click="addClimbInfo"
            v-if="edit"
            type="button"
            class="mt-6 py-2 px-6 rounded-sm self-center text-sm
              text-white bg-at-olive-green duration-200 border-solid
              border-2 border-transparent hover:border-at-olive-green hover:bg-at-dark-green
              hover:text-white"
          >
            Add Climb
          </button> -->
        </div>

        <!-- Boulder -->
        <div v-else class="flex flex-col gap-y-4 w-full">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.about"
            :key="index"
          >
            <div class="flex flex-1 flex-col">
              <label for="grade" class="mb-1 text-sm text-at-dark-green"
                >Sport Climb Grade</label
              >
              <input
                v-if="edit"
                id="grade"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.grade"
              />
              <p v-else>{{ item.grade }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="attempts" class="mb-1 text-sm text-at-dark-green"
                >Attempts</label
              >
              <input
                v-if="edit"
                id="attempts"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="number"
                v-model="item.attempts"
              />
              <p v-else>{{ item.attempts }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="session" class="mb-1 text-sm text-at-dark-green"
                >Session</label
              >
              <input
                v-if="edit"
                id="session"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="number"
                v-model="item.session"
              />
              <p v-else>{{ item.session }}</p>

              <div class="flex flex-1 flex-col">
                <label for="completed" class="mb-1 text-sm text-at-dark-green"
                  >Completed</label
                >
                <input
                  v-if="edit"
                  id="completed"
                  class="p-2 w-full text-gray-500 focus:outline-none"
                  type="checkbox"
                  v-model="item.completed"
                />
                <p v-else>{{ item.completed }}</p>
              </div>

              <div class="flex flex-1 flex-col">
                <label for="comments" class="mb-1 text-sm text-at-dark-green"
                  >Comments</label
                >
                <input
                  v-if="edit"
                  id="comments"
                  class="p-2 w-full text-gray-500 focus:outline-none"
                  type="text"
                  v-model="item.comments"
                />
                <p v-else>{{ item.comments }}</p>
              </div>
             <!--  <img
                @click="deleteClimbInfo(item.id)"
                v-if="edit"
                class=" absolute h-4 w-auto -left-5 cursor-pointer"
                src="@/assets/images/trash-light-green.png"
                alt="trash icon"
              /> -->
            </div>
          </div>
        <!--   <button
            @click="addClimbInfo"
            v-if="edit"
            type="button"
            class="mt-6 py-2 px-6 rounded-sm self-center text-sm
              text-white bg-at-olive-green duration-200 border-solid
              border-2 border-transparent hover:border-at-olive-green hover:bg-at-dark-green
              hover:text-white"
          >
            Add Climb
          </button> -->
        </div>
      </div>
      <!-- Update -->
      <button
        @click="update"
        v-if="edit"
        type="button"
        class="mt-10 py-2 px-6 rounded-sm self-center text-sm
              text-at-dark-green bg-almond duration-200 border-solid
              border-2 border-transparent hover:border-at-olive-green hover:bg-at-dark-green
              hover:text-white"
      >
        Update Climb
      </button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { supabase } from "../supabase/init";
import { useRoute, useRouter } from "vue-router";
import { uid } from "uid";
import store from "../store/index";

export default {
  name: "view-climb",
  setup() {
    // Create data / vars
    const data = ref(null);
    const dataLoaded = ref(null);
    const errorMsg = ref(null);
    const statusMsg = ref(null);
    const route = useRoute();
    const router = useRouter();
    const user = computed(() => store.state.user);

    // Get current Id of route
    const currentId = route.params.climbId;

    // Get workout data
    const getData = async () => {
      try {
        const { data: climbs, error } = await supabase
          .from("climbs")
          .select("*")
          .eq("id", currentId);
        if (error) throw error;
        data.value = climbs[0];
        dataLoaded.value = true;
        console.log(data.value);
      } catch (error) {
        console.value = error.message;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    getData();

    // Delete workout
    const deleteClimb = async () => {
      try {
        const { error } = await supabase
          .from("climbs")
          .delete()
          .eq("id", currentId);
        if (error) throw error;
        router.push({ name: "Home" });
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };
    // Edit mode
    const edit = ref(null);

    const editMode = () => {
      edit.value = !edit.value;
    };
    // Add exercise
    const addClimbInfo = () => {
      if (data.value.climbType === "sport-climb") {
        data.value.about.push({
          id: uid(),
          grade: "",
          attempts: "",
          session: "",
          completed: "",
          comments: "",
        });
        return;
      }
      data.value.about.push({
        id: uid(),
        grade: "",
        attempts: "",
        session: "",
        completed: "",
        comments: "",
      });
    };

    // Delete Info
    const deleteClimbInfo = (id) => {
      if (data.value.about.length > 1) {
        data.value.about = data.value.about.filter((climb) => climb.id !== id);
        return;
      }
      errorMsg.value =
        "Error: Cannot remove, need to have some info to remove.";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };
    // Update Workout
    const update = async () => {
      try {
        const { error } = await supabase
          .from("climbs")
          .update({
            climbName: data.value.climbName,
            about: data.value.about,
          })
          .eq("id", currentId);
        if (error) throw error;
        edit.value = false;
        statusMsg.value = "Climb Updated, bro!";
        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);
      } catch (error) {
        errorMsg.value`Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    return {
      statusMsg,
      data,
      dataLoaded,
      errorMsg,
      edit,
      editMode,
      user,
      deleteClimb,
      addClimbInfo,
      deleteClimbInfo,
      update,
    };
  },
};
</script>

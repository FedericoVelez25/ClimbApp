<template>
  <div class="max-w-screen-md mx-auto px-4 py-10">
    <!-- Status Message -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 bg-almond rounded-md shadow-lg"
    >
      <p class="text-at-olive-green">{{ statusMsg }}</p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- Create Form -->
    <div class="p-8 flex item-start bg-almond rounded-md shadow-lg">
      <!-- Form -->
      <form @submit.prevent="createClimb" class="flex flex-col gap-y-5 w-full">
        <h1 class="text-2xl text-at-dark-green self-center">Add Climb</h1>

        <!-- Climb Name -->
        <div class="flex flex-col">
          <label for="climb-name" class="mb-1 text-sm text-at-dark-green"
            >Climb Name</label
          >
          <input
            type="text"
            required
            name=""
            id="climb-name"
            v-model="climbName"
            class="p-2 text-gray-500 focus:outline-none"
          />
        </div>

        <!-- Climb Type -->
        <div class="flex flex-col">
          <label for="climb-type" class="mb-1 text-sm text-at-dark-green"
            >Climb Type</label
          >
          <select
            @change="climbChange"
            id="climb-type"
            class="p-2 text-gray-500 focus:outline-none"
            required
            v-model="climbType"
          >
            <option value="select-climb">Select Type</option>
            <option value="sport-climb">Sport Climb</option>
            <option value="boulder">Boulder</option>
          </select>
        </div>

        <!-- Sport Climbing Inputs -->
        <div v-if="climbType === 'sport-climb'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md: flex-row"
            v-for="(item, index) in about"
            :key="index"
          >
            <div class="flex gap-10 align-middle">
              <div class="flex flex-col flex-1 lg:w-1/3">
                <label for="grade" class="mb-1 text-sm text-at-dark-green"
                  >Sport Grade</label
                >
                <input
                  required
                  type="text"
                  class="w-full text-gray-500 focus:outline-none"
                  v-model="item.grade"
                />
              </div>
              <!-- NEW ITEMS -->
              <div class="flex flex-col flex-1 lg:w-1/3">
                <label for="attempts" class="mb-1 text-sm text-at-dark-green "
                  >Attempts</label
                >
                <input
                  type="number"
                  class="w-full text-gray-500 focus:outline-none"
                  v-model="item.attempts"
                />
              </div>

              <div class="flex flex-col flex-1 lg:w-1/3">
                <label for="session" class="mb-1 text-sm text-at-dark-green "
                  >Session</label
                >
                <input
                  type="number"
                  class="w-full text-gray-500 focus:outline-none"
                  v-model="item.session"
                />
              </div>
              <!-- NEW ITEM END -->

              <div class="flex flex-col flex-1 lg:w-1/3">
                <label for="completed" class="mb-1 text-sm text-at-dark-green "
                  >Completed</label
                >
                <input
                  type="checkbox"
                  class="w-full text-gray-500 focus:outline-none"
                  v-model="item.completed"
                />
              </div>
            </div>
            <div class="flex flex-col flex-1">
              <label for="comments" class="mb-1 text-sm text-at-dark-green"
                >Comments</label
              >
              <input
                required
                type="text"
                class="p-2 w-full h-20 text-gray-500 focus:outline-none"
                v-model="item.comments"
              />
            </div>

            <!--  <img
              @click="deleteClimb(item.id)"
              src="@/assets/images/trash-light-green.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt="trash"
            /> -->
          </div>
          <!-- <button
            type="button"
            @click="addClimbInfo"
            class="mt-6 py-2 px-6 rounded-sm self-center text-sm
              text-white bg-at-olive-green duration-200 border-solid
              border-2 border-transparent hover:border-at-olive-green hover:bg-at-dark-green
              hover:text-white"
          >
            Add Climb Info
          </button> -->
        </div>

        <!-- Boulder Inputs -->
        <div v-if="climbType === 'boulder'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in about"
            :key="index"
          >
            <div class="flex gap-10 align-middle">
              <div class="flex flex-col flex-1 lg:w-1/3">
                <label for="grade" class="mb-1 text-sm text-at-dark-green"
                  >Boulder Grade</label
                >
                <input
                  required
                  type="text"
                  class="w-full text-gray-500 focus:outline-none"
                  v-model="item.grade"
                />
              </div>

              <div class="flex flex-col flex-1 lg:w-1/3">
                <label for="attempts" class="mb-1 text-sm text-at-dark-green "
                  >Attempts</label
                >
                <input
                  type="number"
                  class="w-full text-gray-500 focus:outline-none"
                  v-model="item.attempts"
                />
              </div>

              <div class="flex flex-col flex-1 lg:w-1/3">
                <label for="session" class="mb-1 text-sm text-at-dark-green "
                  >Session</label
                >
                <input
                  type="number"
                  class="w-full text-gray-500 focus:outline-none"
                  v-model="item.session"
                />
              </div>

              <div class="flex flex-col text-center flex-1 lg:w-1/3">
                <label for="completed" class="mb-1 text-sm text-at-dark-green"
                  >Completed</label
                >
                <input
                  type="checkbox"
                  class="p-2 w-full text-gray-500 focus:outline-none"
                  v-model="item.completed"
                />
              </div>
            </div>

            <div class="flex flex-col flex-1">
              <label for="comments" class="mb-1 text-sm text-at-dark-green"
                >Comments</label
              >
              <input
                required
                type="text"
                class="p-2 h-20 w-full text-gray-500 focus:outline-none"
                v-model="item.comments"
              />
            </div>

            <!--  <img
              @click="deleteClimb(item.id)"
              src="@/assets/images/trash-light-green.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt="trash"
            /> -->
          </div>
          <!--  <button
            type="button"
            @click="addClimbInfo"
            class="mt-6 py-2 px-6 rounded-sm self-center text-sm
              text-white bg-at-olive-green duration-200 border-solid
              border-2 border-transparent hover:border-at-olive-green hover:bg-at-dark-green
              hover:text-white"
          >
            Add Climb Info
          </button> -->
        </div>

        <button
          type="submit"
          class="mt-6 py-2 px-6 rounded-sm self-center text-sm
              text-white bg-at-olive-green duration-200 border-solid
              border-2 border-transparent hover:border-at-olive-green hover:bg-at-dark-green
              hover:text-white"
        >
          Add Climb
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { uid } from "uid";
import { supabase } from "../supabase/init";

export default {
  name: "create",
  setup() {
    // Create data
    const climbName = ref("");
    const climbType = ref("select-climb");
    const about = ref([1]);
    const statusMsg = ref(null);
    const errorMsg = ref(null);
    // Add climb info
    const addClimbInfo = () => {
      if (climbType.value === "sport-climb") {
        about.value.push({
          id: uid(),
          grade: "",
          attempts: "",
          session: "",
          completed: "",
          comments: "",
        });
        return;
      }
      about.value.push({
        id: uid(),
        grade: "",
        attempts: "",
        session: "",
        completed: "",
        comments: "",
      });
    };
    // Delete exercise
    const deleteClimb = (id) => {
      if (about.value.length > 1) {
        about.value = about.value.filter((climb) => climb.id !== id);
        return;
      }
      errorMsg.value =
        "Error: Cannot remove, need to have some info to remove.";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };

    // Listens for chaging of workout type input
    const climbChange = () => {
      about.value = [];
      addClimbInfo();
    };
    // Create climb
    const createClimb = async () => {
      try {
        const { error } = await supabase.from("climbs").insert([
          {
            climbName: climbName.value,
            climbType: climbType.value,
            about: about.value,
          },
        ]);
        if (error) throw error;
        statusMsg.value = "Nice dude, climb added!";
        climbName.value = null;
        climbType.value = "select-climb";
        about.value = [];
        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);

        /* Something happens with this statusMsg */
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    return {
      climbName,
      climbType,
      about,
      statusMsg,
      errorMsg,
      addClimbInfo,
      climbChange,
      deleteClimb,
      createClimb,
    };
  },
};
</script>

<template>
  <div class="flex flex-col">
    <div class="py-12">
      <div class="relative flex justify-between items-center py-3" v-for="todo in todos" :key="todo.id">
        <div class="flex items-center">
          <div @click="todo.isComplete = !todo.isComplete" class="mr-5">
            <div v-if="!todo.isComplete" class="border-2 border-grey h-8 w-8 rounded-full"></div>
            <!-- prettier-ignore -->
            <svg v-else class="w-8" viewBox="0 0 28 28"><path fill-rule="evenodd" clip-rule="evenodd" d="M28 14C28 21.732 21.732 28 14 28C6.26801 28 0 21.732 0 14C0 6.26801 6.26801 0 14 0C21.732 0 28 6.26801 28 14ZM13.8145 15.3462C13.0335 16.1273 11.7671 16.1273 10.9861 15.3462L9.13716 13.4973C8.74664 13.1068 8.11347 13.1068 7.72295 13.4973C7.33242 13.8878 7.33242 14.521 7.72295 14.9115L11.6219 18.8104C11.643 18.8362 11.6656 18.8612 11.6896 18.8853C12.0802 19.2758 12.7133 19.2758 13.1039 18.8853L20.882 11.1071C21.2726 10.7166 21.2726 10.0834 20.882 9.69292C20.4915 9.30239 19.8583 9.30239 19.4678 9.69292L13.8145 15.3462Z" fill="#006CFF"/></svg>
          </div>
          <div class="flex flex-col">
            <p class="text-dark text-lg font-semibold" :class="{ 'opacity-50': todo.isComplete }">{{ todo.title }}</p>
            <div v-if="todo.date" class="flex items-center" :class="{ 'opacity-30': !todo.isComplete, 'opacity-15': todo.isComplete }">
              <!-- prettier-ignore -->
              <svg class="fill-current w-4 mr-2" viewBox="0 0 512 512"><path d="m256 512c-141.164062 0-256-114.835938-256-256s114.835938-256 256-256 256 114.835938 256 256-114.835938 256-256 256zm0-480c-123.519531 0-224 100.480469-224 224s100.480469 224 224 224 224-100.480469 224-224-100.480469-224-224-224zm0 0"/><path d="m368 394.667969c-4.097656 0-8.191406-1.558594-11.308594-4.695313l-112-112c-3.007812-3.007812-4.691406-7.082031-4.691406-11.304687v-149.335938c0-8.832031 7.167969-16 16-16s16 7.167969 16 16v142.699219l107.308594 107.308594c6.25 6.25 6.25 16.382812 0 22.632812-3.117188 3.136719-7.210938 4.695313-11.308594 4.695313zm0 0"/></svg>
              <p class="text-dark font-semibold text-sm">{{ todo.date }}</p>
            </div>
          </div>
        </div>
        <div class="h-3 w-3 rounded-full" :class="[todosTypes.find(x => x.id === todo.type).color]"></div>
        <div class="absolute bottom-0 w-full">
          <p class="ml-12 w-full mx-auto bottom-0 bg-dark opacity-10" style="height: 1px;"></p>
        </div>
      </div>
    </div>
    <div class="flex flex-col ml-6">
      <p class="text-dark opacity-30 text-base font-bold mb-2">Lists</p>
      <div v-for="todosType in getTodosTypeCount" :key="todosType.id" class="mb-3 text-dark flex flex-col rounded-lg px-3 py-2" :class="[todosType.color]">
        <p class="text-xl font-semibold">
          {{ todosType.name }}
        </p>
        <p class="opacity-50">{{ getTodosTypeCountByType(todosType.id) }} tasks</p>
      </div>
    </div>
    <div class="absolute  bottom-0 w-16 h-16 bg-white right-0 rounded-full mr-6 mb-4" style="filter: drop-shadow(0px 10px 30px rgba(19, 28, 41, 0.2));">
      <div @click="isAddNewMenuOpen = !isAddNewMenuOpen" class="h-full flex justify-center items-center">
        <!-- prettier-ignore -->
        <svg class="text-blue w-5" viewBox="0 0 18 18"><path fill-rule="evenodd" clip-rule="evenodd" d="M10 6C10 7.10457 10.8954 8 12 8H17C17.5523 8 18 8.44772 18 9C18 9.55228 17.5523 10 17 10H12C10.8954 10 10 10.8954 10 12V17C10 17.5523 9.55228 18 9 18C8.44772 18 8 17.5523 8 17V12C8 10.8954 7.10457 10 6 10H1C0.447715 10 0 9.55228 0 9C0 8.44772 0.447715 8 1 8H6C7.10457 8 8 7.10457 8 6V1C8 0.447715 8.44772 0 9 0C9.55228 0 10 0.447715 10 1V6Z" fill="#006CFF"/></svg>
      </div>
      <transition name="fade">
        <div v-if="isAddNewMenuOpen" class="flex justify-between flex-col relative bg-white h-24 px-3 py-2 w-64 rounded-lg text-blue" style="bottom: 10.5rem; right: 11.7rem;">
          <div class="flex">
            <!-- prettier-ignore -->
            <svg class="fill-current w-5 mr-3" viewBox="0 0 384 384"><path d="m192 384c105.863281 0 192-86.128906 192-192 0-18.273438-2.550781-36.28125-7.601562-53.527344-2.488282-8.480468-11.34375-13.351562-19.847657-10.863281-8.488281 2.480469-13.34375 11.367187-10.863281 19.847656 4.183594 14.328125 6.3125 29.320313 6.3125 44.542969 0 88.222656-71.777344 160-160 160s-160-71.777344-160-160 71.777344-160 160-160c32.0625 0 62.910156 9.375 89.207031 27.105469 7.320313 4.941406 17.273438 3 22.207031-4.320313 4.9375-7.328125 3.011719-17.273437-4.316406-22.210937-31.601562-21.308594-68.632812-32.574219-107.097656-32.574219-105.863281 0-192 86.128906-192 192s86.136719 192 192 192zm0 0"/><path d="m356.6875 36.6875-164.6875 164.679688-52.6875-52.679688c-6.25-6.246094-16.375-6.246094-22.625 0-6.246094 6.25-6.246094 16.375 0 22.625l64 64c3.128906 3.128906 7.214844 4.6875 11.3125 4.6875s8.183594-1.558594 11.3125-4.6875l176-176c6.246094-6.25 6.246094-16.375 0-22.625-6.25-6.246094-16.375-6.246094-22.625 0zm0 0"/></svg>
            <p class="text-lg font-semibold">Task</p>
          </div>
          <div class="bg-grey border border-grey"></div>
          <div class="flex">
            <!-- prettier-ignore -->
            <svg class="fill-current w-5 mr-3" x="0px" y="0px" viewBox="0 0 477.867 477.867"><g><g><path d="M426.667,0H51.2C22.923,0,0,22.923,0,51.2v375.467c0,28.277,22.923,51.2,51.2,51.2h375.467 c28.277,0,51.2-22.923,51.2-51.2V51.2C477.867,22.923,454.944,0,426.667,0z M443.733,426.667c0,9.426-7.641,17.067-17.067,17.067 H51.2c-9.426,0-17.067-7.641-17.067-17.067V51.2c0-9.426,7.641-17.067,17.067-17.067h375.467c9.426,0,17.067,7.641,17.067,17.067 V426.667z"/></g></g><g><g><path d="M153.6,102.4h-51.2c-9.426,0-17.067,7.641-17.067,17.067s7.641,17.067,17.067,17.067h51.2 c9.426,0,17.067-7.641,17.067-17.067S163.026,102.4,153.6,102.4z"/></g></g><g><g><path d="M375.467,102.4h-153.6c-9.426,0-17.067,7.641-17.067,17.067s7.641,17.067,17.067,17.067h153.6 c9.426,0,17.067-7.641,17.067-17.067S384.892,102.4,375.467,102.4z"/></g></g><g><g><path d="M153.6,221.867h-51.2c-9.426,0-17.067,7.641-17.067,17.067S92.974,256,102.4,256h51.2c9.426,0,17.067-7.641,17.067-17.067 S163.026,221.867,153.6,221.867z"/></g></g><g><g><path d="M375.467,221.867h-153.6c-9.426,0-17.067,7.641-17.067,17.067S212.441,256,221.867,256h153.6 c9.426,0,17.067-7.641,17.067-17.067S384.892,221.867,375.467,221.867z"/></g></g><g><g><path d="M153.6,341.333h-51.2c-9.426,0-17.067,7.641-17.067,17.067s7.641,17.067,17.067,17.067h51.2 c9.426,0,17.067-7.641,17.067-17.067S163.026,341.333,153.6,341.333z"/></g></g><g><g><path d="M375.467,341.333h-153.6c-9.426,0-17.067,7.641-17.067,17.067s7.641,17.067,17.067,17.067h153.6 c9.426,0,17.067-7.641,17.067-17.067S384.892,341.333,375.467,341.333z"/></g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g></svg>
            <!-- <svg class="fill-current w-5 mr-3" viewBox="0 0 384 384"><path d="m192 384c105.863281 0 192-86.128906 192-192 0-18.273438-2.550781-36.28125-7.601562-53.527344-2.488282-8.480468-11.34375-13.351562-19.847657-10.863281-8.488281 2.480469-13.34375 11.367187-10.863281 19.847656 4.183594 14.328125 6.3125 29.320313 6.3125 44.542969 0 88.222656-71.777344 160-160 160s-160-71.777344-160-160 71.777344-160 160-160c32.0625 0 62.910156 9.375 89.207031 27.105469 7.320313 4.941406 17.273438 3 22.207031-4.320313 4.9375-7.328125 3.011719-17.273437-4.316406-22.210937-31.601562-21.308594-68.632812-32.574219-107.097656-32.574219-105.863281 0-192 86.128906-192 192s86.136719 192 192 192zm0 0"/><path d="m356.6875 36.6875-164.6875 164.679688-52.6875-52.679688c-6.25-6.246094-16.375-6.246094-22.625 0-6.246094 6.25-6.246094 16.375 0 22.625l64 64c3.128906 3.128906 7.214844 4.6875 11.3125 4.6875s8.183594-1.558594 11.3125-4.6875l176-176c6.246094-6.25 6.246094-16.375 0-22.625-6.25-6.246094-16.375-6.246094-22.625 0zm0 0"/></svg> -->
            <p class="text-lg font-semibold">List</p>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import { v4 as uuidv4 } from "uuid";

export default {
  data() {
    return {
      todos: [
        {
          id: uuidv4(),
          title: "Start making a presentation",
          isComplete: false,
          date: new Date().toLocaleTimeString("en-US", {
            hour: "2-digit",
            minute: "2-digit",
            hour12: false
          }),
          type: 1
        },
        {
          id: uuidv4(),
          title: "Pay for rent",
          isComplete: false,
          date: new Date().toLocaleTimeString("en-US", {
            hour: "2-digit",
            minute: "2-digit",
            hour12: false
          }),
          type: 2
        },
        {
          id: uuidv4(),
          title: "Buy a milk",
          isComplete: false,
          type: 4
        },
        {
          id: uuidv4(),
          title: "Don’t forget to pick up Mickael from school",
          isComplete: false,
          date: new Date().toLocaleTimeString("en-US", {
            hour: "2-digit",
            minute: "2-digit",
            hour12: false
          }),
          type: 3
        },
        {
          id: uuidv4(),
          title: "Buy a chocolate for Charlotte",
          isComplete: false,
          type: 1
        }
      ],
      todosTypes: [
        {
          id: 1,
          name: "Inbox",
          color: "bg-grey"
        },
        {
          id: 2,
          name: "Work",
          color: "bg-green"
        },
        {
          id: 3,
          name: "Shopping",
          color: "bg-red"
        },
        {
          id: 4,
          name: "Family",
          color: "bg-yellow"
        },
        {
          id: 5,
          name: "Personal",
          color: "bg-purple"
        }
      ],
      isAddNewMenuOpen: false
    };
  },
  methods: {
    getTodosTypeCountByType(type) {
      return this.todos.filter(x => x.type === type).length;
    }
  },
  computed: {
    //Get only types that have todos.
    getTodosTypeCount() {
      return this.todosTypes.filter(x => this.getTodosTypeCountByType(x.id));
    }
  }
};
</script>

<style lang="scss" scoped>
.opacity-30 {
  opacity: 0.3;
}

.opacity-10 {
  opacity: 0.1;
}

.opacity-15 {
  opacity: 0.15;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>

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
    <div class="absolute flex justify-center items-center bottom-0 w-16 h-16 bg-white right-0 rounded-full mr-6 mb-4">
      <!-- prettier-ignore -->
      <svg class="text-blue w-5" viewBox="0 0 18 18"><path fill-rule="evenodd" clip-rule="evenodd" d="M10 6C10 7.10457 10.8954 8 12 8H17C17.5523 8 18 8.44772 18 9C18 9.55228 17.5523 10 17 10H12C10.8954 10 10 10.8954 10 12V17C10 17.5523 9.55228 18 9 18C8.44772 18 8 17.5523 8 17V12C8 10.8954 7.10457 10 6 10H1C0.447715 10 0 9.55228 0 9C0 8.44772 0.447715 8 1 8H6C7.10457 8 8 7.10457 8 6V1C8 0.447715 8.44772 0 9 0C9.55228 0 10 0.447715 10 1V6Z" fill="#006CFF"/></svg>
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
      ]
    };
  },
  methods: {
    getTodosTypeCountByType(type) {
      return this.todos.filter(x => x.type === type).length;
    }
  },
  computed: {
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
</style>

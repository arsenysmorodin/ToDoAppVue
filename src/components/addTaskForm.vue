<template>
  <div class="add-task">
    <input class="add-task__input" :placeholder="currentPlaceholder" @keyup.enter="addTask" type="text"
      v-model="newTodo" />
    <select v-if="this.$route.path == ('/' || '')" class="add-task__select" name="list" @change="changeList($event)">
      <option class="add-task__option" :value="item" v-for="item in $store.state.lists" :key="item">
        {{ item }}
      </option>
    </select>
    <select v-else class="add-task__select" name="list" @change="changeList($event)">
      <option class="add-task__option" :value="choosenListByPath">
        {{ choosenListByPath }}
      </option>
    </select>
    <button class="add-task__btn" @click="addTask"></button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      list: "Default",
      task: {
        name: "",
        list: "",
      },
      // placeholders: ['Buy a bread...', 'Build a house...', 'Create a document...', 'Find a job...', 'Earn 1 million...', 'Learn Eanglish...'],
      currentPlaceholder: "Add task...",
    };
  },
  computed: {
    choosenListByPath() {
      return this.$route.path.slice(1);
    },
  },
  methods: {
    addTask() {
      this.task.name = this.newTodo;
      this.task.list = this.list;
      this.newTodo = "";
      if (!this.task.name.length) {
        this.currentPlaceholder = "Input something!"
      } else {
        this.$store.dispatch("addTask", this.task);
        this.currentPlaceholder = "Add task...";
      }

    },
    changeList(event) {
      this.list = event.target.value;
    },

    //TODO!
    // changePlaceholder() {
    //   for (let i = 0; i < this.placeholders.length; i++) {
    //     setInterval(() => {
    //       this.currentPlaceholder = this.placeholders[i];
    //       if (i = this.placeholders.length - 1) {
    //         i = 0;
    //       }
    //     }, 2000)
    //   }
    // }
  },
  mounted() {
    // this.changePlaceholder();
  },
  updated() {
    let routePath = this.$route.path;
    if (routePath == '' || routePath == '/') {
      routePath = 'Default';
    } else {
      routePath = routePath.slice(1);
    }
    this.list = routePath
  },
};
</script>

<style scoped lang="scss">
@import "@/style/mixins.scss";

.add-task {
  margin: 0 auto;
  display: flex;
  gap: 20px;

  &__input {
    background: $background-color;
    color: $font-color;
    font-size: 16px;
    width: 575px;
    height: 60px;
    border-radius: 16px;
    border: none;
    padding: 20px 20px 20px 40px;
    @include shadowInner;

    &:focus {
      outline: none;
    }

    &::placeholder {
      color: $font-color;
    }
  }

  &__select {
    height: 60px;
    font-size: 16px;
    border: none;
    color: $font-color;
    padding: 10px;
    background: $background-color;

    &:focus {
      outline: none;
    }
  }

  &__option {
    color: $font-color;
  }

  &__btn {
    height: 60px;
    width: 60px;
    border: none;
    border-radius: 50%;
    color: #757575;
    background-color: $background-color;
    background-image: url("@/image/plus.png");
    background-size: 24px;
    background-repeat: no-repeat;
    background-position: center;
    @include shadowOuter;
  }
}
</style>

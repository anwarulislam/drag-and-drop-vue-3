<template>
  <header class="__heading-area">
    <h3>People Sorting System</h3>
    <button
      type="button"
      class="button is-primary"
      @click="isModalVisible = true"
    >
      Start Sorting
    </button>
  </header>

  <main>
    <div class="table-area">
      <table class="table">
        <thead>
          <tr>
            <th>Email</th>
            <th>Potatoes</th>
            <th>Tags</th>
            <th>Full name</th>
            <th>Location</th>
          </tr>
        </thead>

        <draggable
          class="list-group"
          tag="transition-group"
          :component-data="{
            tag: 'tbody',
            type: 'transition-group',
            name: !drag ? 'flip-list' : null,
          }"
          v-model="list"
          v-bind="dragOptions"
          @start="drag = true"
          @end="
            drag = false;
            onDrop();
          "
          item-key="id"
        >
          <template #item="{ element }">
            <Person :person="element" />
          </template>
        </draggable>
      </table>
    </div>
  </main>

  <Modal v-show="isModalVisible" @close="isModalVisible = false">
    <template v-slot:header>
      <h5>How many people?</h5>
    </template>

    <template v-slot:body>
      <div>
        <p class="text-muted">
          Enter a number of how many people you want to add to the list.
        </p>
        <input class="input" type="number" />
      </div>
    </template>

    <template v-slot:footer>
      <div class="action-button">
        <button type="button" class="button is-grey" @click="addPeople">
          Cancel
        </button>
        <button type="button" class="button is-primary" @click="addPeople">
          Start
        </button>
      </div>
    </template>
  </Modal>

  <Modal v-show="isResultModal" @close="isResultModal = false">
    <template v-slot:header> Header </template>

    <template v-slot:body> You have successfully sorted the list! </template>

    <template v-slot:footer> Footer </template>
  </Modal>
</template>

<script>
import draggable from "vuedraggable";
import Modal from "./../components/Modal.vue";
import Person from "./../components/Person.vue";
import data from "./../assets/data.json";

export default {
  name: "transition-example-2",
  display: "Transitions",
  order: 7,
  components: {
    draggable,
    Modal,
    Person,
  },
  data() {
    return {
      list: data.slice(0, 20),
      drag: false,
      isModalVisible: false,
      isResultModal: false,
    };
  },
  methods: {
    onDrop() {
      this.isListSorted(this.list);
    },
    isListSorted(array) {
      let isSorted = false;
      for (let i = 0; i < array.length - 1; i++) {
        if (array[i].potatoes > array[i + 1].potatoes) {
          isSorted = true;
        } else {
          isSorted = false;
          break;
        }
      }
      return isSorted;
    },
    onSort() {
      this.isResultModal = true;
    },
    getRandomIndex(min = 1, max = 500, len = 10) {
      const numbers = [];
      while (numbers.length < len) {
        const randomNumber = Math.floor(Math.random() * (max - min + 1)) + min;
        if (numbers.indexOf(randomNumber) === -1) {
          numbers.push(randomNumber);
        }
      }
      return numbers;
    },
  },
  computed: {
    dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost",
      };
    },
  },
};
</script>

<style>
.__heading-area {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2.2rem;
}

.table-area {
  background: #ffffff;
  border: 1px solid #dddddd;
  box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.101972);
  border-radius: 5px;
}

.flip-list-move {
  transition: transform 0.5s;
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.list-group {
  min-height: 20px;
}
.list-group-item {
  background: #fff;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-bottom: 5px;
  padding: 5px;
  cursor: move;
}
.list-group-item i {
  cursor: pointer;
}
</style>
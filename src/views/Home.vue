<template>
  <header class="__heading-area">
    <div class="__title">
      <h3>People Sorting System</h3>

      <div class="__timer" v-if="seconds > 0">
        <span>{{ secondsToWatch(seconds) }}</span>
      </div>
    </div>

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
      <template v-if="list.length">
        <div class="__area-heading">{{ list.length }} people in the list</div>
        <table class="table">
          <thead>
            <tr>
              <th class="email-td">Email</th>
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
        <div class="__area-heading"></div>
      </template>

      <template v-else>
        <div style="padding: 2rem; text-align: center">
          Please select start sorting and input the number of people you want to
          sort.
        </div>
      </template>
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
        <input
          class="input"
          type="number"
          v-model="numberOfPeople"
          v-on:keyup.enter="addPeople(numberOfPeople)"
        />
      </div>
    </template>

    <template v-slot:footer>
      <div class="action-button">
        <button
          style="margin-right: 0.55rem"
          type="button"
          class="button is-grey"
          @click="isModalVisible = false"
        >
          Cancel
        </button>
        <button
          type="button"
          class="button is-primary"
          @click="addPeople(numberOfPeople)"
        >
          Start
        </button>
      </div>
    </template>
  </Modal>

  <Modal
    :isHideButton="true"
    v-show="isResultModal"
    @close="isResultModal = false"
  >
    <template v-slot:header> <h5>Result</h5> </template>

    <template v-slot:body>
      <div class="result-card">
        <img
          src="https://dl.dropboxusercontent.com/s/e1t2hhowjcrs7f5/100daysui_100icon.png"
        />

        <h5>Congratulations</h5>
        <p>
          You have successfully completed sorting the list within
          {{ secondsToWatch(seconds) }}
        </p>
      </div>
    </template>

    <template v-slot:footer>
      <div class="action-button">
        <button
          type="button"
          class="button is-primary"
          @click="isResultModal = false"
        >
          Ok
        </button>
      </div>
    </template>
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
      list: [],
      drag: false,
      isModalVisible: false,
      isResultModal: false,
      numberOfPeople: null,
      seconds: 0,
      timer: null,
    };
  },
  methods: {
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
    addPeople(numberOfPeople) {
      this.isModalVisible = false;
      const people = [];
      const numbers = this.getRandomIndex(1, 500, numberOfPeople);
      for (let i = 0; i < numberOfPeople; i++) {
        people.push(data[numbers[i]]);
      }
      this.list = people;
      this.startTimer();
    },
    startTimer() {
      this.stopTimer();
      this.seconds = 0;
      this.timer = setInterval(() => {
        this.seconds++;
      }, 1000);
    },
    stopTimer() {
      clearInterval(this.timer);
    },
    onDrop() {
      if (this.isListSorted(this.list)) {
        this.isResultModal = true;
        this.stopTimer();
      }
    },
    isListSorted(array) {
      let isSorted = false;
      for (let i = 0; i < array.length - 1; i++) {
        if (array[i].potatoes >= array[i + 1].potatoes) {
          isSorted = true;
        } else {
          isSorted = false;
          break;
        }
      }
      return isSorted;
    },
    secondsToWatch(seconds) {
      const minutes = Math.floor(seconds / 60);
      const secondsLeft = seconds % 60;
      return `${minutes < 10 ? "0" : ""}${minutes}:${
        secondsLeft < 10 ? "0" : ""
      }${secondsLeft}`;
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
  mounted() {
    this.addPeople(10);
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

.__title {
  display: flex;
  align-items: center;
}

.__title h3 {
  margin-right: 1rem;
}

.__timer {
  display: inline-block;
  background: #ddd;
  padding: 0.5rem;
  border-radius: 6px;
  min-width: 50px;
  text-align: right;
  font-size: 1.2rem;
  color: #676464;
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
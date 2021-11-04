<template>
  <div class="row">
    <div class="col-6">
      <h3>Transition</h3>
      <draggable
        class="list-group"
        tag="transition-group"
        :component-data="{
          tag: 'ul',
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
        item-key="order"
      >
        <template #item="{ element }">
          <li class="list-group-item">
            <i
              :class="
                element.fixed ? 'fa fa-anchor' : 'glyphicon glyphicon-pushpin'
              "
              @click="element.fixed = !element.fixed"
              aria-hidden="true"
            ></i>
            {{ element.name }}
          </li>
        </template>
      </draggable>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";

const users = [
  {
    email: "jo@jo.com",
    name: "JoJo 11",
    potatoes: 11,
    tags: ["potato", "vegetable"],
    location: "London",
  },
  {
    email: "nono@jo.com",
    name: "Aoaao 19",
    potatoes: 19,
    tags: ["potato", "vegetable"],
    location: "London",
  },
  {
    email: "bro@jo.com",
    name: "Kaoaoo 15",
    potatoes: 15,
    tags: ["potato", "vegetable"],
    location: "London",
  },
];

export default {
  name: "transition-example-2",
  display: "Transitions",
  order: 7,
  components: {
    draggable,
  },
  data() {
    return {
      list: users,
      drag: false,
    };
  },
  methods: {
    onDrop(evt) {
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
.button {
  margin-top: 35px;
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
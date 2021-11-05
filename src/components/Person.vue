<template>
  <tr>
    <td>
      <div class="email-cell">
        {{ person.email }}
      </div>
    </td>
    <td>{{ person.potatoes }}</td>
    <td>
      <div class="tag">{{ person.tags }}</div>
    </td>
    <td>{{ person.name }}</td>
    <td>{{ person.location }}</td>
  </tr>
</template>

<script>
export default {
  name: "Person",
  props: {
    person: {},
  },
};
</script>

<style scoped>
tr {
  cursor: move;
}

td {
  padding-top: 10px;
  padding-bottom: 10px;
}
</style>

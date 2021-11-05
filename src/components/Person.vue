<template>
  <tr>
    <td class="email-td">
      <div class="email-cell">
        <div class="form-group">
          <input type="checkbox" :id="'d' + person.id" />
          <label :for="'d' + person.id">{{ person.email }}</label>
        </div>

        <div class="icon" style="margin-left: auto">
          <svg
            width="8"
            height="12"
            viewBox="0 0 8 12"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path d="M1 11L6 6L1 1" stroke="#999999" stroke-width="2" />
          </svg>
        </div>
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

.email-cell {
  display: flex;
  align-items: center;
}

/* CHECKBOX */

.form-group {
  display: block;
}

.form-group input {
  padding: 0;
  height: initial;
  width: initial;
  margin-bottom: 0;
  display: none;
  cursor: pointer;
}

.form-group label {
  position: relative;
  cursor: pointer;
}

.form-group label:before {
  content: "";
  -webkit-appearance: none;
  background-color: transparent;
  border: 2px solid #dddddd;
  border-radius: 5px;
  padding: 10px;
  display: inline-block;
  position: relative;
  vertical-align: middle;
  cursor: pointer;
  margin-right: 1rem;
}

.form-group input:checked + label:before {
  background: #ff8d00;
  border-color: #ff8d00;
}

.form-group input:checked + label:after {
  content: "";
  display: block;
  position: absolute;
  top: 3px;
  left: 9px;
  width: 4px;
  height: 10px;
  border: solid #fff;
  border-width: 0 3px 3px 0;
  transform: rotate(45deg);
}
</style>

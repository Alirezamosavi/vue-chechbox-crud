<template>
  <div class="container">


    
    <div class="text-uppercase text-bold">id selected: {{ selected }}</div>
    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>
            <label class="form-checkbox">
              <input type="checkbox" v-model="selectAll" @click="select" />
              <i class="form-icon"></i>
            </label>
          </th>
          <th>id</th>
          <th>name</th>
          <th>email</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="i in items" :key="i.id">
          <td>
            <label class="form-checkbox">
              <input type="checkbox" :value="i.id" v-model="selected" />
              <i class="form-icon"></i>
            </label>
          </td>
          <td>{{ i.id }}</td>
          <td>{{ i.name }}</td>
          <td>{{ i.email }}</td>
        </tr>
      </tbody>
    </table>

    <div id="root" class="container">

  <h2>Vue Select &amp; Checkbox Interaction Play</h2>

  <!-- checkbox limiter -->
  <select class="limiter" v-model="tickLimit" @change="updateLimit">
    <option value="1">Single</option>
    <option value="2">Two</option>
    <option value="3">Three</option>
    <option value="4">Four</option>
  </select>

  <!-- checkboxes -->
  <div class="boxes">
    <span v-for="box in boxes" class="checkybox"  :key="box.id">
      <input type="checkbox" v-model="box.checked" @change="updateBoxes" :value="box.value" :checked="box.checked" :disabled="box.disabled">
      <label :for="box.value">{{ box.value }}</label>
    </span>

  </div>
  
  <button @click="nukeTicks">Clear all</button>

</div>
  </div>
</template>
 
<script>
export default {
  //  imageSelected
  data() {
    return {
      tickLimit: 1,
    ticks: [],
    boxes: [
      { value: 'first', checked: false, disabled: false },
      { value: 'second', checked: false, disabled: false },
      { value: 'third', checked: false, disabled: false },
      { value: 'fourth', checked: false, disabled: false },
      { value: 'fifth', checked: false, disabled: false },
    ],
      items: [
        {
          id: "id1",
          name: "John Doe",
          email: "email@example.com",
        },
        {
          id: "id2",
          name: "Jone Doe",
          email: "email2@example.com",
        },
      ],
      selected: [],
      selectAll: false,
    };
  },
  methods: {
    select() {
      this.selected = [];
      if (!this.selectAll) {
        for (let i in this.items) {
          this.selected.push(this.items[i].id);
        }
      }
    },
     updateLimit() {
      if (this.ticks.length < this.tickLimit) {
        // keep current selection and activate remaining options
        this.boxes.forEach(box => {
          if (box.disabled) box.disabled = false;
        });
      }

      if (this.ticks.length > this.tickLimit) {
        // revert last selections (roughly) and update boxes based on new limit
        let tickDiff = this.ticks.length - this.tickLimit;
        for (var i = this.ticks.length - 1; i >= (this.ticks.length - tickDiff); i--) {
          this.boxes.find(box => {
            if (box.value == this.ticks[i].value) box.checked = false;
          })
        };
      }

      this.updateBoxes();
    },

    updateBoxes() {
      // update the number of ticks...
      this.ticks = this.boxes.filter(box => box.checked);

      // re-enable checkboxes if back under the limit...
      if (this.ticks.length < this.tickLimit) {
        this.boxes.forEach(box => {
          if (box.checked == false) box.disabled = false;
        });
      }

      // disable empty checkboxes if at the limit...
      if (this.ticks.length == this.tickLimit) {
        this.boxes.forEach(box => {
          if (box.checked == false) box.disabled = true;
        });
      }
    },

    nukeTicks() {
      this.boxes.forEach(box => {
        box.checked = false;
        box.disabled = false;
      });
      this.ticks = [];
    }
  
    
  },
};
</script>

<style>
#root {
  padding-top: 42px;
}

.boxes {
  display: block;
  margin: 18px 0;
}

.checkybox {
  display: block;
}

input[type="checkbox"] + label {
  color: #000;
}

input[type="checkbox"]:checked + label {
  font-weight: bold;
}

input[type="checkbox"]:disabled + label {
    color: #999;
}
</style>
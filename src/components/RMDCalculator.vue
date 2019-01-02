<template>
  <div class="container">
    <div col="5" class="m-5 p-10">
      <h4>Basic RMD Calculator</h4>
      <form class="p-2">
        <div class="form-group">
          <label for="formGroupExampleInput">End of Last Year RMD Assets</label>
          <div class="input-group">
            <div class="input-group-prepend">
              <span class="input-group-text" id="basic-addon1">$</span>
            </div>
            <input
              v-model="accountValue"
              class="form-control"
              id="formGroupExampleInput"
              max="10000000"
              @keyup="calculateRMD();"
            />
          </div>
        </div>
        <div class="form-group">
          <label for="formGroupExampleInput2">End of Last Year Age</label>
          <input
            max="120"
            @keyup="
              calculateRMD();
              validAge();
            "
            @change="calculateRMD();"
            v-model.number="age"
            class="form-control"
          />
        </div>

        <div class="form-group"></div>
      </form>
      <div>
        <div v-if="alert" class="alert alert-danger" role="alert">
          <strong> End of Year Age </strong> must be at least
          <strong>70</strong>.
        </div>
        <div v-else class="alert alert-primary" role="alert">
          <h3>${{ formatCurrency(rmd) }}</h3>
          <p v-if="rmdFactor">
            At age <strong>{{ age }}</strong
            >, your RMD Factor is <strong>{{ rmdFactor }}</strong>
          </p>
        </div>
      </div>
      <div>
        <button @click="toggleTable" class="btn btn-info">Show Schedule</button>
        <table v-if="tableShown" class="table table-hover">
          <tr>
            <th>Age</th>
            <th>Factor</th>
          </tr>
          <tr v-for="factor in rmdSchedule" :key="factor.age">
            <td>{{ factor.age }}</td>
            <td>{{ factor.factor }}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "RMDCalculator",
  methods: {
    validAge() {
      if (this.age < 70) {
        this.alert = true;
      } else {
        this.alert = false;
      }
    },
    calculateRMD() {
      let age = this.age; // get age from our age data property
      let factor = 0; // set a default value
      this.rmdSchedule.forEach(row => {
        if (this.age === row.age) {
          factor = row.factor;
        } else if (this.age > 115) {
          factor = 1.9; // the maximum RMD factor
        }
      });
      this.rmdFactor = factor;
      this.rmd = this.accountValue / factor;
    },
    formatCurrency(value) {
      // To format account values
      let val = (value / 1).toFixed(0).replace(",", ",");
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
    toggleTable() {
      this.tableShown = !this.tableShown;
    }
  },
  data() {
    return {
      rmd: 0,
      tableShown: false,
      alert: false,
      age: 72,
      rmdFactor: 0,
      accountValue: 0,
      rmdSchedule: [
        { age: 70, factor: 27.4 },
        { age: 71, factor: 26.5 },
        { age: 72, factor: 25.6 },
        { age: 73, factor: 24.7 },
        { age: 74, factor: 23.8 },
        { age: 75, factor: 22.9 },
        { age: 76, factor: 22.0 },
        { age: 77, factor: 21.2 },
        { age: 78, factor: 20.3 },
        { age: 79, factor: 19.5 },
        { age: 80, factor: 18.7 },
        { age: 81, factor: 17.9 },
        { age: 82, factor: 17.1 },
        { age: 83, factor: 16.3 },
        { age: 84, factor: 15.5 },
        { age: 85, factor: 14.8 },
        { age: 86, factor: 14.1 },
        { age: 87, factor: 13.4 },
        { age: 88, factor: 12.7 },
        { age: 89, factor: 12.0 },
        { age: 90, factor: 11.4 },
        { age: 91, factor: 10.8 },
        { age: 92, factor: 10.2 },
        { age: 93, factor: 9.6 },
        { age: 94, factor: 9.1 },
        { age: 95, factor: 8.6 },
        { age: 96, factor: 8.1 },
        { age: 97, factor: 7.6 },
        { age: 98, factor: 7.1 },
        { age: 99, factor: 6.7 },
        { age: 100, factor: 6.3 },
        { age: 101, factor: 5.9 },
        { age: 102, factor: 5.5 },
        { age: 103, factor: 5.2 },
        { age: 104, factor: 4.9 },
        { age: 105, factor: 4.5 },
        { age: 106, factor: 4.2 },
        { age: 107, factor: 3.9 },
        { age: 108, factor: 3.7 },
        { age: 109, factor: 3.4 },
        { age: 110, factor: 3.1 },
        { age: 111, factor: 2.9 },
        { age: 112, factor: 2.6 },
        { age: 113, factor: 2.4 },
        { age: 114, factor: 2.1 },
        { age: 115, factor: 1.9 }
      ]
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style></style>

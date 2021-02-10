<template>
  <div class="home">
    <h1>BRTC Report</h1>
    <hr />
    <!-- Instrument Ammeter -->
    <div class="section">
      <h3>Environmental Conditions</h3>
      <div class="input-box">
        <label>Room Temperature: </label>
        <input type="text" v-model="temperature" placeholder="in Celcius" />
      </div>
    </div>
    <div class="section">
      <h3>Instrument Detail</h3>
      <div class="input-box">
        <label>Description: </label>
        <input type="text" v-model="description" placeholder="Description" />
      </div>
      <div class="input-box">
        <label>Model: </label>
        <input type="text" v-model="model" placeholder="Model no." />
      </div>
      <div class="input-box">
        <label>Brand: </label>
        <input type="text" v-model="brand" placeholder="Brand name" />
      </div>
      <div class="input-box">
        <label>Serial No.: </label>
        <input type="text" v-model="serial" placeholder="Serial no" />
      </div>
    </div>
    <div class="section">
      <h3>Collected Data</h3>
      <table>
        <thead>
          <tr>
            <th>Obs No.</th>
            <th>Range</th>
            <th>Standard Meter Value</th>
            <th>Test Meter Value</th>
            <th>Percent Deviation</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(data, index) in readings" :key="data.id">
            <td>
              {{ index + 1 }}
            </td>
            <td>
              {{ data.range }}
            </td>
            <td>
              {{ data.std_data }}
            </td>
            <td>
              {{ data.test_data }}
            </td>
            <td>
              {{ data.percent_error }}
            </td>
            <td>
              <button @click="deleteReading(data.id)">-</button>
            </td>
          </tr>
          <tr>
            <td>
              <input disabled type="text" />
            </td>
            <td>
              <input
                type="text"
                v-model="reading.range"
                placeholder="in Volt"
              />
            </td>
            <td>
              <input
                type="text"
                v-model="reading.std_data"
                placeholder="in Volt"
              />
            </td>
            <td>
              <input
                type="text"
                v-model="reading.test_data"
                placeholder="in Volt"
              />
            </td>
            <td>
              <input disabled type="text" />
            </td>
            <td>
              <button @click="addReading">+</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      temperature: "",
      description: "",
      model: "",
      brand: "",
      serial: "",
      reading: {
        id: '',
        range: "",
        std_data: "",
        test_data: "",
        percent_error: "",
      },
      readings: []
    };
  },
  methods: {
    addReading(){
      var d = new Date();
      this.reading.id = d.getTime()
      this.reading.std_data = parseFloat(this.reading.std_data)
      this.reading.test_data = parseFloat(this.reading.test_data)
      if (!this.reading.std_data){
        this.reading.percent_error='NaN'
      } else {
        this.reading.percent_error=100*(this.reading.std_data-this.reading.test_data)/this.reading.std_data
        this.reading.percent_error.toFixed(2)
      }
      // Push to readings
      this.readings.push({
        id: this.reading.id,
        range: this.reading.range,
        std_data: this.reading.std_data,
        test_data: this.reading.test_data,
        percent_error: this.reading.percent_error
      })
      // Reset data
      this.reading.id= ''
      this.reading.range= ''
      this.reading.std_data= ''
      this.reading.test_data= ''
      this.reading.percent_error= ''
    },
    deleteReading(id){
      console.log(id)
    }
  },
  components: {},
};
</script>

<style>
.home{
  margin-bottom: 100px;
}
.section {
  max-width: 900px;
  margin: 10px auto;
  padding: 10px;
  background: #f2f2f2;
  border: 1px solid #bbb;
  border-radius: 10px;
}
.section h3{
  font-size: 24px;
}
.input-box {
  display: flex;
  margin: 10px auto;
  font-size: 18px;
}
.input-box label {
  width: 30%;
}
.input-box input {
  width: 70%;
  border: 0;
  border-bottom: 1px solid #bbb;
  padding: 5px;
}
.section table td {
  /* width: 100%; */
  padding: 10px;
}
.section table td{
  text-align: center;
}
.section table td input {
  text-align: center;
  width: 100%;
}
</style>
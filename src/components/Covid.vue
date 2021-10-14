<template>
  <div class="covid">
    <v-container>
      <v-row>
        <h2>
          Covid 19 Global & Indonesia Cases Dashboard And Data Visualisation
        </h2>
      </v-row>
      <v-row>
        <select @change="getData()" name="" id="" v-model="country">
          <option value="">Select Country</option>
          <option
            v-for="(CountryList, index) in countryList"
            :key="index"
            :value="CountryList"
          >
            {{ CountryList }}
          </option>
        </select>
      </v-row>
      <v-row>
        <div class="panel">
          <div class="case">
            <h3>Cases</h3>
            <h4>{{ cases }}</h4>
          </div>

          <div class="death">
            <h3>Death</h3>
            <h4>{{ death }}</h4>
          </div>

          <div class="recovered">
            <h3>Recovered</h3>
            <h4>{{ recovered }}</h4>
          </div>
        </div>
      </v-row>
      <v-row>
        <v-col md="7">
          <div class="echart-wrap">
            <div id="main" style="width: 900px; height: 700px">
              Main
            </div>
          </div>
        </v-col>
        <v-col md="5">
          <div id="main2" style="width: 500px; height: 300px">
            Main
          </div>
        </v-col>
      </v-row>
      <v-row>
        <v-col md="7">
          <iframe
            src="https://ourworldindata.org/grapher/total-cases-covid-19?tab=map"
            width="680px"
            height="480px"
          ></iframe>
        </v-col>
        <v-col md="5">
          <div id="main3" style="width: 500px; height: 300px">
            Main3
          </div>
          <div id="main4" style="width: 500px; height: 300px">
            Main4
          </div>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import axios from "axios";
import * as echarts from "echarts";
export default {
  name: "Covid",
  data: () => ({
    country: "",
    countryList: [],
    cases: "",
    death: "",
    recovered: "",
  }),
  mounted() {
    this.getCountry();

    const myChart = echarts.init(document.getElementById("main"));
    axios
      .get("https://apicovid19indonesia-v2.vercel.app/api/indonesia/provinsi")
      .then(function(data) {
        const provinsi = [];
        data.data.forEach((element) => {
          provinsi.push(element["provinsi"]);
        });
        const sembuh = [];
        data.data.forEach((element) => {
          sembuh.push(element["sembuh"]);
        });
        const rawat = [];
        data.data.forEach((element) => {
          rawat.push(element["dirawat"]);
        });
        const meninggal = [];
        data.data.forEach((element) => {
          meninggal.push(element["meninggal"]);
        });
        myChart.setOption({
          title: {
            text: "Cases Indonesia",
            color: "#fff",
          },
          tooltip: {
            trigger: "axis",
            axisPointer: {
              // Use axis to trigger tooltip
              type: "shadow", // 'shadow' as default; can also be 'line' or 'shadow'
            },
          },
          legend: {
            data: ["Treated", "Recovered", "Deaths"],
          },
          grid: {
            left: "3%",
            right: "4%",
            bottom: "3%",
            containLabel: true,
          },
          xAxis: {
            type: "value",
          },
          yAxis: {
            type: "category",
            data: provinsi,
          },
          series: [
            {
              name: "Treated",
              type: "bar",
              stack: "total",
              emphasis: {
                focus: "series",
              },
              data: sembuh,
            },
            {
              name: "Recovered",
              type: "bar",
              stack: "total",
              emphasis: {
                focus: "series",
              },
              data: rawat,
            },
            {
              name: "Deaths",
              type: "bar",
              stack: "total",
              emphasis: {
                focus: "series",
              },
              data: meninggal,
            },
          ],
        });
      });

    // Chart2
    axios
      .get("https://apicovid19indonesia-v2.vercel.app/api/indonesia/harian")
      .then(function(data) {
        const tanggal = [];
        const newTanggal = [];
        data.data.forEach((element, index) => {
          tanggal.push(element["tanggal"]);
          let timestamp = new Date(tanggal[index]).getTime();
          let Day = new Date(timestamp).getDate();
          let Mounth = new Date(timestamp).getMonth() + 1;
          let Year = new Date(timestamp).getFullYear();
          let NewDateFormat = `${Day}/${Mounth}/${Year}`;
          newTanggal.push(NewDateFormat);
        });

        const positif = [];
        data.data.forEach((element) => {
          positif.push(element["positif_kumulatif"]);
        });

        const rawat = [];
        data.data.forEach((element) => {
          rawat.push(element["dirawat_kumulatif"]);
        });

        const sembuh = [];
        data.data.forEach((element) => {
          sembuh.push(element["sembuh_kumulatif"]);
        });

        const meninggal = [];
        data.data.forEach((element) => {
          meninggal.push(element["meninggal_kumulatif"]);
        });

        const myChart2 = echarts.init(document.getElementById("main2"));
        myChart2.setOption({
          title: {
            text: "Cases",
          },
          tooltip: {
            trigger: "axis",
          },
          legend: {
            data: ["Positif", "Rawat", "Sembuh", "Meninggal"],
          },
          grid: {
            left: "3%",
            right: "4%",
            bottom: "3%",
            containLabel: true,
          },
          xAxis: {
            type: "category",
            boundaryGap: false,
            data: newTanggal,
          },
          yAxis: {
            type: "value",
          },
          series: [
            {
              name: "Positif",
              type: "line",
              data: positif,
            },
            {
              name: "Rawat",
              type: "line",
              data: rawat,
            },
            {
              name: "Sembuh",
              type: "line",
              data: sembuh,
            },
            {
              name: "Meninggal",
              type: "line",
              data: meninggal,
            },
          ],
        });
      });

    // Chart3
    axios
      .get("https://apicovid19indonesia-v2.vercel.app/api/indonesia/harian")
      .then(function(data) {
        const tanggal = [];
        const newTanggal = [];
        data.data.forEach((element, index) => {
          tanggal.push(element["tanggal"]);
          let timestamp = new Date(tanggal[index]).getTime();
          let Day = new Date(timestamp).getDate();
          let Mounth = new Date(timestamp).getMonth() + 1;
          let Year = new Date(timestamp).getFullYear();
          let NewDateFormat = `${Day}/${Mounth}/${Year}`;
          newTanggal.push(NewDateFormat);
        });
        const sembuh = [];
        data.data.forEach((element) => {
          sembuh.push(element["sembuh"]);
        });
        const myChart3 = echarts.init(document.getElementById("main3"));
        myChart3.setOption({
          title: {
            text: "Recovery Rate",
          },
          xAxis: {
            type: "category",
            data: newTanggal,
          },
          yAxis: {
            type: "value",
          },
          series: [
            {
              data: sembuh,
              type: "line",
            },
          ],
        });
      });

    // Chart 4
    axios
      .get("https://apicovid19indonesia-v2.vercel.app/api/indonesia/harian")
      .then(function(data) {
        const tanggal = [];
        const newTanggal = [];
        data.data.forEach((element, index) => {
          tanggal.push(element["tanggal"]);
          let timestamp = new Date(tanggal[index]).getTime();
          let Day = new Date(timestamp).getDate();
          let Mounth = new Date(timestamp).getMonth() + 1;
          let Year = new Date(timestamp).getFullYear();
          let NewDateFormat = `${Day}/${Mounth}/${Year}`;
          newTanggal.push(NewDateFormat);
        });
        const meninggal = [];
        data.data.forEach((element) => {
          meninggal.push(element["meninggal"]);
        });
        const myChart4 = echarts.init(document.getElementById("main4"));
        myChart4.setOption({
          title: {
            text: "Death Rate",
          },
          xAxis: {
            type: "category",
            data: newTanggal,
          },
          yAxis: {
            type: "value",
          },
          series: [
            {
              data: meninggal,
              type: "line",
              lineStyle: {
                color: "red",
                type: "line",
              },
            },
          ],
        });
      });
  },
  methods: {
    getCountry() {
      fetch("https://covid-193.p.rapidapi.com/countries", {
        method: "GET",
        headers: {
          "x-rapidapi-host": "covid-193.p.rapidapi.com",
          "x-rapidapi-key":
            "f117361379msh12d7883431ee443p12c955jsn407e7c123c7a",
        },
      })
        .then((response) => response.json())
        .then((data) => (this.countryList = data.response));
    },
    getData() {
      fetch(
        "https://covid-193.p.rapidapi.com/statistics?country=" + this.country,
        {
          method: "GET",
          headers: {
            "x-rapidapi-host": "covid-193.p.rapidapi.com",
            "x-rapidapi-key":
              "f117361379msh12d7883431ee443p12c955jsn407e7c123c7a",
          },
        }
      )
        .then((response) => response.json())
        .then((data) => {
          data = data.response[0];
          this.cases = data.cases.total;
          this.recovered = data.cases.recovered;
          this.death = data.deaths.total;
        });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
select {
  margin-top: 20px;
  box-shadow: 2px 2px 2px rgba(255, 255, 255, 0.15);
  width: 60%;
  height: 40px;
  color: black;
  background-color: grey;
  border-radius: 10px;
}
.panel {
  margin-top: 20px;
  margin-bottom: 40px;
  text-align: left;
  width: 100%;
  display: flex;
  align-items: flex-start;
  justify-content: space-evenly;
}
.echart-wrap {
  display: inline-block;
  width: 680px;
  height: 500px;
  overflow: scroll;
  border: 1px solid black;
}
.echart-wrap::-webkit-scrollbar {
  width: 10px;
}

/* Track */
.echart-wrap::-webkit-scrollbar-track {
  background: #f1f1f1;
}

/* Handle */
.echart-wrap::-webkit-scrollbar-thumb {
  background: #888;
}

/* Handle on hover */
.echart-wrap::-webkit-scrollbar-thumb:hover {
  background: #555;
}
</style>

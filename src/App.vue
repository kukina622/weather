<template>
  <v-app>
    <v-main>
      <v-container>
        <v-card class="mx-auto" max-width="400">
          <v-card-title>
            <v-select
              :items="cities"
              label="選擇縣市"
              v-model="selectedCity"
              @change="changeCity"
            />
          </v-card-title>
        </v-card>
        <v-card class="mx-auto" max-width="400">
          <v-list-item two-line>
            <v-list-item-content>
              <v-list-item-title class="text-h5">
                {{ selectedCity }}
              </v-list-item-title>
              <v-list-item-subtitle
                >{{ fetchTime }}, {{ weatherData.Wx }}</v-list-item-subtitle
              >
            </v-list-item-content>
          </v-list-item>

          <v-card-text>
            <v-row align="center">
              <v-col class="text-h2" cols="7">
                {{
                  (parseInt(weatherData.MaxT) + parseInt(weatherData.MinT)) / 2
                }}&deg;C
              </v-col>
              <v-col cols="5">
                <!-- 自製的組件 -->
                <weatherIcon :weatherCode="weatherCode" />
              </v-col>
            </v-row>
          </v-card-text>

          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-weather-pouring</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle
              >{{ weatherData.PoP }} %
            </v-list-item-subtitle>
          </v-list-item>

          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-bed</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>
              {{ weatherData.CI }}
            </v-list-item-subtitle>
          </v-list-item>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import weatherIcon from "@/components/weatherIcon";

export default {
  name: "App",
  components: { weatherIcon },
  data() {
    return {
      cities: [
        "宜蘭縣",
        "花蓮縣",
        "臺東縣",
        "澎湖縣",
        "金門縣",
        "連江縣",
        "臺北市",
        "新北市",
        "桃園市",
        "臺中市",
        "臺南市",
        "高雄市",
        "基隆市",
        "新竹縣",
        "新竹市",
        "苗栗縣",
        "彰化縣",
        "南投縣",
        "雲林縣",
        "嘉義縣",
        "嘉義市",
        "屏東縣"
      ],
      Authorization: process.env.VUE_APP_AUTHORIZATION || "",
      selectedCity: "",
      fetchTime: "",
      weatherData: {
        MaxT: "", //最高溫
        MinT: "", //最低溫
        CI: "", //舒適度
        PoP: "", //降雨機率
        Wx: "" //天氣現象
      },
      weatherCode: 0
    };
  },
  methods: {
    async fetchWeatherData() {
      const today = new Date();
      const startTime = this.formatDatetime(today);
      const weatherUrl = `https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-C0032-001?Authorization=${this.Authorization}&locationName=${this.selectedCity}&timeFrom=${startTime}`;
      const { data } = await this.axios.get(weatherUrl);
      return data.records.location[0].weatherElement;
    },
    formatDatetime(datetime) {
      return new Date(datetime.toString().split("GMT")[0] + " UTC")
        .toISOString()
        .substr(0, 19);
    },
    changeTime() {
      const week = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
      const today = new Date();
      const hour =
        today.getHours() > 12 ? today.getHours() - 12 : today.getHours();
      const time = `${hour}:${today.getMinutes()} ${
        today.getHours() > 12 ? "PM" : "AM"
      }`;
      this.fetchTime = `${week[today.getDay()]}, ${time}`;
    },
    async changeCity() {
      this.changeTime();
      const weatherElement = await this.fetchWeatherData();
      for (let i = 0; i < weatherElement.length; i++) {
        const { elementName, time } = weatherElement[i];
        const { parameter } = time[0];
        this.weatherData[elementName] = parameter.parameterName;
        if (elementName === "Wx") {
          this.weatherCode = parseInt(parameter.parameterValue);
        }
      }
    }
  }
};
</script>

<style>
* {
  font-family: "Roboto", "Microsoft JhengHei";
}
</style>

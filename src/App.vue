<template>
  <v-app>
    <v-main>
      <v-container>
        <v-card class="mx-auto" max-width="400">
          <v-card-title>
            <v-select :items="cities" label="選擇縣市" />
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
                <img width="120" :src="require('@/assets/weather/1.svg')" />
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
export default {
  name: "App",
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
      Authorization: process.env.VUE_APP_AUTHORIZATION || "", //API KEY
      selectedCity: "", //被選擇的城市
      fetchTime: "", //取得資料的時間
      weatherData: {
        MaxT: "", //最高溫
        MinT: "", //最低溫
        CI: "", //舒適度
        PoP: "", //降雨機率
        Wx: "" //天氣現象
      },
      weatherCode: 0 //天氣描述代碼
    };
  },
  methods: {
    /**
     * 取得weather API資料
     */
    async fetchWeatherData() {
      // write your code here
    },
    /**
     * 傳入的是一個Date物件
     * 會將其轉會為yyyy-MM-ddThh:mm:ss格式的字串
     */
    formatDatetime(datetime) {
      return new Date(datetime.toString().split("GMT")[0] + " UTC")
        .toISOString()
        .substr(0, 19);
    },
    /**
     * 更新時間
     */
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
    /**
     * 當select改變被觸發
     * 會去取得weather API的資料
     * 並更新data裡的weatherData與 weatherCode
     */
    async changeCity() {
      this.changeTime();
      // write your code here
    }
  },
  created() {
    this.selectedCity = "雲林縣";
    this.changeCity();
  }
};
</script>

<style>
* {
  font-family: "Roboto", "Microsoft JhengHei";
}
</style>

<template>
  <div>
    <img src="~/assets/main.jpg" alt="" class="main-img" />
    <div class="container">
      <div class="title-container">
        <div class="title">
          Самолет энтузиастов
          <br />
          <small>сайт авиаперевозок</small>
        </div>
      </div>
      <div class="search-form">
        <el-input
          v-model="from"
          tabindex="1"
          placeholder="Откуда Вас забрать"
        />
        <el-input
          v-model="to"
          tabindex="2"
          placeholder="Куда Вы хотите прилететь"
        />
      </div>
      <div class="page">
        <div class="page-content">
          <div class="numbers">
            <div>
              <div class="num">{{ flights }}</div>
              рейсов
            </div>
            <div>
              <div class="num">{{ airports }}</div>
              аэропортов
            </div>
            <div>
              <div class="num">{{ aircrafts }}</div>
              самолетов
            </div>
            <div>
              <div class="num">{{ tickets }}</div>
              билетов
            </div>
          </div>
          <el-card v-for="item of list" :key="item.num" class="flight-card">
            <div class="flight">
              <div>
                <div class="text-primary">{{ item.departure }}</div>
                <small>Вылет</small>
              </div>
              <small class="text-center">
                <div>{{ item.aircraft }}</div>
                <small>Самолет</small>
              </small>
              <div class="text-right">
                <div class="text-primary">{{ item.arrival }}</div>
                <small>Пребытие</small>
              </div>
            </div>
          </el-card>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
/* eslint-disable camelcase */
import Vue from 'vue'

interface Numbers {
  flights: number
  airports: number
  tickets: number
  aircrafts: number
  flights_list: string[][]
}

interface ListItem {
  num: string
  aircraft: string
  arrival: string
  departure: string
}

export default Vue.extend({
  data() {
    return {
      from: '',
      to: '',
      flights: 0,
      airports: 0,
      tickets: 0,
      aircrafts: 0,
      list: [] as ListItem[],
    }
  },
  async fetch() {
    const {
      data: { tickets, flights, airports, aircrafts, flights_list },
    } = await this.$axios.get<Numbers>('/numbers')

    this.tickets = tickets
    this.flights = flights
    this.airports = airports
    this.aircrafts = aircrafts
    this.list = flights_list.map(([num, aircraft, arrival, departure]) => ({
      num,
      aircraft,
      arrival,
      departure,
    }))
  },
})
</script>

<style>
.text-primary {
  color: #409eff;
}
.text-center {
  text-align: center;
}
.text-right {
  text-align: right;
}

.flight-card {
  margin: 5px 0;
}
.flight {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 10px;
}
.numbers {
  display: flex;
  justify-content: space-between;
  flex-flow: row wrap;
  padding: 10px 0;
  gap: 15px;
}

.num {
  font-size: 1.5em;
}

.page {
  margin-top: 70px;
  width: 100%;
  background: white;
}

.page-content {
  margin: 0 auto;
  width: 95%;
  max-width: 1000px;
  height: 100%;
  padding: 15px;
}

.title-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  height: 180px;
}

.title {
  font-size: 26px;
  padding: 15px 25px;
  border-radius: 5px;
  text-align: center;
  margin: 0 10px;
  color: blue;
  font-weight: 600;
  background: white;
}

.search-form {
  display: flex;
  width: 95%;
  margin: 0 auto;
  max-width: 700px;
  gap: 15px;
}

@media screen and (max-width: 600px) {
  .page {
    margin: 0;
  }
  .search-form {
    height: 110px;
    flex-flow: column;
  }

  .flight {
    grid-template-columns: 1fr;
  }
}

.container {
  margin: 0 auto;
  min-height: max-content;
  width: 100%;
  padding-top: 150px;
}

.main-img {
  z-index: -1;
  position: fixed;
  width: 100vw;
  height: 450px;
  object-fit: cover;
  object-position: center;
  filter: blur(5px);
}
</style>

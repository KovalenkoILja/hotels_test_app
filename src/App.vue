<template>
  <div id="show-hotels">
    <fieldset>
      <legend>Страна</legend>
      <select v-model="filter.country" id="country" class="form-control">
        <option value="">Выберите страну</option>
        <option v-for="item in countryList" :key="item.country" :value="item.country">{{ item.country }}</option>
      </select>
    </fieldset>

    <fieldset>
      <legend>Тип</legend>
      <select v-model="filter.type" id="type" class="form-control">
        <option value="">Выберите тип</option>
        <option v-for="item in typesList" :key="item.type" :value="item.type">{{ item.type }}</option>
      </select>
    </fieldset>

    <fieldset>
      <legend>Звезды</legend>
      <div v-for="star in stars" :key="star.id">
        <input type="checkbox" v-model="filter.stars" :value="star"/>
        <label>{{ star.name }}</label>
      </div>
    </fieldset>

    <fieldset>
      <legend>Количество отзывов от</legend>
      <input type="number" v-model.number="filter.reviews_amount"/>
    </fieldset>

    <fieldset>
      <legend>Цена до</legend>
      {{ filter.price }}
      <input type="range" v-model="filter.price" :min="0" :max="10000">
    </fieldset>

    <div class="form-group">
      <button type="button" class="btn btn-primary" @click="applyBtnClick">Применить</button>
      <button type="button" class="btn btn-primary" @click="resetBtnClick">Сбросить</button>
    </div>
    <h5>Hotels</h5>
    <table class="table table-striped table-bordered" v-if="this.currentHotelList.length > 0">
      <thead>
      <tr>
        <td>Название</td>
        <td>Описание</td>
      </tr>
      </thead>
      <tbody>
      <tr v-for="hotel in displayedHotels" :key="hotel.name">
        <td>{{ hotel.name }}</td>
        <td>{{ hotel.description }}</td>
      </tr>
      </tbody>
    </table>
    <h1 v-else>Записей не найдено</h1>
    <div class="form-group">
      <button type="button" v-if="page !== 1" @click="page--">Предыдущая</button>
      Страница {{ page }} из {{ pages.length }}
      <button type="button" v-if="page < pages.length" @click="page++">Следующая</button>
    </div>
  </div>
</template>

<script>
import hotelsData from "./hotels.json";

export default {
  data() {
    return {
      jsonList: hotelsData,
      currentHotelList: [],
      countryList: [],
      typesList: [],
      page: 1,
      perPage: 3,
      pages: [],
      stars: [
        {
          id: 1,
          name: '1 Звезда',
        },
        {
          id: 2,
          name: '2 Звезды',
        },
        {
          id: 3,
          name: '3 Звезды',
        },
        {
          id: 4,
          name: '4 Звезды',
        },
        {
          id: 5,
          name: '5 Звезд',
        }
      ],
      filter: {
        country: '',
        type: '',
        stars: [],
        price: 0,
        reviews_amount: 0,
        status: false,
      },
    };
  },
  mounted() {
    this.getListOfCountries()
    this.getListOfTypes()
    this.currentHotelList = this.jsonList.hotels
    this.setPages(this.currentHotelList)
  },
  methods: {
    getListOfCountries() {
      this.countryList = this.jsonList.hotels.reduce((seed, current) => {
        return Object.assign(seed, {
          [current.country]: current
        });
      }, {});
    },
    getListOfTypes() {
      this.typesList = this.jsonList.hotels.reduce((seed, current) => {
        return Object.assign(seed, {
          [current.type]: current
        });
      }, {});
    },
    applyBtnClick: function () {
      let query = []

      if (this.filter.country !== '') {
        this.jsonList.hotels.forEach((h) => {
          if (h.country === this.filter.country) {
            query.push(h)
          }
        })
      }

      if (this.filter.type !== '') {
        this.jsonList.hotels.forEach((h) => {
          if (h.type === this.filter.type) {
            query.push(h)
          }
        })
      }

      if (this.filter.price !== 0) {
        this.jsonList.hotels.forEach((h) => {
          if (h.min_price <= this.filter.price) {
            query.push(h)
          }
        })
      }

      if (this.filter.stars.length > 0) {
        this.filter.stars.forEach((star) => {
          this.jsonList.hotels.forEach((h) => {
            if (h.stars === star.id) {
              query.push(h)
            }
          })
        })
      }

      if (this.filter.reviews_amount > 0) {
        this.jsonList.hotels.forEach((h) => {
          if (h.reviews_amount >= this.filter.reviews_amount) {
            query.push(h)
          }
        })
      }

      //only unique
      query = [...new Set(query)]
      this.currentHotelList = query
      this.resetPages()
    },
    resetBtnClick: function () {
      this.filter = {
        country: '',
        type: '',
        stars: [],
        price: 0,
        reviews_amount: 0,
        status: false,
      }
      this.currentHotelList = hotelsData.hotels
      this.resetPages()
    },
    resetPages(){
      this.page = 1
      this.pages = []
      this.setPages(this.currentHotelList)
    },
    setPages(hotels) {
      let numberOfPages = Math.ceil(hotels.length / this.perPage);
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index);
      }
      console.log("setPages: " + this.pages)
    },
    paginate(hotels) {
      let page = this.page;
      let perPage = this.perPage;
      let from = (page * perPage) - perPage;
      let to = (page * perPage);
      return hotels.slice(from, to);
    }
  },
  computed: {
    displayedHotels() {
      return this.paginate(this.currentHotelList);
    }
  },
};
</script>

<style>
#show-hotels {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 10px;
}

label {
  font-weight: bold;
  color: #337ab7;
  margin-right: 20px;
}
button{
  color: #337ab7;
  font-weight: bold;
  margin: 10px;
}
</style>

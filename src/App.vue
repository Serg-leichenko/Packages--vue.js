<template>
  <div id="app">
    <div class="wrap">
      <div class="title">
        <h1>List Packages</h1>
      </div>
      <div class="search-top">
        <b-navbar toggleable="lg" type="dark" variant="info">
        <b-navbar-nav class="m-auto">
          <b-nav-form>
            <b-form-input size="sm" class="mr-sm-2" type="text" v-model="searchValue" placeholder="Search" autocomplete="on"></b-form-input>
          </b-nav-form>           
        </b-navbar-nav>
      </b-navbar>
      </div>
      <div class="list">
        <div class="list__package type">
          <ul>
            <li v-for="(info, index) of filteredInform.slice((page - 1) * perPage, page * perPage)" :key="index">
              <a href="#">{{ info.name }}</a>
              <b-button variant="outline-primary btn" v-b-modal.my-modal>Detail</b-button>
            </li>
          </ul>          
        </div>
      </div>
      <b-modal id="my-modal">
        <div class="package-modal">
          <div class="package-name">
          <ul>
            <li v-for="(info, index) of filteredInform" :key="index">
              <p>Type: {{ info.type }}</p>
            </li>
          </ul>          
        </div>
        <div class="package-hits">
          <ul>
            <li v-for="(info, index) of filteredInform" :key="index">
              <p>Hits: {{ info.hits }}</p>
            </li>
          </ul>          
        </div>  
        </div>      
      </b-modal>
      <div class="btn-next-prev">
        <b-button @click="next(-1)" :disabled="page <= 1">Prev</b-button>
        <b-button @click="next(+1)" :disabled="page >= numPages">Next</b-button>
      </div>
      <Footer />     
    </div>  
  </div>
</template>

<script>
import Footer from '@/components/Footer'
import axios from 'axios'
export default {
  components: {
    Footer
  },
  data() {
    return {
      inform: [].map((n, i) => i + 1),
      searchValue: '',
      page: 1,
      perPage: 10,
      list: 1
    }
  },
  computed: {
    filteredInform: function(){
        return this.inform.filter((info) => {
            return info.name.toLowerCase().match(this.searchValue.toLowerCase());
        });
    },

    numPages() {
      return Math.ceil(this.inform.length / this.perPage);
    },
  },
  mounted() {
    axios
      .get('https://data.jsdelivr.com/v1/stats/packages')
      .then(response => this.inform = response.data);
  },
  methods: {
    next(change) {
      this.page = Math.max(1, Math.min(this.numPages, this.page + change));
    },
  }
}
</script>

<style>
.wrap {
  padding-top: 20px;
}
.title {
text-align: center;
}
ul, li {
  display: block;
  margin: 0;
  padding: 0;
}
.list {
  background-color: #cccccc;
  padding: 10px 0;
}
.list__package {  
  width: 50%;  
  margin: auto;
}
.list__package ul li {
  display: flex;
  justify-content: space-between;
  border: 1px solid #000;
  margin-bottom: 10px;
  padding: 5px 10px;
  border-radius: 5px;
}
.list__package ul li a {
  display: flex;
  align-items: center;
  color: #000;
}
.btn {
  width: 150px;
}
.package-modal {
  display: flex;
  justify-content: space-between;
}
.btn-next-prev {
  width: 30%;
  display: flex;
  justify-content: space-between;
  margin: auto;
  padding: 10px 0;
}
</style>
<template>
  <div @priceVal="pricefilterArray">
    <Breadcrumbs :main="mainCategory" :title="subCategory" />
    <!-- Container-fluid starts-->
    <div
      :class="filtered ? 'container-fluid product-wrapper sidebaron' : 'container-fluid product-wrapper'" 
    >
      <div class="product-grid">
        <div class="feature-products">
          <div class="row">
            <div class="col-xl-12 col-md-12">
              <form>
                <div class="form-group m-0">
                  <input
                    class="form-control"
                    type="text"
                    placeholder="Search.."
                    v-model="searchKey"
                    v-on:keyup="searchProducts"
                  />
                  <i class="fa fa-search"></i>
                </div>
              </form>
            </div>
          </div>
        </div>
        <div class="col-sm-12">
          <div v-if="searchResults.length == 0">
            <div class="search-not-found text-center m-10">
              <p>Sorry, We didn't find any results matching this search</p>
            </div>
          </div>
        </div>
        <div class="product-wrapper-grid" :class="listViewEnable?'list-view':''">
          <div class="row">
            <div
              :class="[col2 ? 'col-md-6': col3 ? 'col-lg-4 col-md-6' : col4 ? 'col-xl-3 col-md-6' : 
                          col6 ? 'col-xl-2 col-lg-4 col-md-6' : list ? 'col-xl-12' : 'col-xl-3 col-md-6']"
              v-for="(product,index) in this.searchResults"
              :key="index"
            >
              <div class="card">
                <div class="product-box">
                  <div class="product-img">
                    <img class="img-fluid" :src="getImgUrl(product.images[0])" alt />
                    <div class="product-hover">
                      <ul>
                        <router-link :to="'/ecommerce/cart'">
                          <li @click="addToCart(product)">
                            <button class="btn" type="button">
                              <i class="icon-shopping-cart"></i>
                            </button>
                          </li>
                        </router-link>
                        <li>
                          <button
                            class="btn"
                            @click="quickView(product)"
                            type="button"
                            data-toggle="modal"
                            data-target="#exampleModalCenter"
                          >
                            <i class="icon-eye"></i>
                          </button>
                        </li>
                      </ul>
                    </div>
                  </div>

                  <div class="product-details">
                    <router-link :to="'/ecommerce/details/'+product.sku">
                      <h4>{{product.name}}</h4>
                    </router-link>
                    <p>New York, United State</p>
                    <!-- <div class="product-price">
                      {{product.price | currency}}
                      <del>{{product.salePrice | currency}}</del>
                    </div> -->
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- QuickView Modal -->
    <b-modal size="lg" centered v-model="modalShow" hide-footer hide-header>
      <button class="close" type="button" v-on:click="quickViewClose(modalShow)">
        <span>×</span>
      </button>
      <div class="product-box quickview row">
        <div class="product-img col-lg-6">
           <img class="img-fluid" :src="getImgUrl(modalShow?quickViewProduct.images[0]:'ecommerce/11.jpg')" alt="">
        </div>
        <div class="product-details col-lg-6 text-left">
          <h4>{{quickViewProduct.name}}</h4>
          <div class="product-price">
            {{quickViewProduct.price | currency }}
            <del>{{quickViewProduct.salePrice | currency }}</del>
          </div>
          <div class="product-view">
            <h6 class="f-w-600">Product Details</h6>
            <p class="mb-0">
              Sed ut perspiciatis, unde omnis iste natus error sit voluptatem accusantium doloremque
              laudantium, totam rem aperiam eaque ipsa, quae ab illo.
            </p>
          </div>
          <div class="product-size">
            <ul>
              <li>
                <button class="btn btn-outline-light" type="button">M</button>
              </li>
              <li>
                <button class="btn btn-outline-light" type="button">L</button>
              </li>
              <li>
                <button class="btn btn-outline-light" type="button">Xl</button>
              </li>
            </ul>
          </div>
          <div class="product-qnty">
            <h6 class="f-w-600">Quantity</h6>
            <div class="qty-box1">
              <div class="input-group">
                <i class="fa fa-minus btnGtr1" @click="decrement()"></i>
                <input class="touchspin1 text-center" v-model="counter" name="quantity" type="text" />
                <i class="fa fa-plus btnLess1" @click="increment()"></i>
              </div>
            </div>
            <div class="addcart-btn mt-3">
              <router-link :to="'/ecommerce/cart'">
                <button
                  class="btn btn-primary m-r-10"
                  type="button"
                  data-original-title="btn btn-info-gradien"
                  title
                  @click="addToCart(quickViewProduct,counter)"
                >Add To Cart</button>
              </router-link>
              <router-link
                :to="'/ecommerce/checkout'"
                class="btn btn-primary cart-btn-transform"
              >Buy Now</router-link>
            </div>
          </div>
        </div>
      </div>
    </b-modal>

  </div>
</template>
  <script>
  import { mapGetters } from 'vuex';
  import Slider from '../ecommerce/filterbar';
  import { Carousel, Slide } from 'vue-carousel';
  
  export default {
    name: 'category',
    components: {
      Slider,
      Carousel,
      Slide
    },
    data() {
      return {
        modalShow: false,
        quickViewProduct: [],
        counter: 1,
        priceArray: [],
        allfilters: [],
        items: [],
        filtered: false,
        col2: false,
        col3: false,
        col4: false,
        col6: true,
        listViewEnable: false,
        list: false,
        value: [0, 450], // filterbar variable
        searchKey: '',
        searchResults: [],
      };
    },
    mounted() {
      this.pricefilterArray(this.value);

      // when page is loaded 
      if(this.searchResults.length == 0) {
        this.searchResults = this.filterProduct;
      }
    },
    props: ['category', 'subcategory'],
    computed: {
      ...mapGetters({
        filterProduct: 'products/filterProducts',
        tags: 'products/setTags'
      }),

      mainCategory: function() {
        return this.category;
      },

      subCategory: function() {
        return this.subcategory;
      },

      categoryProducts: function () {
        // filter products by main and sub category
        return [];
      }
    },
    methods: {
      //search products
      searchProducts: function()
      {
        if(this.searchKey == '')
        {
          this.searchResults = this.filterProduct;
          this.listViewEnable = false;
          return;
        }

        var searchedProducts = [];
        
        for(var i = 0; i < this.filterProduct.length; i++)
        {
          var productName = this.filterProduct[i].name.toLowerCase();
          if(productName.indexOf(this.searchKey.toLowerCase()) >= 0)
          {
            searchedProducts.push(this.filterProduct[i]);
          }
        }

        this.searchResults = searchedProducts;
        this.listViewEnable = true;
      },

      //For getting image path
      getImgUrl(path) {
        return require('../../assets/images/' + path);
      },

      // For Order By  
      onChangeSort(event) {
        this.$store.dispatch('products/sortProducts', event.target.value);
      },

      //Filter by Category, Brand, Color
      allfilter(selectedVal) {
        this.allfilters = selectedVal;
        this.$store.dispatch('products/setTags', selectedVal);
      },

      collapseFilter() {
        this.filtered = !this.filtered;
      },

      //Price Filter
      pricefilterArray(item) {
        this.$store.dispatch('products/priceFilter', item);
      },
    
      //Add to cart
      addToCart: function(product, qty) {
        product.quantity = qty ? qty : 1;
        this.$store.dispatch('products/addToCart', product);
      },

      //Quick View
      quickView: function(product) {
        this.modalShow = true;
        return (this.quickViewProduct = product);
      },
      
      quickViewClose: function() {
        this.modalShow = false;
      },

      //Quantity increment Decrement
      increment() {
        if (this.counter < this.quickViewProduct.stock) this.counter++;
      },

      decrement() {
        if (this.counter > 1) this.counter--;
      },

      //Grid changes
      grid2() {
        this.col2 = true;
        this.col3 = false;
        this.col4 = false;
        this.col6 = false;
        this.listViewEnable = false;
      },
      grid3() {
        this.col2 = false;
        this.col3 = true;
        this.col4 = false;
        this.col6 = false;
        this.listViewEnable = false;
      },
      grid4() {
        this.col2 = false;
        this.col3 = false;
        this.col4 = true;
        this.col6 = false;
        this.listViewEnable = false;
      },
      grid6() {
        this.col2 = false;
        this.col3 = false;
        this.col4 = false;
        this.col6 = true;
        this.listViewEnable = false;
      },
      listView() {
        this.listViewEnable = true;
        this.list = true;
        this.col2 = false;
        this.col3 = false;
        this.col4 = false;
        this.col6 = false;
      },
      gridView() {
        this.listViewEnable = false;
        this.col4 = true;
      }
    }
  };
</script>

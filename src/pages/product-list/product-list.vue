<template>
  <div @priceVal="pricefilterArray">
    <div
      :class="filtered ? 'container-fluid product-wrapper sidebaron' : 'container-fluid product-wrapper'" 
    >
      <div class="product-grid">
        <div class="row">
          <div class="col-6">
            <div class="mb-2">
              <div class="col-form-label">Category</div>
                <multiselect  
                  v-model="categoryDirection" 
                  :allow-empty="false" 
                  openDirection="down" 
                  tag-placeholder="Add this as new tag" 
                  placeholder="Search or add a tag" 
                  label="name" 
                  track-by="code" 
                  :options="categories" :multiple="true" :taggable="true" @tag="addCategory">
                </multiselect>
            </div>
          </div>
          <div class="col-6">
            <div class="mb-2">
              <div class="col-form-label">Type</div>
                <multiselect  
                  v-model="typeDirection" 
                  :allow-empty="false" 
                  openDirection="down" 
                  tag-placeholder="Add this as new tag" 
                  placeholder="Search or add a tag" 
                  label="name" 
                  track-by="code" 
                  :options="types" :multiple="true" :taggable="true" @tag="addType">
                </multiselect>
            </div>
          </div>
        </div>

        <div v-if="filteredProducts.length == 0" class="slider_views">
          <div class="shop_slider_view">
            <div class="silder_title">
            </div>
            <Carousel 
              :autoplay="true" 
              :per-page="5" 
              :loop="true" 
              paginationPosition="bottom-overlay"
              :perPageCustom="[[280, 1], [360, 2], [992, 3],]"
            >
              <Slide
                v-for="(product,index) in filterProduct"
                :key="index"
              >
                <div class="card">
                  <div class="product-box">
                    <div class="product-img">
                      <!-- <div class="ribbon ribbon-danger" v-if="product.sale">Sale</div> -->
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
                      <!-- <div class="rating"><i class="fa fa-star"></i><i class="fa fa-star"></i><i class="fa fa-star"></i><i class="fa fa-star"></i><i class="fa fa-star"></i></div> -->
                      <router-link :to="'/ecommerce/details/'+product.sku">
                        <h5>{{product.name}}</h5>
                      </router-link>
                      <!-- <p>{{product.shortDescription}}</p>
                      <div class="product-price">
                        {{product.price | currency}}
                        <del>{{product.salePrice | currency}}</del>
                      </div> -->
                    </div>
                  </div>
                </div>
              </Slide>
            </Carousel>
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
  import Multiselect from 'vue-multiselect';

  export default {
    name: 'landing',
    components: {
      Slider,
      Carousel,
      Slide,
      Multiselect
    },
    data() {
      return {
        categoryDirection:[
          { name: 'Woman', code: '1' }
        ],
        typeDirection:[
          { name: 'Shirt', code: '1' }
        ],
        categories: [
          { code: 1, name: 'Woman' },
          { code: 2, name: 'Man' },
          { code: 3, name: 'Adult' },
          { code: 4, name: 'Child' },
          { code: 5, name: 'Youth' }
        ],
        types: [
          { code: 1, name: 'Shirt' },
          { code: 2, name: 'Cat' },
          { code: 3, name: 'Groove' },
          { code: 4, name: 'Shoe' },
          { code: 5, name: 'Bag' }
        ],
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
        filteredProducts: []
      };
    },
    mounted() {
      this.pricefilterArray(this.value);
    },
    computed: {
      ...mapGetters({
        filterProduct: 'products/filterProducts',
        tags: 'products/setTags'
      }),
    },
    methods: {
      addCategory (newTag) {
        const tag = {
          name: newTag,
          code: newTag.substring(0, 2) + Math.floor((Math.random() * 10000000))
        };
        this.options.push(tag);
        this.value.push(tag);
      },

      addType (newTag) {
        const tag = {
          name: newTag,
          code: newTag.substring(0, 2) + Math.floor((Math.random() * 10000000))
        };
        this.options.push(tag);
        this.value.push(tag);
      },
      //search products
      searchProducts: function()
      {
        if(this.searchKey == '')
        {
          this.filteredProducts = [];
          this.listView();
          this.list = true;
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

        this.listView();
        this.filteredProducts = searchedProducts;
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

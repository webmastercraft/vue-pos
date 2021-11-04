<template>
  <div>
    <Breadcrumbs main="" title="Cashier"/>
    <!-- Container-fluid starts-->
    <div class="container-fluid">
      <div class="row">
        <div class="col-md-6">
          <button class="btn btn-outline-primary m-10">+ New Order</button>
          <div class="row">
            <div class="col-sm-12">
              <div class="card">
                <div class="card-body">
                  <div class="row">
                    <div class="col-sm-12 empty-cart-cls text-center"  v-if="!cart.length">
                      <img :src='getImgUrl("ecommerce/icon-empty-cart.png")' class="img-fluid mb-4">
                      <h3><strong>Your Cart is Empty</strong></h3>
                      <h4>Add something to make me happy :)</h4>
                      <router-link :to="'/ecommerce/product'" class="btn btn-primary cart-btn-transform m-t-15">continue shopping</router-link>
                    </div>
                    <div class="order-history table-responsive wishlist" v-if="cart.length">
                      <table class="table table-bordered">
                        <thead>
                          <tr>
                            <!-- <th>Prdouct</th> -->
                            <th>Prdouct Name</th>
                            <th>Price</th>
                            <th>Quantity</th>
                            <th>Action</th>
                            <!-- <th>Total</th> -->
                          </tr>
                        </thead>
                        <tbody>
                          <tr v-for="(item,index) in cart" :key="index">
                            <!-- <td><img class="img-fluid img-40" :src='getImgUrl(item.images[0])' alt="#"></td> -->
                            <td>
                              <div class="product-name"><router-link :to="'/ecommerce/details/'+item.sku">{{item.name}}</router-link></div>
                            </td>
                            <td>{{item.price | currency}}</td>
                            <td>
                              <fieldset class="qty-box m-auto p-0">
                                <div class="input-group addToCart">
                                  <i class="fa fa-minus btnGtr1"  v-on:click="decrement(item)"></i>
                                  <input class="touchspin text-center" v-model="item.quantity" name="item.quantity" type="text" >
                                  <i class="fa fa-plus btnLess1"  v-on:click="increment(item)"></i>
                                </div>
                              </fieldset>
                            </td>
                            <td class="card-body btn-group-wrapper">
                              <b-button-group>
                                <b-dropdown left text="More" variant="light">
                                  <b-dropdown-item variant="primary">Discount</b-dropdown-item>
                                  <b-dropdown-item variant="secondary">Edit</b-dropdown-item>
                                  <b-dropdown-item variant="danger"> Remove</b-dropdown-item>
                                </b-dropdown>
                              </b-button-group>
                            </td>
                            <!-- <td>{{item.price * item.quantity | currency}}</td> -->
                          </tr>
                          <tr>
                            <!-- <td colspan="4">                                           
                              <div class="input-group">
                                <input class="form-control mr-2" type="text" placeholder="Enter coupan code"><a class="btn btn-primary" href="#">Apply</a>
                              </div>
                            </td> -->
                            <td class="total-amount">
                              <h6 class="m-0 text-right"><span class="f-w-600">Total Price :</span></h6>
                            </td>
                            <td><span>{{getAmount | currency}}  </span></td>
                          </tr>
                          <!-- <tr>
                            <td class="text-right" colspan="5">
                              <router-link :to="'/ecommerce/product'" class="btn btn-secondary cart-btn-transform">continue shopping</router-link>
                            </td>
                            <td>
                              <router-link :to="'/ecommerce/checkout'" class="btn btn-primary cart-btn-transform"> check out</router-link>
                            </td>
                          </tr> -->
                        </tbody>
                      </table>
                    </div>
                  </div>
                </div>
                <!-- Container-fluid Ends-->
              </div>
            </div>
          </div>
          <div role="group" class="btn-group-square btn-group cashier-btn-group">
            <button type="button" class="btn btn-outline-secondary m-10">Cancel</button>
            <button type="button" @click="quickView(0)" class="btn btn-outline-info m-10">Note</button>
            <button type="button" @click="quickPayView(0)" class="btn btn-outline-success m-10">Pay</button>
          </div>
        </div>
        <div class="col-md-6">
          <ProductList />
        </div>
      </div>
    </div>
    <!-- Container-fluid Ends-->
    <!-- QuickView Modal -->
    <b-modal size="lg" centered v-model="modalShow" hide-footer hide-header>
      <button class="close" type="button" v-on:click="quickViewClose(modalShow)">
        <span>×</span>
      </button>
      <div class="form-group mb-0">
        <label class="text-muted">Message</label>
        <ckeditor :editor="editor"  v-model="editorData" :config="editorConfig"></ckeditor>
      </div>
    </b-modal>
    <b-modal size="lg" centered v-model="payModalShow" hide-footer hide-header>
      <div class="col-sm-12 col-xl-6 xl-100 box-col-12 payment_tab">
          <b-card header-tag="div" no-body>
              <b-card-body>
                  <b-tabs pills slot="header" class="tabbed-card">
                      <b-tab title="Card" active>
                          <div class="card height-equal">
                            <div class="card-body">
                              <div class="row">
                                <div class="col-md-4 text-center"><img class="img-fluid" src="../../assets/images/ecommerce/card.png" alt=""></div>
                                <div class="col-md-8">
                                  <form class="theme-form mega-form">
                                    <div class="form-group">
                                      <input class="form-control" type="text" placeholder="Card number">
                                    </div>
                                    <div class="form-group">
                                      <input class="form-control" type="text" placeholder="Transaction ID">
                                    </div>
                                    <div class="col-12">
                                      <button class="btn btn-primary-gradien btn-block" type="button" data-original-title="btn btn-primary-gradien" title="">Submit</button>
                                    </div>
                                  </form>
                                </div>
                              </div>
                            </div>
                          </div>
                      </b-tab>
                      <b-tab title="Cash">
                          <div class="card">
                            <div class="card-body">
                              <form class="theme-form row">
                                <div class="form-group col-6">
                                  <input class="form-control" type="number" placeholder="Total Price ($1000)">
                                </div>
                                <div class="form-group col-6">
                                  <input class="form-control" type="number" placeholder="Given Price ($700)">
                                </div>
                                <div class="form-group col-6">
                                  <input class="form-control" type="number" placeholder="Remain Price ($300)">
                                </div>
                                <div class="col-12">
                                  <button class="btn btn-primary-gradien btn-block" type="button" data-original-title="btn btn-primary-gradien" title="">Submit</button>
                                </div>
                              </form>
                            </div>
                          </div>
                      </b-tab>
                      <b-tab title="Installment">
                        <div class="card height-equal">
                          <div class="card-body">
                            <form class="theme-form e-commerce-form row">
                              <div class="form-group col-6 p-r-0">
                                <input class="form-control" type="text" placeholder="Total Price ($500)">
                              </div>
                              <div class="form-group col-6">
                                <input class="form-control" type="text" placeholder="Given Price ($500)">
                              </div>
                              <div class="form-group col-6 p-r-0">
                                <input class="form-control" type="text" placeholder="Remain Price ($500)">
                              </div>
                              <div class="form-group col-6">
                                <input class="form-control" type="text" placeholder="Dividing 2">
                              </div>
                              <div class="col-12">
                                <label class="col-form-label p-t-0">Next Payment Date and Amount</label>
                              </div>
                              <div class="form-group col-6">
                                  <b-form-input class="form-control" type="datetime-local" value="2020-01-19T18:45:00"></b-form-input>
                              </div>
                              <div class="form-group col-6 p-r-0">
                                <input class="form-control" type="text" placeholder="Amount ($500)">
                              </div>
                              <div class="col-12">
                                <label class="col-form-label p-t-0">Next Payment Date and Amount</label>
                              </div>
                              <div class="form-group col-6">
                                  <b-form-input type="datetime-local" value="2021-01-19T18:45:00"></b-form-input>
                              </div>
                              <div class="form-group col-6 p-r-0">
                                <input class="form-control" type="text" placeholder="Amount ($500)">
                              </div>
                              <div class="col-12">
                                <button class="btn btn-primary-gradien btn-block" type="button" data-original-title="btn btn-primary-gradien" title="">Submit</button>
                              </div>
                            </form>
                          </div>
                        </div>
                      </b-tab>
                  </b-tabs>
              </b-card-body>
          </b-card>
      </div>
    </b-modal>
  </div>
</template>
<script>
  import { mapState, mapGetters } from 'vuex';
  import ProductList from '../product-list/product-list'
  import CKEditor from '@ckeditor/ckeditor5-vue';
  import ClassicEditor from '@ckeditor/ckeditor5-build-classic';
  
  export default {
    name:'Product',
    components: {
      ProductList,
      ckeditor: CKEditor.component,
    },
    data(){
      return{
        counter: 1,
        modalShow: false,
        payModalShow: false,
        editor: ClassicEditor,
        editorData: '<p>Content of the editor.</p>',
        editorConfig: {
          config:{ height: '30px' }
        },
      };
    },
    computed: {
      ...mapState({
        cart: state => state.products.cart,
        getAmount() {
          return this.totalAmount = this.$store.getters['products/getTotalAmount'];
        }
      })
    },
    methods: {
      getImgUrl(path) {
        return require('../../assets/images/'+path);
      },
      removeProduct: function(product) {
        this.$store.dispatch('products/removeProduct', product);
      },
      increment(product,qty=1) {      
        this.$store.dispatch('products/updateCartQuantity', { 'product':product,'qty':qty });
      },
      decrement(product,qty = -1) {
        this.$store.dispatch('products/updateCartQuantity', { 'product':product,'qty':qty });
      },
      setactiveemails(id) {
        this.editorShow = !this.editorShow;
        this.$store.dispatch('email/setactiveemails',id);
      },
      //Quick View
      quickView: function() {
        this.modalShow = true;
      },
      quickViewClose: function() {
        this.modalShow = false;
      },

      //Quick View
      quickPayView: function() {
        this.payModalShow = true;
      },
      quickPayViewClose: function() {
        this.payModalShow = false;
      },
    }
  };
</script>
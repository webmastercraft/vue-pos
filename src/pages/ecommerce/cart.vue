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
                <px-card :actions="false">
                  <div slot="with-padding">
                    <div class="form-row">
                        <div class="col-md-4 mb-3">
                            <label for="c_form_first_name">User Name:</label>
                            <b-form-input type="text" id="c_form_first_name" v-model="c_form.first_name" :state="c_form_result.first_name" placeholder="Jone Doe"></b-form-input>
                            <b-form-valid-feedback :state="c_form_result.first_name">Lock Good</b-form-valid-feedback>
                        </div>
                        <div class="col-md-3 mb-3">
                            <label for="c_form_zipcode">Barcode:</label>
                            <b-form-input type="text" id="c_form_zipcode" v-model="c_form.zip" :state="c_form_result.zip" placeholder="barcode"></b-form-input>
                            <b-form-invalid-feedback :state="c_form_result.zip">Please provide a valid zip.</b-form-invalid-feedback>
                        </div>
                    </div>
                    <b-button type="submit" variant="primary" @click="addCustomerModalShow">Add Customer</b-button>
                  </div>
                </px-card>
            </div>
          </div>
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
                    <div class="table-responsive vue-smart">
                        <v-table
                            :data="cart" class="table"
                            :currentPage.sync="filter.currentPage"
                            :pageSize="10"
                            @totalPagesChanged="filter.totalPages = $event"
                            :filters="filters"
                        >
                            <thead slot="head">
                                <!-- <v-th sortKey="position">ID</v-th> -->
                                <v-th sortKey="name">Name</v-th>
                                <v-th sortKey="price">Price</v-th>
                                <v-th sortKey="qty">Quantity</v-th>
                                <v-th sortKey="action">Action</v-th>
                            </thead>
                            <tbody slot="body" slot-scope="{displayData}">
                                <tr v-for="row in displayData" :key="row.id">
                                    <td>{{ row.name }}</td>
                                    <td>{{ row.price }}</td>
                                    <td>{{ row.quantity }}</td>
                                    <td>
                                        <b-button-group>
                                            <b-dropdown left text="Operations" variant="light">
                                                <b-dropdown-item variant="primary">Discount</b-dropdown-item>
                                                <b-dropdown-item variant="secondary">Edit</b-dropdown-item>
                                                <b-dropdown-item variant="danger"> Remove</b-dropdown-item>
                                            </b-dropdown>
                                        </b-button-group>
                                    </td>
                                </tr>
                                <tr>
                                  <td class="total-amount">
                                    <h6 class="m-0 text-right"><span class="f-w-600">Total Price :</span></h6>
                                  </td>
                                  <td><span>{{getAmount | currency}}</span></td>
                                  <td></td>
                                  <td></td>
                                </tr>
                            </tbody>
                        </v-table>
                    </div>
                    <smart-pagination
                      :currentPage.sync="filter.currentPage"
                      :totalPages="filter.totalPages"
                    />
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
        <div class="col-md-6 custom_hold_order">
          <px-card title="Hold Orders" :actions="false" class="hold_cards">
            <div slot="with-padding">
              <div class="row">
                <div class="col-xl-3 col-md-6 box-col-6">
                  <div class="prooduct-details-box">                                 
                    <div class="media">
                      <div class="media-body ml-3">
                        <div class="product-name">
                          <h6>Hold 1</h6>
                        </div>
                        <feather class="close" type="x"></feather>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="col-xl-3 col-md-6 box-col-6">
                  <div class="prooduct-details-box">                                 
                    <div class="media">
                      <div class="media-body ml-3">
                        <div class="product-name">
                          <h6>Hold 2</h6>
                        </div>
                        <feather class="close"  type="x"></feather>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </px-card>
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
    <b-modal size="lg" centered v-model="customerModalShow" hide-footer hide-header>
      <button class="close" type="button" v-on:click="addCustomerModalClose(customerModalShow)">
        <span>×</span>
      </button>
      <!-- <b-form class="needs-validation" @submit="onCustomStyleSubmit"> -->
      <b-form class="needs-validation">
        <div class="form-row">
            <div class="col-md-4 mb-3">
                <label for="c_form_first_name">Name</label>
                <b-form-input type="text" id="c_form_name" v-model="c_form.first_name" :state="c_form_result.name" placeholder="Diana Ferens"></b-form-input>
                <b-form-valid-feedback :state="c_form_result.first_name">Lock Good</b-form-valid-feedback>
            </div>
            <div class="col-md-4 mb-3">
                <label for="c_form_last_name">Phone</label>
                <b-form-input type="text" id="c_form_phone" v-model="c_form.last_name" :state="c_form_result.phone" placeholder="+44 7506955712"></b-form-input>
                <b-form-valid-feedback :state="c_form_result.phone">Lock Good</b-form-valid-feedback>
            </div>
            <div class="col-md-4 mb-3">
                <label for="c_form_username">Email</label>
                <b-input-group prepend="@">
                    <b-form-input type="email" id="c_form_username" v-model="c_form.username" :state="c_form_result.email" placeholder="annaferens11@gmail.com"></b-form-input>
                </b-input-group>
                <b-form-invalid-feedback :state="c_form_result.username">Please choose a unique and valid email.</b-form-invalid-feedback>
            </div>
        </div>
        
        <b-button type="submit" variant="primary">Add Customer</b-button>
      </b-form>
    </b-modal>
  </div>
</template>
<script>
  import { mapState, mapGetters } from 'vuex';
  import ProductList from '../product-list/product-list'
  import CKEditor from '@ckeditor/ckeditor5-vue';
  import ClassicEditor from '@ckeditor/ckeditor5-build-classic';
  // import func from 'vue-editor-bridge';
  
  export default {
    name:'Product',
    components: {
      ProductList,
      ckeditor: CKEditor.component,
    },
    data(){
      return{
        filter:{
          currentPage: 1,
          totalPages: 0,
        },
        counter: 1,
        modalShow: false,
        payModalShow: false,
        customerModalShow: false,
        editor: ClassicEditor,
        editorData: '<p>Content of the editor.</p>',
        editorConfig: {
          config:{ height: '30px' }
        },
        c_form:{
          first_name:'',
          zip:''
        },
        c_form_result:{
          first_name:null,
          zip:null
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

      //Customer Modal Show
      addCustomerModalShow: function() {
        this.customerModalShow = true;
      },
      addCustomerModalClose: function() {
        this.customerModalShow = false;
      }
    }
  };
</script>
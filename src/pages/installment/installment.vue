<template>
  <div>
    <Breadcrumbs main="" title="Installment"/>
    <!-- Container-fluid starts-->
    <div class="container-fluid pos_custom_table">
        <b-card header-tag="div" no-body>
            <b-card-body>
                <b-tabs pills slot="header" class="tabbed-card border-tab nav-primary">
                    <a @click="quickView" class="new_order_link">Re-installment</a>
                    <b-tab title="Order List">
                        <template slot="title">
                            Order List
                        </template>
                        <b-card-text>
                            <div class="row">
                                <div class="card-body">
                                    <div class="datatable-vue m-0">
                                        <div class="row filter-smart">
                                            <div class="col-sm-2">
                                                <input class="form-control" v-model="filters.name.value" placeholder="Name"/>
                                            </div>
                                            <div class="col-sm-2">
                                                <input class="form-control" v-model="filters.position.value" placeholder="Position" />
                                            </div>
                                            <div class="col-sm-2">
                                                <input class="form-control" v-model="filters.office.value" placeholder="Office" />
                                            </div>
                                            <div class="col-sm-2">
                                                <input class="form-control" v-model="filters.age.value" placeholder="Age" />
                                            </div>
                                            <div class="col-sm-2">
                                                <input class="form-control" v-model="filters.startdate.value" placeholder="Start Date" />
                                            </div>
                                            <div class="col-sm-2">
                                                <input class="form-control" v-model="filters.salary.value" placeholder="Salary" />
                                            </div>
                                        </div>
                                        <div class="table-responsive vue-smart">
                                            <v-table
                                                :data="users.data" class="table"
                                                :currentPage.sync="filter.currentPage"
                                                :pageSize="10"
                                                @totalPagesChanged="filter.totalPages = $event"
                                                :filters="filters"
                                            >
                                                <thead slot="head">
                                                    <v-th sortKey="name">Name</v-th>
                                                    <v-th sortKey="position">Position</v-th>
                                                    <v-th sortKey="office">Office</v-th>
                                                    <v-th sortKey="age">Age</v-th>
                                                    <v-th sortKey="startdate">Start date</v-th>
                                                    <v-th sortKey="salary">Salary</v-th>
                                                    <v-th sortKey="salary">Action</v-th>
                                                </thead>
                                                <tbody slot="body" slot-scope="{displayData}">
                                                    <tr v-for="row in displayData" :key="row.id">
                                                        <td>{{ row.name }}</td>
                                                        <td>{{ row.position }}</td>
                                                        <td>{{ row.office }}</td>
                                                        <td>{{ row.age }}</td>
                                                        <td>{{ row.startdate }}</td>
                                                        <td>{{ row.salary }}</td>
                                                        <td>
                                                            <b-button-group>
                                                                <b-dropdown left text="Operations" variant="light">
                                                                    <b-dropdown-item variant="primary" @click="quickView">Pay</b-dropdown-item>
                                                                    <b-dropdown-item variant="secondary">Edit</b-dropdown-item>
                                                                    <b-dropdown-item variant="danger">Print</b-dropdown-item>
                                                                </b-dropdown>
                                                            </b-button-group>
                                                        </td>
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
                            </div>
                        </b-card-text>
                    </b-tab>
                </b-tabs>
            </b-card-body>
        </b-card>
    </div>
    <!-- Container-fluid Ends-->
    <!-- QuickView Modal -->
    <b-modal size="lg" centered v-model="modalShow" hide-footer hide-header>
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
import users from '../../data/users';
  
  export default {
    name:'Orders',
    components: {
    },
    data(){
      return{
        users,
        counter: 1,
        modalShow: false,
        filter:{
          currentPage: 1,
          totalPages: 0,
        },
        filters: {
          name: { value: '', keys: ['name'] },
          position: { value: '', keys: ['position'] },
          office: { value: '', keys: ['office'] },
          age: { value: '', keys: ['age'] },
          startdate: { value: '', keys: ['startdate'] },
          salary: { value: '', keys: ['salary'] },
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

    }
  };
</script>
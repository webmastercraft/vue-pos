<template>
  <div>
    <Breadcrumbs main="" title="Expenses"/>
    <!-- Container-fluid starts-->
    <div class="container-fluid pos_custom_table">
        <b-card header-tag="div" no-body>
            <b-card-body>
                <b-tabs pills slot="header" class="tabbed-card border-tab nav-primary">
                    <b-tab title="Order List">
                        <template slot="title">
                            Expenses
                        </template>
                        <div class="row">
                            <a @click="quickView" class="new_order_link">New Expense</a>
                            <div class="row filter-smart">
                                <div class="col-sm-2 col-12">
                                    <input class="form-control" v-model="filters.name.value" placeholder="Name"/>
                                </div>
                                <div class="col-sm-2 col-12">
                                    <input class="form-control" v-model="filters.position.value" placeholder="Position" />
                                </div>
                                <div class="col-sm-2 col-12">
                                    <input class="form-control" v-model="filters.office.value" placeholder="Office" />
                                </div>
                                <div class="col-sm-2 col-12">
                                    <input class="form-control" v-model="filters.age.value" placeholder="Age" />
                                </div>
                                <div class="col-sm-2 col-12">
                                    <input class="form-control" v-model="filters.startdate.value" placeholder="Start Date" />
                                </div>
                                <div class="col-sm-2 col-12">
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
                                                        <b-dropdown-item variant="primary" @click="billModalShow">Add</b-dropdown-item>
                                                        <b-dropdown-item variant="secondary" @click="billModalShow">Edit</b-dropdown-item>
                                                        <b-dropdown-item variant="danger" @click="confirmModalShow">Remove</b-dropdown-item>
                                                        <b-dropdown-item variant="success">Pay</b-dropdown-item>
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
                    </b-tab>
                </b-tabs>
            </b-card-body>
        </b-card>
    </div>
    <!-- Container-fluid Ends-->
    <!-- Add Type Modal -->
    <b-modal size="sm" centered v-model="modalShow" hide-footer hide-header>
        <button class="close cateModalCloseBtn" type="button" v-on:click="quickViewClose(modalShow)">
            <span>×</span>
        </button>
        <!-- <b-form class="needs-validation" @submit="onCustomStyleSubmit"> -->
        <b-form class="needs-validation add-category-modal">
            <div class="form-row">
                <div class="col-md-12 mb-3">
                    <label for="c_form_first_name">Expense Type</label>
                    <b-form-input type="text" 
                        id="c_form_cate_name" 
                        v-model="c_expense_type_form.exp_type_name" 
                        :state="c_expense_type_result.expense_name" 
                        placeholder="Diana Ferens"
                    >
                    </b-form-input>
                    <b-form-valid-feedback :state="c_expense_type_result.expense_name">Lock Good</b-form-valid-feedback>
                </div>
                <!-- <div class="col-md-4 mb-3">
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
                </div> -->
            </div>
            <b-button type="submit" variant="primary">Add Type</b-button>
        </b-form>
    </b-modal>
    <!-- Add Bill Modal -->
    <b-modal size="lg" centered v-model="billmShow" hide-footer hide-header>
        <button class="close" type="button" v-on:click="billModalClose(billmShow)">
            <span>×</span>
        </button>
        <div class="card add-item-modal">
            <div class="card-header">
              <h5>New Bill</h5>
            </div>
            <form class="form theme-form">
                <div class="card-body">
                    <div class="row">
                        <div class="col">
                            <div class="form-group row">
                                <label class="col-sm-2 col-form-label">Bill Name</label>
                                <div class="col-sm-3">
                                    <b-form-input type="text" ></b-form-input>
                                </div>
                                <label class="col-sm-2 col-form-label">Bill Description</label>
                                <div class="col-sm-3">
                                    <b-form-input type="text" placeholder="Type your title in Placeholder"></b-form-input>
                                </div>
                            </div>

                            <div class="form-group row">
                                <label class="col-sm-2 col-form-label">Type</label>
                                <div class="col-sm-3">
                                    <b-form-select v-model="tempTypeSelected" :options="tempType"></b-form-select>
                                </div>
                                <label class="col-sm-2 col-form-label">Amount</label>
                                <div class="col-sm-3">
                                    <b-form-input type="number" placeholder="Number"></b-form-input>
                                </div>
                            </div>

                            <div class="form-group row">
                                <label class="col-sm-2 col-form-label">Bill Date</label>
                                <div class="col-sm-3">
                                    <b-form-input type="date" value="2018-01-01" ></b-form-input>
                                </div>
                                <label class="col-sm-2 col-form-label">Status</label>
                                <div class="col-sm-3">
                                    <b-form-input type="text" value="false" class="add_item_input"></b-form-input>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="card-footer">
                    <div class="col-sm-12">
                        <b-button type="submit" variant="primary" class="mr-1" centered>Add</b-button>
                    </div>
                </div>
            </form>
        </div>
    </b-modal>
    <!-- confirm Modal -->
    <b-modal size="sm" centered v-model="confirmShow" hide-footer hide-header>
      <button class="close cateModalCloseBtn" type="button" v-on:click="confirmModalClose(confirmShow)">
          <span>×</span>
      </button>
      <div class="add-category-modal">
        <h5>Are you sure?</h5>
        <b-button type="submit" variant="primary m-r-10">Sure</b-button>
        <b-button type="submit" variant="danger">Cancel</b-button>
      </div>
    </b-modal>
  </div>
</template> 
<script>
import { mapState, mapGetters } from 'vuex';
import users from '../../data/users';
  
  export default {
    name:'Expenses',
    components: {
    },
    data(){
      return{
        users,
        counter: 1,
        modalShow: false,
        billmShow: false,
        confirmShow:false,
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
        c_expense_type_form:{
          exp_type_name:'',
        },
        c_expense_type_result:{
          expense_name:null,
        },
        tempType:[
          { value:null, text: 'Open this select menu' },
          { value:1, text: 'One' },
          { value:2, text: 'Two' },
          { value:3, text: 'Three' },
        ],
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

      //Bill Modal Show
      billModalShow: function() {
        this.billmShow = true;
      },
      billModalClose: function() {
        this.billmShow = false;
      },
      
      //Confirm Modal Show
      confirmModalShow: function() {
        this.confirmShow = true;
      },
      confirmModalClose: function() {
        this.confirmShow = false;
      }
    }
  };
</script>
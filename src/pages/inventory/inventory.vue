<template>
  <div>
    <Breadcrumbs main="" title="Inventory"/>
    <!-- Container-fluid starts-->
    <div class="container-fluid pos_custom_table">
        <b-card header-tag="div" no-body>
            <b-card-body>
                <b-tabs pills slot="header" class="tabbed-card border-tab nav-primary">
                    <b-tab title="Categories" active>
                        <template slot="title">
                            Categories
                        </template>
                        <div class="row">
                            <a @click="addCategoryModalShow" class="new_order_link">New Category</a>
                            <div class="col-sm-12 empty-cart-cls text-center"  v-if="!cart.length">
                                <img :src='getImgUrl("ecommerce/icon-empty-cart.png")' class="img-fluid mb-4">
                                <h3><strong>Your Cart is Empty</strong></h3>
                                <h4>Add something to make me happy :)</h4>
                                <router-link :to="'/ecommerce/product'" class="btn btn-primary cart-btn-transform m-t-15">continue shopping</router-link>
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
                                        <!-- <v-th sortKey="position">ID</v-th> -->
                                        <v-th sortKey="name">Name</v-th>
                                        <v-th sortKey="name">Action</v-th>
                                    </thead>
                                    <tbody slot="body" slot-scope="{displayData}">
                                        <tr v-for="row in displayData" :key="row.id">
                                            <!-- <td>{{ row.id }}</td> -->
                                            <td>{{ row.name }}</td>
                                            <td>
                                                <b-button-group>
                                                    <b-dropdown left text="Operations" variant="light">
                                                        <b-dropdown-item variant="primary">Duplicate</b-dropdown-item>
                                                        <b-dropdown-item variant="secondary" @click="addCategoryModalShow">Edit</b-dropdown-item>
                                                        <b-dropdown-item variant="danger" @click="confirmModalShow"> Remove</b-dropdown-item>
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
                    <b-tab title="Items">
                        <template slot="title">
                            Items
                        </template>
                        <div class="row">
                            <a @click="addItemModal" class="new_order_link">New Item</a>
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
                                                        <b-dropdown-item variant="primary">Duplicate</b-dropdown-item>
                                                        <b-dropdown-item variant="secondary" @click="addItemModal">Edit</b-dropdown-item>
                                                        <b-dropdown-item variant="danger" @click="confirmModalShow"> Remove</b-dropdown-item>
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
                    <b-tab title="Discounts">
                        <template slot="title">
                            Discounts
                        </template>
                        <div class="row">
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
                                                        <b-dropdown-item variant="primary">Discount</b-dropdown-item>
                                                        <b-dropdown-item variant="secondary">Edit</b-dropdown-item>
                                                        <b-dropdown-item variant="danger"> Remove</b-dropdown-item>
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
                    <b-tab title="Discounts">
                        <template slot="title">
                            Stocks
                        </template>
                        <div class="row">
                            <a @click="addStockModalShow" class="new_order_link">New Stock</a>
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
                                        <v-th sortKey="name">Supplier</v-th>
                                        <v-th sortKey="position">Warehouse</v-th>
                                        <v-th sortKey="office">Date</v-th>
                                        <v-th sortKey="age">Product</v-th>
                                        <v-th sortKey="startdate">Quantity</v-th>
                                        <v-th sortKey="salary">Expiration</v-th>
                                        <v-th sortKey="salary">Action</v-th>
                                    </thead>
                                    <tbody slot="body" slot-scope="{displayData}">
                                        <tr v-for="row in displayData" :key="row.id">
                                            <td>{{ row.name }}</td>
                                            <td>{{ row.position }}</td>
                                            <td>{{ row.startdate }}</td>
                                            <td>{{ row.age }}</td>
                                            <td>{{ row.age }}</td>
                                            <td>{{ row.startdate }}</td>
                                            <td>
                                                <b-button-group>
                                                    <b-dropdown left text="Operations" variant="light">
                                                        <b-dropdown-item variant="secondary" @click="addStockModalShow">Edit</b-dropdown-item>
                                                        <b-dropdown-item variant="danger"> Remove</b-dropdown-item>
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
    <!-- addItemModal Modal -->
    <b-modal size="lg" centered v-model="modalShow" hide-footer hide-header>
        <button class="close" type="button" v-on:click="addItemModalClose(modalShow)">
            <span>×</span>
        </button>
        <div class="card add-item-modal">
            <div class="card-header">
                <h5>New Item</h5>
            </div>
            <form class="form theme-form">
                <div class="card-body">
                    <div class="row">
                        <div class="col">
                            <div class="form-group row">
                                <label class="col-sm-2 col-form-label">Name</label>
                                <div class="col-sm-3">
                                    <b-form-input type="text" ></b-form-input>
                                </div>
                                <label class="col-sm-2 col-form-label">Description</label>
                                <div class="col-sm-3">
                                    <b-form-input type="text" placeholder="Type your title in Placeholder"></b-form-input>
                                </div>
                            </div>

                            <div class="form-group row">
                                <label class="col-sm-2 col-form-label">Bar Code</label>
                                <div class="col-sm-3">
                                    <b-form-input type="text" placeholder="bar code"></b-form-input>
                                </div>
                                <label class="col-sm-2 col-form-label">Cost Price</label>
                                <div class="col-sm-3">
                                    <b-form-input type="number" placeholder="Number"></b-form-input>
                                </div>
                            </div>

                            <div class="form-group row">
                                <label class="col-sm-2 col-form-label">Taxes</label>
                                <div class="col-sm-3">
                                    <b-form-input type="number" placeholder="Number"></b-form-input>
                                </div>
                                <label class="col-sm-2 col-form-label">Selling Price</label>
                                <div class="col-sm-3">
                                    <b-form-input type="number" placeholder="Number"></b-form-input>
                                </div>
                            </div>

                            <div class="form-group row">
                                <div class="col-sm-9">
                                    <b-form-checkbox name="checkbox7" >Include Taxes</b-form-checkbox>
                                </div>
                            </div>

                            <div class="form-group row">
                                <label class="col-sm-2 col-form-label">Type</label>
                                <div class="col-sm-3">
                                    <b-form-select v-model="tempTypeSelected" :options="tempType"></b-form-select>
                                </div>
                                <label class="col-sm-2 col-form-label">Upload Image</label>
                                <div class="col-sm-5">
                                    <b-form-file class="form-control" plain></b-form-file>
                                </div>
                            </div>

                            <div class="form-group row">
                                <div class="modal-part-section col-sm-6">
                                    <div class="form-group row">
                                        <label class="col-sm-3 col-form-label">Date</label>
                                        <div class="col-sm-9">
                                            <b-form-input type="date" value="2018-01-01" ></b-form-input>
                                        </div>
                                        <label class="col-sm-6 col-form-label">Price/Unit</label>
                                        <div class="col-sm-6">
                                            <b-form-input type="number" placeholder="Number" class="add_item_input"></b-form-input>
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label class="col-sm-6 col-form-label">Qty</label>
                                        <div class="col-sm-6">
                                            <b-form-input type="number" placeholder="Number"></b-form-input>
                                        </div>
                                        <label class="col-sm-6 col-form-label">Unit</label>
                                        <div class="col-sm-6">
                                            <b-form-input type="number" placeholder="Number" class="add_item_input"></b-form-input>
                                        </div>
                                    </div>
                                </div>

                                <div class="modal-part-section col-sm-6">
                                    <div class="form-group row">
                                        <label class="col-sm-3 col-form-label">Date</label>
                                        <div class="col-sm-9">
                                            <b-form-input type="date" value="2018-01-01" ></b-form-input>
                                        </div>
                                        <label class="col-sm-6 col-form-label">Price/Unit</label>
                                        <div class="col-sm-6">
                                            <b-form-input type="number" placeholder="Number" class="add_item_input"></b-form-input>
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label class="col-sm-6 col-form-label">Qty</label>
                                        <div class="col-sm-6">
                                            <b-form-input type="number" placeholder="Number"></b-form-input>
                                        </div>
                                        <label class="col-sm-6 col-form-label">Unit</label>
                                        <div class="col-sm-6">
                                            <b-form-input type="number" placeholder="Number" class="add_item_input"></b-form-input>
                                        </div>
                                    </div>
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
    <b-modal size="lg" centered v-model="stockModalShow" hide-footer hide-header>
        <button class="close" type="button" v-on:click="addStockModalShow(stockModalShow)">
            <span>×</span>
        </button>
        <div class="card add-item-modal">
            <div class="card-header">
                <h5>New Stock</h5>
            </div>
            <form class="form theme-form">
                <div class="card-body">
                    <div class="row">
                        <div class="col">
                            <div class="form-group row">
                                <label class="col-sm-2 col-form-label">Supplier</label>
                                <div class="col-sm-3">
                                    <b-form-select v-model="tempTypeSelected" :options="tempType"></b-form-select>
                                </div>
                                <label class="col-sm-2 col-form-label">Supplier</label>
                                <div class="col-sm-3">
                                    <b-form-select v-model="tempTypeSelected" :options="tempType"></b-form-select>
                                </div>
                            </div>

                            <div class="form-group row">
                                <label class="col-sm-2 col-form-label">Date</label>
                                <div class="col-sm-4">
                                    <b-form-input type="date" value="2018-01-01" ></b-form-input>
                                </div>
                                <label class="col-sm-2 col-form-label">Qty</label>
                                <div class="col-sm-3">
                                    <b-form-input type="number" placeholder="Number"></b-form-input>
                                </div>
                            </div>

                            <div class="form-group row">
                                <label class="col-sm-2 col-form-label">Product</label>
                                <div class="col-sm-4">
                                    <input class="form-control" type="text" placeholder="Search..">
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
    <b-modal size="sm" centered v-model="cateModalShow" hide-footer hide-header>
        <button class="close cateModalCloseBtn" type="button" v-on:click="addCategoryModalClose(cateModalShow)">
            <span>×</span>
        </button>
        <!-- <b-form class="needs-validation" @submit="onCustomStyleSubmit"> -->
        <b-form class="needs-validation add-category-modal">
            <div class="form-row">
                <div class="col-md-12 mb-3">
                    <label for="c_form_first_name">Category Name</label>
                    <b-form-input type="text" 
                        id="c_form_cate_name" 
                        v-model="c_cate_form.category_name" 
                        :state="c_cate_form_result.category_name" 
                        placeholder="Diana Ferens"
                    >
                    </b-form-input>
                    <b-form-valid-feedback :state="c_cate_form_result.category_name">Lock Good</b-form-valid-feedback>
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
            <b-button type="submit" variant="primary">Add Customer</b-button>
        </b-form>
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
    name:'Inventory',
    components: {
    },
    data(){
      return{
        users,
        counter: 1,
        confirmShow:false,
        modalShow: false,
        cateModalShow: false,
        stockModalShow: false,
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
        c_cate_form:{
          category_name:'',
        },
        c_cate_form_result:{
          category_name:null,
        },
        tempType:[
          { value:null, text: 'Open this select menu' },
          { value:1, text: 'One' },
          { value:2, text: 'Two' },
          { value:3, text: 'Three' },
        ],
        tempTypeSelected: null,
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
      addItemModal: function() {
        this.modalShow = true;
      },
      addItemModalClose: function() {
        this.modalShow = false;
      },

      //Category Modal Show
      addCategoryModalShow: function() {
        this.cateModalShow = true;
      },
      addCategoryModalClose: function() {
        this.cateModalShow = false;
      },
      
      //Stack Modal Show
      addStockModalShow: function() {
        this.stockModalShow = true;
      },
      addStockModalClose: function() {
        this.stockModalShow = false;
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
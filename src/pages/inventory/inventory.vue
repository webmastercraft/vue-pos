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
                                                        <b-dropdown-item variant="primary">Add</b-dropdown-item>
                                                        <b-dropdown-item variant="secondary">Edit</b-dropdown-item>
                                                        <b-dropdown-item variant="danger"> Remove</b-dropdown-item>
                                                        <b-dropdown-item variant="danger"> Duplicate</b-dropdown-item>
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
                                                                    <b-dropdown-item variant="primary">Add</b-dropdown-item>
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
                                </div>
                            </div>
                        </b-card-text>
                    </b-tab>
                    <b-tab title="Discounts">
                        <template slot="title">
                            Discounts
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
                                                                    <b-dropdown-item variant="primary">Add</b-dropdown-item>
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
        <button class="close" type="button" v-on:click="quickViewClose(modalShow)">
            <span>×</span>
        </button>
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
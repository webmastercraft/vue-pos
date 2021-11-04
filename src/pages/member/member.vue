<template>
  <div>
    <Breadcrumbs main="" title="Inventory"/>
    <!-- Container-fluid starts-->
    <div class="container-fluid pos_custom_table">
        <b-card header-tag="div" no-body>
            <b-card-body>
                <b-tabs pills slot="header" class="tabbed-card border-tab nav-primary">
                    <b-tab title="Income">
                        <template slot="title">
                            Groups
                        </template>
                        <b-card-text>
                            <div class="row">
                                <div class="card-body">
                                    <div class="datatable-vue m-0">
                                        <div class="row filter-smart">
                                           <button @click="quickView" class="btn btn-outline-primary add_option_btn">Add Group</button>
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
                                                    <v-th sortKey="position">Total Members</v-th>
                                                </thead>
                                                <tbody slot="body" slot-scope="{displayData}">
                                                    <tr v-for="row in displayData" :key="row.id">
                                                        <td>{{ row.name }}</td>
                                                        <td>{{ row.age }}</td>
                                                        <td>
                                                            <b-button-group>
                                                                <b-dropdown left text="Operations" variant="light">
                                                                    <b-dropdown-item variant="primary">View</b-dropdown-item>
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
                    <b-tab title="Expenses">
                        <template slot="title">
                            Member
                        </template>
                        <b-card-text>
                            <div class="row">
                                <div class="card-body">
                                    <div class="datatable-vue m-0">
                                        <div class="row filter-smart">
                                           <button @click="quickMemberView" class="btn btn-outline-primary add_option_btn">Add Member</button>
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
        <div class="col-sm-12 modal_header_custom">
            <h5 class="mb-0">New Group</h5>
            <button class="close close_btn" type="button" v-on:click="quickViewClose(modalShow)">
                <span>×</span>
            </button>
        </div>
        <b-form class="needs-validation">
            <div class="form-row m-t-10">
                <div class="col-md-6">
                    <label for="browser_first_name">Name</label>
                    <b-form-input type="text" id="browser_first_name" required placeholder="Group name"></b-form-input>
                </div>
                <div class="col-md-6 m-t-3">
                    <label for="browser_last_name">Existing Groups</label>
                    <b-form-select 
                        class="form-control"
                        v-model="supported_form.selected" 
                        :options="supported_form_select_options" 
                        :state="supported_select_state"
                    >
                    </b-form-select>
                </div>
            </div>
            <div class="col-sm-12">
                <h5 class="m-t-10">Permissions</h5>
            </div>
            <div class="form-row">
                <div class="form-group checkbox col-sm-6">
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox7" >Option 1</b-form-checkbox>
                    </div>
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox8">Option 2</b-form-checkbox>
                    </div>
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox9">Option 3</b-form-checkbox>
                    </div>
                </div>
                <div class="form-group checkbox col-sm-6">
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox7" >Option 1</b-form-checkbox>
                    </div>
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox8">Option 2</b-form-checkbox>
                    </div>
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox9">Option 3</b-form-checkbox>
                    </div>
                </div>
            </div>

            <b-button type="submit" variant="primary">Submit Form</b-button>
        </b-form>
    </b-modal>

    <b-modal size="lg" centered v-model="memberModalShow" hide-footer hide-header>
        <div class="col-sm-12 modal_header_custom">
            <h5 class="mb-0">New Member</h5>
            <button class="close close_btn" type="button" v-on:click="quickMemberViewClose(memberModalShow)">
                <span>×</span>
            </button>
        </div>
        <b-form class="needs-validation">
            <div class="form-row m-t-10">
                <div class="col-md-6">
                    <label for="browser_first_name">Name</label>
                    <b-form-input type="text" id="browser_first_name" required placeholder="User name"></b-form-input>
                </div>
                <div class="col-md-6 m-t-3">
                    <label for="browser_last_name">Existing Groups</label>
                    <b-form-select 
                        class="form-control"
                        v-model="supported_form.selected" 
                        :options="supported_form_select_options" 
                        :state="supported_select_state"
                    >
                    </b-form-select>
                </div>
            </div>
            <div class="col-sm-12">
                <h5 class="m-t-10">Permissions</h5>
            </div>
            <div class="form-row">
                <div class="form-group checkbox col-sm-6">
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox7" >Option 1</b-form-checkbox>
                    </div>
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox8">Option 2</b-form-checkbox>
                    </div>
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox9">Option 3</b-form-checkbox>
                    </div>
                </div>
                <div class="form-group checkbox col-sm-6">
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox7" >Option 1</b-form-checkbox>
                    </div>
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox8">Option 2</b-form-checkbox>
                    </div>
                    <div class="checkbox checkbox-dark">
                        <b-form-checkbox name="checkbox9">Option 3</b-form-checkbox>
                    </div>
                </div>
            </div>

            <b-button type="submit" variant="primary">Submit Form</b-button>
        </b-form>
    </b-modal>
  </div>
</template> 
<script>
import { mapState, mapGetters } from 'vuex';
import users from '../../data/users';
  
  export default {
    name:'Member',
    components: {
    },
    data(){
      return{
        users,
        counter: 1,
        modalShow: false,
        memberModalShow: false,

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
        supported_form:{
          checkbox:false,
          radio:null,
          selected: null,
          file:null
        },
        supported_form_select_options: [
          { value: null, text:'Current Group Names' },
          { value: '1', text:'One' },
          { value: '2', text:'Two' },
          { value: '3', text:'Three' },
        ],
        supported_form_radio_options: [
          { text: 'Toggle this custom radio', value: 'first' },
          { text: 'Or toggle this other custom radio', value: 'second' }
        ],
      };
    },
    computed: {
      ...mapState({
        cart: state => state.products.cart,
        getAmount() {
          return this.totalAmount = this.$store.getters['products/getTotalAmount'];
        },
        supported_select_state(){
            if(this.supported_form.selected == null) {
                return false;
            } else {
                return true;
            }
        },
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
        quickMemberView: function() {
            this.memberModalShow = true;
        },

        quickMemberViewClose: function() {
            this.memberModalShow = false;
        },
    }
  };
</script>
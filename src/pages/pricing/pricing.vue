<template>
    <div>
        <Breadcrumbs title="Pricing"/>
        <!-- Container-fluid starts-->
        <div class="container-fluid">
            <div class="row">
              <div class="col-sm-12">
                <px-card title="Become member" :actions="false">
                  <div slot="with-padding">
                    <div class="row">
                      <div class="col-xl-3 col-sm-6 ">
                        <div class="pricingtable mt-xl-0 mt-lg-4">
                          <div class="pricingtable-header">
                            <h3 class="title">Super</h3>
                          </div>
                          <div class="price-value"><span class="currency">$</span><span class="amount">100</span><span class="duration">/mo</span></div>
                          <ul class="pricing-content">
                            <li>Upload 50 product</li>
                            <li>50 Email Accounts</li>
                            <li>Maintenance</li>
                            <li>15 Subdomains</li>
                          </ul>
                          <div class="pricingtable-signup">
                            <a class="btn btn-primary btn-lg" 
                              data-toggle="modal" 
                              data-target="#exampleModalCenter"
                              @click="quickView"
                            >
                              Sign Up
                            </a>
                          </div>
                        </div>
                      </div>
                      <div class="col-xl-3 col-sm-6 ">
                        <div class="pricingtable mt-xl-0 mt-lg-4">
                          <div class="pricingtable-header">
                            <h3 class="title">Auther</h3>
                          </div>
                          <div class="price-value"><span class="currency">$</span><span class="amount">50</span><span class="duration">/mo</span></div>
                          <ul class="pricing-content">
                            <li>Upload 50 product</li>
                            <li>50 Email Accounts</li>
                            <li>Maintenance</li>
                            <li>15 Subdomains</li>
                          </ul>
                          <div class="pricingtable-signup">
                            <a class="btn btn-primary btn-lg" 
                              data-toggle="modal" 
                              data-target="#exampleModalCenter"
                              @click="quickView"
                            >
                              Sign Up
                            </a>
                          </div>
                        </div>
                      </div>
                      <div class="col-xl-3 col-sm-6">
                        <div class="pricingtable">
                          <div class="pricingtable-header">
                            <h3 class="title">Premium</h3>
                          </div>
                          <div class="price-value"><span class="currency">$</span><span class="amount">20</span><span class="duration">/mo</span></div>
                          <ul class="pricing-content">
                            <li>10% on all product</li>
                            <li>50 Email Accounts</li>
                            <li>Maintenance</li>
                            <li>15 Subdomains</li>
                          </ul>
                          <div class="pricingtable-signup">
                            <a class="btn btn-primary btn-lg" 
                              data-toggle="modal" 
                              data-target="#exampleModalCenter"
                              @click="quickView"
                            >
                              Sign Up
                            </a>
                          </div>
                        </div>
                      </div>
                      <div class="col-xl-3 col-sm-6">
                        <div class="pricingtable">
                          <div class="pricingtable-header">
                            <h3 class="title">Standard</h3>
                          </div>
                          <div class="price-value"><span class="currency">$</span><span class="amount">10</span><span class="duration">/mo</span></div>
                          <ul class="pricing-content">
                            <li>50GB Disk Space</li>
                            <li>50 Email Accounts</li>
                            <li>Maintenance</li>
                            <li>15 Subdomains</li>
                          </ul>
                          <div class="pricingtable-signup">
                            <a class="btn btn-primary btn-lg" 
                              data-toggle="modal" 
                              data-target="#exampleModalCenter"
                              @click="quickView"
                            >
                              Sign Up
                            </a>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </px-card>
              </div>
            </div>
          </div>
        <!-- Container-fluid Ends-->
        <!-- QuickView Modal -->
        <b-modal size="lg" centered v-model="modalShow" hide-footer hide-header>
          <button class="close" type="button" v-on:click="quickViewClose(modalShow)">
            <span>×</span>
          </button>
          <div class="card">
              <div class="card-header">
                  <h5>New Customer</h5>
              </div>
              <form class="form theme-form">
                  <div class="card-body">

                      <div class="row">
                          <div class="col">
                              <div class="form-group">
                                  <label>Email address</label>
                                  <b-form-input type="email" placeholder="name@example.com" ></b-form-input>
                              </div>
                          </div>
                      </div>

                      <div class="row">
                          <div class="col">
                              <div class="form-group">
                                  <label>Phone</label>
                                  <b-form-input type="number" placeholder="+1" ></b-form-input>
                              </div>
                          </div>
                      </div>

                      <div class="row">
                          <div class="col">
                              <div class="form-group">
                                  <label>Password</label>
                                  <b-form-input type="password" placeholder="Password" ></b-form-input>
                              </div>
                          </div>
                      </div>

                  </div>

                  <div class="card-footer">
                      <b-button type="submit" variant="primary" class="mr-1">Submit</b-button>
                      <b-button type="reset" variant="light">Cancel</b-button>
                  </div>
              </form>
          </div>
        </b-modal>
    </div>
</template>

<script>
  export default {
    name: 'pricing',

    data() {
      return {
        modalShow: false,
      }
    },
    methods: {

      //Quick View
      quickView: function() {
        console.log('wondering here');
        this.modalShow = true;
      },

      quickViewClose: function() {
        this.modalShow = false;
      },
    }
  };
</script>
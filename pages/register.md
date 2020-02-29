---

title: Global AppSec Dublin Registration
layout: event_noheader
permalink: /register/

---

<style>
[v-cloak] {display: none}

.registration-container {
  max-width: 80%;
}

.product-list-item {
  margin-bottom: 24px;
  padding: 12px;
  display: flex;
  justify-content: space-between;
}

.product-list-item.selected {
  background-color: #F9E79F;
}

.product-name {
  font-weight: bold;
}

.product-description {
  font-size: 0.85rem;
}

.product-price {
  font-weight: bold;
  margin-left: 30px;
  font-size: 1.45rem;
}

.product-button {
  -webkit-border-radius: 6px;
  -moz-border-radius: 6px;
  border-radius: 6px;
  background-color: #ccc;
  border: 1px solid black;
  cursor: pointer;
  padding: 8px;
}

.product-button.selected {
background-color: #ff0000;
color: #ffffff;
}

.registrant-information {
  margin-top: 40px;
}

.event h1, .event h2 {
  margin-left: 0;
}

.event h2 {
  font-size: 22px;
}

.registrant-form div {
  margin: 14px 0px;
}

.registrant-form input {
  width: 100%;
  border: 1px solid #000000;
  padding: 8px;
}

.button-container {
  margin: 20px 0px;
  text-align: center;
}

.error-text {
  color: #ff0000;
  font-size: 75%;
  margin-top: 4px !important;
}

.help-text {
  color: #ADADAD;
  font-size: 75%;
  margin-top: 4px !important;
}

.submit-button {
  border: 0;
  padding: 16px;
  font-weight: bold;
  color: #ffffff;
  background-color: red;
  text-transform: uppercase;
  font-size: 110%;
  -webkit-border-radius: 4.5px;
  -moz-border-radius: 4.5px;
  border-radius: 4.5px;
}

.product-information {
  max-width: 70%;
}

.checkbox-container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.checkbox-container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.checkbox-container .checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
}

.checkbox-container:hover input ~ .checkmark {
  background-color: #ccc;
}

.checkbox-container input:checked ~ .checkmark {
  background-color: #ff0000;
}

.checkbox-container .checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

.checkbox-container input:checked ~ .checkmark:after {
  display: block;
}

.checkbox-container .checkmark:after {
  left: 9px;
  top: 5px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}

@media (max-width: 768px) {
  .registration-container {
    max-width: 100%;
  }
}

@media (min-width: 768px) {
  .registrant-information {
    max-width: 70%;
  }
}
</style>

{% raw %}
## Global AppSec - Dublin 2020 Registration

Welcome to Global AppSec Dublin 2020 presented by the OWASP Foundation. Formerly known as AppSec EU, the Global AppSec Conference is the premier application security conference for developers and security experts. Designed for private and public sector infosec professionals, the OWASP three day training and two day conference equips developers, defenders, and advocates to build a more secure web.

Join us for a celebration of leading application security technologies, speakers, prospects, and community, at this unique event that will build on everything you already know to expect from an OWASP Global Conference. Questions? [events@owasp.com](mailto:events@owasp.com?subject=Global%20AppSec%20Dublin%20Inquiry)

### Logistics
- Conference: Thursday, June 18 through Friday, June 19, 2020
- Training Offerings: JUne 15-17 and agenda to be finalized before March 25th.
- Location: Spencer Dock, North Wall Quay, Dublin 1 D01 T1W6, Ireland
 
 <div id="registration-app" class="registration-container" v-cloak>
 <h2 style="margin-bottom: 20px;">Tickets</h2>
  <div class="product-listing" style="border-bottom: 1px solid #000000; padding-bottom: 20px; margin-bottom: 20px;">
    <div class="product-list-item" v-for="product in productListing">
      <div class="product-information">
        <div class="product-name">
          <strong>{{ product.name }}</strong>
        </div>
        <div class="product-description" v-html="product.description"></div>
      </div>
      <div class="product-price">
        <div class="product-button" v-on:click="toggleProduct(product.sku)" v-bind:class="selectedProducts.includes(product.sku) ? 'selected': ''">
          {{ product.price }}
        </div>
      </div>
    </div>
  </div>
  <form id="registration-information" v-on:submit.prevent="handleSubmit">
    <div class="registrant-information">
      <h2>Attendee Information</h2>
      <div class="registrant-form">
        <div>
          <input type="text" v-model="email" aria-label="Email Address"
          placeholder="Email Address" />
          <div class="error-text" v-if="errors.email">{{ errors.email[0] }}</div>
        </div>
        <div>
          <input type="text" v-model="email_confirm" aria-label="Confirm Email Address"
          placeholder="Confirm Email Address" />
          <div class="error-text" v-if="errors.email_confirm">{{ errors.email_confirm[0] }}</div>
        </div>
        <div>
          <input type="text" v-model="company" aria-label="Company Name"
          placeholder="Company Name" />
          <div class="error-text" v-if="errors.company">{{ errors.company[0] }}</div>
        </div>
        <div style="display: flex; margin-top: 0px; margin-bottom: 0px;">
          <div style="margin-right: 20px;">
            <input type="text" v-model="first_name" aria-label="First Name"
            placeholder="First Name" />
            <div class="error-text" v-if="errors.first_name">{{ errors.first_name[0] }}</div>
          </div>
          <div style="flex: 1;">
            <input type="text" v-model="last_name" aria-label="Last Name"
            placeholder="Last Name" />
            <div class="error-text" v-if="errors.last_name">{{ errors.last_name[0] }}</div>
          </div>
        </div>
        <div>
          <input type="text" v-model="title" aria-label="Title"
          placeholder="Title" />
          <div class="error-text" v-if="errors.title">{{ errors.title[0] }}</div>
        </div>
        <div>
          <input type="text" v-model="dietary_restrictions" aria-label="Dietary Restrictions"
          placeholder="Dietary Restrictions" />
          <div class="error-text" v-if="errors.dietary_restrictions">{{ errors.dietary_restrictions[0] }}</div>
        </div>
        <div style="margin-bottom: 35px; margin-top: 35px;">
      <label class="checkbox-container">Agree to Terms of Purchase <strong>*</strong>
        <input type="checkbox">
        <span class="checkmark"></span>
      </label>
      <label class="checkbox-container">Join the OWASP Mailing List
        <input type="checkbox">
        <span class="checkmark"></span>
      </label>
        </div>
      </div>
      <div class="button-container" style="display: flex;">
      <div style="width: 250px; margin-right: 20px;">
        <button type="submit" style="display: block;" class="submit-button" v-bind:disabled="loading">Purchase Ticket</button>
        </div>
        <div style="margin-bottom: 20px; flex: 1;">
          <input type="text" style="width: 100%; border: 1px solid black;" v-model="discount_code" aria-label="Discount Code (if applicable)"
          placeholder="Discount Code (if applicable)" />
          <div class="error-text" v-if="errors.discount_code">{{ errors.discount_code[0] }}</div>
          <div class="help-text">Note discounts will be applied at checkout</div>
        </div>
      </div>
      <div class="help-text" style="margin-top: 30px;">
        <strong>*</strong> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean pretium, odio vel fermentum condimentum, ipsum dui rhoncus nisl, ut scelerisque arcu nunc ac diam. Curabitur tempus, libero et sodales egestas, massa quam lacinia diam, eu laoreet urna lectus in odio. Aenean consequat, ante nec cursus ornare, libero lectus dignissim ante, id semper leo quam eget dui. Quisque non nunc et risus blandit mattis. Sed lorem enim, bibendum nec luctus eu, pulvinar et nisi. Integer porta bibendum sapien, ut viverra sem placerat a. Curabitur et eros ac enim gravida feugiat sed sed mauris.
      </div>
    </div>
  </form>
</div>
{% endraw %}

<script src="https://unpkg.com/vue"></script>
<script src="https://js.stripe.com/v3"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.15/lodash.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue-scrollto"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.24.0/moment.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
var stripe = Stripe('pk_test_u4OyMFMbz6tp9sit2bjdHRnT00bac5mrL2');
window.addEventListener('load', function () {
  new Vue({
    data: {
      selectedProduct: null,
      selectedProducts: [],
      name: null,
      company: null,
      email: null,
      email_confirm: null,
      discount_code: null,
      first_name: null,
      last_name: null,
      title: null,
      dietary_restrictions: null,
      products: {{ site.data.products | jsonify }},
      errors: {},
      loading: false
    },
    el: '#registration-app',
    computed: {
      productListing: function () {
        let vm = this;
        let products = [];
        _.each(this.products.products, function (product) {
          let shouldDisplay = true;
          if (product.metadata.display_start || product.metadata.display_end) {
            if (product.metadata.display_start) {
              let display_start = moment(product.metadata.display_start)
              if (moment() < display_start) {
                shouldDisplay = false;
              }
            }
            if (product.metadata.display_end) {
              let display_end = moment(product.metadata.display_end)
              if (moment() > display_end) {
                shouldDisplay = false;
              }
            }
          }
          if (shouldDisplay) {
            products.push({
              sku: product.id,
              name: product.name,
              amount: product.amount,
              price: vm.formatPrice(product.amount),
              description: product.metadata.description
            });
          }
        });
        return products;
      }
    },
    watch: {
      selectedProduct: function (newValue) {
        this.$nextTick(function () {
          VueScrollTo.scrollTo('#registration-information');
        })
      }
    },
    methods: {
      formatPrice: function (amount) {
        const formatter = new Intl.NumberFormat('en-US', {
          style: 'currency',
          currency: this.products.currency,
          minimumFractionDigits: 2
        });
        return formatter.format(amount / 100);
      },
      selectProduct: function (sku) {
        this.selectedProduct = sku;
      },
      handleSubmit: function () {
        let vm = this;
        vm.loading = true;
        vm.validateForm();
        if (Object.keys(vm.errors).length > 0) {
          vm.loading = false;
          vm.$nextTick(function () {
            VueScrollTo.scrollTo('.error-text');
          })
        } else {
          const postData = {
            name: vm.first_name,
            company: vm.company,
            email: vm.email,
            sku: vm.selectedProducts[0],
            discount_code: vm.discount_code
          }
          axios.post('https://owaspadmin.azurewebsites.net/api/EventsCheckout?code=qIyazIloMxpvGtTkSI0cXNoDEwzNIcFe9xp7bGm54t0lakuBEKJ73Q==', postData).then(function (response) {
	    stripe.redirectToCheckout({
	      sessionId: response.data.data.session_id
	    }).then(function (result) {
	      console.log(result.error.message)
	    }); 
	  }).catch(function (error) {
	    vm.errors = error.response.data.errors
	    vm.loading = false
	    vm.$nextTick(function () {
	      VueScrollTo.scrollTo('.error-text');
	    })
	  });
	}
      },
      validateForm: function () {
        let errors = {};

        if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(this.email)) {
          errors.email = ['Please enter a valid email address'];
        }


        this.errors = errors;
        },
        toggleProduct: function (productId) {
          if (this.selectedProducts.includes(productId)) {
            const currentIndex = _.findIndex(this.selectedProducts, { sku: productId })
            if (currentIndex !== -1) {
              this.selectedProducts.splice(currentIndex, 1)
            }
          } else {
            this.selectedProducts.push(productId)
          }
        }
    }
  })
})
</script>

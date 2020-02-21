---

title: Register
layout: register
permalink: /register/

---

<style>
[v-cloak] {display: none}

.registration-container {
  max-width: 70%;
  margin-left: auto;
  margin-right: auto;
}

.product-list-item {
  margin-bottom: 8px;
  background-color: #ECECEC;
  border: 1px solid #DADADA;
  padding: 12px;
  -webkit-border-radius: 6px;
  -moz-border-radius: 6px;
  border-radius: 6px;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
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

@media (max-width: 768px) {
  .registration-container {
    max-width: 100%;
  }
}

@media (min-width: 768px) {
  .registrant-information {
    max-width: 60%;
    margin-left: auto;
    margin-right: auto;
  }
}
</style>

{% raw %}
<div id="registration-app" class="registration-container" v-cloak>
  <h1>Register</h1>
  <p style="font-weight: bold;">Select a ticket option below:</p>
  <div class="product-listing">
    <div class="product-list-item" v-for="product in productListing"
    v-bind:class="selectedProduct === product.sku ? 'selected' : ''"
    v-on:click="selectProduct(product.sku)">
      <div class="product-information">
        <div class="product-name">
          <strong>{{ product.name }}</strong>
        </div>
        <div class="product-description" v-html="product.description"></div>
      </div>
      <div class="product-price">
        {{ product.price }}
      </div>
    </div>
  </div>
  <form id="registration-information" v-if="selectedProduct" v-on:submit.prevent="handleSubmit">
    <div class="registrant-information">
      <h2>Registrant Information</h2>
      <div class="registrant-form">
        <div>
          <input type="text" v-model="name" aria-label="Full Name"
          placeholder="Full Name" />
          <div class="error-text" v-if="errors.name">{{ errors.name[0] }}</div>
        </div>
        <div>
          <input type="text" v-model="company" aria-label="Company Name"
          placeholder="Company Name" />
          <div class="error-text" v-if="errors.company">{{ errors.company[0] }}</div>
        </div>
        <div style="margin-bottom: 40px">
          <input type="text" v-model="email" aria-label="Email Address"
          placeholder="Email Address" />
          <div class="error-text" v-if="errors.email">{{ errors.email[0] }}</div>
        </div>
        <div style="margin-bottom: 20px">
          <input type="text" v-model="discount_code" aria-label="Discount Code"
          placeholder="Discount Code" />
          <div class="error-text" v-if="errors.discount_code">{{ errors.discount_code[0] }}</div>
          <div class="help-text">Your discount will be applied at checkout, after submitting this form.</div>
        </div>
      </div>
      <div class="button-container">
        <button type="submit" class="submit-button" v-bind:disabled="loading">Purchase Ticket</button>
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
      name: null,
      company: null,
      email: null,
      discount_code: null,
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
            name: vm.name,
            company: vm.company,
            email: vm.email,
            sku: vm.selectedProduct,
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

        if (!this.name) {
          errors.name = ['Please enter your name'];
        }

        this.errors = errors;
      }
    }
  })
})
</script>

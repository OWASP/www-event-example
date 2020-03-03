<style>
[v-cloak] {display: none}
</style>

{% raw %}
<div id="registration-app" class="registration-container" v-cloak>
  <div class="ticket-listing">
    <h2 style="margin-bottom: 30px;">Tickets</h2>
    <div class="error-text" v-if="errors.product">{{ errors.product[0] }}</div>
    <div class="ticket-option" v-for="product in productListing">
      <div class="ticket-option-information">
        <div class="ticket-option-title" v-on:click="toggleProduct(product.sku)">{{ product.name }}</div>
        <div class="ticket-option-description" v-html="product.description"></div>
      </div>
      <div class="ticket-buy-button">
        <div class="cta-button grey select" v-on:click="toggleProduct(product.sku)" v-bind:class="{ selected: selectedProducts.includes(product.sku) }">
          <div class="product-price">{{ product.price }}</div>
        </div>
      </div>
    </div>
  </div>

  <div class="registration-form">
    <h2 style="margin-bottom: 30px;">Attendee Information</h2>
    <form v-on:submit.prevent="handleSubmit">
      <div class="pad-field">
        <input type="text" v-model="email" aria-label="Email Address"
        placeholder="Email Address" />
        <div class="error-text" v-if="errors.email">{{ errors.email[0] }}</div>
      </div>
      <div class="extra-pad">
        <input type="text" v-model="email_confirm" aria-label="Confirm Email Address"
        placeholder="Confirm Email Address" />
        <div class="error-text" v-if="errors.email_confirm">{{ errors.email_confirm[0] }}</div>
      </div>
      <div class="pad-field">
        <input type="text" v-model="company" aria-label="Company Name"
        placeholder="Company Name" />
        <div class="error-text" v-if="errors.company">{{ errors.company[0] }}</div>
      </div>
      <div class="extra-pad">
        <input type="text" v-model="title" aria-label="Title"
        placeholder="Title" />
        <div class="error-text" v-if="errors.title">{{ errors.title[0] }}</div>
      </div>
      <div class="extra-pad">
        <input type="text" v-model="name" aria-label="Full Name"
        placeholder="Full Name" />
        <div class="error-text" v-if="errors.name">{{ errors.name[0] }}</div>
      </div>
      <div class="extra-pad" style="display: flex">
        <div style="width: 50%; padding-right: 10px;">
          <multi-select v-bind:options="countries" v-model="country" v-bind:searchable="false" v-bind:show-labels="false" placeholder="Country"></multi-select>
        <div class="error-text" v-if="errors.country">{{ errors.country[0] }}</div>
        </div>
        <div style="width: 50%; padding-left: 10px;">
          <input type="text" v-model="city" aria-label="City"
          placeholder="City" />
          <div class="error-text" v-if="errors.city">{{ errors.city[0] }}</div>
        </div>
      </div>
      <div class="extra-pad">
        <multi-select v-bind:options="experienceOptions" v-model="experience" v-bind:searchable="false" v-bind:show-labels="false" placeholder="Select your experience level"></multi-select>
        <div class="error-text" v-if="errors.experience">{{ errors.experience[0] }}</div>
      </div>
      <div class="extra-pad">
        <multi-select v-bind:options="personaOptions" v-model="persona" v-bind:searchable="false" v-bind:show-labels="false" placeholder="Select your persona"></multi-select>
        <div class="error-text" v-if="errors.persona">{{ errors.persona[0] }}</div>
      </div>
      <div class="extra-pad">
        <multi-select v-bind:options="dietaryRestrictionOptions" v-model="dietary_restrictions" v-bind:searchable="false" v-bind:multiple="true" v-bind:show-labels="false" placeholder="Select your dietary restrictions"></multi-select>
        <div class="error-text" v-if="errors.dietary_restrictions">{{ errors.dietary_restrictions[0] }}</div>
      </div>
      <div class="extra-pad">
        <label class="checkbox-container">Agree to Terms of Purchase <sup><strong>*</strong></sup>
          <input type="checkbox" v-model="terms_of_purchase">
          <span class="checkmark"></span>
        </label>
        <div class="error-text" v-if="errors.terms_of_purchase">{{ errors.terms_of_purchase[0] }}</div>
        <label class="checkbox-container">Join the OWASP Mailing List
          <input type="checkbox" v-model="mailing_list">
          <span class="checkmark"></span>
        </label>
      </div>
      <div class="button-discount-code-container">
        <div style="margin-right: 80px;">
          <button class="cta-button purchase-button" type="submit" v-bind:disabled="loading">Purchase Ticket</button>
        </div>
        <div style="flex: 1;">
          <input type="text" class="discount_code" v-model="discount_code" aria-label="Discount Code (if applicable)" placeholder="Discount Code (if applicable)" />
          <div class="error-text" v-if="errors.discount_code">{{ errors.discount_code[0] }}</div>
          <div class="help-text">Note discounts will be applied at checkout</div>
        </div>
      </div>
      <div class="help-text">
        <sup><strong>*</strong></sup> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur vestibulum egestas arcu a suscipit. Nullam feugiat a urna a convallis. Donec in justo quis lacus condimentum euismod. Sed vulputate turpis mi, sed efficitur enim ornare eu. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras quis porttitor ipsum. Nunc bibendum risus orci, ac dapibus lectus gravida sed.
      </div>
    </form>
  </div>
</div>
{% endraw %}

<script src="https://unpkg.com/vue"></script>
<script src="https://js.stripe.com/v3"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.15/lodash.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue-scrollto"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.24.0/moment.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script src="https://unpkg.com/vue-multiselect@2.1.0"></script>
<link rel="stylesheet" href="https://unpkg.com/vue-multiselect@2.1.0/dist/vue-multiselect.min.css">
<script>
var stripe = Stripe('pk_test_u4OyMFMbz6tp9sit2bjdHRnT00bac5mrL2');
window.addEventListener('load', function () {
  const app = new Vue({
    data: {
      selectedProducts: [],
      company: null,
      email: null,
      email_confirm: null,
      discount_code: null,
      name: null,
      title: null,
      dietary_restrictions: null,
      experience: null,
      persona: null,
      country: null,
      city: null,
      terms_of_purchase: false,
      mailing_list: false,
      products: {{ site.data.products | jsonify }},
      countries: {{ site.data.countries | jsonify }},
      errors: {},
      loading: false,
      experienceOptions: [
        'Beginner',
        'Intermediate',
        'Advanced'
      ],
      personaOptions: [
        'Defender',
        'Builder',
        'Breaker'
      ],
      dietaryRestrictionOptions: [
        'Gluten-Free',
        'Halal',
        'Kosher',
        'Nut Allergy',
        'Shellfish Allergy',
        'Vegan',
        'Vegetarian'
      ]
    },
    components: {
      MultiSelect: window.VueMultiselect.default
    },
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
            title: vm.title,
            dietary_restrictions: vm.dietary_restrictions,
            experience: vm.experience,
            persona: vm.persona,
            country: vm.country,
            city: vm.city,
            sku: vm.selectedProducts[0],
            discount_code: vm.discount_code,
            mailing_list: vm.mailing_list
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
        } else {
          if (this.email !== this.email_confirm) {
            errors.email_confirm = ['These email addresses do not match'];
          }
        }

        if (!this.name) {
          errors.name = ['Name is required'];
        }

        if (!this.country) {
          errors.country = ['Country is required'];
        }

        if (!this.city) {
          errors.city = ['City is required'];
        }

        if (!this.experience) {
          errors.experience = ['Experience level is required'];
        }

        if (!this.persona) {
          errors.persona = ['Persona is required'];
        }

        if (!this.selectedProducts.length) {
          errors.product = ['Please select a ticket option below']
        }
        
        if (!this.terms_of_purchase) {
          errors.terms_of_purchase = ['You must agree to the terms of puchase']
        }

        this.errors = errors;
      },
      toggleProduct: function (sku) {
        if (this.selectedProducts.includes(sku)) {
          const existingIndex = _.findIndex(this.selectedProducts, function (product) {
            return product === sku
          })
          if (existingIndex !== -1) {
            this.selectedProducts.splice(existingIndex, 1)
          }
        } else {
          this.selectedProducts.push(sku)
        }
      }
    }
  }).$mount('#registration-app')
})
</script>

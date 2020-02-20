---

title: Register
layout: register
permalink: /registration-success/

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
<div id="registration-app" class="registration-container">
  <h1>Registration Success</h1>
  <p>Thank you for registering for our event</p>
</div>
{% endraw %}

<template>
  <div>Select payment method</div>
  <div class="note">
    You can select either one of the saved cards of pay with a new one
  </div>
  <div>
    <div
      class="payment-method"
      v-for="(pm, i) in paymentMethods"
      :key="i"
      @click="selectPaymentMethod(pm)"
    >
      <div
        class="checkbox"
        :class="{ __checked: pm.pmId == selectedPaymentMethodId }"
      ></div>
      {{ pm.label }}
    </div>
  </div>
  <div class="payment-methods"></div>
  <div v-show="selectedPaymentMethodId == 'newCard'">
    <div class="note">
      You can use the following test data:<br />
      Card number: 4242 4242 4242 4242<br />
      Expiry Date: any date in the future<br />
      CVC: any 3 digits
    </div>
    <div class="input" id="cardNumber"></div>
    <div class="input" id="cardExpiry"></div>
    <div class="input" id="cvc"></div>
  </div>
  <VButton :disabled="!this.selectedPaymentMethod" @click="pay()"
    >Complete payment</VButton
  >
</template>

<script>
const stripePublishableKey =
  "pk_test_51Jq8pbHWgezegvqG9tSaoQCavBBhTuGjN61JANKLWtuuPddxuf38Xoh0AW1mr4aEcO0ilawn3NTME5pJqxj0zEku00Ey3esKza";

import VButton from "./VButton.vue";

export default {
  name: "CheckoutDemo",
  components: {
    VButton,
  },
  data() {
    return {
      stripe: null,

      // mocked data for saved payment methods
      // in the real case you will get this data from the backend
      savedPaymentMethods: [
        { type: "savedCard", pmId: "saved-card-1", label: "Saved MC" },
        { type: "savedCard", pmId: "saved-card-2", label: "Saved Visa" },
      ],
      selectedPaymentMethodId: null,
    };
  },
  computed: {
    paymentMethods() {
      const newCard = {
        type: "newCard",
        pmId: "newCard",
        label: "New Card",
      };
      const pms = [...this.savedPaymentMethods, newCard];
      return pms;
    },
    selectedPaymentMethod() {
      return this.paymentMethods.find(
        (pm) => pm.pmId == this.selectedPaymentMethodId
      );
    },
  },
  methods: {
    selectPaymentMethod(pm) {
      this.selectedPaymentMethodId = pm.pmId;
    },
    initStripeElements() {
      const stripe = Stripe(stripePublishableKey);
      this.stripe = stripe;

      const elements = stripe.elements({ locale: "en" });
      const cardElement = elements.create("cardNumber");
      cardElement.mount("#cardNumber");
      elements.create("cardExpiry").mount("#cardExpiry");
      elements.create("cardCvc").mount("#cvc");

      window.cardElement = cardElement;
    },
    createStripePaymentMethod() {
      return this.stripe
        .createPaymentMethod({
          type: "card",
          card: window.cardElement,
        })
        .then(function (result) {
          return result;
        });
    },
    async pay() {
      const paymentMethod = this.selectedPaymentMethod;

      if (paymentMethod.type == "savedCard") {
        // in case a user pays with a save card you can just proceed
        // and send pm.id as a payment method to the server
        console.log(`Pay with ${paymentMethod.pmId}`);
      }

      if (paymentMethod.type == "newCard") {
        // in case user pays with a new card
        // you have to create Stripe PaymentMethod first
        // and then pass it's id to the server
        const stripePm = await this.createStripePaymentMethod();
        console.log(`Pay with stipe Payment Method ${stripePm}`);
        return false;
      }
    },
  },
  mounted() {
    this.initStripeElements();
  },
};
</script>

<style scoped>
.note {
  color: #999;
  padding: 10px 0px;
}
.input {
  padding: 12px 10px 10px 10px;
  margin-bottom: 8px;
  border-radius: 2px;
  background: #fff;
  font-family: system-ui;
}
.checkbox {
  position: relative;
  width: 20px;
  height: 20px;
  margin-right: 10px;
  border-radius: 2px;
  background: #1a1a1b;
  float: left;
}
.checkbox.__checked::before {
  position: absolute;
  width: 4px;
  height: 4px;
  left: 8px;
  top: 8px;
  background: #fff;
  display: block;
  content: " ";
}
.payment-method {
  line-height: 20px;
  padding: 10px 0px;
}
.card-number {
  border: 1px red solid;
  padding: 15px 20px;
}
</style>

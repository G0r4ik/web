<template>
  <!-- <div v-for="item in items" :key="item.id">
    {{ item.id }}
  </div> -->
  <header>
    <div class="container">
      <div class="header__inner">
        <a href="#" class="header__logo">The Best Shop</a>
        <button class="header__cart" @click="showCartModal = true">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 60 60"
            style="enable-background: new 0 0 60 60"
            xml:space="preserve"
          >
            <path
              d="m11.68 13-.833-5h-2.99C7.411 6.28 5.859 5 4 5 1.794 5 0 6.794 0 9s1.794 4 4 4c1.859 0 3.411-1.28 3.858-3h1.294l.5 3h-.038l5.171 26.016c-2.465.188-4.518 2.086-4.76 4.474A5.013 5.013 0 0 0 15 49h2c0 3.309 2.691 6 6 6s6-2.691 6-6h11c0 3.309 2.691 6 6 6s6-2.691 6-6h4a1 1 0 1 0 0-2h-4.35c-.826-2.327-3.043-4-5.65-4s-4.824 1.673-5.65 4h-11.7c-.826-2.327-3.043-4-5.65-4s-4.824 1.673-5.65 4H15a3.01 3.01 0 0 1-2.224-.993 2.968 2.968 0 0 1-.761-2.316c.152-1.509 1.546-2.69 3.173-2.69h39.824A4.994 4.994 0 0 0 60 36.013V13H11.68zM4 11c-1.103 0-2-.897-2-2s.897-2 2-2 2 .897 2 2-.897 2-2 2zm42 34c2.206 0 4 1.794 4 4s-1.794 4-4 4-4-1.794-4-4 1.794-4 4-4zm-23 0c2.206 0 4 1.794 4 4s-1.794 4-4 4-4-1.794-4-4 1.794-4 4-4zm35-8.987A2.99 2.99 0 0 1 55.013 39H16.821l-4.77-24H58v21.013z"
            />
          </svg>
          <span class="cart__items-count" v-if="cart.length">
            {{ cart.length }}
          </span>
        </button>
      </div>
    </div>
  </header>
  <main>
    <div class="container card">
      <draggable
        v-model="items"
        group="people"
        @start="drag = true"
        @end="drag = false"
        item-key="items.id"
        class="cards"
      >
        <template #item="{ element }">
          <div>
            <div class="cards__item c-item">
              <img
                :src="require(`./img/${element.img}.jpg`)"
                @click="showCardModalF(element)"
                :alt="element.title"
                class="c-item__img"
              />
              <span class="c-item__title">{{ element.title }}</span>
              <span class="c-item__price">{{ element.price }} ₽</span>
            </div>
          </div>
        </template>
      </draggable>
    </div>
  </main>
  <footer>
    <div class="container">
      <div class="footer__text">Все права защищены.</div>
    </div>
  </footer>
  <teleport to="body">
    <card-o
      :item="currentItem"
      :cart="cart"
      v-if="showCardModal"
      @close="showCardModal = false"
      @addToCart="addToCart"
      @edit="edit"
    ></card-o>
    <cart-o
      v-if="showCartModal"
      @close="showCartModal = false"
      :cart="cart"
      @countPlus="countPlus"
      @countMinus="countMinus"
      @changeCount="changeCount"
      @removeItem="removeItem"
    >
    </cart-o>
  </teleport>
</template>

<script>
import items from './items.json';
import CardO from './components/Card-o';
import CartO from './components/Cart-o';
import draggable from 'vuedraggable';

export default {
  name: 'App',

  watch: {
    items: function (val) {
      console.log(val);
      localStorage.setItem('items', JSON.stringify(this.items));
    },
  },
  data() {
    return {
      items: JSON.parse(localStorage.getItem('items')) || items.items,
      showCardModal: false,
      showCartModal: false,
      currentItem: null,
      cart: [],
      drag: false,
    };
  },
  mounted() {
    for (let i = 0; i < this.items.length; i++) {
      this.items[i].availableCountItems = this.items[i].totalCountItem;
    }
  },

  components: { CardO, CartO, draggable },
  methods: {
    showCardModalF(el) {
      this.showCardModal = true;
      this.currentItem = el;
    },
    addToCart(item) {
      item.availableCountItems -= 1;
      if (this.cart.filter((el) => el.id == item.id).length) {
        item.countInCart++;
      } else {
        item.countInCart = 1;
        this.cart.push(item);
      }
      this.showCardModal = false;
    },

    countMinus(item) {
      if (item.countInCart > 1) {
        item.countInCart--;
        item.availableCountItems++;
      }
    },
    countPlus(item) {
      if (item.availableCountItems) {
        item.availableCountItems--;
        item.countInCart++;
      }
    },
    changeCount(e, item) {
      const v = e.target.value;
      const re = 1 + +v > 1;
      const re2 = item.totalCountItem - v >= 0;
      console.log(re2);
      if (re && re2) {
        item.countInCart = v;
        item.availableCountItems = item.totalCountItem - v;
      }
    },

    removeItem(item) {
      const ind = this.cart.findIndex((el) => el.id == item.id);
      this.cart.splice(ind, 1);
    },
    edit(newEl) {
      newEl.availableCountItems = +newEl.totalCountItem - +newEl.countInCart;
      console.log(newEl.countInCart);
      if (isNaN(newEl.availableCountItems)) {
        newEl.availableCountItems = +newEl.totalCountItem;
      }
      if (newEl.availableCountItems < 0) {
        newEl.countInCart = newEl.totalCountItem;
        newEl.availableCountItems = 0;
      }
      const i = this.items.findIndex((el) => el.id == newEl.id);
      const i2 = this.cart.findIndex((el) => el.id == newEl.id);
      if (i2 != -1) this.cart.splice(i2, 1, newEl);
      this.items.splice(i, 1, newEl);
      this.currentItem = newEl;
    },
  },
};
</script>

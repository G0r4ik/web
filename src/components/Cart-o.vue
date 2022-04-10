<template>
  <div class="cart" @click="$emit('close')">
    <div class="cart__inner card" @click.stop>
      <div class="cart__cross" @click="$emit('close')">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 490 490"
          style="enable-background: new 0 0 490 490"
          xml:space="preserve"
        >
          <path
            d="M11.387 490 245 255.832 478.613 490l10.826-10.826-233.63-234.178 233.63-234.185L478.613 0 245 234.161 11.387 0 .561 10.811l233.63 234.185L.561 479.174z"
          />
        </svg>
      </div>
      <div class="cart__title">Корзина</div>
      <div class="cart__item" v-for="item in cart" :key="item.id">
        <img :src="require(`@/img/${item.img}.jpg`)" alt="" class="cart__img" />
        <div class="cart__text">
          <div class="cart__id">Код: {{ item.id }}</div>
          <div class="cart__title-item">
            {{ item.title }}
          </div>
          <div class="count__price">{{ item.price * item.countInCart }} ₽</div>
        </div>
        <div class="cart__count">
          <div
            id="cart-countMinus"
            class="cart__count-btn"
            @click="$emit('countMinus', item)"
          >
            -
          </div>
          <input
            type="text"
            class="cart__count-n"
            :value="item.countInCart"
            @input="$emit('changeCount', $event, item)"
          />
          <div
            id="cart-countPlus"
            class="cart__count-btn"
            @click="$emit('countPlus', item)"
          >
            +
          </div>
        </div>
        <div class="count__remove" @click="$emit('removeItem', item)">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 30 30"
            width="20"
            height="20"
          >
            <path
              fill="none"
              stroke="#000"
              stroke-linecap="round"
              stroke-miterlimit="10"
              stroke-width="2"
              d="M6 5h18M13 4h4"
            />
            <path d="M6 8v16a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V8H6z" />
          </svg>
        </div>
      </div>
      <div
        class="cart__totalsum"
        :style="{
          textAlign: cart.length ? 'right' : 'left',
          fontSize: cart.length ? '22px' : '16px',
        }"
      >
        {{ cart.length ? totalSum + ' ₽' : 'Ничего нет' }}
      </div>
      <button v-if="cart.length" class="cart__finalyBuy">Купить</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['cart'],
  computed: {
    totalSum() {
      return this.cart.reduce((s, el) => {
        for (let i = 0; i < el.countInCart; i++) {
          s += +el.price;
        }
        return s;
      }, 0);
    },
  },
  methods: {},
};
</script>

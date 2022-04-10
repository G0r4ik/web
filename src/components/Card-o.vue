<template>
  <div class="card-popup" @click="$emit('close')">
    <div @click.stop class="card-popup__inner card">
      <div class="card-popup__cross" @click="$emit('close')">
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
      <div class="card-popup__edit" @click="showEdit = true">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 30 30"
          width="20"
          height="20"
        >
          <path
            d="M22.828 3c-.512 0-1.023.195-1.414.586L19 6l5 5 2.414-2.414a2 2 0 0 0 0-2.828l-2.172-2.172A1.994 1.994 0 0 0 22.828 3zM17 8 5.26 19.74s.918-.082 1.26.26c.342.342.06 2.58.48 3 .42.42 2.644.124 2.963.443.319.32.297 1.297.297 1.297L22 13l-5-5zM4 23l-.943 2.672A1 1 0 0 0 3 26a1 1 0 0 0 1 1 1 1 0 0 0 .328-.057 1 1 0 0 0 .01-.004l.025-.007a1 1 0 0 0 .006-.004L7 26l-1.5-1.5L4 23z"
          />
        </svg>
      </div>
      <div class="card-popup__content">
        <img
          :src="require(`@/img/${item.img}-large.jpg`)"
          class="card-popup__img"
          alt=""
        />
        <div class="card-popup__text">
          <span class="card-popup__title">{{ item.title }}</span>
          <span class="card-popup__price">{{ item.price }} ₽</span>
          <span class="card-popup__date">{{ item.date }} </span>
          <button
            :disabled="cart.find((el) => el.id == item.id)"
            class="card-popup__buy"
            @click="$emit('addToCart', item)"
          >
            {{
              cart.find((el) => el.id == item.id)
                ? 'Уже в коризне'
                : 'В корзину'
            }}
          </button>
        </div>
      </div>
      <div class="card-popup__description">
        {{ item.description }}
      </div>
    </div>
  </div>
  <teleport to="body"> </teleport>
  <edit-o
    v-if="showEdit"
    @close="showEdit = false"
    :item="item"
    @edit="$emit('edit', $event)"
    @edit2="showEdit = false"
  ></edit-o>
</template>

<script>
import EditO from './Edit-o';

export default {
  data() {
    return {
      showEdit: false,
    };
  },

  components: { EditO },
  props: {
    item: {
      type: Object,
      required: true,
    },
    cart: {
      type: Object,
      required: true,
    },
  },
  emits: ['edit', 'close', 'addToCart'],
};
</script>

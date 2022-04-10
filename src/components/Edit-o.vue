<template>
  <div class="edit" @click="$emit('close')">
    <div @click.stop class="edit__inner card">
      <div class="edit__cross" @click="$emit('close')">
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
      <span class="edit__title">Редактирование товара</span>
      <label class="edit__label" for="edit__title-item">
        Название товара:
        <input
          type="text"
          name=""
          class="edit__input"
          id="edit__title-item"
          v-model.lazy.trim="item_.title"
        />
      </label>
      <label class="edit__label" for="edit__price">
        Цена товара:
        <input
          type="text"
          name=""
          class="edit__input"
          id="edit__price"
          v-model.lazy.trim="item_.price"
        />
      </label>
      <label class="edit__label" for="edit__count">
        Количество товара на складе:
        <input
          type="text"
          name=""
          class="edit__input"
          id="edit__count"
          v-model.lazy.trim="item_.totalCountItem"
        />
      </label>
      <label class="edit__label" for="edit__AvCount">
        Доступный товар на складе:
        <input
          disabled
          type="text"
          name=""
          class="edit__input"
          id="edit__AvCount"
          v-model.lazy.trim="item_.availableCountItems"
        />
      </label>
      <label class="edit__label" for="edit__cartCount">
        Товар в корзине:
        <input
          disabled
          type="text"
          name=""
          class="edit__input"
          id="edit__cartCount"
          :value="+item_.countInCart ? +item_.countInCart : 0"
        />
      </label>
      <label class="edit__label" for="edit__img">
        Картинка товара:
        <input
          type="text"
          name=""
          class="edit__input"
          id="edit__img"
          v-model.lazy.trim="item_.img"
        />
      </label>
      <label class="edit__label edit__label_last" for="edit__desc">
        Описание товара:
      </label>
      <textarea
        type="text"
        name=""
        class="edit__textarea"
        id="edit__desc"
        v-model.lazy.trim="item_.description"
      >
      </textarea>
      <button
        class="edit__btn"
        @click="$emit('edit', item_), $emit('edit2', item_)"
      >
        Сохранить
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      item_: { ...this.item },
    };
  },
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
};
</script>

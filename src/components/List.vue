<template>
  <div class="list">
    <div class="list-filters">
      <button
        @click="filterArray('id', position)"
        :class="{ active: active_id }"
      >
        ID
      </button>
      <button
        @click="filterArray('name', position)"
        :class="{ active: active_name }"
      >
        Имя
      </button>
    </div>
    <div class="empty" v-if="list.length === 0">Список пуст</div>
    <ul class="list-ul">
      <li class="list-li" v-for="(item, idx) in list" :key="idx">
        <label class="list-item faculty-item">
          <input
            type="checkbox"
            name="facultie"
            class="faculty-item_checkbox"
            :checked="position === 'right' ? true : false"
            @click.prevent="pushToNextArray(item, idx, position)"
          />
          <span class="faculty-item_id">{{ item.id }}</span>
          <span class="faculty-item_name">{{ item.data.name }}</span>
        </label>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "List",
  props: {
    list: {
      type: Array,
      default: new Array(),
    },
    position: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      active_id: true,
      active_name: false,
    };
  },
  methods: {
    /**
     * Функция передает элемент родителю списка
     * для добовления в новый массив
     * @param item элемент который передаем родителю
     * @param idx id элементa который передаем родителю
     * @param position позиция самого списка (место расположение)
     */
    pushToNextArray(item, idx, position) {
      let itemForPush = { item, position, idx };
      this.$emit("pushToNewArray", itemForPush);
    },
    /**
     * Функция передает родителю списка
     * для фильтрации в указанном массиве
     * @param filterStyle Стиль фильтрации принемает текстовое значение
     * @param position позиция самого списка (место расположение)
     */
    filterArray(filterStyle, position) {
      let filterArrayOption = { filterStyle, position };
      this.$emit("filterArray", filterArrayOption);
      switch (position) {
        case "laft":
          this.btnFilterActivStyle(filterStyle);
          break;
        case "right":
          this.btnFilterActivStyle(filterStyle);
          break;
      }
    },
    /**
     * Добовление активного класса для кнопок фильтрации
     * @param filterStyle указывает на какую кнопку ведаеться класс
     */
    btnFilterActivStyle(filterStyle) {
      switch (filterStyle) {
        case "id":
          this.active_id = true;
          this.active_name = false;
          break;
        case "name":
          this.active_id = false;
          this.active_name = true;
          break;
      }
    },
  },
};
</script>

<style lang="scss">
.empty {
  text-align: center;
  margin-top: 5rem;
  font-weight: bold;
  font-size: 2rem;
}
.list {
  width: 100%;
  margin: 1rem;
  &-filters {
    button {
      margin-left: 5rem;
      padding: 1rem 1.5rem;
      background: rgb(153, 240, 232);
      border: 0.1rem solid rgb(156, 212, 207);
      font-weight: bold;
      &:active {
        transform: scale(0.95);
      }
      &:hover {
        background: rgb(227, 231, 173);
      }
      &.active {
        background: rgb(118, 199, 151);
        &:hover {
          background: rgb(227, 231, 173);
        }
      }
      &:last-child {
        margin-left: 20rem;
      }
    }
  }
}
.faculty-item {
  width: 100%;
  display: inline-flex;
  padding: 0.5rem;
  border-top: 0.1rem solid #ccc;
  &:hover {
    background: rgba(230, 230, 230, 0.568);
  }
  &_id,
  &_name {
    margin-left: 4rem;
  }
  &_id {
    width: 5rem;
  }
}
</style>
<template>
  <div id="app">
    <List
      :list="facultyLaft"
      :position="laft"
      @pushToNewArray="pushToNewArray"
      @filterArray="sortFilterArray"
    />
    <List
      :list="facultyRight"
      :position="right"
      @pushToNewArray="pushToNewArray"
      @filterArray="sortFilterArray"
    />
  </div>
</template>

<script>
import List from "./components/List.vue";
import FullArray from "./Faculties/faculties";
export default {
  name: "App",
  components: {
    List,
  },
  data() {
    return {
      faculty: [],
      facultyLaft: [],
      facultyRight: [],
      laft: "laft",
      right: "right",
      startSortArray: "",
    };
  },
  methods: {
    /**
     * @see methods Получение всего массива и передача его в массив слева.
     */
    getFaculty() {
      this.faculty = FullArray;
      this.facultyLaft = this.faculty;
    },

    /**
     * Добовление одного элемента в выбранный массив
     * @param item объект который содержит в себе данные для переноса
     * @param item.position назначает место перемещения
     * @param item.idx указывает на index в исходном массиве для удоления элемента
     * @param item.item сам элемент в виде полного объекта полученного из списка
     * @function sortArray принемает в себя массив и стиль сортеровки
     */
    pushToNewArray({ item, position, idx }) {
      switch (position) {
        case "laft":
          this.facultyRight.push(item);
          this.facultyLaft.splice(idx, 1);
          this.sortArray(this.facultyRight, this.startSortArray);
          break;
        case "right":
          this.facultyLaft.push(item);
          this.facultyRight.splice(idx, 1);
          this.sortArray(this.facultyLaft, this.startSortArray);
          break;
      }
    },

    /**
     * Функция сортеровки массивов
     * @param array принемает в себя сортеруемый массив
     * @param style принемает в себя стиль сортеровки
     */
    sortArray(array, style) {
      switch (style) {
        case "id":
          array.sort((a, b) => {
            if (a.data.id < b.data.id) {
              return -1;
            }
            if (a.data.id > b.data.id) {
              return 1;
            }
            return 0;
          });
          break;
        case "name":
          array.sort((a, b) => a.data.name.localeCompare(b.data.name));
          break;
      }
    },

    /**
     * Функция фильтрации сортеровки массивов
     * @param array принемает в себя сортеруемый массив
     * @param position принемает в себя местоположение списка
     * @param filterStyle принемает в себя стиль сортеровки
     */
    sortFilterArray({ position, filterStyle }) {
      switch (position) {
        case "laft":
          this.sortArray(this.facultyLaft, filterStyle);
          this.startSortArray = filterStyle;
          break;
        case "right":
          this.sortArray(this.facultyRight, filterStyle);
          this.startSortArray = filterStyle;
      }
    },
  },

  created() {
    this.getFaculty();
  },
};
</script>

<style lang="scss">
*,
*::after,
*::before {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
  transition: all 0.3s ease;
}
html {
  font-size: 10px;
}
body {
  font-size: 16px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  justify-content: space-between;
}
ul {
  list-style: none;
}
</style>

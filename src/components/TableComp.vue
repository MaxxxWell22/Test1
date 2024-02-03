<template>
  <div>
    <v-table class="table">
      <thead>
        <tr>
          <th class="header" v-for="(title, index) in titles" :key="title">
            <ArrowSort :titles="title" @click="sortData(eng[index])" :toggleTheme="toggleTheme"
              :activeTitle="activeTitle" />
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in cloneData" :key="item.id">
          <td v-for="(title, index) in titles" :key="title">{{ item[eng[index]] }}</td>
        </tr>
      </tbody>
    </v-table>
  </div>
</template>
<script setup>
import { ref, defineProps, onMounted } from 'vue';
import ArrowSort from './ArrowSort.vue';

const props = defineProps({
  data: {
    type: Array,
    required: true,
  },
});

const titles = ['Название ЖК', 'Тип', 'Квадратура', 'Этаж', 'Номер Квартиры', 'Дата Сдачи'];
const eng = ['developer', 'type', 'square', 'floor', 'number', 'deadline'];

const sortOrder = ref(1);
const sortTypeNumber = ref(0);
const cloneData = ref(null);
const valueRef = ref(0);
const devValue = ref(0);

const toggleTheme = ref(false);
const activeTitle = ref('');

// Сортирует названия по Алфавиту(Только от A до Z)
const sortDevelopers = () => {
  // Если надо сортировать от A до Z и наоборот от Z до A

  // if (devValue.value === 0) {
  //   cloneData.value.sort((a, b) => {
  //     const nameA = a.developer.toLowerCase();
  //     const nameB = b.developer.toLowerCase();
  //     if (nameA < nameB) return -1;
  //     if (nameA > nameB) return 1;
  //   })
  //   devValue.value = 1
  // } else if (devValue.value === 1) {
  //   cloneData.value.sort((a, b) => {
  //     const nameA = a.developer.toLowerCase();
  //     const nameB = b.developer.toLowerCase();
  //     if (nameA > nameB) return -1 * sortOrder.value;
  //     if (nameA < nameB) return 1 * sortOrder.value;
  //   });
  //   sortOrder.value *= -1;
  //   devValue.value = 0
  // }
  // return cloneData.value

  // Сортировка только от A до Z
  cloneData.value.sort((a, b) => {
    const nameA = a.developer.toLowerCase();
    const nameB = b.developer.toLowerCase();
    if (nameA > nameB) return -1 * sortOrder.value;
    if (nameA < nameB) return 1 * sortOrder.value;
  });
  sortOrder.value *= -1;
  devValue.value = 0
  return cloneData.value
}

// Сортирует кол-во комнат по возрастанию/убыванию
const sortTypes = () => {
  if (sortTypeNumber.value === 0) {
    cloneData.value.sort((a, b) => {
      const typeA = convertTypeNumber(a.type);
      const typeB = convertTypeNumber(b.type);
      return typeA - typeB;
    });
    sortTypeNumber.value = 1;
  } else if (sortTypeNumber.value === 1) {
    cloneData.value.sort((a, b) => {
      const typeA = convertTypeNumber(a.type);
      const typeB = convertTypeNumber(b.type);
      return (typeA - typeB) * sortOrder.value;
    });
    sortOrder.value *= -1;
    sortTypeNumber.value = 0;
  }

  return cloneData.value
};

// Преобразует значение кол-во комнат к числу
const convertTypeNumber = (type) => {
  switch (type) {
    case '2-к':
      return 2;
    case '3-к':
      return 3;
    case 'Студия':
      return 1;
    default:
      return 0;
  }
};

// Сортирует площадь по возрастанию/убыванию
const sortSquareAndNumber = (key) => {
  const value = valueRef.value;
  if (value === 0) {
    cloneData.value.sort((a, b) => a[key] - b[key]);
    valueRef.value = 1;
  } else if (value === 1) {
    cloneData.value.sort((a, b) => (a[key] - b[key]) * sortOrder.value);
    sortOrder.value *= -1;
    valueRef.value = 0;
  }
};

// Функция сортировки 
const sortData = (key) => {
  toggleTheme.value = !toggleTheme.value
  let cloneKey = key;
  if (cloneKey === 'developer') {
    cloneKey = 'Название ЖК'
  } else if (cloneKey === 'type') {
    cloneKey = 'Тип'
  } else if (cloneKey === 'square') {
    cloneKey = 'Квадратура'
  } else if (cloneKey === 'number') {
    cloneKey = 'Номер Квартиры'
  } else if (cloneKey === 'floor') {
    cloneKey = 'Этаж'
  } else if (cloneKey === 'deadline') {
    cloneKey = 'Дата Сдачи'
  }
  activeTitle.value = cloneKey;
  if (key === 'developer') {
    sortDevelopers();
  } else if (key === 'type') {
    sortTypes();
  } else if (key === 'square' || key === 'number' || key === 'floor') {
    sortSquareAndNumber(key);
  }
};

onMounted(() => {
  cloneData.value = JSON.parse(JSON.stringify(props.data));
})

</script>

<style lang="scss" scoped>
.table {
  max-height: 600px;
  max-width: 1500px;
  width: 1300px;

  thead {
    position: sticky;
    top: 0;
  }

  .header {
    background-color: #0D9276;
    &:hover {
      cursor: pointer;
    }
  }
}
</style>
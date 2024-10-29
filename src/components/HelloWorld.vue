<template>
  <div class="hello">
    <div>
      <!-- <div style="color: red" v-if="validationError">
        Please fill in all fields
      </div> -->
      <select v-model="data.selectedCategory">
        <option value="">Select Category</option>
        <option v-for="(category, index) in categoryLists" :key="index">
          {{ category }}
        </option>
      </select>
      <span v-if="!data.selectedCategory" style="color: red"
        >Please Select Category</span
      >
      <br /><br />
      <div>
        <div v-for="(item, index) in data.list" :key="index">
          <input
            @blur="validateRow(index)"
            placeholder="Enter name"
            v-model="item.name"
          />
          <span style="color: red">{{ item.nameError }}</span>
          &nbsp;
          <input
            @blur="validateRow(index)"
            placeholder="Enter price"
            v-model="item.price"
            type="number"
          />
          <span style="color: red">{{ item.priceError }}</span>
          &nbsp;
          <button v-if="index !== 0" @click="deleteData(index)">Delete</button>
        </div>
      </div>
      <br />
      <button @click="addData">Add more</button>
    </div>
    <br />
    <button @click="submitForm">Submit</button>
  </div>
</template>

<script>
import { ref, reactive } from "vue";

export default {
  setup() {
    const categoryLists = ref(["mobile", "laptop", "pen", "switch"]);
    const validationError = ref(false);
    const data = reactive({
      list: [
        { name: "", price: "", category: "", nameError: "", priceError: "" },
      ],
      selectedCategory: "",
    });

    const addData = () => {
      data.list.push({
        name: "",
        price: "",
        category: "",
        nameError: "",
        priceError: "",
      });
    };

    const deleteData = (index) => {
      data.list.splice(index, 1);
    };

    const validateRow = (index) => {
      let row = data.list[index];
      row.nameError = row.name ? "" : "Name is required";
      row.priceError = row.price ? "" : "Price is required";

      if (row.price && !isNaN(row.price)) {
        const decimalPart = row.price.toString().split(".")[1];
        if (decimalPart && decimalPart.length > 2) {
          row.priceError = "Only 2 decimal places allowed";
        }
      }
      validationError.value = data.list.some(
        (item) => item.nameError || item.priceError
      );
    };

    const submitForm = () => {
      validationError.value = data.list.some(
        (item) => !item.name || !item.price || !data.selectedCategory
      );

      if (!validationError.value) {
        const groupedData = {
          [data.selectedCategory]: data.list,
        };
        console.log(groupedData);
      }
    };

    return {
      categoryLists,
      data,
      addData,
      deleteData,
      validateRow,
      validationError,
      submitForm,
    };
  },
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

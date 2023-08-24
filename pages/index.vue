<template>
  <div
    class="flex min-h-full flex-1 flex-col justify-center px-6 py-12 lg:px-8"
  >
    <div class="sm:mx-auto sm:w-full sm:max-w-sm">
      <h2 class="text-base font-semibold leading-7 text-gray-900">Quiz #4</h2>
      <p class="mt-1 text-sm leading-6 text-gray-600">
        ให้เขียนโปรแกรมคิดเงินทอน <br />
        โดยจะสามารถแสดงธนบัตรหรือเหรียญได้ตามจำนวณนี้ 500, 100, 50, 10, 5, 1
        โดยจำนวณที่กล่าวมาข้างต้นจะเป็นข้อมูลในรูปแบบ array เท่านั้น
        และจะต้องมีช่อง input เงินที่รับมา และช่อง input ราคาสินค้า เช่น
        <br /><br />

        ราคาสินค้า 571<br />
        รับเงินมา 1000 บาท<br />
        จะต้องทอน 100 = 4 ใบ<br />
        10 = 2 เหรียญ 5 = 1 เหรียญ 1 = 4 เหรียญ<br />
      </p>
    </div>

    <div class="mt-10 sm:mx-auto sm:w-full sm:max-w-sm">
      <div>
        <label
          for="productPrice"
          class="block text-sm font-medium leading-6 text-gray-900"
          >ราคาสินค้า</label
        >
        <div class="mt-2">
          <input
            id="productPrice"
            type="number"
            v-model="productPrice"
            required=""
            class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          />
        </div>
      </div>

      <div>
        <label
          for="amountPaid"
          class="block text-sm font-medium leading-6 text-gray-900"
          >รับเงินมา</label
        >
        <div class="mt-2">
          <input
            id="amountPaid"
            type="number"
            v-model="amountPaid"
            required=""
            class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          />
        </div>
      </div>

      <div class="mt-2">
        <button
          type="submit"
          class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
          @click="calculateChange()"
        >
          คำนวณ
        </button>
      </div>

      <div v-if="changeCalculated">
        <h2>จะต้องทอน: {{ amountPaid - productPrice }} บาท</h2>
        <ul>
          <li v-for="(count, denomination) in changeMoney" :key="denomination">
            {{ denomination }} = {{ count }}
            {{ denomination.split(" ")[0] }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const productPrice = ref(0);
const amountPaid = ref(0);
let changeCalculated = ref(false);
let changeMoney = ref();

const calculateChange = () => {
  const amount = amountPaid.value;
  const paid = productPrice.value;
  let change = amount - paid;

  const denominations = [500, 100, 50, 10, 5, 1];
  const notesList = {};

  if (change >= 0) {
    for (const denomination of denominations) {
    //   console.log(true ? change >= denomination : false);
      if (change >= denomination) {
        const count = Math.floor(change / denomination);
        notesList[
          denomination > 10 ? `แบงค์ ${denomination}` : `เหรียญ ${denomination}`
        ] = count;
        change -= count * denomination;
      }
    }

    changeMoney.value = notesList;
    changeCalculated.value = true;
  } else {
    alert("จำนวนเงินไม่เพียงพอ");
  }
};
</script>

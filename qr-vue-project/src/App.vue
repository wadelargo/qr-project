<script setup>
  import { ref } from 'vue'
  import { useQRCode } from '@vueuse/integrations/useQRCode'

  const prod = ref([])

  const qrcodes = ref({})

  fetch('https://dummyjson.com/products')
    .then(res => res.json())
    .then(data => {
      prod.value = data.products
      data.products.forEach(product => {
        qrcodes.value[product.id] = useQRCode(String(product.id))
      })
    })
</script>

<template>
  <div id="products">
    <div class="product" v-for="p in prod" :key="p.id">
      <img :src="p.thumbnail" alt="" class="pic">
      <div class="details">
        <h2>{{ p.title }}</h2>
        <h3>{{ p.price }}</h3>
      </div>
      <div class="qrcode"><img :src="qrcodes[p.id]" alt="" /></div>
    </div>
  </div>
</template>

<style scoped>

#products {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  padding: 24px;
  gap: 24px;
}

.product {
  display: flex;
  gap: 4px;
  padding: 12px;
  border: 1px solid #333;
}

.pic {
  width: 150px;
  height: 150px;
  object-fit: cover;
}

.details {
  flex: 1;
}

.qrcode {
  width: 150px;
  height: 150px;
  border: 1px solid #999;
}

</style>
dummyjson.com
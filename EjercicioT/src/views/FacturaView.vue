<template>
    <div class="container">
      <h1>Calculadora de Total con Descuento</h1>
      <div v-for="(item, index) in items" :key="index" lass="invoice-header">
        <input v-model="item.name" placeholder="Nombre del artículo" />
        <input v-model.number="item.quantity" placeholder="Cantidad" />
        <input v-model.number="item.price" placeholder="Precio unitario" />
      </div>
      <button @click="addItem">Agregar Producto</button>
      <button @click="calculateTotal">Calcular Total</button>
      <button @click="clearList">Limpiar Lista</button>
      <div v-if="items.length > 0" class="invoice-item">
        <h2>Productos Agregados:</h2>
        <ul>
          <li v-for="(item, index) in items" :key="index">
            {{ item.name }} - Cantidad: {{ item.quantity }} - Precio: ${{ item.price }}
          </li>
        </ul>
      </div>
      <div v-if="total > 0">
        <p>Cantidad de productos: ${{ totalProducts }}</p>
        <p>Total a pagar: ${{ total }}</p>
        <p>Descuento aplicado: {{ discount }}%</p>
      </div>
    </div>
  </template>
 
  
  <script>
  export default {
    data() {
      return {
        items: [],
        total: 0,
        discount: 0,
        newItem: {
          name: "",
          quantity: 0,
          price: 0,
        },
      };
    },
    methods: {
      addItem() {
        this.items.push({ ...this.newItem });
        this.newItem = { name: "", quantity: 0, price: 0 };
      },
      calculateTotal() {
        const totalQuantity = this.items.reduce((acc, item) => acc + item.quantity, 0);
        const totalPrice = this.items.reduce((acc, item) => acc + item.quantity * item.price, 0);
  
        let maxDiscount = 0;
  
        if (totalQuantity >= 6) {
          maxDiscount = Math.max(maxDiscount, 10);
        }
        if (totalQuantity >= 12) {
          maxDiscount = Math.max(maxDiscount, 20);
        }
        if (totalPrice >= 60000) {
          maxDiscount = Math.max(maxDiscount, 5);
        }
        if (totalPrice >= 120000) {
          maxDiscount = Math.max(maxDiscount, 10);
        }
        if (totalPrice >= 240000) {
          maxDiscount = Math.max(maxDiscount, 15);
        }
        
        this.totalProducts = totalQuantity;
        this.discount = maxDiscount;
        this.total = totalPrice - (totalPrice * (maxDiscount / 100));
      },
      clearList() {
        this.items = [];
        this.total = 0;
        this.discount = 0;
      },
    },
  };
  </script>
  
  <style scoped>
  /* Estilos CSS si los necesitas */
  .container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #5e5c5c;
  border-radius: 5px;
  background-color: #796e6e;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.invoice {
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 5px;
  background-color: #f9f9f9;
  margin-top: 20px;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

input {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  display: block;
  width: 100%;
  padding: 10px;
  background-color: #007BFF;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 5px;
}

p {
  font-size: 18px;
  font-weight: bold;
  margin-top: 10px;
}

/* Estilos para la factura */
.invoice-header {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #ccc;
  padding-bottom: 10px;
  margin-bottom: 10px;
}

.invoice-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 5px;
}

  </style>
  
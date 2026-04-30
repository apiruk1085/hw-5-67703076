<script setup>
import { reactive, ref } from 'vue'

const formData = reactive({
  prodId: '',
  prodName: '',
  prodQty: '',
  prodPrice: ''
})

const isSubmitting = ref(false)

const submitForm = async () => {
  isSubmitting.value = true
  
  const data = {
    "รหัสสินค้า": formData.prodId,
    "ชื่อสินค้า": formData.prodName,
    "จำนวน": formData.prodQty,
    "ราคา": formData.prodPrice
  }

  const webhookUrl = 'http://localhost:5678/webhook-test/6f87075f-16d0-4c4f-aaac-3eda4bb3e42f'

  try {
    await fetch(webhookUrl, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    })
    alert('ส่งข้อมูลเข้า n8n เรียบร้อยแล้ว!')
    // ล้างฟอร์มหลังจากส่งข้อมูลสำเร็จ
    formData.prodId = ''
    formData.prodName = ''
    formData.prodQty = ''
    formData.prodPrice = ''
  } catch (error) {
    console.error('Error:', error)
    alert('เกิดข้อผิดพลาดในการส่งข้อมูล')
  } finally {
    isSubmitting.value = false
  }
}
</script>

<template>
  <div class="app-container">
    <div class="form-wrapper">
      <h2 class="form-title">แบบฟอร์มบันทึกข้อมูลสินค้า</h2>
      <form @submit.prevent="submitForm" class="product-form">
        <div class="input-group">
          <label>รหัสสินค้า:</label>
          <input type="text" v-model="formData.prodId" placeholder="เช่น P001" required>
        </div>
        
        <div class="input-group">
          <label>ชื่อสินค้า:</label>
          <input type="text" v-model="formData.prodName" placeholder="กรอกชื่อสินค้า" required>
        </div>
        
        <div class="input-group">
          <label>จำนวน (ชิ้น):</label>
          <input type="number" v-model="formData.prodQty" placeholder="0" required min="1">
        </div>
        
        <div class="input-group">
          <label>ราคา (บาท):</label>
          <input type="number" v-model="formData.prodPrice" placeholder="0.00" required min="0" step="0.01">
        </div>
        
        <button type="submit" class="submit-btn" :disabled="isSubmitting">
          {{ isSubmitting ? 'กำลังบันทึก...' : 'บันทึกข้อมูลสินค้า' }}
        </button>
      </form>
    </div>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;500;600&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Prompt', sans-serif;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.app-container {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.form-wrapper {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  padding: 40px;
  border-radius: 20px;
  box-shadow: 0 15px 35px rgba(0,0,0,0.1), 0 5px 15px rgba(0,0,0,0.05);
  width: 100%;
  max-width: 450px;
  border: 1px solid rgba(255, 255, 255, 0.5);
  transition: transform 0.3s ease;
}

.form-wrapper:hover {
  transform: translateY(-5px);
}

.form-title {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 30px;
  font-weight: 600;
  font-size: 1.5rem;
}

.product-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.input-group label {
  font-size: 0.9rem;
  color: #4a5568;
  font-weight: 500;
}

.input-group input {
  padding: 12px 16px;
  border: 2px solid #e2e8f0;
  border-radius: 10px;
  font-size: 1rem;
  font-family: inherit;
  transition: all 0.3s ease;
  background: #f8fafc;
}

.input-group input:focus {
  outline: none;
  border-color: #c48c46;
  background: #fff;
  box-shadow: 0 0 0 3px rgba(196, 140, 70, 0.2);
}

.submit-btn {
  background: linear-gradient(135deg, #d4a373 0%, #c48c46 100%);
  color: white;
  padding: 14px;
  border: none;
  border-radius: 10px;
  font-size: 1.1rem;
  font-weight: 500;
  font-family: inherit;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 10px;
  box-shadow: 0 4px 15px rgba(196, 140, 70, 0.3);
}

.submit-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(196, 140, 70, 0.4);
}

.submit-btn:active {
  transform: translateY(1px);
}

.submit-btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
  transform: none;
}
</style>

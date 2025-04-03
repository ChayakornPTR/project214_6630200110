<template>
  <div class="page" id="page4">
    <h1>แก้ไขรายละเอียดการเรียน</h1>
    <p>กรุณากรอกข้อมูลวิชาที่ต้องการแก้ไข</p>
    <div class="form-container">
      <div class="form-group">
        <label for="courseIndex">ลำดับวิชา (1-6)</label>
        <input v-model.number="form.index" type="number" class="form-control" id="courseIndex" min="1" max="6" placeholder="กรอกลำดับวิชาที่ต้องการแก้ไข">
      </div>
      <div class="form-group">
        <label for="courseCode">รหัสวิชา</label>
        <input v-model="form.code" type="text" class="form-control" id="courseCode" placeholder="โปรดกรอกข้อมูล">
      </div>
      <div class="form-group">
        <label for="courseName">ชื่อวิชา</label>
        <input v-model="form.name" type="text" class="form-control" id="courseName" placeholder="โปรดกรอกข้อมูล">
      </div>
      <div class="form-group">
        <label for="credits">หน่วยกิต</label>
        <input v-model.number="form.credits" type="text" class="form-control" id="credits" placeholder="โปรดกรอกข้อมูล">
      </div>
      <div class="form-group">
        <label for="grade">เกรด</label>
        <input v-model="form.grade" type="text" class="form-control" id="grade" placeholder="โปรดกรอกข้อมูล">
      </div>
      <button type="button" class="submit-btn" @click="save">บันทึกข้อมูลการเรียน</button>
    </div>
    <a href="#" class="button" @click.prevent="$emit('navigate', 3)" style="margin-top: 1.5rem;">ย้อนกลับ</a>
  </div>
</template>

<script>
export default {
  name: 'Page4',
  data() {
    return {
      form: {
        index: '',
        code: '',
        name: '',
        credits: '',
        grade: ''
      }
    };
  },
  methods: {
    save() {
      if (Object.values(this.form).some(val => !val)) {
        alert('กรุณากรอกข้อมูลการเรียนให้ครบทุกช่อง');
        return;
      }
      if (this.form.index < 1 || this.form.index > 6) {
        alert('กรุณากรอกลำดับวิชาระหว่าง 1-6');
        return;
      }
      this.$emit('save', { ...this.form });
      this.form = { index: '', code: '', name: '', credits: '', grade: '' };
    }
  }
};
</script>
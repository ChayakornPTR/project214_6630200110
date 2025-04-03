<template>
  <div class="page" id="page2">
    <h1>แก้ไขข้อมูลส่วนตัว</h1>
    <p>กรุณากรอกข้อมูลที่ต้องการแก้ไข</p>
    <div class="form-container">
      <div class="form-group">
        <label for="fullName">ชื่อ-นามสกุล</label>
        <input v-model="form.fullName" type="text" class="form-control" id="fullName" placeholder="โปรดกรอกข้อมูล">
      </div>
      <div class="form-group">
        <label for="studentId">รหัสนิสิต</label>
        <input v-model="form.studentId" type="text" class="form-control" id="studentId" placeholder="โปรดกรอกข้อมูล">
      </div>
      <div class="form-group">
        <label for="faculty">คณะ</label>
        <input v-model="form.faculty" type="text" class="form-control" id="faculty" placeholder="โปรดกรอกข้อมูล">
      </div>
      <div class="form-group">
        <label for="major">สาขาวิชา</label>
        <input v-model="form.major" type="text" class="form-control" id="major" placeholder="โปรดกรอกข้อมูล">
      </div>
      <div class="form-group">
        <label for="school">โรงเรียนที่เรียนจบมา</label>
        <input v-model="form.school" type="text" class="form-control" id="school" placeholder="โปรดกรอกข้อมูล">
      </div>
      <button type="button" class="submit-btn" @click="save">บันทึกข้อมูลส่วนตัว</button>
    </div>
    <a href="#" class="button" @click.prevent="$emit('navigate', 3)" style="margin-top: 1.5rem;">ย้อนกลับ</a>
  </div>
</template>

<script>
export default {
  name: 'Page2',
  data() {
    return {
      form: {
        fullName: '',
        studentId: '',
        faculty: '',
        major: '',
        school: ''
      }
    };
  },
  methods: {
    save() {
      if (Object.values(this.form).some(val => !val)) {
        alert('กรุณากรอกข้อมูลส่วนตัวให้ครบทุกช่อง');
        return;
      }
      this.$emit('save', { ...this.form });
      this.form = { fullName: '', studentId: '', faculty: '', major: '', school: '' };
    }
  }
};
</script>
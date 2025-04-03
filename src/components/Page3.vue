<template>
  <div class="page" id="page3">
    <h1>Profile</h1>
    <p>ข้อมูลส่วนตัวและข้อมูลผลการเรียน</p>
    <div class="content-wrapper">
      <div class="profile-container">
        <div class="image-container">
          <img src="/FNpic1.jpg" alt="Profile Image 1">
          <img src="/FNpic2.jpg" alt="Profile Image 2">
        </div>
        <div class="profile-data">
          <div class="profile-data-item"><b>ชื่อ-นามสกุล:</b> {{ profile.fullName }}</div>
          <div class="profile-data-item"><b>รหัสนิสิต:</b> {{ profile.studentId }}</div>
          <div class="profile-data-item"><b>คณะ:</b> {{ profile.faculty }}</div>
          <div class="profile-data-item"><b>สาขาวิชา:</b> {{ profile.major }}</div>
          <div class="profile-data-item"><b>โรงเรียนเดิม:</b> {{ profile.school }}</div>
        </div>
      </div>
      <div class="course-container">
        <table class="course-table">
          <thead>
            <tr>
              <th>รหัสวิชา</th>
              <th>ชื่อวิชา</th>
              <th>หน่วยกิต</th>
              <th>เกรด</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="course in courses" :key="course.id">
              <td>{{ course.code }}</td>
              <td>{{ course.name }}</td>
              <td>{{ course.credits }}</td>
              <td>{{ course.grade }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div style="margin-top: 2rem;">
      <button class="button" @click="$emit('navigate', 2)">แก้ไขข้อมูลส่วนตัว</button>
      <button class="button" @click="$emit('navigate', 4)" style="margin-left: 1rem;">แก้ไขข้อมูลการเรียน</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Page3',
  props: {
    profile: Object,
    courses: Array
  }
};
</script>
<template>
  <div class="page-container">
    <div class="loader" :class="{ 'loader-top': !loaderVisible }"></div>
    <div class="loader" :class="{ 'loader-bottom': !loaderVisible }"></div>
    <Page1 v-if="currentPage === 1" @navigate="navigateTo" />
    <Page2 v-if="currentPage === 2" @navigate="navigateTo" @save="savePersonalData" />
    <Page3 v-if="currentPage === 3" @navigate="navigateTo" :profile="profile" :courses="courses" />
    <Page4 v-if="currentPage === 4" @navigate="navigateTo" @save="saveCourseData" />
  </div>
</template>

<script>
import { gsap } from 'gsap';
import Page1 from './components/Page1.vue';
import Page2 from './components/Page2.vue';
import Page3 from './components/Page3.vue';
import Page4 from './components/Page4.vue';

export default {
  name: 'App',
  components: { Page1, Page2, Page3, Page4 },
  data() {
    return {
      currentPage: 1,
      loaderVisible: true,
      profile: {},
      courses: []
    };
  },
  mounted() {
    this.fetchData();
    setTimeout(() => {
      this.loaderVisible = false;
      this.animateIn(document.getElementById('page1'));
    }, 500);
    document.addEventListener('mousemove', this.handleMouseMove);
  },
  methods: {
    async fetchData() {
      try {
        const [profileRes, coursesRes] = await Promise.all([
          fetch('http://localhost:3000/profile'),
          fetch('http://localhost:3000/courses')
        ]);
        this.profile = await profileRes.json();
        this.courses = await coursesRes.json();
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
    navigateTo(pageNum) {
      if (pageNum === this.currentPage) return;
      this.transitionToPage(pageNum);
    },
    transitionToPage(pageNum) {
      this.loaderVisible = true;
      const currentPageEl = document.getElementById('page' + this.currentPage);
      this.animateOut(currentPageEl);
      setTimeout(() => {
        this.currentPage = pageNum;
        setTimeout(() => {
          this.loaderVisible = false;
          this.animateIn(document.getElementById('page' + pageNum));
        }, 500);
      }, 500);
    },
    handleMouseMove(e) {
      const x = e.clientX / window.innerWidth;
      const y = e.clientY / window.innerHeight;
      const pages = document.querySelectorAll('.page');
      pages.forEach(page => {
        requestAnimationFrame(() => {
          const bgX = 50 + (x - 0.5) * 8;
          const bgY = 50 + (y - 0.5) * 5;
          page.style.backgroundPosition = `${bgX}% ${bgY}%`;
        });
      });
    },
    async savePersonalData(data) {
      try {
        await fetch('http://localhost:3000/profile', {
          method: 'PUT',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(data)
        });
        this.profile = data;
        this.navigateTo(3);
      } catch (error) {
        console.error('Error saving personal data:', error);
      }
    },
    async saveCourseData(data) {
      try {
        const index = data.index - 1;
        await fetch(`http://localhost:3000/courses/${index + 1}`, {
          method: 'PUT',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ id: index + 1, ...data })
        });
        this.courses[index] = { id: index + 1, ...data };
        this.navigateTo(3);
      } catch (error) {
        console.error('Error saving course data:', error);
      }
    },
    animateIn(element) {
      if (!element) return;
      gsap.set(element.querySelector('h1'), { opacity: 0, y: 20 });
      gsap.set(element.querySelector('p'), { opacity: 0, y: 20 });
      const buttons = element.querySelectorAll('.button');
      buttons.forEach(button => gsap.set(button, { opacity: 0, y: 20 }));
      const form = element.querySelector('.form-container');
      if (form) gsap.set(form, { opacity: 0, y: 20 });
      const profile = element.querySelector('.profile-container');
      if (profile) {
        gsap.set(profile, { opacity: 0, y: 20 });
        gsap.set(profile.querySelectorAll('img'), { opacity: 0, y: 20 });
      }
      const course = element.querySelector('.course-container');
      if (course) gsap.set(course, { opacity: 0, y: 20 });

      gsap.to(element.querySelector('h1'), { opacity: 1, y: 0, duration: 0.6, delay: 0.3, ease: 'power2.out' });
      gsap.to(element.querySelector('p'), { opacity: 1, y: 0, duration: 0.6, delay: 0.5, ease: 'power2.out' });
      buttons.forEach((button, index) => {
        gsap.to(button, { opacity: 1, y: 0, duration: 0.6, delay: 0.7 + index * 0.1, ease: 'power2.out' });
      });
      if (form) gsap.to(form, { opacity: 1, y: 0, duration: 0.6, delay: 0.7, ease: 'power2.out' });
      if (profile) {
        gsap.to(profile, { opacity: 1, y: 0, duration: 0.6, delay: 0.7, ease: 'power2.out' });
        gsap.to(profile.querySelectorAll('img'), { opacity: 1, y: 0, duration: 0.6, delay: 0.9, ease: 'power2.out' });
      }
      if (course) gsap.to(course, { opacity: 1, y: 0, duration: 0.6, delay: 0.9, ease: 'power2.out' });
    },
    animateOut(element) {
      if (!element) return;
      gsap.to(element.querySelector('h1'), { opacity: 0, y: -20, duration: 0.3, ease: 'power2.in' });
      gsap.to(element.querySelector('p'), { opacity: 0, y: -20, duration: 0.3, delay: 0.1, ease: 'power2.in' });
      const buttons = element.querySelectorAll('.button');
      buttons.forEach(button => gsap.to(button, { opacity: 0, y: -20, duration: 0.3, delay: 0.2, ease: 'power2.in' }));
      const form = element.querySelector('.form-container');
      if (form) gsap.to(form, { opacity: 0, y: -20, duration: 0.3, delay: 0.2, ease: 'power2.in' });
      const profile = element.querySelector('.profile-container');
      if (profile) {
        gsap.to(profile, { opacity: 0, y: -20, duration: 0.3, delay: 0.2, ease: 'power2.in' });
        gsap.to(profile.querySelectorAll('img'), { opacity: 0, y: -20, duration: 0.3, delay: 0.1, ease: 'power2.in' });
      }
      const course = element.querySelector('.course-container');
      if (course) gsap.to(course, { opacity: 0, y: -20, duration: 0.3, delay: 0.2, ease: 'power2.in' });
    }
  }
};
</script>

<style scoped>
@import './assets/style.css';
</style>
<!-- формирование списка образований -->
<template>
  <div class="px-1 pt-2">
    <label class="form-label">Образования</label>
    <div v-for="education in educations" :key="education.id">
      <hr>
      <button @click="removeEducation(education.id)">Удалить</button>
      <ResumeEducation :education="education" :vkData="vkData" />
    </div>
    <hr>
    <button class="input-group" @click="addEducation">Добавить</button>
  </div>
</template>
  
<script>
import ResumeEducation from "./ResumeEducation.vue";
const EDUCATION_TEMPLATE = { index: 0, level: '', faculty: '', specialization: '', gard_year: 0 }
export default {
  name: "ResumeEducationList",
  components: { ResumeEducation },
  props: {
    educations: {
      type: Object,
      required: true
    },
    vkData: {
      type: Object,
      required: true
    }
  },
  methods: {
    addEducation(e) {
      let newEducation = structuredClone(EDUCATION_TEMPLATE);
      if (this.educations.length > 0) {
        newEducation.id = this.educations[this.educations.length - 1].id + 1;
      }
      this.educations.push(newEducation);
    },
    removeEducation(educationId) {
      let removingIndex = this.educations.findIndex((education) =>
        education.id === educationId);
      this.educations.splice(removingIndex, 1)
    }
  },
}
</script>
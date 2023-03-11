<template>
  <div class="px-1 pt-2">
    <div>
      <ResumeSelect label="Образование" name="level" :options="educationTypes" @update="setEducationValue" />
      <div class="px-1 pt-2" v-show="education.level && education.level !== 'Среднее'">
        <ResumeInstituteInput name="institution" :vk-data="vkData" @updated="setEducationValue"
         />
        <ResumeInput type="text" label="Факультет" name="faculty" @updated="setEducationValue"
          />
        <ResumeInput type="text" label="Специализация" name="specialization" @updated="setEducationValue"
        />
        <ResumeInput type="number" label="Год окончания" name="gard_year" @updated="setEducationValue" error_message="нереалистичный год"
         />
      </div>
    </div>
  </div>
</template>
  
<script>
import ResumeSelect from "./ResumeSelect.vue";
import ResumeInput from "./ResumeInput.vue";
import ResumeInstituteInput from "./ResumeInstituteInput.vue";
export default {
  name: "ResumeEducation",
  components: { ResumeInstituteInput, ResumeInput, ResumeSelect },
  props: {
    // Данные об образовании
    education: {
      type: Object,
      required: true
    },
    // Данные, полученные из ВК
    vkData: {
      type: Object
    },
  },
  data() {
    return {
      educationTypes: ['Среднее',
        'Среднее специальное',
        'Неоконченное высшее',
        'Высшее',],
    }
  },
  methods: {
    setEducationValue(field, value) {
      this.education[field] = value;
    },
  }
}
</script>
<template>
    <div class="container">
        <div class="apple">
            <!-- форма резюме -->
            <div id="resume" class="half_life px-2 py-lg-5">
                <ResumeSelect label="Статус" name="status" :options="resumeStatuses" default="new" @update="setValue" />

                <ResumeInput label="Ссылка на фото" name="photo" type="text" @updated="setValue" />

                <ResumeInput label="ФИО" name="FIO" type="text" @updated="setValue" regex="[А-ЯA-Zа-яa-z]"
                    error_message="неверный формат имени, допускаются только русские и английские буквы" />

                <ResumeInput label="Дата рождения" name="BD" type="date" @updated="setValue" error_message="Неправдоподобная дата рождения! Вернитесь в свой гроб или садик, либо прекратите паясничать."/>
                <ResumeInputCity name="city" :vkData="vkData" @updated="setValue" />
                <ResumeInput label="О себе" name="about" type="text" @updated="setValue" />

                <ResumeInput label="Профессия" name="profession" type="text" @updated="setValue" />
                <ResumeInput label="Желаемая зарплата" name="salary" type="number" @updated="setValue" />
                <ResumeInput label="Ключевые навыки" name="skills" type="text" @updated="setValue" />

                <ResumeEducationList :educations="resume.educations" :vkData="vkData" />

                <ResumeInput label="Телефон" name="phone" type="text" @updated="setValue" regex="^[0-9]{6,11}$"
                    error_message="неверный формат телефона, допускается 6-10 чисел, без служебных символов" />
                <ResumeInput label="Email" name="email" type="text" @updated="setValue"
                    regex="^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z]+$"
                    error_message="неверный формат email, допускаются только английские буквы" />
            </div>
            <!-- вывод резюме -->
            <div class="half_life px-4 pt-5 text-bg-dark">
                <ResumeView :resume="resume" />
            </div>
        </div>
        <button>Сохранить</button>
    </div>
</template>
  
<script>
import ResumeInput from "./ResumeInput.vue";
import ResumeInputCity from "./ResumeInputCity.vue"
import ResumeEducationList from "./ResumeEducationList.vue";
import ResumeView from "./ResumeView.vue";
import ResumeSelect from "./ResumeSelect.vue";
export default {
    name: 'ResumeResult',
    components: { ResumeSelect, ResumeInput, ResumeInputCity, ResumeEducationList, ResumeView },
    data() {
        return {
            resumeStatuses: {
                new: 'Новое',
                interviewScheduled: 'Назначено собеседование',
                accepted: 'Принят',
                rejected: 'Отказ',
            },
            // Данные для резюме
            resume: {
                status: '',
                profession: '',
                city: '',
                photo: '',
                FIO: '',
                phone: '',
                email: '',
                BD: '',
                educations: [],
                salary: '',
                skills: '',
                about: '',
            },
            vkData: {
                russiaId: undefined,
                cities: [],
                universities: [],
                selectedCity: undefined
            },
        };
    },
    methods: {
        setValue(field, value) {
            this.resume[field] = value;
        },
        resetFormValue(field) {
            this.setValue(field, '');
        },
    }
}
</script>
<style>
.apple {
    display: flex;
}

.half_life {
    width: 50%;
}
</style>
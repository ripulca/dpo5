<template>
    <h3>Мое офигенное резюме(наверное)</h3>
    <div class="py-lg-2">
        <img v-show="resume.photo" width="300" :src="resume.photo">
    </div>
    <div class="words">
        <h3 v-show="resume.FIO">{{ resume.FIO }}</h3>

        <h3 v-show="resume.city || resume.BD || resume.about">О себе</h3>
        <div>
            <p v-show="resume.BD">Дата рождения: {{ resume.BD }}</p>
            <p v-show="resume.city">Город: {{ resume.city }}</p>
            <p v-show="resume.about">О себе: {{ resume.about }}</p>
        </div>

        <div v-for="education in resume.educations" :key="education.id">
            <div v-show="education.level">
                <h3>Образование</h3>
                <div>
                    <p>Уровень: {{ educationTypes[education.level] }}</p>

                    <div v-show="education.level !== 'Среднее'">
                        <p v-show="education.institution">Учебное заведение: {{ education.institution }}</p>
                        <p v-show="education.faculty">Факультет: {{ education.faculty }}</p>
                        <p v-show="education.specialization">Специализация: {{ education.specialization }}</p>
                        <p v-show="education.gard_year">Год окончания: {{ education.gard_year }}</p>
                    </div>
                </div>
            </div>
        </div>
        <div>
            <p v-show="resume.profession">Профессия: {{ resume.profession }}</p>
            <p v-show="resume.salary > 0">Желаемая заработная плата: {{ resume.salary }} ₽</p>

            <p v-show="resume.skills">Ключевые навыки: {{ resume.skills }}</p>
        </div>

        <h3 v-show="resume.phone || resume.email">Контакты</h3>
        <div>
            <p v-show="resume.phone">Телефон: {{ resume.phone }}</p>
            <p v-show="resume.email">Email: {{ resume.email }}</p>
        </div>
    </div>
</template>
  
<script>
export default {
    name: "ResumeView",
    props: {
        resume: {
            type: Object,
            required: true
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
}
</script>
<style>
.words{
    word-break: break-all;
}
</style>
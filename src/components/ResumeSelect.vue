<!-- селект с обработчиком ошибок -->
<template>
    <div class="px-1 pt-2">
        <label class="form-label">{{ label }}</label>
        <select class="input-group" v-model="inputValue" :id="'select-' + name">
            <option v-for="(value, option) in options" :value="option">{{ value }}</option>
        </select>
    </div>
</template>
  
<script>
export default {
    name: "ResumeSelect",
    props: {
        label: {
            type: String,
            required: true
        },
        name: {
            type: String,
            required: true
        },
        options: {
            type: Object,
            required: true
        },
        default: {
            type: String,
            default: ''
        }
    },
    emits: ["update"],
    data() {
        return {
            value: this.default
        }
    },
    computed: {
        inputValue: {
            get() {
                return this.value;
            },
            set(newValue) {
                this.value = newValue;
                this.$emit('update', this.name, newValue);
            }
        }
    },
}
</script>
<template>
  <div class="px-1 pt-2">
    <label class="form-label">{{ label }}</label>
    <input class="input-group" :type="type" v-model="inputValue" :id="'input-' + name" />
    <div v-if="!valid && error_message">
      <span class="error-message">{{ error_message }}</span>
    </div>
  </div>
</template>
  
<script>
export default {
  name: "ResumeInput",
  props: {
    error_message: String,
    name: {
      type: String,
      required: true,
    },
    label: {
      type: String,
      required: true,
    },
    type: {
      type: String,
      required: true,
    },
    regex: {
      type: String,
      default: ""
    }
  },
  emits: ['updated',],
  data() {
    return {
      // Значение, введенное пользователем
      value: '',
    }
  },
  computed: {
    //Обработка вводимого значения
    valid() {
      if (this.value) {
        if(this.type=='date'){
          let min=Date.parse('01.01.1900');
          let max=Date.parse('01.01.2010');
          let birthDate=Date.parse(this.value);
          if(birthDate>max || birthDate<min){
            return false;
          }
          return true;
        }
        return new RegExp(this.regex).test(this.value)
      }
      return true;
    },
    inputValue: {
      get() {
        return this.value;
      },
      set(newValue) {
        this.value = newValue;
        this.$emit('updated', this.name, newValue);
      }
    }
  },
}
</script>
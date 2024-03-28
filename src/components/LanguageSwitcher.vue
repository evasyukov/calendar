<template>
    <button @click="toggleLanguage">{{ language === 'ru' ? 'EN' : 'RU' }}</button>
   </template>
   
   <script>
   export default {
    name: 'LanguageSwitcher',
    props: {
       language: {
         type: String,
         required: true
       }
    },
    methods: {
       toggleLanguage() {
         this.$emit('language-changed', this.language === 'ru' ? 'en' : 'ru');
       }
    }
   };
   </script>
<template>
    <div v-if="contact" class="page">
      <h4>Thêm Liên hệ</h4>
      <ContactForm
        :contact="contact"
        @submit:contact="createContact"
      />
      <p>{{ message }}</p>
    </div>
  </template>
  <script>
  import ContactForm from "@/components/ContactForm.vue";
  import ContactService from "@/services/contact.service";
  export default {
    components: {
      ContactForm,
    },
    data() {
      return {
        contact: {},
        message: "",
      };
    },
    methods: {
      async createContact(data) {
        try {
          this.contact = await ContactService.create(data);
          this.message = "Thêm liên hệ thành công.";
        } catch (error) {
          console.log(error);
          // Chuyển sang trang NotFound đồng thời giữ cho URL không đổi
          this.$router.push({
            name: "notfound",
            params: {
              pathMatch: this.$route.path.split("/").slice(1),
            },
            query: this.$route.query,
            hash: this.$route.hash,
          });
        }
      },
    },
  };
  </script>
  
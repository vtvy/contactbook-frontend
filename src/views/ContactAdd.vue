<template>
    <div v-if="contact" class="page">
        <h4>Thêm Liên hệ</h4>
        <ContactForm
            :contact="contact"
            @submit:contact="onCreateContact"
            @delete:contact="onDeleteContact"
        />
        <p>{{ message }}</p>
    </div>
</template>
<script>
import ContactForm from "@/components/ContactForm.vue";
import { contactService } from "@/services/contact.service";
export default {
    components: {
        ContactForm,
    },
    data() {
        return {
            message: "",
            contact: {
                name: "",
                email: "",
                phone: "",
                favorite: 0,
            },
        };
    },
    methods: {
        async onCreateContact(contact) {
            try {
                this.contact = await contactService.create(contact);
                this.message = "Liên hệ được thêm thành công.";
            } catch (error) {
                console.log(error);
            }
        },
        async onDeleteContact() {
            if (confirm("Bạn muốn xóa Liên hệ này?")) {
                try {
                    await contactService.delete(this.id);
                    this.$router.push({ name: "contactbook" });
                } catch (error) {
                    console.log(error);
                }
            }
        },
    },
    created() {
        this.message = "";
    },
};
</script>

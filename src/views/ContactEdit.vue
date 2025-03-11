<template>
    <div v-if="contact" class="page">
        <h4>Hieu chinh lien he</h4>
        <ContactForm :contact="contact" @submit:contact="updateContact" @delete:contact="deleteContact" />
        <p>{{ message }}</p>
    </div>
</template>

<script>
    import ContactForm from "@/components/ContactForm.vue";
    import ContactService from "@/services/contact.service";
import { data } from "jquery";
    import { type } from "jquery";

    export default {
        components: {
            ContactForm,
        },
        props: {
            id: { type: String, required: true },
        },
        data() {
            return {
                contact: null,
                message: "",
            };
        },
        methods: {
            async getContact(id) {
                try {
                    this.contact = await ContactService.get(id);
                    console.log("Contact found:", this.contact);
                } catch (error) {
                    console.log(error);
                    // Chuyen sang trang NotFound dong thoi giu cho URL ko doi
                    this.$router.push({
                        name: "notfound",
                        params: {
                            pathMatch: this.$route.path.split("/").slice(1)
                        },
                        query: this.$route.query,
                        hash: this.$route.hash,
                    });
                }
            },

            async updateContact(data) {
                console.log("Updating contact with data:", data);
                try {
                    await ContactService.update(this.contact._id, data);
                    alert('Lien he dc cap nhat thanh cong.');
                    this.$router.push({ name: "contactbook" });
                } catch (error) {
                    console.log(error);
                }
            },

            async deleteContact() {
                if (confirm("Ban muon xoa lien he nay?")) {
                    try {
                        await ContactService.delete(this.contact._id);
                        this.$router.push({ name: "contactbook" });
                    } catch (error) {
                        console.log(error);
                    }
                }
            },
        },
        created() {
            this.getContact(this.id);
            this.message = "";
        },
    };
</script>
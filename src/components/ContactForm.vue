<template>
    <form @submit.prevent="submitContact">
        <div class="form-group">
            <label for="name">Ten</label>
            <Field name="name" type="text" class="form-control" v-model="contactLocal.name" />
            <ErrorMessage name="name" class="error-feedback" />
        </div>

        <div class="form-group">
            <label for="email">E-mail</label>
            <Field name="email" type="email" class="form-control" v-model="contactLocal.email" />
            <ErrorMessage name="email" class="error-feedback" />
        </div>

        <div class="form-group">
            <label for="address">Dia chi</label>
            <Field name="address" type="text" class="form-control" v-model="contactLocal.address" />
            <ErrorMessage name="address" class="error-feedback" />
        </div>

        <div class="form-group">
            <label for="phone">Dien thoai</label>
            <Field name="phone" type="tel" class="form-control" v-model="contactLocal.phone" />
            <ErrorMessage name="phone" class="error-feedback" />
        </div>

        <div class="form-group form-check">
            <input name="favorite" type="checkbox" class="form-check-input" v-model="contactLocal.favorite">
            <label for="favorite" class="form-check-label">
                <strong>Lien he yeu thich</strong>
            </label>
        </div>

        <div class="form-group">
            <button type="submit" class="btn btn-primary">Save</button>
            <button v-if="contactLocal._id" type="button" class="ml-2 btn btn-danger" @click="deleteContact">
                Delete
            </button>
        </div>
    </form>
</template>

<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";

export default {
    components: {
        Form,
        Field,
        ErrorMessage,
    },
    emits: ["submit:contact", "delete:contact"],
    props: {
        contact: { type: Object, required: true }
    },
    data() {
        return {
            contactLocal: { ...this.contact },
            contactFormSchema: yup.object().shape({
                name: yup.string().required("Ten phai co gia tri.").min(2, "Ten phai it nhat 2 ky tu.").max(50, "Ten co nhieu nhat 50 ky tu."),
                email: yup.string().email("Email ko dung.").max(50, "Email nhieu nhat 50 ky tu."),
                address: yup.string().max(100, "Dia chi toi da 100 ky tu."),
                phone: yup.string().matches(/((09|03|07|08|05)+([0-9]{8})\b)/g, "So dien thoai ko hop le."),
            })
        };
    },
    methods: {
        submitContact() {
            this.$emit("submit:contact", this.contactLocal);
        },
        deleteContact() {
            this.$emit("delete:contact", this.contactLocal._id);
        }
    }
};
</script>

<style scoped>
    @import "@/assets/form.css";
</style>
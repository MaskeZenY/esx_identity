<script setup>
import { ref } from 'vue'
import { Form, Field, ErrorMessage } from 'vee-validate';
import * as yup from 'yup'
import moment from 'moment'

const onSubmit = (values) => {
    fetch("http://esx_identity/register", {
            method: "POST",
            body: JSON.stringify({
                firstname: values.firstname,
                lastname: values.lastname,
                dateofbirth: moment(values.dob).format("DD/MM/YYYY"),
                sex: values.gender,
                height: values.height,
            }),
        });
}

const schema = yup.object({
    firstname: yup.string().required('Le prénom est requis').min(3, 'Le prénom doit contenir au moins 3 caractères'),
    lastname: yup.string().required('Le nom est requis').min(3, 'Le nom doit contenir au moins 3 caractères'),
    dob: yup.date()
    .required('La date de naissance est requise')
    .min(new Date("1900-01-01"), "Date trop ancienne")
    .max(moment().subtract(1, 'years').toDate(), "Vous devez avoir au moins 1 an"),
    gender: yup.string().required('Le genre est requis'),
    height: yup.number().required('La taille est requise').min(120, 'Taille minimum 120cm').max(220, 'Taille maximum 220cm').typeError('La taille doit être un nombre'),
})

</script>

<template>
    <div class="dialog">
        <div class="dialog__header">
            <div class="header-content">
                <i class="fas fa-id-card header-icon"></i>
                <div>
                    <h1>CRÉATION DU <span>PERSONNAGE</span></h1>
                    <p class="header-subtitle">Commencez votre aventure</p>
                </div>
            </div>
        </div>
        <div class="dialog__body">
            <Form class="dialog__body-form" id="register" action="#" novalidate @submit="onSubmit" :validation-schema="schema">
                <div class="dialog__form-group">
                    <label for="firstname">Prénom</label>
                    <div class="dialog__form-validation">
                        <Field id="firstname" type="text" name="firstname" placeholder="Entrez votre prénom" validateOnInput />
                    </div>
                    <ErrorMessage name="firstname" class="dialog__form-message dialog__form-message--error" />
                </div>
                <div class="dialog__form-group">
                    <label for="lastname">Nom</label>
                    <div class="dialog__form-validation">
                        <Field id="lastname" type="text" name="lastname" placeholder="Entrez votre nom" validateOnInput />
                    </div>
                    <ErrorMessage name="lastname" class="dialog__form-message dialog__form-message--error" />
                </div>
                <div class="dialog__form-group">
                    <label for="dob">Date de naissance</label>
                    <Field id="dob" type="date" name="dob" placeholder="jj/mm/aaaa" validateOnInput />
                    <ErrorMessage name="dob" class="dialog__form-message dialog__form-message--error" />
                </div>
                <div class="dialog__form-group">
                    <label for="gender">Genre</label>
                    <div class="dialog__form-group dialog__form-group--radio">
                        <div class="dialog__form-radio">
                            <Field type="radio" id="male" value="m" name="gender" validateOnInput />
                            <label for="male">
                                <i class="fas fa-mars"></i>Homme
                            </label>
                        </div>
                        <div class="dialog__form-radio">
                            <Field type="radio" id="female" value="f" name="gender" validateOnInput />
                            <label for="female">
                                <i class="fas fa-venus"></i>Femme
                            </label>
                        </div>
                    </div>
                    <ErrorMessage name="gender" class="dialog__form-message dialog__form-message--error" />
                </div>
                <div class="dialog__form-group">
                    <label for="height">Taille (cm)</label>
                    <Field id="height" type="text" name="height" placeholder="175" validateOnInput/>
                    <ErrorMessage name="height" class="dialog__form-message dialog__form-message--error" />
                </div>
                <button class="dialog__form-submit" id="submit" type="submit">
                    <i class="fas fa-user-plus"></i>
                    <span>Créer le personnage</span>
                </button>
            </Form>
        </div>
    </div>
</template>

<style scoped>
.none {
    display: none;
}

.dialog {
    background: linear-gradient(to bottom right, #ffffff, #f8faff);
    border: none;
    box-shadow: 0 20px 60px rgba(52, 152, 219, 0.1);
    max-width: 500px;
    width: 90%;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    overflow: hidden;
    margin: 0;
}

.dialog__header {
    background: linear-gradient(135deg, #3498db, #2980b9);
    padding: 2rem;
}

.header-content {
    display: flex;
    align-items: center;
    gap: 1.5rem;
}

.header-icon {
    font-size: 2.5rem;
    color: rgba(255, 255, 255, 0.9);
}

.header-subtitle {
    margin: 0.5rem 0 0 0;
    font-size: 0.9rem;
    opacity: 0.8;
}

h1 {
    margin: 0;
    font-size: 1.5rem;
    letter-spacing: 1px;
}

.dialog__body {
    padding: 2rem;
}

.dialog__form-group {
    background: white;
    border: 1px solid rgba(52, 152, 219, 0.2);
    margin-bottom: 1.5rem;
    position: relative;
    overflow: hidden;
}

.dialog__form-group:focus-within {
    border-color: #3498db;
    box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.1);
}

.dialog__form-group label {
    font-size: 0.8rem;
    color: #666;
    margin-bottom: 0.3rem;
    display: block;
    padding: 0.5rem 1rem 0;
}

.dialog__form-group input {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    width: 100%;
    border: none;
}

.dialog__form-group--radio {
    display: flex;
    gap: 1rem;
    padding: 0.5rem 1rem;
}

.dialog__form-radio label {
    padding: 0.5rem 1rem;
    border-radius: 6px;
    transition: all 0.2s ease;
}

.dialog__form-radio input:checked + label {
    background-color: rgba(52, 152, 219, 0.1);
    color: #3498db;
}

.dialog__form-submit {
    width: 100%;
    padding: 1rem;
    background: linear-gradient(135deg, #3498db, #2980b9);
    border: none;
    border-radius: 8px;
    color: white;
    font-weight: 600;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    transition: all 0.3s ease;
    cursor: pointer;
}

.dialog__form-submit:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(52, 152, 219, 0.2);
}

.dialog__form-message--error {
    font-size: 0.75rem;
    color: #e74c3c;
    position: absolute;
    bottom: -1.2rem;
    left: 16rem;
}

/* Animation pour les inputs */
.dialog__form-group input:focus {
    background-color: rgba(52, 152, 219, 0.03);
}

/* Style pour le sélecteur de date */
input[type="date"] {
    color: #333;
}

/* Style pour les placeholders */
::placeholder {
    color: #999;
    font-weight: 400;
}

</style>

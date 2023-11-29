
<script>
import axios from "axios";

export default {
    name: "ContactsView",
    data() {
        return {
            base_url: "http://127.0.0.1:8000",
            loading: false,
            name: "",
            email: "",
            phone: "",
            message: "",
            errors: [],
            success: null,
        };
    },
    methods: {
        sendForm() {
            this.loading = true;
            this.errors = [];
            this.success = null;

            const payload = {
                name: this.name,
                email: this.email,
                phone: this.phone,
                message: this.message,
            };
            console.log(payload);

            axios
                .post(this.base_url + "/api/contacts", payload)
                .then((response) => {
                    const success = response.data.success;
                    if (!success) {
                        // there are errors
                        console.log(response);
                        console.log(response.data.errors);
                        this.errors = response.data.errors;
                    } else {
                        // All good
                        console.log(response);
                        console.log(response.data.message);
                        // empty the form
                        this.name = "";
                        this.email = "";
                        this.message = "";
                        this.phone = "";

                        // print a success message
                        this.success = response.data.message;
                    }
                    this.loading = false;
                })
                .catch((error) => {
                    console.error(error.message);
                });
        },
    },
};
</script>





<template>
    <div>



        <div class="p-5 mb-4 bg-dark text-light">
            <div class="container py-5">
                <h1 class="display-5 fw-bold">Scopri di più</h1>
                <p class="col-md-8 fs-4">Vieni a provare le nostre novità</p>
                <button class="btn btn-primary btn-lg" type="button">Find out more</button>
            </div>

        </div>





        <div class="container bg-dark rounded-3 p-4 text-light">
            <form action="" v-on:submit.prevent="sendForm()">
                <div v-if="!loading">
                    <div class="mb-3">
                        <label for="name" class="form-label text-uppercase">Name</label>
                        <input type="text" name="name" id="name" class="form-control" placeholder="Mario Rossi"
                            aria-describedby="nameHelper" v-model="name" :class="{ 'is-invalid': errors.name }" />
                        <small id="nameHelper" class="text-muted">Type your name</small>

                        <div class="alert alert-danger" role="alert" v-if="errors.name">
                            <strong>Errore!</strong>

                            <ul>
                                <li v-for="message in errors.name">{{ message }}</li>
                            </ul>
                        </div>
                    </div>

                    <div class="mb-3">
                        <label for="phone" class="form-label  text-uppercase">phone</label>
                        <input type="tel" name="phone" id="phone" class="form-control" placeholder="123456"
                            aria-describedby="phoneHelper" v-model="phone" :class="{ 'is-invalid': errors.phone }" />
                        <small id="phoneHelper" class="text-muted">Type your phone</small>

                        <div class="alert alert-danger" role="alert" v-if="errors.phone">
                            <strong>Errore!</strong>

                            <ul>
                                <li v-for="message in errors.phone">{{ message }}</li>
                            </ul>
                        </div>
                    </div>
                    <div class="mb-3">
                        <label for="email" class="form-label text-uppercase">email</label>
                        <input type="text" name="email" id="email" class="form-control" placeholder="mario@musk.com"
                            aria-describedby="emailHelper" v-model="email" :class="{ 'is-invalid': errors.email }" />
                        <small id="emailHelper" class="text-muted">Type your email</small>

                        <div class="alert alert-danger" role="alert" v-if="errors.email">
                            <strong>Errore!</strong>

                            <ul>
                                <li v-for="message in errors.email">{{ message }}</li>
                            </ul>
                        </div>
                    </div>

                    <div class="mb-3">
                        <label for="message" class="form-label">Message</label>
                        <textarea class="form-control" name="message" id="message" rows="3"
                            placeholder="Inserisci il tuo messaggio..." v-model="message"
                            :class="{ 'is-invalid': errors.message }"></textarea>

                        <div class="alert alert-danger" role="alert" v-if="errors.message">
                            <strong>Erors!</strong>

                            <ul>
                                <li v-for="message in errors.message">{{ message }}</li>
                            </ul>
                        </div>
                    </div>

                    <button type="submit" class="btn mb-3" :class="{ ' btn-primary w-100': !loading }" :disabled="loading">
                        <span> Send </span>
                    </button>
                </div>

                <div style="height: 25rem;" class="d-flex align-items-center justify-content-center" v-else>

                    <div class="loader">
                        <span class="loader-text">loading</span>
                        <span class="load"></span>
                    </div>

                </div>

                <div class="alert alert-success" role="alert" v-if="success">
                    <strong> La mail è stata inviata correttamente! </strong>
                </div>
            </form>
        </div>



        <div class="bg-dark text-light mt-5">
            <div class=" d-flex">
                <div class="col text-center">


                    <h2 class="py-5">RECAPITI</h2>



                    <div class="d-flex justify-content-center align-items-center gap-3">
                        <img width="50" src="https://th.bing.com/th/id/OIP.55DCXbXlKDgEBoZhKxpzLAAAAA?rs=1&pid=ImgDetMain"
                            alt="">
                        <img width="50"
                            src="https://th.bing.com/th/id/R.26d9974a1feec9905a4e0d5e5ddf8db6?rik=ycoXFwG5Udz08A&pid=ImgRaw&r=0"
                            alt="">
                    </div>

                    <h2 class="pt-5">SEDE</h2>
                    <p>Via palazzo Venezia 12</p>
                    <p>Pescara (PE)</p>

                </div>

                <div class="col">
                    <img class="img-fluid" src="https://foundthebar.com/gall/12.jpg" alt="">
                </div>

            </div>


        </div>

    </div>
</template>


<style scoped>
.loader {
    width: 80px;
    height: 50px;
    position: relative;
}

.loader-text {
    position: absolute;
    top: 0;
    padding: 0;
    margin: 0;
    color: #C8B6FF;
    animation: text_713 3.5s ease both infinite;
    font-size: .8rem;
    letter-spacing: 1px;
}

.load {
    background-color: #9A79FF;
    border-radius: 50px;
    display: block;
    height: 16px;
    width: 16px;
    bottom: 0;
    position: absolute;
    transform: translateX(64px);
    animation: loading_713 3.5s ease both infinite;
}

.load::before {
    position: absolute;
    content: "";
    width: 100%;
    height: 100%;
    background-color: #D1C2FF;
    border-radius: inherit;
    animation: loading2_713 3.5s ease both infinite;
}

@keyframes text_713 {
    0% {
        letter-spacing: 1px;
        transform: translateX(0px);
    }

    40% {
        letter-spacing: 2px;
        transform: translateX(26px);
    }

    80% {
        letter-spacing: 1px;
        transform: translateX(32px);
    }

    90% {
        letter-spacing: 2px;
        transform: translateX(0px);
    }

    100% {
        letter-spacing: 1px;
        transform: translateX(0px);
    }
}

@keyframes loading_713 {
    0% {
        width: 16px;
        transform: translateX(0px);
    }

    40% {
        width: 100%;
        transform: translateX(0px);
    }

    80% {
        width: 16px;
        transform: translateX(64px);
    }

    90% {
        width: 100%;
        transform: translateX(0px);
    }

    100% {
        width: 16px;
        transform: translateX(0px);
    }
}

@keyframes loading2_713 {
    0% {
        transform: translateX(0px);
        width: 16px;
    }

    40% {
        transform: translateX(0%);
        width: 80%;
    }

    80% {
        width: 100%;
        transform: translateX(0px);
    }

    90% {
        width: 80%;
        transform: translateX(15px);
    }

    100% {
        transform: translateX(0px);
        width: 16px;
    }
}
</style>

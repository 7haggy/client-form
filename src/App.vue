<template>
    <div id="app">
        <div class="success-title" :class="{ active: success }">
            Клиент успешно создан
        </div>
        <div class="container">
            <h1 class="title">Создать клиента</h1>
            <div class="form-group">
                <div class="el-group">
                    <div class="label">Фамилия <span class="required">*</span></div>
                    <input
                        class="input"
                        :class="{ error: $v.surname.$error }"
                        v-model="$v.surname.$model"
                        placeholder="Иванов"
                        type="text"
                    />
                    <div class="error-label" v-if="$v.surname.$error">
                        Обязательное поле
                    </div>
                </div>
                <div class="el-group">
                    <div class="label">Имя <span class="required">*</span></div>
                    <input
                        class="input"
                        :class="{ error: $v.name.$error }"
                        v-model="$v.name.$model"
                        type="text"
                        placeholder="Иван"
                    />
                    <div class="error-label" v-if="$v.name.$error">
                        Обязательное поле
                    </div>
                </div>
                <div class="el-group">
                    <div class="label">Отчество</div>
                    <input class="input" v-model="middleName" type="text" placeholder="Иванович" />
                </div>
                <div class="el-group">
                    <div class="label">Дата рождения <span class="required">*</span></div>
                    <input
                        class="input"
                        :class="{ error: $v.birthday.$error }"
                        v-model="$v.birthday.$model"
                        min="1920-01-01"
                        :max="today"
                        type="date"
                    />
                    <div
                        class="error-label"
                        v-if="$v.birthday.$error && $v.birthday.required && !$v.birthday.notMoreCurrentDate"
                    >
                        Дата не может быть больше текущей
                    </div>
                    <div class="error-label" v-if="$v.name.$error && !$v.birthday.required">
                        Обязательное поле
                    </div>
                </div>
                <div class="el-group">
                    <div class="label">Номер телефона <span class="required">*</span></div>
                    <input
                        class="input"
                        :class="{ error: $v.phone.$error }"
                        v-model="$v.phone.$model"
                        placeholder="+7 (908) 414-41-41"
                        type="text"
                        v-mask
                    />
                    <div class="error-label" v-if="$v.phone.$error">
                        Обязательное поле
                    </div>
                </div>
                <div class="el-group">
                    <div class="label">Пол <span class="required">*</span></div>
                    <div class="radio-group">
                        <input class="radio" name="gender" v-model="gender" value="man" type="radio" id="man" />
                        <label class="radio-label" for="man">Мужчина</label>
                        <input class="radio" name="gender" v-model="gender" value="woman" type="radio" id="woman" />
                        <label class="radio-label" for="woman">Женщина</label>
                    </div>
                </div>
                <div class="el-group">
                    <div class="label">Группа клиентов <span class="required">*</span></div>
                    <customSelect
                        :isMulti="true"
                        :class="{ error: $v.clients.$error }"
                        v-model="clients"
                        :options="['Иванов', 'Захаров', 'Чернышева']"
                    />
                </div>
                <div class="el-group">
                    <div class="label">Лечащий врач</div>
                    <customSelect v-model="doctor" :options="['VIP', 'Проблемные', 'ОМС']" />
                </div>
                <div class="el-group">
                    <input class="checkbox" v-model="sms" type="checkbox" id="sms" />
                    <label class="checkbox-label" for="sms">Не отправлять смс</label>
                </div>
            </div>
            <div class="form-group">
                <h3 class="subtitle">Адрес</h3>
                <div class="el-group">
                    <div class="label">Индекс</div>
                    <input class="input" v-model="addressIndex" placeholder="346111" type="text" />
                </div>
                <div class="el-group">
                    <div class="label">Страна</div>
                    <input class="input" v-model="country" placeholder="Россия" type="text" />
                </div>
                <div class="el-group">
                    <div class="label">Область</div>
                    <input class="input" v-model="region" placeholder="Ростовская область" type="text" />
                </div>
                <div class="el-group">
                    <div class="label">Город <span class="required">*</span></div>
                    <input
                        class="input"
                        :class="{ error: $v.city.$error }"
                        v-model="$v.city.$model"
                        placeholder="Ростов на Дону"
                        type="text"
                    />
                    <div class="error-label" v-if="$v.city.$error">
                        Обязательное поле
                    </div>
                </div>
                <div class="el-group">
                    <div class="label">Улица</div>
                    <input class="input" v-model="street" placeholder="Малиновского" type="text" />
                </div>
                <div class="el-group">
                    <div class="label">Дом</div>
                    <input class="input" v-model="house" placeholder="23А" type="text" />
                </div>
            </div>
            <div class="form-group">
                <h3 class="subtitle">Документ удостоверяющий личность</h3>
                <div class="el-group">
                    <div class="label">Тип документа <span class="required">*</span></div>
                    <customSelect
                        v-model="$v.document.$model"
                        :class="{ error: $v.document.$error }"
                        :options="['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение']"
                    />
                    <div class="error-label" v-if="$v.document.$error">
                        Обязательное поле
                    </div>
                </div>
                <div class="el-group">
                    <div class="label">Серия</div>
                    <input class="input" placeholder="4467" type="text" />
                </div>
                <div class="el-group">
                    <div class="label">Номер</div>
                    <input
                        class="input"
                        :class="{ error: $v.docNumber.$error }"
                        v-model="$v.docNumber.$model"
                        placeholder="234567"
                        type="text"
                    />
                    <div class="error-label" v-if="$v.docNumber.$error">Номер должен состоять из 6 символов</div>
                </div>
                <div class="el-group">
                    <div class="label">Кем выдан</div>
                    <input class="input" placeholder="УВД Ростовской области" type="text" />
                </div>
                <div class="el-group">
                    <div class="label">Дата выдачи <span class="required">*</span></div>
                    <input
                        class="input"
                        :class="{ error: $v.dateOfIssue.$error }"
                        v-model="$v.dateOfIssue.$model"
                        min="1934-01-01"
                        :max="today"
                        type="date"
                    />
                    <div class="error-label" v-if="$v.dateOfIssue.$error && !$v.dateOfIssue.required">
                        Обязательное поле
                    </div>
                    <div class="error-label" v-if="!$v.dateOfIssue.isDateValid && $v.dateOfIssue.required">
                        Дата выдачи не может быть ранее даты рождения
                    </div>
                    <div
                        class="error-label"
                        v-if="
                            $v.dateOfIssue.$error &&
                                $v.dateOfIssue.isDateValid &&
                                $v.dateOfIssue.required &&
                                !$v.dateOfIssue.notMoreCurrentDate
                        "
                    >
                        Дата не может быть больше текущей
                    </div>
                </div>
            </div>
            <button class="btn" @click="createUser">Создать</button>
        </div>
    </div>
</template>

<script>
    import customSelect from "./components/customSelect.vue";
    import { required, minLength, maxLength } from "vuelidate/lib/validators";
    function isDateValid(value) {
        if (this.birthday && value) {
            return new Date(value) >= new Date(this.birthday);
        } else {
            return true;
        }
    }
    function notMoreCurrentDate(value) {
        return new Date(value) < new Date().setDate(new Date().getDate() + 1);
    }
    export default {
        name: "App",
        components: { customSelect },
        data() {
            return {
                success: false,
                name: "",
                surname: "",
                middleName: "",
                birthday: "",
                gender: "man",
                clients: [],
                phone: "+7",
                doctor: "",
                addressIndex: "",
                country: "",
                region: "",
                house: "",
                street: "",
                city: "",
                docNumber: "",
                document: [],
                dateOfIssue: "",
                sms: false,
            };
        },
        computed: {
            today() {
                let dd = String(new Date().getDate()).padStart(2, "0");
                let mm = String(new Date().getMonth() + 1).padStart(2, "0");
                let yyyy = new Date().getFullYear();
                return yyyy + "-" + mm + "-" + dd;
            },
        },
        validations: {
            surname: {
                required,
            },
            name: {
                required,
            },
            phone: {
                required,
                minLength: minLength(18),
            },
            birthday: {
                required,
                notMoreCurrentDate,
            },
            clients: {
                required,
            },
            city: {
                required,
            },
            document: {
                required,
            },
            dateOfIssue: {
                required,
                isDateValid,
                notMoreCurrentDate,
            },
            docNumber: {
                minLength: minLength(6),
                maxLength: maxLength(6),
            },
        },
        directives: {
            mask: {
                inserted: function(el) {
                    el.oninput = function(e) {
                        if (!e.isTrusted) {
                            return;
                        }
                        let x = this.value.replace(/\D/g, "").match(/(\d{0,1})(\d{0,3})(\d{0,3})(\d{0,2})(\d{0,2})/);
                        this.value = !x[3]
                            ? "+7 " + x[2]
                            : "+7 " + "(" + x[2] + ") " + x[3] + (x[4] ? "-" + x[4] : "") + (x[5] ? "-" + x[5] : "");
                        el.dispatchEvent(new Event("input"));
                    };
                },
            },
            date: {
                inserted: function(el) {
                    el.oninput = function(e) {
                        if (!e.isTrusted) {
                            return;
                        }
                        let x = this.value.replace(/\D/g, "").match(/(\d{0,2})(\d{0,2})(\d{0,4})/);
                        this.value = !x[2] ? (x[1] > 31 ? 31 : x[1]) : x[1] + "." + (x[3] ? x[2] + "." : x[2]) + x[3];
                        el.dispatchEvent(new Event("input"));
                    };
                },
            },
        },
        methods: {
            createUser() {
                this.$v.$touch();
                if (!this.$v.$error) {
                    this.success = true;
                    setTimeout(() => {
                        this.success = false;
                    }, 1500);
                }
            },
        },
    };
</script>
<style></style>
<style lang="sass">
    .success-title
        width: fit-content
        position: fixed
        top: -100px
        left: 0
        right: 0
        margin: 0 auto
        color: green
        font-size: 25px
        transition: all 0.5s
        z-index: 300
        background: white
        padding: 10px 20px
        border-radius: 10px
        &.active
            top: 0
    .container
        max-width: 500px
        margin: 50px auto
        padding: 23px 80px
        background: #fff
        box-shadow: 0 10px 20px rgba(0,0,0,.1)
        border-radius: 10px
        font-family: system-ui;
        @media(max-width: 585px)
            padding: 23px 15px

        .error
            border: 1px solid red !important

        .error-label
            position: absolute;
            font-size: 12px;
            color: red;

        .btn
            display: block
            margin: 20px auto 0px
            padding: 10px 35px
            border: 0px
            color: white
            border-radius: 5px
            background-color: #f47c69
            cursor: pointer

        .title
            text-align: center
            margin: 0px
        .subtitle
            width: 100%
            margin: 50px 0px 0px 0px;
            @media(max-width: 585px)
                text-align: center
        .form-group
            display: flex
            align-items: center
            flex-wrap: wrap
            max-width: 410px
            margin: 0 auto
            @media(max-width: 585px)
                flex-direction: column
                justify-content: center
            .el-group
                margin: 20px 5px 0px 5px
                .label
                    font-weight: 400
                    font-size: 14px
                    color: #828282
                .checkbox-label
                    position: relative
                    padding-left: 30px
                    color: #828282
                    font-size: 14px
                    display: block
                    cursor: pointer
                    &:before
                        content: ""
                        position: absolute
                        border: 1px solid #f47c69
                        border-radius: 5px
                        width: 20px
                        min-width: 20px
                        height: 20px
                        bottom: -1px
                        left: 0
                .checkbox
                    display: none
                    &:checked + .checkbox-label
                        &:before
                            background-color: #f47c69
                        &:after
                            content: ""
                            background-image: url("./assets/checkbox.svg")
                            width: 11px
                            height: 7px
                            position: absolute
                            top: 3px
                            left: 5px
                            display: inline-block
                .radio-group
                    display: flex
                    align-items: center
                    .radio-label
                        display: block
                        margin: 4px 5px 0px 0px
                        font-weight: 400
                        font-size: 14px
                        color: #828282
                        padding: 10px 14px
                        border: 1px solid #f2f2f2
                        box-shadow: 0 2px 6px rgba(0,0,0,.05)
                        border-radius: 5px
                        cursor: pointer
                        color: black
                    .radio
                        display: none
                        &:checked + .radio-label
                            border: 1px solid #41b883;
                .required
                    color: red
                .input
                    position: relative
                    height: 40px;
                    background-color: #fff
                    font-size: 14px
                    line-height: 16px
                    font-weight: 400
                    border: 1px solid #f2f2f2
                    box-sizing: border-box
                    box-shadow: 0 2px 6px rgba(0,0,0,.05)
                    border-radius: 5px
                    padding: 11px
                    margin-top: 5px

                    &::placeholder
                        color: #bdbdbd
</style>

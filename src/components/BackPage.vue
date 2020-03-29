<template>
    <div class="has-text-weight-bold">
        <div class="columns">
            <div class="column columns is-full">
                <div class="column is-three-fifths">
                    <h1 class="input-box">Opis Bohatera</h1>
                    <div class="text-box">
                        <b-field v-if="editing">
                            <b-input maxlength="500" rows="14" type="textarea" v-model="opis"></b-input>
                        </b-field>
                        <span v-else>{{ opis }}</span>
                    </div>
                </div>
                <div class="column">
                    <h1 class="input-box">Rany i Blizny</h1>
                    <div class="text-box" style="height: inherit">
                        <ul>
                            <li v-for="(val, cz) in czesciCiala" class="columns stats-row">
                                <span class="column has-text-left is-four-fifths">
                                    {{ cz }}
                                </span>
                                <div class="column">
                                    <dice-background v-model="czesciCiala[cz]">
                                    </dice-background>
                                </div>
                            </li>
                            <li class="columns">
                                <span class="column stat-box">
                                    <b-input name="subject" custom-class="is-small" v-model="rany[0]"
                                             v-if="editing"></b-input>
                                    <span v-else>{{ rany[0] }}</span>
                                </span>
                            </li>
                            <li class="columns">
                                <span class="column stat-box">
                                    <b-input name="subject" custom-class="is-small" v-model="rany[1]"
                                             v-if="editing"></b-input>
                                    <span v-else>{{ rany[1] }}</span>
                                </span>
                            </li>
                        </ul>
                    </div>
                    <div class="input-box has-text-left">
                        <div class="columns">
                            <div class="column is-four-fifths">Żywotność</div>
                            <div class="column">
                                <dice-background v-model="zywotnosc">
                                </dice-background>
                            </div>
                        </div>
                    </div>
                    <div class="input-box has-text-left">
                        <div class="columns">
                            <div class="column is-four-fifths">Rany cielesne</div>
                            <div class="column">
                                <dice-background v-model="ranyCiel">
                                </dice-background>
                            </div>
                        </div>
                    </div>
                    <div class="input-box has-text-left">
                        <div class="columns">
                            <div class="column is-four-fifths">Rany psychiczne</div>
                            <div class="column">
                                <dice-background v-model="ranyPsych">
                                </dice-background>
                            </div>
                        </div>
                    </div>
                    <div class="input-box has-text-left">
                        <div class="columns">
                            <div class="column is-four-fifths">Rany krytyczne</div>
                            <div class="column">
                                <dice-background v-model="ranyKryt">
                                </dice-background>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="column columns">
                <div class="column">
                    <h1 class="input-box">Wyposażenie</h1>
                    <div class="text-box">
                        <b-field style="margin-bottom: 4px !important; min-height: 88%">
                            <b-input maxlength="500" rows="13" type="textarea" v-model="wyposazenie"
                                     v-if="editing"></b-input>
                            <span v-else>{{ wyposazenie }}</span>
                        </b-field>
                        <div class="input-box has-text-left is-flex">
                            Udźwig: Max
                            <dice-background style="width: 40px" v-model="udzwig">
                            </dice-background>
                            Suma
                            <dice-background style="width: 40px" v-model="suma">
                            </dice-background>
                        </div>
                    </div>
                </div>
                <div class="column">
                    <h1 class="input-box">Uzbrojenie</h1>
                    <div class="text-box">
                        <b-field v-if="editing">
                            <b-input maxlength="500" rows="15" type="textarea" v-model="uzbrojenie"></b-input>
                        </b-field>
                        <span v-else>{{ uzbrojenie }}</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import DiceBackground from './DiceBackground.vue';

    export default {
        components: {DiceBackground},
        name: 'BackPage.vue',
        props: {
            editing: {
                type: Boolean,
                required: true
            },
            value: {
                type: Object,
                required: false
            }
        },
        data() {
            return {
                zywotnosc: null,
                ranyCiel: null,
                ranyPsych: null,
                ranyKryt: null,
                udzwig: null,
                suma: null,
                wyposazenie: null,
                uzbrojenie: null,
                czesciCiala: {
                    'Głowa': null,
                    'Korpus': null,
                    'Prawa Ręka': null,
                    'Lewa Ręka': null,
                    'Prawa Noga': null,
                    'Lewa Noga': null
                },
                rany: [],
                opis: '',
            }
        },
        watch: {
            $data: {
                handler: function(value) {
                    this.$emit('input', value);
                },
                deep: true
            },
            value(val) {
                for (let property in val) {
                    if (this.$data.hasOwnProperty(property)) {
                        if (isNaN(parseFloat(val[property]))) {
                            this.$data[property] = val[property];
                        } else {
                            this.$data[property] = parseFloat(val[property]);
                        }
                    }
                }
            }
        },
        mounted() {
            this.$emit('input', this.$data);
        }
    }
</script>

<style scoped>
    h1 {
        font-weight: bold;
    }

    .dice {
        font-weight: normal;
    }
</style>
